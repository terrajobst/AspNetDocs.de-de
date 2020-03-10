---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.net Web Pages (Razor) API-Kurzübersicht | Microsoft-Dokumentation
author: Rick-Anderson
description: Diese Seite enthält eine Liste mit kurzen Beispielen zu den am häufigsten verwendeten Objekten, Eigenschaften und Methoden für die Programmierung von ASP.net Web Pages mit Razor-Syntax.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463581"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="a31cd-103">ASP.net Web Pages (Razor) API-kurz Referenz</span><span class="sxs-lookup"><span data-stu-id="a31cd-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>

<span data-ttu-id="a31cd-104">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a31cd-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a31cd-105">Diese Seite enthält eine Liste mit kurzen Beispielen zu den am häufigsten verwendeten Objekten, Eigenschaften und Methoden für die Programmierung von ASP.net Web Pages mit Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="a31cd-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="a31cd-106">Mit "(v2)" markierte Beschreibungen wurden in ASP.net Web Pages Version 2 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="a31cd-107">Eine API-Referenz Dokumentation finden Sie in der [ASP.net Web Pages Referenz Dokumentation](https://go.microsoft.com/fwlink/?LinkId=208659) auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="a31cd-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="a31cd-108">Software Versionen</span><span class="sxs-lookup"><span data-stu-id="a31cd-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="a31cd-109">ASP.net Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a31cd-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a31cd-110">Dieses Tutorial funktioniert auch mit ASP.net Web Pages 2 und ASP.net Web Pages 1,0 (mit Ausnahme von Features, die als V2 gekennzeichnet sind).</span><span class="sxs-lookup"><span data-stu-id="a31cd-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>

<span data-ttu-id="a31cd-111">Diese Seite enthält Referenzinformationen für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a31cd-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="a31cd-112">Klassen</span><span class="sxs-lookup"><span data-stu-id="a31cd-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="a31cd-113">Daten</span><span class="sxs-lookup"><span data-stu-id="a31cd-113">Data</span></span>](#Data)
- [<span data-ttu-id="a31cd-114">Hospiz</span><span class="sxs-lookup"><span data-stu-id="a31cd-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="a31cd-115">Überprüfung</span><span class="sxs-lookup"><span data-stu-id="a31cd-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="a31cd-116">Klassen</span><span class="sxs-lookup"><span data-stu-id="a31cd-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="a31cd-117">Enthält Daten, die von beliebigen Seiten in der Anwendung gemeinsam verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="a31cd-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="a31cd-118">Sie können die Eigenschaft dynamischer `App` für den Zugriff auf dieselben Daten verwenden, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a31cd-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="a31cd-119">Konvertiert einen Zeichen folgen Wert in einen booleschen Wert (true/false).</span><span class="sxs-lookup"><span data-stu-id="a31cd-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="a31cd-120">Gibt false oder den angegebenen Wert zurück, wenn die Zeichenfolge nicht true/false darstellt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="a31cd-121">Konvertiert einen Zeichen folgen Wert in Datum/Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="a31cd-121">Converts a string value to date/time.</span></span> <span data-ttu-id="a31cd-122">Gibt `DateTime.MinValue` oder den angegebenen Wert zurück, wenn die Zeichenfolge keine Datums-/Uhrzeitangaben darstellt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="a31cd-123">Konvertiert einen Zeichen folgen Wert in einen Dezimalwert.</span><span class="sxs-lookup"><span data-stu-id="a31cd-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="a31cd-124">Gibt 0,0 oder den angegebenen Wert zurück, wenn die Zeichenfolge keinen Dezimalwert darstellt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="a31cd-125">Konvertiert einen Zeichen folgen Wert in einen float-Wert.</span><span class="sxs-lookup"><span data-stu-id="a31cd-125">Converts a string value to a float.</span></span> <span data-ttu-id="a31cd-126">Gibt 0,0 oder den angegebenen Wert zurück, wenn die Zeichenfolge keinen Dezimalwert darstellt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="a31cd-127">Konvertiert einen Zeichen folgen Wert in eine ganze Zahl.</span><span class="sxs-lookup"><span data-stu-id="a31cd-127">Converts a string value to an integer.</span></span> <span data-ttu-id="a31cd-128">Gibt 0 oder den angegebenen Wert zurück, wenn die Zeichenfolge keine Ganzzahl darstellt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="a31cd-129">Erstellt eine Browser kompatible URL aus einem lokalen Dateipfad mit optionalen zusätzlichen Pfad teilen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="a31cd-130">Rendert den *Wert* als HTML-Markup, anstatt ihn als HTML-codierte Ausgabe zu rendern.</span><span class="sxs-lookup"><span data-stu-id="a31cd-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="a31cd-131">Gibt true zurück, wenn der Wert von einer Zeichenfolge in den angegebenen Typ konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a31cd-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="a31cd-132">Gibt "true" zurück, wenn das Objekt oder die Variable keinen Wert besitzt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="a31cd-133">Gibt true zurück, wenn die Anforderung ein Post ist.</span><span class="sxs-lookup"><span data-stu-id="a31cd-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="a31cd-134">(Anfängliche Anforderungen sind in der Regel eine GET-Anforderung.)</span><span class="sxs-lookup"><span data-stu-id="a31cd-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="a31cd-135">Gibt den Pfad einer Layoutseite an, die auf diese Seite angewendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="a31cd-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="a31cd-136">Enthält Daten, die zwischen der Seite, den Layoutseiten und partiellen Seiten in der aktuellen Anforderung freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a31cd-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="a31cd-137">Sie können die Eigenschaft dynamischer `Page` für den Zugriff auf dieselben Daten verwenden, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a31cd-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="a31cd-138">(Layoutseiten) Rendert den Inhalt einer Inhaltsseite, die in keinem benannten Abschnitt vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a31cd-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="a31cd-139">Rendert eine Inhaltsseite unter Verwendung des angegebenen Pfads und optionaler zusätzlicher Daten.</span><span class="sxs-lookup"><span data-stu-id="a31cd-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="a31cd-140">Sie können die Werte der zusätzlichen Parameter von `PageData` nach Position (Beispiel 1) oder Schlüssel (Beispiel 2) erhalten.</span><span class="sxs-lookup"><span data-stu-id="a31cd-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="a31cd-141">(Layoutseiten) Rendert einen Inhalts Abschnitt mit einem Namen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="a31cd-142">Legen Sie *erforderlich* auf false fest, um einen Abschnitt optional zu machen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="a31cd-143">Ruft den Wert eines HTTP-Cookies ab oder legt ihn fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="a31cd-144">Ruft die Dateien ab, die in der aktuellen Anforderung hochgeladen wurden.</span><span class="sxs-lookup"><span data-stu-id="a31cd-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="a31cd-145">Ruft Daten ab, die in einem Formular (als Zeichen folgen) gepostet wurden.</span><span class="sxs-lookup"><span data-stu-id="a31cd-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="a31cd-146">`Request[key]` überprüft sowohl die `Request.Form` als auch die `Request.QueryString` Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="a31cd-147">Ruft Daten ab, die in der URL-Abfrage Zeichenfolge angegeben wurden.</span><span class="sxs-lookup"><span data-stu-id="a31cd-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="a31cd-148">`Request[key]` überprüft sowohl die `Request.Form` als auch die `Request.QueryString` Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="a31cd-149">Deaktiviert die Anforderungs Validierung für ein Formular Element, einen Abfrage Zeichen folgen Wert, ein Cookie oder einen Header Wert selektiv.</span><span class="sxs-lookup"><span data-stu-id="a31cd-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="a31cd-150">Die Anforderungs Validierung ist standardmäßig aktiviert und verhindert, dass Benutzer Markup oder andere potenziell gefährliche Inhalte veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="a31cd-151">Fügt der Antwort einen HTTP-Server Header hinzu.</span><span class="sxs-lookup"><span data-stu-id="a31cd-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="a31cd-152">Speichert die Seiten Ausgabe für einen angegebenen Zeitraum zwischen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="a31cd-153">Legen Sie optional einen *Gleit* enden Wert fest, um das Timeout auf den einzelnen Seiten Zugriffen zurückzusetzen, und *varybyparameams* , um unterschiedliche Versionen der Seite für jede andere Abfrage Zeichenfolge in der Seiten Anforderung</span><span class="sxs-lookup"><span data-stu-id="a31cd-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="a31cd-154">Leitet die Browser Anforderung an einen neuen Speicherort um.</span><span class="sxs-lookup"><span data-stu-id="a31cd-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="a31cd-155">Legt den HTTP-Statuscode fest, der an den Browser gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="a31cd-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="a31cd-156">Schreibt den Inhalt der *Daten* in die Antwort mit einem optionalen MIME-Typ.</span><span class="sxs-lookup"><span data-stu-id="a31cd-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="a31cd-157">Schreibt den Inhalt einer Datei in die Antwort.</span><span class="sxs-lookup"><span data-stu-id="a31cd-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="a31cd-158">(Layoutseiten) Definiert einen Inhalts Abschnitt mit einem Namen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="a31cd-159">Decodiert eine HTML-codierte Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a31cd-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="a31cd-160">Codiert eine Zeichenfolge zum Rendern in HTML-Markup.</span><span class="sxs-lookup"><span data-stu-id="a31cd-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="a31cd-161">Gibt den physischen Pfad des Servers für den angegebenen virtuellen Pfad zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="a31cd-162">Decodiert Text aus einer URL.</span><span class="sxs-lookup"><span data-stu-id="a31cd-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="a31cd-163">Codiert Text, der in eine URL eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="a31cd-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="a31cd-164">Dient zum Abrufen oder Festlegen eines Werts, der vorhanden ist, bis der Benutzer den Browser schließt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="a31cd-165">Zeigt eine Zeichen folgen Darstellung des Objekt Werts an.</span><span class="sxs-lookup"><span data-stu-id="a31cd-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="a31cd-166">Ruft weitere Daten aus der URL ab (z. b. */MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="a31cd-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="a31cd-167">Ändert das Kennwort für den angegebenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a31cd-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="a31cd-168">Bestätigt ein Konto mit dem Konto Bestätigungs Token.</span><span class="sxs-lookup"><span data-stu-id="a31cd-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="a31cd-169">Erstellt ein neues Benutzerkonto mit dem angegebenen Benutzernamen und Kennwort.</span><span class="sxs-lookup"><span data-stu-id="a31cd-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="a31cd-170">Damit ein Bestätigungs Token erforderlich ist, übergeben Sie "true" für "Requirements *confirmationtoken".*</span><span class="sxs-lookup"><span data-stu-id="a31cd-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="a31cd-171">Ruft den ganzzahligen Bezeichner für den aktuell angemeldeten Benutzer ab.</span><span class="sxs-lookup"><span data-stu-id="a31cd-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="a31cd-172">Ruft den Namen für den aktuell angemeldeten Benutzer ab.</span><span class="sxs-lookup"><span data-stu-id="a31cd-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="a31cd-173">Generiert ein Token zum Zurücksetzen des Kennworts, das per e-Mail an einen Benutzer gesendet werden kann, sodass der Benutzer das Kennwort zurücksetzen kann.</span><span class="sxs-lookup"><span data-stu-id="a31cd-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="a31cd-174">Gibt die Benutzer-ID aus dem Benutzernamen zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="a31cd-175">Gibt true zurück, wenn der aktuelle Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="a31cd-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="a31cd-176">Gibt "true" zurück, wenn der Benutzer bestätigt wurde (z. b. durch eine Bestätigungs-e-Mail).</span><span class="sxs-lookup"><span data-stu-id="a31cd-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="a31cd-177">Gibt true zurück, wenn der Name des aktuellen Benutzers mit dem angegebenen Benutzernamen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="a31cd-178">Protokolliert den Benutzer in, indem ein Authentifizierungs Token im Cookie festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a31cd-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="a31cd-179">Protokolliert den Benutzer, indem das Authentifizierungs Token-Cookie entfernt wird.</span><span class="sxs-lookup"><span data-stu-id="a31cd-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="a31cd-180">Wenn der Benutzer nicht authentifiziert ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="a31cd-181">Wenn der aktuelle Benutzer kein Mitglied einer der angegebenen Rollen ist, wird der HTTP-Status auf 401 (nicht autorisiert) festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="a31cd-182">Wenn der aktuelle Benutzer nicht der durch *username*angegebene Benutzer ist, legt den HTTP-Status auf 401 (nicht autorisiert) fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="a31cd-183">Wenn das Token zum Zurücksetzen des Kennworts gültig ist, ändert das Kennwort des Benutzers in das neue Kennwort.</span><span class="sxs-lookup"><span data-stu-id="a31cd-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="a31cd-184">Data</span><span class="sxs-lookup"><span data-stu-id="a31cd-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="a31cd-185">Führt *SQLStatement* (mit optionalen Parametern) aus, z. b. Insert, DELETE oder Update, und gibt die Anzahl der betroffenen Datensätze zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="a31cd-186">Gibt die Identitäts Spalte aus der zuletzt eingefügten Zeile zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="a31cd-187">Öffnet entweder die angegebene Datenbankdatei oder die angegebene Datenbank unter Verwendung einer benannten Verbindungs Zeichenfolge aus der Datei " *Web. config* ".</span><span class="sxs-lookup"><span data-stu-id="a31cd-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="a31cd-188">Öffnet eine Datenbank mit der Verbindungs Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a31cd-188">Opens a database using the connection string.</span></span> <span data-ttu-id="a31cd-189">(Dies steht im Gegensatz zu `Database.Open`, die einen Verbindungs Zeichenfolgen-Namen verwendet.)</span><span class="sxs-lookup"><span data-stu-id="a31cd-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="a31cd-190">Fragt die Datenbank mithilfe von *SQLStatement* ab (optional übergeben von Parametern) und gibt die Ergebnisse als Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="a31cd-191">Führt *SQLStatement* (mit optionalen Parametern) aus und gibt einen einzelnen Datensatz zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="a31cd-192">Führt *SQLStatement* (mit optionalen Parametern) aus und gibt einen einzelnen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="a31cd-193">Hilfsmethoden</span><span class="sxs-lookup"><span data-stu-id="a31cd-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="a31cd-194">Rendert den Google Analytics-JavaScript-Code für die angegebene ID.</span><span class="sxs-lookup"><span data-stu-id="a31cd-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="a31cd-195">Rendert den JavaScript-Code der Status Counter Analytics für das angegebene Projekt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="a31cd-196">Rendert den JavaScript-Code für Yahoo Analytics für das angegebene Konto.</span><span class="sxs-lookup"><span data-stu-id="a31cd-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="a31cd-197">Übergibt eine Suche an den Dienst.</span><span class="sxs-lookup"><span data-stu-id="a31cd-197">Passes a search to Bing.</span></span> <span data-ttu-id="a31cd-198">Zum Angeben der zu durchsuchenden Site und eines Titels für das Suchfeld können Sie die Eigenschaften `Bing.SiteUrl` und `Bing.SiteTitle` festlegen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="a31cd-199">Normalerweise legen Sie diese Eigenschaften auf der *\_AppStart* -Seite fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="a31cd-200">Initialisiert ein Diagramm.</span><span class="sxs-lookup"><span data-stu-id="a31cd-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="a31cd-201">Fügt einem Diagramm eine Legende hinzu.</span><span class="sxs-lookup"><span data-stu-id="a31cd-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="a31cd-202">Fügt dem Diagramm eine Reihe von Werten hinzu.</span><span class="sxs-lookup"><span data-stu-id="a31cd-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="a31cd-203">Gibt einen Hash für die angegebenen Daten zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="a31cd-204">Der Standard Algorithmus ist `sha256`.</span><span class="sxs-lookup"><span data-stu-id="a31cd-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="a31cd-205">Ermöglicht Facebook-Benutzern das Herstellen einer Verbindung mit Seiten.</span><span class="sxs-lookup"><span data-stu-id="a31cd-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="a31cd-206">Rendert die Benutzeroberfläche zum Hochladen von Dateien.</span><span class="sxs-lookup"><span data-stu-id="a31cd-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="a31cd-207">Rendert das angegebene Xbox-gamendtag.</span><span class="sxs-lookup"><span data-stu-id="a31cd-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="a31cd-208">Rendert das Gravatar-Bild für die angegebene e-Mail-Adresse.</span><span class="sxs-lookup"><span data-stu-id="a31cd-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="a31cd-209">Konvertiert ein Datenobjekt in eine Zeichenfolge im JSON-Format (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="a31cd-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="a31cd-210">Konvertiert eine JSON-codierte Eingabe Zeichenfolge in ein Datenobjekt, das Sie durchlaufen oder in eine Datenbank einfügen können.</span><span class="sxs-lookup"><span data-stu-id="a31cd-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="a31cd-211">Rendert soziale Netzwerk Verknüpfungen unter Verwendung des angegebenen Titels und der optionalen URL.</span><span class="sxs-lookup"><span data-stu-id="a31cd-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="a31cd-212">Ordnet eine Fehlermeldung einem Formularfeld zu.</span><span class="sxs-lookup"><span data-stu-id="a31cd-212">Associates an error message with a form field.</span></span> <span data-ttu-id="a31cd-213">Verwenden Sie das `ModelState`-Hilfsprogramm, um auf diesen Member zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="a31cd-214">Verknüpft eine Fehlermeldung mit einem Formular.</span><span class="sxs-lookup"><span data-stu-id="a31cd-214">Associates an error message with a form.</span></span> <span data-ttu-id="a31cd-215">Verwenden Sie das `ModelState`-Hilfsprogramm, um auf diesen Member zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="a31cd-216">Gibt true zurück, wenn keine Validierungs Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="a31cd-217">Verwenden Sie das `ModelState`-Hilfsprogramm, um auf diesen Member zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="a31cd-218">Rendert die Eigenschaften und Werte eines Objekts und aller untergeordneten Objekte.</span><span class="sxs-lookup"><span data-stu-id="a31cd-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="a31cd-219">Rendert den reCAPTCHA-Überprüfungs Test.</span><span class="sxs-lookup"><span data-stu-id="a31cd-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="a31cd-220">Legt öffentliche und private Schlüssel für den "reCAPTCHA"-Dienst fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="a31cd-221">Normalerweise legen Sie diese Eigenschaften auf der *\_AppStart* -Seite fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="a31cd-222">Gibt das Ergebnis des "reCAPTCHA"-Tests zurück.</span><span class="sxs-lookup"><span data-stu-id="a31cd-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="a31cd-223">Rendert Statusinformationen über ASP.net Web Pages.</span><span class="sxs-lookup"><span data-stu-id="a31cd-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="a31cd-224">Rendert einen Twitter-Stream für den angegebenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a31cd-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="a31cd-225">Rendert einen Twitter-Stream für den angegebenen Suchtext.</span><span class="sxs-lookup"><span data-stu-id="a31cd-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="a31cd-226">Rendert einen Flash-Video Player für die angegebene Datei mit optionaler Breite und Höhe.</span><span class="sxs-lookup"><span data-stu-id="a31cd-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="a31cd-227">Rendert einen Windows Media Player für die angegebene Datei mit optionaler Breite und Höhe.</span><span class="sxs-lookup"><span data-stu-id="a31cd-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="a31cd-228">Rendert einen Silverlight-Player für die angegebene *XAP* -Datei mit der erforderlichen Breite und Höhe.</span><span class="sxs-lookup"><span data-stu-id="a31cd-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="a31cd-229">Gibt das durch den *Schlüssel*angegebene Objekt oder NULL zurück, wenn das Objekt nicht gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="a31cd-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="a31cd-230">Entfernt das durch den *Schlüssel* angegebene Objekt aus dem Cache.</span><span class="sxs-lookup"><span data-stu-id="a31cd-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="a31cd-231">Legt den *Wert* im Cache unter dem durch *Key*angegebenen Namen ab.</span><span class="sxs-lookup"><span data-stu-id="a31cd-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="a31cd-232">Erstellt ein neues `WebGrid`-Objekt mithilfe von Daten aus einer Abfrage.</span><span class="sxs-lookup"><span data-stu-id="a31cd-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="a31cd-233">Rendert Markup zum Anzeigen von Daten in einer HTML-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="a31cd-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="a31cd-234">Rendert einen Pager für das `WebGrid` Objekt.</span><span class="sxs-lookup"><span data-stu-id="a31cd-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="a31cd-235">Lädt ein Bild aus dem angegebenen Pfad.</span><span class="sxs-lookup"><span data-stu-id="a31cd-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="a31cd-236">Fügt das angegebene Bild als Wasserzeichen hinzu.</span><span class="sxs-lookup"><span data-stu-id="a31cd-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="a31cd-237">Fügt dem Bild den angegebenen Text hinzu.</span><span class="sxs-lookup"><span data-stu-id="a31cd-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="a31cd-238">Flippt das Bild horizontal oder vertikal.</span><span class="sxs-lookup"><span data-stu-id="a31cd-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="a31cd-239">Lädt ein Bild, wenn ein Bild während eines Datei Uploads an eine Seite gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="a31cd-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="a31cd-240">Ändert die Größe des Bilds.</span><span class="sxs-lookup"><span data-stu-id="a31cd-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="a31cd-241">Dreht das Bild nach links oder nach rechts.</span><span class="sxs-lookup"><span data-stu-id="a31cd-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="a31cd-242">Speichert das Bild im angegebenen Pfad.</span><span class="sxs-lookup"><span data-stu-id="a31cd-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="a31cd-243">Legt das Kennwort für den SMTP-Server fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="a31cd-244">Normalerweise legen Sie diese Eigenschaft auf der *\_AppStart* -Seite fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="a31cd-245">Sendet eine E-Mail.</span><span class="sxs-lookup"><span data-stu-id="a31cd-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="a31cd-246">Legt den Namen des SMTP-Servers fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-246">Sets the SMTP server name.</span></span> <span data-ttu-id="a31cd-247">Normalerweise legen Sie diese Eigenschaft auf der *\_AppStart* -Seite fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-247">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="a31cd-248">Legt den Benutzernamen für den SMTP-Server fest.</span><span class="sxs-lookup"><span data-stu-id="a31cd-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="a31cd-249">Normalerweise sollten Sie diese Eigenschaft auf der *\_AppStart* -Seite festlegen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="a31cd-250">Überprüfen</span><span class="sxs-lookup"><span data-stu-id="a31cd-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="a31cd-251">v2 Rendert eine Validierungs Fehlermeldung für das angegebene Feld.</span><span class="sxs-lookup"><span data-stu-id="a31cd-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="a31cd-252">v2 Zeigt eine Liste aller Validierungs Fehler an.</span><span class="sxs-lookup"><span data-stu-id="a31cd-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="a31cd-253">v2 Registriert ein Benutzereingabe Element für den angegebenen Validierungstyp.</span><span class="sxs-lookup"><span data-stu-id="a31cd-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="a31cd-254">v2 Rendert dynamisch CSS-Klassenattribute für die Client seitige Validierung, sodass Sie Validierungs Fehlermeldungen formatieren können.</span><span class="sxs-lookup"><span data-stu-id="a31cd-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="a31cd-255">(Erfordert, dass Sie auf die entsprechenden Client Skript Bibliotheken verweisen und CSS-Klassen definieren.)</span><span class="sxs-lookup"><span data-stu-id="a31cd-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="a31cd-256">v2 Aktiviert die Client seitige Validierung für das Benutzereingabe Feld.</span><span class="sxs-lookup"><span data-stu-id="a31cd-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="a31cd-257">(Erfordert, dass Sie auf die entsprechenden Client Skript Bibliotheken verweisen.)</span><span class="sxs-lookup"><span data-stu-id="a31cd-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="a31cd-258">v2 Gibt true zurück, wenn alle Benutzereingabe Elemente, die für die Validierung registriert sind, gültige Werte enthalten.</span><span class="sxs-lookup"><span data-stu-id="a31cd-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="a31cd-259">v2 Gibt an, dass Benutzer einen Wert für das User Input-Element angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="a31cd-260">v2 Gibt an, dass Benutzer Werte für jedes Benutzereingabe Element angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="a31cd-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="a31cd-261">Diese Methode ermöglicht keine Angabe einer benutzerdefinierten Fehlermeldung.</span><span class="sxs-lookup"><span data-stu-id="a31cd-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="a31cd-262">v2 Gibt einen Validierungstest an, wenn Sie die `Validation.Add`-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="a31cd-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
