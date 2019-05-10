---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Vererbung von komplexen Typen in OData v4 mit ASP.NET Web-API | Microsoft-Dokumentation
author: microsoft
description: Gemäß der OData v4-Spezifikation kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein komplexer Typ ist einen strukturierten Typ ohne einen Schlüssel.) Web-API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132745"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="27c40-104">Vererbung von komplexen Typen in OData v4 mit ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="27c40-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="27c40-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="27c40-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="27c40-106">Gemäß den OData v4 [Spezifikation](http://www.odata.org/documentation/odata-version-4-0/), ein komplexer Typ kann von einem anderen komplexen Typ erben.</span><span class="sxs-lookup"><span data-stu-id="27c40-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="27c40-107">(Ein *komplexe* Typ ist ein strukturierter Typ ohne einen Schlüssel.) Web-API OData 5.3 unterstützt die Vererbung von komplexen Typen.</span><span class="sxs-lookup"><span data-stu-id="27c40-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="27c40-108">In diesem Thema veranschaulicht die Erstellung ein Entity Data Model (EDM) mit von komplexen Vererbungstypen.</span><span class="sxs-lookup"><span data-stu-id="27c40-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="27c40-109">Den vollständigen Quellcode, finden Sie unter [OData Beispiel für komplexe Vererbung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="27c40-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="27c40-110">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="27c40-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="27c40-111">Web-API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="27c40-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="27c40-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="27c40-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="27c40-113">Hierarchie des integritätsmodells</span><span class="sxs-lookup"><span data-stu-id="27c40-113">Model Hierarchy</span></span>

<span data-ttu-id="27c40-114">Um die Vererbung von komplexen Typen zu veranschaulichen, verwenden wir die folgende Hierarchie von Klassen.</span><span class="sxs-lookup"><span data-stu-id="27c40-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="27c40-115">`Shape` wird ein abstrakter komplexer Typ.</span><span class="sxs-lookup"><span data-stu-id="27c40-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="27c40-116">`Rectangle`, `Triangle`, und `Circle` komplexe Typen abgeleitet sind `Shape`, und `RoundRectangle` leitet sich von `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="27c40-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="27c40-117">`Window` ein Entitätstyp und enthält eine `Shape` Instanz.</span><span class="sxs-lookup"><span data-stu-id="27c40-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="27c40-118">Hier sind die CLR-Klassen, die diese Typen definieren.</span><span class="sxs-lookup"><span data-stu-id="27c40-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="27c40-119">Das EDM-Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="27c40-119">Build the EDM Model</span></span>

<span data-ttu-id="27c40-120">Um das EDM zu erstellen, können Sie **ODataConventionModelBuilder**, dem ableitet vererbungsbeziehungen von der CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="27c40-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="27c40-121">Sie können auch erstellen, das EDM explizit mit **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="27c40-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="27c40-122">Dies nimmt mehr Code, aber es gibt Ihnen mehr Kontrolle über das EDM.</span><span class="sxs-lookup"><span data-stu-id="27c40-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="27c40-123">Diese beiden Beispiele erstellen, das gleiche EDM-Schema.</span><span class="sxs-lookup"><span data-stu-id="27c40-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="27c40-124">Metadatendokument</span><span class="sxs-lookup"><span data-stu-id="27c40-124">Metadata Document</span></span>

<span data-ttu-id="27c40-125">Hier ist das OData-Dokument, mit der Vererbung von komplexen Typen.</span><span class="sxs-lookup"><span data-stu-id="27c40-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="27c40-126">Aus dem Dokument sehen Sie, die Folgendes:</span><span class="sxs-lookup"><span data-stu-id="27c40-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="27c40-127">Die `Shape` komplexe Typ abstrakt ist.</span><span class="sxs-lookup"><span data-stu-id="27c40-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="27c40-128">Die `Rectangle`, `Triangle`, und `Circle` komplexen Typs haben den Basistyp `Shape`.</span><span class="sxs-lookup"><span data-stu-id="27c40-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="27c40-129">Die `RoundRectangle` Typ muss der Basistyp `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="27c40-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="27c40-130">Wandeln komplexe Typen</span><span class="sxs-lookup"><span data-stu-id="27c40-130">Casting Complex Types</span></span>

<span data-ttu-id="27c40-131">Für komplexe Typen umwandeln, wird jetzt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="27c40-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="27c40-132">Z. B. die folgende Abfrage Wandelt eine `Shape` zu einem `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="27c40-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="27c40-133">Hier ist die Nutzlast der Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="27c40-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
