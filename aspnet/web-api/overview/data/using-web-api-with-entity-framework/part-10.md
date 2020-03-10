---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Veröffentlichen der app in Azure Azure App Service | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504759"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="54cee-102">Veröffentlichen der app in Azure Azure App Service</span><span class="sxs-lookup"><span data-stu-id="54cee-102">Publish the App to Azure Azure App Service</span></span>

<span data-ttu-id="54cee-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="54cee-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="54cee-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="54cee-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="54cee-105">Im letzten Schritt wird die Anwendung in Azure veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="54cee-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="54cee-106">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="54cee-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="54cee-107">Durch Klicken auf **veröffentlichen** wird das Dialogfeld **Web veröffentlichen** aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="54cee-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="54cee-108">Wenn Sie beim ersten Erstellen des Projekts den **Host in der Cloud** aktiviert haben, sind die Verbindung und die Einstellungen bereits konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="54cee-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="54cee-109">Klicken Sie in diesem Fall einfach auf die Registerkarte **Einstellungen** , und aktivieren Sie &quot;Code First-Migrationen&quot;ausführen.</span><span class="sxs-lookup"><span data-stu-id="54cee-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="54cee-110">(Wenn Sie den **Host in der Cloud** zu Beginn nicht überprüft haben, führen Sie die Schritte im [nächsten Abschnitt](#new-website)aus.)</span><span class="sxs-lookup"><span data-stu-id="54cee-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="54cee-111">Klicken Sie zum Bereitstellen der APP auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="54cee-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="54cee-112">Sie können den Veröffentlichungs Fortschritt im Fenster **Webveröffentlichungs Aktivität** anzeigen.</span><span class="sxs-lookup"><span data-stu-id="54cee-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="54cee-113">(Wählen Sie im Menü **Ansicht** die Option **Weitere Fenster**, und wählen Sie dann **Webveröffentlichungs Aktivität**aus.)</span><span class="sxs-lookup"><span data-stu-id="54cee-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="54cee-114">Wenn Visual Studio die Bereitstellung der APP abgeschlossen hat, öffnet der Standardbrowser automatisch die URL der bereitgestellten Website, und die von Ihnen erstellte Anwendung wird nun in der Cloud ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="54cee-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="54cee-115">Die URL in der Adressleiste des Browsers zeigt, dass die Website aus dem Internet geladen wird.</span><span class="sxs-lookup"><span data-stu-id="54cee-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="54cee-116">Bereitstellen auf einer neuen Website</span><span class="sxs-lookup"><span data-stu-id="54cee-116">Deploying to a New Website</span></span>

<span data-ttu-id="54cee-117">Wenn Sie den **Host in der Cloud** beim ersten Erstellen des Projekts nicht überprüft haben, können Sie jetzt eine neue Web-App konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="54cee-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="54cee-118">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="54cee-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="54cee-119">Wählen Sie die Registerkarte **Profil** , und klicken Sie auf **Microsoft Azure Websites**</span><span class="sxs-lookup"><span data-stu-id="54cee-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="54cee-120">Wenn Sie zurzeit nicht bei Azure angemeldet sind, werden Sie aufgefordert, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="54cee-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="54cee-121">Klicken Sie im Dialogfeld **vorhandene Websites** auf **neu**.</span><span class="sxs-lookup"><span data-stu-id="54cee-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="54cee-122">Geben Sie einen Website Namen ein.</span><span class="sxs-lookup"><span data-stu-id="54cee-122">Enter a site name.</span></span> <span data-ttu-id="54cee-123">Wählen Sie Ihr Azure-Abonnement und die Region aus.</span><span class="sxs-lookup"><span data-stu-id="54cee-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="54cee-124">Wählen Sie unter **Datenbankserver**die Option **neuen Server erstellen**aus, oder wählen Sie einen vorhandenen Server aus.</span><span class="sxs-lookup"><span data-stu-id="54cee-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="54cee-125">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="54cee-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="54cee-126">Klicken Sie auf die Registerkarte **Einstellungen** , und aktivieren Sie &quot;Code First-Migrationen&quot;ausführen.</span><span class="sxs-lookup"><span data-stu-id="54cee-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="54cee-127">Dann wird auf **Veröffentlichen** geklickt.</span><span class="sxs-lookup"><span data-stu-id="54cee-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="54cee-128">Previous</span><span class="sxs-lookup"><span data-stu-id="54cee-128">Previous</span></span>](part-9.md)
