---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Kapselung in odata V4 mithilfe der Web-API 2,2 | Microsoft-Dokumentation
author: rick-anderson
description: 'In der Regel konnte auf eine Entität nur zugegriffen werden, wenn Sie in einer Entitätenmenge gekapselt wurde Odata v4 bietet jedoch zwei zusätzliche Optionen: Singleton und con...'
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421401"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="d9967-104">Kapselung in odata V4 mithilfe der Web-API 2,2</span><span class="sxs-lookup"><span data-stu-id="d9967-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="d9967-105">von jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="d9967-105">by Jinfu Tan</span></span>

> <span data-ttu-id="d9967-106">In der Regel konnte auf eine Entität nur zugegriffen werden, wenn Sie in einer Entitätenmenge gekapselt wurde</span><span class="sxs-lookup"><span data-stu-id="d9967-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="d9967-107">Odata v4 bietet jedoch zwei zusätzliche Optionen: Singleton und Containment, die beide von WebAPI 2,2 unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="d9967-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="d9967-108">In diesem Thema wird gezeigt, wie Sie eine Kapselung in einem odata-Endpunkt in WebAPI 2,2 definieren.</span><span class="sxs-lookup"><span data-stu-id="d9967-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="d9967-109">Weitere Informationen zur Kapselung finden Sie unter [Containment ist mit odata V4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="d9967-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="d9967-110">Informationen zum Erstellen eines odata V4-Endpunkts in der Web-API finden [Sie unter Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d9967-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="d9967-111">Zunächst erstellen wir ein Containment-Domänen Modell im odata-Dienst mit diesem Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="d9967-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Datenmodell](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="d9967-113">Ein Konto enthält viele paymentinstruments (PI), aber wir definieren keine Entitätenmenge für einen Pi.</span><span class="sxs-lookup"><span data-stu-id="d9967-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="d9967-114">Stattdessen kann nur über ein Konto auf die PiS zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="d9967-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="d9967-115">Sie können die in diesem Thema verwendete Lösung von [codeplex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="d9967-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="d9967-116">Definieren des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="d9967-116">Defining the data model</span></span>

1. <span data-ttu-id="d9967-117">Definieren Sie die CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="d9967-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="d9967-118">Das `Contained`-Attribut wird für Eigenschaften der Einschluss Navigation verwendet.</span><span class="sxs-lookup"><span data-stu-id="d9967-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="d9967-119">Generieren Sie das EDM-Modell basierend auf den CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="d9967-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="d9967-120">Der `ODataConventionModelBuilder` behandelt das EDM-Modell, wenn das `Contained`-Attribut der entsprechenden Navigations Eigenschaft hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="d9967-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="d9967-121">Wenn die Eigenschaft ein Sammlungstyp ist, wird auch eine `GetCount(string NameContains)` Funktion erstellt.</span><span class="sxs-lookup"><span data-stu-id="d9967-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="d9967-122">Die generierten Metadaten sehen wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="d9967-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="d9967-123">Das `ContainsTarget`-Attribut gibt an, dass die Navigations Eigenschaft eine Containment ist.</span><span class="sxs-lookup"><span data-stu-id="d9967-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="d9967-124">Definieren des enthaltenden entitätenmengencontrollers</span><span class="sxs-lookup"><span data-stu-id="d9967-124">Define the containing entity set controller</span></span>

<span data-ttu-id="d9967-125">Enthaltene Entitäten haben keinen eigenen Controller. die Aktion wird im enthaltenden entitätenmengencontroller definiert.</span><span class="sxs-lookup"><span data-stu-id="d9967-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="d9967-126">In diesem Beispiel gibt es einen accounst Controller, aber keinen paymentinstrumenscontroller.</span><span class="sxs-lookup"><span data-stu-id="d9967-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="d9967-127">Wenn der odata-Pfad 4 oder mehr Segmente ist, funktioniert nur das Attribut Routing, wie z. b. `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` im obigen Controller.</span><span class="sxs-lookup"><span data-stu-id="d9967-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="d9967-128">Andernfalls funktionieren sowohl das Attribut als auch das herkömmliche Routing: beispielsweise `GetPayInPIs(int key)` Übereinstimmungen `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="d9967-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="d9967-129">*Vielen Dank für den ursprünglichen Inhalt dieses Artikels.*</span><span class="sxs-lookup"><span data-stu-id="d9967-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
