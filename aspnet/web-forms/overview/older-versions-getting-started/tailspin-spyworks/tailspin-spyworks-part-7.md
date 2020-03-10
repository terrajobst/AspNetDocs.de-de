---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Teil 7: Hinzufügen von Features | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 7 fügt zusätzliche Funktionen hinzu, wie z. b. Konto Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521565"
---
# <a name="part-7-adding-features"></a><span data-ttu-id="db8bd-104">Teil 7: Hinzufügen von Features</span><span class="sxs-lookup"><span data-stu-id="db8bd-104">Part 7: Adding Features</span></span>

<span data-ttu-id="db8bd-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="db8bd-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="db8bd-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="db8bd-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="db8bd-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="db8bd-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="db8bd-109">Teil 7 bietet zusätzliche Features, wie z. b. Kontoüberprüfung, Produkt Reviews und "beliebte Elemente" und "auch erworbene" Benutzer Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="db8bd-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>

## <a id="_Toc260221673"></a><span data-ttu-id="db8bd-110">Hinzufügen von Features</span><span class="sxs-lookup"><span data-stu-id="db8bd-110">Adding Features</span></span>

<span data-ttu-id="db8bd-111">Obwohl Benutzer unseren Katalog durchsuchen, Elemente in Ihren Warenkorb platzieren und den Checkout Vorgang durchführen können, gibt es eine Reihe von unterstützenden Features, die wir zur Verbesserung unserer Website berücksichtigen werden.</span><span class="sxs-lookup"><span data-stu-id="db8bd-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="db8bd-112">Kontoüberprüfung (Auflisten von Aufträgen und Anzeigen von Details.)</span><span class="sxs-lookup"><span data-stu-id="db8bd-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="db8bd-113">Fügen Sie der Vorderseite kontextspezifischen Inhalt hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="db8bd-114">Fügen Sie eine Funktion hinzu, mit der Benutzer die Produkte im Katalog überprüfen können.</span><span class="sxs-lookup"><span data-stu-id="db8bd-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="db8bd-115">Erstellen Sie ein Benutzer Steuerelement, um beliebte Elemente anzuzeigen und dieses Steuerelement auf der Vorderseite zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="db8bd-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="db8bd-116">Erstellen Sie ein "auch erworbenes" Benutzer Steuerelement, und fügen Sie es der Seite "Produktdetails" hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="db8bd-117">Fügen Sie eine Kontaktseite hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="db8bd-118">Fügen Sie eine Info-Seite hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-118">Add an About Page.</span></span>
8. <span data-ttu-id="db8bd-119">Globaler Fehler</span><span class="sxs-lookup"><span data-stu-id="db8bd-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="db8bd-120">Kontoüberprüfung</span><span class="sxs-lookup"><span data-stu-id="db8bd-120">Account Review</span></span>

<span data-ttu-id="db8bd-121">Erstellen Sie im Ordner "Account" zwei ASPX-Seiten mit dem Namen "OrderList. aspx" und die andere mit dem Namen "OrderDetails. aspx".</span><span class="sxs-lookup"><span data-stu-id="db8bd-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="db8bd-122">"OrderList. aspx" nutzt die GridView-und EntityDataSource-Steuerelemente ähnlich wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="db8bd-122">OrderList.aspx will leverage the GridView and EntityDataSource controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="db8bd-123">Die EntityDataSource wählt Datensätze aus der Tabelle Orders aus, die nach dem Benutzernamen gefiltert wird (siehe Where Parameter), den wir in einer Sitzungsvariablen festlegen, wenn der Benutzer sich in befindet.</span><span class="sxs-lookup"><span data-stu-id="db8bd-123">The EntityDataSource selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="db8bd-124">Beachten Sie auch diese Parameter im HyperLinkField der GridView:</span><span class="sxs-lookup"><span data-stu-id="db8bd-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="db8bd-125">Diese geben den Link zur Order Details-Ansicht für jedes Produkt an, das das OrderID-Feld als QueryString-Parameter für die Order Details. aspx-Seite angibt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="db8bd-126">OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="db8bd-126">OrderDetails.aspx</span></span>

