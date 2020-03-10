---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Öffnen von Typen in odata v4 mit ASP.net-Web-API | Microsoft-Dokumentation
author: microsoft
description: In odata V4 ist ein offener Typ ein strukturierter Typ, der neben den in der Typdefinition deklarierten Eigenschaften auch dynamische Eigenschaften enthält. Öffnen...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504579"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="8e7af-104">Öffnen von Typen in odata v4 mit ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="8e7af-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="8e7af-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8e7af-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="8e7af-106">In odata V4 ist ein *offener Typ* ein strukturierter Typ, der neben den in der Typdefinition deklarierten Eigenschaften auch dynamische Eigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="8e7af-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="8e7af-107">Mit offenen Typen können Sie Ihren Datenmodellen Flexibilität hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="8e7af-108">In diesem Tutorial wird gezeigt, wie Sie Open types in ASP.net-Web-API odata verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e7af-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="8e7af-109">In diesem Tutorial wird davon ausgegangen, dass Sie bereits wissen, wie ein odata-Endpunkt in ASP.net-Web-API erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="8e7af-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="8e7af-110">Wenn dies nicht der Anfang ist, lesen Sie zuerst [Erstellen eines odata V4-Endpunkts](create-an-odata-v4-endpoint.md) .</span><span class="sxs-lookup"><span data-stu-id="8e7af-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e7af-111">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="8e7af-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8e7af-112">Web-API-odata 5,3</span><span class="sxs-lookup"><span data-stu-id="8e7af-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="8e7af-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="8e7af-113">OData v4</span></span>

<span data-ttu-id="8e7af-114">Zuerst einige odata-Terminologie:</span><span class="sxs-lookup"><span data-stu-id="8e7af-114">First, some OData terminology:</span></span>

