---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Anzeigen von Datenelementen und Details | Microsoft-Dokumentation
author: Erikre
description: In dieser tutorialreihe werden die Grundlagen zum Entwickeln einer ASP.net Web Forms Anwendung mit ASP.NET 4,7 und Microsoft Visual Studio 2017 erläutert.
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520809"
---
# <a name="display-data-items-and-details"></a><span data-ttu-id="0683b-103">Anzeigen von Datenelementen und-Details</span><span class="sxs-lookup"><span data-stu-id="0683b-103">Display data items and details</span></span>

<span data-ttu-id="0683b-104">von [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="0683b-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="0683b-105">Diese tutorialreihe vermittelt Ihnen die Grundlagen des Aufbaus einer ASP.net Web Forms-Anwendung mit ASP.NET 4,7 und Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0683b-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="0683b-106">In diesem Tutorial erfahren Sie, wie Datenelemente und Datenelement Details mit ASP.net-Web Forms und Entity Framework Code First angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0683b-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="0683b-107">Dieses Tutorial baut auf dem vorherigen Tutorial "Benutzeroberfläche und Navigation" im Rahmen der Wingtip-Lernprogramm Reihe "Tutorial Store" auf.</span><span class="sxs-lookup"><span data-stu-id="0683b-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="0683b-108">Nach Abschluss dieses Tutorials sehen Sie Produkte auf der Seite " *productList. aspx* " und die Details eines Produkts auf der Seite " *ProductDetails. aspx* ".</span><span class="sxs-lookup"><span data-stu-id="0683b-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="0683b-109">Sie lernen, die folgende Aufgaben auszuführen:</span><span class="sxs-lookup"><span data-stu-id="0683b-109">You'll learn how to:</span></span>

- <span data-ttu-id="0683b-110">Hinzufügen eines Daten Steuer Elements zum Anzeigen von Produkten aus der Datenbank</span><span class="sxs-lookup"><span data-stu-id="0683b-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="0683b-111">Verbinden eines Daten Steuer Elements mit den ausgewählten Daten</span><span class="sxs-lookup"><span data-stu-id="0683b-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="0683b-112">Hinzufügen eines Daten Steuer Elements zum Anzeigen von Produktdetails aus der Datenbank</span><span class="sxs-lookup"><span data-stu-id="0683b-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="0683b-113">Rufen Sie einen Wert aus der Abfrage Zeichenfolge ab, und verwenden Sie diesen Wert, um die aus der Datenbank abgerufenen Daten einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="0683b-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="0683b-114">In diesem Tutorial eingeführte Features:</span><span class="sxs-lookup"><span data-stu-id="0683b-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="0683b-115">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="0683b-115">Model binding</span></span>
- <span data-ttu-id="0683b-116">Wert Anbieter</span><span class="sxs-lookup"><span data-stu-id="0683b-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="0683b-117">Hinzufügen eines Daten Steuer Elements</span><span class="sxs-lookup"><span data-stu-id="0683b-117">Add a data control</span></span>

<span data-ttu-id="0683b-118">Sie können verschiedene Optionen verwenden, um Daten an ein Server Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="0683b-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="0683b-119">Am häufigsten gehören:</span><span class="sxs-lookup"><span data-stu-id="0683b-119">The most common include:</span></span>

* <span data-ttu-id="0683b-120">Hinzufügen eines Datenquellen-Steuer Elements</span><span class="sxs-lookup"><span data-stu-id="0683b-120">Adding a data source control</span></span>
* <span data-ttu-id="0683b-121">Manuelles Hinzufügen von Code</span><span class="sxs-lookup"><span data-stu-id="0683b-121">Adding code by hand</span></span>
* <span data-ttu-id="0683b-122">Verwenden der Modell Bindung</span><span class="sxs-lookup"><span data-stu-id="0683b-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="0683b-123">Verwenden eines Datenquellen-Steuer Elements zum Binden von Daten</span><span class="sxs-lookup"><span data-stu-id="0683b-123">Use a data source control to bind data</span></span>

<span data-ttu-id="0683b-124">Durch das Hinzufügen eines Datenquellen-Steuer Elements können Sie das Datenquellen-Steuerelement mit dem Steuerelement verknüpfen, das die Daten anzeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="0683b-125">Bei diesem Ansatz können Sie serverseitige Steuerelemente deklarativ und nicht Programm gesteuert mit Datenquellen verbinden.</span><span class="sxs-lookup"><span data-stu-id="0683b-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="0683b-126">Code von Hand zum Binden von Daten</span><span class="sxs-lookup"><span data-stu-id="0683b-126">Code by hand to bind data</span></span>

<span data-ttu-id="0683b-127">Das Codieren per Hand umfasst Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0683b-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="0683b-128">Lesen eines Werts</span><span class="sxs-lookup"><span data-stu-id="0683b-128">Reading a value</span></span>
2. <span data-ttu-id="0683b-129">Überprüfen, ob es NULL ist</span><span class="sxs-lookup"><span data-stu-id="0683b-129">Checking if it's null</span></span>
3. <span data-ttu-id="0683b-130">Umrechnen in einen geeigneten Typ</span><span class="sxs-lookup"><span data-stu-id="0683b-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="0683b-131">Überprüfen der Konvertierung erfolgreich</span><span class="sxs-lookup"><span data-stu-id="0683b-131">Checking conversion success</span></span>
5. <span data-ttu-id="0683b-132">Verwenden des Werts in der Abfrage</span><span class="sxs-lookup"><span data-stu-id="0683b-132">Using the value in the query</span></span> 

<span data-ttu-id="0683b-133">Mit diesem Ansatz haben Sie die volle Kontrolle über Ihre Datenzugriffs Logik.</span><span class="sxs-lookup"><span data-stu-id="0683b-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="0683b-134">Verwenden der Modell Bindung zum Binden von Daten</span><span class="sxs-lookup"><span data-stu-id="0683b-134">Use model binding to bind data</span></span>

<span data-ttu-id="0683b-135">Mit der Modell Bindung können Sie Ergebnisse mit viel geringerem Code binden, und Sie können die Funktionalität in der gesamten Anwendung wieder verwenden.</span><span class="sxs-lookup"><span data-stu-id="0683b-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="0683b-136">Dadurch wird die Arbeit mit Code orientierten Datenzugriffs Logik vereinfacht, während gleichzeitig ein umfassendes Daten Bindungs Framework bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="0683b-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="0683b-137">Produkte anzeigen</span><span class="sxs-lookup"><span data-stu-id="0683b-137">Display products</span></span>

<span data-ttu-id="0683b-138">In diesem Tutorial verwenden Sie die Modell Bindung, um Daten zu binden.</span><span class="sxs-lookup"><span data-stu-id="0683b-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="0683b-139">Wenn Sie ein Daten Steuerelement so konfigurieren möchten, dass die Modell Bindung verwendet wird, um Daten auszuwählen, legen Sie die `SelectMethod`-Eigenschaft des Steuer Elements auf einen Methodennamen im Code der Seite fest.</span><span class="sxs-lookup"><span data-stu-id="0683b-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="0683b-140">Das Daten Steuerelement ruft die Methode zum entsprechenden Zeitpunkt im Lebenszyklus der Seite auf und bindet die zurückgegebenen Daten automatisch.</span><span class="sxs-lookup"><span data-stu-id="0683b-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="0683b-141">Es ist nicht erforderlich, den `DataBind`-Methode explizit aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="0683b-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="0683b-142">ÖffnenSie in Projektmappen-Explorer *productList. aspx*.</span><span class="sxs-lookup"><span data-stu-id="0683b-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="0683b-143">Ersetzen Sie das vorhandene Markup durch dieses Markup:</span><span class="sxs-lookup"><span data-stu-id="0683b-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="0683b-144">Dieser Code verwendet ein **ListView** -Steuerelement mit dem Namen `productList`, um Produkte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0683b-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="0683b-145">Mit Vorlagen und Stilen definieren Sie, wie Daten im **ListView** -Steuerelement angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0683b-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="0683b-146">Es ist nützlich für Daten in jeder sich wiederholenden Struktur.</span><span class="sxs-lookup"><span data-stu-id="0683b-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="0683b-147">Obwohl in diesem **ListView** -Beispiel lediglich Datenbankdaten angezeigt werden, können Sie ohne Code auch Benutzer zum Bearbeiten, einfügen und Löschen von Daten sowie zum Sortieren und Sortieren von Daten aktivieren.</span><span class="sxs-lookup"><span data-stu-id="0683b-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="0683b-148">Wenn Sie die `ItemType`-Eigenschaft im **ListView** -Steuerelement festlegen, ist der Daten Bindungs Ausdruck `Item` verfügbar, und das Steuerelement wird stark typisiert.</span><span class="sxs-lookup"><span data-stu-id="0683b-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="0683b-149">Wie bereits im vorherigen Tutorial erwähnt, können Sie Element Objektdetails mit IntelliSense auswählen, z. b. die `ProductName`angeben:</span><span class="sxs-lookup"><span data-stu-id="0683b-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Anzeigen von Datenelementen und Details-IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="0683b-151">Außerdem verwenden Sie die Modell Bindung, um einen `SelectMethod` Wert anzugeben.</span><span class="sxs-lookup"><span data-stu-id="0683b-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="0683b-152">Dieser Wert (`GetProducts`) entspricht der Methode, die Sie dem Code Behind hinzufügen, um im nächsten Schritt Produkte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0683b-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="0683b-153">Hinzufügen von Code zum Anzeigen von Produkten</span><span class="sxs-lookup"><span data-stu-id="0683b-153">Add code to display products</span></span>

<span data-ttu-id="0683b-154">In diesem Schritt fügen Sie Code hinzu, um das **ListView** -Steuerelement mit Produktdaten aus der Datenbank aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="0683b-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="0683b-155">Der Code unterstützt das darstellen aller Produkte und Produkte der einzelnen Kategorien.</span><span class="sxs-lookup"><span data-stu-id="0683b-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="0683b-156">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *productList. aspx* , und wählen Sie dann **Code anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="0683b-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="0683b-157">Ersetzen Sie den vorhandenen Code in der *productList.aspx.cs* -Datei durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0683b-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="0683b-158">Dieser Code zeigt die `GetProducts` Methode, auf die die `ItemType`-Eigenschaft des **ListView** -Steuer Elements auf der Seite " *productList. aspx* " verweist.</span><span class="sxs-lookup"><span data-stu-id="0683b-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="0683b-159">Um die Ergebnisse auf eine bestimmte Daten Bank Kategorie zu beschränken, legt der Code den `categoryId` Wert aus dem Wert der Abfrage Zeichenfolge fest, der an die Seite *productList. aspx* übermittelt wird, wenn auf die Seite *productList. aspx* navigiert wird.</span><span class="sxs-lookup"><span data-stu-id="0683b-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="0683b-160">Die `QueryStringAttribute`-Klasse im `System.Web.ModelBinding`-Namespace wird verwendet, um den Wert der Abfrage Zeichenfolgen-Variablen `id`abzurufen.</span><span class="sxs-lookup"><span data-stu-id="0683b-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="0683b-161">Dies weist die Modell Bindung an, einen Wert aus der Abfrage Zeichenfolge zur Laufzeit an den `categoryId`-Parameter zu binden.</span><span class="sxs-lookup"><span data-stu-id="0683b-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="0683b-162">Wenn eine gültige Kategorie als Abfrage Zeichenfolge an die Seite übermittelt wird, sind die Ergebnisse der Abfrage auf die Produkte in der Datenbank beschränkt, die dem `categoryId`-Wert entsprechen.</span><span class="sxs-lookup"><span data-stu-id="0683b-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="0683b-163">Wenn z. b. die Seiten-URL der *productList. aspx* -Seite lautet:</span><span class="sxs-lookup"><span data-stu-id="0683b-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="0683b-164">Auf der Seite werden nur die Produkte angezeigt, bei denen der `categoryId` `1`ist.</span><span class="sxs-lookup"><span data-stu-id="0683b-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="0683b-165">Wenn die Seite *productList. aspx* aufgerufen wird, werden alle Produkte angezeigt, wenn keine Abfrage Zeichenfolge enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="0683b-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="0683b-166">Die Quellen der Werte für diese Methoden werden als *Wert Anbieter* (z. b. *QueryString*) bezeichnet, und die Parameter Attribute, die angeben, welcher Wert Anbieter verwendet werden soll, werden als *Wert Anbieter Attribute* bezeichnet (z. b. `id`).</span><span class="sxs-lookup"><span data-stu-id="0683b-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="0683b-167">ASP.net schließt Wert Anbieter und zugehörige Attribute für alle typischen Quellen der Benutzereingabe in einer Web Forms Anwendung ein, z. b. Abfrage Zeichenfolge, Cookies, Formular Werte, Steuerelemente, Ansichts Zustand, Sitzungszustand und Profil Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="0683b-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="0683b-168">Sie können auch benutzerdefinierte Wert Anbieter schreiben.</span><span class="sxs-lookup"><span data-stu-id="0683b-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="0683b-169">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0683b-169">Run the application</span></span>

<span data-ttu-id="0683b-170">Führen Sie die Anwendung jetzt aus, um alle Produkte oder die Produkte einer Kategorie anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0683b-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="0683b-171">Drücken Sie in Visual Studio die **Taste F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0683b-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="0683b-172">Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.</span><span class="sxs-lookup"><span data-stu-id="0683b-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="0683b-173">Wählen Sie im Navigationsmenü der Produktkategorie die Option **Autos** aus.</span><span class="sxs-lookup"><span data-stu-id="0683b-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="0683b-174">Die Seite " *productList. aspx* " zeigt nur Produkte der Kategorie " **Auto** " an.</span><span class="sxs-lookup"><span data-stu-id="0683b-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="0683b-175">Später in diesem Tutorial zeigen Sie Produktdetails an.</span><span class="sxs-lookup"><span data-stu-id="0683b-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Anzeigen von Datenelementen und Details-Autos](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="0683b-177">Wählen Sie im oberen Bereich des Navigationsmenüs die Option **Produkte** aus.</span><span class="sxs-lookup"><span data-stu-id="0683b-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="0683b-178">Auch hier wird die Seite " *productList. aspx* " angezeigt. dieses Mal wird jedoch die gesamte Liste der Produkte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Anzeigen von Datenelementen und Details-Produkte](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="0683b-180">Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.</span><span class="sxs-lookup"><span data-stu-id="0683b-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="0683b-181">Hinzufügen eines Daten Steuer Elements zum Anzeigen von Produktdetails</span><span class="sxs-lookup"><span data-stu-id="0683b-181">Add a data control to display product details</span></span>

<span data-ttu-id="0683b-182">Anschließend ändern Sie das Markup auf der Seite " *ProductDetails. aspx* ", die Sie im vorherigen Tutorial hinzugefügt haben, um bestimmte Produktinformationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0683b-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="0683b-183">Öffnen Sie in **Projektmappen-Explorer**die *Datei ProductDetails. aspx*.</span><span class="sxs-lookup"><span data-stu-id="0683b-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="0683b-184">Ersetzen Sie das vorhandene Markup durch dieses Markup:</span><span class="sxs-lookup"><span data-stu-id="0683b-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="0683b-185">Dieser Code verwendet ein **FormView** -Steuerelement, um bestimmte Produktdetails anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0683b-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="0683b-186">Dieses Markup verwendet Methoden wie die Methoden, die zum Anzeigen von Daten auf der Seite " *productList. aspx* " verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0683b-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="0683b-187">Das **FormView** -Steuerelement wird verwendet, um einen einzelnen Datensatz gleichzeitig aus einer Datenquelle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0683b-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="0683b-188">Wenn Sie das **FormView** -Steuerelement verwenden, erstellen Sie Vorlagen, um Daten gebundene Werte anzuzeigen und zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0683b-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="0683b-189">Diese Vorlagen enthalten Steuerelemente, Bindungs Ausdrücke und Formatierungen, die das Aussehen und die Funktionalität des Formulars definieren.</span><span class="sxs-lookup"><span data-stu-id="0683b-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="0683b-190">Das Herstellen einer Verbindung zwischen dem vorherigen Markup und der Datenbank erfordert zusätzlichen Code.</span><span class="sxs-lookup"><span data-stu-id="0683b-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="0683b-191">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf *ProductDetails. aspx* , und klicken Sie dann auf **Code anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="0683b-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="0683b-192">Die Datei *ProductDetails.aspx.cs* wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="0683b-193">Ersetzen Sie den vorhandenen Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0683b-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="0683b-194">Dieser Code sucht nach einem Abfrage Zeichen folgen Wert vom Typ "`productID`".</span><span class="sxs-lookup"><span data-stu-id="0683b-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="0683b-195">Wenn ein gültiger Wert für die Abfrage Zeichenfolge gefunden wird, wird das entsprechende Produkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="0683b-196">Wenn die Abfrage Zeichenfolge nicht gefunden wird oder der Wert nicht gültig ist, wird kein Produkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="0683b-197">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0683b-197">Run the application</span></span>

<span data-ttu-id="0683b-198">Nun können Sie die Anwendung ausführen, um ein einzelnes Produkt anzuzeigen, das auf der Grundlage der Produkt-ID angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0683b-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="0683b-199">Drücken Sie in Visual Studio die **Taste F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0683b-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="0683b-200">Der Browser wird geöffnet und zeigt die Seite *default. aspx* an.</span><span class="sxs-lookup"><span data-stu-id="0683b-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="0683b-201">Wählen Sie im Navigationsmenü der Kategorie die Option **Boote** aus.</span><span class="sxs-lookup"><span data-stu-id="0683b-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="0683b-202">Die Seite " *productList. aspx* " wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="0683b-203">Wählen Sie **Papierboote** aus der Liste Produkt aus.</span><span class="sxs-lookup"><span data-stu-id="0683b-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="0683b-204">Die Seite *ProductDetails. aspx* wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0683b-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Anzeigen von Datenelementen und Details-Produkte](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="0683b-206">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="0683b-206">Close the browser.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0683b-207">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0683b-207">Additional resources</span></span>

[<span data-ttu-id="0683b-208">Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms</span><span class="sxs-lookup"><span data-stu-id="0683b-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="0683b-209">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="0683b-209">Next steps</span></span>

<span data-ttu-id="0683b-210">In diesem Tutorial haben Sie Markup und Code zum Anzeigen von Produkten und Produktdetails hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0683b-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="0683b-211">Sie haben sich über stark typisierte Daten Steuerelemente, Modell Bindungen und Wert Anbieter kennengelernt.</span><span class="sxs-lookup"><span data-stu-id="0683b-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="0683b-212">Im nächsten Tutorial fügen Sie der Wingtip Toys-Beispielanwendung einen Warenkorb hinzu.</span><span class="sxs-lookup"><span data-stu-id="0683b-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="0683b-213">[Zurück](ui_and_navigation.md)
> [Weiter](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="0683b-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
