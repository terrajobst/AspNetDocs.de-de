---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Erstellen einer benutzerdefinierten Routen Einschränkung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486789"
---
# <a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="92939-104">Erstellen einer benutzerdefinierten Routeneinschränkung (VB)</span><span class="sxs-lookup"><span data-stu-id="92939-104">Creating a Custom Route Constraint (VB)</span></span>

<span data-ttu-id="92939-105">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="92939-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="92939-106">Stephen Walther zeigt, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="92939-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="92939-107">Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird, wenn eine Browser Anforderung von einem Remote Computer aus erfolgt.</span><span class="sxs-lookup"><span data-stu-id="92939-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="92939-108">In diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierte Routen Einschränkung erstellen können.</span><span class="sxs-lookup"><span data-stu-id="92939-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="92939-109">Mit einer benutzerdefinierten Routen Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung stimmt überein.</span><span class="sxs-lookup"><span data-stu-id="92939-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="92939-110">In diesem Tutorial erstellen wir eine localhost-Routen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="92939-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="92939-111">Die localhost-Routen Einschränkung entspricht nur Anforderungen, die vom lokalen Computer ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="92939-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="92939-112">Remote Anforderungen aus dem Internet stimmen nicht überein.</span><span class="sxs-lookup"><span data-stu-id="92939-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="92939-113">Sie implementieren eine benutzerdefinierte Routen Einschränkung, indem Sie die irouteeinschränkung-Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="92939-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="92939-114">Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschreibt:</span><span class="sxs-lookup"><span data-stu-id="92939-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="92939-115">Die-Methode gibt einen booleschen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="92939-115">The method returns a Boolean value.</span></span> <span data-ttu-id="92939-116">Wenn Sie false zurückgeben, entspricht die der Einschränkung zugeordnete Route nicht der Browser Anforderung.</span><span class="sxs-lookup"><span data-stu-id="92939-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="92939-117">Die localhost-Einschränkung ist in der Liste 1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="92939-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="92939-118">**Codebeispiel 1: localhosteinschränkung. vb**</span><span class="sxs-lookup"><span data-stu-id="92939-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="92939-119">Die Einschränkung in der Auflistung 1 nutzt die IsLocal-Eigenschaft, die von der HttpRequest-Klasse verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="92939-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="92939-120">Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung 127.0.0.1 ist oder wenn die IP-Adresse der Anforderung mit der IP-Adresse des Servers identisch ist.</span><span class="sxs-lookup"><span data-stu-id="92939-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="92939-121">Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global. asax" definiert ist.</span><span class="sxs-lookup"><span data-stu-id="92939-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="92939-122">Die Datei Global. asax in der Liste 2 verwendet die localhost-Einschränkung, um zu verhindern, dass eine Administrator Seite von einer Administrator Seite angefordert wird, es sei denn, Sie stellen die Anforderung vom lokalen Server</span><span class="sxs-lookup"><span data-stu-id="92939-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="92939-123">Eine Anforderung für/admin/DeleteAll schlägt beispielsweise fehl, wenn Sie von einem Remote Server aus ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="92939-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="92939-124">**Codebeispiel 2: Global. asax**</span><span class="sxs-lookup"><span data-stu-id="92939-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="92939-125">Die localhost-Einschränkung wird in der Definition der admin-Route verwendet.</span><span class="sxs-lookup"><span data-stu-id="92939-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="92939-126">Diese Route wird nicht mit einer Remote Browser Anforderung abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="92939-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="92939-127">Beachten Sie jedoch, dass andere in "Global. asax" definierte Routen mit derselben Anforderung übereinstimmen könnten.</span><span class="sxs-lookup"><span data-stu-id="92939-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="92939-128">Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route eine Anforderung abstimmt und nicht alle Routen, die in der Datei "Global. asax" definiert sind.</span><span class="sxs-lookup"><span data-stu-id="92939-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="92939-129">Beachten Sie, dass die Standardroute aus der Datei Global. asax in der Liste 2 auskommentiert wurde.</span><span class="sxs-lookup"><span data-stu-id="92939-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="92939-130">Wenn Sie die Standardroute einschließen, entspricht die Standardroute den Anforderungen des Admin-Controllers.</span><span class="sxs-lookup"><span data-stu-id="92939-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="92939-131">In diesem Fall können Remote Benutzer weiterhin Aktionen des Admin-Controllers aufrufen, auch wenn Ihre Anforderungen nicht der admin-Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="92939-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="92939-132">Previous</span><span class="sxs-lookup"><span data-stu-id="92939-132">Previous</span></span>](creating-a-route-constraint-vb.md)
