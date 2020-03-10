---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing-und Aktions Auswahl in ASP.net-Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446913"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="e4ea4-102">Routing-und Aktions Auswahl in ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="e4ea4-102">Routing and Action Selection in ASP.NET Web API</span></span>

<span data-ttu-id="e4ea4-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e4ea4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e4ea4-104">In diesem Artikel wird beschrieben, wie ASP.net-Web-API eine HTTP-Anforderung an eine bestimmte Aktion auf einem Controller weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ea4-105">Eine allgemeine Übersicht über das Routing finden Sie unter [Routing in ASP.net-Web-API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea4-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="e4ea4-106">In diesem Artikel werden die Details des Routing Prozesses erläutert.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="e4ea4-107">Wenn Sie ein Web-API-Projekt erstellen und feststellen, dass einige Anforderungen nicht erwartungsgemäß weitergeleitet werden, wird Ihnen dieser Artikel hoffentlich helfen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="e4ea4-108">Das Routing umfasst drei Hauptphasen:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="e4ea4-109">Übereinstimmung des URIs mit einer Routen Vorlage.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="e4ea4-110">Auswählen eines Controllers.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-110">Selecting a controller.</span></span>
3. <span data-ttu-id="e4ea4-111">Auswählen einer Aktion.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-111">Selecting an action.</span></span>

<span data-ttu-id="e4ea4-112">Sie können einige Teile des Prozesses durch ihre eigenen benutzerdefinierten Verhaltensweisen ersetzen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="e4ea4-113">In diesem Artikel wird das Standardverhalten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="e4ea4-114">Am Ende notieren Sie die stellen, an denen Sie das Verhalten anpassen können.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="e4ea4-115">Routen Vorlagen</span><span class="sxs-lookup"><span data-stu-id="e4ea4-115">Route Templates</span></span>

<span data-ttu-id="e4ea4-116">Eine Routen Vorlage ähnelt einem URI-Pfad, kann jedoch Platzhalter Werte enthalten, die mit geschweiften Klammern angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="e4ea4-117">Wenn Sie eine Route erstellen, können Sie Standardwerte für einige oder alle Platzhalter angeben:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="e4ea4-118">Sie können auch Einschränkungen bereitstellen, die einschränken, wie ein URI-Segment einem Platzhalter entsprechen kann:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="e4ea4-119">Das Framework versucht, die Segmente im URI-Pfad mit der Vorlage abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="e4ea4-120">Literale in der Vorlage müssen exakt übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="e4ea4-121">Ein Platzhalter entspricht einem beliebigen Wert, es sei denn, Sie geben Einschränkungen an.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="e4ea4-122">Das Framework stimmt nicht mit anderen Teilen des URI, wie z. b. dem Hostnamen oder den Abfrage Parametern, ab.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="e4ea4-123">Das Framework wählt die erste Route in der Routing Tabelle aus, die dem URI entspricht.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="e4ea4-124">Es gibt zwei spezielle Platzhalter: "{controller}" und "{action}".</span><span class="sxs-lookup"><span data-stu-id="e4ea4-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="e4ea4-125">"{controller}" gibt den Namen des Controllers an.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="e4ea4-126">"{action}" gibt den Namen der Aktion an.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="e4ea4-127">In der Web-API besteht die übliche Konvention darin, "{action}" auszulassen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="e4ea4-128">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="e4ea4-128">Defaults</span></span>

<span data-ttu-id="e4ea4-129">Wenn Sie Standardwerte angeben, entspricht die Route einem URI, der diese Segmente fehlt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="e4ea4-130">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="e4ea4-131">Die URIs `http://localhost/api/products/all` und `http://localhost/api/products` der vorangehenden Route entsprechen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="e4ea4-132">Im letzteren URI wird dem fehlenden `{category}` Segment der Standardwert `all`zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="e4ea4-133">Routen Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="e4ea4-133">Route Dictionary</span></span>

