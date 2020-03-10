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
# <a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="b75f6-103">Tutorial: Einstieg in signalr 1. x</span><span class="sxs-lookup"><span data-stu-id="b75f6-103">Tutorial: Getting Started with SignalR 1.x</span></span>

<span data-ttu-id="b75f6-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tim teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="b75f6-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b75f6-105">Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="b75f6-106">Sie fügen signalr einer leeren ASP.NET-Webanwendung hinzu und erstellen eine HTML-Seite, um Nachrichten zu senden und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="b75f6-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b75f6-107">Overview</span></span>

<span data-ttu-id="b75f6-108">In diesem Tutorial wird die signalr-Entwicklung eingeführt, indem gezeigt wird, wie eine einfache browserbasierte Chatanwendung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="b75f6-109">Fügen Sie die signalr-Bibliothek einer leeren ASP.NET-Webanwendung hinzu, erstellen Sie eine Hub-Klasse zum Senden von Nachrichten an Clients, und erstellen Sie eine HTML-Seite, mit der Benutzer Chat Nachrichten senden und empfangen können.</span><span class="sxs-lookup"><span data-stu-id="b75f6-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="b75f6-110">Ein ähnliches Tutorial, das zeigt, wie eine Chat-Anwendung in MVC 4 mithilfe einer MVC-Ansicht erstellt wird, finden Sie unter [Getting Started with signalr and MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="b75f6-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b75f6-111">In diesem Tutorial wird die Releaseversion (1. x) von signalr verwendet.</span><span class="sxs-lookup"><span data-stu-id="b75f6-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="b75f6-112">Ausführliche Informationen zu Änderungen zwischen signalr 1. x und 2,0 finden Sie unter [Aktualisieren von signalr 1. x-Projekten](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="b75f6-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="b75f6-113">Signalr ist eine Open-Source-.NET-Bibliothek zum Entwickeln von Webanwendungen, die eine Live-Benutzerinteraktion oder Echtzeitdaten Aktualisierungen erfordern.</span><span class="sxs-lookup"><span data-stu-id="b75f6-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="b75f6-114">Beispiele hierfür sind soziale Anwendungen, Spiele mit mehreren Benutzern, geschäftliche Zusammenarbeit und Nachrichten-, Wetter-oder Finanz Update Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="b75f6-115">Diese werden häufig als Echtzeitanwendungen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b75f6-115">These are often called real-time applications.</span></span>

<span data-ttu-id="b75f6-116">Signalr vereinfacht das Entwickeln von Echtzeitanwendungen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="b75f6-117">Es umfasst eine ASP.NET-Server Bibliothek und eine JavaScript-Client Bibliothek, um die Verwaltung von Client-/Server-Verbindungen und das Übertragung von Inhalts Aktualisierungen an Clients zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="b75f6-118">Sie können die signalr-Bibliothek einer vorhandenen ASP.NET-Anwendung hinzufügen, um Echtzeitfunktionen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b75f6-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="b75f6-119">In diesem Tutorial werden die folgenden signalr-Entwicklungsaufgaben veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="b75f6-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="b75f6-120">Hinzufügen der signalr-Bibliothek zu einer ASP.NET-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="b75f6-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="b75f6-121">Erstellen einer Hub-Klasse zum Übertragung von Inhalten an Clients.</span><span class="sxs-lookup"><span data-stu-id="b75f6-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="b75f6-122">Verwenden der signalr jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Anzeigen von Updates vom Hub.</span><span class="sxs-lookup"><span data-stu-id="b75f6-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="b75f6-123">Der folgende Screenshot zeigt die Chat-Anwendung, die in einem Browser ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="b75f6-124">Jeder neue Benutzer kann Kommentare Posten und Kommentare anzeigen, die nach dem Join des Benutzers hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="b75f6-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Chat Instanzen](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="b75f6-126">Strecken</span><span class="sxs-lookup"><span data-stu-id="b75f6-126">Sections:</span></span>

- [<span data-ttu-id="b75f6-127">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="b75f6-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="b75f6-128">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="b75f6-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="b75f6-129">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="b75f6-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="b75f6-130">Next Steps</span><span class="sxs-lookup"><span data-stu-id="b75f6-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="b75f6-131">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="b75f6-131">Set up the Project</span></span>

<span data-ttu-id="b75f6-132">In diesem Abschnitt wird gezeigt, wie Sie eine leere ASP.NET-Webanwendung erstellen, signalr hinzufügen und die Chat-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="b75f6-133">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="b75f6-133">Prerequisites:</span></span>

- <span data-ttu-id="b75f6-134">Visual Studio 2010 SP1 oder 2012.</span><span class="sxs-lookup"><span data-stu-id="b75f6-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="b75f6-135">Wenn Sie nicht über Visual Studio verfügen, finden Sie weitere Informationen unter [ASP.net Downloads](https://www.asp.net/downloads) , um das kostenlose Visual Studio 2012 Express-Entwicklungs Tool zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b75f6-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="b75f6-136">[Microsoft ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="b75f6-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="b75f6-137">Für Visual Studio 2012 fügt dieses Installationsprogramm neue ASP.NET-Features einschließlich signalr-Vorlagen zu Visual Studio hinzu.</span><span class="sxs-lookup"><span data-stu-id="b75f6-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="b75f6-138">Für Visual Studio 2010 SP1 ist ein Installer nicht verfügbar. Sie können das Tutorial aber durch Installieren des signalr-nuget-Pakets durchführen, wie in den Setup Schritten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="b75f6-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="b75f6-139">In den folgenden Schritten wird Visual Studio 2012 verwendet, um eine leere ASP.NET-Webanwendung zu erstellen und die signalr-Bibliothek hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="b75f6-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="b75f6-140">Erstellen Sie in Visual Studio eine ASP.net leere Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="b75f6-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Leeres Web erstellen](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="b75f6-142">Öffnen Sie die **Paket-Manager-Konsole** , indem Sie auf Extras **| Nuget-Paket-Manager | Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="b75f6-143">Geben Sie im Konsolenfenster den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="b75f6-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="b75f6-144">Mit diesem Befehl wird die aktuellste Version von signalr 1. x installiert.</span><span class="sxs-lookup"><span data-stu-id="b75f6-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="b75f6-145">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen | -Klasse**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="b75f6-146">Nennen Sie die neue Klasse **chathub**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="b75f6-147">Erweitern Sie in **Projektmappen-Explorer** den Knoten Skripts.</span><span class="sxs-lookup"><span data-stu-id="b75f6-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="b75f6-148">Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.</span><span class="sxs-lookup"><span data-stu-id="b75f6-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Bibliotheks Verweise](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="b75f6-150">Ersetzen Sie den Code in der **chathub** -Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b75f6-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="b75f6-151">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen. Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="b75f6-152">Wählen Sie im Dialogfeld **Neues Element hinzufügen** **globale Anwendungsklasse** aus, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Global hinzufügen](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="b75f6-154">Fügen Sie nach den bereitgestellten `using` Anweisungen in der Global.asax.cs-Klasse die folgenden `using`-Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="b75f6-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="b75f6-155">Fügen Sie in der `Application_Start`-Methode der Global-Klasse die folgende Codezeile hinzu, um die Standardroute für signalr-Hubs zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="b75f6-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="b75f6-156">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen. Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="b75f6-157">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option HTML, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="b75f6-158">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die soeben erstellte HTML-Seite, und klicken Sie dann auf **als Start Seite festlegen**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="b75f6-159">Ersetzen Sie den Standard Code in der HTML-Seite durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b75f6-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="b75f6-160">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="b75f6-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="b75f6-161">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="b75f6-161">Run the Sample</span></span>

1. <span data-ttu-id="b75f6-162">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="b75f6-163">Die HTML-Seite wird in einer Browser Instanz geladen, und Sie werden zur Eingabe eines Benutzernamens aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="b75f6-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="b75f6-165">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="b75f6-165">Enter a user name.</span></span>
3. <span data-ttu-id="b75f6-166">Kopieren Sie die URL aus der Adresszeile des Browsers, und verwenden Sie Sie, um zwei weitere Browser Instanzen zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="b75f6-167">Geben Sie in jeder Browser Instanz einen eindeutigen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="b75f6-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="b75f6-168">Fügen Sie in jeder Browser Instanz einen Kommentar hinzu, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="b75f6-169">Die Kommentare sollten in allen Browser Instanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b75f6-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b75f6-170">Diese einfache Chat-Anwendung behält den Diskussions Kontext auf dem Server nicht bei.</span><span class="sxs-lookup"><span data-stu-id="b75f6-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="b75f6-171">Der Hub überträgt Kommentare an alle aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="b75f6-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="b75f6-172">Benutzer, die später dem Chat beitreten, sehen die Nachrichten, die ab dem Zeitpunkt der Verknüpfung hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b75f6-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="b75f6-173">Der folgende Screenshot zeigt die Chat-Anwendung, die in drei Browser Instanzen ausgeführt wird. Diese werden aktualisiert, wenn eine Instanz eine Nachricht sendet:</span><span class="sxs-lookup"><span data-stu-id="b75f6-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="b75f6-175">Überprüfen Sie in **Projektmappen-Explorer**den Knoten **Skript Dokumente** für die Anwendung, die ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="b75f6-176">Es gibt eine Skriptdatei mit dem Namen **Hubs** , die von der signalr-Bibliothek zur Laufzeit dynamisch generiert wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="b75f6-177">Diese Datei verwaltet die Kommunikation zwischen dem jQuery-Skript und dem serverseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="b75f6-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Generiertes Hub-Skript](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="b75f6-179">Untersuchen des Codes</span><span class="sxs-lookup"><span data-stu-id="b75f6-179">Examine the Code</span></span>

<span data-ttu-id="b75f6-180">Die signalr Chat-Anwendung veranschaulicht zwei grundlegende signalr-Entwicklungsaufgaben: das Erstellen eines Hubs als hauptkoordinations Objekt auf dem Server und das Verwenden der signalr jQuery-Bibliothek zum Senden und empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="b75f6-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="b75f6-181">SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="b75f6-181">SignalR Hubs</span></span>

<span data-ttu-id="b75f6-182">Im Codebeispiel wird die **chathub** -Klasse von der **Microsoft. Aspnet. signalr. Hub** -Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="b75f6-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="b75f6-183">Die Ableitung von der **Hub** -Klasse ist eine nützliche Möglichkeit zum Erstellen einer signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b75f6-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="b75f6-184">Sie können öffentliche Methoden für Ihre hubklasse erstellen und dann auf diese Methoden zugreifen, indem Sie Sie von jQuery-Skripts auf einer Webseite aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="b75f6-185">Im chatcode wird die **chathub. Send** -Methode von Clients aufgerufen, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="b75f6-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="b75f6-186">Der Hub sendet die Nachricht seinerseits an alle Clients, indem er " **Clients. all. broadcastmessage**" aufruft.</span><span class="sxs-lookup"><span data-stu-id="b75f6-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="b75f6-187">Die **Send** -Methode veranschaulicht verschiedene Hub-Konzepte:</span><span class="sxs-lookup"><span data-stu-id="b75f6-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="b75f6-188">Deklarieren Sie öffentliche Methoden auf einem Hub, damit Sie von Clients aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="b75f6-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="b75f6-189">Verwenden Sie die dynamische Eigenschaft **Microsoft. Aspnet. signalr. Hub. Clients** für den Zugriff auf alle Clients, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="b75f6-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="b75f6-190">Zum Aktualisieren von Clients wird eine jQuery-Funktion auf dem Client (z. b. die `broadcastMessage`-Funktion) aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="b75f6-191">Signalr und jQuery</span><span class="sxs-lookup"><span data-stu-id="b75f6-191">SignalR and jQuery</span></span>

<span data-ttu-id="b75f6-192">Die HTML-Seite im Codebeispiel zeigt, wie die signalr jQuery-Bibliothek für die Kommunikation mit einem signalr-Hub verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="b75f6-193">Die wesentlichen Aufgaben im Code sind das Deklarieren eines Proxys für den Verweis auf den Hub und das Deklarieren einer Funktion, die der Server zum Übertragen von Inhalten an Clients und zum Starten einer Verbindung zum Senden von Nachrichten an den Hub aufruft.</span><span class="sxs-lookup"><span data-stu-id="b75f6-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="b75f6-194">Der folgende Code deklariert einen Proxy für einen Hub.</span><span class="sxs-lookup"><span data-stu-id="b75f6-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="b75f6-195">In jQuery wird der Verweis auf die Serverklasse und ihre Member in der Kamel-Schreibweise angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b75f6-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="b75f6-196">Das Codebeispiel verweist auf C# die **chathub** -Klasse in jQuery als **chathub**.</span><span class="sxs-lookup"><span data-stu-id="b75f6-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>

<span data-ttu-id="b75f6-197">Im folgenden Code wird erläutert, wie Sie eine Rückruffunktion im Skript erstellen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="b75f6-198">Die Hub-Klasse auf dem Server ruft diese Funktion auf, um Inhalts Aktualisierungen an jeden Client zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="b75f6-199">Die zwei Zeilen, in denen der Inhalt vor dem Anzeigen von HTML codiert wird, sind optional und stellen eine einfache Möglichkeit dar, Skript Injektion zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="b75f6-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="b75f6-200">Der folgende Code zeigt, wie eine Verbindung mit dem Hub geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="b75f6-201">Der Code startet die Verbindung und übergibt dann eine Funktion, um das Click-Ereignis in der Schaltfläche " **senden** " auf der HTML-Seite zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="b75f6-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="b75f6-202">Dieser Ansatz stellt sicher, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b75f6-202">This approach insures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="b75f6-203">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b75f6-203">Next Steps</span></span>

<span data-ttu-id="b75f6-204">Sie haben gelernt, dass signalr ein Framework zum Entwickeln von Echt Zeit Webanwendungen ist.</span><span class="sxs-lookup"><span data-stu-id="b75f6-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="b75f6-205">Außerdem haben Sie verschiedene signalr-Entwicklungsaufgaben kennengelernt: Hinzufügen von signalr zu einer ASP.NET-Anwendung, Erstellen einer Hub-Klasse und senden und empfangen von Nachrichten vom Hub.</span><span class="sxs-lookup"><span data-stu-id="b75f6-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="b75f6-206">Sie können die Beispielanwendung in diesem Tutorial oder in anderen signalr-Anwendungen über das Internet zur Verfügung stellen, indem Sie Sie für einen Hostinganbieter bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b75f6-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="b75f6-207">Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem kostenlosen [Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)an.</span><span class="sxs-lookup"><span data-stu-id="b75f6-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="b75f6-208">Eine exemplarische Vorgehensweise zum Bereitstellen der Beispiel-signalr-Anwendung finden Sie unter Veröffentlichen des Beispiels für die ersten Schritte mit [signalr als Windows Azure-Website](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="b75f6-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="b75f6-209">Ausführliche Informationen zum Bereitstellen eines Visual Studio-Webprojekts auf einer Windows Azure-Website finden Sie unter Bereitstellen [einer ASP.NET-Anwendung auf einer Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)-Website.</span><span class="sxs-lookup"><span data-stu-id="b75f6-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="b75f6-210">(Hinweis: der WebSocket-Transport wird für Windows Azure-Websites derzeit nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b75f6-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="b75f6-211">Wenn der WebSocket-Transport nicht verfügbar ist, verwendet signalr die anderen verfügbaren Transporte, wie im Abschnitt Transporte des [Themas Introduction to signalr](index.md)beschrieben.)</span><span class="sxs-lookup"><span data-stu-id="b75f6-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="b75f6-212">Weitere Informationen zu den erweiterten Konzepten der signalr-Entwicklung finden Sie auf den folgenden Websites für signalr-Quellcode und-Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="b75f6-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="b75f6-213">Signalr-Projekt</span><span class="sxs-lookup"><span data-stu-id="b75f6-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="b75f6-214">Signalr GitHub und Beispiele</span><span class="sxs-lookup"><span data-stu-id="b75f6-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b75f6-215">Signalr-wiki</span><span class="sxs-lookup"><span data-stu-id="b75f6-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
