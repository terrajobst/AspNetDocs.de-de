---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Verwenden der Web-API mit ASP.net Web Forms-ASP.NET 4. x
author: MikeWasson
description: Tutorial mit Code Schritt für Schritt zum Hinzufügen einer Web-API zu einer ASP.net Forms-Anwendung für ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448533"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="4cd28-103">Verwenden der Web-API mit ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="4cd28-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="4cd28-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4cd28-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4cd28-105">Dieses Tutorial führt Sie durch die Schritte zum Hinzufügen einer Web-API zu einer herkömmlichen ASP.net Web Forms-Anwendung in ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="4cd28-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="4cd28-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="4cd28-106">Overview</span></span>

<span data-ttu-id="4cd28-107">Obwohl ASP.net-Web-API mit ASP.NET MVC verpackt ist, ist es einfach, eine Web-API zu einer herkömmlichen ASP.net Web Forms-Anwendung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="4cd28-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="4cd28-108">Um die Web-API in einer Web Forms Anwendung zu verwenden, gibt es zwei Hauptschritte:</span><span class="sxs-lookup"><span data-stu-id="4cd28-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="4cd28-109">Fügt einen Web-API-Controller hinzu, der von der **apicontroller** -Klasse abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="4cd28-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="4cd28-110">Fügen Sie der **Anwendungs\_Start** -Methode eine Routentabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cd28-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="4cd28-111">Erstellen eines Web Forms Projekts</span><span class="sxs-lookup"><span data-stu-id="4cd28-111">Create a Web Forms Project</span></span>

<span data-ttu-id="4cd28-112">Starten Sie Visual Studio, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="4cd28-113">Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="4cd28-114">Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten.</span><span class="sxs-lookup"><span data-stu-id="4cd28-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="4cd28-115">Wählen Sie unter **Visualisierung C#** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="4cd28-116">Wählen Sie in der Liste der Projektvorlagen die Option **ASP.net Web Forms Anwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="4cd28-117">Geben Sie einen Namen für das Projekt ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cd28-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="4cd28-118">Erstellen des Modells und des Controllers</span><span class="sxs-lookup"><span data-stu-id="4cd28-118">Create the Model and Controller</span></span>

<span data-ttu-id="4cd28-119">In diesem Tutorial werden die gleichen Modell-und Controller Klassen wie im Tutorial " [Getting Started](tutorial-your-first-web-api.md) " verwendet.</span><span class="sxs-lookup"><span data-stu-id="4cd28-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="4cd28-120">Fügen Sie zunächst eine Modell Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cd28-120">First, add a model class.</span></span> <span data-ttu-id="4cd28-121">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="4cd28-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="4cd28-122">Benennen Sie die Klasse Product, und fügen Sie die folgende Implementierung ein:</span><span class="sxs-lookup"><span data-stu-id="4cd28-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="4cd28-123">Als Nächstes fügen Sie dem Projekt einen Web-API-Controller hinzu. ein *Controller* ist das Objekt, das HTTP-Anforderungen für die Web-API verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="4cd28-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="4cd28-124">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="4cd28-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="4cd28-125">Wählen Sie **Neues Element hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="4cd28-126">Erweitern Sie unter **installierte Vorlagen**die Option **Visualisierung C#**  , und wählen Sie **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="4cd28-127">Wählen Sie dann in der Liste der Vorlagen die **Web-API-Controller Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="4cd28-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="4cd28-128">Nennen Sie den Controller "ProductController", und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="4cd28-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="4cd28-129">Mit dem Assistenten zum **Hinzufügen eines neuen Elements** wird eine Datei namens ProductsController.cs erstellt.</span><span class="sxs-lookup"><span data-stu-id="4cd28-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="4cd28-130">Löschen Sie die Methoden, die der Assistent enthielt, und fügen Sie die folgenden Methoden hinzu:</span><span class="sxs-lookup"><span data-stu-id="4cd28-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="4cd28-131">Weitere Informationen zum Code in diesem Controller finden Sie im Tutorial [zu](tutorial-your-first-web-api.md) den ersten Schritten.</span><span class="sxs-lookup"><span data-stu-id="4cd28-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="4cd28-132">Routing Informationen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4cd28-132">Add Routing Information</span></span>

<span data-ttu-id="4cd28-133">Als Nächstes fügen wir eine URI-Route hinzu, sodass URIs der Form &quot;/API/Products/-&quot; an den Controller weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="4cd28-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="4cd28-134">Doppelklicken Sie in **Projektmappen-Explorer**auf Global. asax, um die Code Behind-Datei Global.asax.cs zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="4cd28-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="4cd28-135">Fügen Sie die folgende **using** -Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cd28-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="4cd28-136">Fügen Sie dann dem **Anwendungs\_Start** -Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="4cd28-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="4cd28-137">Weitere Informationen zu Routing Tabellen finden Sie unter [Routing in ASP.net-Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4cd28-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="4cd28-138">Client seitiges AJAX hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4cd28-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="4cd28-139">Das ist alles, was Sie benötigen, um eine Web-API zu erstellen, auf die Clients zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="4cd28-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="4cd28-140">Nun fügen wir eine HTML-Seite hinzu, die jQuery verwendet, um die API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="4cd28-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="4cd28-141">Stellen Sie sicher, dass Ihre Master Seite (z *. b. Site. Master*) eine `ContentPlaceHolder` mit `ID="HeadContent"`enthält:</span><span class="sxs-lookup"><span data-stu-id="4cd28-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="4cd28-142">Öffnen Sie die Datei default. aspx.</span><span class="sxs-lookup"><span data-stu-id="4cd28-142">Open the file Default.aspx.</span></span> <span data-ttu-id="4cd28-143">Ersetzen Sie den Text, der sich im Hauptinhalts Abschnitt befindet, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="4cd28-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="4cd28-144">Fügen Sie als nächstes im Abschnitt `HeaderContent` einen Verweis auf die jQuery-Quelldatei hinzu:</span><span class="sxs-lookup"><span data-stu-id="4cd28-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="4cd28-145">Hinweis: Sie können den Skript Verweis problemlos hinzufügen, indem Sie die Datei per Drag & Drop aus **Projektmappen-Explorer** in das Fenster "Code-Editor" verschieben.</span><span class="sxs-lookup"><span data-stu-id="4cd28-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="4cd28-146">Fügen Sie unterhalb des jQuery-Skripttags den folgenden Skriptblock hinzu:</span><span class="sxs-lookup"><span data-stu-id="4cd28-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="4cd28-147">Wenn das Dokument geladen wird, sendet dieses Skript eine AJAX-Anforderung an &quot;API/Produkte&quot;.</span><span class="sxs-lookup"><span data-stu-id="4cd28-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="4cd28-148">Die Anforderung gibt eine Liste von Produkten im JSON-Format zurück.</span><span class="sxs-lookup"><span data-stu-id="4cd28-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="4cd28-149">Das Skript fügt die Produktinformationen der HTML-Tabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cd28-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="4cd28-150">Wenn Sie die Anwendung ausführen, sollte Sie wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="4cd28-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
