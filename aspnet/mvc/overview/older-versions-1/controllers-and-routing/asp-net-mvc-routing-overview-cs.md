---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: ASP.NET MVC-Routing ÜbersichtC#() | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial zeigt Stephen Walther, wie das ASP.NET MVC-Framework Browser Anforderungen Controller Aktionen zuordnet.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437703"
---
# <a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="c03ae-103">ASP.NET MVC-Routing – Übersicht (C#)</span><span class="sxs-lookup"><span data-stu-id="c03ae-103">ASP.NET MVC Routing Overview (C#)</span></span>

<span data-ttu-id="c03ae-104">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c03ae-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="c03ae-105">In diesem Tutorial zeigt Stephen Walther, wie das ASP.NET MVC-Framework Browser Anforderungen Controller Aktionen zuordnet.</span><span class="sxs-lookup"><span data-stu-id="c03ae-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>

<span data-ttu-id="c03ae-106">In diesem Tutorial haben Sie eine wichtige Funktion jeder ASP.NET MVC-Anwendung mit dem Namen " *ASP.NET Routing*" eingeführt.</span><span class="sxs-lookup"><span data-stu-id="c03ae-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="c03ae-107">Das ASP.NET-Routing Modul ist für die Zuordnung eingehender Browser Anforderungen zu bestimmten MVC-Controller Aktionen verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="c03ae-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="c03ae-108">Am Ende dieses Tutorials erfahren Sie, wie die Standardrouten Tabelle Anforderungen Controller Aktionen zuordnet.</span><span class="sxs-lookup"><span data-stu-id="c03ae-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="c03ae-109">Verwenden der Standardrouten Tabelle</span><span class="sxs-lookup"><span data-stu-id="c03ae-109">Using the Default Route Table</span></span>

<span data-ttu-id="c03ae-110">Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, ist die Anwendung bereits für die Verwendung des ASP.NET-Routings konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="c03ae-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="c03ae-111">ASP.NET Routing ist an zwei Stellen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="c03ae-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="c03ae-112">Zuerst ist ASP.NET Routing in der Webkonfigurationsdatei der Anwendung (Web. config-Datei) aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c03ae-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="c03ae-113">Die Konfigurationsdatei enthält vier Abschnitte, die für das Routing relevant sind: den Abschnitt "System. Web. HttpModules", den Abschnitt "System. Web. httpHandlers", den Abschnitt "System. Webserver. modules" und den Abschnitt "System. Webserver. Handlers".</span><span class="sxs-lookup"><span data-stu-id="c03ae-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="c03ae-114">Achten Sie darauf, diese Abschnitte nicht zu löschen, da das Routing ohne diese Abschnitte nicht mehr funktioniert.</span><span class="sxs-lookup"><span data-stu-id="c03ae-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="c03ae-115">Zweitens und noch wichtiger ist, dass eine Routing Tabelle in der Datei Global. asax der Anwendung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c03ae-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="c03ae-116">Die Datei "Global. asax" ist eine spezielle Datei, die Ereignishandler für ASP.net-Anwendungslebenszyklus-Ereignisse enthält.</span><span class="sxs-lookup"><span data-stu-id="c03ae-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="c03ae-117">Die Routing Tabelle wird während des Anwendungs Start Ereignisses erstellt.</span><span class="sxs-lookup"><span data-stu-id="c03ae-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="c03ae-118">Die Datei in "Listing 1" enthält die Standarddatei "Global. asax" für eine ASP.NET MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c03ae-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="c03ae-119">**Codebeispiel 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="c03ae-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="c03ae-120">Wenn eine MVC-Anwendung zum ersten Mal gestartet wird, wird die Anwendung\_Start ()-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="c03ae-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="c03ae-121">Diese Methode ruft wiederum die RegisterRoutes ()-Methode auf.</span><span class="sxs-lookup"><span data-stu-id="c03ae-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="c03ae-122">Die Methode "RegisterRoutes ()" erstellt die Routing Tabelle.</span><span class="sxs-lookup"><span data-stu-id="c03ae-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="c03ae-123">Die Standardrouten Tabelle enthält eine einzelne Route (mit dem Namen Standard).</span><span class="sxs-lookup"><span data-stu-id="c03ae-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="c03ae-124">Die Standardroute ordnet das erste Segment einer URL einem Controller Namen, das zweite Segment einer URL zu einer Controller Aktion und das dritte Segment einem Parameter mit dem Namen " **ID**" zu.</span><span class="sxs-lookup"><span data-stu-id="c03ae-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="c03ae-125">Stellen Sie sich vor, dass Sie die folgende URL in die Adressleiste Ihres Webbrowsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="c03ae-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="c03ae-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="c03ae-126">/Home/Index/3</span></span>

<span data-ttu-id="c03ae-127">Die Standardroute ordnet diese URL den folgenden Parametern zu:</span><span class="sxs-lookup"><span data-stu-id="c03ae-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="c03ae-128">Controller = Startseite</span><span class="sxs-lookup"><span data-stu-id="c03ae-128">controller = Home</span></span>

- <span data-ttu-id="c03ae-129">Action = Index</span><span class="sxs-lookup"><span data-stu-id="c03ae-129">action = Index</span></span>

- <span data-ttu-id="c03ae-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="c03ae-130">id = 3</span></span>

<span data-ttu-id="c03ae-131">Wenn Sie die URL/Home/Index/3 anfordern, wird der folgende Code ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="c03ae-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="c03ae-132">HomeController. Index (3)</span><span class="sxs-lookup"><span data-stu-id="c03ae-132">HomeController.Index(3)</span></span>

