---
uid: web-api/overview/advanced/http-cookies
title: HTTP-Cookies in der ASP.NET Web-API – ASP.NET 4.x
author: MikeWasson
description: Beschreibt, wie zum Senden und Empfangen von HTTP-Cookies in Web-API für ASP.NET 4.x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65126238"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="94e3b-103">HTTP-Cookies in der ASP.NET-Web-API</span><span class="sxs-lookup"><span data-stu-id="94e3b-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="94e3b-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="94e3b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="94e3b-105">In diesem Thema wird beschrieben, wie zum Senden und Empfangen von HTTP-Cookies in Web-API wird.</span><span class="sxs-lookup"><span data-stu-id="94e3b-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="94e3b-106">Hintergrundinformationen zu HTTP-Cookies</span><span class="sxs-lookup"><span data-stu-id="94e3b-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="94e3b-107">Dieser Abschnitt enthält eine kurze Übersicht darüber, wie Cookies auf HTTP-Ebene implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="94e3b-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="94e3b-108">Weitere Informationen finden Sie in [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="94e3b-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="94e3b-109">Ein Cookie ist ein Teil der Daten, die ein Server in der HTTP-Antwort sendet.</span><span class="sxs-lookup"><span data-stu-id="94e3b-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="94e3b-110">Der Client ist (optional) das Cookie gespeichert und bei nachfolgenden Anforderungen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="94e3b-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="94e3b-111">Dadurch wird dem Client und Server aus möglich.</span><span class="sxs-lookup"><span data-stu-id="94e3b-111">This allows the client and server to share state.</span></span> <span data-ttu-id="94e3b-112">Um ein Cookie festlegen, enthält der Server einen Set-Cookie-Header in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="94e3b-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="94e3b-113">Das Format eines Cookies ist ein Name / Wert-Paar, mit optionalen Attributen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="94e3b-114">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="94e3b-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="94e3b-115">Hier ist ein Beispiel mit Attributen:</span><span class="sxs-lookup"><span data-stu-id="94e3b-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="94e3b-116">Um ein Cookie an den Server zurückzugeben, fügt der Client einen Cookie-Header in höhere Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="94e3b-117">Eine HTTP-Antwort kann mehrere Set-Cookie-Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="94e3b-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="94e3b-118">Der Client gibt mehrere Cookies, die mit einem einzelnen Cookieheader zurück.</span><span class="sxs-lookup"><span data-stu-id="94e3b-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="94e3b-119">Umfang und Dauer eines Cookies werden durch die folgenden Attribute im Set-Cookie-Header gesteuert:</span><span class="sxs-lookup"><span data-stu-id="94e3b-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="94e3b-120">**Domain**: Teilt dem Client die Domäne des Cookies erhalten soll.</span><span class="sxs-lookup"><span data-stu-id="94e3b-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="94e3b-121">Z. B. wenn die Domäne "Beispiel.com" ist, gibt der Client jede Unterdomäne von "example.com" das Cookie zurück.</span><span class="sxs-lookup"><span data-stu-id="94e3b-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="94e3b-122">Wenn nicht angegeben, ist die Domäne dem Ursprungsserver.</span><span class="sxs-lookup"><span data-stu-id="94e3b-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="94e3b-123">**Pfad**: Schränkt das Cookie für den angegebenen Pfad innerhalb der Domäne an.</span><span class="sxs-lookup"><span data-stu-id="94e3b-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="94e3b-124">Wenn nicht angegeben, wird der Pfad des Anforderungs-URI verwendet.</span><span class="sxs-lookup"><span data-stu-id="94e3b-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="94e3b-125">**Läuft ab**: Legt ein Ablaufdatum für das Cookie fest.</span><span class="sxs-lookup"><span data-stu-id="94e3b-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="94e3b-126">Der Client löscht das Cookie an, nach dessen Ablauf.</span><span class="sxs-lookup"><span data-stu-id="94e3b-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="94e3b-127">**Max-Age-**: Legt das maximale Alter für das Cookie fest.</span><span class="sxs-lookup"><span data-stu-id="94e3b-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="94e3b-128">Der Client löscht den Cookie, wenn sie das maximale Alter erreicht.</span><span class="sxs-lookup"><span data-stu-id="94e3b-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="94e3b-129">Wenn beide `Expires` und `Max-Age` festgelegt sind, `Max-Age` hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="94e3b-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="94e3b-130">Wenn keines von beiden festgelegt ist, löscht der Client das Cookie an, wenn die aktuelle Sitzung beendet.</span><span class="sxs-lookup"><span data-stu-id="94e3b-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="94e3b-131">(Die genaue Bedeutung von "Session" richtet sich nach der Benutzer-Agent.)</span><span class="sxs-lookup"><span data-stu-id="94e3b-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="94e3b-132">Bedenken Sie jedoch, dass Clients möglicherweise Cookies ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="94e3b-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="94e3b-133">Beispielsweise kann ein Benutzer die Cookies aus Datenschutzgründen deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="94e3b-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="94e3b-134">Clients können Cookies löschen, bevor sie ablaufen, oder die Anzahl der Cookies gespeichert.</span><span class="sxs-lookup"><span data-stu-id="94e3b-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="94e3b-135">Aus Gründen des Datenschutzes ablehnen Clients häufig "Fremdanbieter" Cookies, in dem die Domäne nicht mit den Ursprungsserver übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="94e3b-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="94e3b-136">Kurz gesagt: der Server nicht empfehlenswert Rückkehr die Cookies, die festgelegt.</span><span class="sxs-lookup"><span data-stu-id="94e3b-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="94e3b-137">Cookies in Web-API</span><span class="sxs-lookup"><span data-stu-id="94e3b-137">Cookies in Web API</span></span>

