---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Aktions Ergebnisse in der Web-API 2-ASP.NET 4. x
author: MikeWasson
description: Beschreibt, wie ASP.net-Web-API den Rückgabewert einer Controller Aktion in eine HTTP-Antwortnachricht in ASP.NET 4. x konvertiert.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985839"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="1f329-103">Aktionsergebnisse in Web-API 2</span><span class="sxs-lookup"><span data-stu-id="1f329-103">Action Results in Web API 2</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="1f329-104">In diesem Thema wird beschrieben, wie ASP.net-Web-API den Rückgabewert einer Controller Aktion in eine HTTP-Antwortnachricht konvertiert.</span><span class="sxs-lookup"><span data-stu-id="1f329-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="1f329-105">Eine Web-API-Controller Aktion kann Folgendes zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="1f329-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="1f329-106">void</span><span class="sxs-lookup"><span data-stu-id="1f329-106">void</span></span>
2. <span data-ttu-id="1f329-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="1f329-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="1f329-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="1f329-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="1f329-109">Ein anderer Typ</span><span class="sxs-lookup"><span data-stu-id="1f329-109">Some other type</span></span>

<span data-ttu-id="1f329-110">Abhängig davon, welche dieser zurückgegeben wird, verwendet die Web-API einen anderen Mechanismus zum Erstellen der HTTP-Antwort.</span><span class="sxs-lookup"><span data-stu-id="1f329-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="1f329-111">Rückgabetyp</span><span class="sxs-lookup"><span data-stu-id="1f329-111">Return type</span></span> | <span data-ttu-id="1f329-112">Wie die Web-API die Antwort erstellt</span><span class="sxs-lookup"><span data-stu-id="1f329-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="1f329-113">void</span><span class="sxs-lookup"><span data-stu-id="1f329-113">void</span></span> | <span data-ttu-id="1f329-114">Rückgabe von leeren 204 (kein Inhalt)</span><span class="sxs-lookup"><span data-stu-id="1f329-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="1f329-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="1f329-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="1f329-116">Direkt in eine HTTP-Antwortnachricht konvertieren.</span><span class="sxs-lookup"><span data-stu-id="1f329-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="1f329-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="1f329-117">**IHttpActionResult**</span></span> | <span data-ttu-id="1f329-118">Rufen Sie **ExecuteAsync** auf, um ein **httpresponanmessage**-Element zu erstellen und dann in eine HTTP-Antwortnachricht zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="1f329-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="1f329-119">Anderer Typ</span><span class="sxs-lookup"><span data-stu-id="1f329-119">Other type</span></span> | <span data-ttu-id="1f329-120">Schreiben Sie den serialisierten Rückgabewert in den Antworttext. gibt 200 zurück (OK).</span><span class="sxs-lookup"><span data-stu-id="1f329-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="1f329-121">Im weiteren Verlauf dieses Themas werden die einzelnen Optionen ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="1f329-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="1f329-122">void</span><span class="sxs-lookup"><span data-stu-id="1f329-122">void</span></span>

<span data-ttu-id="1f329-123">Wenn der Rückgabetyp ist `void`, gibt die Web-API einfach eine leere HTTP-Antwort mit dem Statuscode 204 (kein Inhalt) zurück.</span><span class="sxs-lookup"><span data-stu-id="1f329-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="1f329-124">Beispiel Controller:</span><span class="sxs-lookup"><span data-stu-id="1f329-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="1f329-125">HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="1f329-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="1f329-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="1f329-126">HttpResponseMessage</span></span>

<span data-ttu-id="1f329-127">Wenn die Aktion ein [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)-Objekt zurückgibt, konvertiert die Web-API den Rückgabewert direkt in eine HTTP-Antwortnachricht, wobei die Eigenschaften des **HttpResponseMessage** -Objekts verwendet werden, um die Antwort aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="1f329-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="1f329-128">Diese Option bietet eine große Kontrolle über die Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="1f329-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="1f329-129">Beispielsweise wird mit der folgenden Controller Aktion der Cache-Control-Header festgelegt.</span><span class="sxs-lookup"><span data-stu-id="1f329-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="1f329-130">Antwort:</span><span class="sxs-lookup"><span data-stu-id="1f329-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="1f329-131">Wenn Sie ein Domänen Modell an die Methode " **samateresponse** " übergeben, verwendet die Web-API einen [medienformatierer](../formats-and-model-binding/media-formatters.md) , um das serialisierte Modell in den Antworttext zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="1f329-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="1f329-132">Die Web-API verwendet den Accept-Header in der Anforderung, um das Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="1f329-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="1f329-133">Weitere Informationen finden Sie unter [inhaltsaus](../formats-and-model-binding/content-negotiation.md)Handlung.</span><span class="sxs-lookup"><span data-stu-id="1f329-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="1f329-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="1f329-134">IHttpActionResult</span></span>

