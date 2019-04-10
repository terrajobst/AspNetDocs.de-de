---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Verwenden $select, $expand, und $value in OData der ASP.NET Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Übersicht über und Codebeispiele für die $expand, $select, und $value "Optionen" in der OData-Web-API 2 für ASP.NET 4.x.
ms.author: riande
ms.date: 10/11/2013
ms.custom: seoapril2019
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: 8b5d3e87c679a31f1908aa648219ae5c6b701a1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400697"
---
# <a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="e9a47-103">Verwenden $select, $expand, und $value in OData der ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e9a47-103">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="e9a47-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9a47-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e9a47-105">Übersicht über und Codebeispiele für die $expand, $select, und $value "Optionen" in der OData-Web-API 2 für ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="e9a47-105">Overview and code samples for the $expand, $select, and $value options in OData Web API 2 for ASP.NET 4.x.</span></span> <span data-ttu-id="e9a47-106">Mit diesen Optionen können einen Client, um die Darstellung zu steuern, die sie wieder vom Server abruft.</span><span class="sxs-lookup"><span data-stu-id="e9a47-106">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="e9a47-107">**der $expand-** bewirkt, dass verknüpfte Entitäten Inlineschemainformationen enthält, in der Antwort zu sein.</span><span class="sxs-lookup"><span data-stu-id="e9a47-107">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="e9a47-108">**$select** wählt eine Teilmenge der Eigenschaften in der Antwort eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e9a47-108">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="e9a47-109">**$value** Ruft den Rohwert einer Eigenschaft ab.</span><span class="sxs-lookup"><span data-stu-id="e9a47-109">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="e9a47-110">Beispielschema</span><span class="sxs-lookup"><span data-stu-id="e9a47-110">Example Schema</span></span>

<span data-ttu-id="e9a47-111">In diesem Artikel verwende ich einen OData-Dienst, der drei Entitäten definiert: Produkt, Lieferanten und Kategorie.</span><span class="sxs-lookup"><span data-stu-id="e9a47-111">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="e9a47-112">Jedes Produkt verfügt über eine Kategorie und ein Lieferant.</span><span class="sxs-lookup"><span data-stu-id="e9a47-112">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="e9a47-113">Hier sind die C#-Klassen, die die Entitätsmodelle zu definieren:</span><span class="sxs-lookup"><span data-stu-id="e9a47-113">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="e9a47-114">Beachten Sie, dass die `Product` -Klasse definiert die Navigationseigenschaften für die `Supplier` und `Category`.</span><span class="sxs-lookup"><span data-stu-id="e9a47-114">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="e9a47-115">Die `Category` Klasse definiert eine Navigationseigenschaft für die Produkte in jeder Kategorie.</span><span class="sxs-lookup"><span data-stu-id="e9a47-115">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="e9a47-116">Verwenden Sie das Gerüst für Visual Studio 2013, um einen OData-Endpunkt für dieses Schema zu erstellen, siehe [Erstellen eines OData-Endpunkts in ASP.NET Web-API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e9a47-116">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="e9a47-117">Fügen Sie für Produkt, Kategorie und Lieferanten separate Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="e9a47-117">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="e9a47-118">Erweitern Sie mit der Aktivierung von $ und $select</span><span class="sxs-lookup"><span data-stu-id="e9a47-118">Enabling $expand and $select</span></span>

