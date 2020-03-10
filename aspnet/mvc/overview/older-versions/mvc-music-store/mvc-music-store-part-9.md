---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Teil 9: Registrierung und Checkout | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 9 umfasst die Registrierung und das Auschecken.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450897"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="b8acc-104">Teil 9: Registrierung und Checkout</span><span class="sxs-lookup"><span data-stu-id="b8acc-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="b8acc-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b8acc-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b8acc-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b8acc-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b8acc-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="b8acc-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b8acc-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="b8acc-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b8acc-109">Teil 9 umfasst die Registrierung und das Auschecken.</span><span class="sxs-lookup"><span data-stu-id="b8acc-109">Part 9 covers Registration and Checkout.</span></span>

<span data-ttu-id="b8acc-110">In diesem Abschnitt erstellen wir einen checkoutcontroller, der die Adresse des Kunden und die Zahlungsinformationen sammelt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="b8acc-111">Wir verlangen, dass sich Benutzer vor dem Auschecken bei unserer Website registrieren, sodass für diesen Controller eine Autorisierung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="b8acc-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="b8acc-112">Benutzer navigieren in Ihrem Warenkorb zum Checkout-Prozess, indem Sie auf die Schaltfläche "Auschecken" klicken.</span><span class="sxs-lookup"><span data-stu-id="b8acc-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="b8acc-113">Wenn der Benutzer nicht angemeldet ist, wird er dazu aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="b8acc-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="b8acc-114">Nach erfolgreicher Anmeldung wird dem Benutzer die Ansicht Adresse und Zahlung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="b8acc-115">Nachdem Sie das Formular ausgefüllt und die Bestellung übermittelt haben, wird Ihnen der Bestätigungsbildschirm angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="b8acc-116">Wenn Sie versuchen, eine nicht vorhandene Bestellung oder eine Bestellung, die nicht zu gehört, anzuzeigen, wird die Fehler Ansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="b8acc-117">Migrieren des Warenkorbs</span><span class="sxs-lookup"><span data-stu-id="b8acc-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="b8acc-118">Der Einkaufsprozess ist anonym, wenn der Benutzer auf die Schaltfläche "Auschecken" klickt, muss er sich registrieren und anmelden.</span><span class="sxs-lookup"><span data-stu-id="b8acc-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="b8acc-119">Benutzer erwarten, dass die Warenkorb-Informationen zwischen den besuchen aufbewahrt werden. Daher müssen wir die Warenkorb-Informationen einem Benutzer zuordnen, wenn Sie die Registrierung oder Anmeldung durchführen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="b8acc-120">Dies ist sehr einfach, da die ShoppingCart-Klasse bereits über eine-Methode verfügt, die alle Elemente im aktuellen Warenkorb einem Benutzernamen zuordnet.</span><span class="sxs-lookup"><span data-stu-id="b8acc-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="b8acc-121">Diese Methode muss nur aufgerufen werden, wenn ein Benutzer die Registrierung oder Anmeldung abschließt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="b8acc-122">Öffnen Sie die **AccountController** -Klasse, die wir beim Einrichten der Mitgliedschaft und Autorisierung hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="b8acc-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="b8acc-123">Fügen Sie eine using-Anweisung hinzu, die auf mvcmusicstore. Models verweist, und fügen Sie die folgende MigrateShoppingCart-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="b8acc-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="b8acc-124">Ändern Sie als nächstes die Aktion Logon Post so, dass MigrateShoppingCart aufgerufen wird, nachdem der Benutzer überprüft wurde, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="b8acc-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="b8acc-125">Nehmen Sie die gleiche Änderung an der Registrierungs Beitrags Aktion vor, unmittelbar nachdem das Benutzerkonto erfolgreich erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="b8acc-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="b8acc-126">Das ist nun der Fall: ein anonymer Warenkorb wird nach erfolgreicher Registrierung oder Anmeldung automatisch an ein Benutzerkonto übertragen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="b8acc-127">Erstellen von checkoutcontroller</span><span class="sxs-lookup"><span data-stu-id="b8acc-127">Creating the CheckoutController</span></span>

