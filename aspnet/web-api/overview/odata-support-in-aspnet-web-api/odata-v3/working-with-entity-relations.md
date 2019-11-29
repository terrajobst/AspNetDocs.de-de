---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Unterstützen von Entitäts Beziehungen in odata V3 mit Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten. Mithilfe von odata können Clients navigieren...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600314"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="d0417-104">Unterstützen von Entitäts Beziehungen in odata V3 mit der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="d0417-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>

<span data-ttu-id="d0417-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d0417-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d0417-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="d0417-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="d0417-107">Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="d0417-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="d0417-108">Mithilfe von odata können Clients durch Entitäts Beziehungen navigieren.</span><span class="sxs-lookup"><span data-stu-id="d0417-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="d0417-109">Wenn Sie ein Produkt erhalten, finden Sie den Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="d0417-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="d0417-110">Sie können auch Beziehungen erstellen oder entfernen.</span><span class="sxs-lookup"><span data-stu-id="d0417-110">You can also create or remove relationships.</span></span> <span data-ttu-id="d0417-111">Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.</span><span class="sxs-lookup"><span data-stu-id="d0417-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="d0417-112">In diesem Tutorial wird gezeigt, wie diese Vorgänge in ASP.net-Web-API unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="d0417-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="d0417-113">Das Tutorial baut auf dem Tutorial zum [Erstellen eines odata V3-Endpunkts mit der Web-API 2](creating-an-odata-endpoint.md)auf.</span><span class="sxs-lookup"><span data-stu-id="d0417-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d0417-114">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="d0417-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d0417-115">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="d0417-115">Web API 2</span></span>
> - <span data-ttu-id="d0417-116">Odata, Version 3</span><span class="sxs-lookup"><span data-stu-id="d0417-116">OData Version 3</span></span>
> - <span data-ttu-id="d0417-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d0417-117">Entity Framework 6</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="d0417-118">Hinzufügen einer Lieferanten Entität</span><span class="sxs-lookup"><span data-stu-id="d0417-118">Add a Supplier Entity</span></span>

<span data-ttu-id="d0417-119">Zuerst müssen wir dem odata-Feed einen neuen Entitätstyp hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d0417-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="d0417-120">Wir fügen eine `Supplier` Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="d0417-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="d0417-121">Diese Klasse verwendet eine Zeichenfolge für den Entitäts Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="d0417-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="d0417-122">In der Praxis ist dies möglicherweise weniger häufig als die Verwendung eines ganzzahligen Schlüssels.</span><span class="sxs-lookup"><span data-stu-id="d0417-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="d0417-123">Es ist jedoch zu sehen, wie odata andere Schlüsseltypen außer Ganzzahlen behandelt.</span><span class="sxs-lookup"><span data-stu-id="d0417-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="d0417-124">Als Nächstes erstellen wir eine Beziehung, indem wir der `Product`-Klasse eine `Supplier`-Eigenschaft hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="d0417-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="d0417-125">Fügen Sie der `ProductServiceContext`-Klasse ein neues **dbset** hinzu, damit Entity Framework die `Supplier`-Tabelle in der Datenbank einschließt.</span><span class="sxs-lookup"><span data-stu-id="d0417-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="d0417-126">Fügen Sie in WebApiConfig.cs dem EDM-Modell eine Entität "Suppliers" hinzu:</span><span class="sxs-lookup"><span data-stu-id="d0417-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="d0417-127">Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="d0417-127">Navigation Properties</span></span>

