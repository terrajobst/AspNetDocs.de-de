---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hosten von owin in einer Azure-workerrolle | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie owin in einer Microsoft Azure workerrolle selbst hosten. Open Web Interface for .net (owin) definiert eine Abstraktion zwischen dem .NET-Webserver...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472395"
---
# <a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="30761-104">Hosten von OWIN in einer Azure-Workerrolle</span><span class="sxs-lookup"><span data-stu-id="30761-104">Host OWIN in an Azure Worker Role</span></span>

<span data-ttu-id="30761-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="30761-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="30761-106">In diesem Tutorial wird gezeigt, wie Sie owin in einer Microsoft Azure workerrolle selbst hosten.</span><span class="sxs-lookup"><span data-stu-id="30761-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
>
> <span data-ttu-id="30761-107">[Open Web Interface for .net](http://owin.org/) (owin) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="30761-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="30761-108">Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS, erstellt wird – z. b. innerhalb einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="30761-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="30761-109">In diesem Tutorial erfahren Sie, wie Sie eine owin-Anwendung in einer Microsoft Azure workerrolle selbst hosten.</span><span class="sxs-lookup"><span data-stu-id="30761-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="30761-110">Weitere Informationen zu workerrollen finden Sie unter [Azure-Ausführungs Modelle](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="30761-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="30761-111">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="30761-111">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="30761-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="30761-112">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [<span data-ttu-id="30761-113">Azure SDK für .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="30761-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="30761-114">Microsoft. owin. SelfHost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="30761-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="30761-115">Erstellen eines Microsoft Azure Projekts</span><span class="sxs-lookup"><span data-stu-id="30761-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="30761-116">Starten Sie Visual Studio mit Administratorrechten.</span><span class="sxs-lookup"><span data-stu-id="30761-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="30761-117">Administrator Rechte sind erforderlich, um die Anwendung lokal mit dem Azure-Server Emulator zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="30761-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="30761-118">Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="30761-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="30761-119">Klicken Sie in **installierte Vorlagen**unter C#Visual auf **Cloud** , und klicken Sie dann auf **Windows Azure-clouddienst**.</span><span class="sxs-lookup"><span data-stu-id="30761-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="30761-120">Nennen Sie das Projekt "azureapp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="30761-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="30761-121">Doppelklicken Sie im Dialogfeld **neuer Windows Azure-clouddienst** auf workerrolle.</span><span class="sxs-lookup"><span data-stu-id="30761-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="30761-122">Belassen Sie den Standardnamen ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="30761-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="30761-123">In diesem Schritt wird der Projekt Mappe eine workerrolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="30761-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="30761-124">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="30761-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="30761-125">Die erstellte Visual Studio-Projekt Mappe enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="30761-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="30761-126">&quot;azureapp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="30761-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="30761-127">&quot;WorkerRole1-&quot; enthält den Code für die workerrolle.</span><span class="sxs-lookup"><span data-stu-id="30761-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="30761-128">Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzige Rolle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="30761-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="30761-129">Hinzufügen der selbst gehosteter owin-Pakete</span><span class="sxs-lookup"><span data-stu-id="30761-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="30761-130">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und klicken Sie dann auf Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="30761-130">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="30761-131">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="30761-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="30761-132">Hinzufügen eines HTTP-Endpunkts</span><span class="sxs-lookup"><span data-stu-id="30761-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="30761-133">Erweitern Sie in Projektmappen-Explorer das Projekt azureapp.</span><span class="sxs-lookup"><span data-stu-id="30761-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="30761-134">Erweitern Sie den Knoten Rollen, klicken Sie mit der rechten Maustaste auf WorkerRole1, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="30761-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="30761-135">Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Endpunkt hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="30761-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="30761-136">Wählen Sie in der Dropdown Liste **Protokoll** die Option "http" aus.</span><span class="sxs-lookup"><span data-stu-id="30761-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="30761-137">Geben Sie unter **öffentlicher Port** und **privater Port**80 ein.</span><span class="sxs-lookup"><span data-stu-id="30761-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="30761-138">Diese Portnummern dürfen sich unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="30761-138">These port numbers can be different.</span></span> <span data-ttu-id="30761-139">Der öffentliche Port wird vom Client verwendet, wenn er eine Anforderung an die Rolle sendet.</span><span class="sxs-lookup"><span data-stu-id="30761-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="30761-140">Erstellen der owin-Startklasse</span><span class="sxs-lookup"><span data-stu-id="30761-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="30761-141">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen</span><span class="sxs-lookup"><span data-stu-id="30761-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="30761-142">Geben Sie der Klassen den Namen `Startup`.</span><span class="sxs-lookup"><span data-stu-id="30761-142">Name the class `Startup`.</span></span>

<span data-ttu-id="30761-143">Ersetzen Sie den gesamten Code mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="30761-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="30761-144">Die `UseWelcomePage`-Erweiterungsmethode fügt Ihrer Anwendung eine einfache HTML-Seite hinzu, um zu überprüfen, ob die Website Funktions bereit ist.</span><span class="sxs-lookup"><span data-stu-id="30761-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="30761-145">Starten des owin-Hosts</span><span class="sxs-lookup"><span data-stu-id="30761-145">Start the OWIN Host</span></span>

<span data-ttu-id="30761-146">Öffnen Sie die Datei WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="30761-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="30761-147">Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet und beendet wird.</span><span class="sxs-lookup"><span data-stu-id="30761-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="30761-148">Fügen Sie die folgende using-Anweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="30761-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="30761-149">Fügen Sie der `WorkerRole`-Klasse ein **iverwerfbares** Element hinzu:</span><span class="sxs-lookup"><span data-stu-id="30761-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="30761-150">Fügen Sie in der `OnStart`-Methode den folgenden Code hinzu, um den Host zu starten:</span><span class="sxs-lookup"><span data-stu-id="30761-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="30761-151">Die **webapp. Start** -Methode startet den owin-Host.</span><span class="sxs-lookup"><span data-stu-id="30761-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="30761-152">Der Name der `Startup` Klasse ist ein Typparameter für die Methode.</span><span class="sxs-lookup"><span data-stu-id="30761-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="30761-153">Gemäß der Konvention ruft der Host die `Configure`-Methode dieser Klasse auf.</span><span class="sxs-lookup"><span data-stu-id="30761-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="30761-154">Überschreiben Sie die `OnStop`, um die *\_App* -Instanz zu löschen:</span><span class="sxs-lookup"><span data-stu-id="30761-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="30761-155">Im folgenden finden Sie den gesamten Code für WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="30761-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="30761-156">Erstellen Sie die Projekt Mappe, und drücken Sie F5, um die Anwendung lokal im Azure-Server Emulator auszuführen.</span><span class="sxs-lookup"><span data-stu-id="30761-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="30761-157">Abhängig von den Firewalleinstellungen müssen Sie den Emulator möglicherweise über die Firewall zulassen.</span><span class="sxs-lookup"><span data-stu-id="30761-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="30761-158">Der Server Emulator weist dem Endpunkt eine lokale IP-Adresse zu.</span><span class="sxs-lookup"><span data-stu-id="30761-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="30761-159">Sie finden die IP-Adresse, indem Sie die Server Emulator-Benutzeroberfläche anzeigen.</span><span class="sxs-lookup"><span data-stu-id="30761-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="30761-160">Klicken Sie im Benachrichtigungsbereich der Taskleiste mit der rechten Maustaste auf das Symbol Emulator, und wählen Sie Server **Emulator-Benutzeroberfläche anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="30761-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="30761-161">Suchen Sie die IP-Adresse unter Dienst Bereitstellungen, Bereitstellung [ID] und Dienst Details.</span><span class="sxs-lookup"><span data-stu-id="30761-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="30761-162">Öffnen Sie einen Webbrowser, und navigieren Sie zu http:\/\/*Adresse*, wobei *Address* die IP-Adresse ist, die vom Server Emulator zugewiesen wird. beispielsweise `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="30761-162">Open a web browser and navigate to http:\/\/*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="30761-163">Die owin-Willkommensseite sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="30761-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="30761-164">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="30761-164">Deploy to Azure</span></span>

<span data-ttu-id="30761-165">Für diesen Schritt müssen Sie über ein Azure-Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="30761-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="30761-166">Wenn Sie noch nicht über eins verfügen, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen.</span><span class="sxs-lookup"><span data-stu-id="30761-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="30761-167">Weitere Informationen finden Sie unter [Microsoft Azure kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="30761-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="30761-168">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt azureapp.</span><span class="sxs-lookup"><span data-stu-id="30761-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="30761-169">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="30761-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="30761-170">Wenn Sie nicht bei Ihrem Azure-Konto angemeldet sind, klicken Sie auf **Anmelden**.</span><span class="sxs-lookup"><span data-stu-id="30761-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="30761-171">Nachdem Sie angemeldet sind, wählen Sie ein Abonnement aus, und klicken Sie auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="30761-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="30761-172">Geben Sie einen Namen für den clouddienst ein, und wählen Sie eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="30761-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="30761-173">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="30761-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="30761-174">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="30761-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="30761-175">Das Fenster Azure-Aktivitätsprotokoll zeigt den Fortschritt der Bereitstellung an.</span><span class="sxs-lookup"><span data-stu-id="30761-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="30761-176">Wenn die APP bereitgestellt ist, navigieren Sie zu `http://appname.cloudapp.net/`, wobei *appname* der Name Ihres Cloud-Diensts ist.</span><span class="sxs-lookup"><span data-stu-id="30761-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30761-177">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="30761-177">Additional Resources</span></span>

- [<span data-ttu-id="30761-178">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="30761-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="30761-179">Katana-Projekt auf GitHub</span><span class="sxs-lookup"><span data-stu-id="30761-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
