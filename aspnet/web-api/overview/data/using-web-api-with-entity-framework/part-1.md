---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Verwenden der Web-API 2 mit Entity Framework 6 | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Erstellung einer Webanwendung mit einem ASP.net-Web-API-Back-End. Das Tutorial verwendet Entity Framework 6 für die Daten...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504837"
---
# <a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="e251f-104">Verwendung von Web-API 2 mit Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e251f-104">Using Web API 2 with Entity Framework 6</span></span>

[<span data-ttu-id="e251f-105">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="e251f-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="e251f-106">Dieses Tutorial vermittelt Ihnen die Grundlagen der Erstellung einer Webanwendung mit einem ASP.net-Web-API-Back-End.</span><span class="sxs-lookup"><span data-stu-id="e251f-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="e251f-107">Das Tutorial verwendet Entity Framework 6 für die Datenschicht und Knockout. js für die Client seitige JavaScript-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="e251f-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="e251f-108">Das Tutorial zeigt außerdem, wie Sie die APP für Azure App Service Web-Apps bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e251f-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e251f-109">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="e251f-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="e251f-110">Web-API 2,1</span><span class="sxs-lookup"><span data-stu-id="e251f-110">Web API 2.1</span></span>
> - <span data-ttu-id="e251f-111">Visual Studio 2017 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)herunterladen)</span><span class="sxs-lookup"><span data-stu-id="e251f-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="e251f-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e251f-112">Entity Framework 6</span></span>
> - <span data-ttu-id="e251f-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="e251f-113">.NET 4.7</span></span>
> - <span data-ttu-id="e251f-114">[Knockout. js](http://knockoutjs.com/) 3,1</span><span class="sxs-lookup"><span data-stu-id="e251f-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="e251f-115">In diesem Tutorial wird ASP.net-Web-API 2 mit Entity Framework 6 verwendet, um eine Webanwendung zu erstellen, die eine Back-End-Datenbank bearbeitet.</span><span class="sxs-lookup"><span data-stu-id="e251f-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="e251f-116">Im folgenden finden Sie einen Screenshot der Anwendung, die Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="e251f-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="e251f-117">Die APP verwendet einen Single-Page-Anwendungs Entwurf (Single-Page Application, Spa).</span><span class="sxs-lookup"><span data-stu-id="e251f-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="e251f-118">"Single-Page Application" ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt und dann die Seite dynamisch aktualisiert, anstatt neue Seiten zu laden.</span><span class="sxs-lookup"><span data-stu-id="e251f-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="e251f-119">Nach dem anfänglichen Laden der Seite kommuniziert die APP mit dem Server über AJAX-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="e251f-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="e251f-120">Die AJAX-Anforderungen geben JSON-Daten zurück, die die APP verwendet, um die Benutzeroberfläche zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="e251f-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="e251f-121">AJAX ist nicht neu, aber heute gibt es JavaScript-Frameworks, die das Erstellen und Verwalten einer großen anspruchsvollen Spa-Anwendung vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="e251f-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="e251f-122">In diesem Tutorial wird [Knockout. js](http://knockoutjs.com/)verwendet, Sie können jedoch ein beliebiges JavaScript-Client Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="e251f-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="e251f-123">Hier sind die wichtigsten Bausteine für diese APP:</span><span class="sxs-lookup"><span data-stu-id="e251f-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="e251f-124">ASP.NET MVC erstellt die HTML-Seite.</span><span class="sxs-lookup"><span data-stu-id="e251f-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="e251f-125">ASP.net-Web-API behandelt die AJAX-Anforderungen und gibt JSON-Daten zurück.</span><span class="sxs-lookup"><span data-stu-id="e251f-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="e251f-126">Knockout. js-Daten: bindet die HTML-Elemente an die JSON-Daten.</span><span class="sxs-lookup"><span data-stu-id="e251f-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="e251f-127">Entity Framework mit der-Datenbank kommuniziert.</span><span class="sxs-lookup"><span data-stu-id="e251f-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="e251f-128">Diese APP wird in Azure ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e251f-128">See this app running on Azure</span></span>

<span data-ttu-id="e251f-129">Möchten Sie sehen, dass die fertige Site als Live-Web-App ausgeführt wird?</span><span class="sxs-lookup"><span data-stu-id="e251f-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="e251f-130">Sie können eine vollständige Version der APP für Ihr Azure-Konto bereitstellen, indem Sie die folgende Schaltfläche auswählen.</span><span class="sxs-lookup"><span data-stu-id="e251f-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="e251f-131">Sie benötigen ein Azure-Konto, um diese Lösung in Azure bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="e251f-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="e251f-132">Wenn Sie noch nicht über ein Konto verfügen, haben Sie die folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="e251f-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="e251f-133">[Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.</span><span class="sxs-lookup"><span data-stu-id="e251f-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="e251f-134">[Aktivieren von MSDN-Abonnenten Vorteilen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : Ihr MSDN-Abonnement bietet Ihnen jeden Monat ein Guthaben, das Sie für kostenpflichtige Azure-Dienste nutzen können.</span><span class="sxs-lookup"><span data-stu-id="e251f-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="e251f-135">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="e251f-135">Create the project</span></span>

<span data-ttu-id="e251f-136">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e251f-136">Open Visual Studio.</span></span> <span data-ttu-id="e251f-137">Klicken Sie im Menü **Datei** auf **neu**, und wählen Sie dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="e251f-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="e251f-138">(Oder wählen Sie auf der Start Seite die Option **Neues Projekt** aus.)</span><span class="sxs-lookup"><span data-stu-id="e251f-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="e251f-139">Wählen Sie im Dialogfeld **Neues Projekt** im linken Bereich **Web** aus, und **ASP.NET-Webanwendung (.NET Framework)** im mittleren Bereich.</span><span class="sxs-lookup"><span data-stu-id="e251f-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="e251f-140">Benennen Sie das Projekt **BookService** , und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="e251f-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="e251f-141">Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die **Web-API** -Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="e251f-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

<span data-ttu-id="e251f-142">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e251f-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="e251f-143">Konfigurieren von Azure-Einstellungen (optional)</span><span class="sxs-lookup"><span data-stu-id="e251f-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="e251f-144">Nachdem Sie das Projekt erstellt haben, können Sie die Bereitstellung für die Azure App Service von Web-Apps jederzeit auswählen.</span><span class="sxs-lookup"><span data-stu-id="e251f-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="e251f-145">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="e251f-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="e251f-146">Klicken Sie im angezeigten Fenster auf **Start**.</span><span class="sxs-lookup"><span data-stu-id="e251f-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="e251f-147">Das Dialogfeld **Veröffentlichungsziel auswählen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e251f-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="e251f-148">Klicken Sie auf **Profil erstellen**.</span><span class="sxs-lookup"><span data-stu-id="e251f-148">Select **Create Profile**.</span></span> <span data-ttu-id="e251f-149">Das Dialogfeld **App Service erstellen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e251f-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="e251f-150">Übernehmen Sie die Standardwerte, oder geben Sie unterschiedliche Werte für Anwendungsname, Ressourcengruppe, Hostingplan, Azure-Abonnement und geografische Region ein.</span><span class="sxs-lookup"><span data-stu-id="e251f-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="e251f-151">Wählen Sie **SQL-Datenbank erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="e251f-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="e251f-152">Das Dialogfeld **SQL Server konfigurieren** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e251f-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="e251f-153">Akzeptieren Sie die Standardwerte, oder geben Sie andere Werte ein</span><span class="sxs-lookup"><span data-stu-id="e251f-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="e251f-154">Geben Sie einen **Administrator Benutzernamen** und ein **Administrator Kennwort** für die neue Datenbank ein.</span><span class="sxs-lookup"><span data-stu-id="e251f-154">Enter an **Administrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="e251f-155">Wählen Sie **OK** aus, wenn Sie fertig sind.</span><span class="sxs-lookup"><span data-stu-id="e251f-155">Select **OK** when you're done.</span></span> <span data-ttu-id="e251f-156">Die Seite **App Service erstellen** wird erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e251f-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="e251f-157">Wählen Sie **Erstellen** aus, um Ihr Profil zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e251f-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="e251f-158">In der unteren rechten Ecke wird eine Meldung angezeigt, die anzeigt, dass die Bereitstellung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e251f-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="e251f-159">Nach kurzer Zeit wird das Fenster " **veröffentlichen** " erneut angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e251f-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="e251f-160">Das Profil, das Sie zum Bereitstellen der App erstellt haben, ist jetzt verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e251f-160">The profile you created to deploy the app is now available.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="e251f-161">Weiter</span><span class="sxs-lookup"><span data-stu-id="e251f-161">Next</span></span>](part-2.md)
