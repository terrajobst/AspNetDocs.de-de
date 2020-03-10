---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Bson-Unterstützung in ASP.net-Web-API 2,1-ASP.NET 4. x
author: MikeWasson
description: zeigt, wie bson in einem Web-API-Controller (serverseitig) und in einer .NET-Client-App für ASP.NET 4. x verwendet wird.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504681"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="6262a-103">Bson-Unterstützung in ASP.net-Web-API 2,1</span><span class="sxs-lookup"><span data-stu-id="6262a-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="6262a-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6262a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6262a-105">In diesem Thema wird gezeigt, wie Sie bson in Ihrem Web-API-Controller (serverseitig) und in einer .NET-Client-App verwenden.</span><span class="sxs-lookup"><span data-stu-id="6262a-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="6262a-106">Die Web-API 2,1 führt die Unterstützung für bson ein.</span><span class="sxs-lookup"><span data-stu-id="6262a-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="6262a-107">Was ist bson?</span><span class="sxs-lookup"><span data-stu-id="6262a-107">What is BSON?</span></span>

<span data-ttu-id="6262a-108">[Bson](http://bsonspec.org/) ist ein binäres Serialisierungsformat.</span><span class="sxs-lookup"><span data-stu-id="6262a-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="6262a-109">"Bson" steht für "Binary JSON", bson und JSON werden jedoch sehr unterschiedlich serialisiert.</span><span class="sxs-lookup"><span data-stu-id="6262a-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="6262a-110">Bson ist "JSON-like", da Objekte ähnlich wie JSON als Name-Wert-Paare dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="6262a-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="6262a-111">Im Gegensatz zu JSON werden numerische Datentypen als Bytes und nicht als Zeichen folgen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="6262a-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="6262a-112">Bson war so konzipiert, dass es sich um eine einfache, leicht zu Scanende und schnelle Codierung/Decodierung handelt.</span><span class="sxs-lookup"><span data-stu-id="6262a-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="6262a-113">Bson ist vergleichbar mit JSON.</span><span class="sxs-lookup"><span data-stu-id="6262a-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="6262a-114">Je nach den Daten kann eine BSON-Nutzlast kleiner oder größer als eine JSON-Nutzlast sein.</span><span class="sxs-lookup"><span data-stu-id="6262a-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="6262a-115">Zum Serialisieren von Binärdaten, wie z. b. einer Bilddatei, ist bson kleiner als JSON, da die Binärdaten nicht base64-codiert sind.</span><span class="sxs-lookup"><span data-stu-id="6262a-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="6262a-116">Bson-Dokumente können leicht überprüft werden, da Elementen ein Längenfeld vorangestellt wird, sodass ein Parser Elemente überspringen kann, ohne Sie zu decodieren.</span><span class="sxs-lookup"><span data-stu-id="6262a-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="6262a-117">Codierung und Decodierung sind effizient, da numerische Datentypen als Zahlen und nicht als Zeichen folgen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="6262a-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="6262a-118">Native Clients, wie z. b. .NET-Client-apps, können von der Verwendung von bson anstelle von textbasierten Formaten wie JSON oder XML profitieren.</span><span class="sxs-lookup"><span data-stu-id="6262a-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="6262a-119">Bei Browser Clients ist es wahrscheinlich, dass Sie sich auf JSON beschränken, da JavaScript die JSON-Nutzlast direkt konvertieren kann.</span><span class="sxs-lookup"><span data-stu-id="6262a-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="6262a-120">Glücklicherweise verwendet die Web-API [inhaltsaus](content-negotiation.md)Handlung, sodass Ihre API beide Formate unterstützen und den Client auswählen kann.</span><span class="sxs-lookup"><span data-stu-id="6262a-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="6262a-121">Aktivieren von bson auf dem Server</span><span class="sxs-lookup"><span data-stu-id="6262a-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="6262a-122">Fügen Sie in Ihrer Web-API-Konfiguration den **bsonmediatyetformatter** der formatiererauflistung hinzu.</span><span class="sxs-lookup"><span data-stu-id="6262a-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="6262a-123">Wenn der Client nun "Application/bson" anfordert, verwendet die Web-API den bson-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="6262a-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="6262a-124">Um bson anderen Medientypen zuzuordnen, fügen Sie Sie der supportedmediatypes-Auflistung hinzu.</span><span class="sxs-lookup"><span data-stu-id="6262a-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="6262a-125">Mit dem folgenden Code wird "application/vnd. Configuration. config" zu den unterstützten Medientypen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="6262a-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="6262a-126">HTTP-Beispielsitzung</span><span class="sxs-lookup"><span data-stu-id="6262a-126">Example HTTP Session</span></span>

<span data-ttu-id="6262a-127">In diesem Beispiel verwenden wir die folgende Modell Klasse plus einen einfachen Web-API-Controller:</span><span class="sxs-lookup"><span data-stu-id="6262a-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="6262a-128">Ein Client sendet möglicherweise die folgende HTTP-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="6262a-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="6262a-129">Hier ist die Antwort:</span><span class="sxs-lookup"><span data-stu-id="6262a-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="6262a-130">Hier habe ich die Binärdaten durch &quot;ersetzt.&quot; Zeichen.</span><span class="sxs-lookup"><span data-stu-id="6262a-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="6262a-131">Der folgende Screenshot von "fddler" zeigt die unformatierten Hex-Werte.</span><span class="sxs-lookup"><span data-stu-id="6262a-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="6262a-132">Verwenden von bson mit httpclient</span><span class="sxs-lookup"><span data-stu-id="6262a-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="6262a-133">.NET-Client-Apps können den bson-Formatierer mit **HttpClient**verwenden.</span><span class="sxs-lookup"><span data-stu-id="6262a-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="6262a-134">Weitere Informationen zu **HttpClient**finden Sie unter [Aufrufen einer Web-API von einem .NET-Client](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="6262a-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="6262a-135">Der folgende Code sendet eine GET-Anforderung, die bson akzeptiert, und deserialisiert dann die bson-Nutzlast in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="6262a-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="6262a-136">Um bson vom Server anzufordern, legen Sie den Accept-Header auf "Application/bson" fest:</span><span class="sxs-lookup"><span data-stu-id="6262a-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="6262a-137">Um den Antworttext zu deserialisieren, verwenden Sie den **bsonmediatypeer Formatter**.</span><span class="sxs-lookup"><span data-stu-id="6262a-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="6262a-138">Dieser Formatierer ist nicht in der standardformatiererauflistung enthalten, daher müssen Sie ihn beim Lesen des Antwort Texts angeben:</span><span class="sxs-lookup"><span data-stu-id="6262a-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="6262a-139">Im nächsten Beispiel wird gezeigt, wie eine Post-Anforderung gesendet wird, die bson enthält.</span><span class="sxs-lookup"><span data-stu-id="6262a-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="6262a-140">Ein Großteil dieses Codes ist mit dem vorherigen Beispiel identisch.</span><span class="sxs-lookup"><span data-stu-id="6262a-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="6262a-141">Geben Sie in der **postasync** -Methode jedoch **bsonmediatypeer Formatter** als Formatierer an:</span><span class="sxs-lookup"><span data-stu-id="6262a-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="6262a-142">Serialisieren primitiver Typen der obersten Ebene</span><span class="sxs-lookup"><span data-stu-id="6262a-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="6262a-143">Jedes bson-Dokument ist eine Liste von Schlüssel-Wert-Paaren. Die bson-Spezifikation definiert keine Syntax zum Serialisieren eines einzelnen Rohwerts, wie z. b. eine ganze Zahl oder eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="6262a-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="6262a-144">Um diese Einschränkung zu umgehen, behandelt **bsonmediatypformatter** primitive Typen als Sonderfall.</span><span class="sxs-lookup"><span data-stu-id="6262a-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="6262a-145">Vor der Serialisierung konvertiert Sie den Wert in ein Schlüssel/Wert-Paar mit dem Schlüssel "Wert".</span><span class="sxs-lookup"><span data-stu-id="6262a-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="6262a-146">Angenommen, Ihr API-Controller gibt eine ganze Zahl zurück:</span><span class="sxs-lookup"><span data-stu-id="6262a-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="6262a-147">Vor der Serialisierung konvertiert das bson-Formatierungs Programm dieses in das folgende Schlüssel-Wert-Paar:</span><span class="sxs-lookup"><span data-stu-id="6262a-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="6262a-148">Wenn Sie deserialisieren, konvertiert der Formatierer die Daten zurück in den ursprünglichen Wert.</span><span class="sxs-lookup"><span data-stu-id="6262a-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="6262a-149">Clients, die einen anderen bson-Parser verwenden, müssen diesen Fall jedoch verarbeiten, wenn Ihre Web-API Rohwerte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="6262a-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="6262a-150">Im Allgemeinen sollten Sie die Rückgabe strukturierter Daten anstelle von Rohwerten in Erwägung gezogen.</span><span class="sxs-lookup"><span data-stu-id="6262a-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6262a-151">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6262a-151">Additional Resources</span></span>

[<span data-ttu-id="6262a-152">Bson-Beispiel für Web-API</span><span class="sxs-lookup"><span data-stu-id="6262a-152">Web API BSON Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[<span data-ttu-id="6262a-153">Medienformatierer</span><span class="sxs-lookup"><span data-stu-id="6262a-153">Media Formatters</span></span>](media-formatters.md)
