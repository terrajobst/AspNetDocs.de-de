---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parameter Bindung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt, wie die Web-API Parameter bindet und wie der Bindungsprozess in ASP.NET 4. x angepasst wird.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5386532ab581e023d93d16a5d4107e07f40b986f
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985826"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="7eb70-103">Parameter Bindung in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="7eb70-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="7eb70-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7eb70-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="7eb70-105">In diesem Artikel wird beschrieben, wie die Web-API Parameter bindet und wie Sie den Bindungsprozess anpassen können.</span><span class="sxs-lookup"><span data-stu-id="7eb70-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="7eb70-106">Wenn die Web-API eine Methode auf einem Controller aufruft, muss Sie Werte für die Parameter festlegen, einen Prozess namens " *Binding*".</span><span class="sxs-lookup"><span data-stu-id="7eb70-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span>

<span data-ttu-id="7eb70-107">Standardmäßig verwendet die Web-API die folgenden Regeln, um Parameter zu binden:</span><span class="sxs-lookup"><span data-stu-id="7eb70-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="7eb70-108">Wenn es sich bei dem Parameter um einen einfachen Typ handelt, versucht die Web-API, den Wert aus dem URI zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="7eb70-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="7eb70-109">Einfache Typen umfassen die [primitiven](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .NET-Typen (**int**, **bool**, **Double**usw.) sowie **TimeSpan**, **DateTime**, **GUID**, **Decimal**und **String** *sowie* einen beliebigen Typ mit einem Typ. Konverter, der von einer Zeichenfolge konvertieren kann.</span><span class="sxs-lookup"><span data-stu-id="7eb70-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="7eb70-110">(Weitere Informationen zu Typkonvertern später.)</span><span class="sxs-lookup"><span data-stu-id="7eb70-110">(More about type converters later.)</span></span>
- <span data-ttu-id="7eb70-111">Bei komplexen Typen versucht die Web-API, den Wert aus dem Nachrichtentext mit einem [Medientyp-Formatierer](media-formatters.md)zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="7eb70-112">Hier ist z. b. eine typische Web-API-Controller Methode:</span><span class="sxs-lookup"><span data-stu-id="7eb70-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="7eb70-113">Der *ID* -Parameter ist &quot;ein&quot; einfacher Typ, daher versucht die Web-API, den Wert aus dem Anforderungs-URI zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="7eb70-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="7eb70-114">Der *Element* Parameter ist ein komplexer Typ, sodass die Web-API einen Medientyp-Formatierer verwendet, um den Wert aus dem Anforderungs Text zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="7eb70-115">Um einen Wert aus dem URI zu erhalten, sucht die Web-API in den Routendaten und in der URI-Abfrage Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="7eb70-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="7eb70-116">Die Routendaten werden aufgefüllt, wenn das Routing System den URI analysiert und mit einer Route übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="7eb70-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="7eb70-117">Weitere Informationen finden Sie unter [Routing und Aktions Auswahl](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="7eb70-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="7eb70-118">Im weiteren Verlauf dieses Artikels zeige ich Ihnen, wie Sie den Modell Bindungsprozess anpassen können.</span><span class="sxs-lookup"><span data-stu-id="7eb70-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="7eb70-119">Bei komplexen Typen empfiehlt es sich jedoch, nach Möglichkeit Medientyp-Formatierer zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7eb70-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="7eb70-120">Ein Hauptprinzip von http ist, dass Ressourcen im Nachrichtentext gesendet werden, wobei die Inhaltsaushandlung zum Angeben der Darstellung der Ressource verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="7eb70-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="7eb70-121">Medientyp-Formatierer wurden für genau diesen Zweck entwickelt.</span><span class="sxs-lookup"><span data-stu-id="7eb70-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="7eb70-122">Verwenden [fromuri]</span><span class="sxs-lookup"><span data-stu-id="7eb70-122">Using [FromUri]</span></span>

<span data-ttu-id="7eb70-123">Fügen Sie dem-Parameter das **[fromuri]** -Attribut hinzu, um zu erzwingen, dass die Web-API einen komplexen Typ aus dem URI liest.</span><span class="sxs-lookup"><span data-stu-id="7eb70-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="7eb70-124">Im folgenden Beispiel wird ein `GeoPoint` -Typ zusammen mit einer Controller Methode definiert, die `GeoPoint` den aus dem URI abruft.</span><span class="sxs-lookup"><span data-stu-id="7eb70-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="7eb70-125">Der Client kann die Werte für breiten-und Längengrade in der Abfrage Zeichenfolge ablegen, und die Web- `GeoPoint`API verwendet diese, um ein zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="7eb70-126">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7eb70-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="7eb70-127">Verwenden von [frombody]</span><span class="sxs-lookup"><span data-stu-id="7eb70-127">Using [FromBody]</span></span>

<span data-ttu-id="7eb70-128">Wenn Sie erzwingen möchten, dass die Web-API einen einfachen Typ aus dem Anforderungs Text liest, fügen Sie das Attribut **[frombody]** dem Parameter hinzu:</span><span class="sxs-lookup"><span data-stu-id="7eb70-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="7eb70-129">In diesem Beispiel verwendet die Web-API einen Medientyp-Formatierer, um den Wert von *Name* aus dem Anforderungs Text zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="7eb70-130">Im folgenden finden Sie ein Beispiel für eine Client Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7eb70-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="7eb70-131">Wenn ein Parameter [frombody] aufweist, verwendet die Web-API den Content-Type-Header, um einen Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="7eb70-132">In diesem Beispiel ist &quot;der Inhaltstyp "Application/JSON&quot; ", und der Anforderungs Text ist eine unformatierte JSON-Zeichenfolge (kein JSON-Objekt).</span><span class="sxs-lookup"><span data-stu-id="7eb70-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="7eb70-133">Höchstens ein Parameter darf aus dem Nachrichtentext gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="7eb70-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="7eb70-134">Dies funktioniert nicht:</span><span class="sxs-lookup"><span data-stu-id="7eb70-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="7eb70-135">Der Grund für diese Regel besteht darin, dass der Anforderungs Text möglicherweise in einem nicht gepufferten Stream gespeichert wird, der nur einmal gelesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="7eb70-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="7eb70-136">Typkonverter</span><span class="sxs-lookup"><span data-stu-id="7eb70-136">Type Converters</span></span>

<span data-ttu-id="7eb70-137">Sie können festlegen, dass die Web-API eine Klasse als einfachen Typ behandelt (sodass die Web-API versucht, Sie aus dem URI zu binden), indem Sie einen **TypeConverter** erstellen und eine Zeichen folgen Konvertierung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="7eb70-138">Der folgende Code zeigt eine `GeoPoint` Klasse, die einen geografischen Punkt darstellt, sowie einen **TypeConverter** , der von Zeichen `GeoPoint` folgen in-Instanzen konvertiert.</span><span class="sxs-lookup"><span data-stu-id="7eb70-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="7eb70-139">Die `GeoPoint` Klasse wird mit einem **[TypeConverter]** -Attribut versehen, um den Typkonverter anzugeben.</span><span class="sxs-lookup"><span data-stu-id="7eb70-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="7eb70-140">(Dieses Beispiel wurde vom Blogbeitrag von Mike Stall [zum Binden an benutzerdefinierte Objekte in Aktions Signaturen in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)inspiriert.)</span><span class="sxs-lookup"><span data-stu-id="7eb70-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="7eb70-141">Nun wird die Web- `GeoPoint` API als einfacher Typ behandelt, d. h., `GeoPoint` es wird versucht, Parameter aus dem URI zu binden.</span><span class="sxs-lookup"><span data-stu-id="7eb70-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="7eb70-142">Sie müssen **[fromuri]** nicht für den Parameter einschließen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="7eb70-143">Der Client kann die-Methode mit einem URI wie dem folgenden aufrufen:</span><span class="sxs-lookup"><span data-stu-id="7eb70-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="7eb70-144">Modellbinder</span><span class="sxs-lookup"><span data-stu-id="7eb70-144">Model Binders</span></span>

<span data-ttu-id="7eb70-145">Eine flexiblere Option als ein Typkonverter besteht darin, einen benutzerdefinierten Modell Binder zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="7eb70-146">Mit einem Modell Binder haben Sie Zugriff auf Dinge wie die HTTP-Anforderung, die Aktions Beschreibung und die Rohwerte aus den Routendaten.</span><span class="sxs-lookup"><span data-stu-id="7eb70-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="7eb70-147">Um einen Modell Binder zu erstellen, implementieren Sie die **IModelBinder** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="7eb70-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="7eb70-148">Diese Schnittstelle definiert eine einzige Methode, " **BindModel**":</span><span class="sxs-lookup"><span data-stu-id="7eb70-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="7eb70-149">Hier ist ein Modell Binder für `GeoPoint` -Objekte.</span><span class="sxs-lookup"><span data-stu-id="7eb70-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="7eb70-150">Ein Modell Binder ruft roheingabe Werte von einem *Wert Anbieter*ab.</span><span class="sxs-lookup"><span data-stu-id="7eb70-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="7eb70-151">Dieser Entwurf trennt zwei unterschiedliche Funktionen:</span><span class="sxs-lookup"><span data-stu-id="7eb70-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="7eb70-152">Der Wert Anbieter nimmt die HTTP-Anforderung an und füllt ein Wörterbuch von Schlüssel-Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="7eb70-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="7eb70-153">Der Modell Binder verwendet dieses Wörterbuch zum Auffüllen des Modells.</span><span class="sxs-lookup"><span data-stu-id="7eb70-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="7eb70-154">Der Standardwert Anbieter in der Web-API ruft Werte aus den Routendaten und der Abfrage Zeichenfolge ab.</span><span class="sxs-lookup"><span data-stu-id="7eb70-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="7eb70-155">Wenn der URI beispielsweise ist `http://localhost/api/values/1?location=48,-122`, erstellt der Wert Anbieter die folgenden Schlüssel-Wert-Paare:</span><span class="sxs-lookup"><span data-stu-id="7eb70-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="7eb70-156">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="7eb70-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="7eb70-157">Speicherort &quot;= 48.122&quot;</span><span class="sxs-lookup"><span data-stu-id="7eb70-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="7eb70-158">(Ich gehe von der Standardrouten Vorlage aus, die &quot;API/{Controller}/{ID}&quot;ist.)</span><span class="sxs-lookup"><span data-stu-id="7eb70-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="7eb70-159">Der Name des Parameters, der gebunden werden soll, wird in der **modelbindingcontext. modelname** -Eigenschaft gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7eb70-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="7eb70-160">Der Modell Binder sucht einen Schlüssel mit diesem Wert im Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="7eb70-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="7eb70-161">Wenn der Wert vorhanden ist und in einen `GeoPoint`konvertiert werden kann, weist der Modell Binder den gebundenen Wert der **modelbindingcontext. Model** -Eigenschaft zu.</span><span class="sxs-lookup"><span data-stu-id="7eb70-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="7eb70-162">Beachten Sie, dass der Modell Binder nicht auf eine einfache Typkonvertierung beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="7eb70-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="7eb70-163">In diesem Beispiel sucht der Modell Binder zuerst in einer Tabelle bekannter Speicherorte, und wenn dies nicht der Fall ist, wird die Typkonvertierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="7eb70-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="7eb70-164">**Festlegen der Modell Bindung**</span><span class="sxs-lookup"><span data-stu-id="7eb70-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="7eb70-165">Es gibt mehrere Möglichkeiten, einen Modell Binder festzulegen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="7eb70-166">Zuerst können Sie dem-Parameter ein **[Modelbinder]** -Attribut hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="7eb70-167">Sie können dem Typ auch ein **[Modelbinder]** -Attribut hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="7eb70-168">Die Web-API verwendet den angegebenen Modell Binder für alle Parameter dieses Typs.</span><span class="sxs-lookup"><span data-stu-id="7eb70-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="7eb70-169">Schließlich können Sie der **httpconfiguration**einen Modell Binder Anbieter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="7eb70-170">Ein Modell Binder Anbieter ist einfach eine Factoryklasse, die einen Modell Binder erstellt.</span><span class="sxs-lookup"><span data-stu-id="7eb70-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="7eb70-171">Sie können einen Anbieter erstellen, indem Sie von der [modelbinderprovider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) -Klasse ableiten.</span><span class="sxs-lookup"><span data-stu-id="7eb70-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="7eb70-172">Wenn Ihr Modell Binder jedoch einen einzelnen Typ behandelt, ist es einfacher, den integrierten **simplemodelbinderprovider**zu verwenden, der für diesen Zweck entwickelt wurde.</span><span class="sxs-lookup"><span data-stu-id="7eb70-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="7eb70-173">Dies wird im folgenden Code veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="7eb70-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="7eb70-174">Bei einem Modell Bindungs Anbieter müssen Sie dem Parameter immer noch das Attribut **[Modelbinder]** hinzufügen, um der Web-API mitzuteilen, dass Sie einen Modell Binder und keinen Medientyp Formatierer verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="7eb70-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="7eb70-175">Jetzt müssen Sie jedoch nicht den Typ des Modell Binders im Attribut angeben:</span><span class="sxs-lookup"><span data-stu-id="7eb70-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="7eb70-176">Wert Anbieter</span><span class="sxs-lookup"><span data-stu-id="7eb70-176">Value Providers</span></span>

<span data-ttu-id="7eb70-177">Ich habe erwähnt, dass ein Modell Binder Werte von einem Wert Anbieter abruft.</span><span class="sxs-lookup"><span data-stu-id="7eb70-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="7eb70-178">Um einen benutzerdefinierten Wert Anbieter zu schreiben, implementieren Sie die **IValueProvider** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="7eb70-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="7eb70-179">Es folgt ein Beispiel, das Werte aus den Cookies in der Anforderung abruft:</span><span class="sxs-lookup"><span data-stu-id="7eb70-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="7eb70-180">Sie müssen auch eine wertanbieterfactory erstellen, indem Sie von der **valueproviderfactory** -Klasse ableiten.</span><span class="sxs-lookup"><span data-stu-id="7eb70-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="7eb70-181">Fügen Sie der **httpconfiguration** die wertanbieterfactory wie folgt hinzu.</span><span class="sxs-lookup"><span data-stu-id="7eb70-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="7eb70-182">Die Web-API verfasst alle Wert Anbieter, d. h., wenn ein Modell Binder " **valueprovider. GetValue**" aufruft, empfängt der Modell Binder den Wert des ersten Wert Anbieters, der ihn liefern kann.</span><span class="sxs-lookup"><span data-stu-id="7eb70-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="7eb70-183">Alternativ können Sie die wertanbieterfactory auf Parameter Ebene festlegen, indem Sie das **valueprovider** -Attribut wie folgt verwenden:</span><span class="sxs-lookup"><span data-stu-id="7eb70-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="7eb70-184">Dies weist die Web-API an, die Modell Bindung mit der angegebenen wertanbieterfactory zu verwenden und keinen der anderen registrierten Wert Anbieter zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7eb70-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="7eb70-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="7eb70-185">HttpParameterBinding</span></span>

<span data-ttu-id="7eb70-186">Modell Binder sind eine bestimmte Instanz eines allgemeineren Mechanismus.</span><span class="sxs-lookup"><span data-stu-id="7eb70-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="7eb70-187">Wenn Sie sich das **[Modelbinder]** -Attribut ansehen, sehen Sie, dass es von der abstrakten **parameterbindingattribute** -Klasse abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="7eb70-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="7eb70-188">Diese Klasse definiert eine einzelne Methode, **GetBinding**, die ein **httpparameterbinding** -Objekt zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="7eb70-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="7eb70-189">Eine **httpparameterbinding** ist dafür verantwortlich, einen Parameter an einen Wert zu binden.</span><span class="sxs-lookup"><span data-stu-id="7eb70-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="7eb70-190">Im Fall von **[Modelbinder]** gibt das Attribut eine **httpparameterbinding** -Implementierung zurück, die einen **IModelBinder** verwendet, um die tatsächliche Bindung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="7eb70-191">Sie können auch Ihre eigene **httpparameterbinding**implementieren.</span><span class="sxs-lookup"><span data-stu-id="7eb70-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="7eb70-192">Nehmen Sie beispielsweise an, dass Sie Etags aus `if-match` - `if-none-match` und-Headern in der Anforderung erhalten möchten.</span><span class="sxs-lookup"><span data-stu-id="7eb70-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="7eb70-193">Wir beginnen mit der Definition einer Klasse zur Darstellung von Etags.</span><span class="sxs-lookup"><span data-stu-id="7eb70-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="7eb70-194">Wir definieren außerdem eine Enumeration, die angibt, ob das ETag aus dem `if-match` Header oder dem `if-none-match` Header erhalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="7eb70-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="7eb70-195">Im folgenden finden Sie eine **httpparameterbinding** , die das ETag aus dem gewünschten Header abruft und an einen Parameter des Typs ETag bindet:</span><span class="sxs-lookup"><span data-stu-id="7eb70-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="7eb70-196">Die **executebindingasync** -Methode führt die Bindung aus.</span><span class="sxs-lookup"><span data-stu-id="7eb70-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="7eb70-197">Fügen Sie in dieser Methode den gebundenen Parameterwert dem **Aktions Argument** -Wörterbuch in **httpactioncontext**hinzu.</span><span class="sxs-lookup"><span data-stu-id="7eb70-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="7eb70-198">Wenn die **executebindingasync** -Methode den Text der Anforderungs Nachricht liest, überschreiben Sie die **willread Body** -Eigenschaft, um true zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="7eb70-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="7eb70-199">Bei dem Anforderungs Text kann es sich um einen nicht gepufferten Datenstrom handeln, der nur einmal gelesen werden kann, sodass die Web-API eine Regel erzwingt, dass höchstens eine Bindung den Nachrichtentext lesen kann.</span><span class="sxs-lookup"><span data-stu-id="7eb70-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>

<span data-ttu-id="7eb70-200">Um ein benutzerdefiniertes **httpparameterbinding**-Attribut anzuwenden, können Sie ein Attribut definieren, das von **parameterbindingattribute**abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="7eb70-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="7eb70-201">Für `ETagParameterBinding`werden zwei Attribute definiert, eine für `if-match` Header und eine für `if-none-match` Header.</span><span class="sxs-lookup"><span data-stu-id="7eb70-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="7eb70-202">Beide werden von einer abstrakten Basisklasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="7eb70-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="7eb70-203">Im folgenden finden Sie eine Controller Methode, `[IfNoneMatch]` die das-Attribut verwendet.</span><span class="sxs-lookup"><span data-stu-id="7eb70-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="7eb70-204">Neben **parameterbindingattribute**gibt es einen anderen Hook zum Hinzufügen eines benutzerdefinierten **httpparameterbinding**-Objekts.</span><span class="sxs-lookup"><span data-stu-id="7eb70-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="7eb70-205">Im **httpconfiguration** -Objekt ist die **parameterbindingrules** -Eigenschaft eine Auflistung anonymer Funktionen vom Typ (**httpparameterdescriptor**  - &gt; **httpparameterbinding**).</span><span class="sxs-lookup"><span data-stu-id="7eb70-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="7eb70-206">Beispielsweise können Sie eine Regel hinzufügen, die von jedem ETag-Parameter einer get `ETagParameterBinding` - `if-none-match`Methode mit verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="7eb70-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="7eb70-207">Die Funktion sollte für `null` Parameter zurückgeben, bei denen die Bindung nicht anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="7eb70-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="7eb70-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="7eb70-208">IActionValueBinder</span></span>

<span data-ttu-id="7eb70-209">Der gesamte Parameter Bindungsprozess wird durch einen austauschbaren Dienst ( **iaktionvaluebinder**) gesteuert.</span><span class="sxs-lookup"><span data-stu-id="7eb70-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="7eb70-210">Die Standard Implementierung von **iaktionvaluebinder** führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="7eb70-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="7eb70-211">Suchen Sie nach einem **parameterbindingattribute** -Parameter.</span><span class="sxs-lookup"><span data-stu-id="7eb70-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="7eb70-212">Dies umfasst **[frombody]** , **[fromuri]** und **[Modelbinder]** oder benutzerdefinierte Attribute.</span><span class="sxs-lookup"><span data-stu-id="7eb70-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="7eb70-213">Suchen Sie andernfalls in **httpconfiguration. parameterbindingrules** nach einer Funktion, die einen **httpparameterbinding**-Wert zurückgibt, der nicht NULL ist.</span><span class="sxs-lookup"><span data-stu-id="7eb70-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="7eb70-214">Verwenden Sie andernfalls die Standardregeln, die Sie zuvor beschrieben haben.</span><span class="sxs-lookup"><span data-stu-id="7eb70-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="7eb70-215">Wenn der Parametertyp "Simple" oder ein Typkonverter ist, binden Sie den URI aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="7eb70-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="7eb70-216">Dies entspricht dem Platzieren des **[fromuri]** -Attributs für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7eb70-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="7eb70-217">Versuchen Sie andernfalls, den-Parameter aus dem Nachrichtentext zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="7eb70-218">Dies entspricht dem Einfügen von **[frombody]** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7eb70-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="7eb70-219">Wenn Sie möchten, können Sie den gesamten **iaktionvaluebinder** -Dienst durch eine benutzerdefinierte Implementierung ersetzen.</span><span class="sxs-lookup"><span data-stu-id="7eb70-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7eb70-220">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7eb70-220">Additional Resources</span></span>

[<span data-ttu-id="7eb70-221">Beispiel für benutzerdefinierte Parameter Bindung</span><span class="sxs-lookup"><span data-stu-id="7eb70-221">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="7eb70-222">Mike Stall hat eine gute Reihe von Blogbeiträgen zur Web-API-Parameter Bindung verfasst:</span><span class="sxs-lookup"><span data-stu-id="7eb70-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="7eb70-223">Funktionsweise der Parameter Bindung durch die Web-API</span><span class="sxs-lookup"><span data-stu-id="7eb70-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="7eb70-224">MVC-Stil Parameter Bindung für Web-API</span><span class="sxs-lookup"><span data-stu-id="7eb70-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="7eb70-225">Binden an benutzerdefinierte Objekte in Aktions Signaturen in MVC/Web-API</span><span class="sxs-lookup"><span data-stu-id="7eb70-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="7eb70-226">Erstellen eines benutzerdefinierten Wert Anbieters in der Web-API</span><span class="sxs-lookup"><span data-stu-id="7eb70-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="7eb70-227">Web-API-Parameter Bindung im Hintergrund</span><span class="sxs-lookup"><span data-stu-id="7eb70-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
