---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Erstellen einer benutzerdefinierten Routen EinschränkungC#() | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486825"
---
# <a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="d2afa-104">Erstellen einer benutzerdefinierten Routeneinschränkung (C#)</span><span class="sxs-lookup"><span data-stu-id="d2afa-104">Creating a Custom Route Constraint (C#)</span></span>

<span data-ttu-id="d2afa-105">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d2afa-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d2afa-106">Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="d2afa-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="d2afa-107">Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird, wenn eine Browser Anforderung von einem Remote Computer aus erfolgt.</span><span class="sxs-lookup"><span data-stu-id="d2afa-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="d2afa-108">In diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="d2afa-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="d2afa-109">Mit einer benutzerdefinierten Routen Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung stimmt überein.</span><span class="sxs-lookup"><span data-stu-id="d2afa-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="d2afa-110">In diesem Tutorial erstellen wir eine localhost-Routen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="d2afa-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="d2afa-111">Die localhost-Routen Einschränkung entspricht nur Anforderungen, die vom lokalen Computer ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="d2afa-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="d2afa-112">Remote Anforderungen aus dem Internet stimmen nicht überein.</span><span class="sxs-lookup"><span data-stu-id="d2afa-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="d2afa-113">Sie implementieren eine benutzerdefinierte Routen Einschränkung, indem Sie die irouteeinschränkung-Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="d2afa-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="d2afa-114">Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschreibt:</span><span class="sxs-lookup"><span data-stu-id="d2afa-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="d2afa-115">Die-Methode gibt einen booleschen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="d2afa-115">The method returns a Boolean value.</span></span> <span data-ttu-id="d2afa-116">Wenn Sie false zurückgeben, entspricht die der Einschränkung zugeordnete Route nicht der Browser Anforderung.</span><span class="sxs-lookup"><span data-stu-id="d2afa-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="d2afa-117">Die localhost-Einschränkung ist in der Liste 1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="d2afa-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="d2afa-118">**Codebeispiel 1-LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="d2afa-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="d2afa-119">Die Einschränkung in der Auflistung 1 nutzt die IsLocal-Eigenschaft, die von der HttpRequest-Klasse verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="d2afa-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="d2afa-120">Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung 127.0.0.1 ist oder wenn die IP-Adresse der Anforderung mit der IP-Adresse des Servers identisch ist.</span><span class="sxs-lookup"><span data-stu-id="d2afa-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="d2afa-121">Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global. asax" definiert ist.</span><span class="sxs-lookup"><span data-stu-id="d2afa-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="d2afa-122">Die Datei Global. asax in der Liste 2 verwendet die localhost-Einschränkung, um zu verhindern, dass eine Administrator Seite von einer Administrator Seite angefordert wird, es sei denn, Sie stellen die Anforderung vom lokalen Server</span><span class="sxs-lookup"><span data-stu-id="d2afa-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="d2afa-123">Eine Anforderung für/admin/DeleteAll schlägt beispielsweise fehl, wenn Sie von einem Remote Server aus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d2afa-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="d2afa-124">**Codebeispiel 2: Global. asax**</span><span class="sxs-lookup"><span data-stu-id="d2afa-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="d2afa-125">Die localhost-Einschränkung wird in der Definition der admin-Route verwendet.</span><span class="sxs-lookup"><span data-stu-id="d2afa-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="d2afa-126">Diese Route wird nicht mit einer Remote Browser Anforderung abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="d2afa-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="d2afa-127">Beachten Sie jedoch, dass andere in "Global. asax" definierte Routen mit derselben Anforderung übereinstimmen könnten.</span><span class="sxs-lookup"><span data-stu-id="d2afa-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="d2afa-128">Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route eine Anforderung abstimmt und nicht alle Routen, die in der Datei "Global. asax" definiert sind.</span><span class="sxs-lookup"><span data-stu-id="d2afa-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="d2afa-129">Beachten Sie, dass die Standardroute aus der Datei Global. asax in der Liste 2 auskommentiert wurde.</span><span class="sxs-lookup"><span data-stu-id="d2afa-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="d2afa-130">Wenn Sie die Standardroute einschließen, entspricht die Standardroute den Anforderungen des Admin-Controllers.</span><span class="sxs-lookup"><span data-stu-id="d2afa-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="d2afa-131">In diesem Fall können Remote Benutzer weiterhin Aktionen des Admin-Controllers aufrufen, auch wenn Ihre Anforderungen nicht der admin-Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="d2afa-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d2afa-132">[Zurück](creating-a-route-constraint-cs.md)
> [Weiter](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d2afa-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
