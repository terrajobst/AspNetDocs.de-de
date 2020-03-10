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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431565"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="c2d67-104">Tutorial: Echt Zeit Chat mit signalr 2</span><span class="sxs-lookup"><span data-stu-id="c2d67-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="c2d67-105">In diesem Tutorial wird gezeigt, wie Sie signalr verwenden, um eine echt Zeit Chat-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="c2d67-106">Sie fügen signalr einer leeren ASP.NET-Webanwendung hinzu und erstellen eine HTML-Seite, um Nachrichten zu senden und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="c2d67-107">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="c2d67-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c2d67-108">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="c2d67-108">Set up the project</span></span>
> * <span data-ttu-id="c2d67-109">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="c2d67-109">Run the sample</span></span>
> * <span data-ttu-id="c2d67-110">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="c2d67-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="c2d67-111">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="c2d67-111">Prerequisites</span></span>

* <span data-ttu-id="c2d67-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="c2d67-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="c2d67-113">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="c2d67-113">Set up the Project</span></span>

<span data-ttu-id="c2d67-114">In diesem Abschnitt wird gezeigt, wie Sie mit Visual Studio 2017 und signalr 2 eine leere ASP.NET-Webanwendung erstellen, signalr hinzufügen und die Chat-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="c2d67-115">Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="c2d67-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web erstellen](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="c2d67-117">Lassen Sie im Fenster **New ASP.net Project-signalrchat** den Eintrag **leer** , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="c2d67-118">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="c2d67-119">Wählen Sie unter **Neues Element hinzufügen-signalrchat**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="c2d67-120">Benennen Sie die Klasse *chathub* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="c2d67-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="c2d67-121">In diesem Schritt wird die *ChatHub.cs* -Klassendatei erstellt, und dem Projekt wird ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="c2d67-122">Ersetzen Sie den Code in der neuen *ChatHub.cs* -Klassendatei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c2d67-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="c2d67-123">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="c2d67-124">Wählen Sie in **Neues Element hinzufügen-signalrchat** die Option **installiert** > **Visual C#**  > **Web** aus, und wählen Sie dann **owin-Startklasse**</span><span class="sxs-lookup"><span data-stu-id="c2d67-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="c2d67-125">Benennen Sie die Klasse *Startup* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="c2d67-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="c2d67-126">Ersetzen Sie den Standard Code in der *Startup* -Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c2d67-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="c2d67-127">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **HTML-Seite** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="c2d67-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="c2d67-128">Benennen Sie den neuen Seiten *Index* , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="c2d67-129">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die erstellte HTML-Seite, und wählen Sie **als Start Seite festlegen**aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="c2d67-130">Ersetzen Sie den Standard Code in der HTML-Seite durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c2d67-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="c2d67-131">Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.</span><span class="sxs-lookup"><span data-stu-id="c2d67-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="c2d67-132">Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.</span><span class="sxs-lookup"><span data-stu-id="c2d67-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c2d67-133">Der Paket-Manager hat möglicherweise eine neuere Version der signalr-Skripts installiert.</span><span class="sxs-lookup"><span data-stu-id="c2d67-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="c2d67-134">Überprüfen Sie, ob die Skript Verweise im Codeblock den Versionen der Skriptdateien im Projekt entsprechen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="c2d67-135">Skript Verweise aus dem ursprünglichen Codeblock:</span><span class="sxs-lookup"><span data-stu-id="c2d67-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="c2d67-136">Wenn keine Entsprechung gefunden wird, aktualisieren Sie die *HTML* -Datei.</span><span class="sxs-lookup"><span data-stu-id="c2d67-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="c2d67-137">Wählen Sie in der Menüleiste **Datei** > **Alle speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="c2d67-138">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="c2d67-138">Run the Sample</span></span>

1. <span data-ttu-id="c2d67-139">Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um das Beispiel im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="c2d67-141">Wenn der Browser geöffnet wird, geben Sie einen Namen für Ihre Chat Identität ein.</span><span class="sxs-lookup"><span data-stu-id="c2d67-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="c2d67-142">Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="c2d67-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="c2d67-143">Geben Sie in jedem Browser einen eindeutigen Namen ein.</span><span class="sxs-lookup"><span data-stu-id="c2d67-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="c2d67-144">Fügen Sie nun einen Kommentar hinzu, und wählen Sie **senden**aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="c2d67-145">Wiederholen Sie diesen Vorgang in den anderen Browsern.</span><span class="sxs-lookup"><span data-stu-id="c2d67-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="c2d67-146">Die Kommentare werden in Echtzeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c2d67-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d67-147">Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei.</span><span class="sxs-lookup"><span data-stu-id="c2d67-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="c2d67-148">Der Hub überträgt Kommentare an alle aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="c2d67-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="c2d67-149">Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="c2d67-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="c2d67-150">Sehen Sie, wie die Chat-Anwendung in drei verschiedenen Browsern ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c2d67-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="c2d67-151">Wenn Tom, Anand und Susan Nachrichten senden, werden alle Browser in Echtzeit aktualisiert:</span><span class="sxs-lookup"><span data-stu-id="c2d67-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Alle drei Browser zeigen denselben Chatverlauf an.](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="c2d67-153">Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c2d67-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="c2d67-154">Es gibt eine Skriptdatei mit dem Namen *Hubs* , die die signalr-Bibliothek zur Laufzeit generiert.</span><span class="sxs-lookup"><span data-stu-id="c2d67-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="c2d67-155">Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="c2d67-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![automatisch generiertes Hubs-Skript im Knoten "Skript Dokumente"](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="c2d67-157">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="c2d67-157">Examine the Code</span></span>

