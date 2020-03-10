---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Erstellen einer Hochfrequenz-Echt Zeit-App mit signalr 2 | Microsoft-Dokumentation'
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr verwendet, um hoch frequentes Messaging-Funktionen bereitzustellen.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450117"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="09f70-103">Tutorial: Erstellen einer Hochfrequenz-Echt Zeit-App mit signalr 2</span><span class="sxs-lookup"><span data-stu-id="09f70-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="09f70-104">In diesem Tutorial wird gezeigt, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um hoch frequentes Messaging-Funktionen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="09f70-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="09f70-105">In diesem Fall bedeutet "hoch frequentes Messaging", dass der Server Updates mit fester Geschwindigkeit sendet.</span><span class="sxs-lookup"><span data-stu-id="09f70-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="09f70-106">Sie senden bis zu 10 Nachrichten pro Sekunde.</span><span class="sxs-lookup"><span data-stu-id="09f70-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="09f70-107">Die von Ihnen erstellte Anwendung zeigt eine Form an, die Benutzer ziehen können.</span><span class="sxs-lookup"><span data-stu-id="09f70-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="09f70-108">Der Server aktualisiert die Position der Form in allen verbundenen Browsern entsprechend der Position der gezogenen Form mithilfe von zeitgesteuerten Updates.</span><span class="sxs-lookup"><span data-stu-id="09f70-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="09f70-109">Die in diesem Tutorial eingeführten Konzepte enthalten Anwendungen in Echtzeit-spielen und anderen Simulationsanwendungen.</span><span class="sxs-lookup"><span data-stu-id="09f70-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="09f70-110">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="09f70-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09f70-111">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="09f70-111">Set up the project</span></span>
> * <span data-ttu-id="09f70-112">Erstellen der Basisanwendung</span><span class="sxs-lookup"><span data-stu-id="09f70-112">Create the base application</span></span>
> * <span data-ttu-id="09f70-113">Beim Starten der APP dem Hub zuordnen</span><span class="sxs-lookup"><span data-stu-id="09f70-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="09f70-114">Hinzufügen des Clients</span><span class="sxs-lookup"><span data-stu-id="09f70-114">Add the client</span></span>
> * <span data-ttu-id="09f70-115">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="09f70-115">Run the app</span></span>
> * <span data-ttu-id="09f70-116">Hinzufügen der Client Schleife</span><span class="sxs-lookup"><span data-stu-id="09f70-116">Add the client loop</span></span>
> * <span data-ttu-id="09f70-117">Hinzufügen der Server Schleife</span><span class="sxs-lookup"><span data-stu-id="09f70-117">Add the server loop</span></span>
> * <span data-ttu-id="09f70-118">Smooth Animation hinzufügen</span><span class="sxs-lookup"><span data-stu-id="09f70-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="09f70-119">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="09f70-119">Prerequisites</span></span>

* <span data-ttu-id="09f70-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="09f70-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="09f70-121">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="09f70-121">Set up the project</span></span>

<span data-ttu-id="09f70-122">In diesem Abschnitt erstellen Sie das Projekt in Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="09f70-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="09f70-123">In diesem Abschnitt wird gezeigt, wie Sie Visual Studio 2017 verwenden, um eine leere ASP.NET-Webanwendung zu erstellen und die signalr-und jQuery. UI-Bibliotheken hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="09f70-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="09f70-124">Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="09f70-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web erstellen](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="09f70-126">Lassen Sie im Fenster **neue ASP.NET-Webanwendung-haphapdemo** die Option **leer** , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="09f70-127">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="09f70-128">Wählen Sie unter **Neues Element hinzufügen-kuveshapeer Demo**die Option **installiert** > **Visual C#**  > **Web** > **signalr** aus, und wählen Sie dann **signalr Hub-Klasse (v2)** aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="09f70-129">Nennen Sie die Klasse " *hveshapeer Hub* ", und fügen Sie Sie dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="09f70-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="09f70-130">In diesem Schritt wird die *MoveShapeHub.cs* -Klassendatei erstellt.</span><span class="sxs-lookup"><span data-stu-id="09f70-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="09f70-131">Gleichzeitig werden dem Projekt ein Satz von Skriptdateien und Assemblyverweisen hinzugefügt, die signalr unterstützen.</span><span class="sxs-lookup"><span data-stu-id="09f70-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="09f70-132">Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="09f70-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="09f70-133">Führen Sie in der **Paket-Manager-Konsole**den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="09f70-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="09f70-134">Mit dem Befehl wird die jQuery-UI-Bibliothek installiert.</span><span class="sxs-lookup"><span data-stu-id="09f70-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="09f70-135">Sie verwenden Sie zum Animieren der Form.</span><span class="sxs-lookup"><span data-stu-id="09f70-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="09f70-136">Erweitern Sie in **Projektmappen-Explorer**den Knoten Skripts.</span><span class="sxs-lookup"><span data-stu-id="09f70-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Skript Bibliotheks Verweise](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="09f70-138">Skript Bibliotheken für jQuery, jqueryui und signalr sind im Projekt sichtbar.</span><span class="sxs-lookup"><span data-stu-id="09f70-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="09f70-139">Erstellen der Basisanwendung</span><span class="sxs-lookup"><span data-stu-id="09f70-139">Create the base application</span></span>