<span data-ttu-id="db8bd-127">Wir verwenden ein EntityDataSource-Steuerelement für den Zugriff auf die Bestellungen und eine FormView zum Anzeigen der Bestelldaten und eine weitere EntityDataSource mit einer GridView, um alle Positionen der Bestellung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="db8bd-128">In der Code Behind-Datei (OrderDetails.aspx.cs) gibt es zwei wenige Teile der Hausführung.</span><span class="sxs-lookup"><span data-stu-id="db8bd-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="db8bd-129">Zuerst müssen wir sicherstellen, dass Order Details immer eine OrderID erhält.</span><span class="sxs-lookup"><span data-stu-id="db8bd-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="db8bd-130">Wir müssen auch die Bestellsumme aus den Zeilen Elementen berechnen und anzeigen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="db8bd-131">Die Startseite</span><span class="sxs-lookup"><span data-stu-id="db8bd-131">The Home Page</span></span>

<span data-ttu-id="db8bd-132">Fügen wir der Seite Default. aspx einige statische Inhalte hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="db8bd-133">Zuerst erstelle ich einen Ordner "Content" (Inhalt) und innerhalb des Ordners "Images" (Ich füge ein Bild ein, das auf der Startseite verwendet werden soll.)</span><span class="sxs-lookup"><span data-stu-id="db8bd-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="db8bd-134">Fügen Sie im unteren Platzhalter der default. aspx-Seite das folgende Markup hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="db8bd-135">Produkt Reviews</span><span class="sxs-lookup"><span data-stu-id="db8bd-135">Product Reviews</span></span>

<span data-ttu-id="db8bd-136">Zuerst fügen wir eine Schaltfläche mit einem Link zu einem Formular hinzu, das wir verwenden können, um eine Produkt Überprüfung einzugeben.</span><span class="sxs-lookup"><span data-stu-id="db8bd-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="db8bd-137">Beachten Sie, dass die ProductID in der Abfrage Zeichenfolge übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="db8bd-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="db8bd-138">Als Nächstes fügen wir die Seite "reviewadd. aspx" hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="db8bd-139">Auf dieser Seite wird das ASP.NET AJAX Control Toolkit verwendet.</span><span class="sxs-lookup"><span data-stu-id="db8bd-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="db8bd-140">Wenn Sie dies noch nicht getan haben, können Sie es von [DevExpress](http://devexpress.com/act) herunterladen. es gibt Anleitungen zum Einrichten des Toolkits für die Verwendung mit Visual Studio [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="db8bd-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="db8bd-141">Ziehen Sie im Entwurfs Modus Steuerelemente und Validierungs Steuerelemente aus der Toolbox, und erstellen Sie ein Formular wie das folgende.</span><span class="sxs-lookup"><span data-stu-id="db8bd-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="db8bd-142">Das Markup sieht in etwa wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="db8bd-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="db8bd-143">Nachdem wir nun die Überprüfungen eingegeben haben, können diese Überprüfungen auf der Produktseite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="db8bd-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="db8bd-144">Fügen Sie dieses Markup der Seite "ProductDetails. aspx" hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="db8bd-145">Wenn Sie die Anwendung jetzt ausführen und zu einem Produkt navigieren, werden die Produktinformationen einschließlich Kunden Reviews angezeigt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="db8bd-146">Beliebte Elemente-Steuerelement (Erstellen von Benutzer Steuerelementen)</span><span class="sxs-lookup"><span data-stu-id="db8bd-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="db8bd-147">Um den Umsatz auf Ihrer Website zu steigern, fügen wir einige Features zu beliebten oder verwandten Produkten hinzu.</span><span class="sxs-lookup"><span data-stu-id="db8bd-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="db8bd-148">Die erste dieser Features ist eine Liste der beliebtesten Produkte in unserem Produktkatalog.</span><span class="sxs-lookup"><span data-stu-id="db8bd-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="db8bd-149">Wir erstellen ein "Benutzer Steuerelement", um die am besten verkauften Elemente auf der Startseite der Anwendung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="db8bd-150">Da dies ein Steuerelement ist, können wir es auf jeder Seite verwenden, indem wir einfach das Steuerelement im Designer von Visual Studio auf eine beliebige Seite ziehen und ablegen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="db8bd-151">Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf den Projektmappennamen, und erstellen Sie ein neues Verzeichnis namens "Controls".</span><span class="sxs-lookup"><span data-stu-id="db8bd-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="db8bd-152">Dies ist zwar nicht erforderlich, aber wir helfen, das Projekt zu organisieren, indem wir alle Benutzer Steuerelemente im Verzeichnis "Controls" erstellen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="db8bd-153">Klicken Sie mit der rechten Maustaste auf den Ordner Steuerelemente, und wählen Sie "Neues Element":</span><span class="sxs-lookup"><span data-stu-id="db8bd-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="db8bd-154">Geben Sie einen Namen für das Steuerelement "popultems" an.</span><span class="sxs-lookup"><span data-stu-id="db8bd-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="db8bd-155">Beachten Sie, dass es sich bei der Dateierweiterung für Benutzer Steuerelemente um. ascx not. aspx handelt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="db8bd-156">Das Benutzer Steuerelement "beliebte Elemente" wird wie folgt definiert.</span><span class="sxs-lookup"><span data-stu-id="db8bd-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="db8bd-157">Hier verwenden wir eine Methode, die wir noch nicht in dieser Anwendung verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="db8bd-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="db8bd-158">Wir verwenden das Repeater-Steuerelement und anstatt ein Datenquellen-Steuerelement zu verwenden, binden wir das Repeater-Steuerelement an die Ergebnisse einer LINQ to Entities Abfrage.</span><span class="sxs-lookup"><span data-stu-id="db8bd-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="db8bd-159">Im Code Behind unseres Steuer Elements gehen wir wie folgt vor.</span><span class="sxs-lookup"><span data-stu-id="db8bd-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="db8bd-160">Beachten Sie auch diese wichtige Zeile am oberen Rand des Markup Elements des Steuer Elements.</span><span class="sxs-lookup"><span data-stu-id="db8bd-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="db8bd-161">Da die am häufigsten verwendeten Elemente nicht auf Minutenbasis geändert werden, können wir eine aching-Direktive hinzufügen, um die Leistung unserer Anwendung zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="db8bd-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="db8bd-162">Diese Direktive bewirkt, dass der Steuerelement Code nur ausgeführt wird, wenn die zwischengespeicherte Ausgabe des Steuer Elements abläuft.</span><span class="sxs-lookup"><span data-stu-id="db8bd-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="db8bd-163">Andernfalls wird die zwischengespeicherte Version der Ausgabe des Steuer Elements verwendet.</span><span class="sxs-lookup"><span data-stu-id="db8bd-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="db8bd-164">Nun müssen wir nur noch unser neues Steuerelement auf der Seite "default. aspx" einschließen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-164">Now all we have to do is include our new control in our Default.aspx page.</span></span>

