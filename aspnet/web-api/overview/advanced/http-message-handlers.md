---
uid: web-api/overview/advanced/http-message-handlers
title: HTTP-Nachrichten Handler in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Übersicht über HTTP-Nachrichten Handler in ASP.net-Web-API für ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504927"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="12d46-103">HTTP-Nachrichten Handler in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="12d46-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="12d46-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="12d46-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="12d46-105">Ein *Nachrichten Handler* ist eine Klasse, die eine HTTP-Anforderung empfängt und eine HTTP-Antwort zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="12d46-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="12d46-106">Meldungs Handler werden von der abstrakten **httpmessagehandler** -Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="12d46-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="12d46-107">In der Regel werden eine Reihe von Meldungs Handlern verkettet.</span><span class="sxs-lookup"><span data-stu-id="12d46-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="12d46-108">Der erste Handler empfängt eine HTTP-Anforderung, verarbeitet einige Verarbeitungsschritte und übergibt die Anforderung an den nächsten Handler.</span><span class="sxs-lookup"><span data-stu-id="12d46-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="12d46-109">An einem bestimmten Punkt wird die Antwort erstellt, und die Kette wird wieder hergestellt.</span><span class="sxs-lookup"><span data-stu-id="12d46-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="12d46-110">Dieses Muster wird als *delegier ender* Handler bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="12d46-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="12d46-111">Server seitige Meldungs Handler</span><span class="sxs-lookup"><span data-stu-id="12d46-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="12d46-112">Auf der Serverseite verwendet die Web-API-Pipeline einige integrierte Nachrichten Handler:</span><span class="sxs-lookup"><span data-stu-id="12d46-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="12d46-113">**HTTPServer** Ruft die Anforderung vom Host ab.</span><span class="sxs-lookup"><span data-stu-id="12d46-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="12d46-114">**Httproutingdispatcher** sendet die Anforderung basierend auf der Route.</span><span class="sxs-lookup"><span data-stu-id="12d46-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="12d46-115">**Httpcontrollerdispatcher** sendet die Anforderung an einen Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="12d46-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="12d46-116">Sie können der Pipeline benutzerdefinierte Handler hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="12d46-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="12d46-117">Meldungs Handler sind für übergreifende Aspekte geeignet, die auf der Ebene von HTTP-Nachrichten (anstelle von Controller Aktionen) funktionieren.</span><span class="sxs-lookup"><span data-stu-id="12d46-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="12d46-118">Ein Meldungs Handler könnte beispielsweise Folgendes:</span><span class="sxs-lookup"><span data-stu-id="12d46-118">For example, a message handler might:</span></span>

- <span data-ttu-id="12d46-119">Lesen oder Ändern von Anforderungs Headern.</span><span class="sxs-lookup"><span data-stu-id="12d46-119">Read or modify request headers.</span></span>
- <span data-ttu-id="12d46-120">Fügen Sie Antworten einen Antwortheader hinzu.</span><span class="sxs-lookup"><span data-stu-id="12d46-120">Add a response header to responses.</span></span>
- <span data-ttu-id="12d46-121">Überprüfen Sie die Anforderungen, bevor Sie den Controller erreichen.</span><span class="sxs-lookup"><span data-stu-id="12d46-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="12d46-122">Dieses Diagramm zeigt zwei benutzerdefinierte Handler, die in die Pipeline eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="12d46-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="12d46-123">Auf der Clientseite verwendet httpclient auch Nachrichten Handler.</span><span class="sxs-lookup"><span data-stu-id="12d46-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="12d46-124">Weitere Informationen finden Sie unter [HttpClient](httpclient-message-handlers.md)-Meldungs Handler.</span><span class="sxs-lookup"><span data-stu-id="12d46-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="12d46-125">Benutzerdefinierte Meldungs Handler</span><span class="sxs-lookup"><span data-stu-id="12d46-125">Custom Message Handlers</span></span>

