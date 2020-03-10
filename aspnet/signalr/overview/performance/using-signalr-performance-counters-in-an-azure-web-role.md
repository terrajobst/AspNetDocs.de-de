---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle | Microsoft-Dokumentation
author: guardrex
description: Installieren und Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle.
keywords: ASP. net, signalr, Leistungs Counter, Azure-webrolle
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467565"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="2f46b-104">Verwenden von signalr-Leistungsindikatoren in einer Azure-webrolle</span><span class="sxs-lookup"><span data-stu-id="2f46b-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="2f46b-105">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2f46b-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="2f46b-106">Signalr-Leistungsindikatoren werden verwendet, um die Leistung Ihrer APP in einer Azure-webrolle zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="2f46b-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="2f46b-107">Die Leistungsindikatoren werden von Microsoft Azure-Diagnose aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="2f46b-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="2f46b-108">Sie installieren signalr-Leistungsindikatoren in Azure mit *signalr. exe*, dem gleichen Tool, das auch für eigenständige oder lokale Apps verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f46b-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="2f46b-109">Da Azure-Rollen flüchtig sind, konfigurieren Sie eine APP so, dass Sie signalr-Leistungsindikatoren beim Start installieren und registrieren.</span><span class="sxs-lookup"><span data-stu-id="2f46b-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f46b-110">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="2f46b-110">Prerequisites</span></span>

