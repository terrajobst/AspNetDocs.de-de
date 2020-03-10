---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Allgemeine Konfigurations Unterschiede zwischen Entwicklungs-und ProduktionsC#Umgebungen () | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir unsere Website bereitgestellt, indem wir alle relevanten Dateien aus der Entwicklungsumgebung in die Produktionsumgebung kopiert haben. Ich...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 60379c87a8cf58b89066a6070ac659e65930b4fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439011"
---
# <a name="common-configuration-differences-between-development-and-production-c"></a>Allgemeine Konfigurationsunterschiede zwischen Entwicklungs- und Produktionsumgebungen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> In den vorherigen Tutorials haben wir unsere Website bereitgestellt, indem wir alle relevanten Dateien aus der Entwicklungsumgebung in die Produktionsumgebung kopiert haben. Es ist jedoch nicht ungewöhnlich, dass es Konfigurations Unterschiede zwischen Umgebungen gibt, die erfordern, dass jede Umgebung über eine eindeutige Web. config-Datei verfügt. In diesem Tutorial werden typische Konfigurations Unterschiede untersucht, und es werden Strategien für die Verwaltung separater Konfigurationsinformationen erläutert.

## <a name="introduction"></a>Einführung

Die letzten beiden Tutorials wurden durch das Bereitstellen einer einfachen Webanwendung durchgegangen. Im Tutorial bereitstellen [*Ihres Standorts mithilfe eines FTP-Clients*](deploying-your-site-using-an-ftp-client-cs.md) wurde gezeigt, wie Sie einen eigenständigen FTP-Client verwenden, um die erforderlichen Dateien aus der Entwicklungsumgebung in die Produktionsumgebung zu kopieren. Das vorangehende Tutorial zum Bereitstellen [*Ihrer Website mithilfe von Visual Studio*](deploying-your-site-using-visual-studio-cs.md)untersuchte die Bereitstellung mit dem Tool zum Kopieren von Websites und der Veröffentlichungs Option von Visual Studio. In beiden Tutorials war jede Datei in der Produktionsumgebung eine Kopie einer Datei in der Entwicklungsumgebung. Es ist jedoch nicht ungewöhnlich, dass sich die Konfigurationsdateien in der Produktionsumgebung von denen in der Entwicklungsumgebung unterscheiden. Die Konfiguration einer Webanwendung wird in der `Web.config`-Datei gespeichert und enthält in der Regel Informationen über externe Ressourcen, z. b. Datenbank-, Web-und e-Mail-Server. Außerdem wird das Verhalten der Anwendung in bestimmten Situationen, z. b. beim Auftreten einer nicht behandelten Ausnahme, durchgeführt.

Beim Bereitstellen einer Webanwendung ist es wichtig, dass die richtigen Konfigurationsinformationen in der Produktionsumgebung enden. In den meisten Fällen kann die `Web.config` Datei in der Entwicklungsumgebung unverändert nicht in die Produktionsumgebung kopiert werden. Stattdessen muss eine angepasste Version von `Web.config` in die Produktion hochgeladen werden. In diesem Tutorial werden einige der gängigeren Konfigurations Unterschiede kurz erläutert. Außerdem werden einige Techniken zusammengefasst, mit denen unterschiedliche Konfigurationsinformationen zwischen den Umgebungen beibehalten werden.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typische Konfigurations Unterschiede zwischen Entwicklungs-und Produktionsumgebungen

Die `Web.config` Datei enthält eine Reihe von Konfigurationsinformationen für eine ASP.NET-Anwendung. Einige dieser Konfigurationsinformationen sind unabhängig von der Umgebung identisch. Beispielsweise sind die Authentifizierungs Einstellungen und URL-Autorisierungs Regeln, die in den `<authentication>` und `<authorization>` Elementen der `Web.config` Datei geschrieben werden, in der Regel unabhängig von der Umgebung identisch. Andere Konfigurationsinformationen, wie z. b. Informationen zu externen Ressourcen, unterscheiden sich in der Regel je nach Umgebung.