<span data-ttu-id="e4ea4-134">Wenn das Framework eine Entsprechung für einen URI findet, wird ein Wörterbuch erstellt, das den Wert für jeden Platzhalter enthält.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="e4ea4-135">Bei den Schlüsseln handelt es sich um die Platzhalter Namen, ohne die geschweiften Klammern.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="e4ea4-136">Die Werte werden aus dem URI-Pfad oder aus den Standardwerten entnommen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="e4ea4-137">Das Wörterbuch wird im **ihttproutedata** -Objekt gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="e4ea4-138">Während dieser Weiterleitungs Übereinstimmungs Phase werden die besonderen Platzhalter "{controller}" und "{action}" genau wie die anderen Platzhalter behandelt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="e4ea4-139">Sie werden einfach im Wörterbuch mit den anderen Werten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="e4ea4-140">Ein Standardwert kann den besonderen Wert **RouteParameter. optional**aufweisen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="e4ea4-141">Wenn ein Platzhalter diesem Wert zugewiesen wird, wird der Wert nicht dem Routen Wörterbuch hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="e4ea4-142">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="e4ea4-143">Für den URI-Pfad "API/Products" enthält das Routen Wörterbuch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="e4ea4-144">Controller: "Products"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-144">controller: "products"</span></span>
- <span data-ttu-id="e4ea4-145">Kategorie: "alle"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-145">category: "all"</span></span>

<span data-ttu-id="e4ea4-146">Für "API/Products/Toys/123" enthält das Routen Wörterbuch jedoch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="e4ea4-147">Controller: "Products"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-147">controller: "products"</span></span>
- <span data-ttu-id="e4ea4-148">Kategorie: "Toys"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-148">category: "toys"</span></span>
- <span data-ttu-id="e4ea4-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-149">id: "123"</span></span>

<span data-ttu-id="e4ea4-150">Die Standardwerte können auch einen Wert enthalten, der nicht an einer beliebigen Stelle in der Routen Vorlage angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="e4ea4-151">Wenn die Route übereinstimmt, wird dieser Wert im Wörterbuch gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="e4ea4-152">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="e4ea4-153">Wenn der URI-Pfad "API/root/8" lautet, enthält das Wörterbuch zwei Werte:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="e4ea4-154">Controller: "Customers"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-154">controller: "customers"</span></span>
- <span data-ttu-id="e4ea4-155">ID: "8"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="e4ea4-156">Auswählen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="e4ea4-156">Selecting a Controller</span></span>

<span data-ttu-id="e4ea4-157">Die Controller Auswahl wird von der **ihttpcontrollerselector. selectcontroller** -Methode behandelt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="e4ea4-158">Diese Methode nimmt eine **httprequestmessage** -Instanz an und gibt einen **httpcontrollerdescriptor**zurück.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="e4ea4-159">Die Standard Implementierung wird von der **defaulthttpcontrollerselector** -Klasse bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="e4ea4-160">Diese Klasse verwendet einen einfachen Algorithmus:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="e4ea4-161">Suchen Sie im Routen Wörterbuch nach dem Schlüssel "controller".</span><span class="sxs-lookup"><span data-stu-id="e4ea4-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="e4ea4-162">Nehmen Sie den Wert für diesen Schlüssel an, und fügen Sie die Zeichenfolge "controller" an, um den Namen des Controller Typs zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="e4ea4-163">Suchen Sie nach einem Web-API-Controller mit diesem Typnamen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="e4ea4-164">Wenn das Routen Wörterbuch beispielsweise das Schlüssel-Wert-Paar "controller" = "Products" enthält, ist der Controllertyp "productscontroller".</span><span class="sxs-lookup"><span data-stu-id="e4ea4-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="e4ea4-165">Wenn kein übereinstimmender Typ oder mehrere Übereinstimmungen vorhanden sind, gibt das Framework einen Fehler an den Client zurück.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="e4ea4-166">In Schritt 3 verwendet **defaulthttpcontrollerselector** die **ihttpcontrollertyperesolver** -Schnittstelle, um die Liste der Web-API-Controller Typen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="e4ea4-167">Die Standard Implementierung von **ihttpcontrollertyperesolver** gibt alle öffentlichen Klassen zurück, die (a) **ihttpcontroller**implementieren, (b) nicht abstrakt sind und (c) einen Namen haben, der auf "controller" endet.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="e4ea4-168">Aktions Auswahl</span><span class="sxs-lookup"><span data-stu-id="e4ea4-168">Action Selection</span></span>

