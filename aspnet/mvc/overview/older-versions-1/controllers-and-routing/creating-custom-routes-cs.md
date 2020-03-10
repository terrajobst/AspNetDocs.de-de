---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Erstellen von benutzerdefiniertenC#Routen () | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie einer ASP.NET MVC-Anwendung benutzerdefinierte Routen hinzufügen. In diesem Tutorial erfahren Sie, wie Sie die Standardrouten Tabelle in der Datei "Global. asax" ändern.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486723"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="bef51-104">Erstellen von benutzerdefinierten Routen (C#)</span><span class="sxs-lookup"><span data-stu-id="bef51-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="bef51-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bef51-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bef51-106">Erfahren Sie, wie Sie einer ASP.NET MVC-Anwendung benutzerdefinierte Routen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bef51-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="bef51-107">In diesem Tutorial erfahren Sie, wie Sie die Standardrouten Tabelle in der Datei "Global. asax" ändern.</span><span class="sxs-lookup"><span data-stu-id="bef51-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="bef51-108">In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte Route zu einer ASP.NET MVC-Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bef51-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="bef51-109">Sie erfahren, wie Sie die Standardrouten Tabelle in der Datei "Global. asax" mit einer benutzerdefinierten Route ändern.</span><span class="sxs-lookup"><span data-stu-id="bef51-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="bef51-110">Für viele einfache ASP.NET-MVC-Anwendungen funktioniert die Standardrouten Tabelle problemlos.</span><span class="sxs-lookup"><span data-stu-id="bef51-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="bef51-111">Allerdings können Sie feststellen, dass Sie über spezielle Routing Anforderungen verfügen.</span><span class="sxs-lookup"><span data-stu-id="bef51-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="bef51-112">In diesem Fall können Sie eine benutzerdefinierte Route erstellen.</span><span class="sxs-lookup"><span data-stu-id="bef51-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="bef51-113">Stellen Sie sich beispielsweise vor, dass Sie eine Blog Anwendung entwickeln.</span><span class="sxs-lookup"><span data-stu-id="bef51-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="bef51-114">Möglicherweise möchten Sie eingehende Anforderungen verarbeiten, die wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="bef51-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="bef51-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="bef51-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="bef51-116">Wenn ein Benutzer diese Anforderung eingibt, möchten Sie den Blogeintrag zurückgeben, der dem Datum 12/25/2009 entspricht.</span><span class="sxs-lookup"><span data-stu-id="bef51-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="bef51-117">Um diese Art von Anforderung zu verarbeiten, müssen Sie eine benutzerdefinierte Route erstellen.</span><span class="sxs-lookup"><span data-stu-id="bef51-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="bef51-118">Die Datei "Global. asax" in der Liste 1 enthält eine neue benutzerdefinierte Route mit dem Namen "Blog", die Anforderungen behandelt, die wie/Archive/*Entry Date*aussehen.</span><span class="sxs-lookup"><span data-stu-id="bef51-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="bef51-119">**Codebeispiel 1: Global. asax (mit benutzerdefinierter Route)**</span><span class="sxs-lookup"><span data-stu-id="bef51-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="bef51-120">Die Reihenfolge der Routen, die Sie der Routentabelle hinzufügen, ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="bef51-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="bef51-121">Unsere neue benutzerdefinierte Blog Route wird vor der vorhandenen Standardroute hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bef51-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="bef51-122">Wenn Sie die Reihenfolge umkehren, wird die Standardroute immer anstelle der benutzerdefinierten Route aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="bef51-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="bef51-123">Die benutzerdefinierte Blog Route stimmt mit allen Anforderungen überein, die mit/Archive/. beginnen.</span><span class="sxs-lookup"><span data-stu-id="bef51-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="bef51-124">Dies entspricht allen folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="bef51-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="bef51-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="bef51-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="bef51-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="bef51-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="bef51-127">/Archive/apple</span><span class="sxs-lookup"><span data-stu-id="bef51-127">/Archive/apple</span></span>

<span data-ttu-id="bef51-128">Die benutzerdefinierte Route ordnet die eingehende Anforderung einem Controller namens Archive zu und ruft die Aktion Entry () auf.</span><span class="sxs-lookup"><span data-stu-id="bef51-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="bef51-129">Wenn die Entry ()-Methode aufgerufen wird, wird das Eintrags Datum als Parameter namens entrydate übergeben.</span><span class="sxs-lookup"><span data-stu-id="bef51-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="bef51-130">Sie können die benutzerdefinierte Blog Route mit dem Controller in der Liste 2 verwenden.</span><span class="sxs-lookup"><span data-stu-id="bef51-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="bef51-131">**Codebeispiel 2: ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="bef51-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="bef51-132">Beachten Sie, dass die Entry ()-Methode in der Liste 2 einen Parameter vom Typ DateTime akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="bef51-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="bef51-133">Das MVC-Framework ist intelligent genug, um das Eingangsdatum automatisch von der URL in einen DateTime-Wert zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="bef51-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="bef51-134">Wenn der Parameter Entry Date von der URL nicht in einen DateTime-Wert konvertiert werden kann, wird ein Fehler ausgelöst (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="bef51-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="bef51-135">**Abbildung 1: Fehler beim Umrechnen des Parameters**</span><span class="sxs-lookup"><span data-stu-id="bef51-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="bef51-136">[![des Dialog Felds "Neues Projekt"](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bef51-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="bef51-137">**Abbildung 01**: Fehler beim Umrechnen des Parameters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="bef51-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="bef51-138">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="bef51-138">Summary</span></span>

<span data-ttu-id="bef51-139">In diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierte Route erstellen können.</span><span class="sxs-lookup"><span data-stu-id="bef51-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="bef51-140">Sie haben gelernt, wie Sie der Routing Tabelle in der Datei "Global. asax", die Blogeinträge darstellt, eine benutzerdefinierte Route hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bef51-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="bef51-141">Wir haben erläutert, wie Anforderungen für Blogeinträge einem Controller namens archivecontroller und einer Controller Aktion namens Entry () zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="bef51-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bef51-142">[Zurück](aspnet-mvc-controllers-overview-cs.md)
> [Weiter](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bef51-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
