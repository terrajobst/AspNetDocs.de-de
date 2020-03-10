---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Teil 4: Auflisten von Produkten | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 4 enthält eine Auflistung von Produkten mit der GridView-Konsole...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457281"
---
# <a name="part-4-listing-products"></a><span data-ttu-id="05651-104">Teil 4: Auflisten von Produkten</span><span class="sxs-lookup"><span data-stu-id="05651-104">Part 4: Listing Products</span></span>

<span data-ttu-id="05651-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="05651-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="05651-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="05651-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="05651-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="05651-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="05651-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="05651-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="05651-109">Teil 4 umfasst das Auflisten von Produkten mit dem GridView-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="05651-109">Part 4 covers listing products with the GridView control.</span></span>

## <a id="_Toc260221670"></a><span data-ttu-id="05651-110">Auflisten von Produkten mit dem GridView-Steuerelement</span><span class="sxs-lookup"><span data-stu-id="05651-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="05651-111">Wir beginnen mit der Implementierung der Seite "productList. aspx", indem wir mit der rechten Maustaste auf die Projekt Mappe Klicken und "hinzufügen" und "Neues Element" auswählen.</span><span class="sxs-lookup"><span data-stu-id="05651-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="05651-112">Wählen Sie "Webformular mithilfe der Master Seite" aus, und geben Sie den Seitennamen "productList. aspx" ein.</span><span class="sxs-lookup"><span data-stu-id="05651-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="05651-113">Klicken Sie auf "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="05651-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="05651-114">Wählen Sie als nächstes den Ordner "Stile" aus, in den wir die Seite "Site. Master" eingefügt haben, und wählen Sie ihn im Fenster "Ordnerinhalte" aus.</span><span class="sxs-lookup"><span data-stu-id="05651-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="05651-115">Klicken Sie auf "OK", um die Seite zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="05651-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="05651-116">Die Datenbank wird wie unten dargestellt mit den Produktdaten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="05651-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="05651-117">Nachdem unsere Seite erstellt wurde, verwenden wir erneut eine Entitäts Datenquelle, um auf diese Produktdaten zuzugreifen. in diesem Fall müssen wir jedoch die Product-Entitäten auswählen und die Elemente beschränken, die nur für die ausgewählte Kategorie zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="05651-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="05651-118">Um dies zu erreichen, wird der EntityDataSource mitgeteilt, dass die WHERE-Klausel automatisch generiert wird, und wir geben den Where-Parameter an.</span><span class="sxs-lookup"><span data-stu-id="05651-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="05651-119">Sie erinnern sich, dass Sie beim Erstellen der Menü Elemente im Menü "Produktkategorie" den Link dynamisch erstellt haben, indem Sie der Abfrage Zeichenfolge für jeden Link die CategoryID hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="05651-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CategoryID to the QueryString for each link.</span></span> <span data-ttu-id="05651-120">Wir weisen die Entitäts Datenquelle an, den Where-Parameter von diesem QueryString-Parameter abzuleiten.</span><span class="sxs-lookup"><span data-stu-id="05651-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="05651-121">Als Nächstes konfigurieren wir das ListView-Steuerelement, um eine Liste der Produkte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="05651-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="05651-122">Zum Erstellen eines optimalen Einkaufserlebnisses komprimieren wir einige präzise Features in jedem einzelnen Produkt, das in unserer listvew angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="05651-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="05651-123">Der Produktname ist ein Link zur Detailansicht des Produkts.</span><span class="sxs-lookup"><span data-stu-id="05651-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="05651-124">Der Preis des Produkts wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="05651-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="05651-125">Ein Bild des Produkts wird angezeigt, und wir wählen das Image dynamisch aus einem Katalog Abbild Verzeichnis in unserer Anwendung aus.</span><span class="sxs-lookup"><span data-stu-id="05651-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="05651-126">Wir fügen einen Link ein, mit dem das jeweilige Produkt sofort dem Warenkorb hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="05651-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="05651-127">Hier ist das Markup für die ListView-Steuerelement Instanz.</span><span class="sxs-lookup"><span data-stu-id="05651-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="05651-128">Wir bauen dynamisch mehrere Verknüpfungen für jedes angezeigte Produkt auf.</span><span class="sxs-lookup"><span data-stu-id="05651-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="05651-129">Vor dem Testen der neuen Seite müssen wir die Verzeichnisstruktur für die Produktkatalog Images wie folgt erstellen.</span><span class="sxs-lookup"><span data-stu-id="05651-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="05651-130">Sobald auf unsere Produkt Images zugegriffen werden kann, können wir unsere Produktlisten Seite testen.</span><span class="sxs-lookup"><span data-stu-id="05651-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="05651-131">Klicken Sie auf der Startseite der Site auf eine der Listen Verknüpfungen der Kategorie.</span><span class="sxs-lookup"><span data-stu-id="05651-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="05651-132">Nun müssen wir die Seite "ProductDetails. aspx" und die Funktion "adddecart" implementieren.</span><span class="sxs-lookup"><span data-stu-id="05651-132">Now we need to implement the ProductDetails.aspx page and the AddToCart functionality.</span></span>

