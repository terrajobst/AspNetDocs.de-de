---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attribut Routing in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446997"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="14477-102">Attribut Routing in ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="14477-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="14477-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="14477-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="14477-104">Das *Routing* ist die Art und Weise, wie die Web-API einem URI eine Aktion entspricht</span><span class="sxs-lookup"><span data-stu-id="14477-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="14477-105">Web-API 2 unterstützt eine neue Art von Routing namens *Attribut Routing*.</span><span class="sxs-lookup"><span data-stu-id="14477-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="14477-106">Wie der Name schon sagt, verwendet das Attribut Routing Attribute zum Definieren von Routen.</span><span class="sxs-lookup"><span data-stu-id="14477-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="14477-107">Das Attribut Routing ermöglicht Ihnen mehr Kontrolle über die URIs in Ihrer Web-API.</span><span class="sxs-lookup"><span data-stu-id="14477-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="14477-108">Beispielsweise können Sie ganz einfach URIs erstellen, die Ressourcen Hierarchien beschreiben.</span><span class="sxs-lookup"><span data-stu-id="14477-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="14477-109">Der frühere Routing Stil, der als Konvention-basiertes Routing bezeichnet wird, wird weiterhin vollständig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="14477-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="14477-110">Tatsächlich können Sie beide Techniken im gleichen Projekt kombinieren.</span><span class="sxs-lookup"><span data-stu-id="14477-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="14477-111">In diesem Thema wird gezeigt, wie das Attribut Routing aktiviert wird und die verschiedenen Optionen für das Attribut Routing beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="14477-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="14477-112">Ein End-to-End-Tutorial, in dem Attribut Routing verwendet wird, finden Sie unter [Erstellen einer Rest-API mit Attribut Routing in der Web-API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="14477-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="14477-113">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="14477-113">Prerequisites</span></span>

<span data-ttu-id="14477-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="14477-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="14477-115">Verwenden Sie alternativ den nuget-Paket-Manager, um die erforderlichen Pakete zu installieren.</span><span class="sxs-lookup"><span data-stu-id="14477-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="14477-116">Wählen Sie **im Menü Extras** in Visual Studio **nuget-Paket-Manager**und dann **Paket-Manager-Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="14477-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="14477-117">Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="14477-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="14477-118">Gründe für Attribut Routing</span><span class="sxs-lookup"><span data-stu-id="14477-118">Why Attribute Routing?</span></span>

<span data-ttu-id="14477-119">In der ersten Version der Web-API wurde ein *Konventionen basiertes* Routing verwendet.</span><span class="sxs-lookup"><span data-stu-id="14477-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="14477-120">Bei dieser Art von Routing definieren Sie eine oder mehrere Routen Vorlagen, bei denen es sich im Grunde um parametrisierte Zeichen folgen handelt.</span><span class="sxs-lookup"><span data-stu-id="14477-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="14477-121">Wenn das Framework eine Anforderung empfängt, entspricht es dem URI für die Routen Vorlage.</span><span class="sxs-lookup"><span data-stu-id="14477-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="14477-122">(Weitere Informationen zum Konventionen basierten Routing finden Sie unter [Routing in ASP.net-Web-API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="14477-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="14477-123">Ein Vorteil des Konventionen basierten Routings besteht darin, dass Vorlagen an einer zentralen Stelle definiert werden und die Routing Regeln einheitlich auf alle Controller angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="14477-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="14477-124">Leider ist es bei einem Konventionen basierten Routing schwierig, bestimmte URI-Muster zu unterstützen, die in Rest-APIs häufig vorkommen.</span><span class="sxs-lookup"><span data-stu-id="14477-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="14477-125">Beispielsweise enthalten Ressourcen häufig untergeordnete Ressourcen: Kunden haben Aufträge, Filme haben Actors, Bücher haben Autoren und so weiter.</span><span class="sxs-lookup"><span data-stu-id="14477-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="14477-126">Es ist natürlich, URIs zu erstellen, die diese Beziehungen widerspiegeln:</span><span class="sxs-lookup"><span data-stu-id="14477-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="14477-127">Diese Art von URI ist schwierig zu erstellen, wenn ein Konventionen-basiertes Routing verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="14477-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="14477-128">Obwohl es möglich ist, können die Ergebnisse nicht gut skaliert werden, wenn Sie über viele Controller oder Ressourcentypen verfügen.</span><span class="sxs-lookup"><span data-stu-id="14477-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="14477-129">Beim Attribut Routing ist es trivial, eine Route für diesen URI zu definieren.</span><span class="sxs-lookup"><span data-stu-id="14477-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="14477-130">Sie fügen der Controller Aktion einfach ein Attribut hinzu:</span><span class="sxs-lookup"><span data-stu-id="14477-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="14477-131">Im folgenden finden Sie einige andere Muster, die das Attribut Routing leicht macht.</span><span class="sxs-lookup"><span data-stu-id="14477-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="14477-132">**API-Versionsverwaltung**</span><span class="sxs-lookup"><span data-stu-id="14477-132">**API versioning**</span></span>

<span data-ttu-id="14477-133">In diesem Beispiel würde "/API/v1/Products" an einen anderen Controller als "/API/v2/Products" weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="14477-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="14477-134">**Überladene URI Segmente**</span><span class="sxs-lookup"><span data-stu-id="14477-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="14477-135">In diesem Beispiel ist "1" eine Bestellnummer, aber "Pending" wird einer Auflistung zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="14477-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="14477-136">**Mehrere Parametertypen**</span><span class="sxs-lookup"><span data-stu-id="14477-136">**Multiple parameter types**</span></span>

<span data-ttu-id="14477-137">In diesem Beispiel ist "1" eine Bestellnummer, aber "2013/06/16" gibt ein Datum an.</span><span class="sxs-lookup"><span data-stu-id="14477-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="14477-138">Aktivieren von Attribut Routing</span><span class="sxs-lookup"><span data-stu-id="14477-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="14477-139">Um das Attribut Routing zu aktivieren, wird **maphttpattributeroutes** während der Konfiguration aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="14477-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="14477-140">Diese Erweiterungsmethode wird in der **System. Web. http. httpconfigurationextensions** -Klasse definiert.</span><span class="sxs-lookup"><span data-stu-id="14477-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="14477-141">Das Attribut Routing kann mit einem [Konvention-basierten](routing-in-aspnet-web-api.md) Routing kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="14477-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="14477-142">Um auf Konventionen basierende Routen zu definieren, nennen Sie die **maphttproute** -Methode.</span><span class="sxs-lookup"><span data-stu-id="14477-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="14477-143">Weitere Informationen zum Konfigurieren der Web-API finden Sie unter [Konfigurieren von ASP.net-Web-API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="14477-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="14477-144">Hinweis: Migrieren von der Web-API 1</span><span class="sxs-lookup"><span data-stu-id="14477-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="14477-145">Vor der Web-API 2 hat die Web-API-Projektvorlagen Code wie den folgenden generiert:</span><span class="sxs-lookup"><span data-stu-id="14477-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="14477-146">Wenn das Attribut Routing aktiviert ist, löst dieser Code eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="14477-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="14477-147">Wenn Sie ein vorhandenes Web-API-Projekt aktualisieren, um Attribut Routing zu verwenden, stellen Sie sicher, dass Sie diesen Konfigurations Code wie folgt aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="14477-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="14477-148">Weitere Informationen finden Sie unter [Konfigurieren der Web-API mit ASP.net-Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="14477-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="14477-149">Hinzufügen von Routen Attributen</span><span class="sxs-lookup"><span data-stu-id="14477-149">Adding Route Attributes</span></span>

<span data-ttu-id="14477-150">Im folgenden finden Sie ein Beispiel für eine Route, die mit einem-Attribut definiert wird:</span><span class="sxs-lookup"><span data-stu-id="14477-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="14477-151">Die Zeichenfolge &quot;Customers/{CustomerID}/Orders&quot; ist die URI-Vorlage für die Route.</span><span class="sxs-lookup"><span data-stu-id="14477-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="14477-152">Die Web-API versucht, den Anforderungs-URI mit der Vorlage zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="14477-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="14477-153">In diesem Beispiel sind "Customers" und "Orders" literalsegmente, und "{CustomerID}" ist ein Variablen Parameter.</span><span class="sxs-lookup"><span data-stu-id="14477-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="14477-154">Die folgenden URIs entsprechen dieser Vorlage:</span><span class="sxs-lookup"><span data-stu-id="14477-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="14477-155">Sie können die Übereinstimmung mithilfe von [Einschränkungen](#constraints)einschränken, die weiter unten in diesem Thema beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="14477-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="14477-156">Beachten Sie, dass der &quot;{CustomerID}&quot; Parameter in der Routen Vorlage mit dem Namen des *CustomerID-* Parameters in der-Methode übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="14477-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="14477-157">Wenn die Web-API die Controller Aktion aufruft, versucht Sie, die Routen Parameter zu binden.</span><span class="sxs-lookup"><span data-stu-id="14477-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="14477-158">Wenn der URI z. b. `http://example.com/customers/1/orders`ist, versucht die Web-API, den Wert "1" an den Parameter " *CustomerID* " in der Aktion zu binden.</span><span class="sxs-lookup"><span data-stu-id="14477-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="14477-159">Eine URI-Vorlage kann mehrere Parameter aufweisen:</span><span class="sxs-lookup"><span data-stu-id="14477-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="14477-160">Alle Controller Methoden, die nicht über ein Routen Attribut verfügen, verwenden ein auf Konventionen basierendes Routing.</span><span class="sxs-lookup"><span data-stu-id="14477-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="14477-161">Auf diese Weise können Sie beide Routing Typen im selben Projekt kombinieren.</span><span class="sxs-lookup"><span data-stu-id="14477-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="14477-162">HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="14477-162">HTTP Methods</span></span>

<span data-ttu-id="14477-163">Die Web-API wählt auch Aktionen basierend auf der HTTP-Methode der Anforderung aus (Get, Post usw.).</span><span class="sxs-lookup"><span data-stu-id="14477-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="14477-164">Standardmäßig sucht die Web-API nach einer Entsprechung ohne Beachtung der Groß-/Kleinschreibung mit dem Anfang des Controller Methoden namens.</span><span class="sxs-lookup"><span data-stu-id="14477-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="14477-165">Beispielsweise entspricht eine Controller Methode namens `PutCustomers` einer HTTP PUT-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="14477-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="14477-166">Sie können diese Konvention überschreiben, indem Sie die-Methode mit den folgenden Attributen versehen:</span><span class="sxs-lookup"><span data-stu-id="14477-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="14477-167">**HttpDelete**</span><span class="sxs-lookup"><span data-stu-id="14477-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="14477-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="14477-168">**[HttpGet]**</span></span>
- <span data-ttu-id="14477-169">**[Httphead]**</span><span class="sxs-lookup"><span data-stu-id="14477-169">**[HttpHead]**</span></span>
- <span data-ttu-id="14477-170">**[Httpoptions]**</span><span class="sxs-lookup"><span data-stu-id="14477-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="14477-171">**[Httppatch]**</span><span class="sxs-lookup"><span data-stu-id="14477-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="14477-172">**HttpPost**</span><span class="sxs-lookup"><span data-stu-id="14477-172">**[HttpPost]**</span></span>
- <span data-ttu-id="14477-173">**HttpPut**</span><span class="sxs-lookup"><span data-stu-id="14477-173">**[HttpPut]**</span></span>

<span data-ttu-id="14477-174">Im folgenden Beispiel wird die Methode "foratebook" http Post-Anforderungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="14477-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="14477-175">Verwenden Sie für alle anderen HTTP-Methoden, einschließlich nicht-standardmäßiger Methoden, das Attribut " **Accept tverbs** ", das eine Liste von HTTP-Methoden annimmt.</span><span class="sxs-lookup"><span data-stu-id="14477-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="14477-176">Routen Präfixe</span><span class="sxs-lookup"><span data-stu-id="14477-176">Route Prefixes</span></span>

<span data-ttu-id="14477-177">Häufig beginnen die Routen in einem Controller mit dem gleichen Präfix.</span><span class="sxs-lookup"><span data-stu-id="14477-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="14477-178">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="14477-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="14477-179">Sie können ein gemeinsames Präfix für einen gesamten Controller mit dem **[routeprefix]** -Attribut festlegen:</span><span class="sxs-lookup"><span data-stu-id="14477-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="14477-180">Verwenden Sie eine Tilde (~) für das Method-Attribut, um das Routen Präfix zu überschreiben:</span><span class="sxs-lookup"><span data-stu-id="14477-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="14477-181">Das Routen Präfix kann Parameter enthalten:</span><span class="sxs-lookup"><span data-stu-id="14477-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="14477-182">Routen Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="14477-182">Route Constraints</span></span>

<span data-ttu-id="14477-183">Mit Routen Einschränkungen können Sie einschränken, wie die Parameter in der Routen Vorlage abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="14477-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="14477-184">Die allgemeine Syntax ist &quot;{Parameter: Einschränkung}&quot;.</span><span class="sxs-lookup"><span data-stu-id="14477-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="14477-185">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="14477-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="14477-186">Hier wird die erste Route nur ausgewählt, wenn die &quot;-ID&quot; Segment des URIs eine ganze Zahl ist.</span><span class="sxs-lookup"><span data-stu-id="14477-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="14477-187">Andernfalls wird die zweite Route ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="14477-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="14477-188">In der folgenden Tabelle sind die unterstützten Einschränkungen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="14477-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="14477-189">Constraint</span><span class="sxs-lookup"><span data-stu-id="14477-189">Constraint</span></span> | <span data-ttu-id="14477-190">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="14477-190">Description</span></span> | <span data-ttu-id="14477-191">Beispiel</span><span class="sxs-lookup"><span data-stu-id="14477-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="14477-192">Alpha</span><span class="sxs-lookup"><span data-stu-id="14477-192">alpha</span></span> | <span data-ttu-id="14477-193">Entspricht groß-oder Kleinbuchstaben lateinische Alphabet Zeichen (a-z, a-z)</span><span class="sxs-lookup"><span data-stu-id="14477-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="14477-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="14477-194">{x:alpha}</span></span> |
| <span data-ttu-id="14477-195">bool</span><span class="sxs-lookup"><span data-stu-id="14477-195">bool</span></span> | <span data-ttu-id="14477-196">Entspricht einem booleschen Wert.</span><span class="sxs-lookup"><span data-stu-id="14477-196">Matches a Boolean value.</span></span> | <span data-ttu-id="14477-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="14477-197">{x:bool}</span></span> |
| <span data-ttu-id="14477-198">datetime</span><span class="sxs-lookup"><span data-stu-id="14477-198">datetime</span></span> | <span data-ttu-id="14477-199">Entspricht einem **DateTime** -Wert.</span><span class="sxs-lookup"><span data-stu-id="14477-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="14477-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="14477-200">{x:datetime}</span></span> |
| <span data-ttu-id="14477-201">Decimal</span><span class="sxs-lookup"><span data-stu-id="14477-201">decimal</span></span> | <span data-ttu-id="14477-202">Entspricht einem Dezimalwert.</span><span class="sxs-lookup"><span data-stu-id="14477-202">Matches a decimal value.</span></span> | <span data-ttu-id="14477-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="14477-203">{x:decimal}</span></span> |
| <span data-ttu-id="14477-204">Doppelt</span><span class="sxs-lookup"><span data-stu-id="14477-204">double</span></span> | <span data-ttu-id="14477-205">Entspricht einem 64-Bit-Gleit Komma Wert.</span><span class="sxs-lookup"><span data-stu-id="14477-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="14477-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="14477-206">{x:double}</span></span> |
| <span data-ttu-id="14477-207">float</span><span class="sxs-lookup"><span data-stu-id="14477-207">float</span></span> | <span data-ttu-id="14477-208">Entspricht einem 32-Bit-Gleit Komma Wert.</span><span class="sxs-lookup"><span data-stu-id="14477-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="14477-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="14477-209">{x:float}</span></span> |
| <span data-ttu-id="14477-210">guid</span><span class="sxs-lookup"><span data-stu-id="14477-210">guid</span></span> | <span data-ttu-id="14477-211">Entspricht einem GUID-Wert.</span><span class="sxs-lookup"><span data-stu-id="14477-211">Matches a GUID value.</span></span> | <span data-ttu-id="14477-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="14477-212">{x:guid}</span></span> |
| <span data-ttu-id="14477-213">int</span><span class="sxs-lookup"><span data-stu-id="14477-213">int</span></span> | <span data-ttu-id="14477-214">Entspricht einem ganzzahligen Wert von 32 Bit.</span><span class="sxs-lookup"><span data-stu-id="14477-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="14477-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="14477-215">{x:int}</span></span> |
| <span data-ttu-id="14477-216">Länge</span><span class="sxs-lookup"><span data-stu-id="14477-216">length</span></span> | <span data-ttu-id="14477-217">Entspricht einer Zeichenfolge mit der angegebenen Länge oder innerhalb eines angegebenen Längen Bereichs.</span><span class="sxs-lookup"><span data-stu-id="14477-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="14477-218">{x:length (6)} {x:length (1, 20)}</span><span class="sxs-lookup"><span data-stu-id="14477-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="14477-219">long</span><span class="sxs-lookup"><span data-stu-id="14477-219">long</span></span> | <span data-ttu-id="14477-220">Entspricht einem ganzzahligen Wert von 64 Bit.</span><span class="sxs-lookup"><span data-stu-id="14477-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="14477-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="14477-221">{x:long}</span></span> |
| <span data-ttu-id="14477-222">max</span><span class="sxs-lookup"><span data-stu-id="14477-222">max</span></span> | <span data-ttu-id="14477-223">Entspricht einer ganzen Zahl mit einem maximalen Wert.</span><span class="sxs-lookup"><span data-stu-id="14477-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="14477-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="14477-224">{x:max(10)}</span></span> |
| <span data-ttu-id="14477-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="14477-225">maxlength</span></span> | <span data-ttu-id="14477-226">Entspricht einer Zeichenfolge mit einer maximalen Länge.</span><span class="sxs-lookup"><span data-stu-id="14477-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="14477-227">{x:maxLength (10)}</span><span class="sxs-lookup"><span data-stu-id="14477-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="14477-228">Min.</span><span class="sxs-lookup"><span data-stu-id="14477-228">min</span></span> | <span data-ttu-id="14477-229">Entspricht einem Integer-Wert mit einem Minimalwert.</span><span class="sxs-lookup"><span data-stu-id="14477-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="14477-230">{x:min (10)}</span><span class="sxs-lookup"><span data-stu-id="14477-230">{x:min(10)}</span></span> |
| <span data-ttu-id="14477-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="14477-231">minlength</span></span> | <span data-ttu-id="14477-232">Entspricht einer Zeichenfolge mit einer Mindestlänge.</span><span class="sxs-lookup"><span data-stu-id="14477-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="14477-233">{x:minLength (10)}</span><span class="sxs-lookup"><span data-stu-id="14477-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="14477-234">range</span><span class="sxs-lookup"><span data-stu-id="14477-234">range</span></span> | <span data-ttu-id="14477-235">Entspricht einer ganzen Zahl innerhalb eines Wertebereichs.</span><span class="sxs-lookup"><span data-stu-id="14477-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="14477-236">{x:Range (10, 50)}</span><span class="sxs-lookup"><span data-stu-id="14477-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="14477-237">regex</span><span class="sxs-lookup"><span data-stu-id="14477-237">regex</span></span> | <span data-ttu-id="14477-238">Entspricht einem regulären Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="14477-238">Matches a regular expression.</span></span> | <span data-ttu-id="14477-239">{x:Regex (^ \d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="14477-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="14477-240">Beachten Sie, dass einige der Einschränkungen, z. b. &quot;min&quot;, Argumente in Klammern setzen.</span><span class="sxs-lookup"><span data-stu-id="14477-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="14477-241">Sie können mehrere Einschränkungen auf einen Parameter anwenden, getrennt durch einen Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="14477-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="14477-242">Benutzerdefinierte Routeneinschränkungen</span><span class="sxs-lookup"><span data-stu-id="14477-242">Custom Route Constraints</span></span>

<span data-ttu-id="14477-243">Sie können benutzerdefinierte Routen Einschränkungen erstellen, indem Sie die **ihttprouteeinschränkung** -Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="14477-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="14477-244">Beispielsweise schränkt die folgende Einschränkung einen Parameter auf einen ganzzahligen Wert ungleich 0 (null) ein.</span><span class="sxs-lookup"><span data-stu-id="14477-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="14477-245">Der folgende Code zeigt, wie Sie die Einschränkung registrieren:</span><span class="sxs-lookup"><span data-stu-id="14477-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="14477-246">Nun können Sie die Einschränkung in Ihren Routen anwenden:</span><span class="sxs-lookup"><span data-stu-id="14477-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="14477-247">Sie können auch die gesamte **defaultinlineconstraintresolver** -Klasse ersetzen, indem Sie die **iinlineconstraintresolver** -Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="14477-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="14477-248">Dadurch werden alle integrierten Einschränkungen ersetzt, es sei denn, ihre Implementierung von **iinlineconstraintresolver** fügt Sie explizit hinzu.</span><span class="sxs-lookup"><span data-stu-id="14477-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="14477-249">Optionale URI-Parameter und Standardwerte</span><span class="sxs-lookup"><span data-stu-id="14477-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="14477-250">Sie können einen URI-Parameter optional machen, indem Sie dem Routen Parameter ein Fragezeichen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="14477-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="14477-251">Wenn ein Routen Parameter optional ist, müssen Sie einen Standardwert für den Methoden Parameter definieren.</span><span class="sxs-lookup"><span data-stu-id="14477-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="14477-252">In diesem Beispiel geben `/api/books/locale/1033` und `/api/books/locale` dieselbe Ressource zurück.</span><span class="sxs-lookup"><span data-stu-id="14477-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="14477-253">Alternativ können Sie einen Standardwert in der Routen Vorlage wie folgt angeben:</span><span class="sxs-lookup"><span data-stu-id="14477-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="14477-254">Dies ist fast identisch mit dem vorherigen Beispiel, aber es gibt einen geringfügigen Unterschied des Verhaltens, wenn der Standardwert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="14477-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="14477-255">Im ersten Beispiel ("{LCID: int?}") wird der Standardwert 1033 direkt dem-Methoden Parameter zugewiesen, sodass der-Parameter diesen exakten Wert erhält.</span><span class="sxs-lookup"><span data-stu-id="14477-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="14477-256">Im zweiten Beispiel ("{LCID: int = 1033}") durchläuft der Standardwert "1033" den Modell Bindungsprozess.</span><span class="sxs-lookup"><span data-stu-id="14477-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="14477-257">Der Standardmodell Binder konvertiert "1033" in den numerischen Wert 1033.</span><span class="sxs-lookup"><span data-stu-id="14477-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="14477-258">Allerdings könnten Sie einen benutzerdefinierten Modell Binder einbinden, der etwas anderes Unternehmen kann.</span><span class="sxs-lookup"><span data-stu-id="14477-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="14477-259">(In den meisten Fällen sind die beiden Formulare äquivalent, es sei denn, Sie haben in ihrer Pipeline benutzerdefinierte Modell Binder.)</span><span class="sxs-lookup"><span data-stu-id="14477-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="14477-260">Routennamen</span><span class="sxs-lookup"><span data-stu-id="14477-260">Route Names</span></span>

<span data-ttu-id="14477-261">In der Web-API weist jede Route einen Namen auf.</span><span class="sxs-lookup"><span data-stu-id="14477-261">In Web API, every route has a name.</span></span> <span data-ttu-id="14477-262">Routennamen sind nützlich, um Links zu erstellen, sodass Sie einen Link in eine HTTP-Antwort einfügen können.</span><span class="sxs-lookup"><span data-stu-id="14477-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="14477-263">Um den Routennamen anzugeben, legen Sie die **Name** -Eigenschaft für das-Attribut fest.</span><span class="sxs-lookup"><span data-stu-id="14477-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="14477-264">Im folgenden Beispiel wird gezeigt, wie der Routen Name festgelegt wird und wie der Routen Name beim Erstellen eines Links verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="14477-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="14477-265">Routen Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="14477-265">Route Order</span></span>

<span data-ttu-id="14477-266">Wenn das Framework versucht, einen URI mit einer Route abzugleichen, wertet es die Routen in einer bestimmten Reihenfolge aus.</span><span class="sxs-lookup"><span data-stu-id="14477-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="14477-267">Um die Reihenfolge anzugeben, legen Sie die **Order** -Eigenschaft für das Routen Attribut fest.</span><span class="sxs-lookup"><span data-stu-id="14477-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="14477-268">Niedrigere Werte werden zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="14477-268">Lower values are evaluated first.</span></span> <span data-ttu-id="14477-269">Der Standard Reihen folgen Wert ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="14477-269">The default order value is zero.</span></span>

<span data-ttu-id="14477-270">So wird die Gesamt Reihenfolge bestimmt:</span><span class="sxs-lookup"><span data-stu-id="14477-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="14477-271">Vergleichen Sie die **Order** -Eigenschaft des Route-Attributs.</span><span class="sxs-lookup"><span data-stu-id="14477-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="14477-272">Sehen Sie sich die einzelnen URI-Segmente in der Routen Vorlage an.</span><span class="sxs-lookup"><span data-stu-id="14477-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="14477-273">Sortieren Sie für jedes Segment folgende Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="14477-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="14477-274">Literale Segmente.</span><span class="sxs-lookup"><span data-stu-id="14477-274">Literal segments.</span></span>
    2. <span data-ttu-id="14477-275">Routen Parameter mit Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="14477-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="14477-276">Routen Parameter ohne Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="14477-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="14477-277">Platzhalter Parameter Segmente mit Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="14477-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="14477-278">Platzhalter Parameter Segmente ohne Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="14477-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="14477-279">Im Fall eines gleich Zeichens werden Routen durch einen Ordinalzeichenfolgenvergleich ohne Berücksichtigung der Groß-/Kleinschreibung ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) der Routen Vorlage geordnet.</span><span class="sxs-lookup"><span data-stu-id="14477-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="14477-280">Im Folgenden ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="14477-280">Here is an example.</span></span> <span data-ttu-id="14477-281">Angenommen, Sie definieren den folgenden Controller:</span><span class="sxs-lookup"><span data-stu-id="14477-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="14477-282">Diese Routen werden wie folgt angeordnet.</span><span class="sxs-lookup"><span data-stu-id="14477-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="14477-283">Bestellungen/Details</span><span class="sxs-lookup"><span data-stu-id="14477-283">orders/details</span></span>
2. <span data-ttu-id="14477-284">Orders/{ID}</span><span class="sxs-lookup"><span data-stu-id="14477-284">orders/{id}</span></span>
3. <span data-ttu-id="14477-285">Orders/{CustomerName}</span><span class="sxs-lookup"><span data-stu-id="14477-285">orders/{customerName}</span></span>
4. <span data-ttu-id="14477-286">Orders/{\*Datum}</span><span class="sxs-lookup"><span data-stu-id="14477-286">orders/{\*date}</span></span>
5. <span data-ttu-id="14477-287">Aufträge/ausstehend</span><span class="sxs-lookup"><span data-stu-id="14477-287">orders/pending</span></span>

<span data-ttu-id="14477-288">Beachten Sie, dass "Details" ein literales Segment ist und vor "{ID}" angezeigt wird, aber "Pending" (ausstehend) angezeigt wird, da die **Order** -Eigenschaft 1 ist.</span><span class="sxs-lookup"><span data-stu-id="14477-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="14477-289">(In diesem Beispiel wird davon ausgegangen, dass keine Kunden mit dem Namen "Details" oder "Pending" vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="14477-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="14477-290">Vermeiden Sie im Allgemeinen, mehrdeutige Routen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="14477-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="14477-291">In diesem Beispiel ist eine bessere Routen Vorlage für `GetByCustomer` "Customers/{CustomerName}").</span><span class="sxs-lookup"><span data-stu-id="14477-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
