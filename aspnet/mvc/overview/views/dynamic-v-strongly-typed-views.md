---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamische im Vergleich zu Stark typisierte Ansichten | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3235fc58fbf93cb87946f8ebd4a478eff7ce80e3
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59386137"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="76ed4-103">Dynamische im Vergleich zu</span><span class="sxs-lookup"><span data-stu-id="76ed4-103">Dynamic v.</span></span> <span data-ttu-id="76ed4-104">Stark typisierten Ansichten</span><span class="sxs-lookup"><span data-stu-id="76ed4-104">Strongly Typed Views</span></span>

<span data-ttu-id="76ed4-105">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="76ed4-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="76ed4-106">Es gibt drei Möglichkeiten zum Übergeben von Informationen von einem Controller an eine Ansicht in ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="76ed4-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="76ed4-107">Als ein stark typisiertes Modell-Objekt.</span><span class="sxs-lookup"><span data-stu-id="76ed4-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="76ed4-108">Als ein dynamischer Typ (mit @model dynamische)</span><span class="sxs-lookup"><span data-stu-id="76ed4-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="76ed4-109">Verwenden die ViewBag-Klasse</span><span class="sxs-lookup"><span data-stu-id="76ed4-109">Using the ViewBag</span></span>

<span data-ttu-id="76ed4-110">Ich habe eine einfache MVC 3 oben Blog-Anwendung zum Vergleichen und gegenüberstellen von dynamischen und stark typisierte Ansichten geschrieben.</span><span class="sxs-lookup"><span data-stu-id="76ed4-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="76ed4-111">Der Controller beginnt mit einer einfachen Liste mit Blogs zu:</span><span class="sxs-lookup"><span data-stu-id="76ed4-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="76ed4-112">Klicken Sie mit der rechten Maustaste in die IndexNotStonglyTyped()-Methode, und fügen Sie eine Razor-Ansicht.</span><span class="sxs-lookup"><span data-stu-id="76ed4-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

[![8<span data-ttu-id="76ed4-113">475.NotStronglyTypedView[1]]</span><span class="sxs-lookup"><span data-stu-id="76ed4-113">475.NotStronglyTypedView[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

<span data-ttu-id="76ed4-114">Stellen Sie sicher, dass die **eine stark typisierte Ansicht erstellen** Kontrollkästchen nicht aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="76ed4-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="76ed4-115">Die resultierende Ansicht enthalten nicht viel:</span><span class="sxs-lookup"><span data-stu-id="76ed4-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="76ed4-116">Da wir ein dynamisches und nicht für eine stark typisierte Ansicht verwenden, nicht in Intellisense uns helfen.</span><span class="sxs-lookup"><span data-stu-id="76ed4-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="76ed4-117">Der vollständige Code wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="76ed4-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6<span data-ttu-id="76ed4-118">646.NotStronglyTypedView_5F00_IE[1]]</span><span class="sxs-lookup"><span data-stu-id="76ed4-118">646.NotStronglyTypedView_5F00_IE[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

<span data-ttu-id="76ed4-119">Jetzt fügen wir eine stark typisierte Ansicht.</span><span class="sxs-lookup"><span data-stu-id="76ed4-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="76ed4-120">Fügen Sie den folgenden Code, mit dem Controller:</span><span class="sxs-lookup"><span data-stu-id="76ed4-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="76ed4-121">Beachten Sie, dass sie genau die gleichen return View(topBlogs) ist. Rufen Sie als nicht stark typisierten Ansicht.</span><span class="sxs-lookup"><span data-stu-id="76ed4-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="76ed4-122">Klicken Sie mit der rechten Maustaste innerhalb des *StonglyTypedIndex()* , und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="76ed4-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="76ed4-123">Wählen Sie dieses Mal die **Blog** Modellklasse, und wählen Sie **Liste** als Gerüst Vorlage.</span><span class="sxs-lookup"><span data-stu-id="76ed4-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

[![5<span data-ttu-id="76ed4-124">658.StrongView[1]]</span><span class="sxs-lookup"><span data-stu-id="76ed4-124">658.StrongView[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

<span data-ttu-id="76ed4-125">In der neuen ansichtsvorlage erhalten wir Intellisense-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="76ed4-125">Inside the new view template we get intellisense support.</span></span>

[![7<span data-ttu-id="76ed4-126">002.IntelliSense[1]]</span><span class="sxs-lookup"><span data-stu-id="76ed4-126">002.IntelliSense[1]]</span></span>(dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

<span data-ttu-id="76ed4-127">C#-Projekt kann heruntergeladen werden [hier](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="76ed4-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
