---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Echt Zeit Chat mit signalr 2 | Microsoft-Dokumentation'
author: bradygaster
description: Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie fügen signalr einer leeren ASP.NET-Webanwendung hinzu.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600472"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Tutorial: Echt Zeit Chat mit signalr 2

In diesem Tutorial wird gezeigt, wie Sie signalr verwenden, um eine echt Zeit Chat-Anwendung zu erstellen. Sie fügen signalr einer leeren ASP.NET-Webanwendung hinzu und erstellen eine HTML-Seite, um Nachrichten zu senden und anzuzeigen.

In diesem Tutorial:

> [!div class="checklist"]
> * Einrichten des Projekts
> * Beispielausführung
> * Untersuchen des Codes

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Erforderliche Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Arbeitsauslastung **ASP.net und Webentwicklung** .

## <a name="set-up-the-project"></a>Einrichten des Projekts

In diesem Abschnitt wird gezeigt, wie Sie mit Visual Studio 2017 und signalr 2 eine leere ASP.NET-Webanwendung erstellen, signalr hinzufügen und die Chat-Anwendung erstellen.

1. Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.

    ![Web erstellen](tutorial-getting-started-with-signalr/_static/image2.png)

1. Lassen Sie im Fenster **New ASP.net Project-signalrchat** den Eintrag **leer** , und wählen Sie **OK**aus.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie unter **Neues Element hinzufügen-signalrchat**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.

1. Benennen Sie die Klasse *chathub* , und fügen Sie Sie dem Projekt hinzu.

    In diesem Schritt wird die *ChatHub.cs* -Klassendatei erstellt, und dem Projekt wird ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.

1. Ersetzen Sie den Code in der neuen *ChatHub.cs* -Klassendatei durch den folgenden Code:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.

1. Wählen Sie in **Neues Element hinzufügen-signalrchat** die Option **installiert** > **Visual C#**  > **Web** aus, und wählen Sie dann **owin-Startklasse**

1. Benennen Sie die Klasse *Startup* , und fügen Sie Sie dem Projekt hinzu.

1. Ersetzen Sie den Standard Code in der *Startup* -Klasse durch den folgenden Code:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **HTML-Seite** **Hinzufügen** .

1. Benennen Sie den neuen Seiten *Index* , und wählen Sie **OK**aus.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die erstellte HTML-Seite, und wählen Sie **als Start Seite festlegen**aus.

1. Ersetzen Sie den Standard Code in der HTML-Seite durch folgenden Code:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.

    Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.

    > [!IMPORTANT]
    > Der Paket-Manager hat möglicherweise eine neuere Version der signalr-Skripts installiert.

1. Überprüfen Sie, ob die Skript Verweise im Codeblock den Versionen der Skriptdateien im Projekt entsprechen.

    Skript Verweise aus dem ursprünglichen Codeblock:

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Wenn keine Entsprechung gefunden wird, aktualisieren Sie die *HTML* -Datei.

1. Wählen Sie in der Menüleiste **Datei** > **Alle speichern**aus.

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um das Beispiel im Debugmodus auszuführen.

    ![Benutzernamen eingeben](tutorial-getting-started-with-signalr/_static/image3.png)

1. Wenn der Browser geöffnet wird, geben Sie einen Namen für Ihre Chat Identität ein.

1. Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.

1. Geben Sie in jedem Browser einen eindeutigen Namen ein.

1. Fügen Sie nun einen Kommentar hinzu, und wählen Sie **senden**aus. Wiederholen Sie diesen Vorgang in den anderen Browsern. Die Kommentare werden in Echtzeit angezeigt.

    > [!NOTE]
    > Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei. Der Hub überträgt Kommentare an alle aktuellen Benutzer. Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.

    Sehen Sie, wie die Chat-Anwendung in drei verschiedenen Browsern ausgeführt wird. Wenn Tom, Anand und Susan Nachrichten senden, werden alle Browser in Echtzeit aktualisiert:

    ![Alle drei Browser zeigen denselben Chatverlauf an.](tutorial-getting-started-with-signalr/_static/image4.png)

1. Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird. Es gibt eine Skriptdatei mit dem Namen *Hubs* , die die signalr-Bibliothek zur Laufzeit generiert. Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.

    ![automatisch generiertes Hubs-Skript im Knoten "Skript Dokumente"](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Untersuchen des Codes

In der signalrchat-Anwendung werden zwei grundlegende signalr-Entwicklungsaufgaben veranschaulicht. Es zeigt, wie Sie einen Hub erstellen. Der Server verwendet diesen Hub als hauptkoordinations Objekt. Der Hub verwendet die signalr jQuery-Bibliothek, um Nachrichten zu senden und zu empfangen.

### <a name="signalr-hubs-in-the-chathubcs"></a>Signalr Hubs in ChatHub.cs

Im obigen Codebeispiel wird die `ChatHub`-Klasse von der `Microsoft.AspNet.SignalR.Hub`-Klasse abgeleitet. Das Ableiten von der `Hub`-Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung. Sie können öffentliche Methoden für Ihre hubklasse erstellen und dann diese Methoden verwenden, indem Sie Sie aus Skripts auf einer Webseite aufrufen.

Im chatcode wird die `ChatHub.Send`-Methode aufgerufen, um eine neue Nachricht zu senden. Der Hub sendet die Nachricht dann an alle Clients, indem `Clients.All.broadcastMessage`aufgerufen wird.

Die `Send`-Methode veranschaulicht verschiedene Hub-Konzepte:

* Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.

* Verwenden Sie die dynamische `Microsoft.AspNet.SignalR.Hub.Clients`-Eigenschaft, um mit allen Clients zu kommunizieren, die mit diesem Hub verbunden sind.

* Es wird eine Funktion auf dem Client aufgerufen (z. b. die `broadcastMessage` Funktion), um Clients zu aktualisieren.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>Signalr und jQuery in der Datei "index. html"

Die Seite " *Index. html* " im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird. Der Code führt viele wichtige Aufgaben aus. Er deklariert einen Proxy, der auf den Hub verweist, deklariert eine Funktion, die der Server zum Übertragen von Inhalten an Clients senden kann, und startet eine Verbindung, um Nachrichten an den Hub zu senden.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> In JavaScript muss der Verweis auf die Serverklasse und deren Member "CamelCase" lauten. Das Codebeispiel verweist auf C# die *chathub* -Klasse in JavaScript, wie `chatHub`.

In diesem Codeblock erstellen Sie eine Rückruffunktion im Skript.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen. Die beiden Zeilen, in denen der Inhalt vor der Anzeige HTML-codiert wird, sind optional und weisen eine gute Möglichkeit zum Verhindern von Skript Einschleusungen auf.

Dieser Code öffnet eine Verbindung mit dem Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Mit diesem Ansatz wird sichergestellt, dass der Code eine Verbindung herstellt, bevor der Ereignishandler ausgeführt wird.

Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis in der Schaltfläche " **senden** " auf der HTML-Seite zu verarbeiten.

## <a name="get-the-code"></a>Abrufen des Codes

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Weitere Ressourcen

Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:

* [Signalr-Projekt](http://signalr.net)

* [Signalr GitHub und Beispiele](https://github.com/SignalR/SignalR)

* [Signalr-wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial erfahren Sie Folgendes:

> [!div class="checklist"]
> * Einrichten des Projekts
> * Das Beispiel ausgeführt
> * Untersuchen des Codes

Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie signalr und MVC 5 verwenden.
> [!div class="nextstepaction"]
> [Signalr 2 und MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)