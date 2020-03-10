---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout. js | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447861"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="29ac6-102">Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout. js</span><span class="sxs-lookup"><span data-stu-id="29ac6-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="29ac6-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="29ac6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="29ac6-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="29ac6-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="29ac6-105">Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js</span><span class="sxs-lookup"><span data-stu-id="29ac6-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="29ac6-106">In diesem Abschnitt verwenden wir Knockout. js zum Hinzufügen von Funktionen zur Administrator Ansicht.</span><span class="sxs-lookup"><span data-stu-id="29ac6-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="29ac6-107">[Knockout. js](http://knockoutjs.com/) ist eine JavaScript-Bibliothek, die das Binden von HTML-Steuerelementen an Daten vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="29ac6-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="29ac6-108">Knockout. js verwendet das Model-View-ViewModel (MVVM)-Muster.</span><span class="sxs-lookup"><span data-stu-id="29ac6-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="29ac6-109">Das *Modell* ist die serverseitige Darstellung der Daten in der Geschäftsdomäne (in unserem Fall Produkte und Aufträge).</span><span class="sxs-lookup"><span data-stu-id="29ac6-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="29ac6-110">Die *Ansicht* ist die Darstellungs Schicht (HTML).</span><span class="sxs-lookup"><span data-stu-id="29ac6-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="29ac6-111">Das *View-Model* ist ein JavaScript-Objekt, das die Modelldaten enthält.</span><span class="sxs-lookup"><span data-stu-id="29ac6-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="29ac6-112">Das Ansichts Modell ist eine Code Abstraktion der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="29ac6-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="29ac6-113">Die HTML-Darstellung ist nicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="29ac6-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="29ac6-114">Stattdessen stellt Sie abstrakte Features der Sicht dar, z. b. "eine Liste von Elementen".</span><span class="sxs-lookup"><span data-stu-id="29ac6-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="29ac6-115">Die Ansicht ist Daten gebunden an das Ansichts Modell.</span><span class="sxs-lookup"><span data-stu-id="29ac6-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="29ac6-116">Updates für das Ansichts Modell werden automatisch in der Ansicht widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="29ac6-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="29ac6-117">Das View-Model ruft auch Ereignisse aus der Ansicht ab, z. b. Schaltflächen Klicks, und führt Vorgänge für das Modell aus, z. b. das Erstellen eines Auftrags.</span><span class="sxs-lookup"><span data-stu-id="29ac6-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="29ac6-118">Zuerst definieren wir das Ansichts Modell.</span><span class="sxs-lookup"><span data-stu-id="29ac6-118">First we'll define the view-model.</span></span> <span data-ttu-id="29ac6-119">Danach binden wir das HTML-Markup an das Ansichts Modell.</span><span class="sxs-lookup"><span data-stu-id="29ac6-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="29ac6-120">Fügen Sie admin. cshtml den folgenden Razor-Abschnitt hinzu:</span><span class="sxs-lookup"><span data-stu-id="29ac6-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="29ac6-121">Sie können diesen Abschnitt an beliebiger Stelle in der Datei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="29ac6-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="29ac6-122">Wenn die Ansicht gerendert wird, wird der Abschnitt unten auf der HTML-Seite direkt vor dem schließenden &lt;/Body&gt; Tag angezeigt.</span><span class="sxs-lookup"><span data-stu-id="29ac6-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="29ac6-123">Alle Skripts für diese Seite werden innerhalb des Skripttags angezeigt, das durch den Kommentar angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="29ac6-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="29ac6-124">Definieren Sie zunächst eine Ansichts Modell Klasse:</span><span class="sxs-lookup"><span data-stu-id="29ac6-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="29ac6-125">**ko. observablearray** ist eine besondere Art von Objekt in Knockout, das als *Observable*bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="29ac6-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="29ac6-126">Aus der [Knockout. js-Dokumentation](http://knockoutjs.com/documentation/observables.html): ein Observable-Objekt ist ein JavaScript-Objekt, mit dem Abonnenten über Änderungen benachrichtigt werden können.</span><span class="sxs-lookup"><span data-stu-id="29ac6-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="29ac6-127">Wenn sich der Inhalt einer wahrnehmbaren Änderung ändert, wird die Ansicht automatisch entsprechend aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="29ac6-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="29ac6-128">Um das `products` Array aufzufüllen, stellen Sie eine AJAX-Anforderung an die Web-API.</span><span class="sxs-lookup"><span data-stu-id="29ac6-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="29ac6-129">Erinnern Sie sich daran, dass wir den Basis-URI für die API im Ansichts Behälter gespeichert haben (siehe [Teil 4](using-web-api-with-entity-framework-part-4.md) des Tutorials).</span><span class="sxs-lookup"><span data-stu-id="29ac6-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="29ac6-130">Fügen Sie als nächstes dem Ansichts Modell Funktionen zum Erstellen, aktualisieren und Löschen von Produkten hinzu.</span><span class="sxs-lookup"><span data-stu-id="29ac6-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="29ac6-131">Diese Funktionen übermitteln AJAX-Aufrufe an die Web-API und verwenden die Ergebnisse, um das Ansichts Modell zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="29ac6-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="29ac6-132">Der wichtigste Teil: Wenn das DOM geladen ist, wird die Funktion " **ko. applybinding** " aufgerufen und eine neue Instanz der `ProductsViewModel`übergeben:</span><span class="sxs-lookup"><span data-stu-id="29ac6-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="29ac6-133">Die Methode " **ko. applybinding** " aktiviert Knockout und verbindet das Ansichts Modell mit der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="29ac6-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="29ac6-134">Nachdem wir nun über ein Ansichts Modell verfügen, können wir die Bindungen erstellen.</span><span class="sxs-lookup"><span data-stu-id="29ac6-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="29ac6-135">In Knockout. js fügen Sie den HTML-Elementen `data-bind` Attribute hinzu.</span><span class="sxs-lookup"><span data-stu-id="29ac6-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="29ac6-136">Um z. b. eine HTML-Liste an ein Array zu binden, verwenden Sie die `foreach` Bindung:</span><span class="sxs-lookup"><span data-stu-id="29ac6-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="29ac6-137">Die `foreach` Bindung durchläuft das Array und erstellt untergeordnete Elemente für jedes Objekt im Array.</span><span class="sxs-lookup"><span data-stu-id="29ac6-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="29ac6-138">Bindungen für die untergeordneten Elemente können auf Eigenschaften der Array Objekte verweisen.</span><span class="sxs-lookup"><span data-stu-id="29ac6-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="29ac6-139">Fügen Sie der Liste "Update-Products" die folgenden Bindungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="29ac6-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="29ac6-140">Das `<li>`-Element tritt innerhalb des Gültigkeits Bereichs der **foreach** -Bindung auf.</span><span class="sxs-lookup"><span data-stu-id="29ac6-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="29ac6-141">Das bedeutet, dass Knockout das Element einmal für jedes Produkt im `products` Array Rendering wird.</span><span class="sxs-lookup"><span data-stu-id="29ac6-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="29ac6-142">Alle Bindungen im `<li>`-Element verweisen auf diese Produkt Instanz.</span><span class="sxs-lookup"><span data-stu-id="29ac6-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="29ac6-143">`$data.Name` bezieht sich z. b. auf die `Name`-Eigenschaft des Produkts.</span><span class="sxs-lookup"><span data-stu-id="29ac6-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="29ac6-144">Um die Werte der Texteingaben festzulegen, verwenden Sie die `value` Bindung.</span><span class="sxs-lookup"><span data-stu-id="29ac6-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="29ac6-145">Die Schaltflächen werden mithilfe der `click` Bindung an Funktionen in der Modell Ansicht gebunden.</span><span class="sxs-lookup"><span data-stu-id="29ac6-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="29ac6-146">Die Product-Instanz wird als Parameter an jede Funktion übergeben.</span><span class="sxs-lookup"><span data-stu-id="29ac6-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="29ac6-147">Weitere Informationen finden Sie in der [Knockout. js-Dokumentation](http://knockoutjs.com/documentation/observables.html) mit guten Beschreibungen der verschiedenen Bindungen.</span><span class="sxs-lookup"><span data-stu-id="29ac6-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="29ac6-148">Fügen Sie als nächstes eine Bindung für das **Sende Ereignis auf dem Formular** "Produkt hinzufügen" hinzu:</span><span class="sxs-lookup"><span data-stu-id="29ac6-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="29ac6-149">Diese Bindung Ruft die `create`-Funktion für das Ansichts Modell auf, um ein neues Produkt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="29ac6-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="29ac6-150">Im folgenden finden Sie den gesamten Code für die Administrator Ansicht:</span><span class="sxs-lookup"><span data-stu-id="29ac6-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="29ac6-151">Führen Sie die Anwendung aus, melden Sie sich mit dem Administrator Konto an, und klicken Sie auf den Link "admin".</span><span class="sxs-lookup"><span data-stu-id="29ac6-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="29ac6-152">Sie sollten die Liste der Produkte sehen und Produkte erstellen, aktualisieren oder löschen können.</span><span class="sxs-lookup"><span data-stu-id="29ac6-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="29ac6-153">[Zurück](using-web-api-with-entity-framework-part-4.md)
> [Weiter](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="29ac6-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
