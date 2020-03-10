---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Einstieg in signalr 1. x | Microsoft-Dokumentation'
author: bradygaster
description: Verwenden Sie ASP.net signalr, um eine echt Zeit Chat-Anwendung auf einer HTML-Seite zu erstellen.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505749"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a>Tutorial: Einstieg in signalr 1. x

von [Patrick Fletcher](https://github.com/pfletcher), [Tim teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie fügen signalr einer leeren ASP.NET-Webanwendung hinzu und erstellen eine HTML-Seite, um Nachrichten zu senden und anzuzeigen.

## <a name="overview"></a>Übersicht

In diesem Tutorial wird die signalr-Entwicklung eingeführt, indem gezeigt wird, wie eine einfache browserbasierte Chatanwendung erstellt wird. Fügen Sie die signalr-Bibliothek einer leeren ASP.NET-Webanwendung hinzu, erstellen Sie eine Hub-Klasse zum Senden von Nachrichten an Clients, und erstellen Sie eine HTML-Seite, mit der Benutzer Chat Nachrichten senden und empfangen können. Ein ähnliches Tutorial, das zeigt, wie eine Chat-Anwendung in MVC 4 mithilfe einer MVC-Ansicht erstellt wird, finden Sie unter [Getting Started with signalr and MVC 4](index.md).

> [!NOTE]
> In diesem Tutorial wird die Releaseversion (1. x) von signalr verwendet. Ausführliche Informationen zu Änderungen zwischen signalr 1. x und 2,0 finden Sie unter [Aktualisieren von signalr 1. x-Projekten](../releases/upgrading-signalr-1x-projects-to-20.md).

Signalr ist eine Open-Source-.NET-Bibliothek zum Entwickeln von Webanwendungen, die eine Live-Benutzerinteraktion oder Echtzeitdaten Aktualisierungen erfordern. Beispiele hierfür sind soziale Anwendungen, Spiele mit mehreren Benutzern, geschäftliche Zusammenarbeit und Nachrichten-, Wetter-oder Finanz Update Anwendungen. Diese werden häufig als Echtzeitanwendungen bezeichnet.

Signalr vereinfacht das Entwickeln von Echtzeitanwendungen. Es umfasst eine ASP.NET-Server Bibliothek und eine JavaScript-Client Bibliothek, um die Verwaltung von Client-/Server-Verbindungen und das Übertragung von Inhalts Aktualisierungen an Clients zu vereinfachen. Sie können die signalr-Bibliothek einer vorhandenen ASP.NET-Anwendung hinzufügen, um Echtzeitfunktionen zu erhalten.

In diesem Tutorial werden die folgenden signalr-Entwicklungsaufgaben veranschaulicht:

- Hinzufügen der signalr-Bibliothek zu einer ASP.NET-Webanwendung.
- Erstellen einer Hub-Klasse zum Übertragung von Inhalten an Clients.
- Verwenden der signalr jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Anzeigen von Updates vom Hub.

Der folgende Screenshot zeigt die Chat-Anwendung, die in einem Browser ausgeführt wird. Jeder neue Benutzer kann Kommentare Posten und Kommentare anzeigen, die nach dem Join des Benutzers hinzugefügt wurden.

![Chat Instanzen](tutorial-getting-started-with-signalr/_static/image1.png)

Strecken

- [Einrichten des Projekts](#setup)
- [Ausführen des Beispiels](#run)
- [Untersuchen des Codes](#code)
- [Next Steps](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Einrichten des Projekts

In diesem Abschnitt wird gezeigt, wie Sie eine leere ASP.NET-Webanwendung erstellen, signalr hinzufügen und die Chat-Anwendung erstellen.

Erforderliche Komponenten:

- Visual Studio 2010 SP1 oder 2012. Wenn Sie nicht über Visual Studio verfügen, finden Sie weitere Informationen unter [ASP.net Downloads](https://www.asp.net/downloads) , um das kostenlose Visual Studio 2012 Express-Entwicklungs Tool zu erhalten.
- [Microsoft ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941). Für Visual Studio 2012 fügt dieses Installationsprogramm neue ASP.NET-Features einschließlich signalr-Vorlagen zu Visual Studio hinzu. Für Visual Studio 2010 SP1 ist ein Installer nicht verfügbar. Sie können das Tutorial aber durch Installieren des signalr-nuget-Pakets durchführen, wie in den Setup Schritten beschrieben.

In den folgenden Schritten wird Visual Studio 2012 verwendet, um eine leere ASP.NET-Webanwendung zu erstellen und die signalr-Bibliothek hinzuzufügen:

1. Erstellen Sie in Visual Studio eine ASP.net leere Webanwendung.

    ![Leeres Web erstellen](tutorial-getting-started-with-signalr/_static/image2.png)
2. Öffnen Sie die **Paket-Manager-Konsole** , indem Sie auf Extras **| Nuget-Paket-Manager | Paket-Manager-Konsole**. Geben Sie im Konsolenfenster den folgenden Befehl ein:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Mit diesem Befehl wird die aktuellste Version von signalr 1. x installiert.
3. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen | -Klasse**. Nennen Sie die neue Klasse **chathub**.
4. Erweitern Sie in **Projektmappen-Explorer** den Knoten Skripts. Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.

    ![Bibliotheks Verweise](tutorial-getting-started-with-signalr/_static/image3.png)
5. Ersetzen Sie den Code in der **chathub** -Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen. Neues Element**. Wählen Sie im Dialogfeld **Neues Element hinzufügen** **globale Anwendungsklasse** aus, und klicken Sie auf **Hinzufügen**.

    ![Global hinzufügen](tutorial-getting-started-with-signalr/_static/image4.png)
7. Fügen Sie nach den bereitgestellten `using` Anweisungen in der Global.asax.cs-Klasse die folgenden `using`-Anweisungen hinzu.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Fügen Sie in der `Application_Start`-Methode der Global-Klasse die folgende Codezeile hinzu, um die Standardroute für signalr-Hubs zu registrieren.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen. Neues Element**. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option HTML, und klicken Sie auf **Hinzufügen**.
10. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die soeben erstellte HTML-Seite, und klicken Sie dann auf **als Start Seite festlegen**.
11. Ersetzen Sie den Standard Code in der HTML-Seite durch den folgenden Code.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen. Die HTML-Seite wird in einer Browser Instanz geladen, und Sie werden zur Eingabe eines Benutzernamens aufgefordert.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image5.png)
2. Geben Sie einen Benutzernamen ein.
3. Kopieren Sie die URL aus der Adresszeile des Browsers, und verwenden Sie Sie, um zwei weitere Browser Instanzen zu öffnen. Geben Sie in jeder Browser Instanz einen eindeutigen Benutzernamen ein.
4. Fügen Sie in jeder Browser Instanz einen Kommentar hinzu, und klicken Sie auf **senden**. Die Kommentare sollten in allen Browser Instanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei. Der Hub überträgt Kommentare an alle aktuellen Benutzer. Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.

    Der folgende Screenshot zeigt die Chat-Anwendung, die in drei Browser Instanzen ausgeführt wird. Diese werden aktualisiert, wenn eine Instanz eine Nachricht sendet:

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image6.png)
5. Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird. Es gibt eine Skriptdatei mit dem Namen **Hubs** , die von der signalr-Bibliothek zur Laufzeit dynamisch generiert wird. Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.

    ![Generiertes Hub-Skript](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Untersuchen des Codes

Die signalr Chat-Anwendung veranschaulicht zwei grundlegende signalr-Entwicklungsaufgaben: das Erstellen eines Hubs als hauptkoordinations Objekt auf dem Server und das Verwenden der signalr jQuery-Bibliothek zum Senden und empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR-Hubs

Im Codebeispiel wird die **chathub** -Klasse von der **Microsoft. Aspnet. signalr. Hub** -Klasse abgeleitet. Die Ableitung von der **Hub** -Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung. Sie können öffentliche Methoden für Ihre hubklasse erstellen und dann auf diese Methoden zugreifen, indem Sie Sie von jQuery-Skripts auf einer Webseite aufrufen.

Im chatcode wird die **chathub. Send** -Methode von Clients aufgerufen, um eine neue Nachricht zu senden. Der Hub sendet die Nachricht seinerseits an alle Clients, indem er " **Clients. all. broadcastmessage**" aufruft.

Die **Send** -Methode veranschaulicht verschiedene Hub-Konzepte:

- Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.
- Verwenden Sie die dynamische Eigenschaft **Microsoft. Aspnet. signalr. Hub. Clients** für den Zugriff auf alle Clients, die mit diesem Hub verbunden sind.
- Zum Aktualisieren von Clients wird eine jQuery-Funktion auf dem Client (z. b. die `broadcastMessage`-Funktion) aufgerufen.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr und jQuery

Die HTML-Seite im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird. Die wesentlichen Aufgaben im Code sind das Deklarieren eines Proxys für den Verweis auf den Hub und das Deklarieren einer Funktion, die der Server zum Übertragen von Inhalten an Clients und zum Starten einer Verbindung zum Senden von Nachrichten an den Hub aufruft.

Der folgende Code deklariert einen Proxy für einen Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> In jQuery wird der Verweis auf die Serverklasse und ihre Member in der Kamel-Schreibweise angezeigt. Das Codebeispiel verweist auf C# die **chathub** -Klasse in jQuery als **chathub**.

Im folgenden Code wird erläutert, wie Sie eine Rückruffunktion im Skript erstellen. Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen. Die zwei Zeilen, in denen der Inhalt vor dem Anzeigen von HTML codiert wird, sind optional und stellen eine einfache Möglichkeit dar, Skript Injektion zu verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Der folgende Code zeigt, wie eine Verbindung mit dem Hub geöffnet wird. Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis in der Schaltfläche " **senden** " auf der HTML-Seite zu verarbeiten.

> [!NOTE]
> Dieser Ansatz stellt sicher, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, dass signalr ein Framework zum Entwickeln von Echt Zeit Webanwendungen ist. Außerdem haben Sie verschiedene signalr-Entwicklungsaufgaben kennengelernt: Hinzufügen von signalr zu einer ASP.NET-Anwendung, Erstellen einer Hub-Klasse und senden und empfangen von Nachrichten vom Hub.

Sie können die Beispielanwendung in diesem Tutorial oder in anderen signalr-Anwendungen über das Internet zur Verfügung stellen, indem Sie Sie für einen Hostinganbieter bereitstellen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem kostenlosen [Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)an. Eine exemplarische Vorgehensweise zum Bereitstellen der Beispiel-signalr-Anwendung finden Sie unter Veröffentlichen des Beispiels für die ersten Schritte mit [signalr als Windows Azure-Website](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Ausführliche Informationen zum Bereitstellen eines Visual Studio-Webprojekts auf einer Windows Azure-Website finden Sie unter Bereitstellen [einer ASP.NET-Anwendung auf einer Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)-Website. (Hinweis: der WebSocket-Transport wird für Windows Azure-Websites derzeit nicht unterstützt. Wenn der WebSocket-Transport nicht verfügbar ist, verwendet signalr die anderen verfügbaren Transporte, wie im Abschnitt Transporte des [Themas Introduction to signalr](index.md)beschrieben.)

Weitere Informationen zu den erweiterten Konzepten der signalr-Entwicklung finden Sie auf den folgenden Websites für signalr-Quellcode und-Ressourcen:

- [Signalr-Projekt](http://signalr.net)
- [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)
- [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)
