---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.net-Web-API 2 in einer Azure-workerrolle ASP.NET 4. x
author: MikeWasson
description: 'Tutorial: Hosten von ASP.net-Web-API in einer Azure-workerrolle mithilfe von owin, um das Web-API-Framework selbst zu hosten.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448407"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="900e1-103">Host ASP.net-Web-API 2 in einer Azure-workerrolle</span><span class="sxs-lookup"><span data-stu-id="900e1-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="900e1-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="900e1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="900e1-105">In diesem Tutorial wird gezeigt, wie Sie ASP.net-Web-API in einer Azure-workerrolle hosten, indem Sie owin zum Selbsthosten des Web-API-Frameworks</span><span class="sxs-lookup"><span data-stu-id="900e1-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="900e1-106">[Open Web Interface for .net](http://owin.org/) (owin) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="900e1-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="900e1-107">Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS, erstellt wird – z. b. innerhalb einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="900e1-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="900e1-108">In diesem Tutorial verwenden Sie das Microsoft. owin. Host. HttpListener-Paket, das einen HTTP-Server bereitstellt, der zum selbst Hosten von owin-Anwendungen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="900e1-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="900e1-109">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="900e1-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="900e1-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="900e1-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="900e1-111">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="900e1-111">Web API 2</span></span>
> - [<span data-ttu-id="900e1-112">Azure SDK für .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="900e1-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="900e1-113">Erstellen eines Microsoft Azure Projekts</span><span class="sxs-lookup"><span data-stu-id="900e1-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="900e1-114">Starten Sie Visual Studio mit Administratorrechten.</span><span class="sxs-lookup"><span data-stu-id="900e1-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="900e1-115">Administrator Rechte sind erforderlich, um die Anwendung lokal mit dem Azure-Server Emulator zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="900e1-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="900e1-116">Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="900e1-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="900e1-117">Klicken Sie in **installierte Vorlagen**unter C#Visual auf **Cloud** , und klicken Sie dann auf **Windows Azure-clouddienst**.</span><span class="sxs-lookup"><span data-stu-id="900e1-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="900e1-118">Nennen Sie das Projekt "azureapp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="900e1-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="900e1-119">Doppelklicken Sie im Dialogfeld **neuer Windows Azure-clouddienst** auf workerrolle.</span><span class="sxs-lookup"><span data-stu-id="900e1-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="900e1-120">Belassen Sie den Standardnamen ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="900e1-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="900e1-121">In diesem Schritt wird der Projekt Mappe eine workerrolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="900e1-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="900e1-122">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="900e1-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="900e1-123">Die erstellte Visual Studio-Projekt Mappe enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="900e1-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="900e1-124">&quot;azureapp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="900e1-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="900e1-125">&quot;WorkerRole1-&quot; enthält den Code für die workerrolle.</span><span class="sxs-lookup"><span data-stu-id="900e1-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="900e1-126">Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzige Rolle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="900e1-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="900e1-127">Hinzufügen der Web-API und der owin-Pakete</span><span class="sxs-lookup"><span data-stu-id="900e1-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="900e1-128">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und klicken Sie dann auf Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="900e1-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="900e1-129">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="900e1-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="900e1-130">Hinzufügen eines HTTP-Endpunkts</span><span class="sxs-lookup"><span data-stu-id="900e1-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="900e1-131">Erweitern Sie in Projektmappen-Explorer das Projekt azureapp.</span><span class="sxs-lookup"><span data-stu-id="900e1-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="900e1-132">Erweitern Sie den Knoten Rollen, klicken Sie mit der rechten Maustaste auf WorkerRole1, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="900e1-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="900e1-133">Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Endpunkt hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="900e1-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="900e1-134">Wählen Sie in der Dropdown Liste **Protokoll** die Option "http" aus.</span><span class="sxs-lookup"><span data-stu-id="900e1-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="900e1-135">Geben Sie unter **öffentlicher Port** und **privater Port**80 ein.</span><span class="sxs-lookup"><span data-stu-id="900e1-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="900e1-136">Diese Portnummern dürfen sich unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="900e1-136">These port numbers can be different.</span></span> <span data-ttu-id="900e1-137">Der öffentliche Port wird vom Client verwendet, wenn er eine Anforderung an die Rolle sendet.</span><span class="sxs-lookup"><span data-stu-id="900e1-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="900e1-138">Web-API für Self-Host konfigurieren</span><span class="sxs-lookup"><span data-stu-id="900e1-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="900e1-139">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie / **Klasse** **Hinzufügen** , um eine neue Klasse hinzuzufügen</span><span class="sxs-lookup"><span data-stu-id="900e1-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="900e1-140">Geben Sie der Klassen den Namen `Startup`.</span><span class="sxs-lookup"><span data-stu-id="900e1-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="900e1-141">Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="900e1-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="900e1-142">Hinzufügen eines Web-API-Controllers</span><span class="sxs-lookup"><span data-stu-id="900e1-142">Add a Web API Controller</span></span>

<span data-ttu-id="900e1-143">Fügen Sie als nächstes eine Web-API-Controller Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="900e1-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="900e1-144">Klicken Sie mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie / **Klasse** **Hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="900e1-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="900e1-145">Benennen Sie die Klasse Testcontroller.</span><span class="sxs-lookup"><span data-stu-id="900e1-145">Name the class TestController.</span></span> <span data-ttu-id="900e1-146">Ersetzen Sie den gesamten Code in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="900e1-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="900e1-147">Der Einfachheit halber definiert dieser Controller nur zwei Get-Methoden, die nur Text zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="900e1-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="900e1-148">Starten des owin-Hosts</span><span class="sxs-lookup"><span data-stu-id="900e1-148">Start the OWIN Host</span></span>

<span data-ttu-id="900e1-149">Öffnen Sie die Datei WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="900e1-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="900e1-150">Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet und beendet wird.</span><span class="sxs-lookup"><span data-stu-id="900e1-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="900e1-151">Fügen Sie die folgende using-Anweisung hinzu:</span><span class="sxs-lookup"><span data-stu-id="900e1-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="900e1-152">Fügen Sie der `WorkerRole`-Klasse ein **iverwerfbares** Element hinzu:</span><span class="sxs-lookup"><span data-stu-id="900e1-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="900e1-153">Fügen Sie in der `OnStart`-Methode den folgenden Code hinzu, um den Host zu starten:</span><span class="sxs-lookup"><span data-stu-id="900e1-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="900e1-154">Die **webapp. Start** -Methode startet den owin-Host.</span><span class="sxs-lookup"><span data-stu-id="900e1-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="900e1-155">Der Name der `Startup` Klasse ist ein Typparameter für die Methode.</span><span class="sxs-lookup"><span data-stu-id="900e1-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="900e1-156">Gemäß der Konvention ruft der Host die `Configure`-Methode dieser Klasse auf.</span><span class="sxs-lookup"><span data-stu-id="900e1-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="900e1-157">Überschreiben Sie die `OnStop`, um die *\_App* -Instanz zu löschen:</span><span class="sxs-lookup"><span data-stu-id="900e1-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="900e1-158">Im folgenden finden Sie den gesamten Code für WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="900e1-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="900e1-159">Erstellen Sie die Projekt Mappe, und drücken Sie F5, um die Anwendung lokal im Azure-Server Emulator auszuführen.</span><span class="sxs-lookup"><span data-stu-id="900e1-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="900e1-160">Abhängig von den Firewalleinstellungen müssen Sie den Emulator möglicherweise über die Firewall zulassen.</span><span class="sxs-lookup"><span data-stu-id="900e1-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="900e1-161">Wenn Sie eine Ausnahme wie die folgende erhalten, finden Sie in [diesem Blogbeitrag](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) eine Problem Umgehung.</span><span class="sxs-lookup"><span data-stu-id="900e1-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="900e1-162">"Die Datei oder Assembly ' Microsoft. owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bs3856ad364e35 ' oder eine ihrer Abhängigkeiten konnte nicht geladen werden.</span><span class="sxs-lookup"><span data-stu-id="900e1-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="900e1-163">Die Manifest-Definition der lokalisierte Assembly stimmt nicht mit dem Assemblyverweis.</span><span class="sxs-lookup"><span data-stu-id="900e1-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="900e1-164">(Ausnahme von HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="900e1-164">(Exception from HRESULT: 0x80131040)"</span></span>

<span data-ttu-id="900e1-165">Der Server Emulator weist dem Endpunkt eine lokale IP-Adresse zu.</span><span class="sxs-lookup"><span data-stu-id="900e1-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="900e1-166">Sie finden die IP-Adresse, indem Sie die Server Emulator-Benutzeroberfläche anzeigen.</span><span class="sxs-lookup"><span data-stu-id="900e1-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="900e1-167">Klicken Sie im Benachrichtigungsbereich der Taskleiste mit der rechten Maustaste auf das Symbol Emulator, und wählen Sie Server **Emulator-Benutzeroberfläche anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="900e1-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="900e1-168">Suchen Sie die IP-Adresse unter Dienst Bereitstellungen, Bereitstellung [ID] und Dienst Details.</span><span class="sxs-lookup"><span data-stu-id="900e1-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="900e1-169">Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Address</em>/Test/1, wobei <em>Address</em> die IP-Adresse ist, die vom Server Emulator zugewiesen wird. beispielsweise `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="900e1-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="900e1-170">Die Antwort des Web-API-Controllers sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="900e1-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="900e1-171">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="900e1-171">Deploy to Azure</span></span>

<span data-ttu-id="900e1-172">Für diesen Schritt müssen Sie über ein Azure-Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="900e1-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="900e1-173">Wenn Sie noch nicht über eins verfügen, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen.</span><span class="sxs-lookup"><span data-stu-id="900e1-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="900e1-174">Weitere Informationen finden Sie unter [Microsoft Azure kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="900e1-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="900e1-175">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt azureapp.</span><span class="sxs-lookup"><span data-stu-id="900e1-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="900e1-176">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="900e1-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="900e1-177">Wenn Sie nicht bei Ihrem Azure-Konto angemeldet sind, klicken Sie auf **Anmelden**.</span><span class="sxs-lookup"><span data-stu-id="900e1-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="900e1-178">Nachdem Sie angemeldet sind, wählen Sie ein Abonnement aus, und klicken Sie auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="900e1-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="900e1-179">Geben Sie einen Namen für den clouddienst ein, und wählen Sie eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="900e1-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="900e1-180">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="900e1-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="900e1-181">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="900e1-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="900e1-182">Das Fenster Azure-Aktivitätsprotokoll zeigt den Fortschritt der Bereitstellung an.</span><span class="sxs-lookup"><span data-stu-id="900e1-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="900e1-183">Wenn die APP bereitgestellt ist, navigieren Sie zu http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="900e1-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="900e1-184">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="900e1-184">Additional Resources</span></span>

- [<span data-ttu-id="900e1-185">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="900e1-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="900e1-186">Katana-Projekt auf GitHub</span><span class="sxs-lookup"><span data-stu-id="900e1-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
