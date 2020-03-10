---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Teil 2: Datenzugriffs Ebene | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. In Teil 2 wird das Hinzufügen der Datenzugriffs Ebene behandelt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462651"
---
# <a name="part-2-data-access-layer"></a><span data-ttu-id="9ced2-104">Teil 2: Datenzugriffs Ebene</span><span class="sxs-lookup"><span data-stu-id="9ced2-104">Part 2: Data Access Layer</span></span>

<span data-ttu-id="9ced2-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9ced2-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9ced2-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9ced2-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9ced2-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="9ced2-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9ced2-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9ced2-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9ced2-109">In Teil 2 wird das Hinzufügen der Datenzugriffs Ebene behandelt.</span><span class="sxs-lookup"><span data-stu-id="9ced2-109">Part 2 covers adding the data access layer.</span></span>

## <a id="_Toc260221668"></a><span data-ttu-id="9ced2-110">Hinzufügen der Datenzugriffs Ebene</span><span class="sxs-lookup"><span data-stu-id="9ced2-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="9ced2-111">Unsere eCommerce-Anwendung hängt von zwei Datenbanken ab.</span><span class="sxs-lookup"><span data-stu-id="9ced2-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="9ced2-112">Für Kundeninformationen verwenden wir die ASP.NET-Standard Mitgliedschafts Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9ced2-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="9ced2-113">Für den Warenkorb und den Produktkatalog implementieren wir wie folgt eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9ced2-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="9ced2-114">Wenn Sie die Datenbank (Commerce. mdf) im App-\_Datenordner der Anwendung erstellt haben, können wir mit dem Erstellen unserer Datenzugriffs Ebene mit dem .NET-Entity Framework fortfahren.</span><span class="sxs-lookup"><span data-stu-id="9ced2-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="9ced2-115">Erstellen Sie einen Ordner mit dem Namen "Data\_Access", und klicken Sie mit der rechten Maustaste auf diesen Ordner, und wählen Sie "Neues Element hinzufügen" aus.</span><span class="sxs-lookup"><span data-stu-id="9ced2-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="9ced2-116">Geben Sie im Element "installierte Vorlagen" den Eintrag "ADO.NET Entity Data Model" ein, und klicken Sie dann auf "EDM\_Commerce. edmx", und klicken Sie auf die Schaltfläche "hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="9ced2-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="9ced2-117">Wählen Sie "aus Datenbank generieren" aus.</span><span class="sxs-lookup"><span data-stu-id="9ced2-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="9ced2-118">Speichern und erstellen Sie.</span><span class="sxs-lookup"><span data-stu-id="9ced2-118">Save and build.</span></span>

<span data-ttu-id="9ced2-119">Nun können wir unsere erste Funktion hinzufügen – ein Menü für die Produktkategorie.</span><span class="sxs-lookup"><span data-stu-id="9ced2-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9ced2-120">[Zurück](tailspin-spyworks-part-1.md)
> [Weiter](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="9ced2-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
