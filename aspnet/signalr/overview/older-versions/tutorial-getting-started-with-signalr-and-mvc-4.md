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
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="a3398-103">Tutorial: Einstieg in signalr 1. x und MVC 4</span><span class="sxs-lookup"><span data-stu-id="a3398-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>

<span data-ttu-id="a3398-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tim teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="a3398-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="a3398-105">In diesem Tutorial wird gezeigt, wie Sie ASP.net signalr verwenden, um eine echt Zeit Chat-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3398-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="a3398-106">Sie werden signalr einer MVC 4-Anwendung hinzufügen und eine Chat Ansicht erstellen, um Nachrichten zu senden und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a3398-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="a3398-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a3398-107">Overview</span></span>

<span data-ttu-id="a3398-108">Dieses Tutorial bietet eine Einführung in die Entwicklung von Webanwendungen in Echtzeit mit ASP.net signalr und ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="a3398-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="a3398-109">Das Tutorial verwendet den gleichen Chat-Anwendungscode wie das Tutorial zu den ersten Schritten mit [signalr](tutorial-getting-started-with-signalr.md), aber zeigt, wie Sie es einer MVC 4-Anwendung auf der Grundlage der Internet Vorlage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a3398-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="a3398-110">In diesem Thema erfahren Sie mehr über die folgenden signalr-Entwicklungsaufgaben:</span><span class="sxs-lookup"><span data-stu-id="a3398-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="a3398-111">Hinzufügen der signalr-Bibliothek zu einer MVC 4-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a3398-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="a3398-112">Erstellen einer Hub-Klasse zum Übertragung von Inhalten an Clients.</span><span class="sxs-lookup"><span data-stu-id="a3398-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="a3398-113">Verwenden der signalr jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Anzeigen von Updates vom Hub.</span><span class="sxs-lookup"><span data-stu-id="a3398-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="a3398-114">Der folgende Screenshot zeigt die abgeschlossene Chat-Anwendung, die in einem Browser ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Chat Instanzen](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="a3398-116">Strecken</span><span class="sxs-lookup"><span data-stu-id="a3398-116">Sections:</span></span>

- [<span data-ttu-id="a3398-117">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="a3398-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="a3398-118">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="a3398-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="a3398-119">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="a3398-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="a3398-120">Next Steps</span><span class="sxs-lookup"><span data-stu-id="a3398-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="a3398-121">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="a3398-121">Set up the Project</span></span>

<span data-ttu-id="a3398-122">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="a3398-122">Prerequisites:</span></span>

- <span data-ttu-id="a3398-123">Visual Studio 2010 SP1, Visual Studio 2012 oder Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="a3398-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="a3398-124">Wenn Sie nicht über Visual Studio verfügen, finden Sie weitere Informationen unter [ASP.net Downloads](https://www.asp.net/downloads) , um das kostenlose Visual Studio 2012 Express-Entwicklungs Tool zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="a3398-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="a3398-125">Installieren Sie [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)für Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a3398-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="a3398-126">In diesem Abschnitt wird gezeigt, wie Sie eine ASP.NET MVC 4-Anwendung erstellen, die signalr-Bibliothek hinzufügen und die Chat-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3398-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="a3398-127">Erstellen Sie in Visual Studio eine ASP.NET MVC 4-Anwendung, benennen Sie Sie signalrchat, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="a3398-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="a3398-128">Wählen Sie in VS 2010 im Dropdown-Steuerelement Framework-Version die Option **.NET Framework 4** aus.</span><span class="sxs-lookup"><span data-stu-id="a3398-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="a3398-129">Signalr-Code wird auf .NET Framework-Versionen 4 und 4,5 ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a3398-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![MVC-Web erstellen](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="a3398-131">Wählen Sie die Vorlage Internet Anwendung aus, deaktivieren Sie die Option zum **Erstellen eines Komponenten Testprojekts**, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="a3398-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Erstellen einer MVC-Internet Site](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="a3398-133">Öffnen Sie die **Tools > nuget-Paket-Manager > Paket-Manager-Konsole** und führen Sie den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="a3398-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="a3398-134">In diesem Schritt wird dem Projekt ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die die signalr-Funktionalität ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="a3398-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="a3398-135">Erweitern Sie in **Projektmappen-Explorer** den Ordner Skripts.</span><span class="sxs-lookup"><span data-stu-id="a3398-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="a3398-136">Beachten Sie, dass dem Projekt Skript Bibliotheken für signalr hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="a3398-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Bibliotheks Verweise](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="a3398-138">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner mit dem Namen **Hubs**hinzu.</span><span class="sxs-lookup"><span data-stu-id="a3398-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="a3398-139">Klicken Sie mit der rechten Maustaste auf den Ordner **Hubs** und dann auf **Hinzufügen -Klasse**, und erstellen Sie C# eine neue Klasse mit dem Namen **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="a3398-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="a3398-140">Sie verwenden diese Klasse als signalr-Server-Hub, der Nachrichten an alle Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="a3398-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="a3398-141">Wenn Sie Visual Studio 2012 verwenden und das [ASP.net and Web Tools 2012,2-Update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)installiert haben, können Sie die neue signalr-Element Vorlage verwenden, um die Hub-Klasse zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3398-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="a3398-142">Klicken Sie hierzu mit der rechten Maustaste auf den Ordner **Hubs** , und klicken Sie auf **hinzufügen. Neues Element**, wählen Sie **signalr-Hub-Klasse (v1)** aus, und benennen Sie die Klasse **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="a3398-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>

1. <span data-ttu-id="a3398-143">Ersetzen Sie den Code in der **chathub** -Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="a3398-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="a3398-144">Öffnen Sie die Datei " **Global. asax** " für das Projekt, und fügen Sie der-Methode einen aufzurufenden `RouteTable.Routes.MapHubs();` als erste Codezeile in der `Application_Start`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="a3398-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="a3398-145">Mit diesem Code wird die Standardroute für signalr Hubs registriert und muss aufgerufen werden, bevor Sie andere Routen registrieren.</span><span class="sxs-lookup"><span data-stu-id="a3398-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="a3398-146">Die abgeschlossene `Application_Start`-Methode sieht wie im folgenden Beispiel aus.</span><span class="sxs-lookup"><span data-stu-id="a3398-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="a3398-147">Bearbeiten Sie die `HomeController`-Klasse, die in **Controllers/HomeController. cs** gefunden wurde, und fügen Sie der-Klasse die folgende Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="a3398-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="a3398-148">Diese Methode gibt die **Chat** Ansicht zurück, die Sie in einem späteren Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3398-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="a3398-149">Klicken Sie mit der rechten Maustaste in die soeben erstellte `Chat` Methode, und klicken Sie auf **Ansicht hinzufügen** , um eine neue Ansichts Datei zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a3398-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="a3398-150">Vergewissern Sie sich, dass im Dialogfeld **Ansicht hinzufügen** das Kontrollkästchen für die **Verwendung eines Layouts oder einer Master Seite** aktiviert ist (deaktivieren Sie die anderen Kontrollkästchen), und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a3398-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="a3398-152">Bearbeiten Sie die neue Ansichts Datei mit dem Namen " **Chat. cshtml**".</span><span class="sxs-lookup"><span data-stu-id="a3398-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="a3398-153">Fügen Sie nach dem Tag &lt;H2&gt; den folgenden &lt;div&gt; Abschnitt und `@section scripts` Codeblock in die Seite ein.</span><span class="sxs-lookup"><span data-stu-id="a3398-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="a3398-154">Dieses Skript ermöglicht es der Seite, Chat Nachrichten zu senden und Nachrichten vom Server anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a3398-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="a3398-155">Der gesamte Code für die Chat Ansicht wird im folgenden Codeblock angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3398-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a3398-156">Wenn Sie signalr und andere Skript Bibliotheken zu Ihrem Visual Studio-Projekt hinzufügen, installiert der Paket-Manager möglicherweise Versionen der Skripts, die aktueller als die in diesem Thema gezeigten Versionen sind.</span><span class="sxs-lookup"><span data-stu-id="a3398-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="a3398-157">Stellen Sie sicher, dass die Skript Verweise im Code den Versionen der Skript Bibliotheken entsprechen, die in Ihrem Projekt installiert sind.</span><span class="sxs-lookup"><span data-stu-id="a3398-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="a3398-158">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="a3398-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="a3398-159">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="a3398-159">Run the Sample</span></span>

1. <span data-ttu-id="a3398-160">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a3398-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="a3398-161">Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt an.</span><span class="sxs-lookup"><span data-stu-id="a3398-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="a3398-162">Die Chat Seite wird in einer Browser Instanz geladen, und Sie werden zur Eingabe eines Benutzernamens aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="a3398-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="a3398-164">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="a3398-164">Enter a user name.</span></span>
4. <span data-ttu-id="a3398-165">Kopieren Sie die URL aus der Adresszeile des Browsers, und verwenden Sie Sie, um zwei weitere Browser Instanzen zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="a3398-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="a3398-166">Geben Sie in jeder Browser Instanz einen eindeutigen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="a3398-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="a3398-167">Fügen Sie in jeder Browser Instanz einen Kommentar hinzu, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="a3398-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="a3398-168">Die Kommentare sollten in allen Browser Instanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a3398-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a3398-169">Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei.</span><span class="sxs-lookup"><span data-stu-id="a3398-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="a3398-170">Der Hub überträgt Kommentare an alle aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a3398-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="a3398-171">Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="a3398-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="a3398-172">Der folgende Screenshot zeigt die Chat-Anwendung, die in einem Browser ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="a3398-174">Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="a3398-175">Dieser Knoten ist im Debugmodus sichtbar, wenn Sie Internet Explorer als Browser verwenden.</span><span class="sxs-lookup"><span data-stu-id="a3398-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="a3398-176">Es gibt eine Skriptdatei mit dem Namen **Hubs** , die von der signalr-Bibliothek zur Laufzeit dynamisch generiert wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="a3398-177">Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="a3398-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="a3398-178">Wenn Sie einen anderen Browser als Internet Explorer verwenden, können Sie auch auf die Datei "Dynamic **Hubs** " zugreifen, indem Sie direkt zu dieser Datei navigieren, z. b. http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="a3398-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Generiertes Hub-Skript](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="a3398-180">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="a3398-180">Examine the Code</span></span>

<span data-ttu-id="a3398-181">Die signalr Chat-Anwendung veranschaulicht zwei grundlegende signalr-Entwicklungsaufgaben: das Erstellen eines Hubs als hauptkoordinations Objekt auf dem Server und das Verwenden der signalr jQuery-Bibliothek zum Senden und empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="a3398-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="a3398-182">SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a3398-182">SignalR Hubs</span></span>

<span data-ttu-id="a3398-183">Im Codebeispiel wird die **chathub** -Klasse von der **Microsoft. Aspnet. signalr. Hub** -Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="a3398-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="a3398-184">Die Ableitung von der **Hub** -Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a3398-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="a3398-185">Sie können öffentliche Methoden für Ihre hubklasse erstellen und dann auf diese Methoden zugreifen, indem Sie Sie von jQuery-Skripts auf einer Webseite aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a3398-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="a3398-186">Im chatcode wird die **chathub. Send** -Methode von Clients aufgerufen, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="a3398-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="a3398-187">Der Hub sendet die Nachricht seinerseits an alle Clients, indem er " **Clients. all. addnewmessagetopage**" aufruft.</span><span class="sxs-lookup"><span data-stu-id="a3398-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="a3398-188">Die **Send** -Methode veranschaulicht verschiedene Hub-Konzepte:</span><span class="sxs-lookup"><span data-stu-id="a3398-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="a3398-189">Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="a3398-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="a3398-190">Verwenden Sie die Eigenschaft **Microsoft. Aspnet. signalr. Hub. Clients** , um auf alle Clients zuzugreifen, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="a3398-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="a3398-191">Zum Aktualisieren von Clients wird eine jQuery-Funktion auf dem Client (z. b. die `addNewMessageToPage`-Funktion) aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a3398-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="a3398-192">Signalr und jQuery</span><span class="sxs-lookup"><span data-stu-id="a3398-192">SignalR and jQuery</span></span>

<span data-ttu-id="a3398-193">Die **Chat. cshtml** -Ansichts Datei im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="a3398-194">Die wesentlichen Aufgaben im Code sind das Erstellen eines Verweises auf den automatisch generierten Proxy für den Hub, das Deklarieren einer Funktion, die der Server zum Übertragen von Inhalten an Clients und das Starten einer Verbindung zum Senden von Nachrichten an den Hub aufruft.</span><span class="sxs-lookup"><span data-stu-id="a3398-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="a3398-195">Der folgende Code deklariert einen Proxy für einen Hub.</span><span class="sxs-lookup"><span data-stu-id="a3398-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="a3398-196">In jQuery wird der Verweis auf die Serverklasse und ihre Member in der Kamel-Schreibweise angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3398-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="a3398-197">Das Codebeispiel verweist auf C# die **chathub** -Klasse in jQuery als **chathub**.</span><span class="sxs-lookup"><span data-stu-id="a3398-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="a3398-198">Wenn Sie in jQuery mit herkömmlicher Pascal-Schreibweise wie in C#auf die `ChatHub`-Klasse verweisen möchten, bearbeiten Sie die ChatHub.cs-Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="a3398-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="a3398-199">Fügen Sie eine `using` Anweisung hinzu, um auf den `Microsoft.AspNet.SignalR.Hubs` Namespace zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="a3398-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="a3398-200">Fügen Sie dann der `ChatHub`-Klasse das `HubName`-Attribut hinzu, z. b. `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="a3398-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="a3398-201">Aktualisieren Sie abschließend den jQuery-Verweis auf die `ChatHub`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="a3398-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>

<span data-ttu-id="a3398-202">Der folgende Code zeigt, wie eine Rückruffunktion im Skript erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="a3398-203">Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="a3398-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="a3398-204">Der optionale-Befehl für die `htmlEncode`-Funktion zeigt, wie Sie den Nachrichten Inhalt vor der Anzeige auf der Seite in HTML codieren, um die Skript Injektion zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="a3398-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="a3398-205">Der folgende Code zeigt, wie eine Verbindung mit dem Hub geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="a3398-206">Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis auf der Schaltfläche " **senden** " auf der Chat Seite zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="a3398-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="a3398-207">Mit diesem Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a3398-207">This approach ensures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="a3398-208">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a3398-208">Next Steps</span></span>

<span data-ttu-id="a3398-209">Sie haben gelernt, dass signalr ein Framework zum Entwickeln von Echt Zeit Webanwendungen ist.</span><span class="sxs-lookup"><span data-stu-id="a3398-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="a3398-210">Außerdem haben Sie verschiedene signalr-Entwicklungsaufgaben kennengelernt: Hinzufügen von signalr zu einer ASP.NET-Anwendung, Erstellen einer Hub-Klasse und senden und empfangen von Nachrichten vom Hub.</span><span class="sxs-lookup"><span data-stu-id="a3398-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="a3398-211">Weitere Informationen zu den erweiterten Konzepten der signalr-Entwicklung finden Sie auf den folgenden Websites für signalr-Quellcode und-Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="a3398-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="a3398-212">Signalr-Projekt</span><span class="sxs-lookup"><span data-stu-id="a3398-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="a3398-213">Signalr GitHub und Beispiele</span><span class="sxs-lookup"><span data-stu-id="a3398-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="a3398-214">Signalr-wiki</span><span class="sxs-lookup"><span data-stu-id="a3398-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