<span data-ttu-id="09f70-140">In diesem Abschnitt erstellen Sie eine Browser Anwendung.</span><span class="sxs-lookup"><span data-stu-id="09f70-140">In this section, you create a browser application.</span></span> <span data-ttu-id="09f70-141">Die APP sendet den Speicherort der Form während jedes Maus Verschiebungs Ereignisses an den Server.</span><span class="sxs-lookup"><span data-stu-id="09f70-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="09f70-142">Der Server überträgt diese Informationen in Echtzeit an alle anderen verbundenen Clients.</span><span class="sxs-lookup"><span data-stu-id="09f70-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="09f70-143">In späteren Abschnitten erfahren Sie mehr über diese Anwendung.</span><span class="sxs-lookup"><span data-stu-id="09f70-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="09f70-144">Öffnen Sie die Datei *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="09f70-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="09f70-145">Ersetzen Sie den Code in der Datei *MoveShapeHub.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="09f70-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="09f70-146">Speichern Sie die Datei .</span><span class="sxs-lookup"><span data-stu-id="09f70-146">Save the file.</span></span>

<span data-ttu-id="09f70-147">Die `MoveShapeHub`-Klasse ist eine Implementierung eines signalr-Hubs.</span><span class="sxs-lookup"><span data-stu-id="09f70-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="09f70-148">Wie im Lernprogramm für die ersten Schritte [mit signalr](tutorial-getting-started-with-signalr.md) hat der Hub eine Methode, die von den Clients direkt aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="09f70-149">In diesem Fall sendet der Client ein Objekt mit den neuen X-und Y-Koordinaten der Form an den Server.</span><span class="sxs-lookup"><span data-stu-id="09f70-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="09f70-150">Diese Koordinaten werden an alle anderen verbundenen Clients übertragen.</span><span class="sxs-lookup"><span data-stu-id="09f70-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="09f70-151">Signalr serialisiert dieses Objekt automatisch mithilfe von JSON.</span><span class="sxs-lookup"><span data-stu-id="09f70-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="09f70-152">Die APP sendet das `ShapeModel`-Objekt an den Client.</span><span class="sxs-lookup"><span data-stu-id="09f70-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="09f70-153">Sie enthält Member, um die Position der Form zu speichern.</span><span class="sxs-lookup"><span data-stu-id="09f70-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="09f70-154">Die-Version des-Objekts auf dem Server verfügt auch über einen Member, um zu verfolgen, welche Client Daten gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="09f70-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="09f70-155">Dieses Objekt verhindert, dass der Server die Daten eines Clients zurück an sich selbst sendet.</span><span class="sxs-lookup"><span data-stu-id="09f70-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="09f70-156">Dieser Member verwendet das `JsonIgnore`-Attribut, um zu verhindern, dass die Anwendung die Daten serialisiert und an den Client zurücksendet.</span><span class="sxs-lookup"><span data-stu-id="09f70-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="09f70-157">Beim Starten der APP dem Hub zuordnen</span><span class="sxs-lookup"><span data-stu-id="09f70-157">Map to the hub when app starts</span></span>

