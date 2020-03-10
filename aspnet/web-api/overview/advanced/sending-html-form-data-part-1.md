---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Senden von HTML-Formulardaten in ASP.net-Web-API: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: In diesem Artikel wird gezeigt, wie Sie form-urlencoded-Daten an einen Web-API-Controller mit ASP.NET 4. x senden.
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449241"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="036fb-103">Senden von HTML-Formulardaten in ASP.net-Web-API: Formular-urlencoded-Daten</span><span class="sxs-lookup"><span data-stu-id="036fb-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="036fb-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="036fb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="036fb-105">Teil 1: Formular-urlencoded-Daten</span><span class="sxs-lookup"><span data-stu-id="036fb-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="036fb-106">In diesem Artikel wird gezeigt, wie Sie in Form von urlencoded-Daten an einen Web-API-Controller Posten.</span><span class="sxs-lookup"><span data-stu-id="036fb-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="036fb-107">Übersicht über HTML-Formulare</span><span class="sxs-lookup"><span data-stu-id="036fb-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="036fb-108">Senden komplexer Typen</span><span class="sxs-lookup"><span data-stu-id="036fb-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="036fb-109">Senden von Formulardaten über AJAX</span><span class="sxs-lookup"><span data-stu-id="036fb-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="036fb-110">Senden von einfachen Typen</span><span class="sxs-lookup"><span data-stu-id="036fb-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="036fb-111">[Laden Sie das abgeschlossene Projekt herunter](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="036fb-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="036fb-112">Übersicht über HTML-Formulare</span><span class="sxs-lookup"><span data-stu-id="036fb-112">Overview of HTML Forms</span></span>

<span data-ttu-id="036fb-113">HTML-Formulare verwenden entweder Get oder Post zum Senden von Daten an den Server.</span><span class="sxs-lookup"><span data-stu-id="036fb-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="036fb-114">Das **method** -Attribut des **Form** -Elements gibt die HTTP-Methode an:</span><span class="sxs-lookup"><span data-stu-id="036fb-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="036fb-115">Die Standardmethode ist "Get".</span><span class="sxs-lookup"><span data-stu-id="036fb-115">The default method is GET.</span></span> <span data-ttu-id="036fb-116">Wenn das Formular Get verwendet, werden die Formulardaten im URI als Abfrage Zeichenfolge codiert.</span><span class="sxs-lookup"><span data-stu-id="036fb-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="036fb-117">Wenn das Formular Post verwendet, werden die Formulardaten im Anforderungs Text abgelegt.</span><span class="sxs-lookup"><span data-stu-id="036fb-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="036fb-118">Für veröffentlichte Daten gibt das **enctype** -Attribut das Format des Anforderungs Texts an:</span><span class="sxs-lookup"><span data-stu-id="036fb-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="036fb-119">"CType"</span><span class="sxs-lookup"><span data-stu-id="036fb-119">enctype</span></span> | <span data-ttu-id="036fb-120">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="036fb-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="036fb-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="036fb-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="036fb-122">Formulardaten werden als Name-Wert-Paare codiert, ähnlich wie eine URI-Abfrage Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="036fb-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="036fb-123">Dies ist das Standardformat für Post.</span><span class="sxs-lookup"><span data-stu-id="036fb-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="036fb-124">Multipart/Form-Data</span><span class="sxs-lookup"><span data-stu-id="036fb-124">multipart/form-data</span></span> | <span data-ttu-id="036fb-125">Formulardaten werden als mehrteilige MIME-Nachricht codiert.</span><span class="sxs-lookup"><span data-stu-id="036fb-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="036fb-126">Verwenden Sie dieses Format, wenn Sie eine Datei auf den Server hochladen.</span><span class="sxs-lookup"><span data-stu-id="036fb-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="036fb-127">In Teil 1 dieses Artikels wird das Format "x-www-form-urlencoded" untersucht.</span><span class="sxs-lookup"><span data-stu-id="036fb-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="036fb-128">[Teil 2](sending-html-form-data-part-2.md) beschreibt mehrteilige MIME.</span><span class="sxs-lookup"><span data-stu-id="036fb-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="036fb-129">Senden komplexer Typen</span><span class="sxs-lookup"><span data-stu-id="036fb-129">Sending Complex Types</span></span>

<span data-ttu-id="036fb-130">Normalerweise senden Sie einen komplexen Typ, der aus Werten besteht, die von verschiedenen Formular Steuerelementen entnommen werden.</span><span class="sxs-lookup"><span data-stu-id="036fb-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="036fb-131">Beachten Sie das folgende Modell, das eine Statusaktualisierung darstellt:</span><span class="sxs-lookup"><span data-stu-id="036fb-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="036fb-132">Hier ist ein Web-API-Controller, der eine `Update` Objekt per Post akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="036fb-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="036fb-133">Dieser Controller verwendet [Aktions basiertes Routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), sodass die Routen Vorlage &quot;API/{Controller}/{Action}/{ID}&quot;ist.</span><span class="sxs-lookup"><span data-stu-id="036fb-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="036fb-134">Der Client sendet die Daten an &quot;/API/Updates/Complex-&quot;.</span><span class="sxs-lookup"><span data-stu-id="036fb-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="036fb-135">Nun schreiben wir ein HTML-Formular, damit Benutzer eine Statusaktualisierung übermitteln können.</span><span class="sxs-lookup"><span data-stu-id="036fb-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="036fb-136">Beachten Sie, dass das **Action** -Attribut im Formular der URI der Controller Aktion ist.</span><span class="sxs-lookup"><span data-stu-id="036fb-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="036fb-137">Hier ist das Formular, in das einige Werte eingegeben werden:</span><span class="sxs-lookup"><span data-stu-id="036fb-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="036fb-138">Wenn der Benutzer auf Senden klickt, sendet der Browser eine HTTP-Anforderung ähnlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="036fb-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="036fb-139">Beachten Sie, dass der Anforderungs Text die Formulardaten enthält, die als Name-Wert-Paare formatiert sind.</span><span class="sxs-lookup"><span data-stu-id="036fb-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="036fb-140">Die Web-API konvertiert die Name-Wert-Paare automatisch in eine Instanz der `Update`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="036fb-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="036fb-141">Senden von Formulardaten über AJAX</span><span class="sxs-lookup"><span data-stu-id="036fb-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="036fb-142">Wenn ein Benutzer ein Formular sendet, navigiert der Browser von der aktuellen Seite Weg und rendert den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="036fb-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="036fb-143">Das ist in Ordnung, wenn die Antwort eine HTML-Seite ist.</span><span class="sxs-lookup"><span data-stu-id="036fb-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="036fb-144">Bei einer Web-API ist der Antworttext jedoch in der Regel entweder leer oder enthält strukturierte Daten, wie z. b. JSON.</span><span class="sxs-lookup"><span data-stu-id="036fb-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="036fb-145">In diesem Fall ist es sinnvoller, die Formulardaten mit einer AJAX-Anforderung zu senden, damit die Seite die Antwort verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="036fb-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="036fb-146">Der folgende Code zeigt, wie Sie Formulardaten mithilfe von jQuery veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="036fb-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="036fb-147">Die jQuery-Funktion zum **senden** ersetzt die Form-Aktion durch eine neue Funktion.</span><span class="sxs-lookup"><span data-stu-id="036fb-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="036fb-148">Dies überschreibt das Standardverhalten der Schaltfläche "Senden".</span><span class="sxs-lookup"><span data-stu-id="036fb-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="036fb-149">Die **serialisieren** -Funktion serialisiert die Formulardaten in Name/Wert-Paare.</span><span class="sxs-lookup"><span data-stu-id="036fb-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="036fb-150">Um die Formulardaten an den Server zu senden, wenden Sie `$.post()`an.</span><span class="sxs-lookup"><span data-stu-id="036fb-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="036fb-151">Wenn die Anforderung abgeschlossen ist, zeigt der `.success()` oder `.error()` Handler dem Benutzer eine entsprechende Meldung an.</span><span class="sxs-lookup"><span data-stu-id="036fb-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="036fb-152">Senden von einfachen Typen</span><span class="sxs-lookup"><span data-stu-id="036fb-152">Sending Simple Types</span></span>

<span data-ttu-id="036fb-153">In den vorherigen Abschnitten haben wir einen komplexen Typ gesendet, der die Web-API in eine Instanz einer Modell Klasse deserialisiert hat.</span><span class="sxs-lookup"><span data-stu-id="036fb-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="036fb-154">Sie können auch einfache Typen, z. b. eine Zeichenfolge, senden.</span><span class="sxs-lookup"><span data-stu-id="036fb-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="036fb-155">Vor dem Senden eines einfachen Typs sollten Sie den Wert stattdessen in einem komplexen Typ umhüllen.</span><span class="sxs-lookup"><span data-stu-id="036fb-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="036fb-156">Dies bietet Ihnen die Vorteile der Modell Validierung auf Serverseite und erleichtert das Erweitern des Modells bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="036fb-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="036fb-157">Die grundlegenden Schritte zum Senden eines einfachen Typs sind identisch, aber es gibt zwei feine Unterschiede.</span><span class="sxs-lookup"><span data-stu-id="036fb-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="036fb-158">Zuerst müssen Sie im Controller den Parameternamen mit dem **frombody** -Attribut ergänzen.</span><span class="sxs-lookup"><span data-stu-id="036fb-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="036fb-159">Standardmäßig versucht die Web-API, einfache Typen aus dem Anforderungs-URI zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="036fb-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="036fb-160">Das **frombody** -Attribut weist die Web-API an, den Wert aus dem Anforderungs Text zu lesen.</span><span class="sxs-lookup"><span data-stu-id="036fb-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="036fb-161">Die Web-API liest den Antworttext höchstens einmal, sodass nur ein Parameter einer Aktion aus dem Anforderungs Text stammen kann.</span><span class="sxs-lookup"><span data-stu-id="036fb-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="036fb-162">Wenn Sie mehrere Werte aus dem Anforderungs Text erhalten müssen, definieren Sie einen komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="036fb-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="036fb-163">Zweitens muss der Client den Wert im folgenden Format senden:</span><span class="sxs-lookup"><span data-stu-id="036fb-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="036fb-164">Insbesondere muss der Namensteil des Name-Wert-Paars für einen einfachen Typ leer sein.</span><span class="sxs-lookup"><span data-stu-id="036fb-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="036fb-165">Nicht alle Browser unterstützen dies für HTML-Formulare, aber Sie erstellen dieses Format wie folgt im Skript:</span><span class="sxs-lookup"><span data-stu-id="036fb-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="036fb-166">Im folgenden finden Sie ein Beispiel für eine Form:</span><span class="sxs-lookup"><span data-stu-id="036fb-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="036fb-167">Und hier ist das Skript zum Übermitteln des Formular Werts.</span><span class="sxs-lookup"><span data-stu-id="036fb-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="036fb-168">Der einzige Unterschied zum vorherigen Skript ist das Argument, das an die **Post** -Funktion übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="036fb-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="036fb-169">Sie können den gleichen Ansatz verwenden, um ein Array einfacher Typen zu senden:</span><span class="sxs-lookup"><span data-stu-id="036fb-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="036fb-170">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="036fb-170">Additional Resources</span></span>

[<span data-ttu-id="036fb-171">Teil 2: Datei Upload und mehrteilige MIME</span><span class="sxs-lookup"><span data-stu-id="036fb-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
