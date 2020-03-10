---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Teil 8: Einkaufswagen mit AJAX-Updates | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 8 umfasst den Warenkorb mit AJAX-Updates.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433515"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="689b3-104">Teil 8: Einkaufswagen mit AJAX-Updates</span><span class="sxs-lookup"><span data-stu-id="689b3-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="689b3-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="689b3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="689b3-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="689b3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="689b3-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="689b3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="689b3-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="689b3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="689b3-109">Teil 8 umfasst den Warenkorb mit AJAX-Updates.</span><span class="sxs-lookup"><span data-stu-id="689b3-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>

<span data-ttu-id="689b3-110">Wir gestatten Benutzern das Platzieren von Alben in Ihrem Warenkorb, ohne sich zu registrieren, aber Sie müssen sich als Gäste registrieren, um das Auschecken abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="689b3-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="689b3-111">Der Einkaufs-und Auscheck Prozess wird in zwei Controller unterteilt: einen ShoppingCart-Controller, der das anonyme Hinzufügen von Elementen zu einem Warenkorb ermöglicht, und einen Checkout-Controller, der den Checkout Prozess verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="689b3-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="689b3-112">Wir beginnen mit dem Warenkorb in diesem Abschnitt und erstellen dann den Checkout-Prozess im folgenden Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="689b3-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="689b3-113">Hinzufügen der Modellklassen Warenkorb, Order und OrderDetail</span><span class="sxs-lookup"><span data-stu-id="689b3-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="689b3-114">In unseren Warenkorb-und Checkout-Prozessen werden einige neue Klassen verwendet.</span><span class="sxs-lookup"><span data-stu-id="689b3-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="689b3-115">Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine Warenkorb-Klasse (Cart.cs) mit dem folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="689b3-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="689b3-116">Diese Klasse ähnelt den anderen, die bisher verwendet wurden, mit Ausnahme des [key]-Attributs für die RecordID-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="689b3-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="689b3-117">Unsere Warenkorb-Elemente verfügen über einen Zeichen folgen Bezeichner namens "cartId", um anonyme Einkäufe zuzulassen, aber die Tabelle enthält einen ganzzahligen Primärschlüssel namens "recordID".</span><span class="sxs-lookup"><span data-stu-id="689b3-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="689b3-118">Gemäß der Konvention erwartet Entity Framework Code zunächst, dass der Primärschlüssel für eine Tabelle mit dem Namen "Cart" entweder "cartId" oder "ID" ist, aber wir können dies problemlos über Anmerkungen oder Code überschreiben.</span><span class="sxs-lookup"><span data-stu-id="689b3-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="689b3-119">Dies ist ein Beispiel dafür, wie wir die einfachen Konventionen in Entity Framework Code verwenden können, wenn Sie sich an uns wenden, aber wir werden nicht durch Sie eingeschränkt, wenn dies nicht der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="689b3-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="689b3-120">Fügen Sie als nächstes eine Order-Klasse (Order.cs) mit dem folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="689b3-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="689b3-121">Diese Klasse verfolgt Zusammenfassungs-und Übermittlungs Informationen für eine Bestellung.</span><span class="sxs-lookup"><span data-stu-id="689b3-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="689b3-122">Die **Kompilierung erfolgt noch nicht**, da Sie eine OrderDetails-Navigations Eigenschaft aufweist, die von einer Klasse abhängt, die noch nicht erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="689b3-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="689b3-123">Beheben Sie dies jetzt, indem Sie eine Klasse mit dem Namen "OrderDetail.cs" hinzufügen und den folgenden Code hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="689b3-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="689b3-124">Wir nehmen ein letztes Update für die Klasse "musicstoreentities" vor, um dbsets aufzunehmen, die diese neuen Modellklassen verfügbar machen, einschließlich einer dbset-&lt;Künstler&gt;.</span><span class="sxs-lookup"><span data-stu-id="689b3-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="689b3-125">Die aktualisierte Klasse "musicstoreentities" wird wie unten dargestellt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="689b3-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="689b3-126">Verwalten der Einkaufswagen-Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="689b3-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="689b3-127">Als Nächstes erstellen wir die Klasse "ShoppingCart" im Ordner "Models".</span><span class="sxs-lookup"><span data-stu-id="689b3-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="689b3-128">Das ShoppingCart-Modell verarbeitet den Datenzugriff auf die Warenkorb-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="689b3-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="689b3-129">Außerdem wird die Geschäftslogik zum Hinzufügen und Entfernen von Elementen aus dem Warenkorb behandelt.</span><span class="sxs-lookup"><span data-stu-id="689b3-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="689b3-130">Da es nicht erforderlich sein soll, dass sich Benutzer für ein Konto registrieren, um dem Warenkorb Elemente hinzuzufügen, weisen wir Benutzern einen temporären eindeutigen Bezeichner (mit einer GUID oder Globally Unique Identifier) zu, wenn Sie auf den Warenkorb zugreifen.</span><span class="sxs-lookup"><span data-stu-id="689b3-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="689b3-131">Wir speichern diese ID mithilfe der ASP.NET Session-Klasse.</span><span class="sxs-lookup"><span data-stu-id="689b3-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="689b3-132">*Hinweis: die ASP.NET-Sitzung ist ein bequemer Ort zum Speichern Benutzer spezifischer Informationen, die ablaufen, nachdem Sie die Website verlassen haben. Obwohl die Verwendung des Sitzungs Zustands die Auswirkungen auf die Leistung von größeren Standorten haben kann, ist unsere helle Verwendung für Demonstrationszwecke gut geeignet.*</span><span class="sxs-lookup"><span data-stu-id="689b3-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="689b3-133">Die ShoppingCart-Klasse stellt die folgenden Methoden zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="689b3-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="689b3-134">**AddTo Cart** nimmt ein Album als Parameter an und fügt es dem Warenkorb des Benutzers hinzu.</span><span class="sxs-lookup"><span data-stu-id="689b3-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="689b3-135">Da die Warenkorb-Tabelle die Menge für jedes Album nachverfolgt, umfasst Sie Logik, um bei Bedarf eine neue Zeile zu erstellen, oder die Menge zu erhöhen, wenn der Benutzer bereits eine Kopie des Albums angeordnet hat.</span><span class="sxs-lookup"><span data-stu-id="689b3-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="689b3-136">**Removefromcart** nimmt eine Album-ID an und entfernt Sie aus dem Warenkorb des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="689b3-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="689b3-137">Wenn der Benutzer nur eine Kopie des Albums in seinem Warenkorb besaß, wird die Zeile entfernt.</span><span class="sxs-lookup"><span data-stu-id="689b3-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="689b3-138">**Emptycart** entfernt alle Elemente aus dem Einkaufswagen eines Benutzers.</span><span class="sxs-lookup"><span data-stu-id="689b3-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="689b3-139">" **Getcartitems** " Ruft eine Liste von "kartitems" zur Anzeige oder Verarbeitung ab.</span><span class="sxs-lookup"><span data-stu-id="689b3-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="689b3-140">**GetCount** Ruft die Gesamtzahl der Alben ab, die ein Benutzer in seinem Warenkorb hat.</span><span class="sxs-lookup"><span data-stu-id="689b3-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="689b3-141">**GetTotal** berechnet die Gesamtkosten aller Elemente im Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="689b3-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="689b3-142">" **Anateorder** " konvertiert den Einkaufswagen in eine Bestellung während der Auscheck Phase.</span><span class="sxs-lookup"><span data-stu-id="689b3-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="689b3-143">**Getcart** ist eine statische Methode, mit der unsere Controller ein Warenkorb-Objekt abrufen können.</span><span class="sxs-lookup"><span data-stu-id="689b3-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="689b3-144">Er verwendet die **getcartid** -Methode, um das Lesen der "cartId" aus der Sitzung des Benutzers zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="689b3-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="689b3-145">Die getcartid-Methode erfordert HttpContextBase, damit Sie die cartId des Benutzers aus der Benutzersitzung lesen kann.</span><span class="sxs-lookup"><span data-stu-id="689b3-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="689b3-146">Hier ist die komplette **ShoppingCart-Klasse**:</span><span class="sxs-lookup"><span data-stu-id="689b3-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="689b3-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="689b3-147">ViewModels</span></span>