<span data-ttu-id="09f70-158">Als nächstes richten Sie die Zuordnung zum Hub ein, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="09f70-159">In signalr 2 wird durch das Hinzufügen einer owin-Startklasse die Zuordnung erstellt.</span><span class="sxs-lookup"><span data-stu-id="09f70-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="09f70-160">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **Neues Element** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="09f70-161">Wählen Sie in **Neues Element hinzufügen-muveshapeer Demo** die Option **installiert** > **Visual C#**  > **Web** aus, und wählen Sie dann **owin-Startklasse**aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="09f70-162">Benennen Sie die Klasse *Startup* , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="09f70-163">Ersetzen Sie den Standard Code in der Datei *Startup.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="09f70-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="09f70-164">Die owin-Startklasse ruft `MapSignalR` auf, wenn die APP die `Configuration`-Methode ausführt.</span><span class="sxs-lookup"><span data-stu-id="09f70-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="09f70-165">Die APP fügt die Klasse dem Startprozess von owin mithilfe des `OwinStartup` Assembly-Attributs hinzu.</span><span class="sxs-lookup"><span data-stu-id="09f70-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="09f70-166">Hinzufügen des Clients</span><span class="sxs-lookup"><span data-stu-id="09f70-166">Add the client</span></span>

<span data-ttu-id="09f70-167">Fügen Sie die HTML-Seite für den Client hinzu.</span><span class="sxs-lookup"><span data-stu-id="09f70-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="09f70-168">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **HTML-Seite** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="09f70-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="09f70-169">Benennen Sie die Seite **Standard** , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="09f70-170">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *default. html* , und wählen Sie **als Start Seite festlegen**aus.</span><span class="sxs-lookup"><span data-stu-id="09f70-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="09f70-171">Ersetzen Sie den Standard Code in der Datei " *default. html* " durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="09f70-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="09f70-172">Erweitern Sie in **Projektmappen-Explorer**den Eintrag **Skripts**.</span><span class="sxs-lookup"><span data-stu-id="09f70-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="09f70-173">Skript Bibliotheken für jQuery und signalr sind im Projekt sichtbar.</span><span class="sxs-lookup"><span data-stu-id="09f70-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="09f70-174">Der Paket-Manager installiert eine spätere Version der signalr-Skripts.</span><span class="sxs-lookup"><span data-stu-id="09f70-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="09f70-175">Aktualisieren Sie die Skript Verweise im Codeblock, damit Sie den Versionen der Skriptdateien im Projekt entsprechen.</span><span class="sxs-lookup"><span data-stu-id="09f70-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="09f70-176">Dieser HTML-und JavaScript-Code erstellt eine rote `div` namens `shape`.</span><span class="sxs-lookup"><span data-stu-id="09f70-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="09f70-177">Er aktiviert das Zieh Verhalten der Form mithilfe der jQuery-Bibliothek und verwendet das `drag`-Ereignis, um die Position der Form an den Server zu senden.</span><span class="sxs-lookup"><span data-stu-id="09f70-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="09f70-178">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="09f70-178">Run the app</span></span>

