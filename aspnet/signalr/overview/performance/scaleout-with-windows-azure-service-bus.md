---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Horizontale Skalierung von signalr mit Azure Service Bus | Microsoft-Dokumentation
author: bradygaster
description: Die in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr Version 2 vorherige Versionen dieses Themas für die signalr 1. x-Version dieses Themas,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467733"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="03a64-103">Horizontale Skalierung in SignalR mit Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="03a64-103">SignalR Scaleout with Azure Service Bus</span></span>

<span data-ttu-id="03a64-104">von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="03a64-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="03a64-105">In diesem Tutorial stellen Sie eine signalr-Anwendung für eine Windows Azure-webrolle bereit und verwenden die Service Bus Rückwand, um Nachrichten an jede Rollen Instanz zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="03a64-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="03a64-106">(Sie können die Service Bus-Rückwand auch mit [Web-Apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/)verwenden.)</span><span class="sxs-lookup"><span data-stu-id="03a64-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="03a64-107">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="03a64-107">Prerequisites:</span></span>

- <span data-ttu-id="03a64-108">Ein Windows Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="03a64-108">A Windows Azure account.</span></span>
- <span data-ttu-id="03a64-109">Das [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="03a64-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="03a64-110">Visual Studio 2012 oder 2013.</span><span class="sxs-lookup"><span data-stu-id="03a64-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="03a64-111">Die Service Bus-Rückwand ist auch mit [Service Bus für Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1,1, kompatibel.</span><span class="sxs-lookup"><span data-stu-id="03a64-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="03a64-112">Es ist jedoch nicht kompatibel mit Version 1,0 von Service Bus für Windows Server.</span><span class="sxs-lookup"><span data-stu-id="03a64-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="03a64-113">Preise</span><span class="sxs-lookup"><span data-stu-id="03a64-113">Pricing</span></span>

<span data-ttu-id="03a64-114">Die Service Bus Rückwand verwendet Themen zum Senden von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="03a64-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="03a64-115">Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="03a64-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="03a64-116">Zum Zeitpunkt der Erstellung dieses Artikels können Sie 1 Million Nachrichten pro Monat für weniger als $1 senden.</span><span class="sxs-lookup"><span data-stu-id="03a64-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="03a64-117">Die Rückwand sendet eine Service Bus-Nachricht für jeden Aufruf einer signalr-hubmethode.</span><span class="sxs-lookup"><span data-stu-id="03a64-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="03a64-118">Außerdem gibt es einige Steuerungs Meldungen für Verbindungen, Trennungen, das beitreten zu Gruppen und so weiter.</span><span class="sxs-lookup"><span data-stu-id="03a64-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="03a64-119">In den meisten Anwendungen ist der Großteil des Nachrichtenverkehrs hubmethoden Aufrufe.</span><span class="sxs-lookup"><span data-stu-id="03a64-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="03a64-120">Übersicht</span><span class="sxs-lookup"><span data-stu-id="03a64-120">Overview</span></span>

<span data-ttu-id="03a64-121">Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="03a64-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="03a64-122">Verwenden Sie das Windows-Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="03a64-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="03a64-123">Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="03a64-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="03a64-124">Microsoft. Aspnet. signalr</span><span class="sxs-lookup"><span data-stu-id="03a64-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="03a64-125">[Microsoft. Aspnet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) oder [Microsoft. Aspnet. signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="03a64-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="03a64-126">Erstellen Sie eine signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="03a64-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="03a64-127">Fügen Sie Startup.cs den folgenden Code hinzu, um die Backplane zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="03a64-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="03a64-128">Dieser Code konfiguriert die Rückwand mit den Standardwerten für [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) und [maxqueuelength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="03a64-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="03a64-129">Weitere Informationen zum Ändern dieser Werte finden Sie unter [signalr Performance: ScaleOut Metrics](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="03a64-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="03a64-130">Wählen Sie für jede Anwendung einen anderen Wert für "yourappname" aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="03a64-131">Verwenden Sie nicht denselben Wert für mehrere Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="03a64-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="03a64-132">Erstellen der Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="03a64-132">Create the Azure Services</span></span>

<span data-ttu-id="03a64-133">Erstellen Sie einen clouddienst, wie unter [Erstellen und Bereitstellen eines clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="03a64-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="03a64-134">Befolgen Sie die Schritte im Abschnitt "Gewusst wie: Erstellen eines clouddiensts mit schneller Fassung".</span><span class="sxs-lookup"><span data-stu-id="03a64-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="03a64-135">Für dieses Tutorial müssen Sie kein Zertifikat hochladen.</span><span class="sxs-lookup"><span data-stu-id="03a64-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="03a64-136">Erstellen Sie einen neuen Service Bus Namespace, wie unter Vorgehens [Weise beim Verwenden von Service Bus Themen/Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="03a64-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="03a64-137">Befolgen Sie die Schritte im Abschnitt "Erstellen eines Dienst Namespace".</span><span class="sxs-lookup"><span data-stu-id="03a64-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="03a64-138">Stellen Sie sicher, dass Sie die gleiche Region für den clouddienst und den Service Bus-Namespace auswählen.</span><span class="sxs-lookup"><span data-stu-id="03a64-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="03a64-139">Erstellen des Visual Studio-Projekts</span><span class="sxs-lookup"><span data-stu-id="03a64-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="03a64-140">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03a64-140">Start Visual Studio.</span></span> <span data-ttu-id="03a64-141">Klicken Sie im Menü **Datei** auf **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="03a64-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="03a64-142">Erweitern Sie im Dialogfeld **Neues Projekt** den **Eintrag C#Visual** .</span><span class="sxs-lookup"><span data-stu-id="03a64-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="03a64-143">Wählen Sie unter **installierte Vorlagen**die Option **Cloud** , und wählen Sie dann **Windows Azure-clouddienst**aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="03a64-144">Behalten Sie die Standard .NET Framework 4,5 bei.</span><span class="sxs-lookup"><span data-stu-id="03a64-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="03a64-145">Nennen Sie die Anwendung Chatservice, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="03a64-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="03a64-146">Wählen Sie im Dialogfeld **neuer Windows Azure-clouddienst** die Option ASP.net Web Role aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="03a64-147">Klicken Sie auf die Schaltfläche mit dem Pfeil nach rechts ( **&gt;** ), um die Rolle der Projekt Mappe hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="03a64-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="03a64-148">Zeigen Sie mit der Maus auf die neue Rolle, damit das Stift Symbol sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="03a64-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="03a64-149">Klicken Sie auf dieses Symbol, um die Rolle umzubenennen.</span><span class="sxs-lookup"><span data-stu-id="03a64-149">Click this icon to rename the role.</span></span> <span data-ttu-id="03a64-150">Benennen Sie die Rolle "signalrchat", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="03a64-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="03a64-151">Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Option **MVC**aus, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="03a64-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="03a64-152">Der Projekt-Assistent erstellt zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="03a64-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="03a64-153">Chatservice: dieses Projekt ist die Windows Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="03a64-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="03a64-154">Dabei werden die Azure-Rollen und andere Konfigurationsoptionen definiert.</span><span class="sxs-lookup"><span data-stu-id="03a64-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="03a64-155">Signalrchat: dieses Projekt ist Ihr ASP.NET MVC 5-Projekt.</span><span class="sxs-lookup"><span data-stu-id="03a64-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="03a64-156">Erstellen der signalr Chat-Anwendung</span><span class="sxs-lookup"><span data-stu-id="03a64-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="03a64-157">Um die Chat-Anwendung zu erstellen, führen Sie die Schritte im Tutorial erstellen [von signalr und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="03a64-158">Verwenden Sie nuget, um die erforderlichen Bibliotheken zu installieren.</span><span class="sxs-lookup"><span data-stu-id="03a64-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="03a64-159">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="03a64-160">Geben Sie im Fenster **Paket-Manager-Konsole** die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="03a64-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="03a64-161">Verwenden Sie die Option `-ProjectName`, um die Pakete für das MVC-Projekt ASP.net anstelle des Windows Azure-Projekts zu installieren.</span><span class="sxs-lookup"><span data-stu-id="03a64-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="03a64-162">Konfigurieren der Backplane</span><span class="sxs-lookup"><span data-stu-id="03a64-162">Configure the Backplane</span></span>

<span data-ttu-id="03a64-163">Fügen Sie in der Startup.cs-Datei Ihrer Anwendung den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="03a64-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="03a64-164">Nun müssen Sie Ihre Service Bus-Verbindungs Zeichenfolge erhalten.</span><span class="sxs-lookup"><span data-stu-id="03a64-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="03a64-165">Wählen Sie im Azure-Portal den Service Bus-Namespace aus, den Sie erstellt haben, und klicken Sie auf das Symbol Zugriffsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="03a64-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="03a64-166">Kopieren Sie die Verbindungs Zeichenfolge in die Zwischenablage, und fügen Sie Sie in die *ConnectionString* -Variable ein.</span><span class="sxs-lookup"><span data-stu-id="03a64-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="03a64-167">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="03a64-167">Deploy to Azure</span></span>

<span data-ttu-id="03a64-168">Erweitern Sie in Projektmappen-Explorer den Ordner **Rollen** im Projekt Chatservice.</span><span class="sxs-lookup"><span data-stu-id="03a64-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="03a64-169">Klicken Sie mit der rechten Maustaste auf die Rolle signalrchat, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="03a64-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="03a64-170">Wählen Sie die Registerkarte **Konfiguration** aus. Wählen Sie unter **Instanzen** 2 aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="03a64-171">Sie können auch die Größe des virtuellen Computers auf " **Extra Small**" festlegen.</span><span class="sxs-lookup"><span data-stu-id="03a64-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="03a64-172">Speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="03a64-172">Save the changes.</span></span>

<span data-ttu-id="03a64-173">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt Chatservice.</span><span class="sxs-lookup"><span data-stu-id="03a64-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="03a64-174">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="03a64-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="03a64-175">Wenn Sie das erste Mal in Windows Azure veröffentlichen, müssen Sie Ihre Anmelde Informationen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="03a64-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="03a64-176">Klicken Sie im **Veröffentlichungs** -Assistenten auf "anmelden, um Anmelde Informationen herunterzuladen".</span><span class="sxs-lookup"><span data-stu-id="03a64-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="03a64-177">Dadurch werden Sie aufgefordert, sich bei der Windows-Azure-Portal anzumelden und eine Datei mit Veröffentlichungs Einstellungen herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="03a64-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="03a64-178">Klicken Sie auf **importieren** , und wählen Sie die heruntergeladene Datei mit den Veröffentlichungs Einstellungen aus.</span><span class="sxs-lookup"><span data-stu-id="03a64-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="03a64-179">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="03a64-179">Click **Next**.</span></span> <span data-ttu-id="03a64-180">Wählen Sie im Dialogfeld **Veröffentlichungs Einstellungen** unter **clouddienst**den clouddienst aus, den Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="03a64-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="03a64-181">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="03a64-181">Click **Publish**.</span></span> <span data-ttu-id="03a64-182">Es kann einige Minuten dauern, bis die Anwendung bereitgestellt und die VMs gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="03a64-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="03a64-183">Wenn Sie nun die Chatanwendung ausführen, kommunizieren die Rollen Instanzen über Azure Service Bus mithilfe eines Service Bus Themas.</span><span class="sxs-lookup"><span data-stu-id="03a64-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="03a64-184">Ein Thema ist eine Nachrichten Warteschlange, die mehrere Abonnenten zulässt.</span><span class="sxs-lookup"><span data-stu-id="03a64-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="03a64-185">Die Rückwand erstellt automatisch das Thema und die Abonnements.</span><span class="sxs-lookup"><span data-stu-id="03a64-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="03a64-186">Um die Abonnements und Nachrichten Aktivität anzuzeigen, öffnen Sie die Azure-Portal, wählen Sie den Service Bus-Namespace aus, und klicken Sie auf "Topics".</span><span class="sxs-lookup"><span data-stu-id="03a64-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="03a64-187">Es dauert einige Minuten, bis die Nachrichten Aktivität im Dashboard angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="03a64-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="03a64-188">Signalr verwaltet die Themen Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="03a64-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="03a64-189">Wenn Ihre Anwendung bereitgestellt ist, versuchen Sie nicht, die Themen manuell zu löschen oder die Einstellungen für das Thema zu ändern.</span><span class="sxs-lookup"><span data-stu-id="03a64-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="03a64-190">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="03a64-190">Troubleshooting</span></span>

<span data-ttu-id="03a64-191">**System. InvalidOperationException "der einzige unterstützte IsolationLevel ist ' IsolationLevel. serialisierbar '."**</span><span class="sxs-lookup"><span data-stu-id="03a64-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="03a64-192">Dieser Fehler kann auftreten, wenn die Transaktionsebene für einen Vorgang auf einen anderen Wert als `Serializable`festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="03a64-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="03a64-193">Vergewissern Sie sich, dass keine Vorgänge mit anderen Transaktions Ebenen ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="03a64-193">Verify that no operations are being performed with other transaction levels.</span></span>
