---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Inhaltsaushandlung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt, wie ASP.net-Web-API die http-Inhaltsaushandlung für ASP.NET 4. x implementiert.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504657"
---
# <a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="ad196-103">Inhalts Aushandlung in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="ad196-103">Content Negotiation in ASP.NET Web API</span></span>

<span data-ttu-id="ad196-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ad196-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ad196-105">In diesem Artikel wird beschrieben, wie ASP.net-Web-API die Inhaltsaushandlung für ASP.NET 4. x implementiert.</span><span class="sxs-lookup"><span data-stu-id="ad196-105">This article describes how ASP.NET Web API implements content negotiation for ASP.NET 4.x.</span></span>

<span data-ttu-id="ad196-106">Die HTTP-Spezifikation (RFC 2616) definiert die Inhaltsaushandlung als "der Prozess der Auswahl der optimalen Darstellung für eine bestimmte Antwort, wenn mehrere Darstellungen verfügbar sind".</span><span class="sxs-lookup"><span data-stu-id="ad196-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="ad196-107">Der primäre Mechanismus für die Inhaltsaushandlung in http sind folgende Anforderungs Header:</span><span class="sxs-lookup"><span data-stu-id="ad196-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="ad196-108">**Akzeptieren:** Welche Medientypen für die Antwort zulässig sind, z. b. "application/json", "Application/XML" oder ein benutzerdefinierter Medientyp, z. b. &quot;application/vnd. example + XML&quot;</span><span class="sxs-lookup"><span data-stu-id="ad196-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="ad196-109">**Accept-Charset:** Die zulässigen Zeichensätze, wie z. b. UTF-8 oder ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="ad196-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="ad196-110">**Accept-Encoding:** Welche Inhalts Codierungen zulässig sind, z. b. gzip.</span><span class="sxs-lookup"><span data-stu-id="ad196-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="ad196-111">**Accept-Language:** Die bevorzugte natürliche Sprache, wie z. b. "en-US".</span><span class="sxs-lookup"><span data-stu-id="ad196-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="ad196-112">Der Server kann auch andere Teile der HTTP-Anforderung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="ad196-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="ad196-113">Wenn die Anforderung z. b. einen X-angeforderten-with-Header enthält, der eine AJAX-Anforderung angibt, wird der Server möglicherweise standardmäßig JSON verwendet, wenn kein Accept-Header vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="ad196-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="ad196-114">In diesem Artikel wird erläutert, wie die Web-API die Accept-und Accept-Charset-Header verwendet.</span><span class="sxs-lookup"><span data-stu-id="ad196-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="ad196-115">(Zu diesem Zeitpunkt gibt es keine integrierte Unterstützung für Accept-Encoding oder Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="ad196-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="ad196-116">Serialisierung</span><span class="sxs-lookup"><span data-stu-id="ad196-116">Serialization</span></span>