<span data-ttu-id="09f70-179">Sie können die app ausführen, um Sie zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="09f70-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="09f70-180">Wenn Sie die Form um ein Browserfenster ziehen, bewegt sich auch die Form in den anderen Browsern.</span><span class="sxs-lookup"><span data-stu-id="09f70-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="09f70-181">Aktivieren Sie auf der Symbolleiste das **Skript Debugging** , und wählen Sie dann die Schaltfläche wiedergeben aus, um die Anwendung im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="09f70-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Screenshot des Benutzers zum Aktivieren des Debugmodus und zum Auswählen von Play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="09f70-183">In der oberen rechten Ecke wird ein Browserfenster mit der roten Form geöffnet.</span><span class="sxs-lookup"><span data-stu-id="09f70-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="09f70-184">Kopieren Sie die URL der Seite.</span><span class="sxs-lookup"><span data-stu-id="09f70-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="09f70-185">Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="09f70-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="09f70-186">Ziehen Sie die Form in eines der Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="09f70-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="09f70-187">Die Form im anderen Browserfenster folgt.</span><span class="sxs-lookup"><span data-stu-id="09f70-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="09f70-188">Obwohl die Anwendung mit dieser Methode funktioniert, ist Sie kein empfohlenes Programmiermodell.</span><span class="sxs-lookup"><span data-stu-id="09f70-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="09f70-189">Es gibt keine Obergrenze für die Anzahl der gesendeten Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="09f70-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="09f70-190">Folglich werden die Clients und der Server mit den Nachrichten und der Leistung beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="09f70-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="09f70-191">Außerdem zeigt die APP eine disjoinanimation auf dem Client an.</span><span class="sxs-lookup"><span data-stu-id="09f70-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="09f70-192">Diese ruckartig-Animation wird durchgeführt, da die Form sofort von jeder Methode bewegt wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="09f70-193">Es ist besser, wenn die Form reibungslos an jeden neuen Speicherort verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="09f70-194">Als Nächstes erfahren Sie, wie Sie diese Probleme beheben.</span><span class="sxs-lookup"><span data-stu-id="09f70-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="09f70-195">Hinzufügen der Client Schleife</span><span class="sxs-lookup"><span data-stu-id="09f70-195">Add the client loop</span></span>

<span data-ttu-id="09f70-196">Wenn Sie den Speicherort der Form bei jedem Maus Verschiebungs Ereignis senden, entsteht eine unnötige Menge an Netzwerk Datenverkehr.</span><span class="sxs-lookup"><span data-stu-id="09f70-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="09f70-197">Die APP muss die Nachrichten vom Client drosseln.</span><span class="sxs-lookup"><span data-stu-id="09f70-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="09f70-198">Verwenden Sie die JavaScript-`setInterval`-Funktion, um eine-Schleife einzurichten, die neue Positionsinformationen mit fester Rate an den Server sendet.</span><span class="sxs-lookup"><span data-stu-id="09f70-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="09f70-199">Diese Schleife ist eine grundlegende Darstellung einer "Game-Schleife".</span><span class="sxs-lookup"><span data-stu-id="09f70-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="09f70-200">Es handelt sich um eine wiederholt aufgerufene Funktion, die die gesamte Funktionalität eines Spiels steuert.</span><span class="sxs-lookup"><span data-stu-id="09f70-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="09f70-201">Ersetzen Sie den Client Code in der Datei " *default. html* " durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="09f70-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="09f70-202">Sie müssen die Skript Verweise erneut ersetzen.</span><span class="sxs-lookup"><span data-stu-id="09f70-202">You have to replace the script references again.</span></span> <span data-ttu-id="09f70-203">Sie müssen den Versionen der Skripts im Projekt entsprechen.</span><span class="sxs-lookup"><span data-stu-id="09f70-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="09f70-204">Mit diesem neuen Code wird die `updateServerModel`-Funktion hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="09f70-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="09f70-205">Sie wird in einer bestimmten Häufigkeit aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="09f70-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="09f70-206">Die-Funktion sendet die Positionsdaten immer dann an den Server, wenn das `moved`-Flag angibt, dass neue Positionsdaten gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="09f70-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="09f70-207">Wiedergabe Schaltfläche zum Starten der Anwendung</span><span class="sxs-lookup"><span data-stu-id="09f70-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="09f70-208">Kopieren Sie die URL der Seite.</span><span class="sxs-lookup"><span data-stu-id="09f70-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="09f70-209">Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="09f70-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="09f70-210">Ziehen Sie die Form in eines der Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="09f70-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="09f70-211">Die Form im anderen Browserfenster folgt.</span><span class="sxs-lookup"><span data-stu-id="09f70-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="09f70-212">Da die APP die Anzahl der Nachrichten drosselt, die an den Server gesendet werden, wird die Animation zuerst nicht als glatt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="09f70-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="09f70-213">Hinzufügen der Server Schleife</span><span class="sxs-lookup"><span data-stu-id="09f70-213">Add the server loop</span></span>

