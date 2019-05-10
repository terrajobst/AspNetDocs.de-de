---
uid: web-api/overview/advanced/http-message-handlers
title: HTTP-Meldungshandler in der ASP.NET Web-API – ASP.NET 4.x
author: MikeWasson
description: Einen Überblick über die HTTP-Meldungshandler in der ASP.NET Web-API für ASP.NET 4.x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115550"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="fff66-103">HTTP-Meldungshandler in der ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="fff66-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="fff66-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fff66-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fff66-105">Ein *Meldungshandler* ist eine Klasse, die eine HTTP-Anforderung empfängt, und gibt eine HTTP-Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="fff66-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="fff66-106">Meldungshandler abgeleitet werden, von der abstrakten **HttpMessageHandler** Klasse.</span><span class="sxs-lookup"><span data-stu-id="fff66-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="fff66-107">In der Regel eine Reihe von Meldungshandler verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="fff66-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="fff66-108">Der erste Handler eine HTTP-Anforderung empfängt, erfolgt die Verarbeitung und gibt die Anforderung an dem nächsten Handler.</span><span class="sxs-lookup"><span data-stu-id="fff66-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="fff66-109">An einem bestimmten Punkt wird die Antwort wird erstellt und wird wieder die Kette.</span><span class="sxs-lookup"><span data-stu-id="fff66-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="fff66-110">Dieses Muster wird aufgerufen, eine *Delegieren* Handler.</span><span class="sxs-lookup"><span data-stu-id="fff66-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="fff66-111">Serverseitige-Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="fff66-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="fff66-112">Auf der Serverseite verwendet die Web-API-Pipeline einige integrierte-Meldungshandler:</span><span class="sxs-lookup"><span data-stu-id="fff66-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="fff66-113">**HttpServer** vom Host der Anforderung ab.</span><span class="sxs-lookup"><span data-stu-id="fff66-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="fff66-114">**HttpRoutingDispatcher** sendet die Anforderung auf Grundlage der Route.</span><span class="sxs-lookup"><span data-stu-id="fff66-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="fff66-115">**HttpControllerDispatcher** sendet die Anforderung an einen Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="fff66-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="fff66-116">Sie können benutzerdefinierte Handler zur Pipeline hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fff66-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="fff66-117">Nachrichtenhandler sind gut für die übergreifende Probleme, die auf der Ebene der HTTP-Nachrichten (statt über Controlleraktionen) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="fff66-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="fff66-118">Beispielsweise können ein Nachrichtenhandler:</span><span class="sxs-lookup"><span data-stu-id="fff66-118">For example, a message handler might:</span></span>

- <span data-ttu-id="fff66-119">Lesen oder Ändern von Anforderungsheadern.</span><span class="sxs-lookup"><span data-stu-id="fff66-119">Read or modify request headers.</span></span>
- <span data-ttu-id="fff66-120">Hinzufügen eines Antwortheaders zu antworten.</span><span class="sxs-lookup"><span data-stu-id="fff66-120">Add a response header to responses.</span></span>
- <span data-ttu-id="fff66-121">Überprüfen Sie Anforderungen, bevor sie den Controller erreichen.</span><span class="sxs-lookup"><span data-stu-id="fff66-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="fff66-122">Dieses Diagramm zeigt zwei benutzerdefinierte Handler, die in der Pipeline eingefügt:</span><span class="sxs-lookup"><span data-stu-id="fff66-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="fff66-123">Klicken Sie auf der Clientseite verwendet "HttpClient" auch Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="fff66-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="fff66-124">Weitere Informationen finden Sie unter [HttpClient-Meldungshandler](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="fff66-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="fff66-125">Benutzerdefinierte Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="fff66-125">Custom Message Handlers</span></span>

