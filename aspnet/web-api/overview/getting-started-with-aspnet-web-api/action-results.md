---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Dies führt Web-API 2 - ASP.NET 4.x
author: MikeWasson
description: Beschreibt, wie den Rückgabewert von ASP.NET Web-API über eine Controlleraktion in HTTP-Antwortnachricht in ASP.NET konvertiert 4.x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 87f71938a5c5f38d3a456ba9339540f67e236e1a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59400892"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="fa4ab-103">Aktionsergebnisse in Web-API 2</span><span class="sxs-lookup"><span data-stu-id="fa4ab-103">Action Results in Web API 2</span></span>

<span data-ttu-id="fa4ab-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fa4ab-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fa4ab-105">In diesem Thema wird beschrieben, wie ASP.NET Web-API den Rückgabewert von eine Controlleraktion in eine HTTP-Response-Nachricht konvertiert.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-105">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="fa4ab-106">Eine Controlleraktion der Web-API kann Folgendes zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-106">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="fa4ab-107">void</span><span class="sxs-lookup"><span data-stu-id="fa4ab-107">void</span></span>
2. <span data-ttu-id="fa4ab-108">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="fa4ab-108">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="fa4ab-109">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="fa4ab-109">**IHttpActionResult**</span></span>
4. <span data-ttu-id="fa4ab-110">Eine andere Art</span><span class="sxs-lookup"><span data-stu-id="fa4ab-110">Some other type</span></span>

<span data-ttu-id="fa4ab-111">Je nachdem, welche dieser zurückgegeben wird, Web-API einen anderen Mechanismus verwendet, um die HTTP-Antwort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-111">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="fa4ab-112">Rückgabetyp</span><span class="sxs-lookup"><span data-stu-id="fa4ab-112">Return type</span></span> | <span data-ttu-id="fa4ab-113">Wie die Antwort erstellt Web-API</span><span class="sxs-lookup"><span data-stu-id="fa4ab-113">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="fa4ab-114">void</span><span class="sxs-lookup"><span data-stu-id="fa4ab-114">void</span></span> | <span data-ttu-id="fa4ab-115">Leere 204 (No Content) zurückgeben</span><span class="sxs-lookup"><span data-stu-id="fa4ab-115">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="fa4ab-116">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="fa4ab-116">**HttpResponseMessage**</span></span> | <span data-ttu-id="fa4ab-117">Konvertieren Sie direkt in HTTP-Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-117">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="fa4ab-118">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="fa4ab-118">**IHttpActionResult**</span></span> | <span data-ttu-id="fa4ab-119">Rufen Sie **ExecuteAsync** zum Erstellen einer **HttpResponseMessage**, dann in HTTP-Antwortnachricht zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-119">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="fa4ab-120">Andere Art</span><span class="sxs-lookup"><span data-stu-id="fa4ab-120">Other type</span></span> | <span data-ttu-id="fa4ab-121">Schreiben Sie den serialisierten Rückgabewert in den Antworttext. 200 (OK) zurück</span><span class="sxs-lookup"><span data-stu-id="fa4ab-121">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="fa4ab-122">Im weiteren Verlauf dieses Themas wird jede Option ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-122">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="fa4ab-123">void</span><span class="sxs-lookup"><span data-stu-id="fa4ab-123">void</span></span>

<span data-ttu-id="fa4ab-124">Wenn der Rückgabetyp ist `void`, Web-API gibt einfach eine leere HTTP-Antwort mit dem Statuscode 204 (No Content) zurück.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-124">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="fa4ab-125">Beispiel-Controller:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-125">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="fa4ab-126">HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-126">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="fa4ab-127">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="fa4ab-127">HttpResponseMessage</span></span>

