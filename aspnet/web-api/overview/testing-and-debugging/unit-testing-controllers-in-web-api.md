---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Komponenten Test Controller in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Thema werden einige spezielle Techniken für Komponenten Test Controller in der Web-API 2 beschrieben. Bevor Sie dieses Thema lesen, sollten Sie die tutorialeinheit lesen...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447003"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="2550e-104">Komponententests für Controller in ASP.NET-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="2550e-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>

<span data-ttu-id="2550e-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2550e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2550e-106">In diesem Thema werden einige spezielle Techniken für Komponenten Test Controller in der Web-API 2 beschrieben.</span><span class="sxs-lookup"><span data-stu-id="2550e-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="2550e-107">Bevor Sie dieses Thema lesen, sollten Sie das Tutorial [Unit Testing ASP.net-Web-API 2](unit-testing-with-aspnet-web-api.md)lesen, das zeigt, wie Sie der Projekt Mappe ein Komponenten Testprojekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2550e-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2550e-108">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="2550e-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="2550e-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2550e-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="2550e-110">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="2550e-110">Web API 2</span></span>
> - <span data-ttu-id="2550e-111">[4.5.30](https://github.com/Moq)</span><span class="sxs-lookup"><span data-stu-id="2550e-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="2550e-112">Ich habe mich für die Verwendung von "muq", aber dieselbe Idee gilt für jedes andere Framework.</span><span class="sxs-lookup"><span data-stu-id="2550e-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="2550e-113">"Muq 4.5.30 (und höher)" unterstützt Visual Studio 2017, Roslyn und .NET 4,5 und höhere Versionen.</span><span class="sxs-lookup"><span data-stu-id="2550e-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="2550e-114">Ein gängiges Muster in Komponententests ist &quot;"Arrange-Act-Assert"-&quot;:</span><span class="sxs-lookup"><span data-stu-id="2550e-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="2550e-115">Anordnen: richten Sie alle Voraussetzungen ein, damit der Test ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="2550e-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="2550e-116">Act: führt den Test aus.</span><span class="sxs-lookup"><span data-stu-id="2550e-116">Act: Perform the test.</span></span>
- <span data-ttu-id="2550e-117">Assert: Überprüfen Sie, ob der Test erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="2550e-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="2550e-118">Im Schritt "Anordnen" werden häufig Mock-oder Stub-Objekte verwendet.</span><span class="sxs-lookup"><span data-stu-id="2550e-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="2550e-119">Dadurch wird die Anzahl der Abhängigkeiten minimiert, daher konzentriert sich der Test auf das Testen eines solchen.</span><span class="sxs-lookup"><span data-stu-id="2550e-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="2550e-120">Im folgenden finden Sie einige Punkte, die Sie in Ihren Web-API-Controllern testen sollten:</span><span class="sxs-lookup"><span data-stu-id="2550e-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="2550e-121">Die Aktion gibt den korrekten Antworttyp zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="2550e-122">Ungültige Parameter geben die korrekte Fehler Antwort zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="2550e-123">Die Aktion ruft die richtige Methode für das Repository oder die Dienst Ebene auf.</span><span class="sxs-lookup"><span data-stu-id="2550e-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="2550e-124">Wenn die Antwort ein Domänen Modell enthält, überprüfen Sie den Modelltyp.</span><span class="sxs-lookup"><span data-stu-id="2550e-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="2550e-125">Dies sind einige der allgemeinen Dinge, die getestet werden müssen, aber die Besonderheiten hängen von ihrer Controller Implementierung ab.</span><span class="sxs-lookup"><span data-stu-id="2550e-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="2550e-126">Insbesondere ist ein großer Unterschied, ob die Controller Aktionen " **HttpResponseMessage** " oder " **ihttpactionresult**" zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="2550e-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="2550e-127">Weitere Informationen zu diesen Ergebnistypen finden Sie unter [Aktions Ergebnisse in der Web-API 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="2550e-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="2550e-128">Testen von Aktionen, die httprespontmessage zurückgeben</span><span class="sxs-lookup"><span data-stu-id="2550e-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="2550e-129">Im folgenden finden Sie ein Beispiel für einen Controller, dessen Aktionen " **httpresponabmessage**" zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="2550e-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="2550e-130">Beachten Sie, dass der Controller eine `IProductRepository`mit Abhängigkeitsinjektion Einschleusung</span><span class="sxs-lookup"><span data-stu-id="2550e-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="2550e-131">Dadurch kann der Controller schneller getestet werden, da Sie ein Mock-Repository einfügen können.</span><span class="sxs-lookup"><span data-stu-id="2550e-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="2550e-132">Der folgende Komponenten Test überprüft, ob die `Get`-Methode eine `Product` in den Antworttext schreibt.</span><span class="sxs-lookup"><span data-stu-id="2550e-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="2550e-133">Angenommen, `repository` ist ein Mock `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="2550e-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="2550e-134">Es ist wichtig, die **Anforderung** und **Konfiguration** auf dem Controller festzulegen.</span><span class="sxs-lookup"><span data-stu-id="2550e-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="2550e-135">Andernfalls schlägt der Test mit einer **ArgumentNullException** oder **InvalidOperationException**fehl.</span><span class="sxs-lookup"><span data-stu-id="2550e-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="2550e-136">Test Link Generierung</span><span class="sxs-lookup"><span data-stu-id="2550e-136">Testing Link Generation</span></span>

<span data-ttu-id="2550e-137">Die `Post`-Methode ruft **Urlhelper. Link** auf, um Links in der Antwort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2550e-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="2550e-138">Dies erfordert ein wenig mehr Setup im Komponenten Test:</span><span class="sxs-lookup"><span data-stu-id="2550e-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="2550e-139">Die **Urlhelper** -Klasse benötigt die Anforderungs-URL und die Routendaten, sodass der Test Werte für diese festlegen muss.</span><span class="sxs-lookup"><span data-stu-id="2550e-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="2550e-140">Eine andere Möglichkeit ist Mock oder Stub **Urlhelper**.</span><span class="sxs-lookup"><span data-stu-id="2550e-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="2550e-141">Bei diesem Ansatz ersetzen Sie den Standardwert von " [apicontroller. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) " durch eine Mock-oder Stubversion, die einen festgelegten Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2550e-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="2550e-142">Wir schreiben den Test mit dem " [muq](https://github.com/Moq) "-Framework neu.</span><span class="sxs-lookup"><span data-stu-id="2550e-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="2550e-143">Installieren Sie das nuget-Paket `Moq` im Testprojekt.</span><span class="sxs-lookup"><span data-stu-id="2550e-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="2550e-144">In dieser Version müssen Sie keine Routendaten einrichten, da der Mock- **Urlhelper** eine Konstante Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2550e-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>

## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="2550e-145">Testen von Aktionen, die ihttpactionresult zurückgeben</span><span class="sxs-lookup"><span data-stu-id="2550e-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="2550e-146">In der Web-API 2 kann eine Controller Aktion **ihttpactionresult**zurückgeben, was analog zu **Action result** in ASP.NET MVC ist.</span><span class="sxs-lookup"><span data-stu-id="2550e-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="2550e-147">Die **ihttpactionresult** -Schnittstelle definiert ein Befehls Muster zum Erstellen von HTTP-Antworten.</span><span class="sxs-lookup"><span data-stu-id="2550e-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="2550e-148">Anstatt die Antwort direkt zu erstellen, gibt der Controller ein **ihttpactionresult**zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="2550e-149">Später Ruft die Pipeline das **ihttpactionresult** auf, um die Antwort zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2550e-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="2550e-150">Diese Vorgehensweise erleichtert das Schreiben von Komponententests, da Sie eine große Menge an Setup überspringen können, die für **HttpResponseMessage**benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="2550e-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="2550e-151">Hier ist ein Beispiel Controller, dessen Aktionen **ihttpactionresult**zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="2550e-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="2550e-152">In diesem Beispiel werden einige gängige Muster mithilfe von **ihttpactionresult**veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="2550e-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="2550e-153">Sehen wir uns an, wie Sie Komponententests ausführen.</span><span class="sxs-lookup"><span data-stu-id="2550e-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="2550e-154">Action gibt 200 (OK) mit einem Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="2550e-155">Die `Get`-Methode ruft `Ok(product)` auf, wenn das Produkt gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="2550e-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="2550e-156">Stellen Sie im Komponenten Test sicher, dass der Rückgabetyp **okaushandatedcontentresult** und das zurückgegebene Produkt die Rechte-ID aufweist.</span><span class="sxs-lookup"><span data-stu-id="2550e-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="2550e-157">Beachten Sie, dass der Komponenten Test das Aktions Ergebnis nicht ausführt.</span><span class="sxs-lookup"><span data-stu-id="2550e-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="2550e-158">Sie können davon ausgehen, dass das Aktions Ergebnis die HTTP-Antwort ordnungsgemäß erstellt.</span><span class="sxs-lookup"><span data-stu-id="2550e-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="2550e-159">(Aus diesem Grund verfügt das Web-API-Framework über eigene Komponententests!)</span><span class="sxs-lookup"><span data-stu-id="2550e-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="2550e-160">Aktion gibt 404 zurück (nicht gefunden)</span><span class="sxs-lookup"><span data-stu-id="2550e-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="2550e-161">Die `Get`-Methode ruft `NotFound()` auf, wenn das Produkt nicht gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="2550e-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="2550e-162">In diesem Fall prüft der Komponenten Test nur, ob der Rückgabetyp **notfoundresult**ist.</span><span class="sxs-lookup"><span data-stu-id="2550e-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="2550e-163">Action gibt 200 (OK) ohne Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="2550e-164">Die `Delete`-Methode ruft `Ok()` auf, um eine leere HTTP 200-Antwort zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="2550e-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="2550e-165">Wie im vorherigen Beispiel überprüft der UnitTest den Rückgabetyp, in diesem Fall **okresult**.</span><span class="sxs-lookup"><span data-stu-id="2550e-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="2550e-166">Aktion gibt 201 (erstellt) mit einem Location-Header zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="2550e-167">Die `Post`-Methode ruft `CreatedAtRoute` auf, um eine HTTP 201-Antwort mit einem URI im Location-Header zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="2550e-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="2550e-168">Überprüfen Sie im Komponenten Test, ob die richtigen Routing Werte von der Aktion festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="2550e-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="2550e-169">Action gibt einen weiteren 2xx mit einem Antworttext zurück.</span><span class="sxs-lookup"><span data-stu-id="2550e-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="2550e-170">Mit der `Put`-Methode wird `Content` aufgerufen, um eine HTTP 202-Antwort (akzeptiert) mit einem Antworttext zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="2550e-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="2550e-171">Dieser Fall ähnelt der Rückgabe von 200 (OK), aber der Komponenten Test sollte auch den Statuscode überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2550e-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="2550e-172">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2550e-172">Additional Resources</span></span>

- [<span data-ttu-id="2550e-173">Entity Framework, wenn Komponententests ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="2550e-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="2550e-174">[Schreiben von Tests für einen ASP.net-Web-API-Dienst](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (Blogbeitrag von Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="2550e-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="2550e-175">Debuggen von ASP.net-Web-API mit dem Routen Debugger</span><span class="sxs-lookup"><span data-stu-id="2550e-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
