---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Entitäts Beziehungen in odata V4 mithilfe von ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten. Mithilfe von odata können Clients navigieren...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484461"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="6ad42-104">Entitäts Beziehungen in odata V4 mithilfe von ASP.net-Web-API 2,2</span><span class="sxs-lookup"><span data-stu-id="6ad42-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="6ad42-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6ad42-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6ad42-106">Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="6ad42-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="6ad42-107">Mithilfe von odata können Clients durch Entitäts Beziehungen navigieren.</span><span class="sxs-lookup"><span data-stu-id="6ad42-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="6ad42-108">Wenn Sie ein Produkt erhalten, finden Sie den Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="6ad42-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="6ad42-109">Sie können auch Beziehungen erstellen oder entfernen.</span><span class="sxs-lookup"><span data-stu-id="6ad42-109">You can also create or remove relationships.</span></span> <span data-ttu-id="6ad42-110">Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.</span><span class="sxs-lookup"><span data-stu-id="6ad42-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="6ad42-111">In diesem Tutorial wird gezeigt, wie diese Vorgänge in odata V4 mithilfe von ASP.net-Web-API unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="6ad42-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="6ad42-112">Das Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md)auf.</span><span class="sxs-lookup"><span data-stu-id="6ad42-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6ad42-113">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="6ad42-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="6ad42-114">Web-API 2,1</span><span class="sxs-lookup"><span data-stu-id="6ad42-114">Web API 2.1</span></span>
> - <span data-ttu-id="6ad42-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="6ad42-115">OData v4</span></span>
> - <span data-ttu-id="6ad42-116">Visual Studio 2013 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)herunterladen)</span><span class="sxs-lookup"><span data-stu-id="6ad42-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="6ad42-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6ad42-117">Entity Framework 6</span></span>
> - <span data-ttu-id="6ad42-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6ad42-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="6ad42-119">Tutorialversionen</span><span class="sxs-lookup"><span data-stu-id="6ad42-119">Tutorial versions</span></span>
>
> <span data-ttu-id="6ad42-120">Informationen zu odata, Version 3, finden Sie [unter unterstützen von Entitäts Beziehungen in odata V3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="6ad42-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="6ad42-121">Hinzufügen einer Lieferanten Entität</span><span class="sxs-lookup"><span data-stu-id="6ad42-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="6ad42-122">Das Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md)auf.</span><span class="sxs-lookup"><span data-stu-id="6ad42-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="6ad42-123">Zuerst benötigen wir eine verwandte Entität.</span><span class="sxs-lookup"><span data-stu-id="6ad42-123">First, we need a related entity.</span></span> <span data-ttu-id="6ad42-124">Fügen Sie im Ordner Models eine Klasse mit dem Namen `Supplier` hinzu.</span><span class="sxs-lookup"><span data-stu-id="6ad42-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="6ad42-125">Fügen Sie eine Navigations Eigenschaft zur `Product`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="6ad42-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="6ad42-126">Fügen Sie der `ProductsContext`-Klasse ein neues **dbset** hinzu, damit Entity Framework die Lieferanten Tabelle in der Datenbank enthält.</span><span class="sxs-lookup"><span data-stu-id="6ad42-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="6ad42-127">Fügen Sie in WebApiConfig.cs dem Entity Data Model eine &quot;Suppliers&quot; Entitätenmenge hinzu:</span><span class="sxs-lookup"><span data-stu-id="6ad42-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="6ad42-128">Hinzufügen eines Suppliers-Controllers</span><span class="sxs-lookup"><span data-stu-id="6ad42-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="6ad42-129">Fügen Sie dem Ordner Controllers eine `SuppliersController` Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="6ad42-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="6ad42-130">Ich zeige Ihnen nicht, wie CRUD-Vorgänge für diesen Controller hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6ad42-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="6ad42-131">Die Schritte sind identisch mit denen für den Produkte Controller (siehe [Erstellen eines odata V4-Endpunkts](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="6ad42-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="6ad42-132">Beziehen verwandter Entitäten</span><span class="sxs-lookup"><span data-stu-id="6ad42-132">Getting Related Entities</span></span>

