---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Unterstützende odata-Aktionen in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'In odata können Aktionen serverseitiges Verhalten hinzugefügt werden, die nicht einfach als CRUD-Vorgänge für Entitäten definiert werden können. Einige Verwendungsmöglichkeiten für Aktionen sind: implementieren...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600349"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="24984-104">Unterstützende odata-Aktionen in ASP.net-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="24984-104">Supporting OData Actions in ASP.NET Web API 2</span></span>

<span data-ttu-id="24984-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="24984-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="24984-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="24984-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="24984-107">In odata können *Aktionen* serverseitiges Verhalten hinzugefügt werden, die nicht einfach als CRUD-Vorgänge für Entitäten definiert werden können.</span><span class="sxs-lookup"><span data-stu-id="24984-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="24984-108">Einige Verwendungsmöglichkeiten für Aktionen sind:</span><span class="sxs-lookup"><span data-stu-id="24984-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="24984-109">Implementieren komplexer Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="24984-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="24984-110">Gleichzeitige Bearbeitung mehrerer Entitäten.</span><span class="sxs-lookup"><span data-stu-id="24984-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="24984-111">Zulassen von Updates nur für bestimmte Eigenschaften einer Entität.</span><span class="sxs-lookup"><span data-stu-id="24984-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="24984-112">Senden von Informationen an den Server, der nicht in einer Entität definiert ist.</span><span class="sxs-lookup"><span data-stu-id="24984-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="24984-113">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="24984-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="24984-114">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="24984-114">Web API 2</span></span>
> - <span data-ttu-id="24984-115">Odata, Version 3</span><span class="sxs-lookup"><span data-stu-id="24984-115">OData Version 3</span></span>
> - <span data-ttu-id="24984-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="24984-116">Entity Framework 6</span></span>

## <a name="example-rating-a-product"></a><span data-ttu-id="24984-117">Beispiel: bewerten eines Produkts</span><span class="sxs-lookup"><span data-stu-id="24984-117">Example: Rating a Product</span></span>

<span data-ttu-id="24984-118">In diesem Beispiel möchten wir, dass Benutzer Produkte bewerten und dann die durchschnittlichen Bewertungen für die einzelnen Produkte verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="24984-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="24984-119">In der-Datenbank speichern wir eine Liste mit Bewertungen, die in Produkte eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="24984-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="24984-120">Dies ist das Modell, mit dem die Bewertungen in Entity Framework dargestellt werden können:</span><span class="sxs-lookup"><span data-stu-id="24984-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="24984-121">Wir möchten jedoch nicht, dass Clients ein `ProductRating` Objekt in einer "Bewertungs Sammlung" veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="24984-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="24984-122">Die Bewertung ist intuitiv mit der Produktsammlung verknüpft, und der Client sollte nur den Bewertungs Wert veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="24984-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="24984-123">Anstatt die normalen CRUD-Vorgänge zu verwenden, wird daher eine Aktion definiert, die ein Client für ein Produkt aufrufen kann.</span><span class="sxs-lookup"><span data-stu-id="24984-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="24984-124">In der odata-Terminologie ist die-Aktion an Product-Entitäten *gebunden* .</span><span class="sxs-lookup"><span data-stu-id="24984-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="24984-125">Aktionen haben Nebeneffekte auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="24984-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="24984-126">Aus diesem Grund werden Sie mithilfe von HTTP POST-Anforderungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="24984-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="24984-127">Aktionen können über Parameter und Rückgabe Typen verfügen, die in den Dienst Metadaten beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="24984-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="24984-128">Der Client sendet die Parameter im Anforderungs Text, und der Server sendet den Rückgabewert im Antworttext.</span><span class="sxs-lookup"><span data-stu-id="24984-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="24984-129">Zum Aufrufen der Aktion "Produkt Raten" sendet der Client wie folgt einen Post-Vorgang an einen URI:</span><span class="sxs-lookup"><span data-stu-id="24984-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="24984-130">Bei den Daten in der Post-Anforderung handelt es sich lediglich um die Produktbewertung:</span><span class="sxs-lookup"><span data-stu-id="24984-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="24984-131">Deklarieren Sie die Aktion im Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="24984-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="24984-132">Fügen Sie die Aktion in Ihrer Web-API-Konfiguration dem Entity Data Model (EDM) hinzu:</span><span class="sxs-lookup"><span data-stu-id="24984-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="24984-133">In diesem Code wird "rateproduct" als eine Aktion definiert, die für Product-Entitäten ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="24984-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="24984-134">Außerdem wird deklariert, dass die Aktion einen **int** -Parameter mit dem Namen "Rating" annimmt und einen **int** -Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="24984-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="24984-135">Hinzufügen der Aktion zum Controller</span><span class="sxs-lookup"><span data-stu-id="24984-135">Add the Action to the Controller</span></span>