Daten bankverbindungs Zeichenfolgen sind ein gutes Beispiel für Konfigurationsinformationen, die sich je nach Umgebung unterscheiden. Wenn eine Webanwendung mit einem Datenbankserver kommuniziert, muss Sie zuerst eine Verbindung herstellen. Dies wird über eine [Verbindungs Zeichenfolge](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)erreicht. Obwohl es möglich ist, die Daten bankverbindungs Zeichenfolge direkt in den Webseiten oder in dem Code, der eine Verbindung mit der Datenbank herstellt, hart zu codieren, empfiehlt es sich, die Daten der Verbindungs [zeich`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx) enfolge an einem zentralen Ort zu `Web.config`platzieren. Häufig wird eine andere Datenbank während der Entwicklung verwendet, als in der Produktionsumgebung verwendet wird. Folglich müssen die Verbindungs Zeichenfolgen-Informationen für jede Umgebung eindeutig sein.

> [!NOTE]
> In zukünftigen Tutorials wird die Bereitstellung Daten gesteuerter Anwendungen erläutert. zu diesem Zeitpunkt werden die Besonderheiten der Speicherung von Daten bankverbindungs Zeichenfolgen in der Konfigurationsdatei erläutert.

Das beabsichtigte Verhalten der Entwicklungs-und Produktionsumgebungen unterscheidet sich erheblich. Eine Webanwendung in der Entwicklungsumgebung wird von einer kleinen Gruppe von Entwicklern erstellt, getestet und deentschlgt. In der Produktionsumgebung wird dieselbe Anwendung von vielen unterschiedlichen gleichzeitigen Benutzern besucht. ASP.NET enthält eine Reihe von Features, die Entwicklern beim Testen und Debuggen einer Anwendung helfen. diese Features sollten jedoch aus Leistungs-und Sicherheitsgründen in der Produktionsumgebung deaktiviert werden. Sehen wir uns einige Konfigurationseinstellungen an.

### <a name="configuration-settings-that-impact-performance"></a>Konfigurationseinstellungen, die die Leistung beeinträchtigen

Wenn eine ASP.NET-Seite zum ersten Mal (oder zum ersten Mal, nachdem Sie geändert wurde) aufgerufen wird, muss Ihr deklaratives Markup in eine Klasse konvertiert werden, und diese Klasse muss kompiliert werden. Wenn die Webanwendung die automatische Kompilierung verwendet, muss auch die Code-Behind-Klasse der Seite kompiliert werden. Sie können eine Reihe von Kompilierungsoptionen über das [`<compilation>`-Element](https://msdn.microsoft.com/library/s10awwz0.aspx)der `Web.config` Datei konfigurieren.

Das debug-Attribut ist eines der wichtigsten Attribute im `<compilation>`-Element. Wenn das `debug`-Attribut auf "true" festgelegt ist, enthalten die kompilierten Assemblys Debugsymbole, die beim Debuggen einer Anwendung in Visual Studio benötigt werden. Debugsymbole erhöhen jedoch die Größe der Assembly und erzwingen beim Ausführen des Codes zusätzliche Speicheranforderungen. Wenn das `debug`-Attribut auf "true" festgelegt ist, werden alle von `WebResource.axd` zurückgegebenen Inhalte nicht zwischengespeichert. das bedeutet, dass jedes Mal, wenn ein Benutzer eine Seite besucht, die von `WebResource.axd`zurückgegebenen statischen Inhalte erneut heruntergeladen werden müssen.

> [!NOTE]
> `WebResource.axd` ist ein integrierter HTTP-Handler, der in ASP.NET 2,0 eingeführt wurde und der von Server Steuerelementen verwendet wird, um eingebettete Ressourcen wie Skriptdateien, Bilder, CSS-Dateien und anderen Inhalt abzurufen. Weitere Informationen zur Funktionsweise von `WebResource.axd` und deren Verwendung für den Zugriff auf eingebettete Ressourcen über benutzerdefinierte Server Steuerelemente finden Sie unter [zugreifen auf eingebettete Ressourcen über eine URL mithilfe von `WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).

Das `debug`-Attribut des `<compilation>` Elements wird in der Regel in der Entwicklungsumgebung auf "true" festgelegt. Tatsächlich muss dieses Attribut auf "true" festgelegt werden, um eine Webanwendung zu debuggen. Wenn Sie versuchen, eine ASP.NET-Anwendung aus Visual Studio zu Debuggen und das `debug`-Attribut auf "false" festgelegt ist, zeigt Visual Studio eine Meldung mit dem Hinweis an, dass die Anwendung nicht debuggt werden kann, bis das `debug`-Attribut auf "true" festgelegt ist und diese Änderung für Sie vornehmen kann.