<span data-ttu-id="12d46-126">Zum Schreiben eines benutzerdefinierten Nachrichten Handlers leiten Sie von **System .net. http. delegatinghandler** ab und überschreiben die **SendAsync** -Methode.</span><span class="sxs-lookup"><span data-stu-id="12d46-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="12d46-127">Diese Methode hat die folgende Signatur:</span><span class="sxs-lookup"><span data-stu-id="12d46-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="12d46-128">Die Methode nimmt eine **httprequestmessage** als Eingabe an und gibt asynchron eine **httpresponsmessage**zurück.</span><span class="sxs-lookup"><span data-stu-id="12d46-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="12d46-129">Eine typische-Implementierung führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="12d46-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="12d46-130">Verarbeiten Sie die Anforderungs Nachricht.</span><span class="sxs-lookup"><span data-stu-id="12d46-130">Process the request message.</span></span>
2. <span data-ttu-id="12d46-131">Ruft `base.SendAsync` auf, um die Anforderung an den inneren Handler zu senden.</span><span class="sxs-lookup"><span data-stu-id="12d46-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="12d46-132">Der innere Handler gibt eine Antwortnachricht zurück.</span><span class="sxs-lookup"><span data-stu-id="12d46-132">The inner handler returns a response message.</span></span> <span data-ttu-id="12d46-133">(Dieser Schritt ist asynchron.)</span><span class="sxs-lookup"><span data-stu-id="12d46-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="12d46-134">Verarbeiten Sie die Antwort und geben Sie Sie an den Aufrufer zurück.</span><span class="sxs-lookup"><span data-stu-id="12d46-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="12d46-135">Im folgenden finden Sie ein einfaches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="12d46-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="12d46-136">Der Aufruf von `base.SendAsync` ist asynchron.</span><span class="sxs-lookup"><span data-stu-id="12d46-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="12d46-137">Wenn der Handler nach diesem-Befehl eine beliebige Aufgabe durchführt, verwenden Sie das **Erwartungs Wort,** wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="12d46-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="12d46-138">Ein delegier ender Handler kann auch den inneren Handler überspringen und die Antwort direkt erstellen:</span><span class="sxs-lookup"><span data-stu-id="12d46-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="12d46-139">Wenn ein delegier Ende Handler die Antwort erstellt, ohne `base.SendAsync`zu aufrufen, überspringt die Anforderung den Rest der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="12d46-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="12d46-140">Dies kann für einen Handler nützlich sein, der die Anforderung überprüft (Erstellen einer Fehler Antwort).</span><span class="sxs-lookup"><span data-stu-id="12d46-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="12d46-141">Hinzufügen eines Handlers zur Pipeline</span><span class="sxs-lookup"><span data-stu-id="12d46-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="12d46-142">Zum Hinzufügen eines Nachrichten Handlers auf der Serverseite fügen Sie den Handler der **httpconfiguration. messagehandlers** -Auflistung hinzu.</span><span class="sxs-lookup"><span data-stu-id="12d46-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="12d46-143">Wenn Sie die Vorlage "ASP.NET MVC 4-Webanwendung" verwendet haben, um das Projekt zu erstellen, können Sie dies innerhalb der **webapiconfig** -Klasse tun:</span><span class="sxs-lookup"><span data-stu-id="12d46-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="12d46-144">Meldungs Handler werden in derselben Reihenfolge aufgerufen, in der Sie in der **messagehandlers** -Auflistung angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="12d46-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="12d46-145">Da Sie eingebettet sind, wird die Antwortnachricht in der anderen Richtung bewegt.</span><span class="sxs-lookup"><span data-stu-id="12d46-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="12d46-146">Das heißt, der letzte Handler ist der erste, der die Antwortnachricht erhält.</span><span class="sxs-lookup"><span data-stu-id="12d46-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="12d46-147">Beachten Sie, dass Sie die inneren Handler nicht festlegen müssen. das Web-API-Framework verbindet automatisch die Nachrichten Handler.</span><span class="sxs-lookup"><span data-stu-id="12d46-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="12d46-148">Wenn Sie [selbst Hosting](../older-versions/self-host-a-web-api.md)haben, erstellen Sie eine Instanz der **httpselfhostconfiguration** -Klasse, und fügen Sie die Handler der **messagehandlers** -Auflistung hinzu.</span><span class="sxs-lookup"><span data-stu-id="12d46-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="12d46-149">Sehen wir uns nun einige Beispiele für benutzerdefinierte Meldungs Handler an.</span><span class="sxs-lookup"><span data-stu-id="12d46-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="12d46-150">Beispiel: X-http-Method-override</span><span class="sxs-lookup"><span data-stu-id="12d46-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="12d46-151">X-http-Method-override ist ein nicht-Standard-HTTP-Header.</span><span class="sxs-lookup"><span data-stu-id="12d46-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="12d46-152">Es ist für Clients konzipiert, die bestimmte HTTP-Anforderungs Typen, z. b. Put oder DELETE, nicht senden können.</span><span class="sxs-lookup"><span data-stu-id="12d46-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="12d46-153">Stattdessen sendet der Client eine Post-Anforderung und legt den Header "X-http-Method-override" auf die gewünschte Methode fest.</span><span class="sxs-lookup"><span data-stu-id="12d46-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="12d46-154">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="12d46-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="12d46-155">Hier ist ein Meldungs Handler, der die Unterstützung für X-http-Method-override hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="12d46-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="12d46-156">In der **SendAsync** -Methode überprüft der Handler, ob die Anforderungs Nachricht eine Post-Anforderung ist und ob Sie den Header "X-http-Method-override" enthält.</span><span class="sxs-lookup"><span data-stu-id="12d46-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="12d46-157">Wenn dies der Fall ist, wird der Header Wert überprüft und dann die Anforderungs Methode geändert.</span><span class="sxs-lookup"><span data-stu-id="12d46-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="12d46-158">Schließlich ruft der Handler `base.SendAsync` auf, um die Nachricht an den nächsten Handler zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="12d46-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="12d46-159">Wenn die Anforderung die **httpcontrollerdispatcher** -Klasse erreicht, leitet **httpcontrollerdispatcher** die Anforderung basierend auf der aktualisierten Anforderungs Methode weiter.</span><span class="sxs-lookup"><span data-stu-id="12d46-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="12d46-160">Beispiel: Hinzufügen eines benutzerdefinierten Antwort Headers</span><span class="sxs-lookup"><span data-stu-id="12d46-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="12d46-161">Hier ist ein Meldungs Handler, der jeder Antwortnachricht einen benutzerdefinierten Header hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="12d46-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="12d46-162">Zuerst ruft der Handler `base.SendAsync` auf, um die Anforderung an den inneren Nachrichten Handler zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="12d46-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="12d46-163">Der innere Handler gibt eine Antwortnachricht zurück, aber dies erfolgt asynchron mit einem **Task&lt;t&gt;** -Objekts.</span><span class="sxs-lookup"><span data-stu-id="12d46-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="12d46-164">Die Antwortnachricht ist erst verfügbar, wenn `base.SendAsync` asynchron abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="12d46-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="12d46-165">In diesem Beispiel wird das **-Schlüsselwort** mit dem Erwartungs Wort zum asynchronen Ausführen von arbeiten nach Abschluss `SendAsync` verwendet</span><span class="sxs-lookup"><span data-stu-id="12d46-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="12d46-166">Wenn Sie .NET Framework 4,0 verwenden, verwenden Sie den **Task**&lt;t&gt; **. ContinueWith** -Methode:</span><span class="sxs-lookup"><span data-stu-id="12d46-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="12d46-167">Beispiel: Überprüfen auf einen API-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="12d46-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="12d46-168">Einige Webdienste erfordern, dass Clients einen API-Schlüssel in Ihre Anforderung einschließen.</span><span class="sxs-lookup"><span data-stu-id="12d46-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="12d46-169">Das folgende Beispiel zeigt, wie ein Nachrichten Handler Anforderungen auf einen gültigen API-Schlüssel überprüfen kann:</span><span class="sxs-lookup"><span data-stu-id="12d46-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="12d46-170">Dieser Handler sucht in der URI-Abfrage Zeichenfolge nach dem API-Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="12d46-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="12d46-171">(In diesem Beispiel wird davon ausgegangen, dass es sich bei dem Schlüssel um eine statische Zeichenfolge handelt.</span><span class="sxs-lookup"><span data-stu-id="12d46-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="12d46-172">Eine wirkliche Implementierung würde wahrscheinlich eine komplexere Validierung verwenden.) Wenn die Abfrage Zeichenfolge den Schlüssel enthält, übergibt der Handler die Anforderung an den inneren Handler.</span><span class="sxs-lookup"><span data-stu-id="12d46-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="12d46-173">Wenn die Anforderung keinen gültigen Schlüssel hat, erstellt der Handler eine Antwortnachricht mit dem Status 403, verboten.</span><span class="sxs-lookup"><span data-stu-id="12d46-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="12d46-174">In diesem Fall ruft der Handler `base.SendAsync`nicht auf, sodass der innere Handler die Anforderung nie empfängt, und auch nicht den Controller.</span><span class="sxs-lookup"><span data-stu-id="12d46-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="12d46-175">Daher kann der Controller davon ausgehen, dass alle eingehenden Anforderungen über einen gültigen API-Schlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="12d46-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="12d46-176">Wenn der API-Schlüssel nur für bestimmte Controller Aktionen gilt, empfiehlt es sich, anstelle eines Nachrichten Handlers einen Aktionsfilter zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="12d46-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="12d46-177">Aktionsfilter werden nach dem URI-Routing ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="12d46-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="12d46-178">Nachrichten Handler pro Route</span><span class="sxs-lookup"><span data-stu-id="12d46-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="12d46-179">Handler in der **httpconfiguration. messagehandlers** -Auflistung gelten global.</span><span class="sxs-lookup"><span data-stu-id="12d46-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="12d46-180">Alternativ können Sie einen Nachrichten Handler zu einer bestimmten Route hinzufügen, wenn Sie die Route definieren:</span><span class="sxs-lookup"><span data-stu-id="12d46-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="12d46-181">In diesem Beispiel wird die Anforderung an `MessageHandler2`gesendet, wenn der Anforderungs-URI "Route2" entspricht.</span><span class="sxs-lookup"><span data-stu-id="12d46-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="12d46-182">Das folgende Diagramm zeigt die Pipeline für diese beiden Routen:</span><span class="sxs-lookup"><span data-stu-id="12d46-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="12d46-183">Beachten Sie, dass `MessageHandler2` den Standardwert **httpcontrollerdispatcher**ersetzt.</span><span class="sxs-lookup"><span data-stu-id="12d46-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="12d46-184">In diesem Beispiel erstellt `MessageHandler2` die Antwort, und Anforderungen, die "Route2" entsprechen, werden nie an einen Controller gesendet.</span><span class="sxs-lookup"><span data-stu-id="12d46-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="12d46-185">Auf diese Weise können Sie den gesamten Web-API-Controller Mechanismus durch ihren eigenen benutzerdefinierten Endpunkt ersetzen.</span><span class="sxs-lookup"><span data-stu-id="12d46-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="12d46-186">Alternativ kann ein Nachrichten Handler pro Route an **httpcontrollerdispatcher**delegieren, der dann an einen Controller weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="12d46-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="12d46-187">Der folgende Code zeigt, wie Sie diese Route konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="12d46-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