<span data-ttu-id="09f70-214">In der aktuellen Anwendung werden Nachrichten, die vom Server an den Client gesendet werden, so oft wie Sie empfangen.</span><span class="sxs-lookup"><span data-stu-id="09f70-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="09f70-215">Der Netzwerk Datenverkehr weist ein ähnliches Problem auf wie auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="09f70-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="09f70-216">Die APP kann Nachrichten häufiger als benötigt senden.</span><span class="sxs-lookup"><span data-stu-id="09f70-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="09f70-217">Die Verbindung kann daher überflutet werden.</span><span class="sxs-lookup"><span data-stu-id="09f70-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="09f70-218">In diesem Abschnitt wird beschrieben, wie Sie den Server aktualisieren, um einen Zeitgeber hinzuzufügen, der die Rate der ausgehenden Nachrichten drosselt.</span><span class="sxs-lookup"><span data-stu-id="09f70-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="09f70-219">Ersetzen Sie den Inhalt `MoveShapeHub.cs` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="09f70-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="09f70-220">Wählen Sie die Wiedergabe Schaltfläche, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="09f70-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="09f70-221">Kopieren Sie die URL der Seite.</span><span class="sxs-lookup"><span data-stu-id="09f70-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="09f70-222">Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="09f70-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="09f70-223">Ziehen Sie die Form in eines der Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="09f70-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="09f70-224">Dieser Code erweitert den Client, um die `Broadcaster`-Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="09f70-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="09f70-225">Die neue Klasse schränkt die ausgehenden Nachrichten mit der `Timer`-Klasse von .NET Framework ein.</span><span class="sxs-lookup"><span data-stu-id="09f70-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="09f70-226">Es ist gut zu lernen, dass der Hub selbst Transitory ist.</span><span class="sxs-lookup"><span data-stu-id="09f70-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="09f70-227">Es wird jedes Mal erstellt, wenn es benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-227">It's created every time it's needed.</span></span> <span data-ttu-id="09f70-228">Daher erstellt die APP den `Broadcaster` als Singleton.</span><span class="sxs-lookup"><span data-stu-id="09f70-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="09f70-229">Sie verwendet die verzögerte Initialisierung, um die Erstellung des `Broadcaster`zu verzögern, bis Sie benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="09f70-230">Dadurch wird sichergestellt, dass die APP die erste Hub-Instanz vollständig erstellt, bevor der Timer gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="09f70-231">Der Aufrufe der `UpdateShape` Funktion der Clients wird dann aus der `UpdateModel`-Methode des Hubs verschoben.</span><span class="sxs-lookup"><span data-stu-id="09f70-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="09f70-232">Er wird nicht mehr sofort aufgerufen, wenn die APP eingehende Nachrichten empfängt.</span><span class="sxs-lookup"><span data-stu-id="09f70-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="09f70-233">Stattdessen sendet die APP die Nachrichten mit einer Rate von 25 Aufrufen pro Sekunde an die Clients.</span><span class="sxs-lookup"><span data-stu-id="09f70-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="09f70-234">Der Prozess wird vom `_broadcastLoop` Timer innerhalb der `Broadcaster`-Klasse verwaltet.</span><span class="sxs-lookup"><span data-stu-id="09f70-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="09f70-235">Anstatt die Client Methode direkt vom Hub aufzurufenden zu lassen, muss die `Broadcaster` Klasse einen Verweis auf den aktuell Betriebs `_hubContext` Hub abrufen.</span><span class="sxs-lookup"><span data-stu-id="09f70-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="09f70-236">Der Verweis wird mit dem `GlobalHost`abgerufen.</span><span class="sxs-lookup"><span data-stu-id="09f70-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="09f70-237">Smooth Animation hinzufügen</span><span class="sxs-lookup"><span data-stu-id="09f70-237">Add smooth animation</span></span>

