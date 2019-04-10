---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: 'Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1: ASP.NET 4.x'
author: MikeWasson
description: Tutorial wird gezeigt, wie zur Unterstützung von CRUD-Vorgänge in einem HTTP-Dienst mithilfe von ASP.NET Web-API für ASP.NET 4.x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 855c3fa35d82173c87d13adb51e10fd13698ade5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381353"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="ca1db-103">Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1</span><span class="sxs-lookup"><span data-stu-id="ca1db-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="ca1db-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ca1db-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ca1db-105">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="ca1db-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="ca1db-106">In diesem Tutorial wird gezeigt, wie zur Unterstützung von CRUD-Vorgänge in einem HTTP-Dienst mithilfe von ASP.NET Web-API für ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ca1db-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ca1db-107">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ca1db-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ca1db-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ca1db-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="ca1db-109">Web-API 1 (Außerdem funktioniert mit Web-API 2)</span><span class="sxs-lookup"><span data-stu-id="ca1db-109">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="ca1db-110">CRUD steht für &quot;erstellen, lesen, aktualisieren und löschen,&quot; die sind die vier grundlegenden Datenbankvorgänge.</span><span class="sxs-lookup"><span data-stu-id="ca1db-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="ca1db-111">Viele HTTP-Dienste das Modell auch CRUD-Vorgänge über REST oder die REST-ähnliche APIs.</span><span class="sxs-lookup"><span data-stu-id="ca1db-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="ca1db-112">In diesem Tutorial erstellen Sie ein sehr einfaches Web-API, um eine Liste von Produkten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="ca1db-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="ca1db-113">Jedes Produkt, Preis, und Auswählen einer Kategorie enthält (z. B. &quot;Toys&quot; oder &quot;Hardware&quot;), sowie eine Produkt-ID.</span><span class="sxs-lookup"><span data-stu-id="ca1db-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="ca1db-114">Die API-Produkte werden folgende Methoden verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="ca1db-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="ca1db-115">Aktion</span><span class="sxs-lookup"><span data-stu-id="ca1db-115">Action</span></span> | <span data-ttu-id="ca1db-116">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="ca1db-116">HTTP method</span></span> | <span data-ttu-id="ca1db-117">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="ca1db-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ca1db-118">Abrufen einer Liste aller Produkte</span><span class="sxs-lookup"><span data-stu-id="ca1db-118">Get a list of all products</span></span> | <span data-ttu-id="ca1db-119">GET</span><span class="sxs-lookup"><span data-stu-id="ca1db-119">GET</span></span> | <span data-ttu-id="ca1db-120">/api/products</span><span class="sxs-lookup"><span data-stu-id="ca1db-120">/api/products</span></span> |
| <span data-ttu-id="ca1db-121">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="ca1db-121">Get a product by ID</span></span> | <span data-ttu-id="ca1db-122">GET</span><span class="sxs-lookup"><span data-stu-id="ca1db-122">GET</span></span> | <span data-ttu-id="ca1db-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ca1db-123">/api/products/*id*</span></span> |
| <span data-ttu-id="ca1db-124">Abrufen eines Produkts nach Kategorie</span><span class="sxs-lookup"><span data-stu-id="ca1db-124">Get a product by category</span></span> | <span data-ttu-id="ca1db-125">GET</span><span class="sxs-lookup"><span data-stu-id="ca1db-125">GET</span></span> | <span data-ttu-id="ca1db-126">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="ca1db-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="ca1db-127">Erstellen eines neuen Produkts</span><span class="sxs-lookup"><span data-stu-id="ca1db-127">Create a new product</span></span> | <span data-ttu-id="ca1db-128">POST</span><span class="sxs-lookup"><span data-stu-id="ca1db-128">POST</span></span> | <span data-ttu-id="ca1db-129">/api/products</span><span class="sxs-lookup"><span data-stu-id="ca1db-129">/api/products</span></span> |
| <span data-ttu-id="ca1db-130">Aktualisieren eines Produkts</span><span class="sxs-lookup"><span data-stu-id="ca1db-130">Update a product</span></span> | <span data-ttu-id="ca1db-131">PUT</span><span class="sxs-lookup"><span data-stu-id="ca1db-131">PUT</span></span> | <span data-ttu-id="ca1db-132">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ca1db-132">/api/products/*id*</span></span> |
| <span data-ttu-id="ca1db-133">Löschen eines Produkts</span><span class="sxs-lookup"><span data-stu-id="ca1db-133">Delete a product</span></span> | <span data-ttu-id="ca1db-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="ca1db-134">DELETE</span></span> | <span data-ttu-id="ca1db-135">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ca1db-135">/api/products/*id*</span></span> |

<span data-ttu-id="ca1db-136">Beachten Sie, dass einige der URIs die Produkt-ID im Pfad enthalten.</span><span class="sxs-lookup"><span data-stu-id="ca1db-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="ca1db-137">Um das Produkt zu erhalten, deren ID ist die 28, z. B. sendet der Client eine GET-Anforderung für `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="ca1db-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="ca1db-138">Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ca1db-138">Resources</span></span>

<span data-ttu-id="ca1db-139">Die API-Produkte werden URIs für zwei Ressourcentypen definiert:</span><span class="sxs-lookup"><span data-stu-id="ca1db-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="ca1db-140">Ressource</span><span class="sxs-lookup"><span data-stu-id="ca1db-140">Resource</span></span> | <span data-ttu-id="ca1db-141">URI</span><span class="sxs-lookup"><span data-stu-id="ca1db-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ca1db-142">Die Liste aller Produkte.</span><span class="sxs-lookup"><span data-stu-id="ca1db-142">The list of all the products.</span></span> | <span data-ttu-id="ca1db-143">/api/products</span><span class="sxs-lookup"><span data-stu-id="ca1db-143">/api/products</span></span> |
| <span data-ttu-id="ca1db-144">Ein einzelnes Produkt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-144">An individual product.</span></span> | <span data-ttu-id="ca1db-145">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ca1db-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="ca1db-146">Methoden</span><span class="sxs-lookup"><span data-stu-id="ca1db-146">Methods</span></span>

<span data-ttu-id="ca1db-147">Die vier wichtigsten HTTP-Methoden (GET, PUT, POST und DELETE) können die CRUD-Vorgängen wie folgt zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="ca1db-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="ca1db-148">GET Ruft die Darstellung der Ressource am angegebenen URI ab.</span><span class="sxs-lookup"><span data-stu-id="ca1db-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="ca1db-149">GET, sollte auf dem Server keine Nebeneffekte haben.</span><span class="sxs-lookup"><span data-stu-id="ca1db-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="ca1db-150">PUT aktualisiert eine Ressource auf einem angegebenen URI.</span><span class="sxs-lookup"><span data-stu-id="ca1db-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="ca1db-151">PUT kann auch verwendet werden, um eine neue Ressource am angegebenen URI, zu erstellen, wenn der Server, Clients an neue URIs zulässt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="ca1db-152">In diesem Tutorial wird in die API-Erstellung über PUT nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="ca1db-153">POST wird eine neue Ressource erstellt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-153">POST creates a new resource.</span></span> <span data-ttu-id="ca1db-154">Der Server weist den URI für das neue Objekt und gibt diesen URI als Teil der Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="ca1db-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="ca1db-155">DELETE Löscht eine Ressource auf einem angegebenen URI.</span><span class="sxs-lookup"><span data-stu-id="ca1db-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="ca1db-156">Hinweis: Die PUT-Methode ersetzt die gesamte Product-Entität.</span><span class="sxs-lookup"><span data-stu-id="ca1db-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="ca1db-157">Der Client soll, also eine vollständige Darstellung des aktualisierten Produkts zu senden.</span><span class="sxs-lookup"><span data-stu-id="ca1db-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="ca1db-158">Wenn Sie möchten die teilupdates zu unterstützen, wird die PATCH-Methode bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="ca1db-159">In diesem Tutorial implementiert PATCH nicht.</span><span class="sxs-lookup"><span data-stu-id="ca1db-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="ca1db-160">Erstellen eines neuen Web-API-Projekts</span><span class="sxs-lookup"><span data-stu-id="ca1db-160">Create a New Web API Project</span></span>

<span data-ttu-id="ca1db-161">Starten, indem Sie Ausführung von Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="ca1db-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ca1db-162">Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ca1db-163">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="ca1db-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ca1db-164">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ca1db-165">Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ca1db-166">Nennen Sie das Projekt &quot;ProductStore&quot; , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="ca1db-167">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Web-API-** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="ca1db-168">Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="ca1db-168">Adding a Model</span></span>

<span data-ttu-id="ca1db-169">Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ca1db-170">In ASP.NET Web-API können Sie stark typisierte CLR-Objekte als Modelle verwenden, und sie werden automatisch in XML oder JSON serialisiert werden, für den Client.</span><span class="sxs-lookup"><span data-stu-id="ca1db-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="ca1db-171">Für die API ProductStore, besteht unsere Daten von Produkten, daher wir eine neue Klasse namens erstellen `Product`.</span><span class="sxs-lookup"><span data-stu-id="ca1db-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="ca1db-172">Wenn der Projektmappen-Explorer nicht angezeigt wird, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ca1db-173">Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="ca1db-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ca1db-174">Wählen Sie im Kontextmenü des **hinzufügen**, und wählen Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="ca1db-175">Nennen Sie die Klasse &quot;Produkt&quot;.</span><span class="sxs-lookup"><span data-stu-id="ca1db-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="ca1db-176">Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.</span><span class="sxs-lookup"><span data-stu-id="ca1db-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="ca1db-177">Hinzufügen eines Repositorys</span><span class="sxs-lookup"><span data-stu-id="ca1db-177">Adding a Repository</span></span>

<span data-ttu-id="ca1db-178">Wir müssen eine Sammlung von Produkten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="ca1db-178">We need to store a collection of products.</span></span> <span data-ttu-id="ca1db-179">Es ist eine gute Idee, trennen Sie die Sammlung von unsere dienstimplementierung.</span><span class="sxs-lookup"><span data-stu-id="ca1db-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="ca1db-180">Auf diese Weise können wir Sicherungsspeicher ändern, ohne die Dienstklasse umzuschreiben.</span><span class="sxs-lookup"><span data-stu-id="ca1db-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="ca1db-181">Diese Art von Design wird aufgerufen, die *Repository* Muster.</span><span class="sxs-lookup"><span data-stu-id="ca1db-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="ca1db-182">Starten Sie durch die Definition einer generischen Schnittstelle für das Repository.</span><span class="sxs-lookup"><span data-stu-id="ca1db-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="ca1db-183">Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Modelle** Ordner.</span><span class="sxs-lookup"><span data-stu-id="ca1db-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ca1db-184">Wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="ca1db-185">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie den C#-Knoten.</span><span class="sxs-lookup"><span data-stu-id="ca1db-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="ca1db-186">Wählen Sie unter c#, **Code**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-186">Under C#, select **Code**.</span></span> <span data-ttu-id="ca1db-187">Wählen Sie in der Liste der Codevorlagen, **Schnittstelle**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="ca1db-188">Benennen Sie die Schnittstelle &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="ca1db-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="ca1db-189">Fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="ca1db-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="ca1db-190">Fügen Sie eine andere Klasse jetzt im Ordner "Models", mit dem Namen &quot;ProductRepository.&quot; Diese Klasse implementiert die `IProductRepository`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="ca1db-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="ca1db-191">Fügen Sie die folgende Implementierung:</span><span class="sxs-lookup"><span data-stu-id="ca1db-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="ca1db-192">Das Repository speichert die Liste im lokalen Speicher.</span><span class="sxs-lookup"><span data-stu-id="ca1db-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="ca1db-193">Dies ist in Ordnung ein Tutorial, aber in einer echten Anwendung würden Sie die Daten extern, entweder eine Datenbank speichern oder in einem cloudspeicher.</span><span class="sxs-lookup"><span data-stu-id="ca1db-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="ca1db-194">Das Repository-Muster wird so ändern Sie die Implementierung zu einem späteren Zeitpunkt erleichtern.</span><span class="sxs-lookup"><span data-stu-id="ca1db-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="ca1db-195">Hinzufügen eines Web-API-Controllers</span><span class="sxs-lookup"><span data-stu-id="ca1db-195">Adding a Web API Controller</span></span>

<span data-ttu-id="ca1db-196">Wenn Sie mit ASP.NET MVC gearbeitet haben, sind Sie bereits vertraut mit Controllern.</span><span class="sxs-lookup"><span data-stu-id="ca1db-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="ca1db-197">In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen vom Client verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="ca1db-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="ca1db-198">Der Assistent für neues Projekt für Sie zwei Controller erstellt werden, wenn es sich bei der Erstellung des Projekts.</span><span class="sxs-lookup"><span data-stu-id="ca1db-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="ca1db-199">Um anzuzeigen, erweitern Sie im Projektmappen-Explorer den Ordner "Controllers" aus.</span><span class="sxs-lookup"><span data-stu-id="ca1db-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="ca1db-200">HomeController ist eine herkömmliche ASP.NET MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="ca1db-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="ca1db-201">Es wird dient zum Bereitstellen von HTML-Seiten für den Standort, und nicht direkt an unsere Web-API.</span><span class="sxs-lookup"><span data-stu-id="ca1db-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="ca1db-202">ValuesController ist eine Beispiel-Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="ca1db-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="ca1db-203">Löschen Sie ValuesController, indem Sie mit der rechten Maustaste in der Datei im Projektmappen-Explorer und auswählen **löschen.**</span><span class="sxs-lookup"><span data-stu-id="ca1db-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="ca1db-204">Fügen Sie einen neuen Controller jetzt wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ca1db-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="ca1db-205">In **Projektmappen-Explorer**, mit der rechten Maustaste in den Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="ca1db-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ca1db-206">Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="ca1db-207">In der **Controller hinzufügen** Assistenten benennen Sie den Controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ca1db-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ca1db-208">In der **Vorlage** Dropdown-Liste **leerer API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="ca1db-209">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="ca1db-210">Es ist nicht erforderlich, die Controller in den Ordner Controller zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="ca1db-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="ca1db-211">Der Name des Ordners ist nicht wichtig; Es ist lediglich eine bequeme Möglichkeit, Ihre Quelldateien zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="ca1db-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="ca1db-212">Die **Controller hinzufügen** Assistent erstellt eine Datei namens ProductsController.cs im Ordner "Controllers".</span><span class="sxs-lookup"><span data-stu-id="ca1db-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="ca1db-213">Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="ca1db-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ca1db-214">Fügen Sie die folgenden **mit** Anweisung:</span><span class="sxs-lookup"><span data-stu-id="ca1db-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="ca1db-215">Hinzufügen eines Felds, das enthält ein **IProductRepository** Instanz.</span><span class="sxs-lookup"><span data-stu-id="ca1db-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="ca1db-216">Aufrufen von `new ProductRepository()` im Controller ist nicht der optimale Entwurf, da es den Controller eine bestimmte Implementierung bindet `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ca1db-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="ca1db-217">Ein besserer Ansatz ist, finden Sie unter [mithilfe der Web-API-Abhängigkeitskonfliktlöser](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ca1db-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="ca1db-218">Abrufen einer Ressourcensatzes</span><span class="sxs-lookup"><span data-stu-id="ca1db-218">Getting a Resource</span></span>

<span data-ttu-id="ca1db-219">Die ProductStore-API macht mehrere &quot;lesen&quot; Aktionen als HTTP GET-Methoden.</span><span class="sxs-lookup"><span data-stu-id="ca1db-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="ca1db-220">Jede Aktion entspricht einer Methode in der `ProductsController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="ca1db-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="ca1db-221">Aktion</span><span class="sxs-lookup"><span data-stu-id="ca1db-221">Action</span></span> | <span data-ttu-id="ca1db-222">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="ca1db-222">HTTP method</span></span> | <span data-ttu-id="ca1db-223">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="ca1db-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ca1db-224">Abrufen einer Liste aller Produkte</span><span class="sxs-lookup"><span data-stu-id="ca1db-224">Get a list of all products</span></span> | <span data-ttu-id="ca1db-225">GET</span><span class="sxs-lookup"><span data-stu-id="ca1db-225">GET</span></span> | <span data-ttu-id="ca1db-226">/api/products</span><span class="sxs-lookup"><span data-stu-id="ca1db-226">/api/products</span></span> |
| <span data-ttu-id="ca1db-227">Abrufen eines Produkts nach ID</span><span class="sxs-lookup"><span data-stu-id="ca1db-227">Get a product by ID</span></span> | <span data-ttu-id="ca1db-228">GET</span><span class="sxs-lookup"><span data-stu-id="ca1db-228">GET</span></span> | <span data-ttu-id="ca1db-229">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ca1db-229">/api/products/*id*</span></span> |
| <span data-ttu-id="ca1db-230">Abrufen eines Produkts nach Kategorie</span><span class="sxs-lookup"><span data-stu-id="ca1db-230">Get a product by category</span></span> | <span data-ttu-id="ca1db-231">GET</span><span class="sxs-lookup"><span data-stu-id="ca1db-231">GET</span></span> | <span data-ttu-id="ca1db-232">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="ca1db-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="ca1db-233">Um die Liste aller Produkte zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="ca1db-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="ca1db-234">Der Name der Methode beginnt mit &quot;erhalten&quot;, so, dass gemäß der Konvention sie GET-Anforderungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ca1db-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="ca1db-235">Darüber hinaus, da die Methode keine Parameter enthält, entspricht dem ein URI, der keine enthält ein *&quot;Id&quot;* Segment im Pfad.</span><span class="sxs-lookup"><span data-stu-id="ca1db-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="ca1db-236">Um ein Produkt nach ID zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="ca1db-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="ca1db-237">Dieser Methodenname beginnt auch mit &quot;erhalten&quot;, aber die Methode weist einen Parameter namens *Id*. Dieser Parameter zugeordnet ist die &quot;Id&quot; Segment des URI-Pfads.</span><span class="sxs-lookup"><span data-stu-id="ca1db-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="ca1db-238">Das ASP.NET Web-API-Framework konvertiert automatisch die ID in den richtigen Datentyp (**Int**) für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="ca1db-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="ca1db-239">Die GetProduct-Methode löst eine Ausnahme vom Typ **HttpResponseException** Wenn *Id* ist ungültig.</span><span class="sxs-lookup"><span data-stu-id="ca1db-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="ca1db-240">Diese Ausnahme wird vom Framework in den Fehler 404 (nicht gefunden) übersetzt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="ca1db-241">Abschließend fügen Sie eine Methode, um die Produkte nach Kategorie zu suchen:</span><span class="sxs-lookup"><span data-stu-id="ca1db-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="ca1db-242">Wenn der Anforderungs-URI eine Abfragezeichenfolge enthält, versucht Web-API ab, die Abfrageparameter an einen Parameter in der Controllermethode übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="ca1db-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="ca1db-243">Aus diesem Grund einen URI im Format "api/Produkte? Kategorie =*Kategorie*" an diese Methode zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="ca1db-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="ca1db-244">Erstellen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="ca1db-244">Creating a Resource</span></span>

<span data-ttu-id="ca1db-245">Als Nächstes fügen wir eine Methode, um die `ProductsController` Klasse zum Erstellen eines neuen Produkts.</span><span class="sxs-lookup"><span data-stu-id="ca1db-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="ca1db-246">Hier ist eine einfache Implementierung der Methode ein:</span><span class="sxs-lookup"><span data-stu-id="ca1db-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="ca1db-247">Beachten Sie diese Methode zwei Dinge:</span><span class="sxs-lookup"><span data-stu-id="ca1db-247">Note two things about this method:</span></span>

- <span data-ttu-id="ca1db-248">Der Name der Methode beginnt mit &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="ca1db-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="ca1db-249">Um ein neues Produkt zu erstellen, sendet der Client eine HTTP POST-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="ca1db-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="ca1db-250">Die Methode nimmt einen Parameter vom Typ Produkt.</span><span class="sxs-lookup"><span data-stu-id="ca1db-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="ca1db-251">In der Web-API werden die Parameter mit komplexen Typen aus dem Anforderungstext deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="ca1db-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="ca1db-252">Daher erwarten wir den Client um eine serialisierte Darstellung des ein Product-Objekt, in XML oder JSON-Format zu senden.</span><span class="sxs-lookup"><span data-stu-id="ca1db-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="ca1db-253">Diese Implementierung funktioniert, aber es ist leider unvollständig.</span><span class="sxs-lookup"><span data-stu-id="ca1db-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="ca1db-254">Wir möchten im Idealfall HTTP-Antwort auf die Folgendes umfassen:</span><span class="sxs-lookup"><span data-stu-id="ca1db-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="ca1db-255">**Antwortcode:** Standardmäßig legt das Framework für die Web-API-Statuscode der Antwort 200 (OK) fest.</span><span class="sxs-lookup"><span data-stu-id="ca1db-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="ca1db-256">Doch gemäß der HTTP/1.1-Protokoll, wenn eine POST-Anforderung bei der Erstellung einer Ressource, führt die sollte Serverantwort mit dem Status 201 (erstellt).</span><span class="sxs-lookup"><span data-stu-id="ca1db-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="ca1db-257">**Speicherort:** Wenn der Server eine Ressource erstellt wird, sollte es den URI der neuen Ressource im Location-Header der Antwort enthalten.</span><span class="sxs-lookup"><span data-stu-id="ca1db-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="ca1db-258">ASP.NET Web-API erleichtert die Bearbeitung von HTTP-Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="ca1db-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="ca1db-259">Dies ist die verbesserte Implementierung ein:</span><span class="sxs-lookup"><span data-stu-id="ca1db-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="ca1db-260">Beachten Sie, dass der Rückgabetyp der Methode jetzt **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ca1db-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="ca1db-261">Durch Zurückgeben einer **HttpResponseMessage** anstelle eines Produkts, wir können die Details der HTTP-Antwortnachricht, einschließlich der Statuscode und den Location-Header steuern.</span><span class="sxs-lookup"><span data-stu-id="ca1db-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="ca1db-262">Die **CreateResponse** -Methode erstellt eine **HttpResponseMessage** und schreibt Sie automatisch eine serialisierte Darstellung der Product-Objekt in Text fo die Response-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="ca1db-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="ca1db-263">In diesem Beispiel überprüft nicht die `Product`.</span><span class="sxs-lookup"><span data-stu-id="ca1db-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="ca1db-264">Weitere Informationen zur modellvalidierung finden Sie unter [Model Validation in ASP.NET Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ca1db-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="ca1db-265">Aktualisieren einer Ressource</span><span class="sxs-lookup"><span data-stu-id="ca1db-265">Updating a Resource</span></span>

<span data-ttu-id="ca1db-266">Aktualisieren eines Produkts bei Verwendung von PUT ist einfach:</span><span class="sxs-lookup"><span data-stu-id="ca1db-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="ca1db-267">Der Name der Methode beginnt mit &quot;einfügen... &quot;, sodass es Web-API-PUT-Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="ca1db-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="ca1db-268">Die Methode nimmt zwei Parameter, die Produkt-ID und des aktualisierten Produkts.</span><span class="sxs-lookup"><span data-stu-id="ca1db-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="ca1db-269">Die *Id* Parameter stammt aus dem URI-Pfad, und die *Produkt* Parameter aus dem Anforderungstext deserialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="ca1db-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="ca1db-270">Standardmäßig führt das ASP.NET Web-API-Framework einfache Parametertypen aus der Route und komplexe Typen aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="ca1db-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="ca1db-271">Löschen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="ca1db-271">Deleting a Resource</span></span>

<span data-ttu-id="ca1db-272">Um eine Ressource zu löschen, definieren Sie eine "löschen..." -Methode.</span><span class="sxs-lookup"><span data-stu-id="ca1db-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="ca1db-273">Wenn eine DELETE-Anforderung erfolgreich ist, kann es Status 200 (OK) mit einem Entity-Body zurückgegeben wird, die den Status beschreibt. Status 202 (akzeptiert), wenn der Löschvorgang noch ausstehende; oder der Status 204 (kein Inhalt) ohne Text für die Entität.</span><span class="sxs-lookup"><span data-stu-id="ca1db-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="ca1db-274">In diesem Fall die `DeleteProduct` Methode verfügt über eine `void` Typ zurückgeben, sodass ASP.NET Web-API diese automatisch in den Status der übersetzt code 204 (No Content).</span><span class="sxs-lookup"><span data-stu-id="ca1db-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
