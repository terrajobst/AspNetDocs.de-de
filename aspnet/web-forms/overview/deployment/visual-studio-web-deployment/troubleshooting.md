---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'ASP.net-Webbereitstellung mithilfe von Visual Studio: Problembehandlung | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter zu Azure App Service.
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b42476fca18b04f4557a216ee205cfd9220023e8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623586"
---
# <a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>ASP.net-Webbereitstellung mithilfe von Visual Studio: Problembehandlung

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> In dieser tutorialreihe erfahren Sie, wie Sie eine ASP.NET-Webanwendung bereitstellen (veröffentlichen), um Web-Apps oder einen Drittanbieter-Hostinganbieter mithilfe von Visual Studio 2012 oder Visual Studio 2010 zu Azure App Service. Weitere Informationen zur Reihe finden Sie [im ersten Tutorial der Reihe](introduction.md).

Auf dieser Seite werden einige häufige Probleme beschrieben, die auftreten können, wenn Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio bereitstellen. Mindestens eine mögliche Ursache und entsprechende Lösungen werden bereitgestellt.

Die gezeigten Szenarien gelten sowohl für Azure als auch für Hostinganbieter von Drittanbietern. Weitere Informationen zur Problembehandlung von Web-Apps in Azure App Service finden Sie in den folgenden Ressourcen:

