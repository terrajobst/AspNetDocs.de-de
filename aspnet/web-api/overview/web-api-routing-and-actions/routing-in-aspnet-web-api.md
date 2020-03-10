---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449247"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="cf316-102">Routing in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="cf316-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="cf316-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cf316-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cf316-104">In diesem Artikel wird beschrieben, wie ASP.net-Web-API HTTP-Anforderungen an Controller weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="cf316-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="cf316-105">Wenn Sie mit ASP.NET MVC vertraut sind, ähnelt das Web-API-Routing sehr dem MVC-Routing.</span><span class="sxs-lookup"><span data-stu-id="cf316-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="cf316-106">Der Hauptunterschied besteht darin, dass die Web-API das HTTP-Verb und nicht den URI-Pfad verwendet, um die Aktion auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="cf316-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="cf316-107">Sie können auch das MVC-Routing in der Web-API verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf316-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="cf316-108">In diesem Artikel wird davon ausgegangen, dass keine Kenntnisse über ASP.NET MVC vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cf316-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="cf316-109">Routing Tabellen</span><span class="sxs-lookup"><span data-stu-id="cf316-109">Routing Tables</span></span>

<span data-ttu-id="cf316-110">In ASP.net-Web-API ist ein *Controller* eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="cf316-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="cf316-111">Die öffentlichen Methoden des Controllers werden als *Aktionsmethoden* oder einfach als *Aktionen*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cf316-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="cf316-112">Wenn das Web-API-Framework eine Anforderung empfängt, leitet es die Anforderung an eine Aktion weiter.</span><span class="sxs-lookup"><span data-stu-id="cf316-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="cf316-113">Um zu ermitteln, welche Aktion aufgerufen werden soll, verwendet das Framework eine *Routing Tabelle*.</span><span class="sxs-lookup"><span data-stu-id="cf316-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="cf316-114">Die Visual Studio-Projektvorlage für die Web-API erstellt eine Standardroute:</span><span class="sxs-lookup"><span data-stu-id="cf316-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="cf316-115">Diese Route wird in der Datei *WebApiConfig.cs* definiert, die in der *App\_Start* -Verzeichnisses abgelegt wird:</span><span class="sxs-lookup"><span data-stu-id="cf316-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="cf316-116">Weitere Informationen zum `WebApiConfig`-Klasse finden Sie unter [Konfigurieren von ASP.net-Web-API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="cf316-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="cf316-117">Wenn Sie die Web-API selbst hosten, müssen Sie die Routing Tabelle direkt auf das `HttpSelfHostConfiguration`-Objekt festlegen.</span><span class="sxs-lookup"><span data-stu-id="cf316-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="cf316-118">Weitere Informationen finden Sie unter [Self-Hosting einer Web-API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="cf316-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="cf316-119">Jeder Eintrag in der Routing Tabelle enthält eine *Routen Vorlage*.</span><span class="sxs-lookup"><span data-stu-id="cf316-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="cf316-120">Die Standardrouten Vorlage für die Web-API ist &quot;API/{Controller}/{ID}&quot;.</span><span class="sxs-lookup"><span data-stu-id="cf316-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="cf316-121">In dieser Vorlage ist &quot;API-&quot; ein literales Pfad Segment, und {Controller} und {ID} sind Platzhalter Variablen.</span><span class="sxs-lookup"><span data-stu-id="cf316-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="cf316-122">Wenn das Web-API-Framework eine HTTP-Anforderung empfängt, versucht es, den URI mit einer der Routen Vorlagen in der Routing Tabelle abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="cf316-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="cf316-123">Wenn keine Route übereinstimmt, empfängt der Client einen 404-Fehler.</span><span class="sxs-lookup"><span data-stu-id="cf316-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="cf316-124">Die folgenden URIs entsprechen z. b. der Standardroute:</span><span class="sxs-lookup"><span data-stu-id="cf316-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="cf316-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="cf316-125">/api/contacts</span></span>
- <span data-ttu-id="cf316-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="cf316-126">/api/contacts/1</span></span>
- <span data-ttu-id="cf316-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="cf316-127">/api/products/gizmo1</span></span>

