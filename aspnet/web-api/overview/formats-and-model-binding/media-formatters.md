---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Medien Formatierer in ASP.net-Web-API 2-ASP.NET 4. x
author: MikeWasson
description: Zeigt, wie zusätzliche Medienformate in ASP.net-Web-API für ASP.NET 4. x unterstützt werden.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448941"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="eec2a-103">Medien Formatierer in ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="eec2a-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="eec2a-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="eec2a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="eec2a-105">In diesem Tutorial wird gezeigt, wie zusätzliche Medienformate in ASP.net-Web-API unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="eec2a-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="eec2a-106">Internet Medientypen</span><span class="sxs-lookup"><span data-stu-id="eec2a-106">Internet Media Types</span></span>

<span data-ttu-id="eec2a-107">Ein Medientyp, auch als MIME-Typ bezeichnet, identifiziert das Format der Daten.</span><span class="sxs-lookup"><span data-stu-id="eec2a-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="eec2a-108">In http beschreiben Medientypen das Format des Nachrichten Texts.</span><span class="sxs-lookup"><span data-stu-id="eec2a-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="eec2a-109">Ein Medientyp besteht aus zwei Zeichen folgen, einem Typ und einem Untertyp.</span><span class="sxs-lookup"><span data-stu-id="eec2a-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="eec2a-110">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eec2a-110">For example:</span></span>

- <span data-ttu-id="eec2a-111">text/html</span><span class="sxs-lookup"><span data-stu-id="eec2a-111">text/html</span></span>
- <span data-ttu-id="eec2a-112">image/png</span><span class="sxs-lookup"><span data-stu-id="eec2a-112">image/png</span></span>
- <span data-ttu-id="eec2a-113">Anwendung/json</span><span class="sxs-lookup"><span data-stu-id="eec2a-113">application/json</span></span>