<span data-ttu-id="ad196-117">Wenn ein Web-API-Controller eine Ressource als CLR-Typ zurückgibt, serialisiert die Pipeline den Rückgabewert und schreibt ihn in den HTTP-Antworttext.</span><span class="sxs-lookup"><span data-stu-id="ad196-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="ad196-118">Sehen Sie sich beispielsweise die folgende Controller Aktion an:</span><span class="sxs-lookup"><span data-stu-id="ad196-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="ad196-119">Ein Client kann diese HTTP-Anforderung senden:</span><span class="sxs-lookup"><span data-stu-id="ad196-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="ad196-120">Als Antwort sendet der Server möglicherweise Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ad196-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="ad196-121">In diesem Beispiel hat der Client entweder JSON, JavaScript oder "Anything" (\*/\*) angefordert.</span><span class="sxs-lookup"><span data-stu-id="ad196-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="ad196-122">Der Server hat mit einer JSON-Darstellung des `Product` Objekts geantwortet.</span><span class="sxs-lookup"><span data-stu-id="ad196-122">The server responded with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="ad196-123">Beachten Sie, dass der Content-Type-Header in der Antwort auf &quot;Application/JSON-&quot;festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="ad196-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="ad196-124">Ein Controller kann auch ein **HttpResponseMessage** -Objekt zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="ad196-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="ad196-125">Um ein CLR-Objekt für den Antworttext anzugeben, müssen Sie die Erweiterungsmethode " **samateresponse** " aufrufen:</span><span class="sxs-lookup"><span data-stu-id="ad196-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="ad196-126">Diese Option ermöglicht Ihnen mehr Kontrolle über die Details der Antwort.</span><span class="sxs-lookup"><span data-stu-id="ad196-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="ad196-127">Sie können den Statuscode festlegen, HTTP-Header hinzufügen usw.</span><span class="sxs-lookup"><span data-stu-id="ad196-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="ad196-128">Das Objekt, das die Ressource serialisiert, wird als *medienformatierer*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="ad196-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="ad196-129">Medien Formatierer werden von der **mediatypeer Formatter** -Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="ad196-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="ad196-130">Die Web-API stellt medienformatierer für XML und JSON bereit, und Sie können benutzerdefinierte Formatierer erstellen, um andere Medientypen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ad196-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="ad196-131">Weitere Informationen zum Schreiben eines benutzerdefinierten Formatierers finden Sie unter [medienformatierer](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="ad196-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="ad196-132">So funktioniert die Inhalts Aushandlung</span><span class="sxs-lookup"><span data-stu-id="ad196-132">How Content Negotiation Works</span></span>

<span data-ttu-id="ad196-133">Zuerst Ruft die Pipeline den **icontentvermittler** -Dienst aus dem **httpconfiguration** -Objekt ab.</span><span class="sxs-lookup"><span data-stu-id="ad196-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="ad196-134">Außerdem wird die Liste der Medien Formatierer aus der **httpconfiguration. Formatierungs Programme** -Auflistung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="ad196-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="ad196-135">Als nächstes Ruft die Pipeline **icontentverhandlers. aushandeln**auf und übergibt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ad196-135">Next, the pipeline calls **IContentNegotiator.Negotiate**, passing in:</span></span>

- <span data-ttu-id="ad196-136">Der Typ des zu serialisierenden Objekts.</span><span class="sxs-lookup"><span data-stu-id="ad196-136">The type of object to serialize</span></span>
- <span data-ttu-id="ad196-137">Die Auflistung der Medien Formatierer.</span><span class="sxs-lookup"><span data-stu-id="ad196-137">The collection of media formatters</span></span>
- <span data-ttu-id="ad196-138">Die HTTP-Anforderung</span><span class="sxs-lookup"><span data-stu-id="ad196-138">The HTTP request</span></span>

<span data-ttu-id="ad196-139">Von der **Aushandlungs** Methode werden zwei Informationen zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="ad196-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="ad196-140">Zu verwendende Formatierer</span><span class="sxs-lookup"><span data-stu-id="ad196-140">Which formatter to use</span></span>
- <span data-ttu-id="ad196-141">Der Medientyp für die Antwort.</span><span class="sxs-lookup"><span data-stu-id="ad196-141">The media type for the response</span></span>

<span data-ttu-id="ad196-142">Wenn kein Formatierer gefunden wird, gibt die **Aushandlungs** Methode **null**zurück, und der Client empfängt den HTTP-Fehler 406 (nicht akzeptabel).</span><span class="sxs-lookup"><span data-stu-id="ad196-142">If no formatter is found, the **Negotiate** method returns **null**, and the client receives HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="ad196-143">Der folgende Code zeigt, wie ein Controller die Inhaltsaushandlung direkt aufrufen kann:</span><span class="sxs-lookup"><span data-stu-id="ad196-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="ad196-144">Dieser Code entspricht dem automatischen Funktionsumfang der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="ad196-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="ad196-145">Standard inhaltsverhandler</span><span class="sxs-lookup"><span data-stu-id="ad196-145">Default Content Negotiator</span></span>