<span data-ttu-id="24984-136">Die Aktion "rateproduct" ist an Product Entities gebunden.</span><span class="sxs-lookup"><span data-stu-id="24984-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="24984-137">Fügen Sie dem Products-Controller eine Methode mit dem Namen `RateProduct` hinzu, um die Aktion zu implementieren:</span><span class="sxs-lookup"><span data-stu-id="24984-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="24984-138">Beachten Sie, dass der Methodenname mit dem Namen der Aktion im EDM übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="24984-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="24984-139">Die-Methode verfügt über zwei Parameter:</span><span class="sxs-lookup"><span data-stu-id="24984-139">The method has two parameters:</span></span>

- <span data-ttu-id="24984-140">*Key*: der Schlüssel, der vom Produkt zu bewerten ist.</span><span class="sxs-lookup"><span data-stu-id="24984-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="24984-141">*Parameter*: ein Wörterbuch mit Aktionsparameter Werten.</span><span class="sxs-lookup"><span data-stu-id="24984-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="24984-142">Wenn Sie die standardmäßigen Routing Konventionen verwenden, muss der Schlüsselparameter "Key" lauten.</span><span class="sxs-lookup"><span data-stu-id="24984-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="24984-143">Es ist auch wichtig, das Attribut **[fromudatauri]** wie dargestellt einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="24984-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="24984-144">Dieses Attribut weist die Web-API an, odata-Syntax Regeln zu verwenden, wenn der Schlüssel aus dem Anforderungs-URI analysiert wird.</span><span class="sxs-lookup"><span data-stu-id="24984-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="24984-145">Verwenden Sie das *Parameter* Wörterbuch, um die Aktionsparameter zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="24984-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="24984-146">Wenn der Client die Aktionsparameter im richtigen Format sendet, ist der Wert von **modelstate. IsValid** "true".</span><span class="sxs-lookup"><span data-stu-id="24984-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="24984-147">In diesem Fall können Sie das **odataaktionparameters** -Wörterbuch verwenden, um die Parameterwerte zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="24984-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="24984-148">In diesem Beispiel nimmt die `RateProduct` Aktion einen einzelnen Parameter mit dem Namen "Rating" an.</span><span class="sxs-lookup"><span data-stu-id="24984-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="24984-149">Aktions Metadaten</span><span class="sxs-lookup"><span data-stu-id="24984-149">Action Metadata</span></span>

<span data-ttu-id="24984-150">Um die Dienst Metadaten anzuzeigen, senden Sie eine GET-Anforderung an/odata/-$Metadata.</span><span class="sxs-lookup"><span data-stu-id="24984-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="24984-151">Dies ist der Teil der Metadaten, der die `RateProduct` Aktion deklariert:</span><span class="sxs-lookup"><span data-stu-id="24984-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="24984-152">Das **FunctionImport** -Element deklariert die Aktion.</span><span class="sxs-lookup"><span data-stu-id="24984-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="24984-153">Die meisten Felder sind selbsterklärend, aber zwei sind erwähnenswert:</span><span class="sxs-lookup"><span data-stu-id="24984-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="24984-154">**Isbindable** bedeutet, dass die Aktion für die Ziel Entität mindestens einige Zeit aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="24984-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="24984-155">**Isalwaysbindable** bedeutet, dass die Aktion immer für die Ziel Entität aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="24984-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="24984-156">Der Unterschied besteht darin, dass einige Aktionen für Clients immer verfügbar sind, andere Aktionen jedoch möglicherweise vom Zustand der Entität abhängen.</span><span class="sxs-lookup"><span data-stu-id="24984-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="24984-157">Nehmen Sie beispielsweise an, dass Sie eine Aktion zum Kauf definieren.</span><span class="sxs-lookup"><span data-stu-id="24984-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="24984-158">Sie können nur ein Element erwerben, das sich in der Aktienliste befindet.</span><span class="sxs-lookup"><span data-stu-id="24984-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="24984-159">Wenn das Element nicht vorrätig ist, kann ein Client diese Aktion nicht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="24984-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="24984-160">Wenn Sie das EDM definieren, erstellt die **Aktions** Methode eine immer bindbare Aktion:</span><span class="sxs-lookup"><span data-stu-id="24984-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="24984-161">Ich werde weiter unten in diesem Thema über nicht immer bindbare Aktionen (auch als *vorübergehende* Aktionen bezeichnet) sprechen.</span><span class="sxs-lookup"><span data-stu-id="24984-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="24984-162">Aufrufen der Aktion</span><span class="sxs-lookup"><span data-stu-id="24984-162">Invoking the Action</span></span>

<span data-ttu-id="24984-163">Sehen wir uns nun an, wie ein Client diese Aktion aufruft.</span><span class="sxs-lookup"><span data-stu-id="24984-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="24984-164">Angenommen, der Client möchte eine Bewertung von 2 für das Produkt mit der ID "4" abgeben.</span><span class="sxs-lookup"><span data-stu-id="24984-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="24984-165">Im folgenden finden Sie ein Beispiel für eine Anforderungs Nachricht mit dem JSON-Format für den Anforderungs Text:</span><span class="sxs-lookup"><span data-stu-id="24984-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="24984-166">Die Antwortnachricht lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="24984-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="24984-167">Binden einer Aktion an eine Entitätenmenge</span><span class="sxs-lookup"><span data-stu-id="24984-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="24984-168">Im vorherigen Beispiel ist die Aktion an eine einzelne Entität gebunden: der Client bewertet ein einzelnes Produkt.</span><span class="sxs-lookup"><span data-stu-id="24984-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="24984-169">Sie können eine Aktion auch an eine Auflistung von Entitäten binden.</span><span class="sxs-lookup"><span data-stu-id="24984-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="24984-170">Nehmen Sie einfach die folgenden Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="24984-170">Just make the following changes:</span></span>

