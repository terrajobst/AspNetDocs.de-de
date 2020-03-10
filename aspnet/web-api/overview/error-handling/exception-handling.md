---
uid: web-api/overview/error-handling/exception-handling
title: Ausnahmebehandlung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504699"
---
# <a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="6f300-102">Ausnahmebehandlung in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="6f300-102">Exception Handling in ASP.NET Web API</span></span>

<span data-ttu-id="6f300-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6f300-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6f300-104">Dieser Artikel beschreibt die Behandlung von Fehlern und Ausnahmen in ASP.net-Web-API.</span><span class="sxs-lookup"><span data-stu-id="6f300-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="6f300-105">Httpresponeinexception</span><span class="sxs-lookup"><span data-stu-id="6f300-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="6f300-106">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="6f300-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="6f300-107">Registrieren von Ausnahme Filtern</span><span class="sxs-lookup"><span data-stu-id="6f300-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="6f300-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="6f300-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="6f300-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="6f300-109">HttpResponseException</span></span>

<span data-ttu-id="6f300-110">Was geschieht, wenn ein Web-API-Controller eine nicht abgefangene Ausnahme auslöst?</span><span class="sxs-lookup"><span data-stu-id="6f300-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="6f300-111">Standardmäßig werden die meisten Ausnahmen in eine HTTP-Antwort mit dem Statuscode 500, interner Server Fehler, übersetzt.</span><span class="sxs-lookup"><span data-stu-id="6f300-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="6f300-112">Der **httpresponserexception** -Typ ist ein Sonderfall.</span><span class="sxs-lookup"><span data-stu-id="6f300-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="6f300-113">Diese Ausnahme gibt den HTTP-Statuscode zurück, den Sie im Ausnahmekonstruktor angeben.</span><span class="sxs-lookup"><span data-stu-id="6f300-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="6f300-114">Beispielsweise gibt die folgende Methode 404 zurück, nicht gefunden, wenn der *ID* -Parameter nicht gültig ist.</span><span class="sxs-lookup"><span data-stu-id="6f300-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="6f300-115">Um die Antwort besser steuern zu können, können Sie auch die gesamte Antwortnachricht erstellen und mit **httpresponseexception einschließen:**</span><span class="sxs-lookup"><span data-stu-id="6f300-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="6f300-116">Ausnahmefilter</span><span class="sxs-lookup"><span data-stu-id="6f300-116">Exception Filters</span></span>

<span data-ttu-id="6f300-117">Sie können anpassen, wie die Web-API Ausnahmen behandelt, indem Sie einen *Ausnahme Filter*schreiben.</span><span class="sxs-lookup"><span data-stu-id="6f300-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="6f300-118">Ein Ausnahme Filter wird ausgeführt, wenn eine Controller Methode eine nicht behandelte Ausnahme auslöst, bei der es sich *nicht* um eine **httprespongexception** -Ausnahme handelt.</span><span class="sxs-lookup"><span data-stu-id="6f300-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="6f300-119">Der **httpresponanexception** -Typ ist ein Sonderfall, da er speziell für die Rückgabe einer HTTP-Antwort entworfen wurde.</span><span class="sxs-lookup"><span data-stu-id="6f300-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="6f300-120">Ausnahme Filter implementieren die **System. Web. http. Filters. iexceptionfilter** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="6f300-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="6f300-121">Die einfachste Möglichkeit, einen Ausnahme Filter zu schreiben, besteht darin, von der **System. Web. http. Filters. exceptionfilterattribute** -Klasse abzuleiten und die **OnException** -Methode zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="6f300-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="6f300-122">Ausnahme Filter in ASP.net-Web-API ähneln denen in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6f300-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="6f300-123">Allerdings werden Sie in einem separaten Namespace und einer separaten Funktion deklariert.</span><span class="sxs-lookup"><span data-stu-id="6f300-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="6f300-124">Insbesondere die in MVC verwendete Klasse " **typerrorattribute** " behandelt keine Ausnahmen, die von Web-API-Controllern ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="6f300-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>

<span data-ttu-id="6f300-125">Hier ist ein Filter, der **NotImplementedException** -Ausnahmen in den HTTP-Statuscode 501 konvertiert, nicht implementiert:</span><span class="sxs-lookup"><span data-stu-id="6f300-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="6f300-126">Die **Response** -Eigenschaft des **httpactionexecutedcontext** -Objekts enthält die http-Antwortnachricht, die an den Client gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="6f300-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="6f300-127">Registrieren von Ausnahme Filtern</span><span class="sxs-lookup"><span data-stu-id="6f300-127">Registering Exception Filters</span></span>