<span data-ttu-id="ad196-146">Die **defaultcontentverhandlerklasse** stellt die Standard Implementierung von **icontentvermittler**bereit.</span><span class="sxs-lookup"><span data-stu-id="ad196-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="ad196-147">Es werden verschiedene Kriterien verwendet, um einen Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="ad196-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="ad196-148">Zuerst muss der Formatierer den Typ serialisieren können.</span><span class="sxs-lookup"><span data-stu-id="ad196-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="ad196-149">Dies wird durch den Aufruf von **mediatypformatter. CanWrite tetype**überprüft.</span><span class="sxs-lookup"><span data-stu-id="ad196-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="ad196-150">Als Nächstes untersucht der inhaltsverhandler jeden Formatierer und wertet aus, wie gut er mit der HTTP-Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="ad196-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="ad196-151">Zum Auswerten der Übereinstimmung prüft der inhaltsverhandler zwei Dinge im Formatierer:</span><span class="sxs-lookup"><span data-stu-id="ad196-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="ad196-152">Die **supportedmediatypes** -Auflistung, die eine Liste der unterstützten Medientypen enthält.</span><span class="sxs-lookup"><span data-stu-id="ad196-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="ad196-153">Der inhaltsverhander versucht, diese Liste mit dem Accept-Header der Anforderung abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="ad196-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="ad196-154">Beachten Sie, dass der Accept-Header Bereiche enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="ad196-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="ad196-155">"Text/Plain" ist z. b. eine Entsprechung für Text/\* oder \*/\*.</span><span class="sxs-lookup"><span data-stu-id="ad196-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="ad196-156">Die **mediatypeer Mappings** -Auflistung, die eine Liste von **mediatypeer Mapping** -Objekten enthält.</span><span class="sxs-lookup"><span data-stu-id="ad196-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="ad196-157">Die **mediatypeer Mapping** -Klasse stellt eine generische Methode zum Zuordnen von HTTP-Anforderungen mit Medientypen bereit.</span><span class="sxs-lookup"><span data-stu-id="ad196-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="ad196-158">Beispielsweise kann ein benutzerdefinierter HTTP-Header einem bestimmten Medientyp zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="ad196-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="ad196-159">Wenn mehrere Übereinstimmungen vorhanden sind, gewinnt die Übereinstimmung mit dem höchsten Qualitätsfaktor.</span><span class="sxs-lookup"><span data-stu-id="ad196-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="ad196-160">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ad196-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="ad196-161">In diesem Beispiel hat Application/JSON einen impliziten Qualitätsfaktor von 1,0, daher wird er gegenüber Application/XML bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="ad196-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="ad196-162">Wenn keine Übereinstimmungen gefunden werden, versucht der inhaltsverhandler, ggf. mit dem Medientyp des Anforderungs Texts abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="ad196-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="ad196-163">Wenn die Anforderung beispielsweise JSON-Daten enthält, sucht der inhaltsverhandler nach einem JSON-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="ad196-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="ad196-164">Wenn noch keine Übereinstimmungen vorhanden sind, wählt der Inhalts Vermittler einfach das erste formatierungprogramm aus, das den Typ serialisieren kann.</span><span class="sxs-lookup"><span data-stu-id="ad196-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="ad196-165">Auswählen einer Zeichencodierung</span><span class="sxs-lookup"><span data-stu-id="ad196-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="ad196-166">Nachdem ein Formatierungs Programm ausgewählt wurde, wählt der inhaltsverhandler die beste Zeichencodierung aus, indem er die **supportedencodings** -Eigenschaft des Formatierungs Programms prüft und ihn mit dem Accept-Charset-Header in der Anforderung abruft (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="ad196-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