<span data-ttu-id="6ad42-133">Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="6ad42-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="6ad42-134">Fügen Sie der `ProductsController`-Klasse die folgende Methode hinzu, um diese Anforderung zu unterstützen:</span><span class="sxs-lookup"><span data-stu-id="6ad42-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="6ad42-135">Diese Methode verwendet eine Standard Benennungs Konvention.</span><span class="sxs-lookup"><span data-stu-id="6ad42-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="6ad42-136">Methodenname: getX, wobei X die Navigations Eigenschaft ist.</span><span class="sxs-lookup"><span data-stu-id="6ad42-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="6ad42-137">Parameter Name: *Schlüssel*</span><span class="sxs-lookup"><span data-stu-id="6ad42-137">Parameter name: *key*</span></span>

<span data-ttu-id="6ad42-138">Wenn Sie diese Benennungs Konvention befolgen, ordnet die Web-API die HTTP-Anforderung automatisch der Controller-Methode zu.</span><span class="sxs-lookup"><span data-stu-id="6ad42-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="6ad42-139">HTTP-Beispiel Anforderung:</span><span class="sxs-lookup"><span data-stu-id="6ad42-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="6ad42-140">HTTP-Beispiel Antwort:</span><span class="sxs-lookup"><span data-stu-id="6ad42-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="6ad42-141">Beziehen einer verknüpften Sammlung</span><span class="sxs-lookup"><span data-stu-id="6ad42-141">Getting a related collection</span></span>

