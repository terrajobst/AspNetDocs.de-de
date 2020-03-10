---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Teil 7: Erstellen der Hauptseite | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484449"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="d68ce-102">Teil 7: Erstellen der Hauptseite</span><span class="sxs-lookup"><span data-stu-id="d68ce-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="d68ce-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d68ce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d68ce-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="d68ce-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="d68ce-105">Erstellen der Hauptseite</span><span class="sxs-lookup"><span data-stu-id="d68ce-105">Creating the Main Page</span></span>

<span data-ttu-id="d68ce-106">In diesem Abschnitt erstellen Sie die Haupt Anwendungsseite.</span><span class="sxs-lookup"><span data-stu-id="d68ce-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="d68ce-107">Diese Seite ist komplexer als die Administrator Seite. Daher werden wir Sie in mehreren Schritten betrachten.</span><span class="sxs-lookup"><span data-stu-id="d68ce-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="d68ce-108">Dabei werden einige erweiterte Knockout. js-Techniken angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d68ce-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="d68ce-109">Im folgenden finden Sie das grundlegende Layout der Seite:</span><span class="sxs-lookup"><span data-stu-id="d68ce-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="d68ce-110">"Products" enthält ein Array von Produkten.</span><span class="sxs-lookup"><span data-stu-id="d68ce-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="d68ce-111">"Cart" enthält ein Array von Produkten mit Mengen.</span><span class="sxs-lookup"><span data-stu-id="d68ce-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="d68ce-112">Durch Klicken auf "zu Warenkorb hinzufügen" wird der Warenkorb aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="d68ce-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="d68ce-113">"Orders" enthält ein Array von Auftrags-IDs.</span><span class="sxs-lookup"><span data-stu-id="d68ce-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="d68ce-114">"Details" enthält ein Bestell Detail, bei dem es sich um ein Array von Elementen (Produkte mit Mengen) handelt.</span><span class="sxs-lookup"><span data-stu-id="d68ce-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="d68ce-115">Wir beginnen mit dem definieren einiger grundlegender Layouts in HTML, ohne Datenbindung oder Skript.</span><span class="sxs-lookup"><span data-stu-id="d68ce-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="d68ce-116">Öffnen Sie die Datei views/Home/Index. cshtml, und ersetzen Sie den gesamten Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d68ce-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="d68ce-117">Fügen Sie als nächstes einen Skript Abschnitt hinzu, und erstellen Sie ein leeres Ansichts Modell:</span><span class="sxs-lookup"><span data-stu-id="d68ce-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="d68ce-118">Basierend auf dem zuvor skizzierten Entwurf benötigt unser Ansichts Modell Observables für Produkte, Warenkorb, Bestellungen und Details.</span><span class="sxs-lookup"><span data-stu-id="d68ce-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="d68ce-119">Fügen Sie dem `AppViewModel` Objekt die folgenden Variablen hinzu:</span><span class="sxs-lookup"><span data-stu-id="d68ce-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="d68ce-120">Benutzer können Elemente aus der Liste Produkte zum Warenkorb hinzufügen und Elemente aus dem Warenkorb entfernen.</span><span class="sxs-lookup"><span data-stu-id="d68ce-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="d68ce-121">Um diese Funktionen zu kapseln, erstellen wir eine weitere Ansichts Modell Klasse, die ein Produkt darstellt.</span><span class="sxs-lookup"><span data-stu-id="d68ce-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="d68ce-122">Fügen Sie `AppViewModel` den folgenden Code zu folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d68ce-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="d68ce-123">Die `ProductViewModel`-Klasse enthält zwei Funktionen, die verwendet werden, um das Produkt in und aus dem Warenkorb zu verschieben: `addItemToCart` fügt dem Warenkorb eine Einheit des Produkts hinzu, und `removeAllFromCart` entfernt alle Mengen des Produkts.</span><span class="sxs-lookup"><span data-stu-id="d68ce-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="d68ce-124">Benutzer können eine vorhandene Bestellung auswählen und die Bestelldetails erhalten.</span><span class="sxs-lookup"><span data-stu-id="d68ce-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="d68ce-125">Wir Kapseln diese Funktionalität in ein anderes Ansichts Modell:</span><span class="sxs-lookup"><span data-stu-id="d68ce-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="d68ce-126">Die `OrderDetailsViewModel` wird mit einer Bestellung initialisiert, und die Bestelldetails werden abgerufen, indem eine AJAX-Anforderung an den Server gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="d68ce-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="d68ce-127">Beachten Sie auch die `total`-Eigenschaft für die `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="d68ce-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="d68ce-128">Diese Eigenschaft ist eine besondere Art Observable, die als [berechnete Observable](http://knockoutjs.com/documentation/computedObservables.html)bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="d68ce-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="d68ce-129">Wie der Name schon sagt, können Sie mit einem berechneten Observable Daten an einen berechneten&#8212;Wert binden, in diesem Fall die Gesamtkosten der Bestellung.</span><span class="sxs-lookup"><span data-stu-id="d68ce-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="d68ce-130">Fügen Sie als nächstes diese Funktionen `AppViewModel`hinzu:</span><span class="sxs-lookup"><span data-stu-id="d68ce-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="d68ce-131">`resetCart` entfernt alle Elemente aus dem Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="d68ce-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="d68ce-132">`getDetails` Ruft die Details für eine Bestellung ab (indem eine neue `OrderDetailsViewModel` auf die `details` Liste übertragen wird).</span><span class="sxs-lookup"><span data-stu-id="d68ce-132">`getDetails` gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="d68ce-133">`createOrder` erstellt eine neue Bestellung und leert den Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="d68ce-133">`createOrder` creates a new order and empties the cart.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="d68ce-134">Initialisieren Sie schließlich das Ansichts Modell, indem Sie AJAX-Anforderungen für die Produkte und Bestellungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="d68ce-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="d68ce-135">OK, das ist zwar viel Code, aber wir haben das Schritt-für-Schritt-Code erstellt, und ich hoffe, dass der Entwurf eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="d68ce-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="d68ce-136">Nun können dem HTML einige Knockout. js-Bindungen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="d68ce-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="d68ce-137">**Produkte**</span><span class="sxs-lookup"><span data-stu-id="d68ce-137">**Products**</span></span>

<span data-ttu-id="d68ce-138">Im folgenden finden Sie die Bindungen für die Produktliste:</span><span class="sxs-lookup"><span data-stu-id="d68ce-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="d68ce-139">Dies durchläuft das Products-Array und zeigt den Namen und den Preis an.</span><span class="sxs-lookup"><span data-stu-id="d68ce-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="d68ce-140">Die Schaltfläche "zu Bestellung hinzufügen" ist nur sichtbar, wenn der Benutzer angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="d68ce-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="d68ce-141">Mit der Schaltfläche "zu Bestellung hinzufügen" werden `addItemToCart` für die `ProductViewModel` Instanz des Produkts aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="d68ce-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="d68ce-142">Dies veranschaulicht ein nützliches Feature von Knockout. js: Wenn ein Ansichts Modell andere Ansichts Modelle enthält, können Sie die Bindungen auf das innere Modell anwenden.</span><span class="sxs-lookup"><span data-stu-id="d68ce-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="d68ce-143">In diesem Beispiel werden die Bindungen in der `foreach` auf jede der `ProductViewModel` Instanzen angewendet.</span><span class="sxs-lookup"><span data-stu-id="d68ce-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="d68ce-144">Diese Vorgehensweise ist viel sauberer, als die gesamte Funktionalität in einem einzigen Ansichts Modell zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="d68ce-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="d68ce-145">**Brust**</span><span class="sxs-lookup"><span data-stu-id="d68ce-145">**Cart**</span></span>

<span data-ttu-id="d68ce-146">Hier sind die Bindungen für den Warenkorb:</span><span class="sxs-lookup"><span data-stu-id="d68ce-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="d68ce-147">Dadurch wird das waren Korb Array durchlaufen, und der Name, der Preis und die Menge werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d68ce-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="d68ce-148">Beachten Sie, dass der Link "Remove" und die Schaltfläche "Create Order" an View-Model-Funktionen gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="d68ce-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="d68ce-149">**Bestellungen**</span><span class="sxs-lookup"><span data-stu-id="d68ce-149">**Orders**</span></span>

<span data-ttu-id="d68ce-150">Im folgenden finden Sie die Bindungen für die Liste Bestellungen:</span><span class="sxs-lookup"><span data-stu-id="d68ce-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="d68ce-151">Dadurch werden die Bestellungen durchlaufen, und die Bestell-ID wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d68ce-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="d68ce-152">Das Click-Ereignis für den Link ist an die `getDetails` Funktion gebunden.</span><span class="sxs-lookup"><span data-stu-id="d68ce-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="d68ce-153">**Bestell Details**</span><span class="sxs-lookup"><span data-stu-id="d68ce-153">**Order Details**</span></span>

<span data-ttu-id="d68ce-154">Im folgenden finden Sie die Bindungen für die Bestelldetails:</span><span class="sxs-lookup"><span data-stu-id="d68ce-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="d68ce-155">Dadurch werden die Elemente in der Reihenfolge durchlaufen, und das Produkt, der Preis und die Menge werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d68ce-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="d68ce-156">Das umgebende div-Element ist nur sichtbar, wenn das Detail Array mindestens ein Element enthält.</span><span class="sxs-lookup"><span data-stu-id="d68ce-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d68ce-157">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d68ce-157">Conclusion</span></span>

<span data-ttu-id="d68ce-158">In diesem Tutorial haben Sie eine Anwendung erstellt, die Entity Framework verwendet, um mit der Datenbank zu kommunizieren, und ASP.net-Web-API, um eine öffentlich ausgerichtete Schnittstelle oberhalb der Datenschicht bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="d68ce-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="d68ce-159">Wir verwenden ASP.NET MVC 4 zum Rendering der HTML-Seiten und Knockout. js Plus jQuery, um dynamische Interaktionen ohne Seiten Umladungen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="d68ce-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="d68ce-160">Zusätzliche Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="d68ce-160">Additional resources:</span></span>

- [<span data-ttu-id="d68ce-161">ASP.NET Datenzugriffs-Inhalts Zuordnung</span><span class="sxs-lookup"><span data-stu-id="d68ce-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="d68ce-162">Entity Framework Developer Center</span><span class="sxs-lookup"><span data-stu-id="d68ce-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="d68ce-163">Previous</span><span class="sxs-lookup"><span data-stu-id="d68ce-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