<span data-ttu-id="05651-133">Verwenden Sie File-&gt;New, um einen Seitennamen "ProductDetails. aspx" mithilfe der Standort Master Seite wie zuvor zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="05651-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="05651-134">Wir verwenden erneut ein EntityDataSource-Steuerelement, um auf einen bestimmten Produktdaten Satz in der Datenbank zuzugreifen, und wir verwenden ein ASP.net FormView-Steuerelement, um die Produktdaten wie folgt anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="05651-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="05651-135">Machen Sie sich keine Sorgen, wenn die Formatierung etwas lustig erscheint.</span><span class="sxs-lookup"><span data-stu-id="05651-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="05651-136">Das obige Markup lässt im Anzeige Layout für einige Features, die später implementiert werden, Platz im Anzeige Layout aus.</span><span class="sxs-lookup"><span data-stu-id="05651-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="05651-137">Der Warenkorb stellt die komplexere Logik in unserer Anwendung dar.</span><span class="sxs-lookup"><span data-stu-id="05651-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="05651-138">Verwenden Sie zunächst File-&gt;New, um eine Seite mit dem Namen myshoppingcart. aspx zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="05651-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="05651-139">Beachten Sie, dass der Name "ShoppingCart. aspx" nicht ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="05651-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="05651-140">Unsere Datenbank enthält eine Tabelle mit dem Namen "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="05651-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="05651-141">Beim Generieren einer Entity Data Model eine Klasse für jede Tabelle in der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="05651-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="05651-142">Daher hat der Entity Data Model eine Entitäts Klasse mit dem Namen "ShoppingCart" generiert.</span><span class="sxs-lookup"><span data-stu-id="05651-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="05651-143">Wir könnten das Modell so bearbeiten, dass wir diesen Namen für die Warenkorb-Implementierung verwenden oder es für unsere Anforderungen erweitern können. wir entscheiden uns stattdessen dafür, einfach einen Namen auszuwählen, der den Konflikt vermeidet.</span><span class="sxs-lookup"><span data-stu-id="05651-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply select a name that will avoid the conflict.</span></span>

<span data-ttu-id="05651-144">Beachten Sie auch, dass wir einen einfachen Einkaufswagen erstellen und die Warenkorb-Logik in die Warenkorb-Anzeige einbetten.</span><span class="sxs-lookup"><span data-stu-id="05651-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="05651-145">Wir können uns auch entscheiden, den Warenkorb in einer vollständig separaten Geschäfts Schicht zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="05651-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05651-146">[Zurück](tailspin-spyworks-part-3.md)
> [Weiter](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="05651-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