<span data-ttu-id="6ad42-142">Im vorherigen Beispiel hat ein Produkt einen Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="6ad42-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="6ad42-143">Eine Navigations Eigenschaft kann auch eine Auflistung zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="6ad42-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="6ad42-144">Der folgende Code Ruft die Produkte für einen Lieferanten ab:</span><span class="sxs-lookup"><span data-stu-id="6ad42-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="6ad42-145">In diesem Fall gibt die Methode eine **iquervable** anstelle eines **singleresult-&lt;t zurück&gt;**</span><span class="sxs-lookup"><span data-stu-id="6ad42-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="6ad42-146">HTTP-Beispiel Anforderung:</span><span class="sxs-lookup"><span data-stu-id="6ad42-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="6ad42-147">HTTP-Beispiel Antwort:</span><span class="sxs-lookup"><span data-stu-id="6ad42-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="6ad42-148">Erstellen einer Beziehung zwischen Entitäten</span><span class="sxs-lookup"><span data-stu-id="6ad42-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="6ad42-149">Odata unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei vorhandenen Entitäten.</span><span class="sxs-lookup"><span data-stu-id="6ad42-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="6ad42-150">In der odata V4-Terminologie ist die Beziehung eine &quot;Referenz&quot;.</span><span class="sxs-lookup"><span data-stu-id="6ad42-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="6ad42-151">(In odata V3 wurde die Beziehung als *Link*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6ad42-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="6ad42-152">Die Protokoll Unterschiede sind für dieses Tutorial nicht von Bedeutung.)</span><span class="sxs-lookup"><span data-stu-id="6ad42-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="6ad42-153">Ein Verweis hat seinen eigenen URI, wobei das Formular `/Entity/NavigationProperty/$ref`ist.</span><span class="sxs-lookup"><span data-stu-id="6ad42-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="6ad42-154">Hier ist beispielsweise der URI, der den Verweis zwischen einem Produkt und seinem Lieferanten adressiert:</span><span class="sxs-lookup"><span data-stu-id="6ad42-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="6ad42-155">Zum Hinzufügen einer Beziehung sendet der Client eine Post-oder PUT-Anforderung an diese Adresse.</span><span class="sxs-lookup"><span data-stu-id="6ad42-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="6ad42-156">Put, wenn die Navigations Eigenschaft eine einzelne Entität ist, z. b. `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="6ad42-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="6ad42-157">Post, wenn die Navigations Eigenschaft eine Auflistung ist, z. b. `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="6ad42-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="6ad42-158">Der Anforderungs Text enthält den URI der anderen Entität in der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="6ad42-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="6ad42-159">Hier ist eine Beispiel Anforderung:</span><span class="sxs-lookup"><span data-stu-id="6ad42-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="6ad42-160">In diesem Beispiel sendet der Client eine PUT-Anforderung an `/Products(6)/Supplier/$ref`. Hierbei handelt es sich um den $ref-URI für die `Supplier` des Produkts mit der ID = 6.</span><span class="sxs-lookup"><span data-stu-id="6ad42-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="6ad42-161">Wenn die Anforderung erfolgreich ausgeführt wird, sendet der Server eine Antwort vom Typ "204 (No Content)":</span><span class="sxs-lookup"><span data-stu-id="6ad42-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="6ad42-162">Hier ist die Controller Methode zum Hinzufügen einer Beziehung zu einem `Product`:</span><span class="sxs-lookup"><span data-stu-id="6ad42-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="6ad42-163">Der *NavigationProperty* -Parameter gibt an, welche Beziehung festgelegt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6ad42-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="6ad42-164">(Wenn für die Entität mehr als eine Navigations Eigenschaft vorhanden ist, können Sie weitere `case`-Anweisungen hinzufügen.)</span><span class="sxs-lookup"><span data-stu-id="6ad42-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="6ad42-165">Der *Link* -Parameter enthält den URI des Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="6ad42-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="6ad42-166">Die Web-API analysiert automatisch den Anforderungs Text, um den Wert für diesen Parameter zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="6ad42-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="6ad42-167">Zum Nachschlagen des Lieferanten benötigen wir die ID (oder den Schlüssel), die Teil des *Link* Parameters ist.</span><span class="sxs-lookup"><span data-stu-id="6ad42-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="6ad42-168">Verwenden Sie hierzu die folgende Hilfsmethode:</span><span class="sxs-lookup"><span data-stu-id="6ad42-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="6ad42-169">Diese Methode verwendet im Grunde die odata-Bibliothek, um den URI-Pfad in Segmente aufzuteilen, das Segment zu suchen, das den Schlüssel enthält, und den Schlüssel in den richtigen Typ zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="6ad42-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="6ad42-170">Löschen einer Beziehung zwischen Entitäten</span><span class="sxs-lookup"><span data-stu-id="6ad42-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="6ad42-171">Zum Löschen einer Beziehung sendet der Client eine HTTP DELETE-Anforderung an den $ref-URI:</span><span class="sxs-lookup"><span data-stu-id="6ad42-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="6ad42-172">Hier ist die Controller Methode zum Löschen der Beziehung zwischen einem Produkt und einem Lieferanten:</span><span class="sxs-lookup"><span data-stu-id="6ad42-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="6ad42-173">In diesem Fall ist `Product.Supplier` das &quot;1&quot; Ende einer 1: n-Beziehung, sodass Sie die Beziehung entfernen können, indem Sie `Product.Supplier` auf `null`festlegen.</span><span class="sxs-lookup"><span data-stu-id="6ad42-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="6ad42-174">Im &quot;viele&quot; Ende einer Beziehung muss der Client angeben, welche verwandte Entität entfernt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6ad42-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="6ad42-175">Hierzu sendet der Client den URI der verknüpften Entität in der Abfrage Zeichenfolge der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="6ad42-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="6ad42-176">Um z. b. "Product 1" aus "Supplier 1" zu entfernen:</span><span class="sxs-lookup"><span data-stu-id="6ad42-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="6ad42-177">Um dies in der Web-API zu unterstützen, müssen wir einen zusätzlichen Parameter in die `DeleteRef`-Methode einschließen.</span><span class="sxs-lookup"><span data-stu-id="6ad42-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="6ad42-178">Hier ist die Controller Methode, mit der ein Produkt aus der `Supplier.Products` Beziehung entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="6ad42-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="6ad42-179">Der *Schlüssel* Parameter ist der Schlüssel für den Lieferanten, und der *relatedkey* -Parameter ist der Schlüssel für das Produkt, das aus der `Products` Beziehung entfernt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6ad42-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="6ad42-180">Beachten Sie, dass Web-API automatisch den Schlüssel aus der Abfrage Zeichenfolge abruft.</span><span class="sxs-lookup"><span data-stu-id="6ad42-180">Note that Web API automatically gets the key from the query string.</span></span>
