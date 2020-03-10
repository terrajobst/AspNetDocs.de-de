---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Erstellen einer Routen Einschränkung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial demonstriert Stephen Walther, wie Sie steuern können, wie Browser Anforderungen Routen entsprechen, indem Sie Routen Einschränkungen mit regulären Ausdrücken erstellen.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486771"
---
# <a name="creating-a-route-constraint-vb"></a><span data-ttu-id="b0df9-103">Erstellen einer Routeneinschränkung (VB)</span><span class="sxs-lookup"><span data-stu-id="b0df9-103">Creating a Route Constraint (VB)</span></span>

<span data-ttu-id="b0df9-104">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b0df9-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b0df9-105">In diesem Tutorial demonstriert Stephen Walther, wie Sie steuern können, wie Browser Anforderungen Routen entsprechen, indem Sie Routen Einschränkungen mit regulären Ausdrücken erstellen.</span><span class="sxs-lookup"><span data-stu-id="b0df9-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>

<span data-ttu-id="b0df9-106">Sie verwenden Routen Einschränkungen, um die Browser Anforderungen zu beschränken, die einer bestimmten Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b0df9-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="b0df9-107">Sie können einen regulären Ausdruck verwenden, um eine Routen Einschränkung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="b0df9-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="b0df9-108">Angenommen, Sie haben die Route in der Datei "Global. asax" in der Datei "Global. asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="b0df9-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="b0df9-109">**Codebeispiel 1: Global. asax. vb**</span><span class="sxs-lookup"><span data-stu-id="b0df9-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="b0df9-110">Codebeispiel 1 enthält eine Route mit dem Namen Product.</span><span class="sxs-lookup"><span data-stu-id="b0df9-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="b0df9-111">Sie können die Produkt Route verwenden, um dem ProductController in der Liste 2 Browser Anforderungen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="b0df9-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="b0df9-112">**Codebeispiel 2: controllers\productcontroller.vb**</span><span class="sxs-lookup"><span data-stu-id="b0df9-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="b0df9-113">Beachten Sie, dass die vom Produkt Controller verfügbar gemachte Aktion "Details ()" einen einzelnen Parameter namens "ProductID" annimmt.</span><span class="sxs-lookup"><span data-stu-id="b0df9-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="b0df9-114">Dieser Parameter ist ein ganzzahliger Parameter.</span><span class="sxs-lookup"><span data-stu-id="b0df9-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="b0df9-115">Die in der Liste 1 definierte Route entspricht einer der folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="b0df9-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="b0df9-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="b0df9-116">/Product/23</span></span>
- <span data-ttu-id="b0df9-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="b0df9-117">/Product/7</span></span>

<span data-ttu-id="b0df9-118">Leider entspricht die Route auch den folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="b0df9-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="b0df9-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="b0df9-119">/Product/blah</span></span>
- <span data-ttu-id="b0df9-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="b0df9-120">/Product/apple</span></span>

<span data-ttu-id="b0df9-121">Da die Details ()-Aktion einen Integer-Parameter erwartet, wird eine Anforderung, die einen anderen Wert als einen ganzzahligen Wert enthält, zu einem Fehler führen.</span><span class="sxs-lookup"><span data-stu-id="b0df9-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="b0df9-122">Wenn Sie z. b. die URL/Product/Apple in Ihren Browser eingeben, erhalten Sie die Fehlerseite in Abbildung 1.</span><span class="sxs-lookup"><span data-stu-id="b0df9-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>

<span data-ttu-id="b0df9-123">[![des Dialog Felds "Neues Projekt"](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0df9-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="b0df9-124">**Abbildung 01**: Anzeigen einer Seiten Explosion ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b0df9-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>

<span data-ttu-id="b0df9-125">Sie möchten nur URLs zuordnen, die eine richtige ganzzahlige ProductID enthalten.</span><span class="sxs-lookup"><span data-stu-id="b0df9-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="b0df9-126">Sie können eine Einschränkung verwenden, wenn Sie eine Route definieren, um die URLs einzuschränken, die der Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b0df9-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="b0df9-127">Die geänderte Produkt Route in der Liste 3 enthält eine Einschränkung für reguläre Ausdrücke, die nur mit ganzen Zahlen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b0df9-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="b0df9-128">**Codebeispiel 3: Global. asax. vb**</span><span class="sxs-lookup"><span data-stu-id="b0df9-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="b0df9-129">Der reguläre Ausdruck \d + stimmt mit mindestens einem integerausdruck überein.</span><span class="sxs-lookup"><span data-stu-id="b0df9-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="b0df9-130">Diese Einschränkung bewirkt, dass die Produkt Route den folgenden URLs entspricht:</span><span class="sxs-lookup"><span data-stu-id="b0df9-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="b0df9-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="b0df9-131">/Product/3</span></span>
- <span data-ttu-id="b0df9-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="b0df9-132">/Product/8999</span></span>

<span data-ttu-id="b0df9-133">Aber nicht die folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="b0df9-133">But not the following URLs:</span></span>

- <span data-ttu-id="b0df9-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="b0df9-134">/Product/apple</span></span>
- <span data-ttu-id="b0df9-135">/Product</span><span class="sxs-lookup"><span data-stu-id="b0df9-135">/Product</span></span>

<span data-ttu-id="b0df9-136">Diese Browser Anforderungen werden von einer anderen Route verarbeitet, oder, wenn keine übereinstimmenden Routen vorhanden sind, wird die Fehlermeldung " *die Ressource wurde nicht gefunden* " zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b0df9-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0df9-137">[Zurück](creating-custom-routes-vb.md)
> [Weiter](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b0df9-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