<span data-ttu-id="c2d67-158">In der signalrchat-Anwendung werden zwei grundlegende signalr-Entwicklungsaufgaben veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="c2d67-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="c2d67-159">Es zeigt, wie Sie einen Hub erstellen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-159">It shows you how to create a hub.</span></span> <span data-ttu-id="c2d67-160">Der Server verwendet diesen Hub als hauptkoordinations Objekt.</span><span class="sxs-lookup"><span data-stu-id="c2d67-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="c2d67-161">Der Hub verwendet die signalr jQuery-Bibliothek, um Nachrichten zu senden und zu empfangen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="c2d67-162">Signalr Hubs in ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="c2d67-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="c2d67-163">Im obigen Codebeispiel wird die `ChatHub`-Klasse von der `Microsoft.AspNet.SignalR.Hub`-Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="c2d67-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="c2d67-164">Das Ableiten von der `Hub`-Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c2d67-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="c2d67-165">Sie können öffentliche Methoden für Ihre hubklasse erstellen und dann diese Methoden verwenden, indem Sie Sie aus Skripts auf einer Webseite aufrufen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="c2d67-166">Im chatcode wird die `ChatHub.Send`-Methode aufgerufen, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="c2d67-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="c2d67-167">Der Hub sendet die Nachricht dann an alle Clients, indem `Clients.All.broadcastMessage`aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c2d67-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="c2d67-168">Die `Send`-Methode veranschaulicht verschiedene Hub-Konzepte:</span><span class="sxs-lookup"><span data-stu-id="c2d67-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="c2d67-169">Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="c2d67-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="c2d67-170">Verwenden Sie die dynamische `Microsoft.AspNet.SignalR.Hub.Clients`-Eigenschaft, um mit allen Clients zu kommunizieren, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="c2d67-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="c2d67-171">Es wird eine Funktion auf dem Client aufgerufen (z. b. die `broadcastMessage` Funktion), um Clients zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c2d67-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="c2d67-172">Signalr und jQuery in der Datei "index. html"</span><span class="sxs-lookup"><span data-stu-id="c2d67-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="c2d67-173">Die Seite " *Index. html* " im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c2d67-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="c2d67-174">Der Code führt viele wichtige Aufgaben aus.</span><span class="sxs-lookup"><span data-stu-id="c2d67-174">The code carries out many important tasks.</span></span> <span data-ttu-id="c2d67-175">Er deklariert einen Proxy, der auf den Hub verweist, deklariert eine Funktion, die der Server zum Übertragen von Inhalten an Clients senden kann, und startet eine Verbindung, um Nachrichten an den Hub zu senden.</span><span class="sxs-lookup"><span data-stu-id="c2d67-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="c2d67-176">In JavaScript muss der Verweis auf die Serverklasse und deren Member "CamelCase" lauten.</span><span class="sxs-lookup"><span data-stu-id="c2d67-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="c2d67-177">Das Codebeispiel verweist auf C# die *chathub* -Klasse in JavaScript, wie `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="c2d67-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="c2d67-178">In diesem Codeblock erstellen Sie eine Rückruffunktion im Skript.</span><span class="sxs-lookup"><span data-stu-id="c2d67-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="c2d67-179">Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="c2d67-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="c2d67-180">Die beiden Zeilen, in denen der Inhalt vor der Anzeige HTML-codiert wird, sind optional und weisen eine gute Möglichkeit zum Verhindern von Skript Einschleusungen auf.</span><span class="sxs-lookup"><span data-stu-id="c2d67-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="c2d67-181">Dieser Code öffnet eine Verbindung mit dem Hub.</span><span class="sxs-lookup"><span data-stu-id="c2d67-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="c2d67-182">Mit diesem Ansatz wird sichergestellt, dass der Code eine Verbindung herstellt, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c2d67-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="c2d67-183">Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis in der Schaltfläche " **senden** " auf der HTML-Seite zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="c2d67-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="c2d67-184">Abrufen des Codes</span><span class="sxs-lookup"><span data-stu-id="c2d67-184">Get the code</span></span>

[<span data-ttu-id="c2d67-185">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="c2d67-185">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="c2d67-186">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c2d67-186">Additional resources</span></span>

<span data-ttu-id="c2d67-187">Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="c2d67-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="c2d67-188">Signalr-Projekt</span><span class="sxs-lookup"><span data-stu-id="c2d67-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="c2d67-189">Signalr GitHub und Beispiele</span><span class="sxs-lookup"><span data-stu-id="c2d67-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="c2d67-190">Signalr-wiki</span><span class="sxs-lookup"><span data-stu-id="c2d67-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="c2d67-191">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="c2d67-191">Next steps</span></span>

<span data-ttu-id="c2d67-192">In diesem Tutorial erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="c2d67-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c2d67-193">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="c2d67-193">Set up the project</span></span>
> * <span data-ttu-id="c2d67-194">Das Beispiel ausgeführt</span><span class="sxs-lookup"><span data-stu-id="c2d67-194">Ran the sample</span></span>
> * <span data-ttu-id="c2d67-195">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="c2d67-195">Examined the code</span></span>

<span data-ttu-id="c2d67-196">Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie signalr und MVC 5 verwenden.</span><span class="sxs-lookup"><span data-stu-id="c2d67-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c2d67-197">Signalr 2 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="c2d67-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)