<span data-ttu-id="e4ea4-169">Nachdem Sie den Controller ausgewählt haben, wählt das Framework die Aktion durch Aufrufen der **ihttpactionselector. SelectAction** -Methode aus.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="e4ea4-170">Diese Methode nimmt einen **httpcontrollercontext** an und gibt einen **httpactiondescriptor**zurück.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="e4ea4-171">Die Standard Implementierung wird von der **apicontrolleraction Selector** -Klasse bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="e4ea4-172">Zum Auswählen einer Aktion wird Folgendes untersucht:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="e4ea4-173">Die HTTP-Methode der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="e4ea4-174">Der Platzhalter "{action}" in der Routen Vorlage, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="e4ea4-175">Die Parameter der Aktionen auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="e4ea4-176">Bevor wir uns den Auswahl Algorithmus ansehen, müssen wir einige Dinge zu Controller Aktionen verstehen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="e4ea4-177">**Welche Methoden auf dem Controller werden als "Aktionen" betrachtet?**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="e4ea4-178">Wenn eine Aktion ausgewählt wird, untersucht das Framework nur öffentliche Instanzmethoden auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="e4ea4-179">Außerdem werden ["spezielle namens Methoden"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (Konstruktoren, Ereignisse, Operator Überladungen usw.) und Methoden, die von der **apicontroller** -Klasse geerbt werden, ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="e4ea4-180">**HTTP-Methoden.**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-180">**HTTP Methods.**</span></span> <span data-ttu-id="e4ea4-181">Das Framework wählt nur Aktionen aus, die der HTTP-Methode der Anforderung entsprechen, wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="e4ea4-182">Sie können die HTTP-Methode mit einem Attribut angeben: **akzeptverbs**, **httpdelete**, **HttpGet**, **httphead**, **httpoptions**, **httppatch**, **HttpPost**oder **httpput**.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="e4ea4-183">Andernfalls, wenn der Name der Controller Methode mit "Get", "Post", "Put", "Delete", "Head", "Options" oder "Patch" beginnt, unterstützt die Aktion die HTTP-Methode gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="e4ea4-184">Wenn keine der oben genannten Punkte angegeben ist, unterstützt die Methode Post.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="e4ea4-185">**Parameter Bindungen.**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-185">**Parameter Bindings.**</span></span> <span data-ttu-id="e4ea4-186">Eine Parameter Bindung besteht darin, wie die Web-API einen Wert für einen Parameter erstellt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="e4ea4-187">Dies ist die Standardregel für die Parameter Bindung:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="e4ea4-188">Einfache Typen werden aus dem URI entnommen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="e4ea4-189">Komplexe Typen werden aus dem Anforderungs Text entnommen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="e4ea4-190">Einfache Typen umfassen alle [.NET Framework primitiven Typen](https://msdn.microsoft.com/library/system.type.isprimitive)sowie **DateTime**, **Decimal**, **GUID**, **String**und **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="e4ea4-191">Für jede Aktion kann höchstens ein Parameter den Anforderungs Text lesen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ea4-192">Es ist möglich, die Standard Bindungs Regeln zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="e4ea4-193">Weitere Informationen finden Sie [unter WebAPI-Parameter Bindung im](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>

<span data-ttu-id="e4ea4-194">Mit diesem Hintergrund ist hier der Aktions Auswahl Algorithmus.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="e4ea4-195">Erstellen Sie eine Liste aller Aktionen auf dem Controller, die der HTTP-Anforderungs Methode entsprechen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="e4ea4-196">Wenn das Routen Wörterbuch über einen "action"-Eintrag verfügt, entfernen Sie Aktionen, deren Name diesem Wert nicht entspricht.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="e4ea4-197">Versuchen Sie, die Aktionsparameter wie folgt mit dem URI zu vergleichen:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="e4ea4-198">Für jede Aktion wird eine Liste der Parameter abgerufen, bei denen es sich um einen einfachen Typ handelt, bei dem die Bindung den Parameter aus dem URI abruft.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="e4ea4-199">Optionale Parameter ausschließen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="e4ea4-200">Versuchen Sie aus dieser Liste, eine Entsprechung für jeden Parameternamen zu finden, entweder im Routen Wörterbuch oder in der URI-Abfrage Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="e4ea4-201">Bei Übereinstimmungen wird die Groß-/Kleinschreibung nicht beachtet, und die Reihenfolge der Parameter</span><span class="sxs-lookup"><span data-stu-id="e4ea4-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="e4ea4-202">Wählen Sie eine Aktion aus, bei der jeder Parameter in der Liste eine Entsprechung im URI aufweist.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="e4ea4-203">Wenn mehr als eine Aktion diese Kriterien erfüllt, wählen Sie die mit den meisten Parameter Übereinstimmungen aus.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="e4ea4-204">Ignorieren von Aktionen mit dem **[NonAction]** -Attribut.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="e4ea4-205">Schritt #3 ist wahrscheinlich die verwirrend.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="e4ea4-206">Die grundlegende Idee ist, dass ein Parameter seinen Wert entweder aus dem URI, aus dem Anforderungs Text oder aus einer benutzerdefinierten Bindung erhalten kann.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="e4ea4-207">Für Parameter, die aus dem URI stammen, möchten wir sicherstellen, dass der URI tatsächlich einen Wert für diesen Parameter enthält, entweder im Pfad (über das Routen Wörterbuch) oder in der Abfrage Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="e4ea4-208">Sehen Sie sich beispielsweise die folgende Aktion an:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="e4ea4-209">Der *ID* -Parameter wird an den URI gebunden.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="e4ea4-210">Daher kann diese Aktion nur einem URI entsprechen, der einen Wert für "ID" enthält, entweder im Routen Wörterbuch oder in der Abfrage Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="e4ea4-211">Optionale Parameter stellen eine Ausnahme dar, da Sie optional sind.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="e4ea4-212">Für einen optionalen Parameter ist es in Ordnung, wenn die Bindung den Wert aus dem URI nicht erhalten kann.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="e4ea4-213">Komplexe Typen sind aus einem anderen Grund eine Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="e4ea4-214">Ein komplexer Typ kann nur über eine benutzerdefinierte Bindung an den URI gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="e4ea4-215">Aber in diesem Fall kann das Framework nicht im Voraus wissen, ob der Parameter an einen bestimmten URI gebunden würde.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="e4ea4-216">Zum ermitteln muss die Bindung aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="e4ea4-217">Das Ziel des Auswahl Algorithmus besteht darin, vor dem Aufrufen von Bindungen eine Aktion aus der statischen Beschreibung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="e4ea4-218">Daher werden komplexe Typen aus dem übereinstimmenden Algorithmus ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="e4ea4-219">Nachdem die Aktion ausgewählt wurde, werden alle Parameter Bindungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="e4ea4-220">Zusammenfassung:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-220">Summary:</span></span>

- <span data-ttu-id="e4ea4-221">Die Aktion muss mit der HTTP-Methode der Anforderung identisch sein.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="e4ea4-222">Der Aktionsname muss mit dem Eintrag "action" im Routen Wörterbuch, falls vorhanden, identisch sein.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="e4ea4-223">Wenn der Parameter für jeden Parameter der Aktion aus dem URI entnommen wird, muss der Parameter Name entweder im Routen Wörterbuch oder in der URI-Abfrage Zeichenfolge gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="e4ea4-224">(Optionale Parameter und Parameter mit komplexen Typen werden ausgeschlossen.)</span><span class="sxs-lookup"><span data-stu-id="e4ea4-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="e4ea4-225">Versuchen Sie, die meisten Parameter zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="e4ea4-226">Die beste Entsprechung kann eine Methode ohne Parameter sein.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="e4ea4-227">Erweitertes Beispiel</span><span class="sxs-lookup"><span data-stu-id="e4ea4-227">Extended Example</span></span>

<span data-ttu-id="e4ea4-228">Radwegen</span><span class="sxs-lookup"><span data-stu-id="e4ea4-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="e4ea4-229">Controller:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="e4ea4-230">HTTP-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="e4ea4-231">Routen Abgleich</span><span class="sxs-lookup"><span data-stu-id="e4ea4-231">Route Matching</span></span>

<span data-ttu-id="e4ea4-232">Der URI entspricht der Route mit dem Namen "defaultapi".</span><span class="sxs-lookup"><span data-stu-id="e4ea4-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="e4ea4-233">Das Routen Wörterbuch enthält die folgenden Einträge:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="e4ea4-234">Controller: "Products"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-234">controller: "products"</span></span>
- <span data-ttu-id="e4ea4-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-235">id: "1"</span></span>

<span data-ttu-id="e4ea4-236">Das Routen Wörterbuch enthält nicht die Abfrage Zeichenfolgen-Parameter "Version" und "Details", aber diese werden während der Aktions Auswahl weiterhin berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="e4ea4-237">Controller Auswahl</span><span class="sxs-lookup"><span data-stu-id="e4ea4-237">Controller Selection</span></span>

<span data-ttu-id="e4ea4-238">Aus dem Eintrag "controller" im Routen Wörterbuch ist der Controllertyp `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="e4ea4-239">Aktions Auswahl</span><span class="sxs-lookup"><span data-stu-id="e4ea4-239">Action Selection</span></span>

<span data-ttu-id="e4ea4-240">Die HTTP-Anforderung ist eine GET-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="e4ea4-241">Die Controller Aktionen, die Get unterstützen, sind `GetAll`, `GetById`und `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="e4ea4-242">Das Routen Wörterbuch enthält keinen Eintrag für "action", daher müssen wir nicht mit dem Aktions Namen vergleichen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="e4ea4-243">Als nächstes versuchen wir, die Parameternamen für die Aktionen abzugleichen, indem wir nur die get-Aktionen betrachten.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="e4ea4-244">Aktion</span><span class="sxs-lookup"><span data-stu-id="e4ea4-244">Action</span></span> | <span data-ttu-id="e4ea4-245">Parameter für die Abgleich</span><span class="sxs-lookup"><span data-stu-id="e4ea4-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="e4ea4-246">none</span><span class="sxs-lookup"><span data-stu-id="e4ea4-246">none</span></span> |
| `GetById` | <span data-ttu-id="e4ea4-247">"id"</span><span class="sxs-lookup"><span data-stu-id="e4ea4-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="e4ea4-248">Benennen</span><span class="sxs-lookup"><span data-stu-id="e4ea4-248">"name"</span></span> |

<span data-ttu-id="e4ea4-249">Beachten Sie, dass der *Versions* Parameter von `GetById` nicht berücksichtigt wird, da es sich um einen optionalen Parameter handelt.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="e4ea4-250">Die `GetAll`-Methode entspricht trivial.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="e4ea4-251">Die `GetById`-Methode stimmt ebenfalls überein, da das Routen Wörterbuch "ID" enthält.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="e4ea4-252">Die `FindProductsByName`-Methode stimmt nicht mit ab.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="e4ea4-253">Die `GetById`-Methode gewinnt, da Sie mit einem Parameter übereinstimmt, im Gegensatz zu den Parametern für `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="e4ea4-254">Die-Methode wird mit den folgenden Parameterwerten aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="e4ea4-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="e4ea4-255">*id* = 1</span></span>
- <span data-ttu-id="e4ea4-256">*Version* = 1,5</span><span class="sxs-lookup"><span data-stu-id="e4ea4-256">*version* = 1.5</span></span>

<span data-ttu-id="e4ea4-257">Beachten Sie, dass der Wert des-Parameters aus der URI-Abfrage Zeichenfolge stammt, obwohl die *Version* im Auswahl Algorithmus nicht verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="e4ea4-258">Erweiterungspunkte</span><span class="sxs-lookup"><span data-stu-id="e4ea4-258">Extension Points</span></span>

<span data-ttu-id="e4ea4-259">Die Web-API stellt Erweiterungs Punkte für einige Teile des Routing Prozesses bereit.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="e4ea4-260">Schnittstelle</span><span class="sxs-lookup"><span data-stu-id="e4ea4-260">Interface</span></span> | <span data-ttu-id="e4ea4-261">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e4ea4-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e4ea4-262">**Ihttpcontrollerselector**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="e4ea4-263">Wählt den Controller aus.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-263">Selects the controller.</span></span> |
| <span data-ttu-id="e4ea4-264">**Ihttpcontrollertyperesolver**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="e4ea4-265">Ruft die Liste der Controller Typen ab.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-265">Gets the list of controller types.</span></span> <span data-ttu-id="e4ea4-266">Der " **defaulthttpcontrollerselector" wählt den Controllertyp** aus der Liste aus.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="e4ea4-267">**Iassembliesresolver**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="e4ea4-268">Ruft die Liste der projektenassemblys ab.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="e4ea4-269">Die **ihttpcontrollertyperesolver** -Schnittstelle verwendet diese Liste, um die Controller Typen zu suchen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="e4ea4-270">**Ihttpcontrolleractivator**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="e4ea4-271">Erstellt neue Controller Instanzen.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="e4ea4-272">**Ihttpactionselector**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="e4ea4-273">Wählt die Aktion aus.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-273">Selects the action.</span></span> |
| <span data-ttu-id="e4ea4-274">**Ihttpactioninvoker**</span><span class="sxs-lookup"><span data-stu-id="e4ea4-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="e4ea4-275">Ruft die Aktion auf.</span><span class="sxs-lookup"><span data-stu-id="e4ea4-275">Invokes the action.</span></span> |

<span data-ttu-id="e4ea4-276">Verwenden Sie die **Services** -Auflistung für das **httpconfiguration** -Objekt, um eine eigene Implementierung für eine dieser Schnittstellen bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="e4ea4-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
