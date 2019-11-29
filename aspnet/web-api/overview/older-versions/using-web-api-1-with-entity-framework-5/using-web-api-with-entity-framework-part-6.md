---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Teil 6: Erstellen von Produkt-und Bestell Controllern | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600031"
---
# <a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="65970-102">Teil 6: Erstellen von Produkt-und Bestell Controllern</span><span class="sxs-lookup"><span data-stu-id="65970-102">Part 6: Creating Product and Order Controllers</span></span>

<span data-ttu-id="65970-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="65970-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="65970-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="65970-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="65970-105">Hinzufügen eines Products-Controllers</span><span class="sxs-lookup"><span data-stu-id="65970-105">Add a Products Controller</span></span>

<span data-ttu-id="65970-106">Der Administrator Controller ist für Benutzer, die über Administratorrechte verfügen.</span><span class="sxs-lookup"><span data-stu-id="65970-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="65970-107">Kunden können dagegen Produkte anzeigen, jedoch nicht erstellen, aktualisieren oder löschen.</span><span class="sxs-lookup"><span data-stu-id="65970-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="65970-108">Der Zugriff auf die Post-, Put-und Delete-Methoden kann problemlos eingeschränkt werden, während die Get-Methoden geöffnet lassen.</span><span class="sxs-lookup"><span data-stu-id="65970-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="65970-109">Sehen Sie sich jedoch die Daten an, die für ein Produkt zurückgegeben werden:</span><span class="sxs-lookup"><span data-stu-id="65970-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="65970-110">Die `ActualCost`-Eigenschaft sollte für Kunden nicht sichtbar sein.</span><span class="sxs-lookup"><span data-stu-id="65970-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="65970-111">Die Lösung besteht darin, ein *Datenübertragungs Objekt (Data Transfer Object* , dto) zu definieren, das eine Teilmenge der Eigenschaften enthält, die für Kunden sichtbar sein sollten.</span><span class="sxs-lookup"><span data-stu-id="65970-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="65970-112">Wir verwenden LINQ to Project `Product` Instanzen, um Instanzen `ProductDTO`.</span><span class="sxs-lookup"><span data-stu-id="65970-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="65970-113">Fügen Sie dem Ordner Models eine Klasse mit dem Namen `ProductDTO` hinzu.</span><span class="sxs-lookup"><span data-stu-id="65970-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="65970-114">Fügen Sie jetzt den Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="65970-114">Now add the controller.</span></span> <span data-ttu-id="65970-115">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller.</span><span class="sxs-lookup"><span data-stu-id="65970-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="65970-116">Wählen Sie **Hinzufügen**und dann **Controller**aus.</span><span class="sxs-lookup"><span data-stu-id="65970-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="65970-117">Benennen Sie im Dialogfeld " **Controller hinzufügen** " den Controller &quot;ProductController-&quot;.</span><span class="sxs-lookup"><span data-stu-id="65970-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="65970-118">Wählen Sie unter **Vorlage**die Option **leerer API-Controller**aus.</span><span class="sxs-lookup"><span data-stu-id="65970-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="65970-119">Ersetzen Sie alles in der Quelldatei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="65970-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="65970-120">Der Controller verwendet weiterhin den `OrdersContext`, um die Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="65970-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="65970-121">Anstatt `Product` Instanzen direkt zurückzugeben, wird `MapProducts` aufgerufen, um Sie auf `ProductDTO` Instanzen zu projizieren:</span><span class="sxs-lookup"><span data-stu-id="65970-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="65970-122">Die `MapProducts`-Methode gibt einen **iquerable**-Wert zurück, sodass das Ergebnis mit anderen Abfrage Parametern zusammengesetzt werden kann.</span><span class="sxs-lookup"><span data-stu-id="65970-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="65970-123">Dies wird in der `GetProduct`-Methode angezeigt, mit der der Abfrage eine **Where** -Klausel hinzugefügt wird:</span><span class="sxs-lookup"><span data-stu-id="65970-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="65970-124">Hinzufügen eines Orders-Controllers</span><span class="sxs-lookup"><span data-stu-id="65970-124">Add an Orders Controller</span></span>

