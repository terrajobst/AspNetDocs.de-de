---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Teil 3: Layout und Kategoriemenü | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 3 umfasst das Hinzufügen von Layout und einem Kategoriemenü.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519099"
---
# <a name="part-3-layout-and-category-menu"></a><span data-ttu-id="4f4d5-104">Teil 3: Layout und Kategoriemenü</span><span class="sxs-lookup"><span data-stu-id="4f4d5-104">Part 3: Layout and Category Menu</span></span>

<span data-ttu-id="4f4d5-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4f4d5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4f4d5-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4f4d5-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4f4d5-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4f4d5-109">Teil 3 umfasst das Hinzufügen von Layout und einem Kategoriemenü.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-109">Part 3 covers adding layout and a category menu.</span></span>

## <a id="_Toc260221669"></a><span data-ttu-id="4f4d5-110">Hinzufügen von Layout und einem Kategoriemenü</span><span class="sxs-lookup"><span data-stu-id="4f4d5-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="4f4d5-111">Auf unserer Website-Master Seite fügen wir eine div für die linke Spalte hinzu, die das Menü "Produktkategorie" enthält.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="4f4d5-112">Beachten Sie, dass die gewünschte Ausrichtung und andere Formatierung von der CSS-Klasse bereitgestellt werden, die wir der Datei "Style. CSS" hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="4f4d5-113">Das Menü Product Category wird zur Laufzeit dynamisch erstellt, indem die Commerce-Datenbank nach vorhandenen Produktkategorien abgefragt und die Menü Elemente und die entsprechenden Verknüpfungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="4f4d5-114">Um dies zu erreichen, werden zwei ASP verwendet. Leistungsstarke Daten Steuerelemente von net.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="4f4d5-115">Das Steuerelement "Entitäts Datenquelle" und das ListView-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="4f4d5-116">Wechseln wir zu "Entwurfs Ansicht", und verwenden Sie die Hilfsprogramme, um die Steuerelemente zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="4f4d5-117">Legen Sie die Eigenschaft für die EntityDataSource-ID auf EDS\_Kategorie\_Menü fest, und klicken Sie auf "Datenquelle konfigurieren".</span><span class="sxs-lookup"><span data-stu-id="4f4d5-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="4f4d5-118">Wählen Sie die commerceentities-Verbindung aus, die für uns erstellt wurde, als das Entity Data Source-Modell für unsere Commerce-Datenbank erstellt wurde, und klicken Sie auf "weiter".</span><span class="sxs-lookup"><span data-stu-id="4f4d5-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="4f4d5-119">Wählen Sie den Entitätenmengennamen "categories" aus, und belassen Sie den Rest der Optionen als Standardwert.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="4f4d5-120">Klicken Sie auf "Fertigstellen".</span><span class="sxs-lookup"><span data-stu-id="4f4d5-120">Click "Finish".</span></span>

<span data-ttu-id="4f4d5-121">Nun legen wir die ID-Eigenschaft der ListView-Steuerelement Instanz fest, die wir auf der Seite auf "ListView"\_"productmenu" platziert haben</span><span class="sxs-lookup"><span data-stu-id="4f4d5-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="4f4d5-122">Obwohl wir Steuerelement Optionen verwenden könnten, um die Anzeige und Formatierung von Datenelementen zu formatieren, erfordert unsere Menüerstellung nur einfaches Markup, damit wir den Code in der Quell Ansicht eingeben.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="4f4d5-123">Beachten Sie die "eval"-Anweisung: &lt;% # eval ("CategoryName")%&gt;</span><span class="sxs-lookup"><span data-stu-id="4f4d5-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="4f4d5-124">Die ASP.NET-Syntax &lt;% #%&gt; ist eine Kurzschluss Konvention, die die Laufzeit anweist, beliebige Elemente auszuführen und die Ergebnisse "in Zeile" auszugeben.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="4f4d5-125">Die Anweisung eval ("CategoryName") weist an, dass Sie für den aktuellen Eintrag in der gebundenen Auflistung von Datenelementen den Wert der Entitäts Modell-Elementnamen "CategoryName" abrufen.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CategoryName".</span></span> <span data-ttu-id="4f4d5-126">Dies ist eine präzise Syntax für ein sehr leistungsfähiges Feature.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="4f4d5-127">Die Anwendung kann jetzt ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="4f4d5-128">Beachten Sie, dass das Menü "Produktkategorie" nun angezeigt wird. wenn wir mit dem Mauszeiger auf einen der Kategorie-Menü Elemente zeigen, sehen wir, dass der Link "Menü Element" auf eine Seite verweist, die Sie noch implementieren müssen, und "productList. aspx"  Kategorie-ID.</span><span class="sxs-lookup"><span data-stu-id="4f4d5-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4f4d5-129">[Zurück](tailspin-spyworks-part-2.md)
> [Weiter](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="4f4d5-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
