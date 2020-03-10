---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Echt Zeit Chat mit signalr 2 und MVC 5 | Microsoft-Dokumentation'
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie ASP.net signalr 2 verwendet wird, um eine echt Zeit Chat-Anwendung zu erstellen. Sie fügen signalr einer MVC 5-Anwendung hinzu.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431619"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Tutorial: Echt Zeit Chat mit signalr 2 und MVC 5

In diesem Tutorial wird gezeigt, wie ASP.net signalr 2 verwendet wird, um eine echt Zeit Chat-Anwendung zu erstellen. Sie können signalr zu einer MVC 5-Anwendung hinzufügen und eine Chat Ansicht erstellen, um Nachrichten zu senden und anzuzeigen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten des Projekts
> * Ausführen des Beispiels
> * Untersuchen des Codes

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**

## <a name="set-up-the-project"></a>Einrichten des Projekts

In diesem Abschnitt wird gezeigt, wie Sie mit Visual Studio 2017 und signalr 2 eine leere ASP.NET MVC 5-Anwendung erstellen, die signalr-Bibliothek hinzufügen und die Chat-Anwendung erstellen.

1. Erstellen Sie in Visual Studio eine C# ASP.NET-Anwendung, die auf .NET Framework 4,5 abzielt, benennen Sie Sie mit signalrchat, und klicken Sie auf OK.

    ![Web erstellen](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. Wählen Sie in **New ASP.NET Webanwendung-signalrmvcchat**die Option **MVC** aus, und wählen Sie dann **Authentifizierung ändern**aus.

1. Wählen Sie unter **Authentifizierung ändern**die Option **keine Authentifizierung** , und klicken Sie auf **OK**.

    ![Keine Authentifizierung auswählen](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. Wählen Sie in **New ASP.NET Webanwendung-signalrmvcchat**die Option **OK**aus.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie unter **Neues Element hinzufügen-signalrchat**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.

1. Benennen Sie die Klasse *chathub* , und fügen Sie Sie dem Projekt hinzu.

    In diesem Schritt wird die *ChatHub.cs* -Klassendatei erstellt, und dem Projekt wird ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.

1. Ersetzen Sie den Code in der neuen *ChatHub.cs* -Klassendatei durch den folgenden Code:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Klasse** **Hinzufügen** .

1. Nennen Sie die neue Klasse *Startup* , und fügen Sie Sie dem Projekt hinzu.

1. Ersetzen Sie den Code in der *Startup.cs* -Klassendatei durch den folgenden Code:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. WählenSie in Projektmappen-Explorer **Controller** > **HomeController.cs**aus.

1. Fügen Sie der *HomeController.cs*diese Methode hinzu.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Diese Methode gibt die **Chat** Ansicht zurück, die Sie in einem späteren Schritt erstellen.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf **Ansichten** > **Startseite**, und wählen Sie >  **Ansicht** **Hinzufügen** aus.

1. Benennen Sie in **Ansicht hinzufügen**den neuen View **Chat** , und wählen Sie **Hinzufügen**aus.

1. Ersetzen Sie den Inhalt von **Chat. cshtml** durch den folgenden Code:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.

    Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.

    > [!IMPORTANT]
    > Der Paket-Manager hat möglicherweise eine neuere Version der signalr-Skripts installiert.

1. Überprüfen Sie, ob die Skript Verweise im Codeblock den Versionen der Skriptdateien im Projekt entsprechen.

    Skript Verweise aus dem ursprünglichen Codeblock:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Wenn keine Entsprechung gefunden wird, aktualisieren Sie die *cshtml* -Datei.

1. Wählen Sie in der Menüleiste **Datei** > **Alle speichern**aus.

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um das Beispiel im Debugmodus auszuführen.

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Wenn der Browser geöffnet wird, geben Sie einen Namen für Ihre Chat Identität ein.

1. Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.

1. Geben Sie in jedem Browser einen eindeutigen Namen ein.

1. Fügen Sie nun einen Kommentar hinzu, und wählen Sie **senden**aus. Wiederholen Sie diesen Vorgang in den anderen Browsern. Die Kommentare werden in Echtzeit angezeigt.

    > [!NOTE]
    > Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei. Der Hub überträgt Kommentare an alle aktuellen Benutzer. Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.

    Sehen Sie, wie die Chat-Anwendung in drei verschiedenen Browsern ausgeführt wird. Wenn Tom, Anand und Susan Nachrichten senden, werden alle Browser in Echtzeit aktualisiert:

    ![Alle drei Browser zeigen denselben Chatverlauf an.](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird. Es gibt eine Skriptdatei mit dem Namen *Hubs* , die die signalr-Bibliothek zur Laufzeit generiert. Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.

    ![automatisch generiertes Hubs-Skript im Knoten "Skript Dokumente"](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Untersuchen des Codes

In der signalr Chat-Anwendung werden zwei grundlegende signalr-Entwicklungsaufgaben veranschaulicht. Es zeigt, wie Sie einen Hub erstellen. Der Server verwendet diesen Hub als hauptkoordinations Objekt. Der Hub verwendet die signalr jQuery-Bibliothek, um Nachrichten zu senden und zu empfangen.

### <a name="signalr-hubs-in-the-chathubcs"></a>Signalr Hubs in ChatHub.cs

Im Codebeispiel wird die `ChatHub`-Klasse von der `Microsoft.AspNet.SignalR.Hub`-Klasse abgeleitet. Das Ableiten von der `Hub`-Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung. Sie können öffentliche Methoden für Ihre Hub-Klasse erstellen und dann auf diese Methoden zugreifen, indem Sie Sie aus Skripts auf einer Webseite aufrufen.

Im chatcode wird die `ChatHub.Send`-Methode aufgerufen, um eine neue Nachricht zu senden. Der Hub sendet die Nachricht seinerseits an alle Clients, indem `Clients.All.addNewMessageToPage`aufgerufen wird.

Die `Send`-Methode veranschaulicht verschiedene Hub-Konzepte:

* Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.

* Verwenden Sie die dynamische `Microsoft.AspNet.SignalR.Hub.Clients`-Eigenschaft, um mit allen Clients zu kommunizieren, die mit diesem Hub verbunden sind.

* Es wird eine Funktion auf dem Client aufgerufen (z. b. die `addNewMessageToPage` Funktion), um Clients zu aktualisieren.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>Signalr und jQuery Chat. cshtml

Die *Chat. cshtml* -Ansichts Datei im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird.  Der Code führt viele wichtige Aufgaben aus. Er erstellt einen Verweis auf den automatisch generierten Proxy für den Hub, deklariert eine Funktion, die der Server zum Übertragen von Inhalten an Clients senden kann, und startet eine Verbindung, um Nachrichten an den Hub zu senden.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> In JavaScript befindet sich der Verweis auf die Serverklasse und ihre Member in "CamelCase". Das Codebeispiel verweist auf C# die `ChatHub`-Klasse in JavaScript als `chatHub`.

In diesem Codeblock erstellen Sie eine Rückruffunktion im Skript.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen. Der optionale-Befehl für die `htmlEncode`-Funktion zeigt, wie Sie den Nachrichten Inhalt vor der Anzeige auf der Seite in HTML codieren können. Es ist eine Möglichkeit, Skript Injektion zu verhindern.

Dieser Code öffnet eine Verbindung mit dem Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Mit diesem Ansatz wird sichergestellt, dass Sie eine Verbindung herstellen, bevor der Ereignishandler ausgeführt wird.

Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis auf der Schaltfläche " **senden** " auf der Chat Seite zu verarbeiten.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:

* [Signalr-Projekt](http://signalr.net)

* [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)

* [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten des Projekts
> * Das Beispiel ausgeführt
> * Untersuchen des Codes

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um hoch frequentes Messaging Funktionen bereitzustellen.
> [!div class="nextstepaction"]
> [Web-App mit hoch frequentem Messaging](tutorial-high-frequency-realtime-with-signalr.md)