<span data-ttu-id="24984-171">Fügen Sie im EDM die **Aktion der Auflistungs Eigenschaft der** Entität hinzu.</span><span class="sxs-lookup"><span data-stu-id="24984-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="24984-172">Lassen Sie in der Controller Methode den *Key* -Parameter Weg.</span><span class="sxs-lookup"><span data-stu-id="24984-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="24984-173">Nun ruft der Client die Aktion für die Products-Entitätenmenge auf:</span><span class="sxs-lookup"><span data-stu-id="24984-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="24984-174">Aktionen mit Sammlungs Parametern</span><span class="sxs-lookup"><span data-stu-id="24984-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="24984-175">Aktionen können über Parameter verfügen, die eine Auflistung von Werten annehmen.</span><span class="sxs-lookup"><span data-stu-id="24984-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="24984-176">Verwenden Sie im EDM **collectionparameter&lt;t-&gt;** , um den-Parameter zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="24984-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="24984-177">Dadurch wird ein Parameter mit dem Namen "Ratings" deklariert, der eine Auflistung von **int** -Werten annimmt.</span><span class="sxs-lookup"><span data-stu-id="24984-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="24984-178">In der Controller Methode erhalten Sie immer noch den Parameterwert aus dem **odataaktionparameters** -Objekt, aber der Wert ist nun ein **ICollection-&lt;int&gt;** Wert:</span><span class="sxs-lookup"><span data-stu-id="24984-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="24984-179">Vorübergehende Aktionen</span><span class="sxs-lookup"><span data-stu-id="24984-179">Transient Actions</span></span>

<span data-ttu-id="24984-180">Im Beispiel "rateproduct" können Benutzer immer ein Produkt bewerten, sodass die Aktion immer verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="24984-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="24984-181">Einige Aktionen hängen jedoch vom Zustand der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="24984-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="24984-182">Beispielsweise ist in einem videovermietungs Dienst die Aktion "Auschecken" nicht immer verfügbar.</span><span class="sxs-lookup"><span data-stu-id="24984-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="24984-183">(Es hängt davon ab, ob eine Kopie des Videos verfügbar ist.) Diese Art von Aktion wird als *vorübergehende* Aktion bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="24984-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="24984-184">In den Dienst Metadaten ist **isalwaysbindable** für eine vorübergehende Aktion gleich false.</span><span class="sxs-lookup"><span data-stu-id="24984-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="24984-185">Das ist eigentlich der Standardwert, sodass die Metadaten wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="24984-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="24984-186">Dies ist der Grund, warum dies wichtig ist: Wenn eine Aktion vorübergehend ist, muss der Server dem Client mitteilen, wann die Aktion verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="24984-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="24984-187">Dies erfolgt durch Einschließen eines Links zur Aktion in der Entität.</span><span class="sxs-lookup"><span data-stu-id="24984-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="24984-188">Im folgenden finden Sie ein Beispiel für eine Movie-Entität:</span><span class="sxs-lookup"><span data-stu-id="24984-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="24984-189">Die Eigenschaft "#Checkout" enthält einen Link zur Checkout-Aktion.</span><span class="sxs-lookup"><span data-stu-id="24984-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="24984-190">Wenn die Aktion nicht verfügbar ist, lässt der Server den Link aus.</span><span class="sxs-lookup"><span data-stu-id="24984-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="24984-191">Rufen Sie zum Deklarieren einer vorübergehenden Aktion im EDM die **transientaction** -Methode auf:</span><span class="sxs-lookup"><span data-stu-id="24984-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="24984-192">Außerdem müssen Sie eine Funktion bereitstellen, die einen Aktions Link für eine bestimmte Entität zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="24984-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="24984-193">Legen Sie diese Funktion durch Aufrufen von **hasaction Link**fest.</span><span class="sxs-lookup"><span data-stu-id="24984-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="24984-194">Sie können die Funktion als Lambda-Ausdruck schreiben:</span><span class="sxs-lookup"><span data-stu-id="24984-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="24984-195">Wenn die Aktion verfügbar ist, gibt der Lambda-Ausdruck einen Link zur Aktion zurück.</span><span class="sxs-lookup"><span data-stu-id="24984-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="24984-196">Der odata-Serialisierer schließt diesen Link ein, wenn die Entität serialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="24984-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="24984-197">Wenn die Aktion nicht verfügbar ist, gibt die Funktion `null`zurück.</span><span class="sxs-lookup"><span data-stu-id="24984-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24984-198">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="24984-198">Additional Resources</span></span>

[<span data-ttu-id="24984-199">Beispiel für odata-Aktionen</span><span class="sxs-lookup"><span data-stu-id="24984-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
