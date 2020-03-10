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
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="fa5f6-104">Tutorial: Echt Zeit Chat mit signalr 2 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="fa5f6-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="fa5f6-105">In diesem Tutorial wird gezeigt, wie ASP.net signalr 2 verwendet wird, um eine echt Zeit Chat-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="fa5f6-106">Sie können signalr zu einer MVC 5-Anwendung hinzufügen und eine Chat Ansicht erstellen, um Nachrichten zu senden und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="fa5f6-107">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa5f6-108">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="fa5f6-108">Set up the project</span></span>
> * <span data-ttu-id="fa5f6-109">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="fa5f6-109">Run the sample</span></span>
> * <span data-ttu-id="fa5f6-110">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="fa5f6-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="fa5f6-111">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="fa5f6-111">Prerequisites</span></span>

* <span data-ttu-id="fa5f6-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="fa5f6-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="fa5f6-113">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="fa5f6-113">Set up the Project</span></span>

<span data-ttu-id="fa5f6-114">In diesem Abschnitt wird gezeigt, wie Sie mit Visual Studio 2017 und signalr 2 eine leere ASP.NET MVC 5-Anwendung erstellen, die signalr-Bibliothek hinzufügen und die Chat-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="fa5f6-115">Erstellen Sie in Visual Studio eine C# ASP.NET-Anwendung, die auf .NET Framework 4,5 abzielt, benennen Sie Sie mit signalrchat, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Web erstellen](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="fa5f6-117">Wählen Sie in **New ASP.NET Webanwendung-signalrmvcchat**die Option **MVC** aus, und wählen Sie dann **Authentifizierung ändern**aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="fa5f6-118">Wählen Sie unter **Authentifizierung ändern**die Option **keine Authentifizierung** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Keine Authentifizierung auswählen](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="fa5f6-120">Wählen Sie in **New ASP.NET Webanwendung-signalrmvcchat**die Option **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="fa5f6-121">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="fa5f6-122">Wählen Sie unter **Neues Element hinzufügen-signalrchat**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="fa5f6-123">Benennen Sie die Klasse *chathub* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="fa5f6-124">In diesem Schritt wird die *ChatHub.cs* -Klassendatei erstellt, und dem Projekt wird ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="fa5f6-125">Ersetzen Sie den Code in der neuen *ChatHub.cs* -Klassendatei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="fa5f6-126">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Klasse** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="fa5f6-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="fa5f6-127">Nennen Sie die neue Klasse *Startup* , und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="fa5f6-128">Ersetzen Sie den Code in der *Startup.cs* -Klassendatei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="fa5f6-129">WählenSie in Projektmappen-Explorer **Controller** > **HomeController.cs**aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="fa5f6-130">Fügen Sie der *HomeController.cs*diese Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="fa5f6-131">Diese Methode gibt die **Chat** Ansicht zurück, die Sie in einem späteren Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="fa5f6-132">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf **Ansichten** > **Startseite**, und wählen Sie >  **Ansicht** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="fa5f6-133">Benennen Sie in **Ansicht hinzufügen**den neuen View **Chat** , und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="fa5f6-134">Ersetzen Sie den Inhalt von **Chat. cshtml** durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="fa5f6-135">Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="fa5f6-136">Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fa5f6-137">Der Paket-Manager hat möglicherweise eine neuere Version der signalr-Skripts installiert.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="fa5f6-138">Überprüfen Sie, ob die Skript Verweise im Codeblock den Versionen der Skriptdateien im Projekt entsprechen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="fa5f6-139">Skript Verweise aus dem ursprünglichen Codeblock:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="fa5f6-140">Wenn keine Entsprechung gefunden wird, aktualisieren Sie die *cshtml* -Datei.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="fa5f6-141">Wählen Sie in der Menüleiste **Datei** > **Alle speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="fa5f6-142">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="fa5f6-142">Run the Sample</span></span>

1. <span data-ttu-id="fa5f6-143">Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um das Beispiel im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="fa5f6-145">Wenn der Browser geöffnet wird, geben Sie einen Namen für Ihre Chat Identität ein.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="fa5f6-146">Kopieren Sie die URL aus dem Browser, öffnen Sie zwei andere Browser, und fügen Sie die URLs in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="fa5f6-147">Geben Sie in jedem Browser einen eindeutigen Namen ein.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="fa5f6-148">Fügen Sie nun einen Kommentar hinzu, und wählen Sie **senden**aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="fa5f6-149">Wiederholen Sie diesen Vorgang in den anderen Browsern.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="fa5f6-150">Die Kommentare werden in Echtzeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fa5f6-151">Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="fa5f6-152">Der Hub überträgt Kommentare an alle aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="fa5f6-153">Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="fa5f6-154">Sehen Sie, wie die Chat-Anwendung in drei verschiedenen Browsern ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="fa5f6-155">Wenn Tom, Anand und Susan Nachrichten senden, werden alle Browser in Echtzeit aktualisiert:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Alle drei Browser zeigen denselben Chatverlauf an.](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="fa5f6-157">Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="fa5f6-158">Es gibt eine Skriptdatei mit dem Namen *Hubs* , die die signalr-Bibliothek zur Laufzeit generiert.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="fa5f6-159">Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![automatisch generiertes Hubs-Skript im Knoten "Skript Dokumente"](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="fa5f6-161">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="fa5f6-161">Examine the Code</span></span>