<span data-ttu-id="fff66-126">Zum Schreiben von eines benutzerdefinierten Nachrichtenhandler abgeleitet **System.Net.Http.DelegatingHandler** und überschreiben die **SendAsync** Methode.</span><span class="sxs-lookup"><span data-stu-id="fff66-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="fff66-127">Diese Methode hat die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="fff66-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="fff66-128">Die Methode akzeptiert eine **HttpRequestMessage** als Eingabe und asynchron gibt ein **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="fff66-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="fff66-129">Eine typische Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="fff66-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="fff66-130">Verarbeitet die Anforderungsnachricht an.</span><span class="sxs-lookup"><span data-stu-id="fff66-130">Process the request message.</span></span>
2. <span data-ttu-id="fff66-131">Rufen Sie `base.SendAsync` zum Senden der Anforderung an den inneren Handler.</span><span class="sxs-lookup"><span data-stu-id="fff66-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="fff66-132">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="fff66-132">The inner handler returns a response message.</span></span> <span data-ttu-id="fff66-133">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="fff66-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="fff66-134">Die Antwort zu verarbeiten und an den Aufrufer zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="fff66-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="fff66-135">Hier ist ein sehr einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fff66-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="fff66-136">Der Aufruf von `base.SendAsync` ist asynchron.</span><span class="sxs-lookup"><span data-stu-id="fff66-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="fff66-137">Wenn der Handler für alle Vorgänge nach dem Aufruf ausführt, verwenden Sie die **"await"** -Schlüsselwort, wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="fff66-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="fff66-138">Ein delegierender Handler kann den inneren Handler auch überspringen und direkt die Antwort zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="fff66-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="fff66-139">Wenn eine Delegierung Ereignishandler erstellt die Antwort ohne `base.SendAsync`, die Anforderung wird übersprungen, den Rest der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="fff66-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="fff66-140">Dies kann nach einem Handler nützlich sein, der die Anforderung (erstellen eine Fehlerantwort) überprüft.</span><span class="sxs-lookup"><span data-stu-id="fff66-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="fff66-141">Hinzufügen eines Handlers für die Pipeline</span><span class="sxs-lookup"><span data-stu-id="fff66-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="fff66-142">Um einen Meldungshandler auf dem Server hinzuzufügen, fügen Sie den Handler, der die **HttpConfiguration.MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="fff66-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="fff66-143">Wenn Sie die Vorlage "ASP.NET MVC 4-Webanwendung" verwendet, um das Projekt zu erstellen, erreichen Sie dies in der **WebApiConfig** Klasse:</span><span class="sxs-lookup"><span data-stu-id="fff66-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="fff66-144">Message-Ereignishandler werden aufgerufen, in der gleichen Reihenfolge, die sie in angezeigt werden **MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="fff66-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="fff66-145">Da sie geschachtelt sind, wird die Response-Nachricht in die andere Richtung übertragen.</span><span class="sxs-lookup"><span data-stu-id="fff66-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="fff66-146">Der letzte Handler ist, also den ersten, die Response-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="fff66-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="fff66-147">Beachten Sie, dass Sie nicht der internen Handler festlegen müssen. die Web-API-Framework wird automatisch der Meldungshandler hergestellt.</span><span class="sxs-lookup"><span data-stu-id="fff66-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="fff66-148">Möchten [Selbsthosting](../older-versions/self-host-a-web-api.md), erstellen Sie eine Instanz von der **HttpSelfHostConfiguration** -Klasse und die Handler zum Hinzufügen der **MessageHandlers** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="fff66-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="fff66-149">Jetzt sehen wir uns einige Beispiele für benutzerdefinierte Meldungshandler.</span><span class="sxs-lookup"><span data-stu-id="fff66-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="fff66-150">Beispiel: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="fff66-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="fff66-151">X-HTTP-Method-Override ist einem nicht standardmäßigen HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="fff66-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="fff66-152">Es dient für Clients, die bestimmte HTTP-Anforderungstypen, z. B. PUT oder DELETE senden können.</span><span class="sxs-lookup"><span data-stu-id="fff66-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="fff66-153">Stattdessen wird der Client sendet eine POST-Anforderung, und legt den X-HTTP-Method-Override-Header auf die gewünschte Methode.</span><span class="sxs-lookup"><span data-stu-id="fff66-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="fff66-154">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fff66-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="fff66-155">Hier ist ein Meldungshandler, das Unterstützung für X-HTTP-Method-Override aus:</span><span class="sxs-lookup"><span data-stu-id="fff66-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="fff66-156">In der **SendAsync** -Methode, der Handler überprüft wird, gibt an, ob die Anforderungsnachricht für eine POST-Anforderung ist, und ob es sich um den X-HTTP-Method-Override-Header enthält.</span><span class="sxs-lookup"><span data-stu-id="fff66-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="fff66-157">Wenn dies der Fall ist, überprüft den Wert von Header und ändert anschließend die Anforderungsmethode.</span><span class="sxs-lookup"><span data-stu-id="fff66-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="fff66-158">Der Handler zum Schluss ruft `base.SendAsync` die Nachricht an den nächsten Handler übergeben.</span><span class="sxs-lookup"><span data-stu-id="fff66-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="fff66-159">Wenn die Anforderung erreicht den **HttpControllerDispatcher** -Klasse, **HttpControllerDispatcher** leitet die Anforderung, die basierend auf dem die aktualisierte Anforderungsmethode.</span><span class="sxs-lookup"><span data-stu-id="fff66-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="fff66-160">Beispiel: Hinzufügen eines benutzerdefinierten Antwortheaders</span><span class="sxs-lookup"><span data-stu-id="fff66-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="fff66-161">Hier ist ein Message-Handler, der einen benutzerdefinierten Header zu jeder Response-Nachricht hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="fff66-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="fff66-162">Zunächst ruft der Handler `base.SendAsync` die Anforderung an den inneren Meldungshandler übergeben.</span><span class="sxs-lookup"><span data-stu-id="fff66-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="fff66-163">Der innere Handler gibt eine Antwortnachricht zurück, sondern nur asynchron mit einem **Aufgabe&lt;T&gt;**  Objekt.</span><span class="sxs-lookup"><span data-stu-id="fff66-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="fff66-164">Die Response-Nachricht ist nicht verfügbar, bis `base.SendAsync` asynchron abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="fff66-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="fff66-165">Dieses Beispiel verwendet die **"await"** Schlüsselwort, um die Aufgaben asynchron nach `SendAsync` abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="fff66-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="fff66-166">Wenn Sie .NET Framework 4.0 ausgerichtet sind, verwenden Sie die **Aufgabe**&lt;T&gt;**. ContinueWith** Methode:</span><span class="sxs-lookup"><span data-stu-id="fff66-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="fff66-167">Beispiel: Überprüfen für einen API-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="fff66-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="fff66-168">Einige Webdienste werden Clients für einen API-Schlüssel in der Anforderung angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="fff66-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="fff66-169">Das folgende Beispiel zeigt, wie ein Meldungshandler Anforderungen für einen gültigen API-Schlüssel überprüfen kann:</span><span class="sxs-lookup"><span data-stu-id="fff66-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="fff66-170">Dieser Handler sucht nach den API-Schlüssel in der URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="fff66-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="fff66-171">(In diesem Beispiel wird davon ausgegangen, dass der Schlüssel eine statische Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="fff66-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="fff66-172">"Eine realen Implementierung würden wahrscheinlich eine komplexere Validierung verwenden.) Wenn die Abfragezeichenfolge der Schlüssel enthält, übergibt der Handler für die Anforderung an den inneren Handler.</span><span class="sxs-lookup"><span data-stu-id="fff66-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="fff66-173">Wenn die Anforderung nicht über einen gültigen Schlüssel verfügt, erstellt der Handler für eine Antwortnachricht mit dem Status 403, verboten.</span><span class="sxs-lookup"><span data-stu-id="fff66-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="fff66-174">In diesem Fall ruft der Handler für nicht auf `base.SendAsync`, also der innere Handler empfängt die Anforderung, noch wird des Controllers.</span><span class="sxs-lookup"><span data-stu-id="fff66-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="fff66-175">Der Controller kann daher davon ausgehen, dass alle eingehende Anforderungen über einen gültigen API-Schlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="fff66-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="fff66-176">Wenn der API-Schlüssel nur für bestimmte Controlleraktionen gilt, erwägen Sie, die einen Aktionsfilter anstelle eines meldungshandlers.</span><span class="sxs-lookup"><span data-stu-id="fff66-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="fff66-177">Aktionsfilter werden nach dem URI routing ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fff66-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="fff66-178">Pro-Route-Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="fff66-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="fff66-179">-Handler in der **HttpConfiguration.MessageHandlers** Auflistung global angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="fff66-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="fff66-180">Alternativ können Sie einen Meldungshandler für eine bestimmte Route hinzufügen, wenn Sie die Route definieren:</span><span class="sxs-lookup"><span data-stu-id="fff66-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="fff66-181">In diesem Beispiel, wenn der Anforderungs-URI "Route2", entspricht die Anforderung wird gesendet `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="fff66-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="fff66-182">Das folgende Diagramm zeigt die Pipeline für diese zwei Routen:</span><span class="sxs-lookup"><span data-stu-id="fff66-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="fff66-183">Beachten Sie, dass `MessageHandler2` ersetzt das standardmäßige **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="fff66-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="fff66-184">In diesem Beispiel `MessageHandler2` erstellt die Antwort und -Anforderungen, die mit "Route2" Gehen Sie niemals auf einen Controller übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="fff66-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="fff66-185">Dadurch können Sie den gesamten Web-API-Controller-Mechanismus durch Ihren eigenen benutzerdefinierten Endpunkt zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="fff66-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="fff66-186">Alternativ kann ein Message-Handler pro Route an Delegieren **HttpControllerDispatcher**, die dann an einen Controller sendet.</span><span class="sxs-lookup"><span data-stu-id="fff66-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="fff66-187">Der folgende Code zeigt, wie Sie diese Route zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="fff66-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
