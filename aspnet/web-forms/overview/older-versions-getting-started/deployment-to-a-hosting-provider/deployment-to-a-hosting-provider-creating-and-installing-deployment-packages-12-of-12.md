---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Problembehandlung (12 von 12) | Microsoft-Dokumentation'
author: tdykstra
description: In dieser Reihe von Tutorials wird gezeigt, wie Sie ein ASP.NET-Webanwendungs Projekt bereitstellen (veröffentlichen), das eine SQL Server Compact-Datenbank mit Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: db8f58e3679e6dea865dadb6f64916032dd9f38c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74639875"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mithilfe von Visual Studio oder Visual Web Developer: Problembehandlung (12 von 12)

von [Tom Dykstra](https://github.com/tdykstra)

[Starter Projekt herunterladen](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser Reihe von Tutorials erfahren Sie, wie Sie ein ASP.NET-Webanwendungs Projekt, das eine SQL Server Compact Datenbank enthält, mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web bereitstellen (veröffentlichen). Sie können auch Visual Studio 2010 verwenden, wenn Sie das Webveröffentlichungs Update installieren. Eine Einführung in die Reihe finden Sie [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das nach der RC-Version von Visual Studio 2012 eingeführte Bereitstellungs Funktionen zeigt, zeigt, wie SQL Server Editionen außer SQL Server Compact bereitgestellt werden, und zeigt, wie Sie die Bereitstellung auf Windows Azure-Websites durchführt. Weitere Informationen finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

Auf dieser Seite werden einige häufige Probleme beschrieben, die auftreten können, wenn Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio bereitstellen. Mindestens eine mögliche Ursache und entsprechende Lösungen werden bereitgestellt.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Server Fehler in "/"-Anwendung-aktuelle benutzerdefinierte Fehler Einstellungen verhindern, dass Details des Fehlers Remote angezeigt werden.

### <a name="scenario"></a>Szenario

Nachdem Sie einen Standort auf einem Remote Host bereitgestellt haben, erhalten Sie eine Fehlermeldung, in der die customErrors-Einstellung in der Datei "Web. config" erwähnt wird, jedoch nicht die tatsächliche Ursache des Fehlers:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig zeigt ASP.net nur dann ausführliche Fehlerinformationen an, wenn Ihre Webanwendung auf dem lokalen Computer ausgeführt wird. Im Allgemeinen ist es nicht ratsam, ausführliche Fehlerinformationen anzuzeigen, wenn Ihre Webanwendung über das Internet öffentlich verfügbar ist, da Hacker diese Informationen möglicherweise verwenden können, um Schwachstellen in der Anwendung zu finden. Wenn Sie jedoch einen Standort oder Updates für einen Standort bereitstellen, tritt manchmal ein Fehler auf, und Sie müssen die tatsächliche Fehlermeldung erhalten.

Damit die Anwendung detaillierte Fehlermeldungen anzeigen kann, wenn Sie auf dem Remote Host ausgeführt wird, bearbeiten Sie die Datei "Web. config", um den `customErrors` Modus zu deaktivieren, stellen Sie die Anwendung erneut bereit, und führen Sie die Anwendung erneut aus:

1. Wenn die Datei "Web. config" der Anwendung über ein `customErrors`-Element im `system.web`-Element verfügt, ändern Sie das `mode`-Attribut in "Off". Fügen Sie andernfalls ein `customErrors` Element im `system.web`-Element hinzu, wobei das `mode`-Attribut auf "Off" festgelegt ist, wie im folgenden Beispiel gezeigt:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Stellen Sie die Anwendung bereit.
3. Führen Sie die Anwendung aus, und wiederholen Sie den Vorgang, der zum Auftreten des Fehlers geführt hat. Nun können Sie sehen, welche Fehlermeldung tatsächlich vorliegt.
4. Wenn Sie den Fehler behoben haben, stellen Sie die ursprüngliche `customErrors` Einstellung wieder her, und stellen Sie die Anwendung erneut bereit.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Der Zugriff auf einer Webseite, die SQL Server Compact verwendet, wird verweigert.

### <a name="scenario"></a>Szenario

Wenn Sie einen Standort bereitstellen, der SQL Server Compact verwendet und Sie eine Seite an dem bereitgestellten Standort ausführen, der auf die Datenbank zugreift, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das Netzwerkdienst Konto auf dem Server muss in der Lage sein, systemeigene SQL Service Compact-Binärdateien zu lesen, die sich im Ordner " *bin\amd64* " oder " *bin\x86* " befinden, jedoch nicht über Leseberechtigungen für diese Ordner verfügen. Legen Sie für den Ordner *bin* die Berechtigung Lesen für Netzwerkdienst fest, und stellen Sie sicher, dass Sie die Berechtigungen auf Unterordner erweitern.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Die Konfigurationsdatei kann aufgrund unzureichender Berechtigungen nicht gelesen werden.

### <a name="scenario"></a>Szenario

Wenn Sie auf die Schaltfläche "veröffentlichen" von Visual Studio klicken, um eine Anwendung auf dem lokalen Computer in IIS bereitzustellen, schlägt die Veröffentlichung fehl, und im **Ausgabe** Fenster wird eine Fehlermeldung ähnlich der folgenden angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn Sie die One-Click-Veröffentlichung in IIS auf dem lokalen Computer verwenden möchten, müssen Sie Visual Studio mit Administrator Berechtigungen ausführen. Schließen Sie Visual Studio, und starten Sie es mit Administrator Berechtigungen neu.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Es konnte keine Verbindung mit dem Ziel Computer hergestellt werden... Verwenden des angegebenen Prozesses

### <a name="scenario"></a>Szenario

Wenn Sie auf die Visual Studio-Schaltfläche veröffentlichen klicken, um eine Anwendung bereitzustellen, schlägt die Veröffentlichung fehl, und im Fenster **Ausgabe** wird eine Fehlermeldung ähnlich der folgenden angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Kommunikation mit dem Zielserver wird von einem Proxy Server unterbrochen. Wählen Sie in der Windows-Systemsteuerung oder in Internet Explorer die **Option Internet Optionen** aus, und klicken Sie auf die Registerkarte **Verbindungen** . Klicken Sie im Dialogfeld **Internet Eigenschaften** auf **LAN-Einstellungen**. Deaktivieren Sie im Dialogfeld **Einstellungen für lokales Netzwerk (LAN)** das Kontrollkästchen **Einstellungen automatisch erkennen** . Klicken Sie dann erneut auf die Schaltfläche veröffentlichen.

Wenn das Problem weiterhin besteht, wenden Sie sich an Ihren Systemadministrator, um zu bestimmen, was mit Proxy-oder Firewalleinstellungen möglich ist. Das Problem tritt auf, weil Web deploy einen nicht standardmäßigen Port für die Bereitstellung des Webverwaltungsdiensts verwendet (8172); bei anderen Verbindungen wird von Web deploy Port 80 verwendet. Wenn Sie für einen Drittanbieter-Hosting-Anbieter bereitstellen, verwenden Sie in der Regel den Webverwaltungsdienst.

## <a name="default-net-40-application-pool-does-not-exist"></a>Der .NET 4,0-Standard Anwendungs Pool ist nicht vorhanden.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung bereitstellen, für die die .NET Framework 4 erforderlich ist, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist nicht in IIS installiert. Wenn es sich bei dem Server, auf dem Sie die Bereitstellung durchlaufen, um den Entwicklungs Computer handelt, auf dem Visual Studio 2010 installiert ist, wird ASP.NET 4 auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, auf dem Sie bereitstellen, eine Eingabeaufforderung mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Möglicherweise müssen Sie auch die .NET Framework Version des Standard Anwendungs Pools manuell festlegen. Weitere Informationen finden Sie im Tutorial bereitstellen [in IIS als Test Umgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Das Format der Initialisierungs Zeichenfolge entspricht nicht der Spezifikation, beginnend bei Index 0.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung mit One-Click-Veröffentlichung bereitgestellt haben, erhalten Sie die folgende Fehlermeldung, wenn Sie eine Seite ausführen, die auf die Datenbank zugreift:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Öffnen Sie die Datei " *Web. config* " auf der bereitgestellten Website, und überprüfen Sie, ob die Verbindungs Zeichenfolgen-Werte mit `$(ReplaceableToken_`beginnen, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Wenn die Verbindungs Zeichenfolgen wie in diesem Beispiel aussehen, bearbeiten Sie die Projektdatei, und fügen Sie die folgende Eigenschaft zum `PropertyGroup`-Element hinzu, das für alle Buildkonfigurationen gilt:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Stellen Sie die Anwendung dann erneut bereit.

## <a name="http-500-internal-server-error"></a>HTTP 500-Interner Server Fehler

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, wird die folgende Fehlermeldung ohne spezifische Informationen angezeigt, die auf die Ursache des Fehlers hinweisen:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Es gibt viele Ursachen für 500-Fehler. eine mögliche Ursache, wenn Sie diese Tutorials befolgen, besteht darin, dass Sie ein XML-Element in einer der XML-Transformations Dateien an der falschen Stelle platzieren. Beispielsweise wird dieser Fehler angezeigt, wenn Sie die Transformation einfügen, die ein `<location>` Element unter `<system.web>` anstatt direkt unter `<configuration>`einfügt. Die Lösung besteht in diesem Fall darin, die XML-Transformations Datei zu korrigieren und erneut bereitzustellen.

## <a name="http-50021-internal-server-error"></a>HTTP 500,21-interner Server Fehler

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der von Ihnen bereitgestellte Standort hat das Ziel ASP.NET 4, aber ASP.NET 4 ist nicht in IIS auf dem Server registriert. Öffnen Sie auf dem-Server eine Eingabeaufforderung mit erhöhten Rechten, und registrieren Sie ASP.NET 4 durch Ausführen der folgenden Befehle:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Möglicherweise müssen Sie auch die .NET Framework Version des Standard Anwendungs Pools manuell festlegen. Weitere Informationen finden Sie im Tutorial bereitstellen [in IIS als Test Umgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Fehler beim Öffnen SQL Server Express Datenbank in App-\_Daten.

### <a name="scenario"></a>Szenario

Sie haben die Verbindungs Zeichenfolge der Datei " *Web. config* " so aktualisiert, dass Sie auf eine SQL Server Express Datenbank als *MDF* -Datei in Ihrer *App\_Daten* Ordner verweist. Wenn Sie die Anwendung zum ersten Mal ausführen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Name der *MDF* -Datei kann nicht mit dem Namen einer SQL Server Express Datenbank identisch sein, die jemals auf dem Computer vorhanden war, auch wenn Sie die *MDF* -Datei der bereits vorhandenen Datenbank gelöscht haben. Ändern Sie den Namen der *MDF* -Datei in einen Namen, der noch nie als Datenbankname verwendet wurde, und ändern Sie die Datei *Web. config* so, dass der neue Name verwendet wird. Als Alternative können Sie mit [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) bereits vorhandene SQL Server Express Datenbanken löschen.

## <a name="model-compatibility-cannot-be-checked"></a>Die Modell Kompatibilität kann nicht geprüft werden.

### <a name="scenario"></a>Szenario

Sie haben die Verbindungs Zeichenfolge der Datei " *Web. config* " so aktualisiert, dass Sie auf eine neue SQL Server Express Datenbank verweist. Wenn Sie die Anwendung zum ersten Mal ausführen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn der Datenbankname, den Sie in der Datei "Web. config" abgelegt haben, zuvor auf dem Computer verwendet wurde, ist möglicherweise bereits eine Datenbank mit einigen Tabellen vorhanden. Wählen Sie einen neuen Namen aus, der noch nicht auf dem Computer verwendet wurde, und ändern Sie die Datei *Web. config* so, dass Sie auf diesen neuen Datenbanknamen zu verweisen. Als Alternative können Sie [SQL Server Express-Hilfsprogramm](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) oder [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) verwenden, um die vorhandene Datenbank zu löschen.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL-Fehler, wenn ein Skript versucht, Benutzer oder Rollen zu erstellen

### <a name="scenario"></a>Szenario

Sie verwenden die Daten Bank Bereitstellung, die auf der Registerkarte " **packen/veröffentlichen** " konfiguriert ist. SQL-Skripts, die während der Bereitstellung ausgeführt werden, umfassen Befehle zum Erstellen von Benutzern oder zum Erstellen von Rollen Möglicherweise werden ausführlichere Meldungen wie die folgenden angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Wenn dieser Fehler auftritt, wenn Sie die Daten Bank Bereitstellung im Assistenten **Web veröffentlichen** anstelle der Registerkarte **packen/Veröffentlichen von SQL** konfiguriert haben, erstellen Sie im [Konfigurations-und Bereitstellungs](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) Forum einen Thread, und die Lösung wird dieser Problem Behandlungs Seite hinzugefügt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das Benutzerkonto, das Sie zum Ausführen der Bereitstellung verwenden, verfügt nicht über die Berechtigung zum Erstellen von Benutzern oder Rollen. Beispielsweise kann das Hostingunternehmen dem Benutzerkonto, das Sie für Sie einrichten, die `db_datareader`-, `db_datawriter`-und `db_ddladmin` Rollen zuweisen. Diese sind ausreichend, um die meisten Datenbankobjekte zu erstellen, aber nicht zum Erstellen von Benutzern oder Rollen. Eine Möglichkeit, den Fehler zu vermeiden, besteht darin, Benutzer und Rollen aus der Daten Bank Bereitstellung auszuschließen. Hierzu können Sie das `PreSource`-Element für das automatisch generierte Skript der Datenbank bearbeiten, sodass es die folgenden Attribute enthält:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Weitere Informationen zum Bearbeiten des `PreSource`-Elements in der Projektdatei finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in der Projektdatei](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Wenn sich die Benutzer oder Rollen in der Entwicklungs Datenbank in der Zieldatenbank befinden müssen, wenden Sie sich an Ihren Hostinganbieter, um Unterstützung zu erhalten.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server Timeout Fehler beim Ausführen von benutzerdefinierten Skripts während der Bereitstellung

### <a name="scenario"></a>Szenario

Sie haben benutzerdefinierte SQL-Skripts angegeben, die während der Bereitstellung ausgeführt werden sollen, und wenn Web deploy Sie ausführt, erfolgt ein Timeout.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das Ausführen mehrerer Skripts, die über unterschiedliche Transaktions Modi verfügen, kann Timeout Fehler verursachen. Standardmäßig werden automatisch generierte Skripts in einer Transaktion ausgeführt, benutzerdefinierte Skripts jedoch nicht. Wenn Sie die Option **Pull Data und/oder Schema aus einer vorhandenen Datenbank** auf der Registerkarte " **packen/veröffentlichen** " auswählen und ein benutzerdefiniertes SQL-Skript hinzufügen, müssen Sie die Transaktions Einstellungen für einige Skripts ändern, damit alle Skripts die gleichen Transaktions Einstellungen verwenden. Weitere Informationen finden Sie unter Gewusst [wie: Bereitstellen einer Datenbank mit einem Webanwendungs Projekt](https://msdn.microsoft.com/library/dd465343.aspx).

Wenn Sie Transaktions Einstellungen so konfiguriert haben, dass alle identisch sind, aber immer noch diesen Fehler erhalten, besteht eine mögliche Problem Umgehung darin, die Skripts separat auszuführen. Deaktivieren Sie im Raster **Daten Bank Skripts** auf der Registerkarte **Paket/SQL veröffentlichen** das Kontrollkästchen **einschließen** für das Skript, das den Timeout Fehler verursacht, und veröffentlichen Sie das Projekt. Wechseln Sie dann zurück in das Raster **Daten Bank Skripts** , aktivieren Sie das Kontrollkästchen zum **einschließen** des Skripts, und deaktivieren Sie die Kontrollkästchen **einschließen** für die anderen Skripts. Veröffentlichen Sie das Projekt dann erneut. Dieses Mal wird nur das ausgewählte benutzerdefinierte Skript ausgeführt, wenn Sie veröffentlichen.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream-Daten des Site Manifests sind noch nicht verfügbar.

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket mit der Datei "bereitstellen *. cmd* " mit der Option `t` (Test) installieren, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Fehlermeldung bedeutet, dass der Befehl keinen Testbericht erzeugt. Der Befehl wird jedoch möglicherweise ausgeführt, wenn Sie die Option `y` (tatsächliche Installation) verwenden. Die Meldung gibt nur an, dass ein Problem mit dem Ausführen des Befehls im Testmodus vorliegt.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Diese Anwendung erfordert managedRuntimeVersion v 4.0.

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, bereitzustellen, wird die folgende Fehlermeldung angezeigt:

 Fehler: die Datenstrom Daten von ' sitemanifest/dbfullsql [@path= ' c:\temp\adventureworksgrant.SQL ']/sqlScript ' sind noch nicht verfügbar. Für den Anwendungs Pool, den Sie verwenden möchten, ist die Eigenschaft "managedRuntimeVersion" auf "v 2.0" festgelegt. Diese Anwendung erfordert "v 4.0". 

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist nicht in IIS installiert. Wenn es sich bei dem Server, auf dem Sie die Bereitstellung durchlaufen, um den Entwicklungs Computer handelt, auf dem Visual Studio 2010 installiert ist, wird ASP.NET 4 auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, auf dem Sie bereitstellen, eine Eingabeaufforderung mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Die Umwandlung von "Microsoft. Web. Deployment. deploymentprovideroptions" ist nicht möglich.

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket bereitstellen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Sie versuchen, die Bereitstellung über den IIS-Manager mithilfe der Web deploy 1,1-Benutzeroberfläche auf einem Server mit installiertem Web deploy 2,0 durchzuführen. Wenn Sie das IIS-Remote Verwaltungs Tool für die Bereitstellung durch Importieren eines Pakets verwenden, aktivieren Sie das Dialogfeld **neue Features verfügbar** , wenn Sie die Verbindung herstellen. (Dieses Dialogfeld wird möglicherweise nur einmal angezeigt, wenn die Verbindung erstmalig hergestellt wird. Um die Verbindung zu löschen und neu zu starten, schließen Sie den IIS-Manager, und starten Sie ihn erneut, indem Sie `inetmgr /reset` an der Eingabeaufforderung eingeben.) Wenn eine der aufgelisteten Features **Web deploy Benutzeroberfläche**ist und eine Versionsnummer kleiner als 8 ist, kann auf dem Server, auf dem Sie die Bereitstellung durchlaufen, sowohl 1,1-als auch 2,0-Versionen von Web deploy installiert sein. Zum Bereitstellen von auf einem Client, auf dem 2,0 installiert ist, muss auf dem Server nur Web deploy 2,0 installiert sein. Sie müssen sich an Ihren Hostinganbieter wenden, um dieses Problem zu beheben.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Die systemeigenen Komponenten von SQL Server Compact können nicht geladen werden.

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die bereitgestellte Site verfügt nicht über *amd64* -und *x86* -Unterordner mit den systemeigenen Assemblys im *bin* -Ordner der Anwendung. Auf einem Computer, auf dem SQL Server Compact installiert ist, befinden sich die systemeigenen Assemblys im Verzeichnis *c:\Programme\Microsoft SQL Server Compact edition\v4.0\private*. Die beste Möglichkeit, die richtigen Dateien in die richtigen Ordner in einem Visual Studio-Projekt zu übernehmen, besteht darin, das nuget-Paket sqlservercompact zu installieren. Bei der Paketinstallation wird ein Postbuildskript zum Kopieren der systemeigenen Assemblys in *amd64* und *x86*hinzugefügt. Damit diese bereitgestellt werden können, müssen Sie Sie jedoch manuell in das Projekt einschließen. Weitere Informationen finden Sie im Tutorial Bereitstellen von [SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Fehler "der Pfad ist ungültig" nach der Bereitstellung einer Entity Framework Code First-Anwendung

### <a name="scenario"></a>Szenario

Sie stellen eine Anwendung bereit, die Entity Framework Code First-Migrationen und ein DBMS verwendet, z. b. SQL Server Compact, in dem die Datenbank in einer Datei im Ordner App\_Data gespeichert wird. Sie haben Code First-Migrationen konfiguriert, um die Datenbank nach der ersten Bereitstellung zu erstellen. Wenn Sie die Anwendung ausführen, erhalten Sie eine Fehlermeldung wie im folgenden Beispiel:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Code First versucht, die Datenbank zu erstellen, aber der APP-\_Datenordner ist nicht vorhanden. Entweder haben Sie bei der Bereitstellung keine Dateien in der *App\_Daten* Ordner vorhanden, oder Sie haben im Fenster " **Projekteigenschaften** " auf der Registerkarte " **Web packen/veröffentlichen** " die Option **App\_Daten ausschließen** ausgewählt. Der Bereitstellungs Prozess erstellt keinen Ordner auf dem Server, wenn der Ordner keine Dateien enthält, die auf den Server kopiert werden sollen. Wenn Sie die Datenbank bereits am Standort eingerichtet haben, löscht der Bereitstellungs Prozess die Dateien und die *App\_Daten* Ordner selbst, wenn Sie im Veröffentlichungs Profil **zusätzliche Dateien am Ziel entfernen** ausgewählt haben. Um das Problem zu beheben, fügen Sie eine Platzhalter Datei (z. b. eine txt-Datei) in den *App-\_Daten* Ordner ein, und stellen Sie sicher, dass Sie keine **App\_Daten ausschließen** ausgewählt haben und erneut bereitstellen. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Das COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde, kann nicht verwendet werden."

### <a name="scenario"></a>Szenario

Zum Bereitstellen der Anwendung haben Sie erfolgreich die One-Click-Veröffentlichung verwendet, und anschließend wird dieser Fehler angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

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

Standardmäßig legt Visual Studio Leseberechtigungen für den Stamm Ordner der Website und Schreibberechtigungen für die APP-\_Datenordner fest. Wenn Ihre Anwendung Schreibzugriff auf einen Unterordner benötigt, können Sie Berechtigungen für diesen Ordner festlegen, wie in den Tutorials [Festlegen von Ordner Berechtigungen](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) und bereitstellen [in der Produktionsumgebung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) beschrieben. Wenn Ihre Anwendung Schreibzugriff auf den Stamm Ordner der Website benötigt, müssen Sie verhindern, dass der schreibgeschützte Zugriff auf den Stamm Ordner festgelegt wird, indem Sie **&lt;includesetaclprovideron Destination&gt;false&lt;/IncludeSetACLProviderOnDestination&gt;** der Veröffentlichungs Profil Datei (für ein einzelnes Profil) oder der Datei "WPP. targets" hinzufügen (um alle Profile zu beeinflussen). Weitere Informationen zum Bearbeiten dieser Dateien finden Sie unter Gewusst [wie: Bearbeiten von Bereitstellungs Einstellungen in Profil Dateien (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Konfigurationsfehler-das TargetFramework-Attribut verweist auf eine neuere Version als die installierte Version des .NET Framework

### <a name="scenario"></a>Szenario

Sie haben erfolgreich ein Webprojekt veröffentlicht, das auf ASP.NET 4,5 abzielt. Wenn Sie die Anwendung ausführen (wobei der `customErrors` Modus in der Datei "Web. config" auf "Off" festgelegt ist), erhalten Sie die folgende Fehlermeldung:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Im Feld Quell Fehler der Fehlerseite wird die folgende Zeile aus der Datei "Web. config" als Fehlerursache hervorgehoben:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Server unterstützt ASP.NET 4,5 nicht. Wenden Sie sich an den Hostinganbieter, um zu bestimmen, wann und ob die Unterstützung für 4,5 ASP.net Wenn das Upgrade des Servers nicht möglich ist, müssen Sie stattdessen ein Webprojekt bereitstellen, das ASP.NET 4 oder früher als Ziel verwendet. Wenn Sie ein ASP.NET 4 oder ein älteres Webprojekt für dasselbe Ziel bereitstellen, aktivieren Sie das Kontrollkästchen **zusätzliche Dateien am Ziel entfernen** auf der Registerkarte **Einstellungen** des Assistenten **Web veröffentlichen** . Wenn Sie die Option **zusätzliche Dateien am Ziel entfernen**nicht auswählen, wird weiterhin die Seite Konfigurationsfehler angezeigt.

Das Fenster Projekt **Eigenschaften** enthält eine Dropdown Liste für das Ziel Framework, aber Sie können dieses Problem nicht lösen, indem Sie es einfach von **.NET Framework 4,5** in **.NET Framework 4**ändern. Wenn Sie das Ziel Framework in eine frühere Frameworkversion ändern, enthält das Projekt weiterhin Verweise auf die Assemblys der neueren Frameworkversion und kann nicht ausgeführt werden. Sie müssen diese Verweise manuell ändern oder ein neues Projekt erstellen, das auf .NET Framework 4 oder eine frühere Version abzielt. Weitere Informationen finden Sie unter [.NET Framework Zielsetzungen für Websites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Vorheriges](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