<span data-ttu-id="1f329-135">Die **ihttpactionresult** -Schnittstelle wurde in der Web-API 2 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="1f329-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="1f329-136">Im Wesentlichen wird eine **httpresponsmessage** -Factory definiert.</span><span class="sxs-lookup"><span data-stu-id="1f329-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="1f329-137">Dies sind einige Vorteile der Verwendung der **ihttpactionresult** -Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="1f329-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="1f329-138">Vereinfacht das Komponenten [Testen](../testing-and-debugging/unit-testing-controllers-in-web-api.md) ihrer Controller.</span><span class="sxs-lookup"><span data-stu-id="1f329-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="1f329-139">Verschiebt gängige Logik zum Erstellen von HTTP-Antworten in separate Klassen.</span><span class="sxs-lookup"><span data-stu-id="1f329-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="1f329-140">Macht die Absicht der Controller Aktion klarer, indem die Details auf niedriger Ebene zum Erstellen der Antwort ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="1f329-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="1f329-141">**Ihttpactionresult** enthält eine einzige Methode, **ExecuteAsync**, die asynchron eine **HttpResponseMessage** -Instanz erstellt.</span><span class="sxs-lookup"><span data-stu-id="1f329-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="1f329-142">Wenn eine Controller Aktion ein **ihttpactionresult**zurückgibt, ruft die Web-API die **ExecuteAsync** -Methode auf, um eine **HttpResponseMessage**-Methode zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1f329-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="1f329-143">Anschließend wird **httpresponanmessage** in eine HTTP-Antwortnachricht konvertiert.</span><span class="sxs-lookup"><span data-stu-id="1f329-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="1f329-144">Im folgenden finden Sie eine einfache Implementierung von **ihttpactionresult** , die eine nur-Text-Antwort erstellt:</span><span class="sxs-lookup"><span data-stu-id="1f329-144">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="1f329-145">Beispiel für eine Controller Aktion:</span><span class="sxs-lookup"><span data-stu-id="1f329-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="1f329-146">Antwort:</span><span class="sxs-lookup"><span data-stu-id="1f329-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="1f329-147">In den meisten Fällen verwenden Sie die im **[System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** -Namespace definierten **ihttpactionresult** -Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="1f329-147">More often, you use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="1f329-148">Die **apicontroller** -Klasse definiert Hilfsmethoden, die diese integrierten Aktions Ergebnisse zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="1f329-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="1f329-149">Wenn die Anforderung im folgenden Beispiel nicht mit einer vorhandenen Produkt-ID identisch ist, ruft der Controller [apicontroller. NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) auf, um eine 404-Antwort (nicht gefunden) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1f329-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="1f329-150">Andernfalls ruft der Controller [apicontroller. OK](https://msdn.microsoft.com/library/dn314591.aspx)auf, wodurch eine 200-Antwort (OK) erstellt wird, die das Produkt enthält.</span><span class="sxs-lookup"><span data-stu-id="1f329-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="1f329-151">Andere Rückgabe Typen</span><span class="sxs-lookup"><span data-stu-id="1f329-151">Other Return Types</span></span>

<span data-ttu-id="1f329-152">Für alle anderen Rückgabe Typen verwendet die Web-API einen [medienformatierer](../formats-and-model-binding/media-formatters.md) , um den Rückgabewert zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="1f329-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="1f329-153">Die Web-API schreibt den serialisierten Wert in den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="1f329-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="1f329-154">Der Antwortstatus Code ist 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="1f329-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="1f329-155">Ein Nachteil dieses Ansatzes besteht darin, dass Sie keinen Fehlercode direkt zurückgeben können, z. b. 404.</span><span class="sxs-lookup"><span data-stu-id="1f329-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="1f329-156">Sie können jedoch eine **httpresponmenexception** für Fehlercodes auslösen.</span><span class="sxs-lookup"><span data-stu-id="1f329-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="1f329-157">Weitere Informationen finden Sie unter [Ausnahmebehandlung in ASP.net-Web-API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="1f329-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="1f329-158">Die Web-API verwendet den Accept-Header in der Anforderung, um das Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="1f329-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="1f329-159">Weitere Informationen finden Sie unter [inhaltsaus](../formats-and-model-binding/content-negotiation.md)Handlung.</span><span class="sxs-lookup"><span data-stu-id="1f329-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="1f329-160">Beispiel Anforderung</span><span class="sxs-lookup"><span data-stu-id="1f329-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="1f329-161">Beispiel Antwort</span><span class="sxs-lookup"><span data-stu-id="1f329-161">Example response</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