<span data-ttu-id="94e3b-138">Um eine HTTP-Antwort einen Cookie hinzugefügt haben, erstellen Sie eine **CookieHeaderValue** Instanz, die das Cookie darstellt.</span><span class="sxs-lookup"><span data-stu-id="94e3b-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="94e3b-139">Rufen Sie dann die **AddCookies** Erweiterungsmethode, die in definierten die **System.Net.Http. HttpResponseHeadersExtensions** -Klasse, um das Cookie hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="94e3b-140">Der folgende Code fügt z. B. einen Cookie in eine Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="94e3b-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="94e3b-141">Beachten Sie, dass **AddCookies** akzeptiert ein Array von **CookieHeaderValue** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="94e3b-142">Um die Cookies aus eine Anforderung des Clients zu extrahieren, rufen die **GetCookies** Methode:</span><span class="sxs-lookup"><span data-stu-id="94e3b-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="94e3b-143">Ein **CookieHeaderValue** enthält eine Auflistung von **CookieState** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="94e3b-144">Jede **CookieState** ein Cookie darstellt.</span><span class="sxs-lookup"><span data-stu-id="94e3b-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="94e3b-145">Verwenden Sie die Indexermethode zum Abrufen einer **CookieState** anhand des Namens, wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="94e3b-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="94e3b-146">Strukturierte Cookiedaten</span><span class="sxs-lookup"><span data-stu-id="94e3b-146">Structured Cookie Data</span></span>

<span data-ttu-id="94e3b-147">Viele Browser einschränken, wie viele Cookies, die sie speichert&#8212;sowohl die Gesamtanzahl und die Anzahl der pro Domäne.</span><span class="sxs-lookup"><span data-stu-id="94e3b-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="94e3b-148">Aus diesem Grund kann es nützlich, um strukturierte Daten in ein einzelnes Cookie, statt mehrere Cookies gespeichert sein.</span><span class="sxs-lookup"><span data-stu-id="94e3b-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="94e3b-149">RFC 6265 definiert nicht die Struktur der Cookiedaten.</span><span class="sxs-lookup"><span data-stu-id="94e3b-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="94e3b-150">Mithilfe der **CookieHeaderValue** -Klasse, können Sie eine Liste von Name / Wert-Paare für die Cookiedaten übergeben.</span><span class="sxs-lookup"><span data-stu-id="94e3b-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="94e3b-151">Diese Name / Wert-Paare werden als URL-codierte Form der Daten in der Set-Cookie-Header codiert:</span><span class="sxs-lookup"><span data-stu-id="94e3b-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="94e3b-152">Der obige Code führt die folgenden Set-Cookie-Header:</span><span class="sxs-lookup"><span data-stu-id="94e3b-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="94e3b-153">Die **CookieState** Klasse enthält eine Indexermethode, um die untergeordneten Werte von einem Cookie in der Anforderungsnachricht zu lesen:</span><span class="sxs-lookup"><span data-stu-id="94e3b-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="94e3b-154">Beispiel: Festlegen und Abrufen von Cookies in einen Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="94e3b-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="94e3b-155">In den vorherigen Beispielen wurde gezeigt, wie Cookies aus einem Web-API-Controller verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="94e3b-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="94e3b-156">Eine weitere Möglichkeit ist die Verwendung [message-Handler](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="94e3b-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="94e3b-157">Meldungshandler werden weiter oben in der Pipeline als Controller aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="94e3b-158">Ein Meldungshandler kann Cookies aus der Anforderung zu lesen, bevor die Anforderung den Controller erreichen oder Cookies an die Antwort hinzufügen, nachdem der Controller die Antwort generiert.</span><span class="sxs-lookup"><span data-stu-id="94e3b-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="94e3b-159">Der folgende Code zeigt einen Meldungshandler für Sitzungs-IDs erstellen.</span><span class="sxs-lookup"><span data-stu-id="94e3b-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="94e3b-160">Die Sitzungs-ID wird in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="94e3b-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="94e3b-161">Der Ereignishandler überprüft die Anforderung für das Sitzungscookie.</span><span class="sxs-lookup"><span data-stu-id="94e3b-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="94e3b-162">Wenn das Cookie in die Anforderung nicht enthalten ist, wird der Handler eine neuen Sitzungs-ID generiert.</span><span class="sxs-lookup"><span data-stu-id="94e3b-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="94e3b-163">In beiden Fällen speichert der Handler für die Sitzungs-ID in der **HttpRequestMessage.Properties** Eigenschaftenbehälter.</span><span class="sxs-lookup"><span data-stu-id="94e3b-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="94e3b-164">Außerdem werden die HTTP-Antwort das Sitzungscookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="94e3b-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="94e3b-165">Diese Implementierung überprüft nicht, dass die Sitzungs-ID vom Client tatsächlich vom Server ausgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="94e3b-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="94e3b-166">Verwenden Sie nicht als eine Form der Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="94e3b-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="94e3b-167">Zum Zeitpunkt des im Beispiel wird HTTP-Cookie Management erläutert.</span><span class="sxs-lookup"><span data-stu-id="94e3b-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="94e3b-168">Ein Controller erhalten die Sitzungs-ID aus der **HttpRequestMessage.Properties** Eigenschaftenbehälter.</span><span class="sxs-lookup"><span data-stu-id="94e3b-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