<span data-ttu-id="689b3-148">Der Warenkorb des Warenkorb muss einige komplexe Informationen an seine Ansichten übermitteln, die den Modell Objekten nicht ordnungsgemäß zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="689b3-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="689b3-149">Wir möchten unsere Modelle nicht so ändern, dass Sie an unsere Ansichten angepasst werden. Modellklassen sollten unsere Domäne und nicht die Benutzeroberfläche darstellen.</span><span class="sxs-lookup"><span data-stu-id="689b3-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="689b3-150">Eine Lösung besteht darin, die Informationen mithilfe der viewbag-Klasse an unsere Ansichten weiterzugeben, wie dies bei den Dropdown Informationen des Store-Managers der Fall ist, aber das Übergeben von vielen Informationen über viewbag ist schwierig zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="689b3-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="689b3-151">Eine Lösung hierfür ist die Verwendung des *ViewModel* -Musters.</span><span class="sxs-lookup"><span data-stu-id="689b3-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="689b3-152">Bei Verwendung dieses Musters erstellen wir stark typisierte Klassen, die für unsere spezifischen Ansichts Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte und Inhalte verfügbar machen, die von unseren Ansichts Vorlagen benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="689b3-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="689b3-153">Unsere Controller Klassen können diese Ansichts optimierten Klassen dann auffüllen und an unsere Ansichts Vorlage übergeben, um Sie zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="689b3-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="689b3-154">Dies ermöglicht Typsicherheit, Kompilierungszeit Überprüfung und Editor-IntelliSense innerhalb von Ansichts Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="689b3-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="689b3-155">Wir erstellen zwei Ansichts Modelle für die Verwendung in unserem Warenkorb-Controller: das shoppingcartviewmodel speichert den Inhalt des Einkaufswagens des Benutzers, und das shoppingcartremuveviewmodel wird verwendet, um Bestätigungs Informationen anzuzeigen, wenn ein Benutzer etwas entfernt. aus dem Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="689b3-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="689b3-156">Erstellen Sie im Stammverzeichnis des Projekts einen neuen Ordner "ViewModels", um die Aktivitäten zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="689b3-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="689b3-157">Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie Hinzufügen/neuer Ordner.</span><span class="sxs-lookup"><span data-stu-id="689b3-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="689b3-158">Nennen Sie den Ordner ViewModels.</span><span class="sxs-lookup"><span data-stu-id="689b3-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="689b3-159">Fügen Sie als nächstes die Klasse "shoppingcartviewmodel" im Ordner "ViewModels" hinzu.</span><span class="sxs-lookup"><span data-stu-id="689b3-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="689b3-160">Sie verfügt über zwei Eigenschaften: eine Liste von waren Korb Elementen und einen Dezimalwert, der den Gesamtpreis für alle Artikel im Warenkorb enthält.</span><span class="sxs-lookup"><span data-stu-id="689b3-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="689b3-161">Fügen Sie nun das shoppingcartremuveviewmodel dem Ordner "ViewModels" mit den folgenden vier Eigenschaften hinzu.</span><span class="sxs-lookup"><span data-stu-id="689b3-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="689b3-162">Einkaufswagen Controller</span><span class="sxs-lookup"><span data-stu-id="689b3-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="689b3-163">Der Einkaufswagen Controller hat drei Hauptaufgaben: das Hinzufügen von Elementen zu einem Warenkorb, das Entfernen von Elementen aus dem Warenkorb und das Anzeigen von Elementen im Warenkorb.</span><span class="sxs-lookup"><span data-stu-id="689b3-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="689b3-164">Dabei werden die drei soeben erstellten Klassen verwendet: shoppingcartviewmodel, shoppingcartremuveviewmodel und ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="689b3-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="689b3-165">Wie bei StoreController und storemanagercontroller fügen wir ein Feld hinzu, um eine Instanz von "musicstoreentities" zu speichern.</span><span class="sxs-lookup"><span data-stu-id="689b3-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="689b3-166">Fügen Sie dem Projekt einen neuen Einkaufswagen Controller mit der leeren Controller Vorlage hinzu.</span><span class="sxs-lookup"><span data-stu-id="689b3-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="689b3-167">Hier ist der komplette ShoppingCart-Controller.</span><span class="sxs-lookup"><span data-stu-id="689b3-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="689b3-168">Die Aktionen "index" und "Controller hinzufügen" sollten sehr vertraut aussehen.</span><span class="sxs-lookup"><span data-stu-id="689b3-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="689b3-169">Die Remove-und cartsummary-Controller Aktionen behandeln zwei Sonderfälle, die im folgenden Abschnitt erläutert werden.</span><span class="sxs-lookup"><span data-stu-id="689b3-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="689b3-170">AJAX-Updates mit jQuery</span><span class="sxs-lookup"><span data-stu-id="689b3-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="689b3-171">Im nächsten Schritt erstellen wir eine waren Korb Index Seite, die stark an das shoppingcartviewmodel typisiert ist und die Listen Ansichts Vorlage verwendet, die dieselbe Methode wie zuvor verwendet.</span><span class="sxs-lookup"><span data-stu-id="689b3-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="689b3-172">Anstatt jedoch einen HTML. Action Link zum Entfernen von Elementen aus dem Warenkorb zu verwenden, verwenden wir jQuery, um das Click-Ereignis für alle Links in dieser Ansicht zu übertragen, die über die HTML-Klasse removelink verfügen.</span><span class="sxs-lookup"><span data-stu-id="689b3-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="689b3-173">Anstatt das Formular zu veröffentlichen, stellt dieser Click-Ereignishandler nur einen AJAX-Rückruf an unsere removefromcart-Controller Aktion.</span><span class="sxs-lookup"><span data-stu-id="689b3-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="689b3-174">Removefromcart gibt ein serialisiertes JSON-Ergebnis zurück, das der jQuery-Rückruf dann analysiert und vier schnelle Aktualisierungen der Seite mithilfe von jQuery durchführt:</span><span class="sxs-lookup"><span data-stu-id="689b3-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="689b3-175">Entfernt das gelöschte Album aus der Liste.</span><span class="sxs-lookup"><span data-stu-id="689b3-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="689b3-176">Aktualisiert die Anzahl der waren Korb in der Kopfzeile.</span><span class="sxs-lookup"><span data-stu-id="689b3-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="689b3-177">Zeigt dem Benutzer eine Aktualisierungs Meldung an.</span><span class="sxs-lookup"><span data-stu-id="689b3-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="689b3-178">Aktualisiert den Gesamtpreis des Warenkorbs</span><span class="sxs-lookup"><span data-stu-id="689b3-178">Updates the cart total price</span></span>