* <span data-ttu-id="2f46b-111">Visual Studio 2015 oder [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="2f46b-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="2f46b-112">[Microsoft Azure SDK für Visual Studio](https://azure.microsoft.com/downloads/) **Hinweis: Starten Sie den Computer neu, nachdem Sie das SDK installiert haben.**</span><span class="sxs-lookup"><span data-stu-id="2f46b-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="2f46b-113">Microsoft Azure Abonnement: Informationen zur Registrierung für ein kostenloses Azure-Testkonto finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="2f46b-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="2f46b-114">Erstellen einer Azure-Webrollen Anwendung, die signalr-Leistungsindikatoren verfügbar macht</span><span class="sxs-lookup"><span data-stu-id="2f46b-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="2f46b-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f46b-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="2f46b-116">Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="2f46b-117">Wählen Sie im Dialogfeld **Neues Projekt** auf der linken Seite die Kategorie **Visual C#**  > **Cloud** aus, und wählen Sie dann die Vorlage **Azure-clouddienst** aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="2f46b-118">Benennen Sie die APP **signalrperfcounters** , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Neue cloudanwendung](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="2f46b-120">Wenn die **Cloud** -Vorlagen Kategorie oder die Azure- **clouddienstvorlage** nicht angezeigt wird, müssen Sie die Arbeitsauslastung für die **Azure-Entwicklung** für Visual Studio 2017 installieren.</span><span class="sxs-lookup"><span data-stu-id="2f46b-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="2f46b-121">Wählen Sie unten links im Dialogfeld **Neues Projekt** den Link **Öffnen Visual Studio-Installer** aus, um Visual Studio-Installer zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="2f46b-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="2f46b-122">Wählen Sie die Arbeitsauslastung für die **Azure-Entwicklung** , und wählen Sie dann **ändern** aus, um mit der Installation der</span><span class="sxs-lookup"><span data-stu-id="2f46b-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Azure-Entwicklungs Arbeitsauslastung in Visual Studio-Installer](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="2f46b-124">Wählen Sie im Dialogfeld **neuer Microsoft Azure Cloud-Dienst** die Option **ASP.net Web Role** aus, und klicken Sie auf die Schaltfläche >, um die Rolle dem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f46b-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="2f46b-125">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-125">Select **OK**.</span></span>

   ![ASP.net-webrolle hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="2f46b-127">Wählen Sie im Dialogfeld **neue ASP.NET Webanwendung WebRole1** die **MVC** -Vorlage aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Hinzufügen von MVC und Web-API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="2f46b-129">Öffnen Sie in **Projektmappen-Explorer**die Datei *Diagnostics. wadcfgx* unter **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Projektmappen-Explorer Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="2f46b-131">Ersetzen Sie den Inhalt der Datei durch die folgende Konfiguration, und speichern Sie die Datei:</span><span class="sxs-lookup"><span data-stu-id="2f46b-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="2f46b-132">Öffnen Sie die **Paket-Manager-Konsole** über die **Tools** > **nuget-Paket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="2f46b-133">Geben Sie die folgenden Befehle ein, um die neueste Version von signalr und des signalr-Hilfsprogramme-Pakets zu installieren:</span><span class="sxs-lookup"><span data-stu-id="2f46b-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="2f46b-134">Konfigurieren Sie die APP, um die signalr-Leistungsindikatoren in der Rollen Instanz zu installieren, wenn Sie gestartet oder wieder verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f46b-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="2f46b-135">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **WebRole1** , und wählen Sie > **neuen Ordner** **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="2f46b-136">Nennen Sie den neuen Ordner *Startup*.</span><span class="sxs-lookup"><span data-stu-id="2f46b-136">Name the new folder *Startup*.</span></span>

   ![Startordner hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="2f46b-138">Kopieren Sie die Datei *signalr. exe* (die mit dem Paket **Microsoft. Aspnet. signalr. utils** hinzugefügt wurde) aus \<Projektordner >/SignalRPerfCounters/Packages/Microsoft.Aspnet.SignalR.utils.\<Version >/Tools auf den *Start* Ordner, den Sie im vorherigen Schritt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="2f46b-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="2f46b-139">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Start* , und wählen Sie > **Vorhandenes Element** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="2f46b-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="2f46b-140">Wählen Sie im angezeigten Dialogfeld die Option *signalr. exe* aus, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    !["Signalr. exe" zu Projekt hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="2f46b-142">Klicken Sie mit der rechten Maustaste auf den erstellten *Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="2f46b-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="2f46b-143">Klicken Sie dann auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="2f46b-144">Wählen Sie den Knoten **Allgemein** aus, wählen Sie **Textdatei**aus, und benennen Sie das neue Element *signalrperfcounterinstall. cmd*.</span><span class="sxs-lookup"><span data-stu-id="2f46b-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="2f46b-145">Mit dieser Befehlsdatei werden die signalr-Leistungsindikatoren in der webrolle installiert.</span><span class="sxs-lookup"><span data-stu-id="2f46b-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Erstellen einer Batchdatei für die Installation von signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="2f46b-147">Wenn die Datei " *signalrperfcounterinstall. cmd* " von Visual Studio erstellt wird, wird Sie automatisch im Hauptfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="2f46b-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="2f46b-148">Ersetzen Sie den Inhalt der Datei durch das folgende Skript, speichern Sie die Datei, und schließen Sie Sie.</span><span class="sxs-lookup"><span data-stu-id="2f46b-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="2f46b-149">Mit diesem Skript wird die *signalr. exe*-Anweisung ausgeführt, mit der die signalr-Leistungsindikatoren der Rollen Instanz hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="2f46b-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="2f46b-150">Wählen Sie in **Projektmappen-Explorer**die Datei *signalr. exe* aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="2f46b-151">Legen Sie in den **Eigenschaften**der Datei in **Ausgabeverzeichnis kopieren** auf **immer kopieren**fest.</span><span class="sxs-lookup"><span data-stu-id="2f46b-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![In Ausgabeverzeichnis kopieren auf immer kopieren festlegen](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="2f46b-153">Wiederholen Sie den vorherigen Schritt für die Datei " *signalrperfcounterinstall. cmd* ".</span><span class="sxs-lookup"><span data-stu-id="2f46b-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

16. <span data-ttu-id="2f46b-154">Klicken Sie mit der rechten Maustaste auf die Datei *signalrperfcounterinstall. cmd* , und wählen Sie **Öffnen mit**aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="2f46b-155">Wählen Sie im angezeigten Dialogfeld die Option **Binär-Editor** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Mit binärem Editor öffnen](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="2f46b-157">Wählen Sie im Binär-Editor alle führenden Bytes in der Datei aus, und löschen Sie Sie.</span><span class="sxs-lookup"><span data-stu-id="2f46b-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="2f46b-158">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="2f46b-158">Save and close the file.</span></span>

    ![Führende Bytes löschen](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="2f46b-160">Öffnen Sie *Service Definition. csdef* , und fügen Sie einen Starttask hinzu, der die Datei *signalrperfcounterinstall. cmd* ausführt, wenn der Dienst gestartet wird:</span><span class="sxs-lookup"><span data-stu-id="2f46b-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="2f46b-161">Öffnen Sie `Views/Shared/_Layout.cshtml`, und entfernen Sie das jQuery-Bundle-Skript am Ende der Datei.</span><span class="sxs-lookup"><span data-stu-id="2f46b-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="2f46b-162">Fügen Sie einen JavaScript-Client hinzu, der fortlaufend die `increment`-Methode auf dem Server aufruft.</span><span class="sxs-lookup"><span data-stu-id="2f46b-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="2f46b-163">Öffnen Sie `Views/Home/Index.cshtml`, und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2f46b-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="2f46b-164">Erstellen Sie im **WebRole1** -Projekt einen neuen Ordner namens *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="2f46b-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="2f46b-165">Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Hubs* , und wählen Sie > **Neues Element** **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="2f46b-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="2f46b-166">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Kategorie **Web** > **signalr** aus, und wählen Sie dann die Element Vorlage **signalr Hub-Klasse (v2)** aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="2f46b-167">Benennen Sie den neuen Hub *MyHub.cs* , und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Hinzufügen der signalr Hub-Klasse zum Ordner Hubs im Dialogfeld Neues Element hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="2f46b-169">*MyHub.cs* wird automatisch im Hauptfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="2f46b-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="2f46b-170">Ersetzen Sie den Inhalt durch den folgenden Code, und speichern und schließen Sie dann die Datei:</span><span class="sxs-lookup"><span data-stu-id="2f46b-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="2f46b-171">" *[Crank. exe](signalr-connection-density-testing-with-crank.md)* " ist ein Tool zur Verbindungs Dichte Überprüfung, das mit der signalr-CodeBase bereitgestellt wird</span><span class="sxs-lookup"><span data-stu-id="2f46b-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="2f46b-172">Da für die Kurbel eine persistente Verbindung erforderlich ist, fügen Sie Ihrer Website für die Verwendung beim Testen einen hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f46b-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="2f46b-173">Fügen Sie dem **WebRole1** -Projekt einen neuen Ordner mit dem Namen *persistentconnections*hinzu.</span><span class="sxs-lookup"><span data-stu-id="2f46b-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="2f46b-174">Klicken Sie mit der rechten Maustaste auf diesen Ordner, und wählen Sie **Hinzufügen** > </span><span class="sxs-lookup"><span data-stu-id="2f46b-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2f46b-175">Benennen Sie die neue Klassendatei *MyPersistentConnections.cs* , und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="2f46b-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="2f46b-176">Die Datei *MyPersistentConnections.cs* wird im Hauptfenster von Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="2f46b-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="2f46b-177">Ersetzen Sie den Inhalt durch den folgenden Code, und speichern und schließen Sie dann die Datei:</span><span class="sxs-lookup"><span data-stu-id="2f46b-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="2f46b-178">Mit der `Startup`-Klasse beginnen die signalr-Objekte, wenn owin gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="2f46b-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="2f46b-179">Öffnen oder erstellen Sie *Startup.cs* , und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2f46b-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="2f46b-180">Im obigen Code markiert das `OwinStartup`-Attribut diese Klasse zum Starten von owin.</span><span class="sxs-lookup"><span data-stu-id="2f46b-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="2f46b-181">Die `Configuration`-Methode startet signalr.</span><span class="sxs-lookup"><span data-stu-id="2f46b-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="2f46b-182">Testen Sie die Anwendung im Microsoft Azure-Emulator, indem Sie **F5**drücken.</span><span class="sxs-lookup"><span data-stu-id="2f46b-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2f46b-183">Wenn bei **mapsignalr**eine **FileLoadException-Ausnahme** auftritt, ändern Sie die Bindungs Umleitungen in der Datei " *Web. config* " wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2f46b-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="2f46b-184">Warten Sie ungefähr eine Minute.</span><span class="sxs-lookup"><span data-stu-id="2f46b-184">Wait about one minute.</span></span> <span data-ttu-id="2f46b-185">Öffnen Sie das Cloud-Explorer Tool Fenster in Visual Studio ( > **Cloud-Explorer** **anzeigen** ), und erweitern Sie den Pfad `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="2f46b-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="2f46b-186">Doppelklicken Sie auf **wadperformancecounterstable**.</span><span class="sxs-lookup"><span data-stu-id="2f46b-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="2f46b-187">In den Tabellendaten sollten signalr-Leistungsindikatoren angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2f46b-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="2f46b-188">Wenn die Tabelle nicht angezeigt wird, müssen Sie möglicherweise Ihre Azure Storage Anmelde Informationen erneut eingeben.</span><span class="sxs-lookup"><span data-stu-id="2f46b-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="2f46b-189">Möglicherweise müssen Sie auf die Schaltfläche " **Aktualisieren** " klicken, um die Tabelle in **Cloud-Explorer** anzuzeigen, oder die Schaltfläche " **Aktualisieren** " im geöffneten Tabellenfenster auswählen, um die Daten in der Tabelle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2f46b-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Auswählen der Tabelle "wad-Leistungsindikatoren" in Visual Studio Cloud-Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Die in der Tabelle "wad-Leistungsindikatoren" gesammelten Indikatoren werden angezeigt.](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="2f46b-192">Um Ihre Anwendung in der Cloud zu testen, aktualisieren Sie die Datei **serviceconfiguration. Cloud. cscfg** , und legen Sie die `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` auf eine gültige Verbindungs Zeichenfolge für Azure Storage Konto fest.</span><span class="sxs-lookup"><span data-stu-id="2f46b-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="2f46b-193">Stellen Sie die Anwendung in Ihrem Azure-Abonnement bereit.</span><span class="sxs-lookup"><span data-stu-id="2f46b-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="2f46b-194">Ausführliche Informationen zum Bereitstellen einer Anwendung in Azure finden Sie unter [Erstellen und Bereitstellen eines clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="2f46b-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="2f46b-195">Warten Sie ein paar Minuten.</span><span class="sxs-lookup"><span data-stu-id="2f46b-195">Wait a few minutes.</span></span> <span data-ttu-id="2f46b-196">Suchen Sie in **Cloud-Explorer**das zuvor konfigurierte Speicherkonto, und suchen Sie die `WADPerformanceCountersTable`-Tabelle darin.</span><span class="sxs-lookup"><span data-stu-id="2f46b-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="2f46b-197">In den Tabellendaten sollten signalr-Leistungsindikatoren angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2f46b-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="2f46b-198">Wenn die Tabelle nicht angezeigt wird, müssen Sie möglicherweise Ihre Azure Storage Anmelde Informationen erneut eingeben.</span><span class="sxs-lookup"><span data-stu-id="2f46b-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="2f46b-199">Möglicherweise müssen Sie auf die Schaltfläche " **Aktualisieren** " klicken, um die Tabelle in **Cloud-Explorer** anzuzeigen, oder die Schaltfläche " **Aktualisieren** " im geöffneten Tabellenfenster auswählen, um die Daten in der Tabelle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2f46b-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="2f46b-200">Besonders vielen Dank an [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) für den ursprünglichen Inhalt, der in diesem Tutorial verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f46b-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
