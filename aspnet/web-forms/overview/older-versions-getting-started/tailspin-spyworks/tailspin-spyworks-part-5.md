---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Teil 5: Geschäftslogik | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 5 fügt einige Geschäftslogik hinzu.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511545"
---
# <a name="part-5-business-logic"></a><span data-ttu-id="1765a-104">Teil 5: Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="1765a-104">Part 5: Business Logic</span></span>

<span data-ttu-id="1765a-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1765a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="1765a-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1765a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="1765a-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="1765a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="1765a-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1765a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="1765a-109">Teil 5 fügt einige Geschäftslogik hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-109">Part 5 adds some business logic.</span></span>

## <a id="_Toc260221671"></a><span data-ttu-id="1765a-110">Hinzufügen von Geschäftslogik</span><span class="sxs-lookup"><span data-stu-id="1765a-110">Adding Some Business Logic</span></span>

<span data-ttu-id="1765a-111">Wir möchten, dass unsere Einkaufsmöglichkeiten immer verfügbar sind, wenn jemand unsere Website besucht.</span><span class="sxs-lookup"><span data-stu-id="1765a-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="1765a-112">Besucher können Elemente im Warenkorb durchsuchen und hinzufügen, auch wenn Sie nicht registriert oder angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="1765a-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="1765a-113">Wenn Sie bereit sind, sich zu authentifizieren, erhalten Sie die Möglichkeit, sich zu authentifizieren. Wenn Sie noch keine Mitglieder sind, können Sie ein Konto erstellen.</span><span class="sxs-lookup"><span data-stu-id="1765a-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="1765a-114">Dies bedeutet, dass wir die Logik implementieren müssen, um den Warenkorb von einem anonymen Zustand in den Zustand "registrierter Benutzer" zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="1765a-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="1765a-115">Erstellen Sie ein Verzeichnis mit dem Namen "classes", klicken Sie dann mit der rechten Maustaste auf den Ordner, und erstellen Sie eine neue Klassendatei mit dem Namen MyShoppingCart.cs.</span><span class="sxs-lookup"><span data-stu-id="1765a-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="1765a-116">Wie bereits erwähnt, erweitern wir die-Klasse, die die Seite "myshoppingcart. aspx" implementiert, und wir werden dies mit tun. Das leistungsfähige Konstrukt "partielle Klasse" von net.</span><span class="sxs-lookup"><span data-stu-id="1765a-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="1765a-117">Der generierte-Befehl für die MyShoppingCart.aspx.CF-Datei sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="1765a-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="1765a-118">Beachten Sie die Verwendung des Schlüssel Worts "Partial".</span><span class="sxs-lookup"><span data-stu-id="1765a-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="1765a-119">Die soeben generierte Klassendatei sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="1765a-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="1765a-120">Wir werden unsere Implementierungen zusammenführen, indem wir das partielle Schlüsselwort zu dieser Datei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1765a-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="1765a-121">Unsere neue Klassendatei sieht nun wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="1765a-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="1765a-122">Die erste Methode, die wir unserer Klasse hinzufügen werden, ist die "AddItem"-Methode.</span><span class="sxs-lookup"><span data-stu-id="1765a-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="1765a-123">Dies ist die Methode, die letztendlich aufgerufen wird, wenn der Benutzer auf der Seite Produktliste und Produkt Details auf die Links "Add to Art" klickt.</span><span class="sxs-lookup"><span data-stu-id="1765a-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="1765a-124">Fügen Sie Folgendes an die using-Anweisungen am oberen Rand der Seite an.</span><span class="sxs-lookup"><span data-stu-id="1765a-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="1765a-125">Und fügen Sie der myshoppingcart-Klasse diese Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="1765a-126">Wir verwenden LINQ to Entities, um festzustellen, ob das Element bereits im Warenkorb ist.</span><span class="sxs-lookup"><span data-stu-id="1765a-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="1765a-127">Wenn dies der Fall ist, aktualisieren wir die Bestellmenge des Elements, andernfalls erstellen wir einen neuen Eintrag für das ausgewählte Element.</span><span class="sxs-lookup"><span data-stu-id="1765a-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="1765a-128">Um diese Methode aufzurufen, implementieren wir eine AddTo Cart. aspx-Seite, die nicht nur diese Methode verwendet, sondern dann den aktuellen Warenkorb a =-Warenkorb angezeigt, nachdem das Element hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="1765a-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="1765a-129">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektmappennamen, und fügen Sie eine neue Seite mit dem Namen "AddTo Cart. aspx" hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="1765a-130">Diese Seite kann zwar verwendet werden, um Zwischenergebnisse wie niedrige Aktien Probleme anzuzeigen usw. in unserer Implementierung wird die Seite jedoch nicht tatsächlich gerendert, sondern ruft stattdessen die "Add"-Logik und die Umleitung auf.</span><span class="sxs-lookup"><span data-stu-id="1765a-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="1765a-131">Um dies zu erreichen, fügen Sie der Seite\_Lade Ereignis den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="1765a-132">Beachten Sie, dass das Produkt, das dem Warenkorb hinzugefügt wird, aus einem QueryString-Parameter abgerufen und die AddItem-Methode der Klasse aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1765a-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="1765a-133">Wenn keine Fehler auftreten, wird die Steuerung an die ShoppingCart. aspx-Seite weitergeleitet, die wir als nächstes vollständig implementieren werden.</span><span class="sxs-lookup"><span data-stu-id="1765a-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="1765a-134">Wenn ein Fehler vorliegt, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="1765a-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="1765a-135">Zurzeit haben wir noch keinen globalen Fehlerhandler implementiert, sodass diese Ausnahme von der Anwendung nicht behandelt wird. Wir werden dies jedoch in Kürze beheben.</span><span class="sxs-lookup"><span data-stu-id="1765a-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="1765a-136">Beachten Sie auch die Verwendung der Anweisung Debug. Fail () (verfügbar über `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="1765a-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="1765a-137">Wenn die Anwendung im Debugger ausgeführt wird, zeigt diese Methode ein detailliertes Dialogfeld mit Informationen zum Status der Anwendung zusammen mit der angegebenen Fehlermeldung an.</span><span class="sxs-lookup"><span data-stu-id="1765a-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="1765a-138">Bei der Ausführung in der Produktion wird die Debug. Fail ()-Anweisung ignoriert.</span><span class="sxs-lookup"><span data-stu-id="1765a-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="1765a-139">Beachten Sie im obigen Code einen Aufrufen einer Methode in unseren Warenkorb-Klassennamen "getshoppingcartid".</span><span class="sxs-lookup"><span data-stu-id="1765a-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="1765a-140">Fügen Sie den Code zum Implementieren der-Methode wie folgt hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="1765a-141">Beachten Sie, dass wir auch die Schaltflächen "Aktualisieren" und "Auschecken" sowie eine Bezeichnung hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="1765a-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="1765a-142">Wir können nun Artikel zum Warenkorb hinzufügen, aber wir haben die Logik zum Anzeigen des Warenkorbs nach dem Hinzufügen eines Produkts nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="1765a-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="1765a-143">Auf der Seite myshoppingcart. aspx fügen wir also ein EntityDataSource-Steuerelement und ein gridvire-Steuerelement wie folgt hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="1765a-144">Rufen Sie das Formular im Designer auf, damit Sie auf die Schaltfläche zum Aktualisieren des Einkaufswagens doppelklicken und den Click-Ereignishandler generieren können, der in der Deklaration im Markup angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="1765a-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="1765a-145">Die Details werden später implementiert, aber wir können die Anwendung ohne Fehler erstellen und ausführen.</span><span class="sxs-lookup"><span data-stu-id="1765a-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="1765a-146">Wenn Sie die Anwendung ausführen und dem Warenkorb ein Element hinzufügen, wird dies angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1765a-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="1765a-147">Beachten Sie, dass die "Standard"-Raster Anzeige durch die Implementierung von drei benutzerdefinierten Spalten getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="1765a-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="1765a-148">Der erste ist ein bearbeitbares, "gebundenes" Feld für die Menge:</span><span class="sxs-lookup"><span data-stu-id="1765a-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="1765a-149">Der nächste Schritt ist eine "berechnete" Spalte, die die Summe der Zeilen Elemente anzeigt (die Kosten für die Anzahl der zu befolgenden Elemente):</span><span class="sxs-lookup"><span data-stu-id="1765a-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="1765a-150">Schließlich haben wir eine benutzerdefinierte Spalte, die ein CheckBox-Steuerelement enthält, mit dem der Benutzer angeben kann, dass das Element aus dem Einkaufs Diagramm entfernt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1765a-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="1765a-151">Wie Sie sehen können, ist die Reihenfolge der Bestellungen leer, daher fügen wir eine Logik hinzu, um den Gesamtbetrag der Bestellung zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="1765a-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="1765a-152">Wir implementieren zuerst eine "GetTotal"-Methode in unsere myshoppingcart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="1765a-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="1765a-153">Fügen Sie in der Datei MyShoppingCart.cs den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="1765a-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="1765a-154">Anschließend können Sie auf der Seite\_Lade Ereignishandler unsere GetTotal-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="1765a-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="1765a-155">Gleichzeitig fügen wir einen Test hinzu, um zu überprüfen, ob der Warenkorb leer ist, und passen die Anzeige entsprechend an.</span><span class="sxs-lookup"><span data-stu-id="1765a-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="1765a-156">Wenn der Warenkorb nun leer ist, erhalten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="1765a-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="1765a-157">Wenn dies nicht der Wert ist, wird die Summe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1765a-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="1765a-158">Diese Seite ist jedoch noch nicht fertig.</span><span class="sxs-lookup"><span data-stu-id="1765a-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="1765a-159">Wir benötigen zusätzliche Logik, um den Warenkorb neu zu berechnen, indem Sie zum Entfernen markierte Elemente entfernen und neue Mengen Werte festlegen, da einige möglicherweise im Raster vom Benutzer geändert wurden.</span><span class="sxs-lookup"><span data-stu-id="1765a-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="1765a-160">Fügen Sie der Warenkorb-Klasse in MyShoppingCart.cs eine "RemoveItem"-Methode hinzu, um den Fall zu behandeln, wenn ein Benutzer ein Element zum Entfernen kennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="1765a-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="1765a-161">Nun sehen wir uns eine Methode an, die den Umstand behandelt, wenn ein Benutzer einfach die Qualität ändert, die in der GridView angeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="1765a-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="1765a-162">Mit den grundlegenden Features zum Entfernen und aktualisieren können wir die Logik implementieren, die tatsächlich den Warenkorb in der Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="1765a-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="1765a-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="1765a-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="1765a-164">Sie werden feststellen, dass diese Methode zwei Parameter erwartet.</span><span class="sxs-lookup"><span data-stu-id="1765a-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="1765a-165">Eine ist die Warenkorb-ID und die andere ein Array von Objekten des benutzerdefinierten Typs.</span><span class="sxs-lookup"><span data-stu-id="1765a-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="1765a-166">Um die Abhängigkeit unserer Logik in Bezug auf die Benutzeroberfläche möglichst gering zu halten, haben wir eine Datenstruktur definiert, die wir verwenden können, um die Warenkorb-Elemente an unseren Code zu übergeben, ohne dass die Methode direkt auf das GridView-Steuerelement zugreifen muss.</span><span class="sxs-lookup"><span data-stu-id="1765a-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="1765a-167">In unserer MyShoppingCart.aspx.cs-Datei können wir diese Struktur in unserem Click-Ereignishandler für die Update-Schaltfläche wie folgt verwenden.</span><span class="sxs-lookup"><span data-stu-id="1765a-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="1765a-168">Beachten Sie, dass wir nicht nur den Warenkorb aktualisieren, sondern auch die Summe des Warenkorbs.</span><span class="sxs-lookup"><span data-stu-id="1765a-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="1765a-169">Beachten Sie, dass diese Codezeile besonders interessiert ist:</span><span class="sxs-lookup"><span data-stu-id="1765a-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="1765a-170">GetValues () ist eine spezielle Hilfsfunktion, die in MyShoppingCart.aspx.cs wie folgt implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="1765a-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="1765a-171">Dies bietet eine saubere Möglichkeit, auf die Werte der gebundenen Elemente in unserem GridView-Steuerelement zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="1765a-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="1765a-172">Da das Kontrollkästchen "Element entfernen" nicht gebunden ist, wird es über die FindControl ()-Methode darauf zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="1765a-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="1765a-173">In dieser Phase der Projektentwicklung sind wir bereit, den Checkout-Prozess zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="1765a-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="1765a-174">Bevor Sie dies tun, verwenden wir Visual Studio zum Generieren der Mitgliedschafts Datenbank und zum Hinzufügen eines Benutzers zum Mitgliedschaftsrepository.</span><span class="sxs-lookup"><span data-stu-id="1765a-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1765a-175">[Zurück](tailspin-spyworks-part-4.md)
> [Weiter](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="1765a-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
