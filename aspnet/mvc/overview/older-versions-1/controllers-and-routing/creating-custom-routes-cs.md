---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Erstellen von benutzerdefinierten Routen (c#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie benutzerdefinierte Routen zu einer ASP.NET MVC-Anwendung hinzufügen. In diesem Tutorial erfahren Sie, wie die Standardroutingtabelle in der Datei "Global.asax" zu ändern.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123360"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="f8962-104">Erstellen von benutzerdefinierten Routen (C#)</span><span class="sxs-lookup"><span data-stu-id="f8962-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="f8962-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f8962-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f8962-106">Erfahren Sie, wie Sie benutzerdefinierte Routen zu einer ASP.NET MVC-Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f8962-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="f8962-107">In diesem Tutorial erfahren Sie, wie die Standardroutingtabelle in der Datei "Global.asax" zu ändern.</span><span class="sxs-lookup"><span data-stu-id="f8962-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="f8962-108">In diesem Tutorial erfahren Sie, wie eine benutzerdefinierte Route zu einer ASP.NET MVC-Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f8962-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="f8962-109">Erfahren Sie, wie die Standardroutingtabelle in der Datei "Global.asax" mit einer benutzerdefinierten Route ändern.</span><span class="sxs-lookup"><span data-stu-id="f8962-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="f8962-110">Für viele einfache ASP.NET MVC-Anwendungen funktioniert die Standardroutingtabelle.</span><span class="sxs-lookup"><span data-stu-id="f8962-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="f8962-111">Allerdings könnten Sie feststellen, dass Sie Routinganforderungen spezielle.</span><span class="sxs-lookup"><span data-stu-id="f8962-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="f8962-112">In diesem Fall können Sie eine benutzerdefinierte Route erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8962-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="f8962-113">Stellen Sie sich vor, z. B., dass Sie eine Bloganwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8962-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="f8962-114">Möglicherweise möchten die eingehenden Anforderungen zu verarbeiten, die wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="f8962-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="f8962-115">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="f8962-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="f8962-116">Wenn ein Benutzer diese Anforderung eingibt, zurückgegeben werden soll den Blogeintrag, entspricht das Datum 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="f8962-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="f8962-117">Um diese Art von Anforderung zu verarbeiten, müssen Sie eine benutzerdefinierte Route erstellen.</span><span class="sxs-lookup"><span data-stu-id="f8962-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="f8962-118">Die Datei "Global.asax" in Codebeispiel 1 enthält eine neue benutzerdefinierte Route, die mit dem Namen Blog, welche verarbeitet Anforderungen, die wie /Archive/ aussehen*Eingabedatum*.</span><span class="sxs-lookup"><span data-stu-id="f8962-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="f8962-119">**Codebeispiel 1 – "Global.asax" (mit benutzerdefinierten Route)**</span><span class="sxs-lookup"><span data-stu-id="f8962-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="f8962-120">Die Reihenfolge der Routen, die Sie, um die Routentabelle hinzufügen ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="f8962-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="f8962-121">Unsere neuen benutzerdefinierten Blog-Route wird vor der vorhandenen Standardroute hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f8962-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="f8962-122">Wenn Sie die Reihenfolge umgekehrt, wird Klicken Sie dann die Standardroute immer anstelle der benutzerdefinierten Route aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="f8962-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="f8962-123">Die benutzerdefinierte Route für den Blog entspricht jede Anforderung, die mit/Archive/beginnt.</span><span class="sxs-lookup"><span data-stu-id="f8962-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="f8962-124">Daher liegt eine Übereinstimmung mit allen der folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="f8962-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="f8962-125">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="f8962-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="f8962-126">/ Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="f8962-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="f8962-127">/ Archive/apple</span><span class="sxs-lookup"><span data-stu-id="f8962-127">/Archive/apple</span></span>

<span data-ttu-id="f8962-128">Die benutzerdefinierte Route ordnet die eingehende Anforderung an einen Controller mit dem Namen Archiv und die Entry() Aktion aufruft.</span><span class="sxs-lookup"><span data-stu-id="f8962-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="f8962-129">Wenn die Entry()-Methode aufgerufen wird, wird das Eingabedatum als einen Parameter namens EntryDate übergeben.</span><span class="sxs-lookup"><span data-stu-id="f8962-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="f8962-130">Sie können die benutzerdefinierte Blog-Route mit dem Controller im Codebeispiel 2 verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8962-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="f8962-131">**Codebeispiel 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="f8962-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="f8962-132">Beachten Sie, dass die Methode Entry() Programmausdruck 2 einen Parameter vom Typ "DateTime" akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="f8962-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="f8962-133">Das MVC-Framework ist intelligent genug, um das Datum des Eintrags aus der URL automatisch in einen DateTime-Wert konvertieren.</span><span class="sxs-lookup"><span data-stu-id="f8962-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="f8962-134">Wenn der Eintrag Date-Parameter aus der URL in einen datetime-Wert konvertiert werden kann, wird ein Fehler ausgelöst (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="f8962-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="f8962-135">**Abbildung 1: Fehler in Parameter konvertieren**</span><span class="sxs-lookup"><span data-stu-id="f8962-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="f8962-136">[![Das Dialogfeld "Neues Projekt"](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f8962-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="f8962-137">**Abbildung 01**: Fehler durch Konvertieren der Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f8962-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="f8962-138">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f8962-138">Summary</span></span>

<span data-ttu-id="f8962-139">Das Ziel dieses Lernprogramms wurde veranschaulicht, wie Sie eine benutzerdefinierte Route erstellen können.</span><span class="sxs-lookup"><span data-stu-id="f8962-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="f8962-140">Sie haben gelernt, wie eine benutzerdefinierte Route der Routingtabelle, in der Datei "Global.asax" hinzugefügt, der Blog-Einträge darstellt.</span><span class="sxs-lookup"><span data-stu-id="f8962-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="f8962-141">Wir erläutert, wie Anforderungen für Blogeinträge auf einen Controller, die mit dem Namen ArchiveController und eine Controlleraktion, die mit dem Namen Entry() zuordnen.</span><span class="sxs-lookup"><span data-stu-id="f8962-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8962-142">[Zurück](aspnet-mvc-controllers-overview-cs.md)
> [Weiter](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f8962-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