<span data-ttu-id="db8bd-165">Verwenden Sie Drag & Drop, um eine Instanz des Steuer Elements in der Spalte Öffnen des Standardformulars zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="db8bd-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="db8bd-166">Wenn wir nun die Anwendung ausführen, werden auf der Startseite die beliebtesten Elemente angezeigt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="db8bd-167">"Auch gekauft"-Steuerelement (Benutzer Steuerelemente mit Parametern)</span><span class="sxs-lookup"><span data-stu-id="db8bd-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="db8bd-168">Das zweite Benutzer Steuerelement, das wir erstellen, wird durch Hinzufügen der Kontext Genauigkeit den suggestiven Verkauf auf der nächsten Ebene durchführen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="db8bd-169">Die Logik zum Berechnen der obersten "auch erworbenen" Elemente ist nicht trivial.</span><span class="sxs-lookup"><span data-stu-id="db8bd-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="db8bd-170">Mit unserem "auch erworbenen" Steuerelement werden die Order Details-Datensätze (zuvor gekauft) für die aktuell ausgewählte ProductID ausgewählt und die OrderIDs für jede eindeutige Bestellung erfasst.</span><span class="sxs-lookup"><span data-stu-id="db8bd-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="db8bd-171">Anschließend wählen wir die Produkte von all diesen Aufträgen aus und fassen die erworbenen Mengen zusammen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="db8bd-172">Wir sortieren die Produkte nach der Summe und zeigen die ersten fünf Elemente an.</span><span class="sxs-lookup"><span data-stu-id="db8bd-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="db8bd-173">Aufgrund der Komplexität dieser Logik wird dieser Algorithmus als gespeicherte Prozedur implementiert.</span><span class="sxs-lookup"><span data-stu-id="db8bd-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="db8bd-174">Der T-SQL-Wert für die gespeicherte Prozedur lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="db8bd-175">Beachten Sie, dass diese gespeicherte Prozedur (selectpurchasedwithproducts) in der Datenbank vorhanden war, als wir Sie in unsere Anwendung eingefügt haben, und wenn wir das Entity Data Model, das wir angegeben haben, zusätzlich zu den Tabellen und Sichten, die wir benötigten, das Entity Data Model sollte diese gespeicherte Prozedur einschließen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="db8bd-176">Um auf die gespeicherte Prozedur vom Entity Data Model aus zuzugreifen, muss die Funktion importiert werden.</span><span class="sxs-lookup"><span data-stu-id="db8bd-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="db8bd-177">Doppelklicken Sie im Projektmappen-Explorer auf die Entity Data Model, um Sie im Designer zu öffnen, und öffnen Sie den Modell Browser, klicken Sie mit der rechten Maustaste in den Designer, und wählen Sie "Funktions Import hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="db8bd-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="db8bd-178">Dadurch wird dieses Dialogfeld geöffnet.</span><span class="sxs-lookup"><span data-stu-id="db8bd-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="db8bd-179">Füllen Sie die Felder wie oben angezeigt aus, wählen Sie "selectpurchasedwithproducts" aus, und verwenden Sie den Prozedur Namen als Name der importierten Funktion.</span><span class="sxs-lookup"><span data-stu-id="db8bd-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="db8bd-180">Klicken Sie auf "OK".</span><span class="sxs-lookup"><span data-stu-id="db8bd-180">Click "Ok".</span></span>