<span data-ttu-id="d0417-128">Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="d0417-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="d0417-129">Hier ist "Supplier" eine Navigations Eigenschaft für den `Product`-Typ.</span><span class="sxs-lookup"><span data-stu-id="d0417-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="d0417-130">In diesem Fall verweist `Supplier` auf ein einzelnes Element, aber eine Navigations Eigenschaft kann auch eine Auflistung (1: n-oder m:n-Beziehung) zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="d0417-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="d0417-131">Fügen Sie der `ProductsController`-Klasse die folgende Methode hinzu, um diese Anforderung zu unterstützen:</span><span class="sxs-lookup"><span data-stu-id="d0417-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="d0417-132">Der *Schlüssel* Parameter ist der Schlüssel des Produkts.</span><span class="sxs-lookup"><span data-stu-id="d0417-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="d0417-133">Die-Methode gibt die zugehörige Entität&#8212;zurück, in diesem Fall eine `Supplier`-Instanz.</span><span class="sxs-lookup"><span data-stu-id="d0417-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="d0417-134">Der Methodenname und der Parameter Name sind beide wichtig.</span><span class="sxs-lookup"><span data-stu-id="d0417-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="d0417-135">Im Allgemeinen müssen Sie, wenn die Navigations Eigenschaft den Namen "X" hat, eine Methode mit dem Namen "getX" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d0417-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="d0417-136">Die Methode muss einen Parameter mit dem Namen "*Key*" akzeptieren, der mit dem Datentyp des übergeordneten Schlüssels übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="d0417-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="d0417-137">Es ist auch wichtig, das **[fromudatauri]** -Attribut in den *Key* -Parameter einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="d0417-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="d0417-138">Dieses Attribut weist die Web-API an, odata-Syntax Regeln zu verwenden, wenn der Schlüssel aus dem Anforderungs-URI analysiert wird.</span><span class="sxs-lookup"><span data-stu-id="d0417-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="d0417-139">Erstellen und Löschen von Links</span><span class="sxs-lookup"><span data-stu-id="d0417-139">Creating and Deleting Links</span></span>