<span data-ttu-id="eec2a-114">Wenn eine HTTP-Nachricht einen Entitäts Text enthält, gibt der Content-Type-Header das Format des Nachrichten Texts an.</span><span class="sxs-lookup"><span data-stu-id="eec2a-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="eec2a-115">Dadurch wird dem Empfänger mitgeteilt, wie der Inhalt des Nachrichten Texts analysiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="eec2a-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="eec2a-116">Wenn eine HTTP-Antwort beispielsweise ein PNG-Bild enthält, weist die Antwort möglicherweise die folgenden Header auf.</span><span class="sxs-lookup"><span data-stu-id="eec2a-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="eec2a-117">Wenn der Client eine Anforderungs Nachricht sendet, kann er einen Accept-Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="eec2a-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="eec2a-118">Der Accept-Header teilt dem Server mit, welche Medientypen der Client vom Server anfordert.</span><span class="sxs-lookup"><span data-stu-id="eec2a-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="eec2a-119">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="eec2a-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="eec2a-120">Dieser Header weist den Server an, dass der Client HTML, XHTML oder XML wünscht.</span><span class="sxs-lookup"><span data-stu-id="eec2a-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="eec2a-121">Der Medientyp bestimmt, wie die Web-API den HTTP-Nachrichtentext serialisiert und deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="eec2a-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="eec2a-122">Die Web-API verfügt über integrierte Unterstützung für XML-, JSON-, bson-und form-urlencoded-Daten, und Sie können zusätzliche Medientypen unterstützen, indem Sie einen *medienformatierer*schreiben.</span><span class="sxs-lookup"><span data-stu-id="eec2a-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="eec2a-123">Um einen medienformatierer zu erstellen, leiten Sie von einer der folgenden Klassen ab:</span><span class="sxs-lookup"><span data-stu-id="eec2a-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="eec2a-124">[Mediatypeer Formatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="eec2a-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="eec2a-125">Diese Klasse verwendet asynchrone Lese-und Schreib Methoden.</span><span class="sxs-lookup"><span data-stu-id="eec2a-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="eec2a-126">[Bufferedmediatypeer Formatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="eec2a-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="eec2a-127">Diese Klasse wird von **mediatypformatter** abgeleitet, verwendet jedoch synchrone Lese-/Schreibmethoden.</span><span class="sxs-lookup"><span data-stu-id="eec2a-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="eec2a-128">Die Ableitung von **bufferedmediatypformatter** ist einfacher, da es keinen asynchronen Code gibt, aber auch bedeutet, dass der aufrufende Thread während der e/a-Vorgänge blockiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="eec2a-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="eec2a-129">Beispiel: Erstellen eines CSV-Medien Formatierers</span><span class="sxs-lookup"><span data-stu-id="eec2a-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="eec2a-130">Das folgende Beispiel zeigt einen Medientyp Formatierer, mit dem ein Product-Objekt in ein CSV-Format (Comma-Separated Values, Komma getrennte Werte) serialisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="eec2a-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="eec2a-131">In diesem Beispiel wird der Produkttyp verwendet, der im Tutorial [Erstellen einer Web-API definiert ist, die CRUD-Vorgänge unterstützt](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="eec2a-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="eec2a-132">Hier ist die Definition des Product-Objekts:</span><span class="sxs-lookup"><span data-stu-id="eec2a-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="eec2a-133">Um einen CSV-Formatierer zu implementieren, definieren Sie eine Klasse, die von **bufferedmediatypformatter**abgeleitet wird:</span><span class="sxs-lookup"><span data-stu-id="eec2a-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="eec2a-134">Fügen Sie im Konstruktor die vom Formatierer unterstützten Medientypen hinzu.</span><span class="sxs-lookup"><span data-stu-id="eec2a-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="eec2a-135">In diesem Beispiel unterstützt der Formatierer einen einzelnen Medientyp, &quot;Text/CSV&quot;:</span><span class="sxs-lookup"><span data-stu-id="eec2a-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="eec2a-136">Überschreiben Sie die Methode **canschreitetype** , um anzugeben, welche Typen der Formatierer serialisieren kann:</span><span class="sxs-lookup"><span data-stu-id="eec2a-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="eec2a-137">In diesem Beispiel kann der Formatierer einzelne `Product` Objekte und Auflistungen von `Product` Objekten serialisieren.</span><span class="sxs-lookup"><span data-stu-id="eec2a-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="eec2a-138">Überschreiben Sie auf ähnliche Weise die **CanRead Type** -Methode, um anzugeben, welche Typen der Formatierer deserialisieren kann.</span><span class="sxs-lookup"><span data-stu-id="eec2a-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="eec2a-139">In diesem Beispiel unterstützt der Formatierer die Deserialisierung nicht, sodass die Methode einfach **false**zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="eec2a-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="eec2a-140">Überschreiben Sie schließlich die Methode " **Write-to-Stream** ".</span><span class="sxs-lookup"><span data-stu-id="eec2a-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="eec2a-141">Diese Methode serialisiert einen Typ durch Schreiben in einen Stream.</span><span class="sxs-lookup"><span data-stu-id="eec2a-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="eec2a-142">Wenn Ihr Formatierer die Deserialisierung unterstützt, überschreiben Sie auch die Read **FromStream** -Methode.</span><span class="sxs-lookup"><span data-stu-id="eec2a-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="eec2a-143">Hinzufügen eines medienformatierers zur Web-API-Pipeline</span><span class="sxs-lookup"><span data-stu-id="eec2a-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="eec2a-144">Um der Web-API-Pipeline einen Medientyp-Formatierer hinzuzufügen, verwenden Sie die **Formatters** -Eigenschaft für das **httpconfiguration** -Objekt.</span><span class="sxs-lookup"><span data-stu-id="eec2a-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="eec2a-145">Zeichen Codierungen</span><span class="sxs-lookup"><span data-stu-id="eec2a-145">Character Encodings</span></span>

<span data-ttu-id="eec2a-146">Optional kann ein medienformatierer mehrere Zeichen Codierungen unterstützen, z. b. UTF-8 oder ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="eec2a-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="eec2a-147">Fügen Sie im Konstruktor einen oder mehrere [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) -Typen zur **supportedencodings** -Auflistung hinzu.</span><span class="sxs-lookup"><span data-stu-id="eec2a-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="eec2a-148">Legen Sie zuerst die Standard Codierung.</span><span class="sxs-lookup"><span data-stu-id="eec2a-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="eec2a-149">Rufen Sie in den Methoden " **Write to Stream** " und "read **FromStream** " [mediatypformatter. selectcharakteriencoding](https://msdn.microsoft.com/library/hh969054.aspx) auf, um die bevorzugte Zeichencodierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="eec2a-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="eec2a-150">Diese Methode entspricht den Anforderungs Headern mit der Liste der unterstützten Codierungen.</span><span class="sxs-lookup"><span data-stu-id="eec2a-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="eec2a-151">Verwenden Sie die zurückgegebene **Codierung** , wenn Sie aus dem Stream lesen oder schreiben:</span><span class="sxs-lookup"><span data-stu-id="eec2a-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