<span data-ttu-id="fa4ab-128">Wenn die Aktion gibt ein [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web-API konvertiert den Rückgabewert direkt in eine HTTP-Antwortnachricht mithilfe der Eigenschaften des der **HttpResponseMessage** zu füllende Objekt die die Antwort.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-128">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="fa4ab-129">Diese Option erhalten Sie umfassende Kontrolle über die Response-Nachricht.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-129">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="fa4ab-130">Beispielsweise legt die folgende Controlleraktion Cache-Control-Headers.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-130">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="fa4ab-131">Antwort:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-131">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="fa4ab-132">Wenn Sie ein Domänenmodell, übergeben die **CreateResponse** -Methode, die Web-API verwendet einen [medienformatierer](../formats-and-model-binding/media-formatters.md) das serialisierte Modell in den Antworttext schreiben.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-132">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="fa4ab-133">Web-API verwendet den Accept-Header in der Anforderung verwendet, um den Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-133">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="fa4ab-134">Weitere Informationen finden Sie unter [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="fa4ab-134">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="fa4ab-135">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="fa4ab-135">IHttpActionResult</span></span>

<span data-ttu-id="fa4ab-136">Die **IHttpActionResult** -Schnittstelle wurde in der Web-API 2 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-136">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="fa4ab-137">Im Wesentlichen definiert es ein **HttpResponseMessage** Factory.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-137">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="fa4ab-138">Hier sind einige Vorteile der Verwendung der **IHttpActionResult** Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-138">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="fa4ab-139">Vereinfacht die [Komponententests](../testing-and-debugging/unit-testing-controllers-in-web-api.md) Ihren Controllern.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-139">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="fa4ab-140">Verschiebt die allgemeine Logik für das Erstellen von HTTP-Antworten in separate Klassen.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-140">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="fa4ab-141">Macht die Absicht der Controlleraktion klarer, durch das Ausblenden der Details auf niedriger Ebene, der die Antwort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-141">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="fa4ab-142">**IHttpActionResult** enthält eine einzelne Methode, **ExecuteAsync**, erstellt der asynchron eine **HttpResponseMessage** Instanz.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-142">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="fa4ab-143">Wenn eine Controlleraktion zurückgibt ein **IHttpActionResult**, Web-API-Aufrufe der **ExecuteAsync** Methode zum Erstellen einer **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-143">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="fa4ab-144">Anschließend konvertiert der **HttpResponseMessage** in HTTP-Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-144">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="fa4ab-145">Hier ist eine einfache Implementierung der **IHttpActionResult** eine nur-Text-Antwort erstellt:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-145">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="fa4ab-146">Beispiel-Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-146">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="fa4ab-147">Antwort:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-147">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="fa4ab-148">Häufiger, verwenden Sie die **IHttpActionResult** Implementierungen, die definiert, der **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** Namespace.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-148">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="fa4ab-149">Die **ApiController** -Klasse definiert Hilfsmethoden, die diese integrierte Aktionsergebnisse zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-149">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="fa4ab-150">Im folgenden Beispiel, wenn die Anforderung keine vorhandenen Produkt-ID übereinstimmt der Controller ruft [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) zum Erstellen einer 404 (nicht gefunden).</span><span class="sxs-lookup"><span data-stu-id="fa4ab-150">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="fa4ab-151">Der Controller, andernfalls ruft [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), die einer Antwort 200 (OK), erstellt das Produkt enthält.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-151">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="fa4ab-152">Andere Rückgabetypen</span><span class="sxs-lookup"><span data-stu-id="fa4ab-152">Other Return Types</span></span>

<span data-ttu-id="fa4ab-153">Für alle anderen Rückgabetypen, Web-API verwendet einen [medienformatierer](../formats-and-model-binding/media-formatters.md) zum Serialisieren des zurückgegeben Wert.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-153">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="fa4ab-154">Web-API schreibt den serialisierten Wert in den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-154">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="fa4ab-155">Statuscode der Antwort ist 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="fa4ab-155">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="fa4ab-156">Ein Nachteil dieses Ansatzes ist, dass Sie direkt einen Fehlercode, z. B. 404 zurückgeben können.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-156">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="fa4ab-157">Sie können jedoch Folgendes Auslösen einer **HttpResponseException** Fehlercodes.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-157">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="fa4ab-158">Weitere Informationen finden Sie unter [Exception Handling in ASP.NET Web-API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="fa4ab-158">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="fa4ab-159">Web-API verwendet den Accept-Header in der Anforderung verwendet, um den Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="fa4ab-159">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="fa4ab-160">Weitere Informationen finden Sie unter [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="fa4ab-160">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="fa4ab-161">Beispiel für eine Anforderung</span><span class="sxs-lookup"><span data-stu-id="fa4ab-161">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="fa4ab-162">Beispielantwort:</span><span class="sxs-lookup"><span data-stu-id="fa4ab-162">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
