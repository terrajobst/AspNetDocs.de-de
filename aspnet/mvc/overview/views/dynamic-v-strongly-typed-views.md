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
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455656"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="44cf3-103">Dynamische im Vergleich zu</span><span class="sxs-lookup"><span data-stu-id="44cf3-103">Dynamic v.</span></span> <span data-ttu-id="44cf3-104">Stark typisierten Ansichten</span><span class="sxs-lookup"><span data-stu-id="44cf3-104">Strongly Typed Views</span></span>

<span data-ttu-id="44cf3-105">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="44cf3-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44cf3-106">Es gibt drei Möglichkeiten, Informationen von einem Controller an eine Ansicht in ASP.NET MVC 3 zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="44cf3-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="44cf3-107">Als stark typisiertes Modell Objekt.</span><span class="sxs-lookup"><span data-stu-id="44cf3-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="44cf3-108">Als dynamischer Typ (mit @model Dynamic)</span><span class="sxs-lookup"><span data-stu-id="44cf3-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="44cf3-109">Verwenden der viewbag</span><span class="sxs-lookup"><span data-stu-id="44cf3-109">Using the ViewBag</span></span>

<span data-ttu-id="44cf3-110">Ich habe eine einfache Top-Blog Anwendung von MVC 3 geschrieben, um dynamische und stark typisierte Ansichten zu vergleichen und zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="44cf3-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="44cf3-111">Der Controller beginnt mit einer einfachen Liste von Blogs:</span><span class="sxs-lookup"><span data-stu-id="44cf3-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="44cf3-112">Klicken Sie mit der rechten Maustaste in die indexnotstonglytypi()-Methode, und fügen Sie eine Razor-Ansicht hinzu</span><span class="sxs-lookup"><span data-stu-id="44cf3-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="44cf3-113">[![8475. notstronglytypedview [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="44cf3-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="44cf3-114">Stellen Sie sicher, dass das Feld **eine stark typisierte Ansicht erstellen** nicht aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="44cf3-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="44cf3-115">Die resultierende Sicht enthält nicht viel:</span><span class="sxs-lookup"><span data-stu-id="44cf3-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="44cf3-116">Da wir eine dynamische und keine stark typisierte Ansicht verwenden, unterstützt IntelliSense uns nicht.</span><span class="sxs-lookup"><span data-stu-id="44cf3-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="44cf3-117">Der abgeschlossene Code wird unten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="44cf3-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="44cf3-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="44cf3-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="44cf3-119">Nun fügen wir eine stark typisierte Ansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="44cf3-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="44cf3-120">Fügen Sie dem Controller den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="44cf3-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="44cf3-121">Beachten Sie, dass es sich genau um dieselbe Rückgabe Ansicht (TopBlogs) handelt. als nicht stark typisierte Ansicht aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="44cf3-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="44cf3-122">Klicken Sie mit der rechten Maustaste in *stonglytypedindex ()* , und wählen Sie **Ansicht hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="44cf3-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="44cf3-123">Wählen Sie diesmal die **Blog** Modell Klasse aus, und wählen Sie **Liste** als Gerüst Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="44cf3-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="44cf3-124">[![5658). strongview [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="44cf3-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="44cf3-125">In der neuen Ansichts Vorlage erhalten Sie IntelliSense-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="44cf3-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="44cf3-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="44cf3-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="44cf3-127">Das c#-Projekt kann [hier](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="44cf3-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
