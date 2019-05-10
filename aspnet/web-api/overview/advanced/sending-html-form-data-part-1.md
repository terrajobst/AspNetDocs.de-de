---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Senden von HTML-Formulardaten in ASP.NET Web-API: Form-Urlencoded-Daten - ASP.NET 4.x'
author: MikeWasson
description: In diesem Artikel zeigt, wie zum Posten von Form-Urlencoded-Daten an einen Web-API-Controller in ASP.NET 4.x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115472"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="fd460-103">Senden von HTML-Formulardaten in ASP.NET Web-API: Form-urlencoded-Daten</span><span class="sxs-lookup"><span data-stu-id="fd460-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="fd460-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fd460-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="fd460-105">Teil 1: Form-urlencoded-Daten</span><span class="sxs-lookup"><span data-stu-id="fd460-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="fd460-106">In diesem Artikel veranschaulicht das Form-Urlencoded-Daten an einen Web-API-Controller zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="fd460-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="fd460-107">Übersicht über die HTML-Formularen</span><span class="sxs-lookup"><span data-stu-id="fd460-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="fd460-108">Senden von komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="fd460-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="fd460-109">Senden von Formulardaten per AJAX</span><span class="sxs-lookup"><span data-stu-id="fd460-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="fd460-110">Senden von einfachen Typen</span><span class="sxs-lookup"><span data-stu-id="fd460-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="fd460-111">[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="fd460-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="fd460-112">Übersicht über die HTML-Formularen</span><span class="sxs-lookup"><span data-stu-id="fd460-112">Overview of HTML Forms</span></span>

<span data-ttu-id="fd460-113">HTML-Formulare verwenden wird entweder GET oder POST zum Senden von Daten an den Server.</span><span class="sxs-lookup"><span data-stu-id="fd460-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="fd460-114">Die **Methode** Attribut der **Formular** -Element können die HTTP-Methode:</span><span class="sxs-lookup"><span data-stu-id="fd460-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="fd460-115">Die Standardmethode ist GET.</span><span class="sxs-lookup"><span data-stu-id="fd460-115">The default method is GET.</span></span> <span data-ttu-id="fd460-116">Wenn das Formular verwendet zu erhalten, das Formular, das Daten in den URI als Zeichenfolge codiert ist.</span><span class="sxs-lookup"><span data-stu-id="fd460-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="fd460-117">Wenn das Formular POST verwendet wird, werden die Formulardaten im Anforderungstext platziert.</span><span class="sxs-lookup"><span data-stu-id="fd460-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="fd460-118">Für Gebucht-Daten die **Enctype** Attribut gibt an, das Format des Anforderungstexts:</span><span class="sxs-lookup"><span data-stu-id="fd460-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="fd460-119">enctype</span><span class="sxs-lookup"><span data-stu-id="fd460-119">enctype</span></span> | <span data-ttu-id="fd460-120">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="fd460-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fd460-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="fd460-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="fd460-122">Als Name/Wert-Paare, ähnlich wie eine URI-Abfragezeichenfolge ist die Formulardaten codiert.</span><span class="sxs-lookup"><span data-stu-id="fd460-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="fd460-123">Dies ist das Standardformat für POST-Methode.</span><span class="sxs-lookup"><span data-stu-id="fd460-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="fd460-124">Multipart/Form-data</span><span class="sxs-lookup"><span data-stu-id="fd460-124">multipart/form-data</span></span> | <span data-ttu-id="fd460-125">Formulardaten werden als eine mehrteilige MIME-Nachricht codiert.</span><span class="sxs-lookup"><span data-stu-id="fd460-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="fd460-126">Verwenden Sie dieses Format, wenn Sie eine Datei mit dem Server hochladen.</span><span class="sxs-lookup"><span data-stu-id="fd460-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="fd460-127">Teil 1 dieses Artikels untersucht die X-www-form-urlencoded-Format.</span><span class="sxs-lookup"><span data-stu-id="fd460-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="fd460-128">[Teil 2](sending-html-form-data-part-2.md) mehrteiligen MIME-Nachrichten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="fd460-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="fd460-129">Senden von komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="fd460-129">Sending Complex Types</span></span>