<span data-ttu-id="09f70-238">Die Anwendung ist fast fertig, aber wir könnten eine Verbesserung vornehmen.</span><span class="sxs-lookup"><span data-stu-id="09f70-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="09f70-239">Die app verschiebt die Form auf dem Client als Antwort auf Server Meldungen.</span><span class="sxs-lookup"><span data-stu-id="09f70-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="09f70-240">Anstatt die Position der Form auf den neuen Speicherort festzulegen, den der Server erhält, verwenden Sie die `animate`-Funktion der jQuery-UI-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="09f70-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="09f70-241">Sie kann die Form zwischen der aktuellen und der neuen Position reibungslos verschieben.</span><span class="sxs-lookup"><span data-stu-id="09f70-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="09f70-242">Aktualisieren Sie die `updateShape`-Methode des Clients in der Datei " *default. html* " so, dass Sie wie der hervorgehobene Code aussieht:</span><span class="sxs-lookup"><span data-stu-id="09f70-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="09f70-243">Wählen Sie die Wiedergabe Schaltfläche, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="09f70-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="09f70-244">Kopieren Sie die URL der Seite.</span><span class="sxs-lookup"><span data-stu-id="09f70-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="09f70-245">Öffnen Sie einen anderen Browser, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="09f70-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="09f70-246">Ziehen Sie die Form in eines der Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="09f70-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="09f70-247">Die Bewegung der Form im anderen Fenster wird weniger ruckartig angezeigt.</span><span class="sxs-lookup"><span data-stu-id="09f70-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="09f70-248">Die APP interpoliert die Bewegung im Laufe der Zeit, anstatt Sie einmal pro eingehender Nachricht festzulegen.</span><span class="sxs-lookup"><span data-stu-id="09f70-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="09f70-249">Mit diesem Code wird die Form von der alten an die neue Position verschoben.</span><span class="sxs-lookup"><span data-stu-id="09f70-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="09f70-250">Der Server gibt die Position der Form im Verlauf des Animations Intervalls an.</span><span class="sxs-lookup"><span data-stu-id="09f70-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="09f70-251">In diesem Fall ist dies 100 Millisekunden.</span><span class="sxs-lookup"><span data-stu-id="09f70-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="09f70-252">Die APP löscht jede vorherige Animation, die auf der Form ausgeführt wird, bevor die neue Animation gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="09f70-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="09f70-253">Abrufen des Codes</span><span class="sxs-lookup"><span data-stu-id="09f70-253">Get the code</span></span>

[<span data-ttu-id="09f70-254">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="09f70-254">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="09f70-255">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="09f70-255">Additional resources</span></span>

<span data-ttu-id="09f70-256">Das Kommunikations Paradigma, das Sie soeben kennengelernt haben, eignet sich für die Entwicklung von Online spielen und anderen Simulationen, wie z. b. [mit signalr erstellten shootr](https://shootr.azurewebsites.net/)-spielen.</span><span class="sxs-lookup"><span data-stu-id="09f70-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="09f70-257">Weitere Informationen zu signalr finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="09f70-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="09f70-258">Signalr-Projekt</span><span class="sxs-lookup"><span data-stu-id="09f70-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="09f70-259">Signalr GitHub und Beispiele</span><span class="sxs-lookup"><span data-stu-id="09f70-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="09f70-260">Signalr-wiki</span><span class="sxs-lookup"><span data-stu-id="09f70-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="09f70-261">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="09f70-261">Next steps</span></span>

<span data-ttu-id="09f70-262">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="09f70-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09f70-263">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="09f70-263">Set up the project</span></span>
> * <span data-ttu-id="09f70-264">Die Basisanwendung wurde erstellt.</span><span class="sxs-lookup"><span data-stu-id="09f70-264">Created the base application</span></span>
> * <span data-ttu-id="09f70-265">Wird dem Hub beim Starten der APP zugeordnet</span><span class="sxs-lookup"><span data-stu-id="09f70-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="09f70-266">Hinzugefügter Client</span><span class="sxs-lookup"><span data-stu-id="09f70-266">Added the client</span></span>
> * <span data-ttu-id="09f70-267">Die APP wurde ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="09f70-267">Ran the app</span></span>
> * <span data-ttu-id="09f70-268">Die Client Schleife wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="09f70-268">Added the client loop</span></span>
> * <span data-ttu-id="09f70-269">Die Server Schleife wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="09f70-269">Added the server loop</span></span>
> * <span data-ttu-id="09f70-270">Smooth Animation hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="09f70-270">Added smooth animation</span></span>

<span data-ttu-id="09f70-271">Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine Webanwendung erstellen, die ASP.net signalr 2 verwendet, um Server Broadcast Funktionen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="09f70-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="09f70-272">Signalr 2 und Server Übertragung</span><span class="sxs-lookup"><span data-stu-id="09f70-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)