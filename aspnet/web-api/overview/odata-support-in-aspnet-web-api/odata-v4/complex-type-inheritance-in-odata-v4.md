---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Vererbung komplexer Typen in odata v4 mit ASP.net-Web-API | Microsoft-Dokumentation
author: microsoft
description: Gemäß der odata V4-Spezifikation kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein komplexer Typ ist ein strukturierter Typ ohne Schlüssel.) Web-API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448131"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="7991d-104">Vererbung komplexer Typen in odata v4 mit ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="7991d-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="7991d-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7991d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7991d-106">Gemäß der odata V4- [Spezifikation](http://www.odata.org/documentation/odata-version-4-0/)kann ein komplexer Typ von einem anderen komplexen Typ erben.</span><span class="sxs-lookup"><span data-stu-id="7991d-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="7991d-107">(Ein *komplexer* Typ ist ein strukturierter Typ ohne Schlüssel.) Die Web-API odata 5,3 unterstützt die Vererbung komplexer Typen.</span><span class="sxs-lookup"><span data-stu-id="7991d-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="7991d-108">In diesem Thema wird gezeigt, wie ein Entity Data Model (EDM) mit komplexen Vererbungs Typen erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7991d-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="7991d-109">Den gesamten Quellcode finden Sie unter [odata-Beispiel für komplexe Typvererbung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="7991d-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7991d-110">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="7991d-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7991d-111">Web-API-odata 5,3</span><span class="sxs-lookup"><span data-stu-id="7991d-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="7991d-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="7991d-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="7991d-113">Modell Hierarchie</span><span class="sxs-lookup"><span data-stu-id="7991d-113">Model Hierarchy</span></span>

<span data-ttu-id="7991d-114">Um die komplexe Typvererbung zu veranschaulichen, verwenden wir die folgende Klassenhierarchie.</span><span class="sxs-lookup"><span data-stu-id="7991d-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="7991d-115">`Shape` ist ein abstrakter komplexer Typ.</span><span class="sxs-lookup"><span data-stu-id="7991d-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="7991d-116">`Rectangle`, `Triangle`und `Circle` sind komplexe Typen, die von `Shape`abgeleitet sind, und `RoundRectangle` von `Rectangle`abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="7991d-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="7991d-117">`Window` ist ein Entitätstyp und enthält eine `Shape` Instanz.</span><span class="sxs-lookup"><span data-stu-id="7991d-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="7991d-118">Hier sind die CLR-Klassen, die diese Typen definieren.</span><span class="sxs-lookup"><span data-stu-id="7991d-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="7991d-119">Erstellen des EDM-Modells</span><span class="sxs-lookup"><span data-stu-id="7991d-119">Build the EDM Model</span></span>

<span data-ttu-id="7991d-120">Zum Erstellen des EDM können Sie **odataconaconmodelbuilder**verwenden, das die Vererbungs Beziehungen von den CLR-Typen leitet.</span><span class="sxs-lookup"><span data-stu-id="7991d-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="7991d-121">Außerdem können Sie das EDM mithilfe von **odatamodelta Builder**explizit erstellen.</span><span class="sxs-lookup"><span data-stu-id="7991d-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="7991d-122">Dies erfordert mehr Code, bietet Ihnen jedoch mehr Kontrolle über das EDM.</span><span class="sxs-lookup"><span data-stu-id="7991d-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="7991d-123">In diesen beiden Beispielen wird das gleiche EDM-Schema erstellt.</span><span class="sxs-lookup"><span data-stu-id="7991d-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="7991d-124">Metadatendokument</span><span class="sxs-lookup"><span data-stu-id="7991d-124">Metadata Document</span></span>

<span data-ttu-id="7991d-125">Hier finden Sie das odata-Metadatendokument, das eine komplexe Typvererbung zeigt.</span><span class="sxs-lookup"><span data-stu-id="7991d-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="7991d-126">Im Metadatendokument können Sie Folgendes sehen:</span><span class="sxs-lookup"><span data-stu-id="7991d-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="7991d-127">Der `Shape` komplexer Typ ist abstrakt.</span><span class="sxs-lookup"><span data-stu-id="7991d-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="7991d-128">Die `Rectangle`, `Triangle`und `Circle` komplexen Typs verfügen über den Basistyp `Shape`.</span><span class="sxs-lookup"><span data-stu-id="7991d-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="7991d-129">Der `RoundRectangle` Typ verfügt über den Basistyp `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="7991d-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="7991d-130">Umwandeln komplexer Typen</span><span class="sxs-lookup"><span data-stu-id="7991d-130">Casting Complex Types</span></span>

<span data-ttu-id="7991d-131">Das umwandeln komplexer Typen wird jetzt unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7991d-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="7991d-132">Mit der folgenden Abfrage wird z. b. ein `Shape` in eine `Rectangle`umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="7991d-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="7991d-133">Hier ist die Antwort Nutzlast:</span><span class="sxs-lookup"><span data-stu-id="7991d-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