<span data-ttu-id="fd460-130">Senden Sie in der Regel einen komplexen Typ, bestehend aus Werten aus mehrere Steuerelemente des Formulars.</span><span class="sxs-lookup"><span data-stu-id="fd460-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="fd460-131">Betrachten Sie das folgende Modell, das eine statusaktualisierung darstellt:</span><span class="sxs-lookup"><span data-stu-id="fd460-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="fd460-132">Hier ist ein Web-API-Controller, die akzeptiert eine `Update` Objekt über POST.</span><span class="sxs-lookup"><span data-stu-id="fd460-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="fd460-133">Dieser Controller verwendet [Aktion-basiertes routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), sodass die routenvorlage &quot;api / {Controller} / {Action} / {Id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="fd460-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="fd460-134">Der Client wird die Daten zu buchen &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="fd460-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="fd460-135">Jetzt schreiben wir nun ein HTML-Formular für Benutzer, eine statusaktualisierung zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="fd460-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="fd460-136">Beachten Sie, dass die **Aktion** Attribut des Formulars ist der URI des unsere Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="fd460-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="fd460-137">So sieht das Formular mit einigen in eingegebenen Werten aus:</span><span class="sxs-lookup"><span data-stu-id="fd460-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="fd460-138">Klickt der Benutzer senden, sendet der Browser eine HTTP-Anforderung ähnelt dem folgenden:</span><span class="sxs-lookup"><span data-stu-id="fd460-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="fd460-139">Beachten Sie, dass der Anforderungstext Daten aus dem Formular, formatiert als Name/Wert-Paare enthält.</span><span class="sxs-lookup"><span data-stu-id="fd460-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="fd460-140">Web-API konvertiert automatisch Name/Wert-Paare in einer Instanz von der `Update` Klasse.</span><span class="sxs-lookup"><span data-stu-id="fd460-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="fd460-141">Senden von Formulardaten per AJAX</span><span class="sxs-lookup"><span data-stu-id="fd460-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="fd460-142">Wenn ein Benutzer ein Formular übermittelt, wird der Browser von der aktuellen Seite weg navigiert und rendert den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="fd460-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="fd460-143">Das ist in Ordnung Wenn die Antwort auf eine HTML-Seite ist.</span><span class="sxs-lookup"><span data-stu-id="fd460-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="fd460-144">Mit einer Web-API, der Antworttext ist jedoch in der Regel entweder leer ist oder strukturierte Daten, z.B. JSON enthält.</span><span class="sxs-lookup"><span data-stu-id="fd460-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="fd460-145">In diesem Fall macht es mehr Sinn, die zum Senden von Daten aus dem Formular mit einer AJAX-Anforderung, damit an, dass die Antwort von die Seite verarbeitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="fd460-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="fd460-146">Der folgende Code zeigt, wie unter Verwendung von jQuery Formulardaten übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="fd460-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="fd460-147">Die jQuery **übermitteln** -Funktion ersetzt die Formularaktion mit einer neuen Funktion.</span><span class="sxs-lookup"><span data-stu-id="fd460-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="fd460-148">Dies überschreibt das Standardverhalten der Senden-Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="fd460-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="fd460-149">Die **Serialisieren** Funktion serialisiert Daten aus dem Formular in Name/Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="fd460-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="fd460-150">Rufen Sie zum Senden von Daten aus dem Formular an den Server `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="fd460-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="fd460-151">Wenn die Anforderung abgeschlossen ist, die `.success()` oder `.error()` Handler dem Benutzer eine entsprechende Meldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fd460-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="fd460-152">Senden von einfachen Typen</span><span class="sxs-lookup"><span data-stu-id="fd460-152">Sending Simple Types</span></span>

<span data-ttu-id="fd460-153">In den vorherigen Abschnitten haben wir einen komplexen Typ, der Web-API mit einer Instanz von einer Modellklasse deserialisiert gesendet.</span><span class="sxs-lookup"><span data-stu-id="fd460-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="fd460-154">Sie können auch einfache Typen, z. B. eine Zeichenfolge senden.</span><span class="sxs-lookup"><span data-stu-id="fd460-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="fd460-155">Senden einen einfachen Typ, berücksichtigen Sie stattdessen den Wert in einem komplexen Typ umschließt.</span><span class="sxs-lookup"><span data-stu-id="fd460-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="fd460-156">Dies bietet Ihnen die Vorteile der modellvalidierung auf der Serverseite und erleichtert es, Ihr Modell zu erweitern, wenn erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fd460-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="fd460-157">Die grundlegenden Schritte zum Senden von eines einfachen Typs sind identisch, aber es gibt zwei feine Unterschiede.</span><span class="sxs-lookup"><span data-stu-id="fd460-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="fd460-158">Zunächst im Controller muss, ergänzen Sie den Namen des Parameters mit dem **FromBody** Attribut.</span><span class="sxs-lookup"><span data-stu-id="fd460-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="fd460-159">Web-API versucht standardmäßig einfache Typen aus der Anforderungs-URI abrufen.</span><span class="sxs-lookup"><span data-stu-id="fd460-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="fd460-160">Die **FromBody** -Attribut weist die Web-API zum Lesen des Werts aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="fd460-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="fd460-161">Web-API liest den Antworttext nur einen Parameter einer Aktion aus dem Anforderungstext stammen kann wird höchstens einmal zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="fd460-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="fd460-162">Wenn Sie mehrere Werte aus dem Anforderungstext abrufen müssen, definieren Sie einen komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="fd460-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="fd460-163">Zweitens muss der Client zum Senden von des Wert mit dem folgenden Format ein:</span><span class="sxs-lookup"><span data-stu-id="fd460-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="fd460-164">Insbesondere muss der Namensteil von Name/Wert-Paar für einen einfachen Typ leer sein.</span><span class="sxs-lookup"><span data-stu-id="fd460-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="fd460-165">Nicht alle Browser unterstützen diese für die HTML-Formularen, aber Sie erstellen dieses Format im Skript wie folgt:</span><span class="sxs-lookup"><span data-stu-id="fd460-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="fd460-166">Hier ist ein Beispielformular:</span><span class="sxs-lookup"><span data-stu-id="fd460-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="fd460-167">Und hier ist das Skript aus, um den Formularwert zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="fd460-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="fd460-168">Der einzige Unterschied besteht aus dem vorherigen Skript wird das übergebene Argument der **Posten** Funktion.</span><span class="sxs-lookup"><span data-stu-id="fd460-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="fd460-169">Sie können den gleichen Ansatz verwenden, um ein Array von einfachen Typen zu senden:</span><span class="sxs-lookup"><span data-stu-id="fd460-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="fd460-170">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fd460-170">Additional Resources</span></span>

[<span data-ttu-id="fd460-171">Teil 2: Dateiupload und mehrteiligen MIME-Nachrichten</span><span class="sxs-lookup"><span data-stu-id="fd460-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
