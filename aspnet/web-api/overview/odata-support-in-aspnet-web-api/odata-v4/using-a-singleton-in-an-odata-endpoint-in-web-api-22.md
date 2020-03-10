---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Erstellen eines Singleton in odata V4 mithilfe der Web-API 2,2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Thema wird gezeigt, wie ein Singleton in einem odata-Endpunkt in der Web-API 2,2 definiert wird.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504501"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="ea272-103">Erstellen eines Singleton in odata V4 mithilfe der Web-API 2,2</span><span class="sxs-lookup"><span data-stu-id="ea272-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="ea272-104">von Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="ea272-104">by Zoe Luo</span></span>

> <span data-ttu-id="ea272-105">In der Regel konnte auf eine Entität nur zugegriffen werden, wenn Sie in einer Entitätenmenge gekapselt wurde</span><span class="sxs-lookup"><span data-stu-id="ea272-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="ea272-106">Odata v4 bietet jedoch zwei zusätzliche Optionen: Singleton und Containment, die beide von WebAPI 2,2 unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="ea272-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="ea272-107">In diesem Artikel wird gezeigt, wie ein Singleton in einem odata-Endpunkt in der Web-API 2,2 definiert wird.</span><span class="sxs-lookup"><span data-stu-id="ea272-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="ea272-108">Informationen dazu, was ein Singleton ist und wie Sie davon profitieren können, finden Sie unter [Verwenden eines Singleton zum Definieren Ihrer speziellen Entität](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea272-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="ea272-109">Informationen zum Erstellen eines odata V4-Endpunkts in der Web-API finden [Sie unter Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="ea272-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="ea272-110">Wir erstellen einen Singleton in Ihrem Web-API-Projekt mit dem folgenden Datenmodell:</span><span class="sxs-lookup"><span data-stu-id="ea272-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Datenmodell](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="ea272-112">Ein Singleton namens `Umbrella` wird basierend auf dem Typ `Company`definiert, und eine Entitätenmenge mit dem Namen `Employees` wird basierend auf dem Typ `Employee`definiert.</span><span class="sxs-lookup"><span data-stu-id="ea272-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="ea272-113">Die in diesem Tutorial verwendete Lösung kann von [codeplex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="ea272-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="ea272-114">Definieren des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="ea272-114">Define the data model</span></span>

1. <span data-ttu-id="ea272-115">Definieren Sie die CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="ea272-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="ea272-116">Generieren Sie das EDM-Modell basierend auf den CLR-Typen.</span><span class="sxs-lookup"><span data-stu-id="ea272-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="ea272-117">Hier weist `builder.Singleton<Company>("Umbrella")` den Modell-Generator an, im EDM-Modell einen Singleton mit dem Namen `Umbrella` zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea272-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="ea272-118">Die generierten Metadaten sehen wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="ea272-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="ea272-119">In den Metadaten sehen wir, dass die Navigations Eigenschaft `Company` in der `Employees` Entitätenmenge an den Singleton-`Umbrella`gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="ea272-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="ea272-120">Die Bindung wird automatisch durch `ODataConventionModelBuilder`durchgeführt, da nur `Umbrella` den `Company`-Typ aufweist.</span><span class="sxs-lookup"><span data-stu-id="ea272-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="ea272-121">Wenn im Modell Mehrdeutigkeiten vorliegen, können Sie `HasSingletonBinding` verwenden, um eine Navigations Eigenschaft explizit an einen Singleton zu binden. `HasSingletonBinding` hat dieselbe Auswirkung wie das Verwenden des `Singleton`-Attributs in der CLR-Typdefinition:</span><span class="sxs-lookup"><span data-stu-id="ea272-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="ea272-122">Definieren des Singleton-Controllers</span><span class="sxs-lookup"><span data-stu-id="ea272-122">Define the singleton controller</span></span>

<span data-ttu-id="ea272-123">Wie der EntitySet-Controller erbt der Singleton-Controller von `ODataController`, und der Name des Singleton Controllers sollte `[singletonName]Controller`sein.</span><span class="sxs-lookup"><span data-stu-id="ea272-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="ea272-124">Um verschiedene Arten von Anforderungen verarbeiten zu können, müssen Aktionen im Controller vordefiniert werden.</span><span class="sxs-lookup"><span data-stu-id="ea272-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="ea272-125">**Attribut Routing** ist in WebAPI 2,2 standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="ea272-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="ea272-126">Um z. b. eine Aktion zu definieren, mit der die Abfrage `Revenue` von `Company` mithilfe des Attribut Routings behandelt werden soll, verwenden Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ea272-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="ea272-127">Wenn Sie nicht bereit sind, Attribute für jede Aktion zu definieren, definieren Sie einfach Ihre Aktionen nach [odata-Routing Konventionen](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="ea272-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="ea272-128">Da ein Schlüssel für das Abfragen eines Singleton nicht erforderlich ist, unterscheiden sich die im Singleton-Controller definierten Aktionen geringfügig von den Aktionen, die im EntitySet-Controller definiert sind.</span><span class="sxs-lookup"><span data-stu-id="ea272-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="ea272-129">Zur Referenz sind die Methoden Signaturen für jede Aktions Definition im Singleton-Controller unten aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="ea272-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="ea272-130">Im Grunde ist dies alles, was Sie auf der Dienst Seite tun müssen.</span><span class="sxs-lookup"><span data-stu-id="ea272-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="ea272-131">Das [Beispiel Projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) enthält den gesamten Code für die Lösung und den odata-Client, der die Verwendung des Singleton veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="ea272-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="ea272-132">Der-Client wird erstellt, indem die Schritte unter [Erstellen einer odata V4-Client-App](create-an-odata-v4-client-app.md)ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ea272-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="ea272-133">erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ea272-133">.</span></span> 

<span data-ttu-id="ea272-134">*Vielen Dank für den ursprünglichen Inhalt dieses Artikels.*</span><span class="sxs-lookup"><span data-stu-id="ea272-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
