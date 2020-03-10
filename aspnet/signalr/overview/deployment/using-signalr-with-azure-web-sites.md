---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Verwenden von signalr mit Web-Apps in Azure App Service | Microsoft-Dokumentation
author: bradygaster
description: In diesem Dokument wird beschrieben, wie Sie eine signalr-Anwendung konfigurieren, die auf Microsoft Azure ausgeführt wird. Im Tutorial verwendete Software Versionen Visual Studio 2013 oder VIS...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450183"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="1a76f-104">Verwenden von SignalR mit Web-Apps in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1a76f-104">Using SignalR with Web Apps in Azure App Service</span></span>

<span data-ttu-id="1a76f-105">von [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1a76f-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="1a76f-106">In diesem Dokument wird beschrieben, wie Sie eine signalr-Anwendung konfigurieren, die auf Microsoft Azure ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1a76f-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1a76f-107">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="1a76f-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="1a76f-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) oder Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1a76f-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="1a76f-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1a76f-109">.NET 4.5</span></span>
> - <span data-ttu-id="1a76f-110">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="1a76f-110">SignalR version 2</span></span>
> - <span data-ttu-id="1a76f-111">Azure SDK 2,3 für Visual Studio 2013 oder 2012</span><span class="sxs-lookup"><span data-stu-id="1a76f-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="1a76f-112">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="1a76f-112">Questions and comments</span></span>
>
> <span data-ttu-id="1a76f-113">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="1a76f-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="1a76f-114">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), in [StackOverflow.com](http://stackoverflow.com/)oder in den [Microsoft Azure Foren](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="1a76f-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="1a76f-115">Inhaltsverzeichnis</span><span class="sxs-lookup"><span data-stu-id="1a76f-115">Table of Contents</span></span>

- [<span data-ttu-id="1a76f-116">Einführung</span><span class="sxs-lookup"><span data-stu-id="1a76f-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="1a76f-117">Bereitstellen einer signalr-Web-App für Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1a76f-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="1a76f-118">Aktivieren von websockets auf Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1a76f-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="1a76f-119">Verwenden der Azure redis Cache Backplane</span><span class="sxs-lookup"><span data-stu-id="1a76f-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="1a76f-120">Next Steps</span><span class="sxs-lookup"><span data-stu-id="1a76f-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="1a76f-121">Einführung</span><span class="sxs-lookup"><span data-stu-id="1a76f-121">Introduction</span></span>

<span data-ttu-id="1a76f-122">ASP.net signalr kann verwendet werden, um eine neue Ebene der Interaktivität zwischen Servern und Web-oder .NET-Clients zu bieten.</span><span class="sxs-lookup"><span data-stu-id="1a76f-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="1a76f-123">Wenn Sie in Azure gehostet werden, können signalr-Anwendungen die hoch verfügbare, skalierbare und leistungsfähige Umgebung nutzen, die in der Cloud ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1a76f-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="1a76f-124">Bereitstellen einer signalr-Web-App für Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1a76f-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="1a76f-125">Signalr fügt keine speziellen Komplikationen zum Bereitstellen einer Anwendung in Azure und zum Bereitstellen auf einem lokalen Server hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a76f-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="1a76f-126">Eine Anwendung, die signalr verwendet, kann in Azure ohne Änderungen an der Konfiguration oder anderen Einstellungen gehostet werden (für die websocketunterstützung finden Sie unter [Aktivieren von websockets auf Azure App Service weiter](#websocket) unten). In diesem Tutorial stellen Sie die Anwendung bereit, die Sie im [Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) zu den ersten Schritten in Azure erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="1a76f-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="1a76f-127">**Erforderliche Komponenten**</span><span class="sxs-lookup"><span data-stu-id="1a76f-127">**Prerequisites**</span></span>

- <span data-ttu-id="1a76f-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1a76f-128">Visual Studio 2013.</span></span> <span data-ttu-id="1a76f-129">Wenn Sie nicht über Visual Studio verfügen, ist Visual Studio 2013 Express für Web in der Installation des Azure SDK enthalten.</span><span class="sxs-lookup"><span data-stu-id="1a76f-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="1a76f-130">[Azure SDK 2,3 für Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) oder [Azure SDK 2,3 für Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="1a76f-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="1a76f-131">Sie benötigen ein Azure-Abonnement, um dieses Lernprogramm auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1a76f-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="1a76f-132">Sie können [Ihre MSDN-Abonnenten Vorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)oder [sich für ein Testabonnement registrieren](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a76f-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="1a76f-133">Bereitstellen einer signalr-Web-App in Azure</span><span class="sxs-lookup"><span data-stu-id="1a76f-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="1a76f-134">Vervollständigen Sie das [Tutorial "Getting Started](../getting-started/tutorial-getting-started-with-signalr.md)", oder laden Sie das fertige Projekt aus der [Code Galerie](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)herunter.</span><span class="sxs-lookup"><span data-stu-id="1a76f-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="1a76f-135">Wählen Sie in Visual Studio die Option **Erstellen**und dann **signalr Chat veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="1a76f-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="1a76f-136">Wählen Sie im Dialogfeld "Web veröffentlichen" die Option "Windows Azure-Websites" aus.</span><span class="sxs-lookup"><span data-stu-id="1a76f-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Auswählen von Azure-Websites](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="1a76f-138">Wenn Sie nicht bei Ihrem Microsoft-Konto angemeldet sind, klicken Sie im Dialogfeld "vorhandene Website auswählen" auf **anmelden...** , und melden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="1a76f-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Vorhandene Website auswählen](using-signalr-with-azure-web-sites/_static/image2.png)    ![Anmelden bei Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="1a76f-141">Klicken Sie im Dialogfeld "vorhandene Website auswählen" auf **neu**.</span><span class="sxs-lookup"><span data-stu-id="1a76f-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Neue Website](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="1a76f-143">Geben Sie im Dialogfeld "Website in Windows Azure erstellen" einen eindeutigen APP-Namen ein.</span><span class="sxs-lookup"><span data-stu-id="1a76f-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="1a76f-144">Wählen Sie in der Dropdown Liste Region die nächstgelegene Region aus.</span><span class="sxs-lookup"><span data-stu-id="1a76f-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="1a76f-145">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="1a76f-145">Click **Create**.</span></span>

    ![Site auf Azure erstellen](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="1a76f-147">Klicken Sie im Dialogfeld "Web veröffentlichen" auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1a76f-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Website veröffentlichen](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="1a76f-149">Wenn die Veröffentlichung der APP abgeschlossen ist, wird die signalr Chat-Anwendung, die in Azure App Service-Web-Apps gehostet wird, in einem Browser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="1a76f-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Öffnen von Websites in einem Browser](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="1a76f-151">Aktivieren von websockets für Azure App Service-Web-Apps</span><span class="sxs-lookup"><span data-stu-id="1a76f-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="1a76f-152">Websockets müssen in Ihrer Web-App explizit aktiviert sein, damit Sie in einer signalr-Anwendung verwendet werden können. Andernfalls werden andere Protokolle verwendet (Weitere Informationen finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports) ).</span><span class="sxs-lookup"><span data-stu-id="1a76f-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="1a76f-153">Um websockets für Azure App Service Web-Apps zu verwenden, aktivieren Sie diese im Konfigurations Abschnitt der Web-App.</span><span class="sxs-lookup"><span data-stu-id="1a76f-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="1a76f-154">Öffnen Sie hierzu Ihre Web-App im Azure- [Verwaltungsportal](https://manage.windowsazure.com/), und wählen Sie konfigurieren aus.</span><span class="sxs-lookup"><span data-stu-id="1a76f-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Registerkarte „Konfigurieren“](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="1a76f-156">Stellen Sie oben auf der Konfigurationsseite sicher, dass .NET 4,5 für Ihre Web-App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="1a76f-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Einstellung der .NET Framework-Version 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="1a76f-158">Wählen Sie auf der Seite Konfiguration in der Einstellung **websockets** die Option **ein aus.**</span><span class="sxs-lookup"><span data-stu-id="1a76f-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Websockets-Einstellung: ein](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="1a76f-160">Wählen Sie unten auf der Seite Konfiguration die Option **Speichern** aus, um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="1a76f-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Einstellungen speichern](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="1a76f-162">Verwenden der Azure redis Cache Backplane</span><span class="sxs-lookup"><span data-stu-id="1a76f-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="1a76f-163">Wenn Sie für Ihre Web-App mehrere Instanzen verwenden und die Benutzer dieser Instanzen miteinander interagieren müssen (damit beispielsweise Chat-Nachrichten, die in einer Instanz erstellt werden, die mit anderen Instanzen verbundenen Benutzer erreichen können), muss die [Azure redis Cache Rückwand](../performance/scaleout-with-redis.md) in der Anwendung implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="1a76f-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="1a76f-164">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="1a76f-164">Next Steps</span></span>

<span data-ttu-id="1a76f-165">Weitere Informationen zu Web-Apps in Azure App Service finden Sie unter [Übersicht über Web-Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="1a76f-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
