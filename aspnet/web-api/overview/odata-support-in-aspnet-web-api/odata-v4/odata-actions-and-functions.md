---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Aktionen und Funktionen in odata v4 mit ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: In odata können Aktionen und Funktionen serverseitiges Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. In diesem Tutorial wird gezeigt, wie Sie...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448059"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="9b85b-104">Aktionen und Funktionen in odata v4 mit ASP.net-Web-API 2,2</span><span class="sxs-lookup"><span data-stu-id="9b85b-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="9b85b-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9b85b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="9b85b-106">In odata können Aktionen und Funktionen serverseitiges Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="9b85b-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="9b85b-107">In diesem Tutorial wird gezeigt, wie Sie mithilfe der Web-API 2,2 Aktionen und Funktionen zu einem odata V4-Endpunkt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="9b85b-108">Das Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md) auf.</span><span class="sxs-lookup"><span data-stu-id="9b85b-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9b85b-109">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="9b85b-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="9b85b-110">Web-API 2,2</span><span class="sxs-lookup"><span data-stu-id="9b85b-110">Web API 2.2</span></span>
> - <span data-ttu-id="9b85b-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="9b85b-111">OData v4</span></span>
> - <span data-ttu-id="9b85b-112">Visual Studio 2013 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)herunterladen)</span><span class="sxs-lookup"><span data-stu-id="9b85b-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="9b85b-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9b85b-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="9b85b-114">Tutorialversionen</span><span class="sxs-lookup"><span data-stu-id="9b85b-114">Tutorial versions</span></span>
>
> <span data-ttu-id="9b85b-115">Informationen zu odata, Version 3, finden Sie [unter odata-Aktionen in ASP.net-Web-API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="9b85b-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="9b85b-116">Der Unterschied zwischen *Aktionen* und *Funktionen* besteht darin, dass Aktionen Nebeneffekte haben können und Funktionen dies nicht tun.</span><span class="sxs-lookup"><span data-stu-id="9b85b-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="9b85b-117">Sowohl Aktionen als auch Funktionen können Daten zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="9b85b-117">Both actions and functions can return data.</span></span> <span data-ttu-id="9b85b-118">Einige Verwendungsmöglichkeiten für Aktionen sind:</span><span class="sxs-lookup"><span data-stu-id="9b85b-118">Some uses for actions include:</span></span>

- <span data-ttu-id="9b85b-119">Komplexe Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-119">Complex transactions.</span></span>
- <span data-ttu-id="9b85b-120">Gleichzeitige Bearbeitung mehrerer Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9b85b-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="9b85b-121">Zulassen von Updates nur für bestimmte Eigenschaften einer Entität.</span><span class="sxs-lookup"><span data-stu-id="9b85b-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="9b85b-122">Senden von Daten, die keine Entität sind.</span><span class="sxs-lookup"><span data-stu-id="9b85b-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="9b85b-123">Funktionen sind nützlich für die Rückgabe von Informationen, die nicht direkt einer Entität oder Auflistung entsprechen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="9b85b-124">Eine Aktion (oder Funktion) kann eine einzelne Entität oder eine Auflistung als Ziel haben.</span><span class="sxs-lookup"><span data-stu-id="9b85b-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="9b85b-125">In der odata-Terminologie ist dies die *Bindung*.</span><span class="sxs-lookup"><span data-stu-id="9b85b-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="9b85b-126">Sie können auch &quot;ungebundene&quot; Aktionen/Funktionen haben, die als statische Vorgänge für den Dienst aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="9b85b-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="9b85b-127">Beispiel: Hinzufügen einer Aktion</span><span class="sxs-lookup"><span data-stu-id="9b85b-127">Example: Adding an Action</span></span>

<span data-ttu-id="9b85b-128">Definieren wir eine Aktion, um ein Produkt zu bewerten.</span><span class="sxs-lookup"><span data-stu-id="9b85b-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="9b85b-129">Dieses Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md) auf.</span><span class="sxs-lookup"><span data-stu-id="9b85b-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="9b85b-130">Fügen Sie zunächst ein `ProductRating` Modell hinzu, das die Bewertungen darstellt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="9b85b-131">Fügen Sie auch ein **dbset** zur `ProductsContext`-Klasse hinzu, damit EF eine Bewertungstabelle in der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="9b85b-132">Hinzufügen der Aktion zum EDM</span><span class="sxs-lookup"><span data-stu-id="9b85b-132">Add the Action to the EDM</span></span>

<span data-ttu-id="9b85b-133">Fügen Sie in WebApiConfig.cs den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="9b85b-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="9b85b-134">Die **entitytypeconfiguration. Action** -Methode fügt dem Entity Data Model (EDM) eine Aktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b85b-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="9b85b-135">Die **Parameter** Methode gibt einen typisierten Parameter für die Aktion an.</span><span class="sxs-lookup"><span data-stu-id="9b85b-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="9b85b-136">Mit diesem Code wird auch der Namespace für das EDM festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="9b85b-137">Der Namespace ist wichtig, da der URI für die Aktion den voll qualifizierten Aktions Namen enthält:</span><span class="sxs-lookup"><span data-stu-id="9b85b-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="9b85b-138">In einer typischen IIS-Konfiguration führt der Punkt in dieser URL dazu, dass IIS den Fehler 404 zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="9b85b-139">Sie können dieses Problem beheben, indem Sie den folgenden Abschnitt zu Ihrer Web. config-Datei hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="9b85b-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="9b85b-140">Fügen Sie eine Controller Methode für die Aktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b85b-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="9b85b-141">Fügen Sie `ProductsController`die folgende Methode hinzu, um die &quot;Rate&quot; Aktion zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="9b85b-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="9b85b-142">Beachten Sie, dass der Methodenname mit dem Aktions Namen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="9b85b-143">Das Attribut **[HttpPost]** gibt an, dass die Methode eine HTTP Post-Methode ist.</span><span class="sxs-lookup"><span data-stu-id="9b85b-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="9b85b-144">Zum Aufrufen der Aktion sendet der Client eine HTTP POST-Anforderung wie die folgende:</span><span class="sxs-lookup"><span data-stu-id="9b85b-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="9b85b-145">Die &quot;Rate&quot; Aktion ist an Produkt Instanzen gebunden, sodass der URI für die Aktion der voll qualifizierte Aktionsname ist, der an den Entitäts-URI angehängt ist.</span><span class="sxs-lookup"><span data-stu-id="9b85b-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="9b85b-146">(Denken Sie daran, dass der EDM-Namespace auf &quot;productservice&quot;festgelegt wurde, sodass der voll qualifizierte Aktionsname &quot;productservice. Rate&quot;ist.)</span><span class="sxs-lookup"><span data-stu-id="9b85b-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="9b85b-147">Der Anforderungs Text enthält die Aktionsparameter als JSON-Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="9b85b-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="9b85b-148">Die Web-API konvertiert die JSON-Nutzlast automatisch in ein **odataaktionparameters** -Objekt, bei dem es sich nur um ein Wörterbuch von Parameterwerten handelt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="9b85b-149">Verwenden Sie dieses Wörterbuch, um auf die Parameter in ihrer Controller Methode zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="9b85b-150">Wenn der Client die Aktionsparameter im falschen Format sendet, ist der Wert von **modelstate. IsValid** false.</span><span class="sxs-lookup"><span data-stu-id="9b85b-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="9b85b-151">Aktivieren Sie dieses Flag in ihrer Controller Methode, und geben Sie einen Fehler zurück, wenn **IsValid** den Wert false hat.</span><span class="sxs-lookup"><span data-stu-id="9b85b-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="9b85b-152">Beispiel: Hinzufügen einer Funktion</span><span class="sxs-lookup"><span data-stu-id="9b85b-152">Example: Adding a Function</span></span>

<span data-ttu-id="9b85b-153">Nun fügen wir eine odata-Funktion hinzu, die das teuerste Produkt zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="9b85b-154">Wie zuvor besteht der erste Schritt darin, die Funktion dem EDM hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="9b85b-155">Fügen Sie in WebApiConfig.cs den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="9b85b-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="9b85b-156">In diesem Fall ist die-Funktion an die Products-Auflistung gebunden, nicht an einzelne Produkt Instanzen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="9b85b-157">Clients rufen die Funktion auf, indem Sie eine GET-Anforderung senden:</span><span class="sxs-lookup"><span data-stu-id="9b85b-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="9b85b-158">Hier ist die Controller Methode für diese Funktion:</span><span class="sxs-lookup"><span data-stu-id="9b85b-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="9b85b-159">Beachten Sie, dass der Methodenname mit dem Funktionsnamen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="9b85b-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="9b85b-160">Das Attribut **[HttpGet]** gibt an, dass die Methode eine HTTP Get-Methode ist.</span><span class="sxs-lookup"><span data-stu-id="9b85b-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="9b85b-161">Hier ist die HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="9b85b-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="9b85b-162">Beispiel: Hinzufügen einer ungebundenen Funktion</span><span class="sxs-lookup"><span data-stu-id="9b85b-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="9b85b-163">Das vorherige Beispiel war eine Funktion, die an eine Auflistung gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="9b85b-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="9b85b-164">Im nächsten Beispiel erstellen wir eine *ungebundene* Funktion.</span><span class="sxs-lookup"><span data-stu-id="9b85b-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="9b85b-165">Ungebundene Funktionen werden als statische Vorgänge für den Dienst aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="9b85b-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="9b85b-166">Die-Funktion in diesem Beispiel gibt die Verkaufssteuer für eine bestimmte Postleitzahl zurück.</span><span class="sxs-lookup"><span data-stu-id="9b85b-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="9b85b-167">Fügen Sie in der webapiconfig-Datei dem EDM die-Funktion hinzu:</span><span class="sxs-lookup"><span data-stu-id="9b85b-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="9b85b-168">Beachten Sie, dass die **Funktion** anstelle des Entitäts Typs oder der Auflistung direkt für **odatamodelta Builder**aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="9b85b-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="9b85b-169">Dadurch wird dem Modell-Generator mitgeteilt, dass die Funktion nicht gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="9b85b-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="9b85b-170">Dies ist die Controller Methode, die die-Funktion implementiert:</span><span class="sxs-lookup"><span data-stu-id="9b85b-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="9b85b-171">Es spielt keine Rolle, in welchem Web-API-Controller Sie diese Methode platzieren.</span><span class="sxs-lookup"><span data-stu-id="9b85b-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="9b85b-172">Sie können Sie in `ProductsController`platzieren oder einen separaten Controller definieren.</span><span class="sxs-lookup"><span data-stu-id="9b85b-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="9b85b-173">Das **[odataroute]** -Attribut definiert die URI-Vorlage für die Funktion.</span><span class="sxs-lookup"><span data-stu-id="9b85b-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="9b85b-174">Im folgenden finden Sie ein Beispiel für eine Client Anforderung:</span><span class="sxs-lookup"><span data-stu-id="9b85b-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="9b85b-175">Die HTTP-Antwort sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="9b85b-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