<span data-ttu-id="e9a47-119">In Visual Studio 2013 erstellt die Web-API OData-Gerüst ein Controllers, automatisch unterstützt, die $expand- und $select.</span><span class="sxs-lookup"><span data-stu-id="e9a47-119">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="e9a47-120">Zu Referenzzwecken hier sind die Anforderungen zur Unterstützung von $erweitern und $select in einem Controller.</span><span class="sxs-lookup"><span data-stu-id="e9a47-120">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="e9a47-121">Für Sammlungen, die Controller `Get` Methodenrückgabewert muss ein **"IQueryable"**.</span><span class="sxs-lookup"><span data-stu-id="e9a47-121">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="e9a47-122">Für einzelne Entitäten Zurückgeben einer **"SingleResult"&lt;T&gt;**, wobei T ist ein **"IQueryable"** , das NULL Entitäten oder eine Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="e9a47-122">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="e9a47-123">Außerdem ergänzen Ihre `Get` Methoden mit dem **[Queryable]** Attribut, wie in den vorherigen Codeausschnitten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="e9a47-123">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="e9a47-124">Rufen Sie alternativ **EnableQuerySupport** auf die **HttpConfiguration** Objekt beim Start.</span><span class="sxs-lookup"><span data-stu-id="e9a47-124">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="e9a47-125">(Weitere Informationen finden Sie unter [Aktivieren der OData-Abfrageoptionen](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="e9a47-125">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="e9a47-126">Erweitern Sie unter Verwendung von $</span><span class="sxs-lookup"><span data-stu-id="e9a47-126">Using $expand</span></span>

<span data-ttu-id="e9a47-127">Beim Abfragen von einem OData-Entität oder einer Auflistung schließt die Standardantwort verknüpfte Entitäten nicht.</span><span class="sxs-lookup"><span data-stu-id="e9a47-127">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="e9a47-128">So sieht z. B. die Standardantwort für die Entitätenmenge für die Kategorien aus:</span><span class="sxs-lookup"><span data-stu-id="e9a47-128">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="e9a47-129">Wie Sie sehen, enthält die Antwort alle Produkte, keine, obwohl die Category-Entität einen Navigationslink für die Produkte verfügt.</span><span class="sxs-lookup"><span data-stu-id="e9a47-129">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="e9a47-130">Der Client kann jedoch $ verwenden erweitert werden, um die Liste der Produkte für die einzelnen Kategorien zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e9a47-130">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="e9a47-131">Der $expand-Option wird in der Abfragezeichenfolge der Anforderung:</span><span class="sxs-lookup"><span data-stu-id="e9a47-131">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="e9a47-132">Der Server wird jetzt die Produkte für die einzelnen Kategorien, die Inline in die Kategorien enthalten.</span><span class="sxs-lookup"><span data-stu-id="e9a47-132">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="e9a47-133">Hier ist die Nutzlast der Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="e9a47-133">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="e9a47-134">Beachten Sie, dass jeder Eintrag im Array "Value" eine Liste der Produkte enthält.</span><span class="sxs-lookup"><span data-stu-id="e9a47-134">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="e9a47-135">Der $expand-Option akzeptiert eine durch Trennzeichen getrennte Liste der Navigationseigenschaften zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="e9a47-135">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="e9a47-136">Die folgende Anforderung wird erweitert, sowohl die Kategorie und den Lieferanten für ein Produkt.</span><span class="sxs-lookup"><span data-stu-id="e9a47-136">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="e9a47-137">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="e9a47-137">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="e9a47-138">Sie können mehrere Ebenen der Navigationseigenschaft erweitern.</span><span class="sxs-lookup"><span data-stu-id="e9a47-138">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="e9a47-139">Das folgende Beispiel schließt alle Produkte für eine Kategorie und der Lieferant für jedes Produkt.</span><span class="sxs-lookup"><span data-stu-id="e9a47-139">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="e9a47-140">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="e9a47-140">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="e9a47-141">Standardmäßig beschränkt die Web-API die maximale erweiterungstiefe auf 2.</span><span class="sxs-lookup"><span data-stu-id="e9a47-141">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="e9a47-142">Die wird verhindert, dass den Client sendet komplexe Anforderungen wie `$expand=Orders/OrderDetails/Product/Supplier/Region`, der möglicherweise ineffizient, Abfragen und große Antworten erstellen.</span><span class="sxs-lookup"><span data-stu-id="e9a47-142">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="e9a47-143">Um die Standardeinstellung zu überschreiben, legen die **MaxExpansionDepth** Eigenschaft für die **[Queryable]** Attribut.</span><span class="sxs-lookup"><span data-stu-id="e9a47-143">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="e9a47-144">Weitere Informationen zur $expand-Option, finden Sie unter [Expand System Query-Option (expand, $)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in der offiziellen OData-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e9a47-144">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="e9a47-145">$Select verwenden</span><span class="sxs-lookup"><span data-stu-id="e9a47-145">Using $select</span></span>

<span data-ttu-id="e9a47-146">Die $select-Option gibt eine Teilmenge der Eigenschaften in den Antworttext eingeschlossen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e9a47-146">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="e9a47-147">Um nur die Namen und die Preise für jedes Produkt zu erhalten, verwenden Sie z. B. die folgende Abfrage:</span><span class="sxs-lookup"><span data-stu-id="e9a47-147">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="e9a47-148">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="e9a47-148">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="e9a47-149">Können kombiniert werden $select und $expand, in der gleichen Abfrage.</span><span class="sxs-lookup"><span data-stu-id="e9a47-149">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="e9a47-150">Stellen Sie sicher, dass die erweiterte Eigenschaft in der Option "$select" enthalten.</span><span class="sxs-lookup"><span data-stu-id="e9a47-150">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="e9a47-151">Die folgende Anforderung ruft z. B. ab, der Produktname und Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="e9a47-151">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="e9a47-152">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="e9a47-152">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="e9a47-153">Sie können auch die Eigenschaften in einer erweiterten Eigenschaft auswählen.</span><span class="sxs-lookup"><span data-stu-id="e9a47-153">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="e9a47-154">Die folgende Anforderung wird erweitert, Produkte und wählt Kategorienamen und Produktname.</span><span class="sxs-lookup"><span data-stu-id="e9a47-154">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="e9a47-155">Hier ist der Antworttext:</span><span class="sxs-lookup"><span data-stu-id="e9a47-155">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="e9a47-156">Weitere Informationen zur Option "$select" finden Sie unter [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in der offiziellen OData-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e9a47-156">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="e9a47-157">Abrufen von einzelnen Eigenschaften einer Entität ($value)</span><span class="sxs-lookup"><span data-stu-id="e9a47-157">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="e9a47-158">Es gibt zwei Möglichkeiten für einen OData-Client, um eine einzelne Eigenschaft aus einer Entität zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e9a47-158">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="e9a47-159">Der Client kann entweder rufe den Wert in OData-Format, oder rufen Sie den raw-Wert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e9a47-159">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="e9a47-160">Die folgende Anforderung Ruft eine Eigenschaft in der OData-Format.</span><span class="sxs-lookup"><span data-stu-id="e9a47-160">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="e9a47-161">Hier ist Beispiel zeigt eine Antwort im JSON-Format ein:</span><span class="sxs-lookup"><span data-stu-id="e9a47-161">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="e9a47-162">Um den unformatierten Wert der Eigenschaft zu erhalten, fügen Sie $value an den URI ein:</span><span class="sxs-lookup"><span data-stu-id="e9a47-162">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="e9a47-163">Hier ist die Antwort.</span><span class="sxs-lookup"><span data-stu-id="e9a47-163">Here is the response.</span></span> <span data-ttu-id="e9a47-164">Beachten Sie, dass der Inhaltstyp "Text/Plain", nicht JSON ist.</span><span class="sxs-lookup"><span data-stu-id="e9a47-164">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="e9a47-165">Um diese Abfragen in Ihren OData-Controller zu unterstützen, fügen Sie eine Methode namens `GetProperty`, wobei `Property` ist der Name der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e9a47-165">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="e9a47-166">Beispielsweise würde die Methode zum Abrufen der Eigenschaft Name den Namen `GetName`.</span><span class="sxs-lookup"><span data-stu-id="e9a47-166">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="e9a47-167">Die Methode sollte den Wert dieser Eigenschaft zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="e9a47-167">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