<span data-ttu-id="cf316-128">Der folgende URI stimmt jedoch nicht, da der &quot;-API-&quot; Segment fehlt:</span><span class="sxs-lookup"><span data-stu-id="cf316-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="cf316-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="cf316-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="cf316-130">Der Grund für die Verwendung von "API" in der Route besteht darin, Konflikte mit ASP.NET MVC-Routing zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="cf316-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="cf316-131">Auf diese Weise können Sie &quot;/Contacts-&quot; zu einem MVC-Controller wechseln und &quot;/API/Contacts-&quot; zu einem Web-API-Controller wechseln.</span><span class="sxs-lookup"><span data-stu-id="cf316-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="cf316-132">Wenn Ihnen diese Konvention nicht gefällt, können Sie natürlich auch die Standardrouten Tabelle ändern.</span><span class="sxs-lookup"><span data-stu-id="cf316-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="cf316-133">Wenn eine übereinstimmende Route gefunden wird, wählt die Web-API den Controller und die Aktion aus:</span><span class="sxs-lookup"><span data-stu-id="cf316-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="cf316-134">Um den Controller zu finden, fügt die Web-API &quot;Controller&quot; dem Wert der Variablen *{Controller}* hinzu.</span><span class="sxs-lookup"><span data-stu-id="cf316-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="cf316-135">Um die Aktion zu finden, untersucht die Web-API das HTTP-Verb und sucht dann nach einer Aktion, deren Name mit diesem http-Verb Namen beginnt.</span><span class="sxs-lookup"><span data-stu-id="cf316-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="cf316-136">Beispielsweise sucht die Web-API mit einer GET-Anforderung nach einer Aktion, der &quot;Get&quot;vorangestellt ist, z. b. &quot;GetContact&quot; oder &quot;getallcontacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="cf316-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="cf316-137">Diese Konvention gilt nur für Get-, Post-, Put-, DELETE-, Head-, Options-und Patch-Verben.</span><span class="sxs-lookup"><span data-stu-id="cf316-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="cf316-138">Sie können andere HTTP-Verben mithilfe von Attributen auf dem Controller aktivieren.</span><span class="sxs-lookup"><span data-stu-id="cf316-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="cf316-139">Ein Beispiel hierfür wird später angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cf316-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="cf316-140">Andere Platzhalter Variablen in der Routen Vorlage (z. b. *{ID}* ) werden Aktions Parametern zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="cf316-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="cf316-141">Schauen wir uns ein Beispiel an.</span><span class="sxs-lookup"><span data-stu-id="cf316-141">Let's look at an example.</span></span> <span data-ttu-id="cf316-142">Angenommen, Sie definieren den folgenden Controller:</span><span class="sxs-lookup"><span data-stu-id="cf316-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="cf316-143">Im folgenden finden Sie einige mögliche HTTP-Anforderungen zusammen mit der Aktion, die für jede aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="cf316-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="cf316-144">HTTP-Verb</span><span class="sxs-lookup"><span data-stu-id="cf316-144">HTTP Verb</span></span> | <span data-ttu-id="cf316-145">URI-Pfad</span><span class="sxs-lookup"><span data-stu-id="cf316-145">URI Path</span></span> | <span data-ttu-id="cf316-146">Aktion</span><span class="sxs-lookup"><span data-stu-id="cf316-146">Action</span></span> | <span data-ttu-id="cf316-147">Parameter</span><span class="sxs-lookup"><span data-stu-id="cf316-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="cf316-148">GET</span><span class="sxs-lookup"><span data-stu-id="cf316-148">GET</span></span> | <span data-ttu-id="cf316-149">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="cf316-149">api/products</span></span> | <span data-ttu-id="cf316-150">Getallproducts</span><span class="sxs-lookup"><span data-stu-id="cf316-150">GetAllProducts</span></span> | <span data-ttu-id="cf316-151">*gar*</span><span class="sxs-lookup"><span data-stu-id="cf316-151">*(none)*</span></span> |
| <span data-ttu-id="cf316-152">GET</span><span class="sxs-lookup"><span data-stu-id="cf316-152">GET</span></span> | <span data-ttu-id="cf316-153">API/Produkte/4</span><span class="sxs-lookup"><span data-stu-id="cf316-153">api/products/4</span></span> | <span data-ttu-id="cf316-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="cf316-154">GetProductById</span></span> | <span data-ttu-id="cf316-155">4</span><span class="sxs-lookup"><span data-stu-id="cf316-155">4</span></span> |
| <span data-ttu-id="cf316-156">Delete</span><span class="sxs-lookup"><span data-stu-id="cf316-156">DELETE</span></span> | <span data-ttu-id="cf316-157">API/Produkte/4</span><span class="sxs-lookup"><span data-stu-id="cf316-157">api/products/4</span></span> | <span data-ttu-id="cf316-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="cf316-158">DeleteProduct</span></span> | <span data-ttu-id="cf316-159">4</span><span class="sxs-lookup"><span data-stu-id="cf316-159">4</span></span> |
| <span data-ttu-id="cf316-160">POST</span><span class="sxs-lookup"><span data-stu-id="cf316-160">POST</span></span> | <span data-ttu-id="cf316-161">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="cf316-161">api/products</span></span> | <span data-ttu-id="cf316-162">*(keine Entsprechung)*</span><span class="sxs-lookup"><span data-stu-id="cf316-162">*(no match)*</span></span> |  |

