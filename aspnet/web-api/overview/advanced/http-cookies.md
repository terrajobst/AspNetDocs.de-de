---
uid: web-api/overview/advanced/http-cookies
title: HTTP-Cookies in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt das Senden und empfangen von HTTP-Cookies in der Web-API für ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449313"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="ad58b-103">HTTP-Cookies in der ASP.NET-Web-API</span><span class="sxs-lookup"><span data-stu-id="ad58b-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="ad58b-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ad58b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ad58b-105">In diesem Thema wird beschrieben, wie http-Cookies in der Web-API gesendet und empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="ad58b-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="ad58b-106">Hintergrundinformationen zu http-Cookies</span><span class="sxs-lookup"><span data-stu-id="ad58b-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="ad58b-107">In diesem Abschnitt erhalten Sie einen kurzen Überblick darüber, wie Cookies auf der HTTP-Ebene implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="ad58b-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="ad58b-108">Weitere Informationen finden Sie in [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="ad58b-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="ad58b-109">Ein Cookie ist ein Datenelement, das von einem Server in der HTTP-Antwort gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="ad58b-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="ad58b-110">Der Client (optional) speichert das Cookie und gibt es bei nachfolgenden Anforderungen zurück.</span><span class="sxs-lookup"><span data-stu-id="ad58b-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="ad58b-111">Dies ermöglicht dem Client und dem Server das Freigeben des Zustands.</span><span class="sxs-lookup"><span data-stu-id="ad58b-111">This allows the client and server to share state.</span></span> <span data-ttu-id="ad58b-112">Zum Festlegen eines Cookies enthält der Server einen Set-Cookie-Header in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="ad58b-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="ad58b-113">Das Format eines Cookies ist ein Name-Wert-Paar mit optionalen Attributen.</span><span class="sxs-lookup"><span data-stu-id="ad58b-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="ad58b-114">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ad58b-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="ad58b-115">Im folgenden finden Sie ein Beispiel mit Attributen:</span><span class="sxs-lookup"><span data-stu-id="ad58b-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="ad58b-116">Zum Zurückgeben eines Cookies an den Server schließt der Client in späteren Anforderungen einen Cookie-Header ein.</span><span class="sxs-lookup"><span data-stu-id="ad58b-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="ad58b-117">Eine HTTP-Antwort kann mehrere Set-Cookie-Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="ad58b-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="ad58b-118">Der Client gibt mehrere Cookies mithilfe eines einzelnen Cookieheaders zurück.</span><span class="sxs-lookup"><span data-stu-id="ad58b-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="ad58b-119">Der Gültigkeitsbereich und die Dauer eines Cookies werden durch folgende Attribute im Set-Cookie-Header gesteuert:</span><span class="sxs-lookup"><span data-stu-id="ad58b-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="ad58b-120">**Domäne**: weist den Client an, welche Domäne das Cookie erhalten soll.</span><span class="sxs-lookup"><span data-stu-id="ad58b-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="ad58b-121">Wenn die Domäne z. b. "example.com" ist, gibt der Client das Cookie an jede Unterdomäne von example.com zurück.</span><span class="sxs-lookup"><span data-stu-id="ad58b-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="ad58b-122">Wenn nicht angegeben, ist die Domäne der Ursprungsserver.</span><span class="sxs-lookup"><span data-stu-id="ad58b-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="ad58b-123">**Path**: schränkt das Cookie auf den angegebenen Pfad innerhalb der Domäne ein.</span><span class="sxs-lookup"><span data-stu-id="ad58b-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="ad58b-124">Wenn nicht angegeben, wird der Pfad des Anforderungs-URI verwendet.</span><span class="sxs-lookup"><span data-stu-id="ad58b-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="ad58b-125">**Läuft**ab: legt ein Ablaufdatum für das Cookie fest.</span><span class="sxs-lookup"><span data-stu-id="ad58b-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="ad58b-126">Der Client löscht das Cookie, wenn es abläuft.</span><span class="sxs-lookup"><span data-stu-id="ad58b-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="ad58b-127">**Max-age**: legt das maximale Alter für das Cookie fest.</span><span class="sxs-lookup"><span data-stu-id="ad58b-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="ad58b-128">Der Client löscht das Cookie, wenn es das maximale Alter erreicht.</span><span class="sxs-lookup"><span data-stu-id="ad58b-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="ad58b-129">Wenn sowohl `Expires` als auch `Max-Age` festgelegt sind, hat `Max-Age` Vorrang.</span><span class="sxs-lookup"><span data-stu-id="ad58b-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="ad58b-130">Wenn keines der beiden festgelegt ist, löscht der Client das Cookie, wenn die aktuelle Sitzung beendet wird.</span><span class="sxs-lookup"><span data-stu-id="ad58b-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="ad58b-131">(Die genaue Bedeutung von "Session" wird vom Benutzer-Agent bestimmt.)</span><span class="sxs-lookup"><span data-stu-id="ad58b-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="ad58b-132">Beachten Sie jedoch, dass Clients Cookies ignorieren können.</span><span class="sxs-lookup"><span data-stu-id="ad58b-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="ad58b-133">Beispielsweise kann ein Benutzer Cookies aus Datenschutzgründen deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="ad58b-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="ad58b-134">Clients können Cookies löschen, bevor Sie ablaufen, oder die Anzahl der gespeicherten Cookies einschränken.</span><span class="sxs-lookup"><span data-stu-id="ad58b-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="ad58b-135">Aus Datenschutzgründen werden von Clients häufig Cookies von Drittanbietern abgelehnt, bei denen die Domäne nicht dem Ursprungsserver entspricht.</span><span class="sxs-lookup"><span data-stu-id="ad58b-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="ad58b-136">Kurz gesagt, sollte der Server nicht darauf zurückgreifen, dass die von ihm festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ad58b-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="ad58b-137">Cookies in der Web-API</span><span class="sxs-lookup"><span data-stu-id="ad58b-137">Cookies in Web API</span></span>

