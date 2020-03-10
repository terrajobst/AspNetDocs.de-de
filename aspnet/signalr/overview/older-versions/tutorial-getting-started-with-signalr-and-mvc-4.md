---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Einstieg in signalr 1. x und MVC 4 | Microsoft-Dokumentation'
author: bradygaster
description: Verwenden Sie ASP.net signalr und ASP.NET MVC 4, um eine echt Zeit Chat-Anwendung zu erstellen.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468069"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Tutorial: Einstieg in signalr 1. x und MVC 4

von [Patrick Fletcher](https://github.com/pfletcher), [Tim teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Tutorial wird gezeigt, wie Sie ASP.net signalr verwenden, um eine echt Zeit Chat-Anwendung zu erstellen. Sie werden signalr einer MVC 4-Anwendung hinzufügen und eine Chat Ansicht erstellen, um Nachrichten zu senden und anzuzeigen.

## <a name="overview"></a>Übersicht

Dieses Tutorial bietet eine Einführung in die Entwicklung von Webanwendungen in Echtzeit mit ASP.net signalr und ASP.NET MVC 4. Das Tutorial verwendet den gleichen Chat-Anwendungscode wie das Tutorial zu den ersten Schritten mit [signalr](tutorial-getting-started-with-signalr.md), aber zeigt, wie Sie es einer MVC 4-Anwendung auf der Grundlage der Internet Vorlage hinzufügen.

In diesem Thema erfahren Sie mehr über die folgenden signalr-Entwicklungsaufgaben:

- Hinzufügen der signalr-Bibliothek zu einer MVC 4-Anwendung.
- Erstellen einer Hub-Klasse zum Übertragung von Inhalten an Clients.
- Verwenden der signalr jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Anzeigen von Updates vom Hub.

Der folgende Screenshot zeigt die abgeschlossene Chat-Anwendung, die in einem Browser ausgeführt wird.

![Chat Instanzen](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Strecken

- [Einrichten des Projekts](#setup)
- [Ausführen des Beispiels](#run)
- [Untersuchen des Codes](#code)
- [Next Steps](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Einrichten des Projekts

Erforderliche Komponenten:

- Visual Studio 2010 SP1, Visual Studio 2012 oder Visual Studio 2012 Express. Wenn Sie nicht über Visual Studio verfügen, finden Sie weitere Informationen unter [ASP.net Downloads](https://www.asp.net/downloads) , um das kostenlose Visual Studio 2012 Express-Entwicklungs Tool zu erhalten.
- Installieren Sie [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)für Visual Studio 2010.

In diesem Abschnitt wird gezeigt, wie Sie eine ASP.NET MVC 4-Anwendung erstellen, die signalr-Bibliothek hinzufügen und die Chat-Anwendung erstellen.

1. 1. Erstellen Sie in Visual Studio eine ASP.NET MVC 4-Anwendung, benennen Sie Sie signalrchat, und klicken Sie auf OK.

        > [!NOTE]
        > Wählen Sie in VS 2010 im Dropdown-Steuerelement Framework-Version die Option **.NET Framework 4** aus. Signalr-Code wird auf .NET Framework-Versionen 4 und 4,5 ausgeführt.

        ![MVC-Web erstellen](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Wählen Sie die Vorlage Internet Anwendung aus, deaktivieren Sie die Option zum **Erstellen eines Komponenten Testprojekts**, und klicken Sie auf OK.

         ![Erstellen einer MVC-Internet Site](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Öffnen Sie die **Tools > nuget-Paket-Manager > Paket-Manager-Konsole** und führen Sie den folgenden Befehl aus. In diesem Schritt wird dem Projekt ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die die signalr-Funktionalität ermöglichen.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. Erweitern Sie in **Projektmappen-Explorer** den Ordner Skripts. Beachten Sie, dass dem Projekt Skript Bibliotheken für signalr hinzugefügt wurden.

         ![Bibliotheks Verweise](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner mit dem Namen **Hubs**hinzu.
      6. Klicken Sie mit der rechten Maustaste auf den Ordner **Hubs** und dann auf **Hinzufügen -Klasse**, und erstellen Sie C# eine neue Klasse mit dem Namen **ChatHub.cs**. Sie verwenden diese Klasse als signalr-Server-Hub, der Nachrichten an alle Clients sendet.

> [!NOTE]
> Wenn Sie Visual Studio 2012 verwenden und das [ASP.net and Web Tools 2012,2-Update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)installiert haben, können Sie die neue signalr-Element Vorlage verwenden, um die Hub-Klasse zu erstellen. Klicken Sie hierzu mit der rechten Maustaste auf den Ordner **Hubs** , und klicken Sie auf **hinzufügen. Neues Element**, wählen Sie **signalr-Hub-Klasse (v1)** aus, und benennen Sie die Klasse **ChatHub.cs**.

1. Ersetzen Sie den Code in der **chathub** -Klasse durch den folgenden Code.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Öffnen Sie die Datei " **Global. asax** " für das Projekt, und fügen Sie der-Methode einen aufzurufenden `RouteTable.Routes.MapHubs();` als erste Codezeile in der `Application_Start`-Methode hinzu. Mit diesem Code wird die Standardroute für signalr Hubs registriert und muss aufgerufen werden, bevor Sie andere Routen registrieren. Die abgeschlossene `Application_Start`-Methode sieht wie im folgenden Beispiel aus.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Bearbeiten Sie die `HomeController`-Klasse, die in **Controllers/HomeController. cs** gefunden wurde, und fügen Sie der-Klasse die folgende Methode hinzu. Diese Methode gibt die **Chat** Ansicht zurück, die Sie in einem späteren Schritt erstellen.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Klicken Sie mit der rechten Maustaste in die soeben erstellte `Chat` Methode, und klicken Sie auf **Ansicht hinzufügen** , um eine neue Ansichts Datei zu erstellen.
5. Vergewissern Sie sich, dass im Dialogfeld **Ansicht hinzufügen** das Kontrollkästchen für die **Verwendung eines Layouts oder einer Master Seite** aktiviert ist (deaktivieren Sie die anderen Kontrollkästchen), und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Bearbeiten Sie die neue Ansichts Datei mit dem Namen " **Chat. cshtml**". Fügen Sie nach dem Tag &lt;H2&gt; den folgenden &lt;div&gt; Abschnitt und `@section scripts` Codeblock in die Seite ein. Dieses Skript ermöglicht es der Seite, Chat Nachrichten zu senden und Nachrichten vom Server anzuzeigen. Der gesamte Code für die Chat Ansicht wird im folgenden Codeblock angezeigt.

    > [!IMPORTANT]
    > Wenn Sie signalr und andere Skript Bibliotheken zu Ihrem Visual Studio-Projekt hinzufügen, installiert der Paket-Manager möglicherweise Versionen der Skripts, die aktueller als die in diesem Thema gezeigten Versionen sind. Stellen Sie sicher, dass die Skript Verweise im Code den Versionen der Skript Bibliotheken entsprechen, die in Ihrem Projekt installiert sind.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Speichern Sie alle** für das Projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Drücken Sie F5, um das Projekt im Debugmodus auszuführen.
2. Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt an. Die Chat Seite wird in einer Browser Instanz geladen, und Sie werden zur Eingabe eines Benutzernamens aufgefordert.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Geben Sie einen Benutzernamen ein.
4. Kopieren Sie die URL aus der Adresszeile des Browsers, und verwenden Sie Sie, um zwei weitere Browser Instanzen zu öffnen. Geben Sie in jeder Browser Instanz einen eindeutigen Benutzernamen ein.
5. Fügen Sie in jeder Browser Instanz einen Kommentar hinzu, und klicken Sie auf **senden**. Die Kommentare sollten in allen Browser Instanzen angezeigt werden.

    > [!NOTE]
    > Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei. Der Hub überträgt Kommentare an alle aktuellen Benutzer. Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.
6. Der folgende Screenshot zeigt die Chat-Anwendung, die in einem Browser ausgeführt wird.

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird. Dieser Knoten ist im Debugmodus sichtbar, wenn Sie Internet Explorer als Browser verwenden. Es gibt eine Skriptdatei mit dem Namen **Hubs** , die von der signalr-Bibliothek zur Laufzeit dynamisch generiert wird. Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code. Wenn Sie einen anderen Browser als Internet Explorer verwenden, können Sie auch auf die Datei "Dynamic **Hubs** " zugreifen, indem Sie direkt zu dieser Datei navigieren, z. b. http://mywebsite/signalr/hubs.

    ![Generiertes Hub-Skript](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Untersuchen des Codes

Die signalr Chat-Anwendung veranschaulicht zwei grundlegende signalr-Entwicklungsaufgaben: das Erstellen eines Hubs als hauptkoordinations Objekt auf dem Server und das Verwenden der signalr jQuery-Bibliothek zum Senden und empfangen von Nachrichten.

### <a name="signalr-hubs"></a>SignalR-Hubs

Im Codebeispiel wird die **chathub** -Klasse von der **Microsoft. Aspnet. signalr. Hub** -Klasse abgeleitet. Die Ableitung von der **Hub** -Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung. Sie können öffentliche Methoden für Ihre hubklasse erstellen und dann auf diese Methoden zugreifen, indem Sie Sie von jQuery-Skripts auf einer Webseite aufrufen.

Im chatcode wird die **chathub. Send** -Methode von Clients aufgerufen, um eine neue Nachricht zu senden. Der Hub sendet die Nachricht seinerseits an alle Clients, indem er " **Clients. all. addnewmessagetopage**" aufruft.

Die **Send** -Methode veranschaulicht verschiedene Hub-Konzepte:

- Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.
- Verwenden Sie die Eigenschaft **Microsoft. Aspnet. signalr. Hub. Clients** , um auf alle Clients zuzugreifen, die mit diesem Hub verbunden sind.
- Zum Aktualisieren von Clients wird eine jQuery-Funktion auf dem Client (z. b. die `addNewMessageToPage`-Funktion) aufgerufen.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr und jQuery

Die **Chat. cshtml** -Ansichts Datei im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird. Die wesentlichen Aufgaben im Code sind das Erstellen eines Verweises auf den automatisch generierten Proxy für den Hub, das Deklarieren einer Funktion, die der Server zum Übertragen von Inhalten an Clients und das Starten einer Verbindung zum Senden von Nachrichten an den Hub aufruft.

Der folgende Code deklariert einen Proxy für einen Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> In jQuery wird der Verweis auf die Serverklasse und ihre Member in der Kamel-Schreibweise angezeigt. Das Codebeispiel verweist auf C# die **chathub** -Klasse in jQuery als **chathub**. Wenn Sie in jQuery mit herkömmlicher Pascal-Schreibweise wie in C#auf die `ChatHub`-Klasse verweisen möchten, bearbeiten Sie die ChatHub.cs-Klassendatei. Fügen Sie eine `using` Anweisung hinzu, um auf den `Microsoft.AspNet.SignalR.Hubs` Namespace zu verweisen. Fügen Sie dann der `ChatHub`-Klasse das `HubName`-Attribut hinzu, z. b. `[HubName("ChatHub")]`. Aktualisieren Sie abschließend den jQuery-Verweis auf die `ChatHub`-Klasse.

Der folgende Code zeigt, wie eine Rückruffunktion im Skript erstellt wird. Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen. Der optionale-Befehl für die `htmlEncode`-Funktion zeigt, wie Sie den Nachrichten Inhalt vor der Anzeige auf der Seite in HTML codieren, um die Skript Injektion zu verhindern.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Der folgende Code zeigt, wie eine Verbindung mit dem Hub geöffnet wird. Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis auf der Schaltfläche " **senden** " auf der Chat Seite zu verarbeiten.

> [!NOTE]
> Mit diesem Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, dass signalr ein Framework zum Entwickeln von Echt Zeit Webanwendungen ist. Außerdem haben Sie verschiedene signalr-Entwicklungsaufgaben kennengelernt: Hinzufügen von signalr zu einer ASP.NET-Anwendung, Erstellen einer Hub-Klasse und senden und empfangen von Nachrichten vom Hub.

Weitere Informationen zu den erweiterten Konzepten der signalr-Entwicklung finden Sie auf den folgenden Websites für signalr-Quellcode und-Ressourcen:

- [Signalr-Projekt](http://signalr.net)
- [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)
- [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)
