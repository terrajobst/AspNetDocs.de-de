---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: signalr Self-Host | Microsoft-Dokumentation'
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie Sie einen selbst gehosteten signalr 2-Server erstellen und mit einem JavaScript-Client eine Verbindung damit herstellen. Im Tutorial V verwendete Software Versionen...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578566"
---
# <a name="tutorial-signalr-self-host"></a>Tutorial: signalr-Self-Host

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> In diesem Tutorial wird gezeigt, wie Sie einen selbst gehosteten signalr 2-Server erstellen und mit einem JavaScript-Client eine Verbindung damit herstellen.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr Version 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Verwenden von Visual Studio 2012 mit diesem Tutorial
>
>
> Gehen Sie folgendermaßen vor, um Visual Studio 2012 mit diesem Tutorial zu verwenden:
>
> - Aktualisieren Sie den [Paket-Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.
> - Installieren Sie den [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Suchen Sie im Webplattform-Installer nach **ASP.net and Web Tools 2013,1 für Visual Studio 2012**, und installieren Sie ihn. Dadurch werden Visual Studio-Vorlagen für signalr-Klassen wie z. b. **Hub**installiert.
> - Einige Vorlagen (z. b. **owin-Startklasse**) sind nicht verfügbar. Verwenden Sie für diese stattdessen eine Klassendatei.
>
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

## <a name="overview"></a>Übersicht über

Ein signalr-Server wird in der Regel in einer ASP.NET-Anwendung in IIS gehostet, kann aber auch selbstgeh ostet (z. b. in einer Konsolenanwendung oder einem Windows-Dienst) mithilfe der Self-Host-Bibliothek verwendet werden. Diese Bibliothek basiert wie alle signalr 2 auf owin ([Open Web Interface for .net](http://owin.org)). Owin definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen. Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für das selbst Hosting einer Webanwendung in Ihrem eigenen Prozess außerhalb von IIS geeignet ist.

Gründe für das Hosting in IIS:

- Umgebungen, in denen IIS nicht verfügbar oder erwünscht ist, z. b. eine vorhandene Serverfarm ohne IIS.
- Der Leistungs Aufwand von IIS muss vermieden werden.
- Die signalr-Funktionalität muss einer vorhandenen Anwendung hinzugefügt werden, die in einem Windows-Dienst, einer Azure-workerrolle oder einem anderen Prozess ausgeführt wird.

Wenn eine Lösung aus Leistungsgründen als selbstgeh ostet entwickelt wird, wird empfohlen, auch die in IIS gehostete Anwendung zu testen, um den Leistungsvorteil zu ermitteln.

Dieses Tutorial enthält die folgenden Abschnitte:

- [Erstellen des Servers](#server)
- [Zugreifen auf den Server mit einem JavaScript-Client](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Erstellen des Servers

In diesem Tutorial erstellen Sie einen Server, der in einer Konsolenanwendung gehostet wird. der Server kann jedoch in jeder Art von Prozess gehostet werden, z. b. in einem Windows-Dienst oder einer Azure-workerrolle. Beispielcode zum Hosting eines signalr-Servers in einem Windows-Dienst finden Sie unter [Self-Hosting von signalr in einem Windows-Dienst](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Öffnen Sie Visual Studio 2013 mit Administratorrechten. Wählen Sie **Datei**, **Neues Projekt**aus. Wählen Sie unter dem Knoten " **Visual C#**  " im Bereich **Vorlagen** die Option **Windows** aus, und wählen Sie die Vorlage **Konsolenanwendung** aus. Nennen Sie das neue Projekt "signalrselfhost", und klicken Sie auf **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Öffnen Sie die nuget-Paket-Manager-Konsole, indem Sie **Tools** > **nuget-Paket-Manager** > Paket-Manager- **Konsole**
3. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Mit diesem Befehl werden die selbst Host Bibliotheken signalr 2 dem Projekt hinzugefügt.
4. Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Mit diesem Befehl wird die Bibliothek "Microsoft. owin. cors" dem Projekt hinzugefügt. Diese Bibliothek wird für die Domänen übergreifende Unterstützung verwendet, die für Anwendungen erforderlich ist, die signalr und einen Webseiten Client in verschiedenen Domänen hosten. Da Sie den signalr-Server und den WebClient auf unterschiedlichen Ports Hosting, bedeutet dies, dass die Domänen übergreifende Kommunikation zwischen diesen Komponenten aktiviert werden muss.
5. Ersetzen Sie den Inhalt von Program.cs durch den folgenden Code.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Der obige Code umfasst drei Klassen:

    - **Programm**, einschließlich der **Main** -Methode, die den primären Ausführungs Pfad definiert. Bei dieser Methode wird eine Webanwendung vom Typ **Startup** an der angegebenen URL (`http://localhost:8080`) gestartet. Wenn für den Endpunkt Sicherheit erforderlich ist, kann SSL implementiert werden. Weitere Informationen finden [Sie unter Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat](https://msdn.microsoft.com/library/ms733791.aspx) .
    - **Start**: die Klasse, die die Konfiguration für den signalr-Server enthält (die einzige Konfiguration, die in diesem Tutorial verwendet wird, ist der `UseCors`) und der `MapSignalR`, der Routen für beliebige Hub-Objekte im Projekt erstellt.
    - **Myhub**, die signalr-Hub-Klasse, die von der Anwendung für Clients bereitgestellt wird. Diese Klasse verfügt über eine einzige Methode, **Send**, die von Clients aufgerufen wird, um eine Nachricht an alle anderen verbundenen Clients zu senden.
6. Kompilieren Sie die Anwendung, und führen Sie sie aus. Die Adresse, die auf dem Server ausgeführt wird, sollte in einem Konsolenfenster angezeigt werden.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Wenn die Ausführung mit der Ausnahme `System.Reflection.TargetInvocationException was unhandled`fehlschlägt, müssen Sie Visual Studio mit Administratorrechten neu starten.
8. Die Anwendung wird beendet, bevor mit dem nächsten Abschnitt fortgefahren wird.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Zugreifen auf den Server mit einem JavaScript-Client

In diesem Abschnitt verwenden Sie den gleichen JavaScript-Client aus dem [Tutorial zu](../getting-started/tutorial-getting-started-with-signalr.md)den ersten Schritten. Wir nehmen nur eine Änderung am Client vor, d. h. die Hub-URL wird explizit definiert. Bei einer selbst gehosteten Anwendung ist der Server möglicherweise nicht unbedingt mit der Verbindungs-URL identisch (aufgrund von Reverseproxys und Lasten Ausgleichs Modulen), daher muss die URL explizit definiert werden.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Hinzufügen**, **Neues Projekt**aus. Wählen Sie den **webknoten** aus, und wählen Sie die Vorlage **ASP.NET Webanwendung** aus. Nennen Sie das Projekt "javascriptclient", und klicken Sie auf **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Wählen Sie die **leere** Vorlage aus, und lassen Sie die Option verbleibende Optionen deaktiviert. Wählen Sie **Projekt erstellen**aus.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Wählen Sie in der Paket-Manager-Konsole das Projekt "javascriptclient" in der Dropdown-Dropdown- **Standard Projekt** aus, und führen Sie den folgenden Befehl aus:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Dieser Befehl installiert die signalr-und jQuery-Bibliotheken, die Sie auf dem Client benötigen.
4. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**, **Neues Element**. Wählen Sie den **webknoten** aus, und wählen Sie HTML-Seite aus. Nennen Sie die Seite **default. html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Ersetzen Sie den Inhalt der neuen HTML-Seite durch den folgenden Code. Überprüfen Sie, ob die Skript Verweise hier mit den Skripts im Ordner "Scripts" des Projekts zu vergleichen sind.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Der folgende Code (der im obigen Codebeispiel hervorgehoben ist) ist das Hinzufügen, das Sie an dem Client vorgenommen haben, der im Lernprogramm "erste starteschlange" verwendet wurde (zusätzlich zum Aktualisieren des Codes auf signalr, Version 2 Beta). Diese Codezeile legt die basisverbindungs-URL für signalr auf dem Server explizit fest.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Start Projekte festlegen...** aus. Aktivieren Sie das Optionsfeld **mehrere Start Projekte** , und legen Sie die **Aktion** für beide Projekte auf **Start**fest.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Klicken Sie mit der rechten Maustaste auf "default. html", und wählen Sie **als Start Seite festlegen**aus.
8. Führen Sie die Anwendung aus. Der Server und die Seite werden gestartet. Möglicherweise müssen Sie die Webseite erneut laden (oder im Debugger **fortsetzen** ), wenn die Seite geladen wird, bevor der Server gestartet wird.
9. Geben Sie im Browser einen Benutzernamen an, wenn Sie dazu aufgefordert werden. Kopieren Sie die URL der Seite in eine andere Browser Registerkarte oder ein anderes Fenster, und geben Sie einen anderen Benutzernamen Sie können Nachrichten aus einem Browserbereich an den anderen senden, wie im Lernprogramm für die ersten Schritte.