<span data-ttu-id="ad58b-138">Zum Hinzufügen eines Cookies zu einer HTTP-Antwort erstellen Sie eine **cookieheadervalue** -Instanz, die das Cookie darstellt.</span><span class="sxs-lookup"><span data-stu-id="ad58b-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="ad58b-139">Dann rufe Sie die **addcookies** -Erweiterungsmethode auf, die im **System .net. http definiert ist. Httpresponsheadersextensions** -Klasse, um das Cookie hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ad58b-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="ad58b-140">Der folgende Code fügt z. b. ein Cookie innerhalb einer Controller Aktion hinzu:</span><span class="sxs-lookup"><span data-stu-id="ad58b-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="ad58b-141">Beachten Sie, dass **addcookies** ein Array von **cookieheadervalue** -Instanzen annimmt.</span><span class="sxs-lookup"><span data-stu-id="ad58b-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="ad58b-142">Um die Cookies aus einer Client Anforderung zu extrahieren, müssen Sie die **GetCookies** -Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="ad58b-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="ad58b-143">Ein **cookieheadervalue** enthält eine Sammlung von **cookiestate** -Instanzen.</span><span class="sxs-lookup"><span data-stu-id="ad58b-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="ad58b-144">Jede **cookiestate** stellt ein Cookie dar.</span><span class="sxs-lookup"><span data-stu-id="ad58b-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="ad58b-145">Verwenden Sie die Indexer-Methode, um einen **cookiestate** nach dem Namen zu erhalten, wie hier gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ad58b-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="ad58b-146">Strukturierte Cookie-Daten</span><span class="sxs-lookup"><span data-stu-id="ad58b-146">Structured Cookie Data</span></span>