<span data-ttu-id="b8acc-128">Klicken Sie mit der rechten Maustaste auf den Ordner Controllers, und fügen Sie dem Projekt mit dem Namen checkoutcontroller einen neuen Controller mit der leeren Controller Vorlage hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8acc-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="b8acc-129">Fügen Sie zunächst das Autorisierungs Attribut oberhalb der Controller Klassen Deklaration hinzu, damit Benutzer vor dem Auschecken registriert werden:</span><span class="sxs-lookup"><span data-stu-id="b8acc-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="b8acc-130">*Hinweis: Dies ähnelt der Änderung, die wir zuvor an storemanagercontroller vorgenommen haben. in diesem Fall erforderte das Autorisierungs Attribut jedoch, dass der Benutzer in einer Administrator Rolle angemeldet ist. Im Checkout-Controller ist es erforderlich, dass der Benutzer angemeldet ist, aber nicht als Administrator angemeldet ist.*</span><span class="sxs-lookup"><span data-stu-id="b8acc-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="b8acc-131">Der Einfachheit halber beschäftigen wir uns in diesem Tutorial nicht mit Zahlungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="b8acc-132">Stattdessen erlauben wir Benutzern das Auschecken mithilfe eines Aktionscodes.</span><span class="sxs-lookup"><span data-stu-id="b8acc-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="b8acc-133">Wir speichern diesen Aktions Code mit einer Konstanten namens "Promocode".</span><span class="sxs-lookup"><span data-stu-id="b8acc-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="b8acc-134">Wie im StoreController deklarieren wir ein Feld für eine Instanz der Klasse "musicstoreentities" mit dem Namen "storedb".</span><span class="sxs-lookup"><span data-stu-id="b8acc-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="b8acc-135">Damit die Klasse "musicstoreentities" verwendet werden kann, müssen wir eine using-Anweisung für den Namespace "mvcmusicstore. Models" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="b8acc-136">Der obere Rand des Checkout-Controllers wird unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="b8acc-137">Der checkoutcontroller verfügt über die folgenden Controller Aktionen:</span><span class="sxs-lookup"><span data-stu-id="b8acc-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="b8acc-138">**Addressandpayment (Get-Methode)** zeigt ein Formular an, das es dem Benutzer ermöglicht, seine Informationen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="b8acc-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="b8acc-139">**Addressandpayment (Post-Methode)** überprüft die Eingabe und verarbeitet die Bestellung.</span><span class="sxs-lookup"><span data-stu-id="b8acc-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="b8acc-140">Der **Abschluss** wird angezeigt, nachdem ein Benutzer den Auscheck Vorgang erfolgreich abgeschlossen hat.</span><span class="sxs-lookup"><span data-stu-id="b8acc-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="b8acc-141">In dieser Ansicht wird die Bestellnummer des Benutzers als Bestätigung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="b8acc-142">Benennen wir zuerst die Index Controller Aktion (die bei der Erstellung des Controllers generiert wurde) in addressandpayment um.</span><span class="sxs-lookup"><span data-stu-id="b8acc-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="b8acc-143">Diese Controller Aktion zeigt nur das Checkout-Formular an, sodass keine Modellinformationen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="b8acc-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="b8acc-144">Unsere addressandpayment-Post-Methode folgt demselben Muster, das wir im storemanagercontroller verwendet haben: Es wird versucht, die Übermittlung des Formulars zu akzeptieren und die Bestellung abzuschließen, und das Formular wird erneut angezeigt, wenn es fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="b8acc-145">Nachdem die Überprüfung der Formulareingabe unsere Überprüfungsanforderungen für eine Bestellung erfüllt hat, wird der Promocode-Formular Wert direkt überprüft.</span><span class="sxs-lookup"><span data-stu-id="b8acc-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="b8acc-146">Wenn alles richtig ist, speichern wir die aktualisierten Informationen in der Reihenfolge, geben das ShoppingCart-Objekt an, um den Bestellvorgang abzuschließen, und leiten zur vollständigen Aktion um.</span><span class="sxs-lookup"><span data-stu-id="b8acc-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="b8acc-147">Nach erfolgreichem Abschluss des Auscheck Vorgangs werden die Benutzer zur vollständigen Controller Aktion umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="b8acc-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="b8acc-148">Durch diese Aktion wird eine einfache Überprüfung ausgeführt, um zu überprüfen, ob die Bestellung tatsächlich dem angemeldeten Benutzer angehört, bevor die Bestellnummer als Bestätigung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b8acc-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="b8acc-149">*Hinweis: die Fehler Ansicht wurde automatisch für uns im Ordner/views/Shared erstellt, als wir das Projekt gestartet haben.*</span><span class="sxs-lookup"><span data-stu-id="b8acc-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="b8acc-150">Der gesamte checkoutcontroller-Code lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b8acc-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="b8acc-151">Hinzufügen der addressandpayment-Ansicht</span><span class="sxs-lookup"><span data-stu-id="b8acc-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="b8acc-152">Nun erstellen wir die Ansicht "addressandpayment".</span><span class="sxs-lookup"><span data-stu-id="b8acc-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="b8acc-153">Klicken Sie mit der rechten Maustaste auf eine der addressandpayment-Controller Aktionen, und fügen Sie eine Ansicht mit dem Namen addressandpayment hinzu, die als Bestellung stark typisiert ist und die Bearbeitungs Vorlage verwendet, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="b8acc-154">In dieser Ansicht werden zwei der Techniken verwendet, die wir beim Erstellen der storemanageredit-Sicht betrachtet haben:</span><span class="sxs-lookup"><span data-stu-id="b8acc-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="b8acc-155">Wir verwenden HTML. editorformodel (), um Formularfelder für das Bestell Modell anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="b8acc-156">Wir nutzen Validierungsregeln mithilfe einer Order-Klasse mit Validierungs Attributen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="b8acc-157">Wir beginnen mit dem Aktualisieren des Formular Codes, um "HTML. Editor formodel ()" zu verwenden, gefolgt von einem zusätzlichen Textfeld für den Promo-Code.</span><span class="sxs-lookup"><span data-stu-id="b8acc-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="b8acc-158">Der gesamte Code für die Ansicht addressandpayment ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="b8acc-159">Definieren von Validierungsregeln für die Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="b8acc-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="b8acc-160">Nachdem unsere Ansicht eingerichtet ist, richten wir die Validierungsregeln für das Bestell Modell wie zuvor für das Album Modell ein.</span><span class="sxs-lookup"><span data-stu-id="b8acc-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="b8acc-161">Klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine Klasse namens Order hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8acc-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="b8acc-162">Zusätzlich zu den Validierungs Attributen, die wir zuvor für das Album verwendet haben, wird auch ein regulärer Ausdruck verwendet, um die e-Mail-Adresse des Benutzers zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="b8acc-163">Wenn Sie versuchen, das Formular mit fehlenden oder ungültigen Informationen zu senden, wird jetzt eine Fehlermeldung mit der Client seitigen Validierung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="b8acc-164">Okay, wir haben den größten Teil der Arbeit für den Auscheck Prozess abgeschlossen. Wir haben nur ein paar Chancen und enden an der Fertigstellung.</span><span class="sxs-lookup"><span data-stu-id="b8acc-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="b8acc-165">Wir müssen zwei einfache Ansichten hinzufügen, und wir müssen die Übergabe der Warenkorb-Informationen während des Anmeldevorgangs durchführen.</span><span class="sxs-lookup"><span data-stu-id="b8acc-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="b8acc-166">Hinzufügen der Ansicht "Checkout vervollständigen"</span><span class="sxs-lookup"><span data-stu-id="b8acc-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="b8acc-167">Die Ansicht "Auschecken vervollständigen" ist recht einfach, da Sie lediglich die Bestell-ID anzeigen muss.</span><span class="sxs-lookup"><span data-stu-id="b8acc-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="b8acc-168">Klicken Sie mit der rechten Maustaste auf die Aktion Controller vervollständigen, und fügen Sie eine Ansicht mit dem Namen Complete hinzu, die stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="b8acc-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="b8acc-169">Nun aktualisieren wir den Ansichts Code so, dass die Bestell-ID angezeigt wird, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8acc-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="b8acc-170">Aktualisieren der Fehler Ansicht</span><span class="sxs-lookup"><span data-stu-id="b8acc-170">Updating The Error view</span></span>

<span data-ttu-id="b8acc-171">Die Standardvorlage enthält eine Fehler Ansicht im Ordner "freigegebene Ansichten", sodass Sie an anderer Stelle der Site wieder verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b8acc-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="b8acc-172">Diese Fehler Ansicht enthält einen sehr einfachen Fehler und verwendet nicht das Website Layout. daher aktualisieren wir Sie.</span><span class="sxs-lookup"><span data-stu-id="b8acc-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="b8acc-173">Da es sich hierbei um eine allgemeine Fehlerseite handelt, ist der Inhalt sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="b8acc-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="b8acc-174">Wir fügen eine Meldung und einen Link ein, um zur vorherigen Seite im Verlauf zu navigieren, wenn der Benutzer die Aktion wiederholen möchte.</span><span class="sxs-lookup"><span data-stu-id="b8acc-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> <span data-ttu-id="b8acc-175">[Zurück](mvc-music-store-part-8.md)
> [Weiter](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="b8acc-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