- [Problembehandlung in einer Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Überwachen von Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Ankündigung der Veröffentlichung von Windows Azure SDK 2,0 für .net](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (Blog von scottgu), das zeigt, wie Sie Diagnoseprotokolle in Visual Studio erhalten.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Server Fehler in "/"-Anwendung-aktuelle benutzerdefinierte Fehler Einstellungen verhindern, dass Details des Fehlers Remote angezeigt werden.

### <a name="scenario"></a>Szenario

Nachdem Sie einen Standort auf einem Remote Host bereitgestellt haben, erhalten Sie eine Fehlermeldung, in der die customErrors-Einstellung in der Datei "Web. config" erwähnt wird, jedoch nicht die tatsächliche Ursache des Fehlers:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig zeigt ASP.net nur dann ausführliche Fehlerinformationen an, wenn Ihre Webanwendung auf dem lokalen Computer ausgeführt wird. Im Allgemeinen ist es nicht ratsam, ausführliche Fehlerinformationen anzuzeigen, wenn Ihre Webanwendung über das Internet öffentlich verfügbar ist, da Hacker diese Informationen möglicherweise verwenden können, um Schwachstellen in der Anwendung zu finden. Wenn Sie jedoch einen Standort oder Updates für einen Standort bereitstellen, tritt manchmal ein Fehler auf, und Sie müssen die tatsächliche Fehlermeldung erhalten.

Damit die Anwendung detaillierte Fehlermeldungen anzeigen kann, wenn Sie auf dem Remote Host ausgeführt wird, bearbeiten Sie die Datei Web. config, um den customErrors-Modus auf OFF festzulegen, stellen Sie die Anwendung erneut bereit, und führen Sie die Anwendung erneut aus:

1. Wenn die Datei "Web. config" der Anwendung ein customErrors-Element im System. Web-Element aufweist, ändern Sie das Mode-Attribut in "Off". Andernfalls fügen Sie ein customErrors-Element im System. Web-Element hinzu, wobei das Mode-Attribut auf "Off" festgelegt ist, wie im folgenden Beispiel gezeigt: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Stellen Sie die Anwendung bereit.
3. Führen Sie die Anwendung aus, und wiederholen Sie den Vorgang, der zum Auftreten des Fehlers geführt hat. Nun können Sie sehen, welche Fehlermeldung tatsächlich vorliegt.
4. Wenn Sie den Fehler behoben haben, stellen Sie die ursprüngliche Einstellung customErrors wieder her, und stellen Sie die Anwendung erneut bereit.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Das Erstellen/Schatten kopieren von "condesouniversity" ist nicht möglich, wenn diese Datei bereits vorhanden ist.

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, ein Projekt in Visual Studio auszuführen, erhalten Sie eine Fehlerseite mit einer Meldung wie im folgenden Beispiel:

Serverfehler in '/' Anwendung. Das Erstellen/Schatten kopieren von "condesouniversity" ist nicht möglich, wenn diese Datei bereits vorhanden ist.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Warten Sie eine Minute, und aktualisieren Sie den Browser, oder kompilieren Sie die Website neu, und führen Sie Sie erneut aus.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Der Zugriff auf einer Webseite, die SQL Server Compact verwendet, wird verweigert.

### <a name="scenario"></a>Szenario

Wenn Sie einen Standort bereitstellen, der SQL Server Compact verwendet und Sie eine Seite an dem bereitgestellten Standort ausführen, der auf die Datenbank zugreift, wird die folgende Fehlermeldung angezeigt:

Zugriff verweigert. (Ausnahme von HRESULT: 0x80070005 (E\_AccessDenied))

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das Netzwerkdienst Konto auf dem Server muss in der Lage sein, systemeigene SQL Service Compact-Binärdateien zu lesen, die sich im Ordner " *bin\amd64* " oder " *bin\x86* " befinden, jedoch nicht über Leseberechtigungen für diese Ordner verfügen. Legen Sie für den Ordner *bin* die Berechtigung Lesen für Netzwerkdienst fest, und stellen Sie sicher, dass Sie die Berechtigungen auf Unterordner erweitern.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Die Konfigurationsdatei kann aufgrund unzureichender Berechtigungen nicht gelesen werden.

### <a name="scenario"></a>Szenario

Wenn Sie auf die Schaltfläche "veröffentlichen" von Visual Studio klicken, um eine Anwendung auf dem lokalen Computer in IIS bereitzustellen, schlägt die Veröffentlichung fehl, und im **Ausgabe** Fenster wird eine Fehlermeldung ähnlich der folgenden angezeigt:

Fehler beim Lesen der IIS-Konfigurationsdatei "Machine/Redirect". Die Identität, die diesen Vorgang ausführt, war... Fehler: die Konfigurationsdatei kann aufgrund unzureichender Berechtigungen nicht gelesen werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn Sie die One-Click-Veröffentlichung in IIS auf dem lokalen Computer verwenden möchten, müssen Sie Visual Studio mit Administrator Berechtigungen ausführen. Schließen Sie Visual Studio, und starten Sie es mit Administrator Berechtigungen neu.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Es konnte keine Verbindung mit dem Ziel Computer hergestellt werden... Verwenden des angegebenen Prozesses

### <a name="scenario"></a>Szenario

Wenn Sie auf die Visual Studio-Schaltfläche veröffentlichen klicken, um eine Anwendung bereitzustellen, schlägt die Veröffentlichung fehl, und im Fenster **Ausgabe** wird eine Fehlermeldung ähnlich der folgenden angezeigt:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Kommunikation mit dem Zielserver wird von einem Proxy Server unterbrochen. Wählen Sie in der Windows-Systemsteuerung oder in Internet Explorer die **Option Internet Optionen** aus, und klicken Sie auf die Registerkarte **Verbindungen** . Klicken Sie im Dialogfeld **Internet Eigenschaften** auf **LAN-Einstellungen**. Deaktivieren Sie im Dialogfeld **Einstellungen für lokales Netzwerk (LAN)** das Kontrollkästchen **Einstellungen automatisch erkennen** . Klicken Sie dann erneut auf die Schaltfläche veröffentlichen.

Wenn das Problem weiterhin besteht, wenden Sie sich an Ihren Systemadministrator, um zu bestimmen, was mit Proxy-oder Firewalleinstellungen möglich ist. Das Problem tritt auf, weil Web deploy einen nicht standardmäßigen Port für die Bereitstellung des Webverwaltungsdiensts verwendet (8172); bei anderen Verbindungen wird von Web deploy Port 80 verwendet. Wenn Sie für einen Drittanbieter-Hosting-Anbieter bereitstellen, verwenden Sie in der Regel den Webverwaltungsdienst.

## <a name="default-net-40-application-pool-does-not-exist"></a>Der .NET 4,0-Standard Anwendungs Pool ist nicht vorhanden.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung bereitstellen, für die die .NET Framework 4 erforderlich ist, wird die folgende Fehlermeldung angezeigt:

Der .NET 4,0-Standard Anwendungs Pool ist nicht vorhanden, oder die Anwendung konnte nicht hinzugefügt werden. Vergewissern Sie sich, dass ASP.NET 4,0 auf diesem Computer installiert ist.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist nicht in IIS installiert. Wenn es sich bei dem Server, auf dem Sie die Bereitstellung durchlaufen, um den Entwicklungs Computer handelt, auf dem Visual Studio 2010 installiert ist, wird ASP.NET 4 auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, auf dem Sie bereitstellen, eine Eingabeaufforderung mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Möglicherweise müssen Sie auch die .NET Framework Version des Standard Anwendungs Pools manuell festlegen. Weitere Informationen finden Sie im Tutorial bereitstellen in IIS als Test Umgebung in dieser Reihe.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Das Format der Initialisierungs Zeichenfolge entspricht nicht der Spezifikation, beginnend bei Index 0.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung mit One-Click-Veröffentlichung bereitgestellt haben, erhalten Sie die folgende Fehlermeldung, wenn Sie eine Seite ausführen, die auf die Datenbank zugreift:

Das Format der Initialisierungs Zeichenfolge entspricht nicht der Spezifikation, beginnend bei Index 0.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Öffnen Sie die Datei " *Web. config* " auf der bereitgestellten Website, und überprüfen Sie, ob die Verbindungs Zeichenfolgen-Werte mit `$(ReplaceableToken_`beginnen, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Wenn die Verbindungs Zeichenfolgen wie in diesem Beispiel aussehen, bearbeiten Sie die Projektdatei, und fügen Sie die folgende Eigenschaft zum PropertyGroup-Element für alle Buildkonfigurationen hinzu:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Stellen Sie die Anwendung dann erneut bereit.

## <a name="http-500-internal-server-error"></a>HTTP 500-Interner Server Fehler

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, wird die folgende Fehlermeldung ohne spezifische Informationen angezeigt, die auf die Ursache des Fehlers hinweisen:

HTTP-Fehler 500-Interner Server Fehler.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Es gibt viele Ursachen für 500-Fehler. eine mögliche Ursache, wenn Sie diese Tutorials befolgen, besteht darin, dass Sie ein XML-Element in einer der Web. config-Transformations Dateien an der falschen Stelle platzieren. Beispielsweise wird dieser Fehler angezeigt, wenn Sie die Transformation platzieren, die eine &lt;Location&gt;-Element unter &lt;System. Web&gt; anstelle von direkt unter &lt;Konfigurations&gt;einfügt. Sie können das Feature "Web. config Transform Preview" verwenden, um zu überprüfen, ob die Transformationen wie beabsichtigt funktionieren. Die Lösung, wenn Sie eine nicht ordnungsgemäß codierte Transformation finden, besteht darin, die Transformations Datei zu korrigieren und erneut bereitzustellen. Wenn ein Fehler nicht offensichtlich ist, kommentieren Sie Transformationen aus, und stellen Sie eine erneute Bereitstellung durch, um festzustellen, welcher Fehler 500 verursacht.

## <a name="http-50021-internal-server-error"></a>HTTP 500,21-interner Server Fehler

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, wird die folgende Fehlermeldung angezeigt:

HTTP-Fehler 500,21-interner Server Fehler. Der Handler "PageHandlerFactory-integriert" weist ein ungültiges Modul "managedpipelinehandler" in der Modulliste auf.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der von Ihnen bereitgestellte Standort hat das Ziel ASP.NET 4, aber ASP.NET 4 ist nicht in IIS auf dem Server registriert. Öffnen Sie auf dem-Server eine Eingabeaufforderung mit erhöhten Rechten, und registrieren Sie ASP.NET 4 durch Ausführen der folgenden Befehle:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Möglicherweise müssen Sie auch die .NET Framework Version des Standard Anwendungs Pools manuell festlegen. Weitere Informationen finden Sie im Tutorial bereitstellen in IIS als Test Umgebung in dieser Reihe.

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Fehler beim Öffnen SQL Server Express Datenbank in App-\_Daten.

### <a name="scenario"></a>Szenario

Sie haben die Verbindungs Zeichenfolge der Datei " *Web. config* " so aktualisiert, dass Sie auf eine SQL Server Express Datenbank als *MDF* -Datei in Ihrer *App\_Daten* Ordner verweist. Wenn Sie die Anwendung zum ersten Mal ausführen, wird die folgende Fehlermeldung angezeigt:

System. Data. SqlClient. SqlException: die von der Anmeldung angeforderte Datenbank "DatabaseName" kann nicht geöffnet werden. Die Anmeldung ist fehlgeschlagen.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Name der *MDF* -Datei kann nicht mit dem Namen einer SQL Server Express Datenbank identisch sein, die jemals auf dem Computer vorhanden war, auch wenn Sie die *MDF* -Datei der bereits vorhandenen Datenbank gelöscht haben. Ändern Sie den Namen der *MDF* -Datei in einen Namen, der noch nie als Datenbankname verwendet wurde, und ändern Sie die Datei *Web. config* so, dass der neue Name verwendet wird. Als Alternative können Sie mit [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) bereits vorhandene SQL Server Express Datenbanken löschen.

## <a name="model-compatibility-cannot-be-checked"></a>Die Modell Kompatibilität kann nicht geprüft werden.

### <a name="scenario"></a>Szenario

Sie haben die Verbindungs Zeichenfolge der Datei " *Web. config* " so aktualisiert, dass Sie auf eine neue SQL Server Express Datenbank verweist. Wenn Sie die Anwendung zum ersten Mal ausführen, wird die folgende Fehlermeldung angezeigt:

Die Modell Kompatibilität kann nicht überprüft werden, weil die Datenbank keine Modell Metadaten enthält. Stellen Sie sicher, dass IncludeMetadataConvention den dbmodelbuilder-Konventionen hinzugefügt wurde.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn der Datenbankname, den Sie in der Datei "Web. config" abgelegt haben, zuvor auf dem Computer verwendet wurde, ist möglicherweise bereits eine Datenbank mit einigen Tabellen vorhanden. Wählen Sie einen neuen Namen aus, der noch nicht auf dem Computer verwendet wurde, und ändern Sie die Datei *Web. config* so, dass Sie auf diesen neuen Datenbanknamen zu verweisen. Als Alternative können Sie [SQL Server Express-Hilfsprogramm](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) oder [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) verwenden, um die vorhandene Datenbank zu löschen.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL-Fehler, wenn ein Skript versucht, Benutzer oder Rollen zu erstellen

### <a name="scenario"></a>Szenario

Sie verwenden die Daten Bank Bereitstellung, die auf der Registerkarte " **packen/veröffentlichen** " konfiguriert ist. SQL-Skripts, die während der Bereitstellung ausgeführt werden, umfassen Befehle zum Erstellen von Benutzern oder zum Erstellen von Rollen Möglicherweise werden ausführlichere Meldungen wie die folgenden angezeigt:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Wenn dieser Fehler auftritt, wenn Sie die Daten Bank Bereitstellung im Assistenten **Web veröffentlichen** anstelle der Registerkarte **packen/Veröffentlichen von SQL** konfiguriert haben, erstellen Sie im [Konfigurations-und Bereitstellungs](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) Forum einen Thread, und die Lösung wird dieser Problem Behandlungs Seite hinzugefügt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das Benutzerkonto, das Sie zum Ausführen der Bereitstellung verwenden, verfügt nicht über die Berechtigung zum Erstellen von Benutzern oder Rollen. Beispielsweise kann das Hostingunternehmen die DB-\_DataReader-, DB\_DataWriter-und DB-\_ddladmin-Rollen dem Benutzerkonto zuweisen, das für Sie eingerichtet ist. Diese sind ausreichend, um die meisten Datenbankobjekte zu erstellen, aber nicht zum Erstellen von Benutzern oder Rollen. Eine Möglichkeit, den Fehler zu vermeiden, besteht darin, Benutzer und Rollen aus der Daten Bank Bereitstellung auszuschließen. Hierzu können Sie das presource-Element für das automatisch generierte Skript der Datenbank bearbeiten, sodass es die folgenden Attribute enthält:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Informationen zum Bearbeiten des presource-Elements in der Projektdatei finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in der Projektdatei](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Wenn sich die Benutzer oder Rollen in der Entwicklungs Datenbank in der Zieldatenbank befinden müssen, wenden Sie sich an Ihren Hostinganbieter, um Unterstützung zu erhalten.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server Timeout Fehler beim Ausführen von benutzerdefinierten Skripts während der Bereitstellung

### <a name="scenario"></a>Szenario

Sie haben benutzerdefinierte SQL-Skripts angegeben, die während der Bereitstellung ausgeführt werden sollen, und wenn Web deploy Sie ausführt, erfolgt ein Timeout.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das Ausführen mehrerer Skripts, die über unterschiedliche Transaktions Modi verfügen, kann Timeout Fehler verursachen. Standardmäßig werden automatisch generierte Skripts in einer Transaktion ausgeführt, benutzerdefinierte Skripts jedoch nicht. Wenn Sie die Option **Pull Data und/oder Schema aus einer vorhandenen Datenbank** auf der Registerkarte " **packen/veröffentlichen** " auswählen und ein benutzerdefiniertes SQL-Skript hinzufügen, müssen Sie die Transaktions Einstellungen für einige Skripts ändern, damit alle Skripts die gleichen Transaktions Einstellungen verwenden. Weitere Informationen finden Sie unter Gewusst [wie: Bereitstellen einer Datenbank mit einem Webanwendungs Projekt](https://msdn.microsoft.com/library/dd465343.aspx).

Wenn Sie Transaktions Einstellungen so konfiguriert haben, dass alle identisch sind, aber immer noch diesen Fehler erhalten, besteht eine mögliche Problem Umgehung darin, die Skripts separat auszuführen. Deaktivieren Sie im Raster **Daten Bank Skripts** auf der Registerkarte **Paket/SQL veröffentlichen** das Kontrollkästchen **einschließen** für das Skript, das den Timeout Fehler verursacht, und veröffentlichen Sie das Projekt. Wechseln Sie dann zurück in das Raster **Daten Bank Skripts** , aktivieren Sie das Kontrollkästchen zum **einschließen** des Skripts, und deaktivieren Sie die Kontrollkästchen **einschließen** für die anderen Skripts. Veröffentlichen Sie das Projekt dann erneut. Dieses Mal wird nur das ausgewählte benutzerdefinierte Skript ausgeführt, wenn Sie veröffentlichen.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream-Daten des Site Manifests sind noch nicht verfügbar.

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket mit der Datei "bereitstellen *. cmd* " mit der Option "t (Test)" installieren, wird die folgende Fehlermeldung angezeigt:

Fehler: die Datenstrom Daten von ' sitemanifest/dbfullsql [@path= ' c:\temp\adventureworksgrant.SQL ']/sqlScript ' sind noch nicht verfügbar.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Fehlermeldung bedeutet, dass der Befehl keinen Testbericht erzeugt. Der Befehl wird jedoch möglicherweise ausgeführt, wenn Sie die Option y (tatsächliche Installation) verwenden. Die Meldung gibt nur an, dass ein Problem mit dem Ausführen des Befehls im Testmodus vorliegt.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Diese Anwendung erfordert managedRuntimeVersion v 4.0.

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, bereitzustellen, wird die folgende Fehlermeldung angezeigt:

Für den Anwendungs Pool, den Sie verwenden möchten, ist die Eigenschaft "managedRuntimeVersion" auf "v 2.0" festgelegt. Diese Anwendung erfordert "v 4.0".

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist nicht in IIS installiert. Wenn es sich bei dem Server, auf dem Sie die Bereitstellung durchlaufen, um den Entwicklungs Computer handelt, auf dem Visual Studio 2010 installiert ist, wird ASP.NET 4 auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, auf dem Sie bereitstellen, eine Eingabeaufforderung mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Die Umwandlung von "Microsoft. Web. Deployment. deploymentprovideroptions" ist nicht möglich.

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket bereitstellen, wird die folgende Fehlermeldung angezeigt:

Das Objekt vom Typ ' Microsoft. Web. Deployment. deploymentprovideroptions ' kann nicht in ' Microsoft. Web. Deployment. deploymentprovideroptions ' umgewandelt werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Sie versuchen, die Bereitstellung über den IIS-Manager mithilfe der Web deploy 1,1-Benutzeroberfläche auf einem Server mit installiertem Web deploy 2,0 durchzuführen. Wenn Sie das IIS-Remote Verwaltungs Tool für die Bereitstellung durch Importieren eines Pakets verwenden, aktivieren Sie das Dialogfeld **neue Features verfügbar** , wenn Sie die Verbindung herstellen. (Dieses Dialogfeld wird möglicherweise nur einmal angezeigt, wenn die Verbindung erstmalig hergestellt wird. Um die Verbindung zu löschen und neu zu starten, schließen Sie den IIS-Manager, und starten Sie ihn erneut, indem Sie inetmgr/Reset an der Eingabeaufforderung eingeben.) Wenn eine der aufgelisteten Features **Web deploy Benutzeroberfläche**ist und eine Versionsnummer kleiner als 8 ist, kann auf dem Server, auf dem Sie die Bereitstellung durchlaufen, sowohl 1,1-als auch 2,0-Versionen von Web deploy installiert sein. Zum Bereitstellen von auf einem Client, auf dem 2,0 installiert ist, muss auf dem Server nur Web deploy 2,0 installiert sein. Sie müssen sich an Ihren Hostinganbieter wenden, um dieses Problem zu beheben.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Die systemeigenen Komponenten von SQL Server Compact können nicht geladen werden.

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, wird die folgende Fehlermeldung angezeigt:

Die nativen Komponenten von SQL Server Compact, die dem ADO.NET-Anbieter von Version 8482 entsprechen, können nicht geladen werden. Installieren Sie die korrekte Version von SQL Server Compact. Weitere Informationen finden Sie im KB-Artikel 974247.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die bereitgestellte Site verfügt nicht über *amd64* -und *x86* -Unterordner mit den systemeigenen Assemblys im *bin* -Ordner der Anwendung. Auf einem Computer, auf dem SQL Server Compact installiert ist, befinden sich die systemeigenen Assemblys im Verzeichnis *c:\Programme\Microsoft SQL Server Compact edition\v4.0\private*. Die beste Möglichkeit, die richtigen Dateien in die richtigen Ordner in einem Visual Studio-Projekt zu übernehmen, besteht darin, das nuget-Paket sqlservercompact zu installieren. Bei der Paketinstallation wird ein Postbuildskript zum Kopieren der systemeigenen Assemblys in *amd64* und *x86*hinzugefügt. Damit diese bereitgestellt werden können, müssen Sie Sie jedoch manuell in das Projekt einschließen. Weitere Informationen finden Sie im Tutorial Bereitstellen von [SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Fehler "der Pfad ist ungültig" nach der Bereitstellung einer Entity Framework Code First-Anwendung

### <a name="scenario"></a>Szenario

Sie stellen eine Anwendung bereit, die Entity Framework Code First-Migrationen und ein DBMS verwendet, z. b. SQL Server Compact, in dem die Datenbank in einer Datei im Ordner App\_Data gespeichert wird. Sie haben Code First-Migrationen konfiguriert, um die Datenbank nach der ersten Bereitstellung zu erstellen. Wenn Sie die Anwendung ausführen, erhalten Sie eine Fehlermeldung wie im folgenden Beispiel:

Der Pfad ist ungültig. Überprüfen Sie das Verzeichnis für die Datenbank. [Path = c:\inetpub\wwwroot\app\_data\databasename.sdf]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Code First versucht, die Datenbank zu erstellen, aber der APP-\_Datenordner ist nicht vorhanden. Entweder haben Sie bei der Bereitstellung keine Dateien in der *App\_Daten* Ordner vorhanden, oder Sie haben auf der Registerkarte " **Web packen/veröffentlichen** " des Projekts Eigenschaftenfenster die Option " **App\_Daten ausschließen** " ausgewählt. Der Bereitstellungs Prozess erstellt keinen Ordner auf dem Server, wenn der Ordner keine Dateien enthält, die auf den Server kopiert werden sollen. Wenn Sie die Datenbank bereits am Standort eingerichtet haben, löscht der Bereitstellungs Prozess die Dateien und die *App\_Daten* Ordner selbst, wenn Sie im Veröffentlichungs Profil **zusätzliche Dateien am Ziel entfernen** ausgewählt haben. Um das Problem zu beheben, fügen Sie eine Platzhalter Datei (z. b. eine txt-Datei) in den *App-\_Daten* Ordner ein, und stellen Sie sicher, dass Sie keine **App\_Daten ausschließen** ausgewählt haben und erneut bereitstellen.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Das COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde, kann nicht verwendet werden."

### <a name="scenario"></a>Szenario

Zum Bereitstellen der Anwendung haben Sie erfolgreich die One-Click-Veröffentlichung verwendet, und anschließend wird dieser Fehler angezeigt:

Fehler beim Webbereitstellungs Task. (Die Anforderung an die Remote-Agent-URL "<https://serverurl.com/msdeploy.axd?site=sitename>" konnte nicht ausgeführt werden.)  
 Die Anforderung an die Remote-Agent-URL "<https://url/msdeploy.axd?site=sitename>" konnte nicht ausgeführt werden.  
Die Anforderung wurde abgebrochen: die Anforderung wurde abgebrochen.  
Ein COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde, kann nicht verwendet werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn Sie Visual Studio schließen und neu starten, ist in der Regel alles erforderlich, um diesen Fehler zu beheben.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Die Bereitstellung schlägt fehl, weil die für die Veröffentlichung verwendeten Benutzer Anmelde Informationen keine "

### <a name="scenario"></a>Szenario

Die Veröffentlichung schlägt mit einem Fehler fehl, der angibt, dass Sie nicht über die Berechtigung verfügen, Ordner Berechtigungen festzulegen (das von Ihnen verwendete Benutzerkonto verfügt nicht über die setacl-Autorität)

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig legt Visual Studio Leseberechtigungen für den Stamm Ordner der Website und Schreibberechtigungen für die APP-\_Datenordner fest. Wenn Sie wissen, dass die Standard Berechtigungen für Website Ordner richtig sind und nicht festgelegt werden müssen, deaktivieren Sie dieses Verhalten, indem Sie **&lt;includesetaclprovideron Destination&gt;false&lt;/IncludeSetACLProviderOnDestination&gt;** der Veröffentlichungs Profil Datei (um ein einzelnes Profil zu beeinflussen) oder der Datei "WPP. targets" hinzufügen (um alle Profile zu beeinflussen). Weitere Informationen zum Bearbeiten dieser Dateien finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in Profil Dateien (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Zugriffs Verweigerungs Fehler, wenn die Anwendung versucht, in einen Anwendungsordner zu schreiben

### <a name="scenario"></a>Szenario

Die Anwendung ist fehlerhaft, wenn versucht wird, eine Datei in einem der Anwendungsordner zu erstellen oder zu bearbeiten, da Sie nicht über eine Schreib Berechtigung für diesen Ordner verfügt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig legt Visual Studio Leseberechtigungen für den Stamm Ordner der Website und Schreibberechtigungen für die APP-\_Datenordner fest. Wenn Ihre Anwendung Schreibzugriff auf einen Unterordner benötigt, können Sie Berechtigungen für diesen Ordner festlegen, wie in den Tutorials Festlegen von Ordner Berechtigungen und Bereitstellen für die Produktionsumgebung dieser Reihe beschrieben. Wenn Ihre Anwendung Schreibzugriff auf den Stamm Ordner der Website benötigt, müssen Sie verhindern, dass der schreibgeschützte Zugriff auf den Stamm Ordner festgelegt wird, indem Sie **&lt;includesetaclprovideron Destination&gt;false&lt;/IncludeSetACLProviderOnDestination&gt;** der Veröffentlichungs Profil Datei (für ein einzelnes Profil) oder der Datei "WPP. targets" hinzufügen (um alle Profile zu beeinflussen). Weitere Informationen zum Bearbeiten dieser Dateien finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in Profil Dateien (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Konfigurationsfehler-das TargetFramework-Attribut verweist auf eine neuere Version als die installierte Version des .NET Framework

### <a name="scenario"></a>Szenario

Sie haben erfolgreich ein Webprojekt veröffentlicht, das auf ASP.NET 4,5 abzielt. Wenn Sie die Anwendung ausführen, wird der folgende Fehler angezeigt, wenn der customErrors-Modus in der Datei "Web. config" auf "Off" festgelegt ist:

Das Attribut "TargetFramework" im &lt;-Kompilierungs&gt; Element der Datei "Web. config" wird nur für die Zielversion 4,0 und höher des .NET Framework verwendet (z. b. "&lt;Kompilierung TargetFramework =" 4.0 "&gt;"). Das TargetFramework-Attribut verweist zurzeit auf eine Version, die höher als die installierte Version des .NET Framework ist. Geben Sie eine gültige Zielversion des .NET Framework an, oder installieren Sie die erforderliche Version des .NET Framework.

Im Feld Quell Fehler der Fehlerseite wird die folgende Zeile aus der Datei "Web. config" als Fehlerursache hervorgehoben:

&lt;Kompilierung TargetFramework = "4.5"/&gt;

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Server unterstützt ASP.NET 4,5 nicht. Wenden Sie sich an den Hostinganbieter, um zu bestimmen, wann und ob die Unterstützung für 4,5 ASP.net Wenn das Upgrade des Servers nicht möglich ist, müssen Sie stattdessen ein Webprojekt bereitstellen, das ASP.NET 4 oder früher als Ziel verwendet.

Wenn Sie ein ASP.NET 4 oder ein älteres Webprojekt für dasselbe Ziel bereitstellen, aktivieren Sie das Kontrollkästchen **zusätzliche Dateien am Ziel entfernen** auf der Registerkarte **Einstellungen** des Assistenten **Web veröffentlichen** . Wenn Sie die Option **zusätzliche Dateien am Ziel entfernen**nicht auswählen, wird weiterhin die Seite Konfigurationsfehler angezeigt.

Das Fenster Projekt **Eigenschaften** enthält eine Dropdown Liste für das Ziel Framework, aber Sie können dieses Problem nicht lösen, indem Sie es einfach von **.NET Framework 4,5** in **.NET Framework 4**ändern. Wenn Sie das Ziel Framework in eine frühere Frameworkversion ändern, enthält das Projekt weiterhin Verweise auf die Assemblys der neueren Frameworkversion und kann nicht ausgeführt werden. Sie müssen diese Verweise manuell ändern oder ein neues Projekt erstellen, das auf .NET Framework 4 oder eine frühere Version abzielt. Weitere Informationen finden Sie unter [.NET Framework Zielsetzungen für Websites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Fehler bei mittlerer Vertrauensstellung

### <a name="scenario"></a>Szenario

Wenn Sie die Anwendung in der Produktionsumgebung ausführen, wird eine Fehlermeldung im Zusammenhang mit mittlerer Vertrauensstellung ausgegeben.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Viele Hostinganbieter von Drittanbietern führen Ihre Website mit mittlerer Vertrauenswürdigkeit aus. Dies bedeutet, dass es einige Dinge gibt, die es nicht zulässt. Beispielsweise kann der Anwendungscode nicht auf die Windows-Registrierung zugreifen und keine Dateien lesen und schreiben, die sich außerhalb der Ordnerhierarchie Ihrer Anwendung befinden. Standardmäßig wird Ihre Anwendung mit *voller Vertrauens* Würdigkeit auf dem lokalen Computer ausgeführt. Dies bedeutet, dass die Anwendung möglicherweise Dinge ausführen kann, die bei der Bereitstellung in der Produktion fehlschlagen würden.

Sie können die Anwendung so konfigurieren, dass Sie in der lokalen IIS-Umgebung ausgeführt wird, um die Problembehebung zu beheben. Öffnen Sie hierzu die Datei " *Web. config* " der Anwendung, und fügen Sie im Element " **System. Web** " ein **Trust** -Element hinzu, wie im folgenden Beispiel gezeigt.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Die Anwendung wird jetzt in IIS mit mittlerer Vertrauenswürdigkeit auch auf dem lokalen Computer ausgeführt.

Wenn Sie in Azure App Service bereitstellen, sollten Sie dies nicht tun, da Azure keine mittlere Vertrauenswürdigkeit erfordert. Zum Zeitpunkt, an dem dieses Tutorial im Februar 2012 geschrieben wird, führt die Verwendung dieser Methode zum Ausführen der Anwendung mit mittlerer Vertrauensstellung zu einem Fehler in Azure.

Wenn Sie Entity Framework Code First-Migrationen verwenden und die Bereitstellung für einen Hostinganbieter durchführen, der Ihre Anwendung mit mittlerer Vertrauenswürdigkeit ausführt, stellen Sie sicher, dass Sie Version 5,0 oder höher installiert haben. In Entity Framework Version 4,3 ist für Migrationen volle Vertrauenswürdigkeit erforderlich, um das Datenbankschema zu aktualisieren.

## <a name="http-40417-not-found-error"></a>Fehler "http 404,17 nicht gefunden"

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort auf dem Entwicklungs Computer in IIS ausführen, wird die folgende Fehlermeldung angezeigt, die meldet, dass der Server "default. aspx" nicht verarbeiten kann:

HTTP-Fehler 404,17-nicht gefunden

Der angeforderte Inhalt ist anscheinend ein Skript und wird vom statischen Datei Handler nicht bedient.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4,5 ist möglicherweise nicht auf Ihrem Computer installiert. Lesen Sie die Schritte im Tutorial bereitstellen in IIS als Test Umgebung in dieser Reihe, in der die Installation von ASP.NET 4,5 erläutert wird.

> [!div class="step-by-step"]
> [Vorheriges](deploying-extra-files.md)
