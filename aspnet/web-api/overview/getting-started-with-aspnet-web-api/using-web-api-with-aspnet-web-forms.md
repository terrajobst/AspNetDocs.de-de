---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Mithilfe von Web-API mit ASP.NET Web Forms - ASP.NET 4.x
author: MikeWasson
description: Tutorial mit Code Schritt für Schritt eine ASP.NET Forms-Anwendung für ASP.NET Web-API hinzugefügt 4.x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422576"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="b667c-103">Verwenden der Web-API mit ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="b667c-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="b667c-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b667c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b667c-105">Dieses Tutorial führt Sie durch die Schritte zum Hinzufügen von Web-API zu einer herkömmlichen ASP.NET Web Forms-Anwendung in ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="b667c-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="b667c-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b667c-106">Overview</span></span>

<span data-ttu-id="b667c-107">Obwohl ASP.NET Web-API mit ASP.NET MVC gepackt wurde, ist es einfach, eine herkömmliche ASP.NET Web Forms-Anwendung-Web-API hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b667c-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="b667c-108">Um Web-API in einer Web Forms-Anwendung verwenden zu können, gibt es zwei Hauptschritte:</span><span class="sxs-lookup"><span data-stu-id="b667c-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="b667c-109">Fügen Sie einen Web-API-Controller, die von abgeleitet der **ApiController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="b667c-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="b667c-110">Fügen Sie eine Routingtabelle die **Anwendung\_starten** Methode.</span><span class="sxs-lookup"><span data-stu-id="b667c-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="b667c-111">Erstellen einer Web Forms-Projekt</span><span class="sxs-lookup"><span data-stu-id="b667c-111">Create a Web Forms Project</span></span>

<span data-ttu-id="b667c-112">Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="b667c-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="b667c-113">Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="b667c-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b667c-114">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="b667c-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="b667c-115">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="b667c-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b667c-116">Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET Web Forms-Anwendung**.</span><span class="sxs-lookup"><span data-stu-id="b667c-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="b667c-117">Geben Sie einen Namen für das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b667c-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="b667c-118">Erstellen Sie das Modell und Controller</span><span class="sxs-lookup"><span data-stu-id="b667c-118">Create the Model and Controller</span></span>

<span data-ttu-id="b667c-119">In diesem Tutorial verwendet die gleichen Modell und Controller-Klassen als die [Einstieg](tutorial-your-first-web-api.md) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="b667c-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="b667c-120">Fügen Sie zunächst eine Modellklasse.</span><span class="sxs-lookup"><span data-stu-id="b667c-120">First, add a model class.</span></span> <span data-ttu-id="b667c-121">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Klasse hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b667c-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="b667c-122">Benennen Sie die Product-Klasse, und fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="b667c-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="b667c-123">Fügen Sie einen Web-API-Controller zum Projekt., ein *Controller* ist das Objekt, das HTTP-Anforderungen für Web-API behandelt.</span><span class="sxs-lookup"><span data-stu-id="b667c-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="b667c-124">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt.</span><span class="sxs-lookup"><span data-stu-id="b667c-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="b667c-125">Wählen Sie **neues Element hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b667c-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="b667c-126">Klicken Sie unter **installierte Vorlagen**, erweitern Sie **Visual C#-** , und wählen Sie **Web**.</span><span class="sxs-lookup"><span data-stu-id="b667c-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="b667c-127">Wählen Sie dann aus der Liste der Vorlagen, **Web API Controller Class**.</span><span class="sxs-lookup"><span data-stu-id="b667c-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="b667c-128">Nennen Sie den Controller "ProductsController", und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b667c-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="b667c-129">Die **neues Element hinzufügen** Assistenten wird eine Datei namens ProductsController.cs erstellt.</span><span class="sxs-lookup"><span data-stu-id="b667c-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="b667c-130">Löschen Sie die Methoden, die der Assistent enthalten, und fügen Sie die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="b667c-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="b667c-131">Weitere Informationen zu den Code in diesem Controller, finden Sie unter den [Einstieg](tutorial-your-first-web-api.md) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="b667c-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="b667c-132">Hinzufügen von Routinginformationen</span><span class="sxs-lookup"><span data-stu-id="b667c-132">Add Routing Information</span></span>

<span data-ttu-id="b667c-133">Anschließend wir fügen eine URI-Route also diese URIs der Form &quot;/API/Produkte/&quot; an den Controller weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="b667c-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="b667c-134">In **Projektmappen-Explorer**, doppelklicken Sie auf "Global.asax", um den Code-Behind-Datei "Global.asax.cs" zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b667c-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="b667c-135">Fügen Sie die folgenden **mit** Anweisung.</span><span class="sxs-lookup"><span data-stu-id="b667c-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="b667c-136">Klicken Sie dann fügen Sie den folgenden Code der **Anwendung\_starten** Methode:</span><span class="sxs-lookup"><span data-stu-id="b667c-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="b667c-137">Weitere Informationen zu Routingtabellen finden Sie unter [Routing in ASP.NET Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b667c-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="b667c-138">Hinzufügen der clientseitigen AJAX</span><span class="sxs-lookup"><span data-stu-id="b667c-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="b667c-139">Das ist alles, die müssen Sie eine Web-API zu erstellen, die Clients zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="b667c-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="b667c-140">Nun fügen Sie eine HTML-Seite, die jQuery zum Aufrufen der API verwendet.</span><span class="sxs-lookup"><span data-stu-id="b667c-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="b667c-141">Stellen Sie sicher, dass die Masterseite (z. B. *Site.Master*) umfasst eine `ContentPlaceHolder` mit `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="b667c-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="b667c-142">Öffnen Sie die Datei "default.aspx".</span><span class="sxs-lookup"><span data-stu-id="b667c-142">Open the file Default.aspx.</span></span> <span data-ttu-id="b667c-143">Ersetzen Sie den Standardtext, der im Abschnitt Inhalt ist an, wie dargestellt:</span><span class="sxs-lookup"><span data-stu-id="b667c-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="b667c-144">Fügen Sie einen Verweis auf die jQuery-Quelldatei in die `HeaderContent` Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="b667c-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="b667c-145">Hinweis: Sie können problemlos den Skriptverweis hinzufügen, durch Ziehen und Ablegen der Datei aus **Projektmappen-Explorer** in das Fenster des Code-Editor.</span><span class="sxs-lookup"><span data-stu-id="b667c-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="b667c-146">Fügen Sie den nachstehenden Skriptbaustein unterhalb des jQuery-Skript-Tags hinzu:</span><span class="sxs-lookup"><span data-stu-id="b667c-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="b667c-147">Wenn das Dokument geladen wird, wird dieses Skript eine AJAX-Anforderung an &quot;-api/Produkte&quot;.</span><span class="sxs-lookup"><span data-stu-id="b667c-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="b667c-148">Die Anforderung gibt eine Liste der Produkte im JSON-Format zurück.</span><span class="sxs-lookup"><span data-stu-id="b667c-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="b667c-149">Das Skript fügt die Produktinformationen in den HTML-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="b667c-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="b667c-150">Wenn Sie die Anwendung ausführen, sollte es wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="b667c-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