<span data-ttu-id="689b3-179">Da das Entfernungs Szenario von einem AJAX-Rückruf innerhalb der Index Ansicht behandelt wird, benötigen wir keine zusätzliche Ansicht für die removefromcart-Aktion.</span><span class="sxs-lookup"><span data-stu-id="689b3-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="689b3-180">Im folgenden finden Sie den gesamten Code für die/ShoppingCart/Index-Sicht:</span><span class="sxs-lookup"><span data-stu-id="689b3-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="689b3-181">Um dies zu testen, müssen wir in der Lage sein, dem Warenkorb Elemente hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="689b3-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="689b3-182">Wir aktualisieren die Ansicht " **Store-Details** ", um die Schaltfläche "zum Warenkorb hinzufügen" einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="689b3-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="689b3-183">Während wir uns befinden, können wir einige der zusätzlichen Informationen des Albums hinzufügen, die wir seit der letzten Aktualisierung dieser Ansicht hinzugefügt haben: Genre, Künstler, Preis und Album Kunst.</span><span class="sxs-lookup"><span data-stu-id="689b3-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="689b3-184">Der aktualisierte Code für die Details der Speicher Details wird wie unten dargestellt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="689b3-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="689b3-185">Nun können wir durch den Store klicken und testen, wie Sie Alben zu und aus dem Warenkorb hinzufügen und daraus entfernen.</span><span class="sxs-lookup"><span data-stu-id="689b3-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="689b3-186">Führen Sie die Anwendung aus, und navigieren Sie zum Store-Index.</span><span class="sxs-lookup"><span data-stu-id="689b3-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="689b3-187">Klicken Sie anschließend auf ein Genre, um eine Liste der Alben anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="689b3-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="689b3-188">Wenn Sie auf einen Album Titel klicken, wird nun unsere aktualisierte Ansicht mit den Album Details angezeigt, einschließlich der Schaltfläche "zu Warenkorb hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="689b3-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="689b3-189">Wenn Sie auf die Schaltfläche "zum Warenkorb hinzufügen" klicken, wird unsere waren Korb Index Ansicht mit der zusammenfassenden Liste Warenkorb angezeigt.</span><span class="sxs-lookup"><span data-stu-id="689b3-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="689b3-190">Nachdem Sie den Warenkorb geladen haben, können Sie auf den Link aus Warenkorb entfernen klicken, um das AJAX-Update für Ihren Warenkorb anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="689b3-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="689b3-191">Wir haben einen funktionierenden Einkaufswagen erstellt, der es nicht registrierten Benutzern ermöglicht, Ihrem Warenkorb Elemente hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="689b3-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="689b3-192">Im folgenden Abschnitt können wir Sie registrieren und den Auscheck Prozess vervollständigen.</span><span class="sxs-lookup"><span data-stu-id="689b3-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="689b3-193">[Zurück](mvc-music-store-part-7.md)
> [Weiter](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="689b3-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
