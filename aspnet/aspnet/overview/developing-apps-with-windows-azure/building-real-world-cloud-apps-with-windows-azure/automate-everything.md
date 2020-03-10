---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatisieren Sie alles (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: e741a753a36ebdaefbff8eee0b38911785c716ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472677"
---
# <a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatisieren Sie alles (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Eine Einführung in das e-Book finden Sie [im ersten Kapitel](introduction.md).

Die ersten drei Muster, die wir betrachten werden, gelten tatsächlich für alle Softwareentwicklungsprojekte, aber insbesondere für cloudumgebungen. Dieses Muster dient zum Automatisieren von Entwicklungsaufgaben. Es ist ein wichtiges Thema, da manuelle Prozesse langsam und fehleranfällig sind. Wenn Sie so viele wie möglich automatisieren, können Sie einen schnellen, zuverlässigen und agilen Workflow einrichten. Es ist für die cloudentwicklung eindeutig, da Sie problemlos viele Aufgaben automatisieren können, die in einer lokalen Umgebung schwierig oder gar nicht automatisiert werden können. Sie können z. b. ganze Testumgebungen einrichten, z. b. neue Webserver-und Back-End-VMS, Datenbanken, BLOB-Speicher (Dateispeicher), Warteschlangen usw.

## <a name="devops-workflow"></a>Devops-Workflow

Immer mehr hören Sie den Begriff "devops". Der Begriff wurde aus einer Kennung entwickelt, die Sie zum effizienten entwickeln von Software für Entwicklungs-und Betriebs Aufgaben entwickeln müssen. Die Art des Workflows, den Sie aktivieren möchten, ist eine Anwendung, in der Sie eine App entwickeln, bereitstellen, in der Produktionsumgebung verwenden, Sie in Reaktion auf die gewonnenen Inhalte ändern und den Kreis schnell und zuverlässig wiederholen können.

Einige erfolgreiche cloudentwicklungteams stellen mehrmals täglich eine Live Umgebung bereit. Das Azure-Team, das für die Bereitstellung eines wichtigen Updates jeden Zeitraum von 2-3 Monaten verwendet wird, aber jetzt werden kleinere Updates alle 2-3 Tage und die Haupt Releases alle 2-3 Wochen freigegeben. Der Einstieg in diese Kadenz hilft Ihnen, auf Kundenfeedback zu investieren.

Um dies zu erreichen, müssen Sie einen Entwicklungs-und Bereitstellungs Zyklen aktivieren, der wiederholbar, zuverlässig und vorhersagbar ist und eine geringe Zyklen Zeit aufweist.

![Devops-Workflow](automate-everything/_static/image1.png)

Anders ausgedrückt: der Zeitraum zwischen dem Zeitpunkt, zu dem Sie eine Idee für eine Funktion haben, und dem Zeitpunkt, zu dem die Kunden Sie verwenden, und das Bereitstellen von Feedback muss so kurz wie möglich sein. Die ersten drei Muster – automatisieren alles, die Quell Code Verwaltung und die Continuous Integration und Übermittlung. es gelten alle bewährten Methoden, die wir empfehlen, um diese Art von Prozess zu ermöglichen.

## <a name="azure-management-scripts"></a>Azure-Verwaltungs Skripts

In der [Einführung in dieses e-book](introduction.md)haben Sie die webbasierte Konsole, die Azure-Verwaltungsportal, gesehen. Im Verwaltungs Portal können Sie alle Ressourcen, die Sie in Azure bereitgestellt haben, überwachen und verwalten. Es ist eine einfache Möglichkeit, Dienste wie Web-Apps und VMS zu erstellen und zu löschen, diese Dienste zu konfigurieren, den Dienst Vorgang zu überwachen usw. Es ist ein großartiges Tool, aber die Verwendung ist ein manueller Prozess. Wenn Sie eine Produktionsanwendung einer beliebigen Größe und insbesondere in einer Team Umgebung entwickeln möchten, empfiehlt es sich, die Portal-Benutzeroberfläche zu durchlaufen, um sich mit Azure vertraut zu machen und die Prozesse zu automatisieren, die wiederholt ausgeführt werden.

Fast alles, was Sie manuell im Verwaltungs Portal oder in Visual Studio ausführen können, kann auch durch Aufrufen der Rest-Verwaltungs-API erfolgen. Sie können Skripts mithilfe von [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)schreiben, oder Sie können ein Open-Source-Framework verwenden, z. b. [Chef](http://www.opscode.com/chef/) oder [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Sie können auch das bash-Befehlszeilen Tool in einer Mac-oder Linux-Umgebung verwenden. Azure verfügt über Skript-APIs für alle diese verschiedenen Umgebungen und verfügt über eine [.NET-Verwaltungs-API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) für den Fall, dass Sie Code anstelle eines Skripts schreiben möchten.

Für die Korrektur der IT-APP haben wir einige Windows PowerShell-Skripts erstellt, mit denen die Prozesse zum Erstellen einer Testumgebung und zum Bereitstellen des Projekts in dieser Umgebung automatisiert werden, und wir werden einige der Inhalte dieser Skripts überprüfen.

## <a name="environment-creation-script"></a>Umgebungs Erstellungs Skript

Das erste Skript, das wir betrachten, heißt " *New-AzureWebsiteEnv. ps1*". Sie erstellt eine Azure-Umgebung, in der Sie die Korrektur der IT-App für Tests bereitstellen können. Das Skript führt die folgenden Hauptaufgaben aus:

- Erstellen einer Web-App
- Erstellen Sie ein Speicherkonto. (Erforderlich für blobund Warteschlangen, wie Sie in späteren Kapiteln sehen werden.)
- Erstellen Sie einen SQL-Datenbankserver und zwei Datenbanken: eine Anwendungsdatenbank und eine Mitgliedschafts Datenbank.
- Speichern Sie Einstellungen in Azure, die von der APP für den Zugriff auf das Speicherkonto und die Datenbanken verwendet werden.
- Erstellen Sie Einstellungen Dateien, die zur Automatisierung der Bereitstellung verwendet werden.

### <a name="run-the-script"></a>Führen Sie das Skript aus.

> [!NOTE]
> Dieser Teil des Kapitels zeigt Beispiele für Skripts und die Befehle, die Sie eingeben, um Sie auszuführen. Dies ist eine Demo, die Ihnen nicht alles bietet, was Sie wissen müssen, um die Skripts auszuführen. Schritt-für-Schritt-Anleitungen finden Sie unter [Anhang: die Anwendung zum Beheben von IT-Beispielen](the-fix-it-sample-application.md#deploybase).

Zum Ausführen eines PowerShell-Skripts, mit dem Azure-Dienste verwaltet werden, müssen Sie die Azure PowerShell-Konsole installieren und für die Zusammenarbeit mit Ihrem Azure-Abonnement konfigurieren. Nach der Einrichtung können Sie das Skript zum Erstellen der IT-Umgebung mit einem Befehl wie dem folgenden ausführen:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Der `Name`-Parameter gibt den Namen an, der beim Erstellen der Datenbank und der Speicher Konten verwendet werden soll, und der Parameter `SqlDatabasePassword` gibt das Kennwort für das Administrator Konto an, das für die SQL-Datenbank erstellt wird. Es gibt weitere Parameter, die Sie später verwenden können.

![PowerShell-Fenster](automate-everything/_static/image2.png)

Nachdem das Skript abgeschlossen wurde, können Sie im Verwaltungs Portal sehen, was erstellt wurde. Sie finden zwei Datenbanken:

![Databases](automate-everything/_static/image3.png)

Ein Speicherkonto:

![Speicherkonto](automate-everything/_static/image4.png)

Und eine Web-App:

![Website](automate-everything/_static/image5.png)

Auf der Registerkarte **Konfigurieren** für die Web-App können Sie sehen, dass die Speicherkonto Einstellungen und SQL-Daten bankverbindungs Zeichenfolgen für die Korrektur der IT-App eingerichtet sind.

![appSettings und connectionStrings](automate-everything/_static/image6.png)

Der Ordner *Automation* enthält jetzt auch eine *&lt;Website Name&gt;. pubxml* -Datei. In dieser Datei werden Einstellungen gespeichert, die von MSBuild zum Bereitstellen der Anwendung in der soeben erstellten Azure-Umgebung verwendet werden. Beispiel:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Wie Sie sehen können, hat das Skript eine vollständige Testumgebung erstellt, und der gesamte Prozess wird in ungefähr 90 Sekunden ausgeführt.

Wenn ein anderer Benutzer in Ihrem Team eine Testumgebung erstellen möchte, kann er das Skript einfach ausführen. Dies ist nicht nur schnell, sondern auch, dass Sie sicher sein können, dass Sie eine Umgebung verwenden, die mit der von Ihnen verwendeten Umgebung identisch ist. Sie konnten nicht so sicher sein, dass alle Dinge manuell über die Benutzeroberfläche des Verwaltungs Portals festgelegt wurden.

### <a name="a-look-at-the-scripts"></a>Ein Blick auf die Skripts

Es gibt tatsächlich drei Skripts, die dies tun. Sie werden von der Befehlszeile aus aufgerufen, und die beiden anderen werden automatisch verwendet, um einige der Aufgaben auszuführen:

- *New-AzureWebSiteEnv. ps1* ist das Hauptskript.

    - *New-AzureStorage. ps1* erstellt das Speicherkonto.
    - *New-AzureSql. ps1* erstellt die Datenbanken.

### <a name="parameters-in-the-main-script"></a>Parameter im Hauptskript

Das Hauptskript, *New-AzureWebSiteEnv. ps1*, definiert mehrere Parameter:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Es sind zwei Parameter erforderlich:

- Der Name der Web-App, die vom Skript erstellt wird. (Dies wird auch für die URL: `<name>.azurewebsites.net`verwendet.)
- Das Kennwort für den neuen Administrator des Datenbankservers, den das Skript erstellt.

Mit optionalen Parametern können Sie den Speicherort des Rechenzentrums (standardmäßig "USA, Westen"), den Namen des Datenbankserver Administrators (standardmäßig "dbuser") und eine Firewallregel für den Datenbankserver angeben.

### <a name="create-the-web-app"></a>Erstellen der Web-App

Als erstes erstellt das Skript die Web-App durch Aufrufen des Cmdlets "`New-AzureWebsite`" und übergibt dabei die Parameterwerte für Web-App-Name und-Speicherort an die folgenden:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Erstellen des Speicherkontos

Anschließend führt das Hauptskript das Skript *New-AzureStorage. ps1* aus und gibt " *&lt;Website Name&gt;* Storage" für den Namen des Speicher Kontos und denselben Rechenzentrums Standort wie die Web-App an.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*New-AzureStorage. ps1* Ruft das `New-AzureStorageAccount` Cmdlet auf, um das Speicherkonto zu erstellen, und gibt den Kontonamen und die Zugriffsschlüssel Werte zurück. Die Anwendung benötigt diese Werte, um auf die BLOB-und Warteschlangen im Speicherkonto zuzugreifen.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Sie möchten möglicherweise nicht immer ein neues Speicherkonto erstellen. Sie können das Skript verbessern, indem Sie einen Parameter hinzufügen, der ihn optional anweist, ein vorhandenes Speicherkonto zu verwenden.

### <a name="create-the-databases"></a>Erstellen der Datenbanken

Das Hauptskript führt dann das Daten Bank Erstellungs Skript *New-AzureSql. ps1*aus, nachdem die Standarddatenbank und die firewallregelnamen festgelegt wurden:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Das Daten Bank Erstellungs Skript ruft die IP-Adresse des Entwicklers ab und legt eine Firewallregel fest, damit der Entwicklungs Computer eine Verbindung mit dem Server herstellen und ihn verwalten kann. Das Skript für die Daten Bank Erstellung führt dann verschiedene Schritte aus, um die Datenbanken einzurichten:

- Der Server wird mit dem `New-AzureSqlDatabaseServer`-Cmdlet erstellt.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Erstellt Firewallregeln, um dem Entwicklungs Computer die Verwaltung des Servers und das Herstellen einer Verbindung mit der Web-App zu ermöglichen. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Erstellt mithilfe des-Cmdlets `New-AzureSqlDatabaseServerContext` einen Daten Bank Kontext, der den Servernamen und die Anmelde Informationen enthält.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` ist eine Funktion im Skript, mit der das `ConvertTo-SecureString`-Cmdlet aufgerufen wird, um das Kennwort zu verschlüsseln, und gibt ein `PSCredential` Objekt zurück, den gleichen Typ, den das `Get-Credential`-Cmdlet zurückgibt.
- Erstellt die Anwendungsdatenbank und die Mitgliedschafts Datenbank mithilfe des `New-AzureSqlDatabase`-Cmdlets.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Ruft eine lokal definierte Funktion auf, um eine Verbindungs Zeichenfolge für jede Datenbank zu erstellen. Diese Verbindungs Zeichenfolgen werden von der Anwendung für den Zugriff auf die Datenbanken verwendet. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    "Get-sqlazuredatabaseconnectionstring" ist eine im Skript definierte Funktion, die die Verbindungs Zeichenfolge aus den für Sie angegebenen Parameterwerten erstellt.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Gibt eine Hash Tabelle mit dem Namen des Datenbankservers und den Verbindungs Zeichenfolgen zurück.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Die Korrektur der IT-App verwendet separate Mitgliedschafts-und Anwendungsdatenbanken. Es ist auch möglich, sowohl Mitgliedschafts-als auch Anwendungsdaten in einer einzelnen Datenbank zu platzieren.

### <a name="store-app-settings-and-connection-strings"></a>Speichern von App-Einstellungen und Verbindungs Zeichenfolgen

Azure verfügt über eine Funktion, mit der Sie Einstellungen und Verbindungs Zeichenfolgen speichern können, die automatisch überschreiben, was an die Anwendung zurückgegeben wird, wenn versucht wird, die `appSettings` oder `connectionStrings` Sammlungen in der Datei "Web. config" zu lesen. Dies ist eine Alternative zum Anwenden von [Web. config-Transformationen](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) bei der Bereitstellung von. Weitere Informationen finden Sie unter [Speichern von sensiblen Daten in Azure](source-control.md#appsettings) später in diesem e-book.

Das Skript für die Umgebungs Erstellung speichert in Azure alle `appSettings`-und `connectionStrings` Werte, die die Anwendung für den Zugriff auf das Speicherkonto und die Datenbanken benötigt, wenn Sie in Azure ausgeführt wird.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) ist ein telemetrieframework, das wir im Kapitel [Überwachung und Telemetrie](monitoring-and-telemetry.md) veranschaulichen. Das Umgebungs Erstellungs Skript startet auch die Web-App neu, um sicherzustellen, dass die neuen Relic-Einstellungen übernommen werden.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Vorbereiten der Bereitstellung

Am Ende des Prozesses Ruft das Umgebungs Erstellungs Skript zwei Funktionen auf, um Dateien zu erstellen, die vom Bereitstellungs Skript verwendet werden.

Eine dieser Funktionen erstellt ein Veröffentlichungs Profil *(&lt;Website Name&gt;. pubxml* -Datei). Der Code Ruft die Azure-Rest-API auf, um die Veröffentlichungs Einstellungen zu erhalten, und speichert die Informationen in einer *publishsettings* -Datei. Anschließend werden die Informationen aus dieser Datei zusammen mit einer Vorlagen Datei (*pubxml. Template*) verwendet, um die *pubxml* -Datei zu erstellen, die das Veröffentlichungs Profil enthält. Dieser zweistufige Prozess simuliert, was Sie in Visual Studio tun: Laden Sie eine *publishsettings* -Datei herunter, und importieren Sie diese, um ein Veröffentlichungs Profil zu erstellen.

Die andere Funktion verwendet eine andere Vorlagen Datei ("Website-Environment. Template"), um eine *Website-Environment. XML* -Datei zu erstellen, die Einstellungen enthält, die das Bereitstellungs Skript zusammen mit der *pubxml* -Datei verwendet.

### <a name="troubleshooting-and-error-handling"></a>Problembehandlung und Fehlerbehandlung

Skripts sind z. b. Programme: Sie können fehlschlagen, und wenn Sie wissen, wie Sie den Fehler und die Ursache des Fehlers ermitteln können. Aus diesem Grund ändert das Umgebungs Erstellungs Skript den Wert der `VerbosePreference` Variable von `SilentlyContinue` in `Continue`, sodass alle ausführlichen Meldungen angezeigt werden. Außerdem wird der Wert der `ErrorActionPreference` Variable von `Continue` in `Stop`geändert, damit das Skript auch dann beendet wird, wenn es nicht abschließende Fehler feststellt:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Bevor es funktioniert, speichert das Skript die Startzeit, sodass es die verstrichene Zeit berechnen kann:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Nachdem die Arbeit abgeschlossen ist, zeigt das Skript die verstrichene Zeit an:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Und für jeden Schlüssel Vorgang schreibt das Skript ausführliche Meldungen, z. b.:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Bereitstellungsskript

Das Skript " *New-AzureWebsiteEnv. ps1* " für die Erstellung der Umgebung führt das Skript " *Publish-AzureWebsite. ps1* " für die Anwendungs Bereitstellung aus.

Das Bereitstellungs Skript ruft den Namen der Web-App aus der *Website-Environment. XML* -Datei ab, die durch das Skript zur Erstellung der Umgebung erstellt wurde.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Das Benutzer Kennwort für die Bereitstellung wird aus der *publishsettings* -Datei abgerufen:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Er führt den [MSBuild](http://msbuildbook.com/) -Befehl aus, der das Projekt erstellt und bereitstellt:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Wenn Sie den `Launch`-Parameter in der Befehlszeile angegeben haben, wird das `Show-AzureWebsite`-Cmdlet aufgerufen, um den Standardbrowser für die Website-URL zu öffnen.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Sie können das Bereitstellungs Skript mit einem Befehl wie dem folgenden ausführen:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Wenn dies der Fall ist, öffnet sich der Browser mit der Website, die in der Cloud ausgeführt wird, unter der `<websitename>.azurewebsites.net`-URL.

![Reparieren der in Windows Azure bereitgestellten IT-App](automate-everything/_static/image7.png)

## <a name="summary"></a>Zusammenfassung

Mit diesen Skripts können Sie sicher sein, dass dieselben Schritte immer in derselben Reihenfolge ausgeführt werden, indem Sie dieselben Optionen verwenden. Dadurch wird sichergestellt, dass jeder Entwickler im Team etwas nicht verpasst oder etwas durcheinander bringen kann oder etwas benutzerdefiniertes auf seinem eigenen Computer bereitstellt, das nicht auf die gleiche Weise in der Umgebung eines anderen Teammitglieds oder in der Produktionsumgebung funktioniert.

Auf ähnliche Weise können Sie die meisten Azure-Verwaltungsfunktionen automatisieren, die Sie im Verwaltungs Portal ausführen können, indem Sie die Rest-API, Windows PowerShell-Skripts, eine .NET-Programmiersprache-API oder ein Bash-Hilfsprogramm verwenden, das Sie unter Linux oder Mac ausführen können.

Im [nächsten Kapitel](source-control.md) betrachten wir den Quellcode und erläutern, warum es wichtig ist, Ihre Skripts in Ihr Quellcoderepository einzubeziehen.

## <a name="resources"></a>Ressourcen

- [Installieren und konfigurieren Sie Windows PowerShell für Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Erläutert, wie Sie die Azure PowerShell-Cmdlets installieren und das Zertifikat installieren, das Sie auf Ihrem Computer benötigen, um Ihr Azure-Konto zu verwalten. Dies ist ein idealer Ausgangspunkt, da er auch Links zu Ressourcen für das Erlernen von PowerShell selbst enthält.
- [Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). WindowsAzure.com-Portal zu Ressourcen zum Entwickeln von Skripts für die Verwaltung von Azure-Diensten mit Links zu den Tutorials zu den ersten Schritten, der Cmdlet-Referenz Dokumentation und dem Quellcode sowie Beispiel Skripts
- [Wochenend Skripterstellung: Einstieg in Azure und PowerShell](https://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). In einem für Windows PowerShell dedizierten Blog bietet dieser Beitrag eine gute Einführung in die Verwendung von PowerShell für Azure-Verwaltungsfunktionen.
- [Installieren und konfigurieren Sie die plattformübergreifende Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Tutorial zu den ersten Schritten für ein Azure Scripting Framework, das unter Mac und Linux sowie in Windows-Systemen funktioniert.
- [Abschnitt über Befehlszeilen Tools im Thema herunterladen von Azure sdert und Tools](https://azure.microsoft.com/downloads/). Portal Seite für Dokumentation und Downloads im Zusammenhang mit den Befehlszeilen Tools für Azure.
- [Automating everything with the Azure Management Libraries and .NET (Automatisierung mithilfe der Azure-Verwaltungsbibliotheken und .NET, in englischer Sprache)](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman führt die .NET-Verwaltungs-API für Azure ein.
- [Veröffentlichung in Entwicklungs- und Testumgebungen mithilfe von Windows PowerShell-Skripts](https://msdn.microsoft.com/library/azure/dn642480.aspx). In der MSDN-Dokumentation wird erläutert, wie Sie Veröffentlichungs Skripts verwenden, die von Visual Studio automatisch für Webprojekte generiert werden.
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio-Erweiterung, mit der Sprachunterstützung für Windows PowerShell in Visual Studio hinzugefügt wird.

> [!div class="step-by-step"]
> [Zurück](introduction.md)
> [Weiter](source-control.md)
