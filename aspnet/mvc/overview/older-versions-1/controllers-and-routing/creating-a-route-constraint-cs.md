---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Erstellen einer Routen Einschränkung (C#) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial demonstriert Stephen Walther, wie Sie steuern können, wie Browser Anforderungen Routen entsprechen, indem Sie Routen Einschränkungen mit regulären Ausdrücken erstellen.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51ef859287b3424faf85f4a3606a220ab48a9466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470163"
---
# <a name="creating-a-route-constraint-c"></a><span data-ttu-id="652d5-103">Erstellen einer Routeneinschränkung (C#)</span><span class="sxs-lookup"><span data-stu-id="652d5-103">Creating a Route Constraint (C#)</span></span>

<span data-ttu-id="652d5-104">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="652d5-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="652d5-105">In diesem Tutorial demonstriert Stephen Walther, wie Sie steuern können, wie Browser Anforderungen Routen entsprechen, indem Sie Routen Einschränkungen mit regulären Ausdrücken erstellen.</span><span class="sxs-lookup"><span data-stu-id="652d5-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>

<span data-ttu-id="652d5-106">Sie verwenden Routen Einschränkungen, um die Browser Anforderungen zu beschränken, die einer bestimmten Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="652d5-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="652d5-107">Sie können einen regulären Ausdruck verwenden, um eine Routen Einschränkung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="652d5-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="652d5-108">Angenommen, Sie haben die Route in der Datei "Global. asax" in der Datei "Global. asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="652d5-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="652d5-109">**Codebeispiel 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="652d5-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="652d5-110">Codebeispiel 1 enthält eine Route mit dem Namen Product.</span><span class="sxs-lookup"><span data-stu-id="652d5-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="652d5-111">Sie können die Produkt Route verwenden, um dem ProductController in der Liste 2 Browser Anforderungen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="652d5-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="652d5-112">**Codebeispiel 2: controllers\productcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="652d5-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="652d5-113">Beachten Sie, dass die vom Produkt Controller verfügbar gemachte Aktion "Details ()" einen einzelnen Parameter namens "ProductID" annimmt.</span><span class="sxs-lookup"><span data-stu-id="652d5-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="652d5-114">Dieser Parameter ist ein ganzzahliger Parameter.</span><span class="sxs-lookup"><span data-stu-id="652d5-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="652d5-115">Die in der Liste 1 definierte Route entspricht einer der folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="652d5-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="652d5-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="652d5-116">/Product/23</span></span>
- <span data-ttu-id="652d5-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="652d5-117">/Product/7</span></span>

<span data-ttu-id="652d5-118">Leider entspricht die Route auch den folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="652d5-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="652d5-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="652d5-119">/Product/blah</span></span>
- <span data-ttu-id="652d5-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="652d5-120">/Product/apple</span></span>

<span data-ttu-id="652d5-121">Da die Details ()-Aktion einen Integer-Parameter erwartet, wird eine Anforderung, die einen anderen Wert als einen ganzzahligen Wert enthält, zu einem Fehler führen.</span><span class="sxs-lookup"><span data-stu-id="652d5-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="652d5-122">Wenn Sie z. b. die URL/Product/Apple in Ihren Browser eingeben, erhalten Sie die Fehlerseite in Abbildung 1.</span><span class="sxs-lookup"><span data-stu-id="652d5-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>

<span data-ttu-id="652d5-123">[![des Dialog Felds "Neues Projekt"](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="652d5-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="652d5-124">**Abbildung 01**: Anzeigen einer Seiten Explosion ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="652d5-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>

<span data-ttu-id="652d5-125">Sie möchten nur URLs zuordnen, die eine richtige ganzzahlige ProductID enthalten.</span><span class="sxs-lookup"><span data-stu-id="652d5-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="652d5-126">Sie können eine Einschränkung verwenden, wenn Sie eine Route definieren, um die URLs einzuschränken, die der Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="652d5-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="652d5-127">Die geänderte Produkt Route in der Liste 3 enthält eine Einschränkung für reguläre Ausdrücke, die nur mit ganzen Zahlen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="652d5-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="652d5-128">**Codebeispiel 3-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="652d5-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="652d5-129">Der reguläre Ausdruck \d + stimmt mit mindestens einem integerausdruck überein.</span><span class="sxs-lookup"><span data-stu-id="652d5-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="652d5-130">Diese Einschränkung bewirkt, dass die Produkt Route den folgenden URLs entspricht:</span><span class="sxs-lookup"><span data-stu-id="652d5-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="652d5-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="652d5-131">/Product/3</span></span>
- <span data-ttu-id="652d5-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="652d5-132">/Product/8999</span></span>

<span data-ttu-id="652d5-133">Aber nicht die folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="652d5-133">But not the following URLs:</span></span>

- <span data-ttu-id="652d5-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="652d5-134">/Product/apple</span></span>
- <span data-ttu-id="652d5-135">/Product</span><span class="sxs-lookup"><span data-stu-id="652d5-135">/Product</span></span>

- <span data-ttu-id="652d5-136">Diese Browser Anforderungen werden von einer anderen Route verarbeitet, oder, wenn keine übereinstimmenden Routen vorhanden sind, wird die Fehlermeldung " *die Ressource wurde nicht gefunden* " zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="652d5-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="652d5-137">[Zurück](creating-custom-routes-cs.md)
> [Weiter](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="652d5-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