<span data-ttu-id="c03ae-133">Die Standardroute enthält Standardwerte für alle drei Parameter.</span><span class="sxs-lookup"><span data-stu-id="c03ae-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="c03ae-134">Wenn Sie keinen Controller angeben, wird der Controller Parameter standardmäßig auf den Wert **Home**eingestellt.</span><span class="sxs-lookup"><span data-stu-id="c03ae-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="c03ae-135">Wenn Sie keine Aktion angeben, wird der Aktionsparameter standardmäßig auf den Wert **Index**eingestellt.</span><span class="sxs-lookup"><span data-stu-id="c03ae-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="c03ae-136">Wenn Sie keine ID angeben, wird der ID-Parameter standardmäßig auf eine leere Zeichenfolge zurückgelegt.</span><span class="sxs-lookup"><span data-stu-id="c03ae-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="c03ae-137">Betrachten wir einige Beispiele dafür, wie die Standardroute URLs zu Controller Aktionen zuordnet.</span><span class="sxs-lookup"><span data-stu-id="c03ae-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="c03ae-138">Stellen Sie sich vor, dass Sie die folgende URL in die Adressleiste Ihres Browsers eingeben:</span><span class="sxs-lookup"><span data-stu-id="c03ae-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="c03ae-139">/Home</span><span class="sxs-lookup"><span data-stu-id="c03ae-139">/Home</span></span>

<span data-ttu-id="c03ae-140">Aufgrund der standardmäßigen Routen Parameter-Standardwerte führt die Eingabe dieser URL dazu, dass die Index ()-Methode der HomeController-Klasse in der Liste 2 aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c03ae-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="c03ae-141">**Codebeispiel 2: HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="c03ae-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="c03ae-142">In der Liste 2 enthält die HomeController-Klasse eine Methode mit dem Namen Index (), die einen einzelnen Parameter mit dem Namen ID akzeptiert. Die URL/Home bewirkt, dass die Index ()-Methode mit einer leeren Zeichenfolge als Wert des ID-Parameters aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c03ae-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="c03ae-143">Aufgrund der Art und Weise, wie das MVC-Framework Controller Aktionen aufruft, stimmt die URL/Home auch mit der Index ()-Methode der HomeController-Klasse in der Liste 3 überein.</span><span class="sxs-lookup"><span data-stu-id="c03ae-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="c03ae-144">**Codebeispiel 3-HomeController.cs (Index Aktion ohne Parameter)**</span><span class="sxs-lookup"><span data-stu-id="c03ae-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="c03ae-145">Die Index ()-Methode in der Liste 3 akzeptiert keine Parameter.</span><span class="sxs-lookup"><span data-stu-id="c03ae-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="c03ae-146">Die URL/Home bewirkt, dass diese Index ()-Methode aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c03ae-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="c03ae-147">Die URL/Home/Index/3 ruft auch diese Methode auf (die ID wird ignoriert).</span><span class="sxs-lookup"><span data-stu-id="c03ae-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="c03ae-148">Die URL/Home entspricht auch der Index ()-Methode der HomeController-Klasse in der Liste 4.</span><span class="sxs-lookup"><span data-stu-id="c03ae-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="c03ae-149">**Codebeispiel 4-HomeController.cs (Index Aktion mit Parametern, die NULL-Werte zulassen)**</span><span class="sxs-lookup"><span data-stu-id="c03ae-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="c03ae-150">In der Liste 4 weist die Index ()-Methode einen ganzzahligen Parameter auf.</span><span class="sxs-lookup"><span data-stu-id="c03ae-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="c03ae-151">Da der Parameter ein Parameter ist, der NULL-Werte zulässt (der Wert kann NULL sein), kann der Index () aufgerufen werden, ohne dass ein Fehler ausgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="c03ae-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="c03ae-152">Zum Schluss verursacht der Aufruf der Index ()-Methode in der Liste 5 mit der URL/Home eine Ausnahme, da der ID-Parameter *kein Parameter ist* , der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="c03ae-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="c03ae-153">Wenn Sie versuchen, die Index ()-Methode aufzurufen, erhalten Sie den in Abbildung 1 angezeigten Fehler.</span><span class="sxs-lookup"><span data-stu-id="c03ae-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="c03ae-154">**Listing 5-HomeController.cs (Index Aktion mit ID-Parameter)**</span><span class="sxs-lookup"><span data-stu-id="c03ae-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

<span data-ttu-id="c03ae-155">[![Aufrufen einer Controller Aktion, die einen Parameterwert erwartet](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c03ae-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="c03ae-156">**Abbildung 01**: Aufrufen einer Controller Aktion, die einen Parameterwert erwartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c03ae-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="c03ae-157">Die URL/Home/Index/3 hingegen funktioniert problemlos mit der Index Controller Aktion in der Liste 5.</span><span class="sxs-lookup"><span data-stu-id="c03ae-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="c03ae-158">Die Anforderung/Home/Index/3 bewirkt, dass die Index ()-Methode mit einem ID-Parameter mit dem Wert 3 aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="c03ae-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="c03ae-159">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c03ae-159">Summary</span></span>

<span data-ttu-id="c03ae-160">Ziel dieses Tutorials war es, Ihnen eine kurze Einführung in das ASP.NET-Routing zu bieten.</span><span class="sxs-lookup"><span data-stu-id="c03ae-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="c03ae-161">Wir haben die Standardrouten Tabelle untersucht, die Sie mit einer neuen ASP.NET MVC-Anwendung erhalten.</span><span class="sxs-lookup"><span data-stu-id="c03ae-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="c03ae-162">Sie haben gelernt, wie die Standardroute URLs Controller Aktionen zuordnet.</span><span class="sxs-lookup"><span data-stu-id="c03ae-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c03ae-163">Weiter</span><span class="sxs-lookup"><span data-stu-id="c03ae-163">Next</span></span>](understanding-action-filters-cs.md)
