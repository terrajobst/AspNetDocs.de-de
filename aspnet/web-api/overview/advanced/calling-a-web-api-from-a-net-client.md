---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Web-API über einen .NET-Client (C#)-ASP.NET 4. x abrufen
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie Sie eine Web-API aus einer .NET 4. x-Anwendung abrufen.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ab3ba71839123e848dffaa59871f9dac8c1a88d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504957"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="96095-103">Web-API über einen .NET-Client (C#) aufgerufen</span><span class="sxs-lookup"><span data-stu-id="96095-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="96095-104">von [Mike Wasson](https://github.com/MikeWasson) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="96095-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="96095-105">Das [abgeschlossene Projekt herunterladen](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="96095-105">[Download Completed Project](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="96095-106">[Anweisungen zum Download.](/aspnet/core/tutorials/#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="96095-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="96095-107">In diesem Tutorial wird gezeigt, wie Sie eine Web-API mithilfe von [System .net. http. HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) aus einer .NET-Anwendung abrufen.</span><span class="sxs-lookup"><span data-stu-id="96095-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="96095-108">In diesem Tutorial wird eine Client-App geschrieben, die die folgende Web-API nutzt:</span><span class="sxs-lookup"><span data-stu-id="96095-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="96095-109">Aktion</span><span class="sxs-lookup"><span data-stu-id="96095-109">Action</span></span> | <span data-ttu-id="96095-110">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="96095-110">HTTP method</span></span> | <span data-ttu-id="96095-111">Relativer URI</span><span class="sxs-lookup"><span data-stu-id="96095-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="96095-112">Produkt nach ID erhalten</span><span class="sxs-lookup"><span data-stu-id="96095-112">Get a product by ID</span></span> | <span data-ttu-id="96095-113">GET</span><span class="sxs-lookup"><span data-stu-id="96095-113">GET</span></span> | <span data-ttu-id="96095-114">/API/Products/-*ID*</span><span class="sxs-lookup"><span data-stu-id="96095-114">/api/products/*id*</span></span> |
| <span data-ttu-id="96095-115">Erstellen eines neuen Produkts</span><span class="sxs-lookup"><span data-stu-id="96095-115">Create a new product</span></span> | <span data-ttu-id="96095-116">POST</span><span class="sxs-lookup"><span data-stu-id="96095-116">POST</span></span> | <span data-ttu-id="96095-117">/api/products</span><span class="sxs-lookup"><span data-stu-id="96095-117">/api/products</span></span> |
| <span data-ttu-id="96095-118">Aktualisieren eines Produkts</span><span class="sxs-lookup"><span data-stu-id="96095-118">Update a product</span></span> | <span data-ttu-id="96095-119">PUT</span><span class="sxs-lookup"><span data-stu-id="96095-119">PUT</span></span> | <span data-ttu-id="96095-120">/API/Products/-*ID*</span><span class="sxs-lookup"><span data-stu-id="96095-120">/api/products/*id*</span></span> |
| <span data-ttu-id="96095-121">Löschen eines Produkts</span><span class="sxs-lookup"><span data-stu-id="96095-121">Delete a product</span></span> | <span data-ttu-id="96095-122">Delete</span><span class="sxs-lookup"><span data-stu-id="96095-122">DELETE</span></span> | <span data-ttu-id="96095-123">/API/Products/-*ID*</span><span class="sxs-lookup"><span data-stu-id="96095-123">/api/products/*id*</span></span> |

<span data-ttu-id="96095-124">Informationen dazu, wie Sie diese API mit ASP.net-Web-API implementieren, finden Sie unter [Erstellen einer Web-API, die CRUD-Vorgänge unterstützt](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="96095-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="96095-125">Der Einfachheit halber handelt es sich bei der Client Anwendung in diesem Tutorial um eine Windows-Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="96095-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="96095-126">**HttpClient** wird auch für Windows Phone-und Windows Store-Apps unterstützt.</span><span class="sxs-lookup"><span data-stu-id="96095-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="96095-127">Weitere Informationen finden Sie unter [Schreiben von Web-API-Client Code für mehrere Plattformen mit portablen Bibliotheken](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="96095-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="96095-128">Erstellen der Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="96095-128">Create the Console Application</span></span>

<span data-ttu-id="96095-129">Erstellen Sie in Visual Studio eine neue Windows-Konsolen-App mit dem Namen " **httpclientsample** ", und fügen Sie den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="96095-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="96095-130">Der vorangehende Code ist die komplette Client-App.</span><span class="sxs-lookup"><span data-stu-id="96095-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="96095-131">`RunAsync` wird ausgeführt, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="96095-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="96095-132">Die meisten **HttpClient** -Methoden sind asynchron, da Sie Netzwerk-e/a-Vorgänge durchführen.</span><span class="sxs-lookup"><span data-stu-id="96095-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="96095-133">Alle asynchronen Aufgaben werden innerhalb `RunAsync`ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="96095-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="96095-134">Normalerweise blockiert eine APP den Haupt Thread nicht, aber diese APP lässt keine Interaktion zu.</span><span class="sxs-lookup"><span data-stu-id="96095-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="96095-135">Installieren der Web-API-Client Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="96095-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="96095-136">Verwenden Sie den nuget-Paket-Manager zum Installieren des Web-API-Client Bibliotheks Pakets.</span><span class="sxs-lookup"><span data-stu-id="96095-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="96095-137">Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="96095-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="96095-138">Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="96095-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="96095-139">Mit dem vorangehenden Befehl werden dem Projekt die folgenden nuget-Pakete hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="96095-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="96095-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="96095-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="96095-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="96095-141">Newtonsoft.Json</span></span>

<span data-ttu-id="96095-142">Netwonsoft. JSON (auch als JSON.net bezeichnet) ist ein gängiges hochleistungsfähiges JSON-Framework für .net.</span><span class="sxs-lookup"><span data-stu-id="96095-142">Netwonsoft.Json (also known as Json.NET) is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="96095-143">Hinzufügen einer Modell Klasse</span><span class="sxs-lookup"><span data-stu-id="96095-143">Add a Model Class</span></span>

<span data-ttu-id="96095-144">Überprüfen Sie die `Product`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="96095-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="96095-145">Diese Klasse entspricht dem Datenmodell, das von der Web-API verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="96095-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="96095-146">Eine APP kann **HttpClient** verwenden, um eine `Product` Instanz aus einer HTTP-Antwort zu lesen.</span><span class="sxs-lookup"><span data-stu-id="96095-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="96095-147">Die APP muss keinen Deserialisierungscode schreiben.</span><span class="sxs-lookup"><span data-stu-id="96095-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="96095-148">Erstellen und Initialisieren von httpclient</span><span class="sxs-lookup"><span data-stu-id="96095-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="96095-149">Überprüfen Sie die statische **HttpClient** -Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="96095-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="96095-150">**HttpClient** soll einmal instanziiert und während der gesamten Lebensdauer einer Anwendung wieder verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="96095-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="96095-151">Die folgenden Bedingungen können zu **SocketException** -Fehlern führen:</span><span class="sxs-lookup"><span data-stu-id="96095-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="96095-152">Erstellen einer neuen **HttpClient** -Instanz pro Anforderung.</span><span class="sxs-lookup"><span data-stu-id="96095-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="96095-153">Server mit starker Auslastung.</span><span class="sxs-lookup"><span data-stu-id="96095-153">Server under heavy load.</span></span>

<span data-ttu-id="96095-154">Durch das Erstellen einer neuen **HttpClient** -Instanz pro Anforderung können die verfügbaren Sockets erschöpft werden.</span><span class="sxs-lookup"><span data-stu-id="96095-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="96095-155">Der folgende Code initialisiert die **HttpClient** -Instanz:</span><span class="sxs-lookup"><span data-stu-id="96095-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="96095-156">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="96095-156">The preceding code:</span></span>

* <span data-ttu-id="96095-157">Legt den Basis-URI für HTTP-Anforderungen fest.</span><span class="sxs-lookup"><span data-stu-id="96095-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="96095-158">Ändern Sie die Portnummer in den Port, der in der Server-App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="96095-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="96095-159">Die APP funktioniert nur, wenn für die Server-App ein Port verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="96095-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="96095-160">Legt den Accept-Header auf "application/json" fest.</span><span class="sxs-lookup"><span data-stu-id="96095-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="96095-161">Das Festlegen dieses Headers weist den Server an, Daten im JSON-Format zu senden.</span><span class="sxs-lookup"><span data-stu-id="96095-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="96095-162">Senden einer GET-Anforderung zum Abrufen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="96095-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="96095-163">Der folgende Code sendet eine GET-Anforderung für ein Produkt:</span><span class="sxs-lookup"><span data-stu-id="96095-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="96095-164">Die **getasync** -Methode sendet die HTTP GET-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="96095-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="96095-165">Wenn die Methode abgeschlossen ist, gibt Sie eine **httpresponanmessage** zurück, die die HTTP-Antwort enthält.</span><span class="sxs-lookup"><span data-stu-id="96095-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="96095-166">Wenn der Statuscode in der Antwort ein Erfolgs Code ist, enthält der Antworttext die JSON-Darstellung eines Produkts.</span><span class="sxs-lookup"><span data-stu-id="96095-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="96095-167">Aufrufen von "read **asasync** ", um die JSON-Nutzlast in eine `Product` Instanz zu deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="96095-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="96095-168">Die "read **asasync** "-Methode ist asynchron, da der Antworttext beliebig groß sein kann.</span><span class="sxs-lookup"><span data-stu-id="96095-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="96095-169">**HttpClient** löst keine Ausnahme aus, wenn die HTTP-Antwort einen Fehlercode enthält.</span><span class="sxs-lookup"><span data-stu-id="96095-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="96095-170">Stattdessen ist die **issuccess Statuscode** -Eigenschaft **false** , wenn der Status ein Fehlercode ist.</span><span class="sxs-lookup"><span data-stu-id="96095-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="96095-171">Wenn Sie HTTP-Fehlercodes als Ausnahmen behandeln möchten, rufen Sie [HttpResponseMessage. ensuresuccmestatuscode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) für das Response-Objekt auf.</span><span class="sxs-lookup"><span data-stu-id="96095-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="96095-172">`EnsureSuccessStatusCode` löst eine Ausnahme aus, wenn der Statuscode außerhalb des Bereichs 200&ndash;299 liegt.</span><span class="sxs-lookup"><span data-stu-id="96095-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="96095-173">Beachten Sie, dass **HttpClient** Ausnahmen aus anderen Gründen auslösen kann &mdash; z. b. bei einem Timeout der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="96095-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="96095-174">Zu deserialisierende Medientyp-Formatierer</span><span class="sxs-lookup"><span data-stu-id="96095-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="96095-175">Wenn " **leseasasync** " ohne Parameter aufgerufen wird, wird ein Standardsatz von *Medien Formatierern* verwendet, um den Antworttext zu lesen.</span><span class="sxs-lookup"><span data-stu-id="96095-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="96095-176">Die Standardformatierer unterstützen JSON-, XML-und Formular-URL-codierte Daten.</span><span class="sxs-lookup"><span data-stu-id="96095-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="96095-177">Anstatt die Standard Formatierer zu verwenden, können Sie eine Liste von Formatierern für die Methode "read **asasync** " bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="96095-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="96095-178">Die Verwendung einer Liste von Formatierern ist nützlich, wenn Sie über einen benutzerdefinierten Medientyp Formatierer verfügen:</span><span class="sxs-lookup"><span data-stu-id="96095-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="96095-179">Weitere Informationen finden Sie unter [Media Formatters in ASP.net-Web-API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="96095-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="96095-180">Senden einer POST-Anforderung zum Erstellen einer Ressource</span><span class="sxs-lookup"><span data-stu-id="96095-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="96095-181">Der folgende Code sendet eine Post-Anforderung, die eine `Product` Instanz im JSON-Format enthält:</span><span class="sxs-lookup"><span data-stu-id="96095-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="96095-182">Die **postasjsonasync** -Methode:</span><span class="sxs-lookup"><span data-stu-id="96095-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="96095-183">Serialisiert ein Objekt in JSON.</span><span class="sxs-lookup"><span data-stu-id="96095-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="96095-184">Sendet die JSON-Nutzlast in einer POST-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="96095-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="96095-185">Wenn die Anforderung erfolgreich ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="96095-185">If the request succeeds:</span></span>

* <span data-ttu-id="96095-186">Es sollte eine 201-Antwort (created) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="96095-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="96095-187">Die Antwort sollte die URL der erstellten Ressourcen im Location-Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="96095-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="96095-188">Senden einer Put-Anforderung zum Aktualisieren einer Ressource</span><span class="sxs-lookup"><span data-stu-id="96095-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="96095-189">Der folgende Code sendet eine PUT-Anforderung zum Aktualisieren eines Produkts:</span><span class="sxs-lookup"><span data-stu-id="96095-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="96095-190">Die **putasjsonasync** -Methode funktioniert wie **postasjsonasync**, mit dem Unterschied, dass Sie eine PUT-Anforderung anstelle von Post sendet.</span><span class="sxs-lookup"><span data-stu-id="96095-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="96095-191">Hiermit wird eine DELETE-Anforderung zum Löschen einer Ressource gesendet.</span><span class="sxs-lookup"><span data-stu-id="96095-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="96095-192">Der folgende Code sendet eine DELETE-Anforderung zum Löschen eines Produkts:</span><span class="sxs-lookup"><span data-stu-id="96095-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="96095-193">Wie bei Get hat eine Löschanforderung keinen Anforderungs Text.</span><span class="sxs-lookup"><span data-stu-id="96095-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="96095-194">Sie müssen das JSON-oder XML-Format nicht mit DELETE angeben.</span><span class="sxs-lookup"><span data-stu-id="96095-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="96095-195">Testen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="96095-195">Test the sample</span></span>

<span data-ttu-id="96095-196">So testen Sie die Client-App:</span><span class="sxs-lookup"><span data-stu-id="96095-196">To test the client app:</span></span>

1. <span data-ttu-id="96095-197">[Herunterladen](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) und Ausführen der Server-app.</span><span class="sxs-lookup"><span data-stu-id="96095-197">[Download](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="96095-198">[Anweisungen zum Download.](/aspnet/core/#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="96095-198">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="96095-199">Überprüfen Sie, ob die Server-APP funktioniert.</span><span class="sxs-lookup"><span data-stu-id="96095-199">Verify the server app is working.</span></span> <span data-ttu-id="96095-200">Beispielsweise sollte `http://localhost:64195/api/products` eine Liste der Produkte zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="96095-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="96095-201">Legen Sie den Basis-URI für HTTP-Anforderungen fest.</span><span class="sxs-lookup"><span data-stu-id="96095-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="96095-202">Ändern Sie die Portnummer in den Port, der in der Server-App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="96095-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="96095-203">Führen Sie die Client-App aus.</span><span class="sxs-lookup"><span data-stu-id="96095-203">Run the client app.</span></span> <span data-ttu-id="96095-204">Es wird die folgende Ausgabe generiert:</span><span class="sxs-lookup"><span data-stu-id="96095-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
