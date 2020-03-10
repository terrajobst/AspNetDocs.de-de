---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Horizontale Skalierung von signalr mit Azure Service Bus (signalr 1. x) | Microsoft-Dokumentation
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449937"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="9c98b-102">Horizontale Skalierung in SignalR mit Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="9c98b-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>

<span data-ttu-id="9c98b-103">von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="9c98b-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="9c98b-104">In diesem Tutorial stellen Sie eine signalr-Anwendung für eine Windows Azure-webrolle bereit und verwenden die Service Bus Rückwand, um Nachrichten an jede Rollen Instanz zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="9c98b-105">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="9c98b-105">Prerequisites:</span></span>

- <span data-ttu-id="9c98b-106">Ein Windows Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="9c98b-106">A Windows Azure account.</span></span>
- <span data-ttu-id="9c98b-107">Das [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="9c98b-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="9c98b-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9c98b-108">Visual Studio 2012.</span></span>

<span data-ttu-id="9c98b-109">Die Service Bus-Rückwand ist auch mit [Service Bus für Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1,1, kompatibel.</span><span class="sxs-lookup"><span data-stu-id="9c98b-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="9c98b-110">Es ist jedoch nicht kompatibel mit Version 1,0 von Service Bus für Windows Server.</span><span class="sxs-lookup"><span data-stu-id="9c98b-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="9c98b-111">Preise</span><span class="sxs-lookup"><span data-stu-id="9c98b-111">Pricing</span></span>

<span data-ttu-id="9c98b-112">Die Service Bus Rückwand verwendet Themen zum Senden von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="9c98b-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="9c98b-113">Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="9c98b-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="9c98b-114">Zum Zeitpunkt der Erstellung dieses Artikels können Sie 1 Million Nachrichten pro Monat für weniger als $1 senden.</span><span class="sxs-lookup"><span data-stu-id="9c98b-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="9c98b-115">Die Rückwand sendet eine Service Bus-Nachricht für jeden Aufruf einer signalr-hubmethode.</span><span class="sxs-lookup"><span data-stu-id="9c98b-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="9c98b-116">Außerdem gibt es einige Steuerungs Meldungen für Verbindungen, Trennungen, das beitreten zu Gruppen und so weiter.</span><span class="sxs-lookup"><span data-stu-id="9c98b-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="9c98b-117">In den meisten Anwendungen ist der Großteil des Nachrichtenverkehrs hubmethoden Aufrufe.</span><span class="sxs-lookup"><span data-stu-id="9c98b-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="9c98b-118">Übersicht</span><span class="sxs-lookup"><span data-stu-id="9c98b-118">Overview</span></span>

<span data-ttu-id="9c98b-119">Bevor wir zum detaillierten Tutorial gelangen, finden Sie hier einen kurzen Überblick über die Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="9c98b-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="9c98b-120">Verwenden Sie das Windows-Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="9c98b-121">Fügen Sie die folgenden nuget-Pakete zu Ihrer Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="9c98b-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="9c98b-122">Microsoft. Aspnet. signalr</span><span class="sxs-lookup"><span data-stu-id="9c98b-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="9c98b-123">Microsoft. Aspnet. signalr. ServiceBus</span><span class="sxs-lookup"><span data-stu-id="9c98b-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="9c98b-124">Erstellen Sie eine signalr-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9c98b-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="9c98b-125">Fügen Sie "Global. asax" den folgenden Code hinzu, um die Backplane zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="9c98b-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="9c98b-126">Wählen Sie für jede Anwendung einen anderen Wert für "yourappname" aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="9c98b-127">Verwenden Sie nicht denselben Wert für mehrere Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="9c98b-128">Erstellen der Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="9c98b-128">Create the Azure Services</span></span>

<span data-ttu-id="9c98b-129">Erstellen Sie einen clouddienst, wie unter [Erstellen und Bereitstellen eines clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="9c98b-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="9c98b-130">Befolgen Sie die Schritte im Abschnitt "Gewusst wie: Erstellen eines clouddiensts mit schneller Fassung".</span><span class="sxs-lookup"><span data-stu-id="9c98b-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="9c98b-131">Für dieses Tutorial müssen Sie kein Zertifikat hochladen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="9c98b-132">Erstellen Sie einen neuen Service Bus Namespace, wie unter Vorgehens [Weise beim Verwenden von Service Bus Themen/Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)beschrieben.</span><span class="sxs-lookup"><span data-stu-id="9c98b-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="9c98b-133">Befolgen Sie die Schritte im Abschnitt "Erstellen eines Dienst Namespace".</span><span class="sxs-lookup"><span data-stu-id="9c98b-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="9c98b-134">Stellen Sie sicher, dass Sie die gleiche Region für den clouddienst und den Service Bus-Namespace auswählen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="9c98b-135">Erstellen des Visual Studio-Projekts</span><span class="sxs-lookup"><span data-stu-id="9c98b-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="9c98b-136">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c98b-136">Start Visual Studio.</span></span> <span data-ttu-id="9c98b-137">Klicken Sie im Menü **Datei** auf **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="9c98b-138">Erweitern Sie im Dialogfeld **Neues Projekt** den **Eintrag C#Visual** .</span><span class="sxs-lookup"><span data-stu-id="9c98b-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="9c98b-139">Wählen Sie unter **installierte Vorlagen**die Option **Cloud** , und wählen Sie dann **Windows Azure-clouddienst**aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="9c98b-140">Behalten Sie die Standard .NET Framework 4,5 bei.</span><span class="sxs-lookup"><span data-stu-id="9c98b-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="9c98b-141">Nennen Sie die Anwendung Chatservice, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="9c98b-142">Wählen Sie im Dialogfeld **neuer Windows Azure-clouddienst** die Option ASP.NET MVC 4 Web Role aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="9c98b-143">Klicken Sie auf die Schaltfläche mit dem Pfeil nach rechts ( **&gt;** ), um die Rolle der Projekt Mappe hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="9c98b-144">Zeigen Sie mit der Maus auf die neue Rolle, damit das Stift Symbol sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="9c98b-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="9c98b-145">Klicken Sie auf dieses Symbol, um die Rolle umzubenennen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-145">Click this icon to rename the role.</span></span> <span data-ttu-id="9c98b-146">Benennen Sie die Rolle "signalrchat", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="9c98b-147">Wählen Sie im Assistenten für **neue ASP.NET MVC 4-Projekte** die Option **Internet Anwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="9c98b-148">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-148">Click **OK**.</span></span> <span data-ttu-id="9c98b-149">Der Projekt-Assistent erstellt zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="9c98b-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="9c98b-150">Chatservice: dieses Projekt ist die Windows Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9c98b-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="9c98b-151">Dabei werden die Azure-Rollen und andere Konfigurationsoptionen definiert.</span><span class="sxs-lookup"><span data-stu-id="9c98b-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="9c98b-152">Signalrchat: dieses Projekt ist Ihr ASP.NET MVC 4-Projekt.</span><span class="sxs-lookup"><span data-stu-id="9c98b-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="9c98b-153">Erstellen der signalr Chat-Anwendung</span><span class="sxs-lookup"><span data-stu-id="9c98b-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="9c98b-154">Um die Chat-Anwendung zu erstellen, befolgen Sie die Schritte im Tutorial erstellen [von signalr und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="9c98b-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="9c98b-155">Verwenden Sie nuget, um die erforderlichen Bibliotheken zu installieren.</span><span class="sxs-lookup"><span data-stu-id="9c98b-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="9c98b-156">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="9c98b-157">Geben Sie im Fenster **Paket-Manager-Konsole** die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="9c98b-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="9c98b-158">Verwenden Sie die Option `-ProjectName`, um die Pakete für das MVC-Projekt ASP.net anstelle des Windows Azure-Projekts zu installieren.</span><span class="sxs-lookup"><span data-stu-id="9c98b-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="9c98b-159">Konfigurieren der Backplane</span><span class="sxs-lookup"><span data-stu-id="9c98b-159">Configure the Backplane</span></span>

<span data-ttu-id="9c98b-160">Fügen Sie in der Datei "Global. asax" Ihrer Anwendung den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="9c98b-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="9c98b-161">Nun müssen Sie Ihre Service Bus-Verbindungs Zeichenfolge erhalten.</span><span class="sxs-lookup"><span data-stu-id="9c98b-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="9c98b-162">Wählen Sie im Azure-Portal den Service Bus-Namespace aus, den Sie erstellt haben, und klicken Sie auf das Symbol Zugriffsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="9c98b-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="9c98b-163">Kopieren Sie die Verbindungs Zeichenfolge in die Zwischenablage, und fügen Sie Sie in die *ConnectionString* -Variable ein.</span><span class="sxs-lookup"><span data-stu-id="9c98b-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="9c98b-164">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="9c98b-164">Deploy to Azure</span></span>

<span data-ttu-id="9c98b-165">Erweitern Sie in Projektmappen-Explorer den Ordner **Rollen** im Projekt Chatservice.</span><span class="sxs-lookup"><span data-stu-id="9c98b-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="9c98b-166">Klicken Sie mit der rechten Maustaste auf die Rolle signalrchat, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="9c98b-167">Wählen Sie die Registerkarte **Konfiguration** aus. Wählen Sie unter **Instanzen** 2 aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="9c98b-168">Sie können auch die Größe des virtuellen Computers auf " **Extra Small**" festlegen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="9c98b-169">Speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-169">Save the changes.</span></span>

<span data-ttu-id="9c98b-170">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt Chatservice.</span><span class="sxs-lookup"><span data-stu-id="9c98b-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="9c98b-171">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="9c98b-172">Wenn Sie das erste Mal in Windows Azure veröffentlichen, müssen Sie Ihre Anmelde Informationen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="9c98b-173">Klicken Sie im **Veröffentlichungs** -Assistenten auf "anmelden, um Anmelde Informationen herunterzuladen".</span><span class="sxs-lookup"><span data-stu-id="9c98b-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="9c98b-174">Dadurch werden Sie aufgefordert, sich bei der Windows-Azure-Portal anzumelden und eine Datei mit Veröffentlichungs Einstellungen herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="9c98b-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="9c98b-175">Klicken Sie auf **importieren** , und wählen Sie die heruntergeladene Datei mit den Veröffentlichungs Einstellungen aus.</span><span class="sxs-lookup"><span data-stu-id="9c98b-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="9c98b-176">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-176">Click **Next**.</span></span> <span data-ttu-id="9c98b-177">Wählen Sie im Dialogfeld **Veröffentlichungs Einstellungen** unter **clouddienst**den clouddienst aus, den Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="9c98b-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="9c98b-178">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="9c98b-178">Click **Publish**.</span></span> <span data-ttu-id="9c98b-179">Es kann einige Minuten dauern, bis die Anwendung bereitgestellt und die VMs gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="9c98b-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="9c98b-180">Wenn Sie nun die Chatanwendung ausführen, kommunizieren die Rollen Instanzen über Azure Service Bus mithilfe eines Service Bus Themas.</span><span class="sxs-lookup"><span data-stu-id="9c98b-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="9c98b-181">Ein Thema ist eine Nachrichten Warteschlange, die mehrere Abonnenten zulässt.</span><span class="sxs-lookup"><span data-stu-id="9c98b-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="9c98b-182">Die Rückwand erstellt automatisch das Thema und die Abonnements.</span><span class="sxs-lookup"><span data-stu-id="9c98b-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="9c98b-183">Um die Abonnements und Nachrichten Aktivität anzuzeigen, öffnen Sie die Azure-Portal, wählen Sie den Service Bus-Namespace aus, und klicken Sie auf "Topics".</span><span class="sxs-lookup"><span data-stu-id="9c98b-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="9c98b-184">Es dauert einige Minuten, bis die Nachrichten Aktivität im Dashboard angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="9c98b-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="9c98b-185">Signalr verwaltet die Themen Lebensdauer.</span><span class="sxs-lookup"><span data-stu-id="9c98b-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="9c98b-186">Wenn Ihre Anwendung bereitgestellt ist, versuchen Sie nicht, die Themen manuell zu löschen oder die Einstellungen für das Thema zu ändern.</span><span class="sxs-lookup"><span data-stu-id="9c98b-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
