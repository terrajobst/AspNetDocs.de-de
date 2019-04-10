---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hosten von ASP.NET Web-API 2 in ein Azure-Workerrolle - ASP.NET 4.x
author: MikeWasson
description: 'Tutorial: Hosten von ASP.NET-Web-API in einer Azure-Workerrolle, die mithilfe von OWIN zum selfhosten der Web-API-Framework.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: bfb23aafb814010e8651965dad91ca20a37fd786
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59404623"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="b8557-103">Hosten von ASP.NET Web-API 2 in ein Azure-Workerrolle</span><span class="sxs-lookup"><span data-stu-id="b8557-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="b8557-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b8557-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b8557-105">Dieses Tutorial zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN zum selfhosten der Web-API-Framework.</span><span class="sxs-lookup"><span data-stu-id="b8557-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="b8557-106">[Öffnen von Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="b8557-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b8557-107">OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS Self-hosting – z. B. in einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="b8557-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="b8557-108">In diesem Tutorial verwenden Sie das Paket Microsoft.Owin.Host.HttpListener, die einem HTTP-Server enthält, die verwendet werden, um die OWIN-Anwendungen Self-Hosting.</span><span class="sxs-lookup"><span data-stu-id="b8557-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b8557-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b8557-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="b8557-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b8557-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b8557-111">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="b8557-111">Web API 2</span></span>
> - [<span data-ttu-id="b8557-112">Azure SDK für .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="b8557-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="b8557-113">Erstellen Sie ein Microsoft Azure-Projekt</span><span class="sxs-lookup"><span data-stu-id="b8557-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="b8557-114">Starten Sie Visual Studio mit Administratorrechten aus.</span><span class="sxs-lookup"><span data-stu-id="b8557-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="b8557-115">Administratorberechtigungen sind erforderlich, um die Anwendung lokal zu debuggen, mit dem Azure-Serveremulator.</span><span class="sxs-lookup"><span data-stu-id="b8557-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="b8557-116">Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b8557-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="b8557-117">Von **installierte Vorlagen**, klicken Sie unter Visual c# **Cloud** , und klicken Sie dann auf **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="b8557-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="b8557-118">Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8557-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="b8557-119">In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**.</span><span class="sxs-lookup"><span data-stu-id="b8557-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="b8557-120">Übernehmen Sie den Standardnamen ("WorkerRole1") aus.</span><span class="sxs-lookup"><span data-stu-id="b8557-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="b8557-121">Dieser Schritt wird der Projektmappe eine Worker-Rolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b8557-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="b8557-122">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8557-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="b8557-123">Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="b8557-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="b8557-124">&quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b8557-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="b8557-125">&quot;WorkerRole1&quot; enthält den Code für die Worker-Rolle.</span><span class="sxs-lookup"><span data-stu-id="b8557-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="b8557-126">Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzelne Rolle verwendet.</span><span class="sxs-lookup"><span data-stu-id="b8557-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="b8557-127">Fügen Sie die Web-API und OWIN-Pakete</span><span class="sxs-lookup"><span data-stu-id="b8557-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="b8557-128">Von der **Tools** Menü klicken Sie auf **NuGet Package Manager**, klicken Sie dann auf **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="b8557-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="b8557-129">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b8557-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="b8557-130">Fügen Sie einen HTTP-Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8557-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="b8557-131">Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp aus.</span><span class="sxs-lookup"><span data-stu-id="b8557-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="b8557-132">Erweitern Sie den Knoten "Rollen", mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="b8557-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="b8557-133">Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="b8557-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="b8557-134">In der **Protokoll** Dropdown-Liste, Option "http".</span><span class="sxs-lookup"><span data-stu-id="b8557-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="b8557-135">In **öffentlicher Port** und **privater Port**, geben Sie 80.</span><span class="sxs-lookup"><span data-stu-id="b8557-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="b8557-136">Diese Portnummern können sich unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="b8557-136">These port numbers can be different.</span></span> <span data-ttu-id="b8557-137">Der öffentliche Port ist, welche Clients verwenden, wenn sie eine Anforderung an die Rolle zu senden.</span><span class="sxs-lookup"><span data-stu-id="b8557-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="b8557-138">Konfigurieren von Web-API für selbst gehostete Dienste</span><span class="sxs-lookup"><span data-stu-id="b8557-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="b8557-139">Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b8557-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="b8557-140">Nennen Sie die Klasse `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b8557-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="b8557-141">Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b8557-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="b8557-142">Fügen Sie eine Web-API-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8557-142">Add a Web API Controller</span></span>

<span data-ttu-id="b8557-143">Als Nächstes fügen Sie eine Web-API-Controller-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8557-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="b8557-144">Mit der rechten Maustaste in des WorkerRole1-Projekts, und wählen Sie **hinzufügen** / **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="b8557-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="b8557-145">Der Name der TestController-Klasse.</span><span class="sxs-lookup"><span data-stu-id="b8557-145">Name the class TestController.</span></span> <span data-ttu-id="b8557-146">Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b8557-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="b8557-147">Der Einfachheit halber definiert diesen Controller lediglich zwei GET-Methoden, die nur-Text zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b8557-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="b8557-148">Starten Sie die OWIN-Host</span><span class="sxs-lookup"><span data-stu-id="b8557-148">Start the OWIN Host</span></span>

<span data-ttu-id="b8557-149">Öffnen Sie die Datei "WorkerRole.cs".</span><span class="sxs-lookup"><span data-stu-id="b8557-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="b8557-150">Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.</span><span class="sxs-lookup"><span data-stu-id="b8557-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="b8557-151">Fügen Sie die folgenden using-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="b8557-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="b8557-152">Hinzufügen einer **"IDisposable"** Member der `WorkerRole` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b8557-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="b8557-153">In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:</span><span class="sxs-lookup"><span data-stu-id="b8557-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="b8557-154">Die **WebApp.Start** Methode startet den OWIN-Host.</span><span class="sxs-lookup"><span data-stu-id="b8557-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="b8557-155">Der Name des der `Startup` Klasse ist ein Typparameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="b8557-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="b8557-156">Gemäß der Konvention der Host Ruft die `Configure` Methode dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="b8557-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="b8557-157">Überschreiben der `OnStop` zum Verwerfen der  *\_app* Instanz:</span><span class="sxs-lookup"><span data-stu-id="b8557-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="b8557-158">Hier ist der vollständige Code für "WorkerRole.cs":</span><span class="sxs-lookup"><span data-stu-id="b8557-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="b8557-159">Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator ausführen.</span><span class="sxs-lookup"><span data-stu-id="b8557-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="b8557-160">Abhängig von Ihren Firewalleinstellungen müssen Sie den Emulator über die Firewall zulassen.</span><span class="sxs-lookup"><span data-stu-id="b8557-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="b8557-161">Wenn Sie eine Ausnahme wie die folgende erhalten, finden Sie unter [in diesem Blogbeitrag](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) für dieses Problem zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="b8557-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="b8557-162">"Konnte nicht geladen werden, Datei oder Assembly ' Microsoft.Owin, Version = 2.0.2.0, Kultur = Neutral, PublicKeyToken = 31bf3856ad364e35' oder eine ihrer Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="b8557-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="b8557-163">Das manifest der sich Assemblydefinition entspricht nicht den der Assemblyverweis verweist.</span><span class="sxs-lookup"><span data-stu-id="b8557-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="b8557-164">(Ausnahme von HRESULT: 0x80131040)"</span><span class="sxs-lookup"><span data-stu-id="b8557-164">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="b8557-165">Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an.</span><span class="sxs-lookup"><span data-stu-id="b8557-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="b8557-166">Sie finden die IP-Adresse, indem Sie die Serveremulator-Benutzeroberfläche anzeigen.</span><span class="sxs-lookup"><span data-stu-id="b8557-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="b8557-167">Mit der rechten Maustaste in des Emulator-Symbols im Infobereich der Taskleiste und, und wählen **Serveremulator-Benutzeroberfläche anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="b8557-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="b8557-168">Suchen Sie die IP-Adresse unter Dienstbereitstellungen, die Bereitstellung [Id], Dienstdetails.</span><span class="sxs-lookup"><span data-stu-id="b8557-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="b8557-169">Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Adresse</em>/Test/1, in denen <em>Adresse</em> ist die IP-Adresse vom Serveremulator; z. B. `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="b8557-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="b8557-170">Die Antwort von der Web-API-Controller sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="b8557-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="b8557-171">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="b8557-171">Deploy to Azure</span></span>

<span data-ttu-id="b8557-172">Für diesen Schritt müssen Sie ein Azure-Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="b8557-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="b8557-173">Wenn Sie noch nicht haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen.</span><span class="sxs-lookup"><span data-stu-id="b8557-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b8557-174">Weitere Informationen finden Sie unter [kostenlose Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="b8557-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="b8557-175">Klicken Sie im Projektmappen-Explorer das Projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="b8557-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="b8557-176">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b8557-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="b8557-177">Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **Anmeldung**.</span><span class="sxs-lookup"><span data-stu-id="b8557-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="b8557-178">Nachdem Sie angemeldet sind, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="b8557-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="b8557-179">Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="b8557-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="b8557-180">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b8557-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="b8557-181">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="b8557-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="b8557-182">Das Fenster "Azure Activity Log" zeigt den Fortschritt der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="b8557-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="b8557-183">Wenn die app bereitgestellt wurde, navigieren Sie zur http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="b8557-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="b8557-184">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b8557-184">Additional Resources</span></span>

- [<span data-ttu-id="b8557-185">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="b8557-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="b8557-186">Katana-Projekt auf GitHub</span><span class="sxs-lookup"><span data-stu-id="b8557-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