<span data-ttu-id="65970-125">Fügen Sie als nächstes einen Controller hinzu, mit dem Benutzer Bestellungen erstellen und anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="65970-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="65970-126">Wir beginnen mit einem weiteren dto.</span><span class="sxs-lookup"><span data-stu-id="65970-126">We'll start with another DTO.</span></span> <span data-ttu-id="65970-127">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine Klasse mit dem Namen `OrderDTO` der folgenden Implementierung hinzu:</span><span class="sxs-lookup"><span data-stu-id="65970-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="65970-128">Fügen Sie jetzt den Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="65970-128">Now add the controller.</span></span> <span data-ttu-id="65970-129">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller.</span><span class="sxs-lookup"><span data-stu-id="65970-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="65970-130">Wählen Sie **Hinzufügen**und dann **Controller**aus.</span><span class="sxs-lookup"><span data-stu-id="65970-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="65970-131">Legen Sie im Dialogfeld **Controller hinzufügen** die folgenden Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="65970-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="65970-132">Geben Sie unter **Controller Name den Namen**orderscontroller ein.</span><span class="sxs-lookup"><span data-stu-id="65970-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="65970-133">Wählen Sie unter **Vorlage**die Option "API-Controller mit Lese-/Schreibaktionen mit Entity Framework" aus.</span><span class="sxs-lookup"><span data-stu-id="65970-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="65970-134">Wählen Sie unter **Modell Klasse**die Option &quot;Order (productstore. Models)&quot;aus.</span><span class="sxs-lookup"><span data-stu-id="65970-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="65970-135">Wählen Sie unter **Datenkontext Klasse**die Option &quot;orderscontext (productstore. Models)&quot;aus.</span><span class="sxs-lookup"><span data-stu-id="65970-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="65970-136">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="65970-136">Click **Add**.</span></span> <span data-ttu-id="65970-137">Dadurch wird eine Datei mit dem Namen OrdersController.cs hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="65970-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="65970-138">Als nächstes müssen wir die Standard Implementierung des Controllers ändern.</span><span class="sxs-lookup"><span data-stu-id="65970-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="65970-139">Löschen Sie zunächst die Methoden `PutOrder` und `DeleteOrder`.</span><span class="sxs-lookup"><span data-stu-id="65970-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="65970-140">In diesem Beispiel können Kunden vorhandene Aufträge nicht ändern oder löschen.</span><span class="sxs-lookup"><span data-stu-id="65970-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="65970-141">In einer echten Anwendung benötigen Sie viel Back-End-Logik, um diese Fälle zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="65970-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="65970-142">(Z. b. wurde die Bestellung bereits versendet?)</span><span class="sxs-lookup"><span data-stu-id="65970-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="65970-143">Ändern Sie die `GetOrders`-Methode so, dass nur die Bestellungen zurückgegeben werden, die zum Benutzer gehören:</span><span class="sxs-lookup"><span data-stu-id="65970-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="65970-144">Ändern Sie die `GetOrder`-Methode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="65970-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="65970-145">Im folgenden finden Sie die Änderungen, die wir an der-Methode vorgenommen haben:</span><span class="sxs-lookup"><span data-stu-id="65970-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="65970-146">Der Rückgabewert ist eine `OrderDTO` Instanz anstelle eines `Order`.</span><span class="sxs-lookup"><span data-stu-id="65970-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="65970-147">Wenn die Datenbank für die Bestellung abgefragt wird, verwenden wir die [dbquery. include](https://msdn.microsoft.com/library/gg696395) -Methode, um die zugehörigen `OrderDetail` und `Product` Entitäten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="65970-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="65970-148">Wir vereinfachen das Ergebnis mithilfe einer Projektion.</span><span class="sxs-lookup"><span data-stu-id="65970-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="65970-149">Die HTTP-Antwort enthält ein Array von Produkten mit Mengen:</span><span class="sxs-lookup"><span data-stu-id="65970-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="65970-150">Dieses Format ist für Clients einfacher zu verwenden als das ursprüngliche Objekt Diagramm, das die Entitäten (Reihenfolge, Details und Produkte) enthält.</span><span class="sxs-lookup"><span data-stu-id="65970-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="65970-151">Die letzte Methode, die als `PostOrder`betrachtet werden soll.</span><span class="sxs-lookup"><span data-stu-id="65970-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="65970-152">Derzeit nimmt diese Methode eine `Order`-Instanz an.</span><span class="sxs-lookup"><span data-stu-id="65970-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="65970-153">Beachten Sie jedoch, was geschieht, wenn ein Client einen Anforderungs Text wie den folgenden sendet:</span><span class="sxs-lookup"><span data-stu-id="65970-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="65970-154">Dies ist eine gut strukturierte Reihenfolge, und Entity Framework wird sie glücklicherweise in die Datenbank einfügen.</span><span class="sxs-lookup"><span data-stu-id="65970-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="65970-155">Sie enthält jedoch eine Product-Entität, die zuvor noch nicht vorhanden war.</span><span class="sxs-lookup"><span data-stu-id="65970-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="65970-156">Der Client hat soeben ein neues Produkt in unserer Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="65970-156">The client just created a new product in our database!</span></span> <span data-ttu-id="65970-157">Dies ist eine Überraschung für die Auftrags Erfüllungs Abteilung, wenn Ihnen eine Bestellung für Koala-Bären angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="65970-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="65970-158">Die Moral besteht darin, die Daten, die Sie in einer Post-oder PUT-Anforderung akzeptieren, wirklich vorsichtig zu sein.</span><span class="sxs-lookup"><span data-stu-id="65970-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="65970-159">Um dieses Problem zu vermeiden, ändern Sie die `PostOrder`-Methode, um eine `OrderDTO`-Instanz zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="65970-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="65970-160">Verwenden Sie die `OrderDTO`, um den `Order`zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="65970-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="65970-161">Beachten Sie, dass die Eigenschaften `ProductID` und `Quantity` verwendet werden und alle Werte ignoriert werden, die der Client entweder für den Produktnamen oder den Preis gesendet hat.</span><span class="sxs-lookup"><span data-stu-id="65970-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="65970-162">Wenn die Produkt-ID ungültig ist, verstößt sie gegen die FOREIGN KEY-Einschränkung in der Datenbank, und die Einfügung schlägt wie folgt fehl.</span><span class="sxs-lookup"><span data-stu-id="65970-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="65970-163">Dies ist die gesamte `PostOrder`-Methode:</span><span class="sxs-lookup"><span data-stu-id="65970-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="65970-164">Fügen Sie schließlich dem Controller das Attribut " **autorisieren** " hinzu:</span><span class="sxs-lookup"><span data-stu-id="65970-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="65970-165">Jetzt können nur registrierte Benutzer Bestellungen erstellen oder anzeigen.</span><span class="sxs-lookup"><span data-stu-id="65970-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="65970-166">[Zurück](using-web-api-with-entity-framework-part-5.md)
> [Weiter](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="65970-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