<span data-ttu-id="fa5f6-162">In der signalr Chat-Anwendung werden zwei grundlegende signalr-Entwicklungsaufgaben veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="fa5f6-163">Es zeigt, wie Sie einen Hub erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-163">It shows you how to create a hub.</span></span> <span data-ttu-id="fa5f6-164">Der Server verwendet diesen Hub als hauptkoordinations Objekt.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="fa5f6-165">Der Hub verwendet die signalr jQuery-Bibliothek, um Nachrichten zu senden und zu empfangen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="fa5f6-166">Signalr Hubs in ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="fa5f6-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="fa5f6-167">Im Codebeispiel wird die `ChatHub`-Klasse von der `Microsoft.AspNet.SignalR.Hub`-Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="fa5f6-168">Das Ableiten von der `Hub`-Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="fa5f6-169">Sie können öffentliche Methoden für Ihre Hub-Klasse erstellen und dann auf diese Methoden zugreifen, indem Sie Sie aus Skripts auf einer Webseite aufrufen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="fa5f6-170">Im chatcode wird die `ChatHub.Send`-Methode aufgerufen, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="fa5f6-171">Der Hub sendet die Nachricht seinerseits an alle Clients, indem `Clients.All.addNewMessageToPage`aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="fa5f6-172">Die `Send`-Methode veranschaulicht verschiedene Hub-Konzepte:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="fa5f6-173">Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="fa5f6-174">Verwenden Sie die dynamische `Microsoft.AspNet.SignalR.Hub.Clients`-Eigenschaft, um mit allen Clients zu kommunizieren, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="fa5f6-175">Es wird eine Funktion auf dem Client aufgerufen (z. b. die `addNewMessageToPage` Funktion), um Clients zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="fa5f6-176">Signalr und jQuery Chat. cshtml</span><span class="sxs-lookup"><span data-stu-id="fa5f6-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="fa5f6-177">Die *Chat. cshtml* -Ansichts Datei im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="fa5f6-178">Der Code führt viele wichtige Aufgaben aus.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-178">The code carries out many important tasks.</span></span> <span data-ttu-id="fa5f6-179">Er erstellt einen Verweis auf den automatisch generierten Proxy für den Hub, deklariert eine Funktion, die der Server zum Übertragen von Inhalten an Clients senden kann, und startet eine Verbindung, um Nachrichten an den Hub zu senden.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="fa5f6-180">In JavaScript befindet sich der Verweis auf die Serverklasse und ihre Member in "CamelCase".</span><span class="sxs-lookup"><span data-stu-id="fa5f6-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="fa5f6-181">Das Codebeispiel verweist auf C# die `ChatHub`-Klasse in JavaScript als `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="fa5f6-182">In diesem Codeblock erstellen Sie eine Rückruffunktion im Skript.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="fa5f6-183">Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="fa5f6-184">Der optionale-Befehl für die `htmlEncode`-Funktion zeigt, wie Sie den Nachrichten Inhalt vor der Anzeige auf der Seite in HTML codieren können.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="fa5f6-185">Es ist eine Möglichkeit, Skript Injektion zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="fa5f6-186">Dieser Code öffnet eine Verbindung mit dem Hub.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="fa5f6-187">Mit diesem Ansatz wird sichergestellt, dass Sie eine Verbindung herstellen, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="fa5f6-188">Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis auf der Schaltfläche " **senden** " auf der Chat Seite zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="fa5f6-189">Abrufen des Codes</span><span class="sxs-lookup"><span data-stu-id="fa5f6-189">Get the code</span></span>

[<span data-ttu-id="fa5f6-190">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="fa5f6-190">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="fa5f6-191">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fa5f6-191">Additional resources</span></span>

<span data-ttu-id="fa5f6-192">Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="fa5f6-193">Signalr-Projekt</span><span class="sxs-lookup"><span data-stu-id="fa5f6-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="fa5f6-194">Signalr GitHub und Beispiele</span><span class="sxs-lookup"><span data-stu-id="fa5f6-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="fa5f6-195">Signalr-wiki</span><span class="sxs-lookup"><span data-stu-id="fa5f6-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="fa5f6-196">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="fa5f6-196">Next steps</span></span>

<span data-ttu-id="fa5f6-197">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="fa5f6-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa5f6-198">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="fa5f6-198">Set up the project</span></span>
> * <span data-ttu-id="fa5f6-199">Das Beispiel ausgeführt</span><span class="sxs-lookup"><span data-stu-id="fa5f6-199">Ran the sample</span></span>
> * <span data-ttu-id="fa5f6-200">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="fa5f6-200">Examined the code</span></span>

<span data-ttu-id="fa5f6-201">Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um hoch frequentes Messaging Funktionen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="fa5f6-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="fa5f6-202">Web-App mit hoch frequentem Messaging</span><span class="sxs-lookup"><span data-stu-id="fa5f6-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)