<span data-ttu-id="6f300-128">Es gibt mehrere Möglichkeiten zum Registrieren eines Web-API-Ausnahmefilters:</span><span class="sxs-lookup"><span data-stu-id="6f300-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="6f300-129">Nach Aktion</span><span class="sxs-lookup"><span data-stu-id="6f300-129">By action</span></span>
- <span data-ttu-id="6f300-130">Nach Controller</span><span class="sxs-lookup"><span data-stu-id="6f300-130">By controller</span></span>
- <span data-ttu-id="6f300-131">Global</span><span class="sxs-lookup"><span data-stu-id="6f300-131">Globally</span></span>

<span data-ttu-id="6f300-132">Um den Filter auf eine bestimmte Aktion anzuwenden, fügen Sie den Filter der Aktion als Attribut hinzu:</span><span class="sxs-lookup"><span data-stu-id="6f300-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="6f300-133">Fügen Sie der Controller Klasse den Filter als Attribut hinzu, um den Filter auf alle Aktionen auf einem Controller anzuwenden:</span><span class="sxs-lookup"><span data-stu-id="6f300-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="6f300-134">Wenn Sie den Filter auf alle Web-API-Controller Global anwenden möchten, fügen Sie eine Instanz des Filters zur **globalconfiguration. Configuration. Filters** -Auflistung hinzu.</span><span class="sxs-lookup"><span data-stu-id="6f300-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="6f300-135">Ausnahmefilter in dieser Sammlung gelten für alle Web-API-Controlleraktionen.</span><span class="sxs-lookup"><span data-stu-id="6f300-135">Exception filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="6f300-136">Wenn Sie das Projekt mit der Projektvorlage "ASP.NET MVC 4-Webanwendung" erstellen, fügen Sie Ihren Web-API-Konfigurations Code in die `WebApiConfig`-Klasse ein, die sich im Ordner "App\_Start" befindet:</span><span class="sxs-lookup"><span data-stu-id="6f300-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="6f300-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="6f300-137">HttpError</span></span>

<span data-ttu-id="6f300-138">Das **HttpError** -Objekt bietet eine konsistente Möglichkeit, Fehlerinformationen im Antworttext zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="6f300-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="6f300-139">Im folgenden Beispiel wird gezeigt, wie der HTTP-Statuscode 404 (nicht gefunden) mit **HttpError** im Antworttext zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6f300-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="6f300-140">" **" Ist eine** Erweiterungsmethode, die in der Klasse " **System .net. http. httprequestmessageextensions** " definiert ist.</span><span class="sxs-lookup"><span data-stu-id="6f300-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="6f300-141">Intern erstellt " **deateerrorresponse** " eine **HttpError** -Instanz und erstellt dann eine **HttpResponseMessage** , die **HttpError**enthält.</span><span class="sxs-lookup"><span data-stu-id="6f300-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="6f300-142">In diesem Beispiel wird das Produkt in der HTTP-Antwort zurückgegeben, wenn die Methode erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="6f300-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="6f300-143">Wenn das angeforderte Produkt jedoch nicht gefunden wird, enthält die HTTP-Antwort einen **HttpError** im Anforderungs Text.</span><span class="sxs-lookup"><span data-stu-id="6f300-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="6f300-144">Die Antwort kann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="6f300-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="6f300-145">Beachten Sie, dass **HttpError** in diesem Beispiel in JSON serialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="6f300-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="6f300-146">Ein Vorteil der Verwendung von **HttpError** besteht darin, dass Sie den gleichen [inhaltsaushandlungs-](../formats-and-model-binding/content-negotiation.md) und Serialisierungsprozess wie jedes andere stark typisierte Modell durchläuft.</span><span class="sxs-lookup"><span data-stu-id="6f300-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="6f300-147">HttpError und Modell Validierung</span><span class="sxs-lookup"><span data-stu-id="6f300-147">HttpError and Model Validation</span></span>

<span data-ttu-id="6f300-148">Bei der Modell Validierung können Sie den Modell Zustand an " **anateerrorresponse**" übergeben, um die Validierungs Fehler in die Antwort einzubeziehen:</span><span class="sxs-lookup"><span data-stu-id="6f300-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="6f300-149">In diesem Beispiel wird möglicherweise die folgende Antwort zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="6f300-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="6f300-150">Weitere Informationen zur Modell Validierung finden Sie unter [Modell Validierung in ASP.net-Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6f300-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="6f300-151">Verwenden von HttpError mit httpresponpexception</span><span class="sxs-lookup"><span data-stu-id="6f300-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="6f300-152">In den vorherigen Beispielen wird eine **httpresponsmessage** -Nachricht aus der Controller Aktion zurückgegeben, Sie können jedoch auch **httpresponsexception** verwenden, um einen **HttpError**-Wert zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="6f300-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="6f300-153">Auf diese Weise können Sie ein stark typisiertes Modell im normalen Erfolgsfall zurückgeben, während bei einem Fehler weiterhin **HttpError** zurückgegeben wird:</span><span class="sxs-lookup"><span data-stu-id="6f300-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