<span data-ttu-id="cf316-163">Beachten Sie, dass das *{ID}* -Segment des URIs, sofern vorhanden, dem *ID* -Parameter der Aktion zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cf316-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="cf316-164">In diesem Beispiel definiert der Controller zwei Get-Methoden, eine mit einem *ID* -Parameter und eine ohne Parameter.</span><span class="sxs-lookup"><span data-stu-id="cf316-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="cf316-165">Beachten Sie außerdem, dass die Post-Anforderung fehlschlägt, da der Controller keine &quot;Post...&quot;-Methode definiert.</span><span class="sxs-lookup"><span data-stu-id="cf316-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="cf316-166">Routing Variationen</span><span class="sxs-lookup"><span data-stu-id="cf316-166">Routing Variations</span></span>

<span data-ttu-id="cf316-167">Im vorherigen Abschnitt wurde der grundlegende Routing Mechanismus für ASP.net-Web-API beschrieben.</span><span class="sxs-lookup"><span data-stu-id="cf316-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="cf316-168">In diesem Abschnitt werden einige Variationen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="cf316-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="cf316-169">HTTP-Verben</span><span class="sxs-lookup"><span data-stu-id="cf316-169">HTTP verbs</span></span>

<span data-ttu-id="cf316-170">Anstatt die Benennungs Konvention für HTTP-Verben zu verwenden, können Sie das HTTP-Verb für eine Aktion explizit angeben, indem Sie die Aktionsmethode mit einem der folgenden Attribute versehen:</span><span class="sxs-lookup"><span data-stu-id="cf316-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="cf316-171">Im folgenden Beispiel wird die `FindProduct`-Methode GET-Anforderungen zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="cf316-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="cf316-172">Verwenden Sie das `[AcceptVerbs]`-Attribut, das eine Liste von HTTP-Verben annimmt, um mehrere HTTP-Verben für eine Aktion zuzulassen oder um andere HTTP-Verben als Get, Put, Post, DELETE, Head, Options und Patch zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="cf316-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="cf316-173">Routing nach Aktions Name</span><span class="sxs-lookup"><span data-stu-id="cf316-173">Routing by Action Name</span></span>

<span data-ttu-id="cf316-174">Mit der Standard Routing Vorlage verwendet die Web-API das HTTP-Verb, um die Aktion auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="cf316-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="cf316-175">Sie können jedoch auch eine Route erstellen, bei der der Aktionsname im URI enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="cf316-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="cf316-176">In dieser Routen Vorlage benennt der Parameter " *{Action}* " die Aktionsmethode auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="cf316-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="cf316-177">Verwenden Sie bei diesem Routing Stil Attribute, um die zulässigen HTTP-Verben anzugeben.</span><span class="sxs-lookup"><span data-stu-id="cf316-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="cf316-178">Nehmen Sie beispielsweise an, dass Ihr Controller die folgende Methode hat:</span><span class="sxs-lookup"><span data-stu-id="cf316-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="cf316-179">In diesem Fall würde eine GET-Anforderung für "API/Products/Details/1" der `Details`-Methode zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="cf316-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="cf316-180">Diese Art des Routings ähnelt ASP.NET MVC und eignet sich möglicherweise für eine API im RPC-Stil.</span><span class="sxs-lookup"><span data-stu-id="cf316-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="cf316-181">Sie können den Aktions Namen überschreiben, indem Sie das `[ActionName]`-Attribut verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf316-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="cf316-182">Im folgenden Beispiel werden zwei Aktionen &quot;API/Products/Miniaturansicht/*ID*zugeordnet. Eine unterstützt Get und die andere unterstützt Post:</span><span class="sxs-lookup"><span data-stu-id="cf316-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="cf316-183">Nicht-Aktionen</span><span class="sxs-lookup"><span data-stu-id="cf316-183">Non-Actions</span></span>

<span data-ttu-id="cf316-184">Verwenden Sie das `[NonAction]`-Attribut, um zu verhindern, dass eine Methode als Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cf316-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="cf316-185">Dies signalisiert dem Framework, dass es sich bei der Methode nicht um eine Aktion handelt, auch wenn Sie andernfalls den Routing Regeln entspricht.</span><span class="sxs-lookup"><span data-stu-id="cf316-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="cf316-186">Weitere nützliche Informationen</span><span class="sxs-lookup"><span data-stu-id="cf316-186">Further Reading</span></span>

<span data-ttu-id="cf316-187">Dieses Thema bietet eine allgemeine Übersicht über das Routing.</span><span class="sxs-lookup"><span data-stu-id="cf316-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="cf316-188">Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](routing-and-action-selection.md), die genau beschreiben, wie das Framework mit einem URI einer Route übereinstimmt, einen Controller auswählt und dann die aufzurufende Aktion auswählt.</span><span class="sxs-lookup"><span data-stu-id="cf316-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