Aufgrund der Auswirkung auf die Leistung sollte das `debug` Attribut in einer Produktionsumgebung **nie** auf "true" festgelegt sein. Eine ausführlichere Erläuterung zu diesem Thema finden Sie im Blogbeitrag von [Scott Guthrie](https://weblogs.asp.net/scottgu/). [führen Sie keine Produktions ASP.NET Anwendungen mit aktiviertem `debug="true"`](https://weblogs.asp.net/scottgu/442448)aus.

### <a name="custom-errors-and-tracing"></a>Benutzerdefinierte Fehler und Ablauf Verfolgung

Wenn ein nicht behandelter Ausnahme Fehler in einer ASP.NET-Anwendung auftritt, wird eine Blase zur Laufzeit hochskalieren, zu der drei Dinge passieren:

- Eine generische Lauf Zeit Fehlermeldung wird angezeigt. Auf dieser Seite wird dem Benutzer mitgeteilt, dass ein Laufzeitfehler aufgetreten ist, aber keine Details zum Fehler bereitgestellt werden.
- Eine Ausnahme Detail Meldung wird angezeigt, die Informationen zu der soeben ausgelösten Ausnahme enthält.
- Eine benutzerdefinierte Fehlerseite wird angezeigt. Hierbei handelt es sich um eine von Ihnen erstellte ASP.NET-Seite, auf der eine beliebige gewünschte Nachricht angezeigt wird.

Was geschieht, wenn eine nicht behandelte Ausnahme auftritt, hängt vom [`<customErrors>` Abschnitt](https://msdn.microsoft.com/library/h0hfz6fc.aspx)der `Web.config` Datei ab.

Beim entwickeln und Testen einer Anwendung können Details zu jeder Ausnahme im Browser angezeigt werden. Das Anzeigen von Ausnahme Details in einer Anwendung in der Produktion stellt jedoch ein potenzielles Sicherheitsrisiko dar. Außerdem ist es nicht Schmeichelhafter, und Ihre Website sieht nicht mehr aus. Im Idealfall zeigt eine Webanwendung in der Entwicklungsumgebung im Falle einer nicht behandelten Ausnahme die Details der Ausnahme an, während die gleiche Anwendung in der Produktionsumgebung eine benutzerdefinierte Fehlerseite anzeigt.

> [!NOTE]
> In der Standardeinstellung für den `<customErrors>` Abschnitt wird die Ausnahme Details nur angezeigt, wenn die Seite über "localhost" aufgerufen wird. andernfalls wird die allgemeine Lauf Zeit Fehlerseite angezeigt. Das ist nicht ideal, aber es wird sicherzustellen, dass das Standardverhalten keine Ausnahme Details für nicht lokale Besucher offen gibt. In einem zukünftigen Tutorial wird der `<customErrors>` Abschnitt ausführlicher erläutert und veranschaulicht, wie eine benutzerdefinierte Fehlerseite angezeigt wird, wenn ein Fehler in der Produktionsumgebung auftritt.

Ein weiteres ASP.NET Feature, das während der Entwicklung nützlich ist, ist die Ablauf Verfolgung Wenn diese Option aktiviert ist, werden Informationen zu jeder eingehenden Anforderung aufgezeichnet, und es wird eine besondere Webseite (`Trace.axd`) zum Anzeigen der aktuellen Anforderungs Details bereitstellt. Sie können die Ablauf Verfolgung über das`<trace>`- [Element](https://msdn.microsoft.com/library/6915t83k.aspx) in `Web.config`aktivieren und konfigurieren.

Wenn Sie die Ablauf Verfolgung aktivieren, stellen Sie sicher, dass Sie in der Produktion deaktiviert ist. Da die Ablauf Verfolgungs Informationen Cookies, Sitzungsdaten und andere potenziell vertrauliche Informationen enthalten, ist es wichtig, die Ablauf Verfolgung in der Produktionsumgebung zu deaktivieren. Die gute Nachricht ist, dass die Ablauf Verfolgung standardmäßig deaktiviert ist und die `Trace.axd` Datei nur über "localhost" zugänglich ist. Wenn Sie diese Standardeinstellungen in der Entwicklung ändern, stellen Sie sicher, dass Sie in der Produktionsumgebung wieder ausgeschaltet werden.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniken zum Verwalten separater Konfigurationsinformationen

Durch die unterschiedlichen Konfigurationseinstellungen in den Entwicklungs-und Produktionsumgebungen wird der Bereitstellungs Prozess erschwert. In den vorherigen beiden Tutorials hat der Bereitstellungs Prozess alle erforderlichen Dateien aus der Entwicklung in die Produktion kopiert, aber dieser Ansatz funktioniert nur, wenn die Konfigurationsinformationen in beiden Umgebungen identisch sind. Es gibt eine Reihe von Techniken zum Bereitstellen einer Anwendung mit unterschiedlichen Konfigurationsinformationen. Wir katalogisieren einige dieser Optionen für gehostete Webanwendungen.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Manuelles Bereitstellen der Konfigurationsdatei für die Produktionsumgebung

Der einfachste Ansatz besteht darin, zwei Versionen der `Web.config` Datei beizubehalten: eine für die Entwicklungsumgebung und eine für die Produktionsumgebung. Beim Bereitstellen eines Standorts in der Produktionsumgebung müssen alle Dateien auf den Produktionsserver in der Entwicklungsumgebung kopiert werden, *mit Ausnahme* der `Web.config` Datei. Stattdessen wird die Umgebungs spezifische `Web.config` Datei in die Produktion kopiert.

Diese Vorgehensweise ist nicht sehr ausgereift, aber Sie lässt sich leicht implementieren, da Konfigurationsinformationen selten geändert werden. Dies funktioniert am besten für Anwendungen mit einem kleinen Entwicklungsteam, die auf einem einzelnen Webserver gehostet werden und deren Konfigurationsinformationen selten geändert werden. Dies ist bei der manuellen Bereitstellung der Anwendungs Dateien mit einem eigenständigen FTP-Client möglich. Wenn Sie das Tool "Website kopieren" oder "veröffentlichen" von Visual Studio verwenden, müssen Sie zunächst die Bereitstellungs spezifische `Web.config` Datei mit der produktionsspezifischen Datei vor der Bereitstellung auslagern und Sie nach Abschluss der Bereitstellung erneut austauschen.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Ändern der Konfiguration während der Erstellung oder des Bereitstellungs Prozesses

Die bisherigen Diskussionen haben einen Ad-hoc-Build und einen Bereitstellungs Prozess angenommen. Viele größere Softwareprojekte weisen formalisierte Prozesse auf, die Open Source-, Heim-oder Drittanbieter Tools nutzen. Bei solchen Projekten können Sie den Build-oder Bereitstellungs Prozess wahrscheinlich so anpassen, dass die Konfigurationsinformationen entsprechend geändert werden, bevor Sie in die Produktion übermittelt werden. Wenn Sie Ihre Webanwendung mithilfe von [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [nant](http://nant.sourceforge.net/)oder einem anderen Buildtool erstellen, können Sie wahrscheinlich einen Buildschritt hinzufügen, um die `Web.config` Datei so zu ändern, dass Sie die produktionsspezifischen Einstellungen einschließt. Oder der Bereitstellungs Workflow konnte Programm gesteuert eine Verbindung mit dem Quell Code Verwaltungs Server herstellen und die entsprechende `Web.config` Datei abrufen.

Der tatsächliche Ansatz, die entsprechenden Konfigurationsinformationen in die Produktion zu bringen, variiert je nach Ihren Tools und dem Workflow. Daher wird dieses Thema nicht weiter erläutert. Wenn Sie ein beliebtes Buildtool wie MSBuild oder nant verwenden, finden Sie Bereitstellungs Artikel und Lernprogramme, die für diese Tools spezifisch sind, über eine Websuche.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Verwalten von Konfigurations unterschieden über das Webbereitstellungs Projekt-Add-in

In 2006 hat Microsoft das Webentwicklungs Projekt-Add-in für Visual Studio 2005 veröffentlicht. Ein Add-in für Visual Studio 2008 wurde in 2008 veröffentlicht. Mit diesem Add-in können ASP.NET-Entwickler neben dem Webanwendungs Projekt ein separates Webbereitstellungs Projekt erstellen, das bei der Erstellung die Webanwendung explizit kompiliert und die Dateien kopiert, die in einem lokalen Ausgabeverzeichnis bereitgestellt werden sollen. Das Webanwendungs Projekt verwendet MSBuild im Hintergrund.

Standardmäßig wird die `Web.config` Datei der Entwicklungsumgebung in das Ausgabeverzeichnis kopiert, Sie können jedoch das Webbereitstellungs Projekt einrichten, um das

Konfigurationsinformationen, die auf folgende Weise in dieses Verzeichnis kopiert werden:

- Über `Web.config` Datei Abschnitts Ersetzung, in der Sie den zu ersetzenden Abschnitt und eine XML-Datei angeben, die den Ersetzungstext enthält.
- Durch Bereitstellen eines Pfads zu einer externen Konfigurations Quelldatei. Wenn diese Option ausgewählt ist, kopiert das Webbereitstellungs Projekt eine bestimmte `Web.config` Datei in das Ausgabeverzeichnis (anstelle der in der Entwicklungsumgebung verwendeten `Web.config` Datei).
- Durch Hinzufügen von benutzerdefinierten Regeln zur MSBuild-Datei, die vom Webbereitstellungs Projekt verwendet werden.

Erstellen Sie zum Bereitstellen der Webanwendung das Webbereitstellungs Projekt, und kopieren Sie dann die Dateien aus dem Ausgabeordner des Projekts in die Produktionsumgebung.

Weitere Informationen zur Verwendung des Webbereitstellungs Projekts finden Sie im Artikel zu den [Webbereitstellungs Projekten](https://msdn.microsoft.com/magazine/cc163448.aspx) aus dem [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)vom April 2007. Sie können auch die Links im Abschnitt Weitere Informationen am Ende dieses Tutorials lesen.

> [!NOTE]
> Das Webbereitstellungs Projekt kann nicht mit Visual Web Developer verwendet werden, da das Webbereitstellungs Projekt als Visual Studio-Add-in implementiert ist und die Visual Studio Express Editionen (einschließlich Visual Web Developer) keine Add-Ins unterstützen.

## <a name="summary"></a>Zusammenfassung

Die externen Ressourcen und das Verhalten einer Webanwendung in der Entwicklung unterscheiden sich in der Regel von der Verwendung der gleichen Anwendung in der Produktionsumgebung. Beispielsweise unterscheiden sich Daten bankverbindungs Zeichenfolgen, Kompilierungsoptionen und das Verhalten bei Auftreten einer nicht behandelten Ausnahme häufig zwischen Umgebungen. Beim Bereitstellungs Prozess müssen diese Unterschiede berücksichtigt werden. Wie in diesem Tutorial erläutert, besteht der einfachste Ansatz darin, eine alternative Konfigurationsdatei manuell in die Produktionsumgebung zu kopieren. Wenn Sie das Add-in für das Webbereitstellungs Projekt oder einen formalisierten Build-oder Bereitstellungs Prozess verwenden, der solche Anpassungen unterstützen kann, sind elegante Lösungen möglich.

Fröhliche Programmierung!

### <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Erläuterte Verbindungs Zeichenfolgen](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Daten bankverbindungs Zeichenfolgen @ connectionStrings.com](http://www.connectionstrings.com/)
- [Produktions ASP.NET Anwendungen nicht mit aktiviertem `debug="true"` ausführen](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Ordnungsgemäße Reaktion auf nicht behandelte Ausnahmen: anzeigen benutzerfreundlicher Fehlerseiten](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gewusst wie: Verwenden eines Visual Studio 2008-Webbereitstellungs Projekts](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Wichtige Konfigurationseinstellungen beim Bereitstellen einer Datenbank](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008-Webbereitstellungs Projekte herunter](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) laden | [Visual Studio 2005-Webbereitstellungs Projekte herunterladen](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Vs 2008 Webbereitstellungs Projekte](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [vs 2008 Webbereitstellungs Projekt-Unterstützung veröffentlicht](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Webbereitstellungsprojekte](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-your-site-using-visual-studio-cs.md)
> [Weiter](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