- <span data-ttu-id="8e7af-115">Entitätstyp: ein strukturierter Typ mit einem Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="8e7af-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="8e7af-116">Komplexer Typ: ein strukturierter Typ ohne Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="8e7af-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="8e7af-117">Open Type: ein Typ mit dynamischen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="8e7af-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="8e7af-118">Beide Entitäts Typen und komplexe Typen können geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="8e7af-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="8e7af-119">Der Wert einer dynamischen Eigenschaft kann ein primitiver Typ, ein komplexer Typ oder ein Enumerationstyp sein. oder eine Auflistung von diesen Typen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="8e7af-120">Weitere Informationen zu offenen Typen finden Sie in der [odata V4-Spezifikation](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="8e7af-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="8e7af-121">Installieren der webodata-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="8e7af-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="8e7af-122">Verwenden Sie den nuget-Paket-Manager zum Installieren der neuesten Web-API-odata-Bibliotheken.</span><span class="sxs-lookup"><span data-stu-id="8e7af-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="8e7af-123">Im Fenster der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="8e7af-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="8e7af-124">Definieren der CLR-Typen</span><span class="sxs-lookup"><span data-stu-id="8e7af-124">Define the CLR Types</span></span>

<span data-ttu-id="8e7af-125">Definieren Sie zunächst die EDM-Modelle als CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="8e7af-126">Wenn die Entity Data Model (EDM) erstellt wird,</span><span class="sxs-lookup"><span data-stu-id="8e7af-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="8e7af-127">`Category` ist ein Enumerationstyp.</span><span class="sxs-lookup"><span data-stu-id="8e7af-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="8e7af-128">`Address` ist ein komplexer Typ.</span><span class="sxs-lookup"><span data-stu-id="8e7af-128">`Address` is a complex type.</span></span> <span data-ttu-id="8e7af-129">(Er verfügt nicht über einen Schlüssel, daher handelt es sich nicht um einen Entitätstyp.)</span><span class="sxs-lookup"><span data-stu-id="8e7af-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="8e7af-130">`Customer` ist ein Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="8e7af-130">`Customer` is an entity type.</span></span> <span data-ttu-id="8e7af-131">(Er verfügt über einen Schlüssel.)</span><span class="sxs-lookup"><span data-stu-id="8e7af-131">(It has a key.)</span></span>
- <span data-ttu-id="8e7af-132">`Press` ist ein offener komplexer Typ.</span><span class="sxs-lookup"><span data-stu-id="8e7af-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="8e7af-133">`Book` ist ein offener Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="8e7af-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="8e7af-134">Um einen geöffneten Typ zu erstellen, muss der CLR-Typ über eine Eigenschaft vom Typ `IDictionary<string, object>`verfügen, die die dynamischen Eigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="8e7af-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="8e7af-135">Erstellen des EDM-Modells</span><span class="sxs-lookup"><span data-stu-id="8e7af-135">Build the EDM Model</span></span>

<span data-ttu-id="8e7af-136">Wenn Sie **odataconaconmodelbuilder** zum Erstellen des EDM verwenden, werden `Press` und `Book` automatisch als offene Typen hinzugefügt, basierend auf dem vorhanden sein einer `IDictionary<string, object>`-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8e7af-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="8e7af-137">Außerdem können Sie das EDM mithilfe von **odatamodelta Builder**explizit erstellen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="8e7af-138">Hinzufügen eines odata-Controllers</span><span class="sxs-lookup"><span data-stu-id="8e7af-138">Add an OData Controller</span></span>

<span data-ttu-id="8e7af-139">Fügen Sie als nächstes einen odata-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="8e7af-139">Next, add an OData controller.</span></span> <span data-ttu-id="8e7af-140">Für dieses Tutorial verwenden wir einen vereinfachten Controller, der nur Get-und Post-Anforderungen unterstützt und eine in-Memory-Liste zum Speichern von Entitäten verwendet.</span><span class="sxs-lookup"><span data-stu-id="8e7af-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="8e7af-141">Beachten Sie, dass die erste `Book` Instanz keine dynamischen Eigenschaften hat.</span><span class="sxs-lookup"><span data-stu-id="8e7af-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="8e7af-142">Die zweite `Book` Instanz verfügt über die folgenden dynamischen Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="8e7af-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="8e7af-143">"Veröffentlicht": primitiver Typ</span><span class="sxs-lookup"><span data-stu-id="8e7af-143">"Published": Primitive type</span></span>
- <span data-ttu-id="8e7af-144">"Authors": Auflistung primitiver Typen</span><span class="sxs-lookup"><span data-stu-id="8e7af-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="8e7af-145">"Othercategories": Sammlung von Enumerationstypen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="8e7af-146">Außerdem weist die `Press`-Eigenschaft dieser `Book` Instanz die folgenden dynamischen Eigenschaften auf:</span><span class="sxs-lookup"><span data-stu-id="8e7af-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="8e7af-147">"Blog": primitiver Typ</span><span class="sxs-lookup"><span data-stu-id="8e7af-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="8e7af-148">"Address": komplexer Typ</span><span class="sxs-lookup"><span data-stu-id="8e7af-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="8e7af-149">Abfragen der Metadaten</span><span class="sxs-lookup"><span data-stu-id="8e7af-149">Query the Metadata</span></span>

<span data-ttu-id="8e7af-150">Um das odata-Metadatendokument zu erhalten, senden Sie eine GET-Anforderung an `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="8e7af-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="8e7af-151">Der Antworttext sollte in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="8e7af-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="8e7af-152">Im Metadatendokument können Sie Folgendes sehen:</span><span class="sxs-lookup"><span data-stu-id="8e7af-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="8e7af-153">Für die `Book`-und `Press` Typen ist der Wert des `OpenType`-Attributs "true".</span><span class="sxs-lookup"><span data-stu-id="8e7af-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="8e7af-154">Die `Customer`-und `Address` Typen haben dieses Attribut nicht.</span><span class="sxs-lookup"><span data-stu-id="8e7af-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="8e7af-155">Der `Book` Entitätstyp verfügt über drei deklarierte Eigenschaften: ISBN, Title und drücken.</span><span class="sxs-lookup"><span data-stu-id="8e7af-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="8e7af-156">Die odata-Metadaten enthalten nicht die `Book.Properties`-Eigenschaft aus der CLR-Klasse.</span><span class="sxs-lookup"><span data-stu-id="8e7af-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="8e7af-157">Entsprechend verfügt der `Press` Complex-Typ nur über zwei deklarierte Eigenschaften: "Name" und "Category".</span><span class="sxs-lookup"><span data-stu-id="8e7af-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="8e7af-158">Die Metadaten enthalten nicht die `Press.DynamicProperties`-Eigenschaft aus der CLR-Klasse.</span><span class="sxs-lookup"><span data-stu-id="8e7af-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="8e7af-159">Abfragen einer Entität</span><span class="sxs-lookup"><span data-stu-id="8e7af-159">Query an Entity</span></span>

<span data-ttu-id="8e7af-160">Um das Buch mit ISBN gleich "978-0-7356-7942-9" zu erhalten, senden Sie eine GET-Anforderung an `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="8e7af-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="8e7af-161">Der Antworttext sollte in etwa wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-161">The response body should look similar to the following.</span></span> <span data-ttu-id="8e7af-162">(Eingerückt, um Sie besser lesbar zu machen.)</span><span class="sxs-lookup"><span data-stu-id="8e7af-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="8e7af-163">Beachten Sie, dass die dynamischen Eigenschaften Inline mit den deklarierten Eigenschaften enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="8e7af-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="8e7af-164">Eine Entität Posten</span><span class="sxs-lookup"><span data-stu-id="8e7af-164">POST an Entity</span></span>

<span data-ttu-id="8e7af-165">Um eine Buch Entität hinzuzufügen, senden Sie eine Post-Anforderung an `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="8e7af-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="8e7af-166">Der Client kann dynamische Eigenschaften in der Anforderungs Nutzlast festlegen.</span><span class="sxs-lookup"><span data-stu-id="8e7af-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="8e7af-167">Im folgenden finden Sie ein Beispiel für eine Anforderung.</span><span class="sxs-lookup"><span data-stu-id="8e7af-167">Here is an example request.</span></span> <span data-ttu-id="8e7af-168">Beachten Sie die Eigenschaften "Price" und "veröffentlicht".</span><span class="sxs-lookup"><span data-stu-id="8e7af-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="8e7af-169">Wenn Sie in der Controller Methode einen Haltepunkt festlegen, sehen Sie, dass die Web-API diese Eigenschaften dem `Properties` Wörterbuch hinzugefügt hat.</span><span class="sxs-lookup"><span data-stu-id="8e7af-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="8e7af-170">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8e7af-170">Additional Resources</span></span>

[<span data-ttu-id="8e7af-171">Odata-Open-Type-Beispiel</span><span class="sxs-lookup"><span data-stu-id="8e7af-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