<span data-ttu-id="ad58b-147">Viele Browser beschränken, wie viele Cookies sowohl die&#8212;Gesamtzahl als auch die Anzahl pro Domäne speichern werden.</span><span class="sxs-lookup"><span data-stu-id="ad58b-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="ad58b-148">Daher kann es hilfreich sein, strukturierte Daten in einem einzigen Cookie zu platzieren, anstatt mehrere Cookies festzulegen.</span><span class="sxs-lookup"><span data-stu-id="ad58b-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="ad58b-149">RFC 6265 definiert nicht die Struktur von Cookie-Daten.</span><span class="sxs-lookup"><span data-stu-id="ad58b-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="ad58b-150">Mithilfe der **cookieheadervalue** -Klasse können Sie eine Liste von Name-Wert-Paaren für die Cookiedaten übergeben.</span><span class="sxs-lookup"><span data-stu-id="ad58b-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="ad58b-151">Diese Name-Wert-Paare werden als URL-codierte Formulardaten im Set-Cookie-Header codiert:</span><span class="sxs-lookup"><span data-stu-id="ad58b-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="ad58b-152">Der vorherige Code erzeugt den folgenden Set-Cookie-Header:</span><span class="sxs-lookup"><span data-stu-id="ad58b-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="ad58b-153">Die **cookiestate** -Klasse stellt eine Indexer-Methode bereit, um die untergeordneten Werte aus einem Cookie in der Anforderungs Nachricht zu lesen:</span><span class="sxs-lookup"><span data-stu-id="ad58b-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="ad58b-154">Beispiel: festlegen und Abrufen von Cookies in einem Nachrichten Handler</span><span class="sxs-lookup"><span data-stu-id="ad58b-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="ad58b-155">In den vorherigen Beispielen wurde gezeigt, wie Cookies in einem Web-API-Controller verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ad58b-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="ad58b-156">Eine andere Möglichkeit ist die Verwendung von [Nachrichten Handlern](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ad58b-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="ad58b-157">Meldungs Handler werden zuvor in der Pipeline aufgerufen als Controller.</span><span class="sxs-lookup"><span data-stu-id="ad58b-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="ad58b-158">Ein Nachrichten Handler kann Cookies aus der Anforderung lesen, bevor die Anforderung den Controller erreicht, oder der Antwort Cookies hinzufügen, nachdem der Controller die Antwort generiert hat.</span><span class="sxs-lookup"><span data-stu-id="ad58b-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="ad58b-159">Der folgende Code zeigt einen Nachrichten Handler zum Erstellen von Sitzungs-IDs.</span><span class="sxs-lookup"><span data-stu-id="ad58b-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="ad58b-160">Die Sitzungs-ID wird in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ad58b-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="ad58b-161">Der Handler überprüft die Anforderung für das Sitzungs Cookie.</span><span class="sxs-lookup"><span data-stu-id="ad58b-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="ad58b-162">Wenn das Cookie nicht in der Anforderung enthalten ist, generiert der Handler eine neue Sitzungs-ID.</span><span class="sxs-lookup"><span data-stu-id="ad58b-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="ad58b-163">In beiden Fällen speichert der Handler die Sitzungs-ID im **httprequestmessage. Properties-Eigenschaften** Behälter.</span><span class="sxs-lookup"><span data-stu-id="ad58b-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="ad58b-164">Außerdem wird der HTTP-Antwort das Sitzungs Cookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ad58b-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="ad58b-165">Diese Implementierung überprüft nicht, ob die Sitzungs-ID des Clients tatsächlich vom Server ausgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="ad58b-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="ad58b-166">Verwenden Sie es nicht als Form der Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="ad58b-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="ad58b-167">Der Punkt des Beispiels besteht darin, die HTTP-Cookie-Verwaltung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ad58b-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="ad58b-168">Ein Controller kann die Sitzungs-ID aus dem **httprequestmessage. Properties-Eigenschaften** Behälter erhalten.</span><span class="sxs-lookup"><span data-stu-id="ad58b-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
