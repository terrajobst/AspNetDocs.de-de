---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Quell Code Verwaltung (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5a1e0d7cd3c396d4be79c8958422602055eb3db1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457101"
---
# <a name="source-control-building-real-world-cloud-apps-with-azure"></a>Quell Code Verwaltung (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Die Quell Code Verwaltung ist für alle cloudentwicklungsprojekte unverzichtbar, nicht nur für Team Umgebungen. Sie sind nicht in der Lage, Quellcode oder sogar ein Word-Dokument ohne Rückgängig-Funktion und automatische Sicherungen zu bearbeiten, und die Quell Code Verwaltung bietet Ihnen diese Funktionen auf Projektebene, wo Sie noch mehr Zeit sparen können, wenn etwas schief geht. Mit den Cloud-Quell Code Verwaltungsdiensten müssen Sie sich nicht mehr um eine komplizierte Einrichtung kümmern, und Sie können Azure Repos Source Control kostenlos für bis zu 5 Benutzer verwenden.

Im ersten Teil dieses Kapitels werden drei wichtige bewährte Methoden erläutert, die Sie berücksichtigen sollten:

- [Behandeln Sie Automatisierungs Skripts als Quellcode, und versidieren](#scripts) Sie Sie mit Ihrem Anwendungscode.
- [Checken Sie Geheimnisse](#secrets) (vertrauliche Daten wie Anmelde Informationen) niemals in ein Quellcoderepository ein.
- [Richten Sie quellbranches](#devops) ein, um den devops-Workflow zu aktivieren.

Der Rest des Kapitels enthält einige Beispiel Implementierungen dieser Muster in Visual Studio, Azure und Azure Repos:

- [Hinzufügen von Skripts zur Quell Code Verwaltung in Visual Studio](#vsscripts)
- [Speichern von sensiblen Daten in Azure](#appsettings)
- [Verwenden von git in Visual Studio und Azure Repos](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Behandeln von Automatisierungs Skripts als Quellcode

Wenn Sie an einem cloudprojekt arbeiten, ändern Sie die Dinge häufig, und Sie möchten schnell auf Probleme reagieren können, die von ihren Kunden gemeldet werden. Die schnelle Reaktion umfasst die Verwendung von Automatisierungs Skripts, wie im Kapitel [Automatisieren von allem](automate-everything.md) erläutert. Alle Skripts, mit denen Sie Ihre Umgebung erstellen, bereitstellen, skalieren, usw., müssen mit dem Quellcode der Anwendung synchronisiert werden.

Um Skripts mit Code synchron zu halten, speichern Sie diese in Ihrem Quell Code Verwaltungssystem. Wenn Sie also jemals Änderungen zurücksetzen oder eine schnelle Korrektur des Produktionscodes durchführen müssen, der sich von dem Entwicklungscode unterscheidet, müssen Sie keine Zeit verschwenden, um zu verfolgen, welche Einstellungen geändert wurden oder welche Teammitglieder Kopien der Version haben, die Sie benötigen. Sie sind sicher, dass die benötigten Skripts mit der Codebasis synchronisiert sind, für die Sie Sie benötigen, und Sie sind sicher, dass alle Teammitglieder mit denselben Skripts arbeiten. Unabhängig davon, ob Sie das Testen und die Bereitstellung eines Hotfixes für die Produktion oder die Entwicklung neuer Features automatisieren müssen, verfügen Sie über das richtige Skript für den Code, der aktualisiert werden muss.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Geheimnisse nicht einchecken

Ein Quellcoderepository ist in der Regel für zu viele Personen zugänglich, damit es ein angemessen sicherer Ort für sensible Daten wie Kenn Wörter ist. Wenn Skripts auf geheimen Schlüsseln wie Kenn Wörtern basieren, Parametrisieren Sie diese Einstellungen so, dass Sie nicht im Quellcode gespeichert werden, und speichern Sie Ihre geheimen Schlüssel an einem anderen Ort.

Azure ermöglicht beispielsweise das Herunterladen von Dateien, die Veröffentlichungs Einstellungen enthalten, um die Erstellung von Veröffentlichungs Profilen zu automatisieren. Diese Dateien enthalten Benutzernamen und Kenn Wörter, die zum Verwalten Ihrer Azure-Dienste autorisiert sind. Wenn Sie diese Methode zum Erstellen von Veröffentlichungs Profilen verwenden und diese Dateien in die Quell Code Verwaltung einchecken, kann jeder Benutzer mit Zugriff auf Ihr Repository diese Benutzernamen und Kenn Wörter sehen. Sie können das Kennwort im Veröffentlichungs Profil selbst sicher speichern, da es verschlüsselt ist und es sich in einer *pubxml. User* -Datei befindet, die standardmäßig nicht in der Quell Code Verwaltung enthalten ist.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktur der quellbranches zum Vereinfachen des devops-Workflows

Wie Sie branches in Ihrem Repository implementieren, wirkt sich auf ihre Fähigkeit aus, neue Features zu entwickeln und Probleme in der Produktion zu beheben. Im folgenden finden Sie ein Muster, das von vielen mittelgroßen Teams verwendet wird:

![Quellbranchstruktur](source-control/_static/image1.png)

Der masterb Ranch entspricht immer dem Code, der sich in der Produktion befindet. Verzweigungen unterhalb des Masters entsprechen verschiedenen Phasen des Entwicklungs Lebenszyklus. In der Development-Verzweigung implementieren Sie neue Funktionen. Bei einem kleinen Team haben Sie möglicherweise nur die Master-und Entwicklungsumgebung, aber wir empfehlen häufig, dass die Benutzer über einen stagingbranch zwischen Entwicklung und Master verfügen. Sie können das Staging für abschließende Integrationstests verwenden, bevor ein Update in die Produktion verschoben wird.

Für große Teams gibt es möglicherweise separate Verzweigungen für jedes neue Feature. für ein kleineres Team müssen Sie möglicherweise alle Benutzer an der Development-Verzweigung einchecken.

Wenn Sie über eine Verzweigung für jede Funktion verfügen, können Sie, wenn Feature a bereit ist, die Quell Codeänderungen in die entwicklungsverzweigung und in die anderen featurebranches zusammenführen. Diese Zusammenführung des Quellcodes kann zeitaufwändig sein. um diese Arbeit zu vermeiden, während Features weiterhin separat aufbewahrt werden, implementieren einige Teams eine Alternative namens *[Funktions Umschalter](http://en.wikipedia.org/wiki/Feature_toggle)* (auch als *featureflags*bezeichnet). Dies bedeutet, dass sich der gesamte Code für alle Funktionen in derselben Verzweigung befindet, Sie jedoch die einzelnen Features aktivieren oder deaktivieren, indem Sie die Schalter im Code verwenden. Angenommen, Feature a ist ein neues Feld für die Behebung von IT-App-Aufgaben, und Feature B bietet Funktionen zum Zwischenspeichern. Der Code für beide Features kann sich in der Development-Verzweigung befinden, aber die APP zeigt nur das neue Feld an, wenn eine Variable auf true festgelegt ist, und verwendet nur Caching, wenn eine andere Variable auf true festgelegt ist. Wenn Feature A nicht für die herauf Stufung bereit ist, aber das Feature b bereit ist, können Sie den gesamten Code in die Produktion herauf Stufen, indem Sie das Feature a Switch Off und die Funktion b Switch on. Sie können dann Feature A Fertigstellen und es später herauf Stufen, alle ohne Zusammenführung des Quellcodes.

Unabhängig davon, ob Sie Verzweigungen oder UMSCHALT Flächen für Funktionen verwenden, können Sie mit einer Verzweigungs Struktur wie dieser auf Agile und wiederholbare Weise Ihren Code von der Entwicklung in die Produktion übertragen.

Diese Struktur ermöglicht es Ihnen auch, schnell auf Kundenfeedback zu reagieren. Wenn Sie eine schnelle Lösung für die Produktion erstellen müssen, können Sie dies auch auf agile Weise tun. Sie können eine Verzweigung aus Master oder Staging erstellen, und wenn Sie bereit ist, können Sie Sie in die Master-und featurebranches zusammenführen.

![Hotfixbranch](source-control/_static/image2.png)

Ohne eine Verzweigungs Struktur wie diese durch die Trennung von Produktions-und entwicklungsbranches könnte ein Produktionsproblem darin liegen, dass Sie neue featurecodes zusammen mit der Produktionslösung herauf Stufen müssen. Der neue Featurecode ist möglicherweise nicht vollständig getestet und bereit für die Produktion, und Sie müssen möglicherweise viel Arbeit durchführen, um Änderungen zu sichern, die nicht bereit sind. Möglicherweise müssen Sie ihre Behebung aber verzögern, um Änderungen zu testen und Sie für die Bereitstellung vorzubereiten.

Als nächstes sehen Sie Beispiele für die Implementierung dieser drei Muster in Visual Studio, Azure und Azure Repos. Dabei handelt es sich um Beispiele und nicht um ausführliche Anweisungen zur Vorgehensweise. Ausführliche Anweisungen, die den gesamten erforderlichen Kontext bereitstellen, finden Sie im Abschnitt " [Ressourcen](#resources) " am Ende des Kapitels.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Hinzufügen von Skripts zur Quell Code Verwaltung in Visual Studio

Sie können der Quell Code Verwaltung Skripts in Visual Studio hinzufügen, indem Sie Sie in einen Visual Studio-Projektmappenordner einschließen (vorausgesetzt, das Projekt befindet sich in der Quell Code Dies ist eine Möglichkeit, dies zu tun.

Erstellen Sie einen Ordner für die Skripts in Ihrem Projektmappenordner (dem Ordner mit der *sln* -Datei).

![Automation-Ordner](source-control/_static/image3.png)

Kopieren Sie die Skriptdateien in den Ordner.

![Inhalt des Automatisierungs Ordners](source-control/_static/image4.png)

Fügen Sie in Visual Studio dem Projekt einen Projektmappenordner hinzu.

![Menü Auswahl für neuen Projektmappenordner](source-control/_static/image5.png)

Und fügen die Skriptdateien dem Projektmappenordner hinzu.

![Menü Auswahl für vorhandenes Element hinzufügen](source-control/_static/image6.png)

![Vorhandenes Element hinzufügen (Dialogfeld)](source-control/_static/image7.png)

Die Skriptdateien sind nun in Ihrem Projekt enthalten, und die Quell Code Verwaltung verfolgt ihre Versionsänderungen zusammen mit den entsprechenden Quell Codeänderungen nach.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Speichern von sensiblen Daten in Azure

Wenn Sie Ihre Anwendung auf einer Azure-Website ausführen, können Sie das Speichern von Anmelde Informationen in der Quell Code Verwaltung vermeiden, indem Sie Sie stattdessen in Azure speichern.

Beispielsweise speichert die IT-Anwendung in der Datei Web. config zwei Verbindungs Zeichenfolgen, die Kenn Wörter in der Produktion enthalten, und einen Schlüssel, der Zugriff auf Ihr Azure Storage-Konto gewährt.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Wenn Sie tatsächliche Produktionswerte für diese Einstellungen in die Datei " *Web. config* " einfügen, oder wenn Sie Sie in der Datei " *Web. Release. config* " ablegen, um eine Web. config-Transformation zum Einfügen während der Bereitstellung zu konfigurieren, werden Sie im Quellrepository gespeichert. Wenn Sie die Daten bankverbindungs Zeichenfolgen in das Produktions Veröffentlichungs Profil eingeben, wird das Kennwort in der *pubxml* -Datei angezeigt. (Sie können die *pubxml* -Datei aus der Quell Code Verwaltung ausschließen, aber dann verlieren Sie den Vorteil, dass alle anderen Bereitstellungs Einstellungen gemeinsam genutzt werden.)

Azure bietet Ihnen eine Alternative zu den Abschnitten " **appSettings** " und "Connection Strings" in der Datei " *Web. config* ". Hier ist der relevante Teil der Registerkarte " **Konfiguration** " für eine Website im Azure-Verwaltungs Portal:

![appSettings und connectionStrings im Portal](source-control/_static/image8.png)

Wenn Sie ein Projekt auf dieser Website bereitstellen und die Anwendung ausgeführt wird, überschreiben die in Azure gespeicherten Werte alle Werte in der Datei "Web. config".

Sie können diese Werte in Azure entweder mithilfe des Verwaltungs Portals oder mithilfe von Skripts festlegen. Das Automation-Skript für die Umgebungs Erstellung, das Sie im Kapitel [Automatisieren von alles](automate-everything.md) gesehen haben, erstellt eine Azure SQL-Datenbank, ruft die Speicher-und SQL-Daten bankverbindungs Zeichenfolgen ab und speichert diese geheimen Schlüssel in den Einstellungen für Ihre Website

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Beachten Sie, dass die Skripts parametrisiert werden, damit tatsächliche Werte nicht im Quellrepository persistent gespeichert werden.

Wenn Sie lokal in Ihrer Entwicklungsumgebung ausgeführt werden, liest die APP die lokale Datei "Web. config", und ihre Verbindungs Zeichenfolge verweist auf eine localdb-SQL Server-Datenbank im *App-\_Daten* Ordner des Webprojekts. Wenn Sie die app in Azure ausführen und die APP versucht, diese Werte aus der Datei "Web. config" zu lesen, werden die Werte, die für die Website gespeichert werden, und die in der Datei "Web. config" verwendeten Werte nicht wieder verwendet.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Verwenden von git in Visual Studio und Azure devops

Sie können eine beliebige Quell Code Verwaltungs Umgebung zum Implementieren der zuvor dargestellten devops-Verzweigungs Struktur verwenden. Für verteilte Teams kann ein [verteiltes Versionskontrollsystem](http://en.wikipedia.org/wiki/Distributed_revision_control) funktionieren. für andere Teams kann ein [zentralisiertes System](http://en.wikipedia.org/wiki/Revision_control) besser funktionieren.

[Git](http://git-scm.com/) ist ein gängiges verteiltes Versionskontrollsystem. Wenn Sie git für die Quell Code Verwaltung verwenden, verfügen Sie über eine vollständige Kopie des Repository mit dem gesamten Verlauf auf dem lokalen Computer. Viele Leute bevorzugen dies, weil es einfacher ist, weiterzuarbeiten, wenn Sie nicht mit dem Netzwerk verbunden sind. Sie können weiterhin Commits und Rollbacks durchführen, Verzweigungen erstellen und wechseln usw. Auch wenn Sie mit dem Netzwerk verbunden sind, ist es einfacher und schneller, Verzweigungen und switchbranches zu erstellen, wenn alles lokal ist. Sie können auch lokale Commits und Rollbacks durchführen, ohne dass sich dies auf andere Entwickler auswirkt. Und Sie können Commits vor dem Senden an den Server in Batches verpacken.

[Azure Repos](/azure/devops/repos/index?view=vsts) bietet [git](/azure/devops/repos/git/?view=vsts) und [Team Foundation-Versionskontrolle](/azure/devops/repos/tfvc/index?view=vsts) (tfvc; zentralisierte Quell Code Verwaltung). Beginnen Sie [hier](https://app.vsaex.visualstudio.com/signup)mit Azure devops.

Visual Studio 2017 umfasst integrierte, erstklassige [git-Unterstützung](https://msdn.microsoft.com/library/hh850437.aspx). Im folgenden finden Sie eine kurze Demo, wie dies funktioniert.

Wenn ein Projekt in Visual Studio geöffnet ist, klicken Sie mit der rechten Maustaste auf **Projektmappen-Explorer**, und wählen Sie dann Projekt Mappe **zur Quell Code Verwaltung hinzufügen**aus.

![Projekt Mappe zur Quell Code Verwaltung hinzufügen](source-control/_static/image9.png)

In Visual Studio wird gefragt, ob Sie tfvc (zentralisierte Versionskontrolle) oder git verwenden möchten.

![Quell Code Verwaltung auswählen](source-control/_static/image10.png)

Wenn Sie git auswählen und auf **OK**klicken, erstellt Visual Studio in Ihrem Projektmappenordner ein neues lokales git-Repository. Das neue Repository enthält noch keine Dateien. Sie müssen Sie dem Repository hinzufügen, indem Sie einen git-Commit durchgeführt haben. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und klicken Sie auf **Commit**.

![Commit](source-control/_static/image11.png)

Visual Studio stellt automatisch alle Projektdateien für den Commit bereit und listet sie in **Team Explorer** im Bereich **enthaltene Änderungen** auf. (Wenn einige von Ihnen nicht in den Commit eingeschlossen werden sollen, können Sie Sie auswählen, mit der rechten Maustaste klicken und dann auf **ausschließen**klicken.)

![Team Explorer](source-control/_static/image12.png)

Geben Sie einen Commit-Kommentar ein, und klicken Sie auf **Commit**. Visual Studio führt den Commit aus und zeigt die Commit-ID an.

![Team Explorer Änderungen](source-control/_static/image13.png)

Wenn Sie nun Code ändern, sodass er sich von dem im Repository unterscheidet, können Sie die Unterschiede leicht anzeigen. Klicken Sie mit der rechten Maustaste auf eine Datei, die Sie geändert haben, wählen Sie **mit unveränderter vergleichen**aus, und Sie erhalten eine Vergleichs Anzeige, in der die Änderungen ohne Commit angezeigt werden.

![Mit unveränderter vergleichen](source-control/_static/image14.png)

![Diff mit Änderungen](source-control/_static/image15.png)

Sie können leicht feststellen, welche Änderungen Sie vornehmen und Einchecken.

Angenommen, Sie müssen eine Verzweigung erstellen – Dies ist auch in Visual Studio möglich. Klicken Sie in **Team Explorer**auf **neuer Branch**.

![Neue Verzweigung Team Explorer](source-control/_static/image16.png)

Geben Sie einen branchnamen ein, klicken Sie auf **Branch erstellen**, und wenn Sie **Branch**Auschecken ausgewählt haben, prüft Visual Studio automatisch den neuen Branch.

![Neue Verzweigung Team Explorer](source-control/_static/image17.png)

Sie können jetzt Änderungen an Dateien vornehmen und diese in diese Verzweigung einchecken. Und Sie können problemlos zwischen branches wechseln, und Visual Studio synchronisiert die Dateien automatisch mit der Verzweigung, die Sie ausgecheckt haben. In diesem Beispiel wurde der Webseiten Titel in *\_"Layout. cshtml* " in "Hot Fix 1" in der HotFix1-Verzweigung geändert.

![Hotfix1-Verzweigung](source-control/_static/image18.png)

Wenn Sie zurück zum masterb Ranch wechseln, wird der Inhalt der *\_Layout. cshtml* -Datei automatisch auf den Inhalt des masterb Ranch zurückgesetzt.

![Masterb Ranch](source-control/_static/image19.png)

Dies ist ein einfaches Beispiel dafür, wie Sie schnell einen Branch erstellen und zwischen Verzweigungen hin-und herwechseln können. Diese Funktion ermöglicht einen hochgradig agilen Workflow mithilfe der Verzweigungs Struktur und der Automatisierungs Skripts, die im Kapitel [Automatisieren von allem](automate-everything.md) dargestellt werden. Sie können z. b. in der Development-Verzweigung arbeiten, eine hotfixverzweigung aus Master erstellen, zur neuen Verzweigung wechseln, die Änderungen dort vornehmen und einen Commit für Sie ausführen und dann wieder in die Development-Verzweigung wechseln und fortfahren, was Sie tun.

Hier sehen Sie, wie Sie mit einem lokalen git-Repository in Visual Studio arbeiten. In einer Team Umgebung Übertragung Sie in der Regel auch Änderungen an ein gemeinsames Repository. Mit den Visual Studio-Tools können Sie auch auf ein Remote-git-Repository zeigen. Sie können GitHub.com zu diesem Zweck verwenden, oder Sie können [git und Azure Repos](/azure/devops/repos/git/overview?view=vsts) verwenden, die in alle anderen Azure devops-Funktionen integriert sind, wie z. b. Arbeitsaufgaben und Fehler Nachverfolgung.

Dies ist natürlich nicht die einzige Möglichkeit, eine Agile Verzweigungs Strategie zu implementieren. Sie können den gleichen Agile-Workflow mithilfe eines zentralisierten Quellcodeverwaltungs-Repository aktivieren.

## <a name="summary"></a>Zusammenfassung

Messen Sie den Erfolg des Quell Code Verwaltungssystems, je nachdem, wie schnell Sie eine Änderung vornehmen und auf sichere und vorhersagbare Weise wiederherstellen können. Wenn Sie sich für eine Änderung sorgen, weil Sie einen Tag oder zwei manuelle Tests ausführen müssen, Fragen Sie sich möglicherweise, was Sie Prozess Weise oder testweise tun müssen, damit Sie diese Änderung in wenigen Minuten oder im schlechtesten als einer Stunde durchführen können. Eine Strategie hierfür ist die Implementierung von Continuous Integration und Continuous Delivery, die im [nächsten Kapitel](continuous-integration-and-continuous-delivery.md)behandelt werden.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen zu Verzweigungs Strategien finden Sie in den folgenden Ressourcen:

- [Erstellung einer releasepipeline mit Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Microsoft Patterns and Practices-Dokumentation. Eine Erläuterung der Verzweigungs Strategien finden Sie in Kapitel 6. Befürworter von Features umgeschaltet über featurebranches, und wenn Verzweigungen für Funktionen verwendet werden, hält Sie in der kurzlebigen Nähe (Stunden oder Tage).
- [Versions Kontroll Handbuch](https://aka.ms/vsarsolutions). Leitfaden für Verzweigungs Strategien von Alm Rangers. Weitere Informationen finden Sie unter Branching Strategies. PDF auf der Registerkarte Downloads.
- [Software Entwicklung mit Feature-Schalter](https://msdn.microsoft.com/magazine/dn683796.aspx). MSDN Magazine-Artikel.
- [Funktion umschalten](http://martinfowler.com/bliki/FeatureToggle.html). Einführung in Feature-Funktionen und featureflags im Blog von Martin Fowler.
- [Funktion](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)schaltet im Vergleich zu featurebranches um. Ein weiterer Blogbeitrag zu Feature-Schalter von Dylan Smith.

Weitere Informationen zur Behandlung vertraulicher Informationen, die nicht in Quellcodeverwaltungs-Depots aufbewahrt werden sollten, finden Sie in den folgenden Ressourcen:

- [Bewährte Methoden für die Bereitstellung von Kenn Wörtern und anderen sensiblen Daten für ASP.net und Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Azure-Websites: Funktionsweise von Anwendungs-und Verbindungs](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)Zeichenfolgen. Erläutert das Azure-Feature, das `appSettings` und `connectionStrings` Daten in der Datei " *Web. config* " überschreibt.
- [Benutzerdefinierte Konfigurations-und Anwendungseinstellungen auf Azure-Websites: Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Informationen zu anderen Methoden, um vertrauliche Informationen aus der Quell Code Verwaltung herauszuhalten, finden Sie unter [ASP.NET MVC: Keep Private Settings out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Zurück](automate-everything.md)
> [Weiter](continuous-integration-and-continuous-delivery.md)