<span data-ttu-id="db8bd-181">Nachdem Sie dies getan haben, können wir einfach mit der gespeicherten Prozedur programmieren, wie es bei jedem anderen Element im Modell möglich ist.</span><span class="sxs-lookup"><span data-stu-id="db8bd-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="db8bd-182">Erstellen Sie daher im Ordner "Steuerelemente" ein neues Benutzer Steuerelement mit dem Namen "alsopurgejagt. ascx".</span><span class="sxs-lookup"><span data-stu-id="db8bd-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="db8bd-183">Das Markup für dieses Steuerelement wird dem popultems-Steuerelement vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="db8bd-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="db8bd-184">Der bedeutende Unterschied besteht darin, dass die Ausgabe nicht zwischenspeichert, da das zu rendernde Element je nach Produkt unterschiedlich ist.</span><span class="sxs-lookup"><span data-stu-id="db8bd-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="db8bd-185">Die ProductID ist eine "Eigenschaft" für das Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="db8bd-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="db8bd-186">Im PreRender-Ereignishandler des Steuer Elements haben wir drei Dinge durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="db8bd-187">Stellen Sie sicher, dass ProductID festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="db8bd-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="db8bd-188">Überprüfen Sie, ob Produkte vorhanden sind, die mit dem aktuellen gekauft wurden.</span><span class="sxs-lookup"><span data-stu-id="db8bd-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="db8bd-189">Ausgabe einiger Elemente, wie in #2 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="db8bd-190">Beachten Sie, wie einfach es ist, die gespeicherte Prozedur über das Modell aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="db8bd-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="db8bd-191">Nachdem Sie festgestellt haben, dass auch "auch gekauft" vorhanden ist, können wir einfach das Repeater an die von der Abfrage zurückgegebenen Ergebnisse binden.</span><span class="sxs-lookup"><span data-stu-id="db8bd-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="db8bd-192">Wenn keine "auch erworbenen" Elemente vorhanden waren, zeigen wir einfach andere beliebte Elemente aus unserem Katalog an.</span><span class="sxs-lookup"><span data-stu-id="db8bd-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="db8bd-193">Um die "auch erworbenen" Elemente anzuzeigen, öffnen Sie die Seite ProductDetails. aspx, und ziehen Sie das Steuerelement alsopurgejagt aus dem Projektmappen-Explorer, sodass es an dieser Position im Markup angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="db8bd-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="db8bd-194">Dadurch wird am oberen Rand der Seite "ProductDetails" ein Verweis auf das Steuerelement erstellt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="db8bd-195">Da das Benutzer Steuerelement "alsopurgejagt" eine ProductID-Nummer erfordert, wird die ProductID-Eigenschaft des Steuer Elements mit einer eval-Anweisung für das aktuelle Datenmodell Element der Seite festgelegt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="db8bd-196">Wenn wir jetzt erstellen und ausführen und zu einem Produkt navigieren, werden die Elemente "auch gekauft" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="db8bd-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="db8bd-197">[Zurück](tailspin-spyworks-part-6.md)
> [Weiter](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="db8bd-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