<span data-ttu-id="d0417-140">Odata unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei Entitäten.</span><span class="sxs-lookup"><span data-stu-id="d0417-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="d0417-141">In der odata-Terminologie ist die Beziehung ein "Link".</span><span class="sxs-lookup"><span data-stu-id="d0417-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="d0417-142">Jeder Link weist einen URI mit dem Formular *Entität*/$Links/*Entität*auf.</span><span class="sxs-lookup"><span data-stu-id="d0417-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="d0417-143">Beispielsweise sieht der Link Zwischenprodukt und Lieferant wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="d0417-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="d0417-144">Zum Erstellen eines neuen Links sendet der Client eine Post-Anforderung an den Link-URI.</span><span class="sxs-lookup"><span data-stu-id="d0417-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="d0417-145">Der Anforderungs Text ist der URI der Ziel Entität.</span><span class="sxs-lookup"><span data-stu-id="d0417-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="d0417-146">Nehmen wir beispielsweise an, dass es einen Lieferanten mit dem Schlüssel "ctso" gibt.</span><span class="sxs-lookup"><span data-stu-id="d0417-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="d0417-147">Zum Erstellen einer Verknüpfung zwischen "Produkt (1)" und "Lieferant (" ctso ") sendet der Client eine Anforderung wie die folgende:</span><span class="sxs-lookup"><span data-stu-id="d0417-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="d0417-148">Zum Löschen eines Links sendet der Client eine DELETE-Anforderung an den Link-URI.</span><span class="sxs-lookup"><span data-stu-id="d0417-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="d0417-149">**Erstellen von Links**</span><span class="sxs-lookup"><span data-stu-id="d0417-149">**Creating Links**</span></span>

<span data-ttu-id="d0417-150">Fügen Sie der `ProductsController`-Klasse den folgenden Code hinzu, um es einem Client zu ermöglichen, Produktlieferanten Links zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="d0417-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="d0417-151">Diese Methode nimmt drei Parameter an:</span><span class="sxs-lookup"><span data-stu-id="d0417-151">This method takes three parameters:</span></span>

- <span data-ttu-id="d0417-152">*Key*: der Schlüssel für die übergeordnete Entität (das Produkt)</span><span class="sxs-lookup"><span data-stu-id="d0417-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="d0417-153">*NavigationProperty*: der Name der Navigations Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d0417-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="d0417-154">In diesem Beispiel ist die einzige gültige Navigations Eigenschaft "Supplier".</span><span class="sxs-lookup"><span data-stu-id="d0417-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="d0417-155">*Link*: der odata-URI der verknüpften Entität.</span><span class="sxs-lookup"><span data-stu-id="d0417-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="d0417-156">Dieser Wert wird aus dem Anforderungs Text entnommen.</span><span class="sxs-lookup"><span data-stu-id="d0417-156">This value is taken from the request body.</span></span> <span data-ttu-id="d0417-157">Der Link-URI könnte z. b. "`http://localhost/odata/Suppliers('CTSO')`lauten, d. h. der Lieferant mit ID = ' ctso '.</span><span class="sxs-lookup"><span data-stu-id="d0417-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="d0417-158">Die-Methode verwendet den Link, um den Lieferanten zu suchen.</span><span class="sxs-lookup"><span data-stu-id="d0417-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="d0417-159">Wenn der übereinstimmende Lieferant gefunden wird, legt die-Methode die `Product.Supplier`-Eigenschaft fest und speichert das Ergebnis in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d0417-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="d0417-160">Der schwierigste Teil besteht darin, den Link-URI zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="d0417-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="d0417-161">Im Grunde genommen müssen Sie das Ergebnis des Sendens einer GET-Anforderung an diesen URI simulieren.</span><span class="sxs-lookup"><span data-stu-id="d0417-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="d0417-162">Die folgende Hilfsmethode zeigt, wie dies geschieht.</span><span class="sxs-lookup"><span data-stu-id="d0417-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="d0417-163">Die-Methode ruft den Routing Prozess der Web-API auf und ruft eine **odatapath** -Instanz ab, die den analysierten odata-Pfad darstellt.</span><span class="sxs-lookup"><span data-stu-id="d0417-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="d0417-164">Bei einem Link-URI sollte eines der Segmente der Entitäts Schlüssel sein.</span><span class="sxs-lookup"><span data-stu-id="d0417-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="d0417-165">(Andernfalls sendet der Client einen ungültigen URI.)</span><span class="sxs-lookup"><span data-stu-id="d0417-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="d0417-166">**Löschen von Links**</span><span class="sxs-lookup"><span data-stu-id="d0417-166">**Deleting Links**</span></span>

<span data-ttu-id="d0417-167">Fügen Sie der `ProductsController`-Klasse den folgenden Code hinzu, um einen Link zu löschen:</span><span class="sxs-lookup"><span data-stu-id="d0417-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="d0417-168">In diesem Beispiel ist die Navigations Eigenschaft eine einzelne `Supplier` Entität.</span><span class="sxs-lookup"><span data-stu-id="d0417-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="d0417-169">Wenn die Navigations Eigenschaft eine Auflistung ist, muss der URI zum Löschen eines Links einen Schlüssel für die zugehörige Entität enthalten.</span><span class="sxs-lookup"><span data-stu-id="d0417-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="d0417-170">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d0417-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="d0417-171">Diese Anforderung entfernt die Reihenfolge 1 von Kunde 1.</span><span class="sxs-lookup"><span data-stu-id="d0417-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="d0417-172">In diesem Fall hat die Delta-Ink-Methode die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="d0417-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="d0417-173">Der *relatedkey* -Parameter gibt den Schlüssel für die zugehörige Entität an.</span><span class="sxs-lookup"><span data-stu-id="d0417-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="d0417-174">Suchen Sie in der `DeleteLink`-Methode die primäre Entität mit dem *Schlüssel* Parameter, suchen Sie die verknüpfte Entität nach dem Parameter *relatedkey* , und entfernen Sie dann die Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="d0417-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="d0417-175">Abhängig von Ihrem Datenmodell müssen Sie möglicherweise beide Versionen von `DeleteLink`implementieren.</span><span class="sxs-lookup"><span data-stu-id="d0417-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="d0417-176">Die Web-API ruft die richtige Version auf Grundlage des Anforderungs-URI auf.</span><span class="sxs-lookup"><span data-stu-id="d0417-176">Web API will call the correct version based on the request URI.</span></span>
