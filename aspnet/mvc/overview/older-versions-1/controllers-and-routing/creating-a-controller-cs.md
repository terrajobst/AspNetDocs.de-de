---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Erstellen eines Controllers (C#) | Microsoft-Dokumentation
author: StephenWalther
description: In diesem Tutorial demonstriert Stephen Walther, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e3d0bae7f07410637c2b06c500d94a02c821f5c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437649"
---
# <a name="creating-a-controller-c"></a><span data-ttu-id="a0845-103">Erstellen eines Controllers (C#)</span><span class="sxs-lookup"><span data-stu-id="a0845-103">Creating a Controller (C#)</span></span>

<span data-ttu-id="a0845-104">von [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a0845-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a0845-105">In diesem Tutorial demonstriert Stephen Walther, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="a0845-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>

<span data-ttu-id="a0845-106">In diesem Tutorial wird erläutert, wie Sie neue ASP.NET MVC-Controller erstellen.</span><span class="sxs-lookup"><span data-stu-id="a0845-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="a0845-107">Sie erfahren, wie Sie mithilfe der Menüoption Controller Hinzufügen von Visual Studio Controller und durch manuelles Erstellen einer Klassendatei Controller erstellen.</span><span class="sxs-lookup"><span data-stu-id="a0845-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="a0845-108">Verwenden der Menü Option "Controller hinzufügen"</span><span class="sxs-lookup"><span data-stu-id="a0845-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="a0845-109">Am einfachsten können Sie einen neuen Controller erstellen, indem Sie im Fenster Visual Studio Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controllers klicken und die Menüoption **hinzufügen, Controller** auswählen (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="a0845-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="a0845-110">Wenn Sie diese Menüoption auswählen, wird das Dialogfeld **Controller hinzufügen** geöffnet (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="a0845-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>

<span data-ttu-id="a0845-111">[![des Dialog Felds "Neues Projekt"](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a0845-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="a0845-112">**Abbildung 01**: Hinzufügen eines neuen Controllers ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a0845-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>

<span data-ttu-id="a0845-113">[![des Dialog Felds "Neues Projekt"](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a0845-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="a0845-114">**Abbildung 02**: Dialogfeld "Controller hinzufügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a0845-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>

<span data-ttu-id="a0845-115">Beachten Sie, dass der erste Teil des Controller namens im Dialogfeld " **Controller hinzufügen** " hervorgehoben ist.</span><span class="sxs-lookup"><span data-stu-id="a0845-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="a0845-116">Jeder Controller Name muss mit dem Suffix *Controller*enden.</span><span class="sxs-lookup"><span data-stu-id="a0845-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="a0845-117">Beispielsweise können Sie einen Controller namens *ProductController* erstellen, aber keinen Controller mit dem Namen *Product*.</span><span class="sxs-lookup"><span data-stu-id="a0845-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>

<span data-ttu-id="a0845-118">Wenn Sie einen Controller erstellen, dem das *Controller* Suffix fehlt, können Sie den Controller nicht aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a0845-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="a0845-119">Dies ist nicht der Fall: Ich habe nach diesem Fehler unzählige Stunden meines Lebenszyklus verschwendet.</span><span class="sxs-lookup"><span data-stu-id="a0845-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>

<span data-ttu-id="a0845-120">**Codebeispiel 1-controllers\productcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="a0845-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="a0845-121">Sie sollten immer Controller im Ordner Controller erstellen.</span><span class="sxs-lookup"><span data-stu-id="a0845-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="a0845-122">Andernfalls verletzen Sie die Konventionen von ASP.NET MVC, und andere Entwickler haben eine schwierigere Zeit, Ihre Anwendung zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="a0845-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="a0845-123">Gerüst Aktionsmethoden</span><span class="sxs-lookup"><span data-stu-id="a0845-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="a0845-124">Wenn Sie einen Controller erstellen, haben Sie die Möglichkeit, automatisch Create-, Update-und Details-Aktionsmethoden zu generieren (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="a0845-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="a0845-125">Wenn Sie diese Option auswählen, wird die Controller Klasse in der Liste 2 generiert.</span><span class="sxs-lookup"><span data-stu-id="a0845-125">If you select this option then the controller class in Listing 2 is generated.</span></span>

<span data-ttu-id="a0845-126">[Automatisches Erstellen von Aktionsmethoden ![](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a0845-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="a0845-127">**Abbildung 03**: Automatisches Erstellen von Aktionsmethoden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a0845-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>

<span data-ttu-id="a0845-128">**Codebeispiel 2: controllers\customercontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="a0845-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="a0845-129">Diese generierten Methoden sind Stub-Methoden.</span><span class="sxs-lookup"><span data-stu-id="a0845-129">These generated methods are stub methods.</span></span> <span data-ttu-id="a0845-130">Sie müssen die eigentliche Logik zum Erstellen, aktualisieren und Anzeigen von Details für einen Kunden hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a0845-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="a0845-131">Die Stub-Methoden bieten Ihnen aber einen guten Ausgangspunkt.</span><span class="sxs-lookup"><span data-stu-id="a0845-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="a0845-132">Erstellen einer Controller Klasse</span><span class="sxs-lookup"><span data-stu-id="a0845-132">Creating a Controller Class</span></span>

<span data-ttu-id="a0845-133">Der ASP.NET MVC-Controller ist nur eine Klasse.</span><span class="sxs-lookup"><span data-stu-id="a0845-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="a0845-134">Wenn Sie möchten, können Sie das bequeme Visual Studio-Controller Gerüst ignorieren und eine Controller Klasse per Hand erstellen.</span><span class="sxs-lookup"><span data-stu-id="a0845-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="a0845-135">Folgen Sie diesen Schritten:</span><span class="sxs-lookup"><span data-stu-id="a0845-135">Follow these steps:</span></span>

1. <span data-ttu-id="a0845-136">Klicken Sie mit der rechten Maustaste auf den Ordner Controllers, und wählen Sie die Menüoption **hinzufügen, neues Element** und dann die **Klassen** Vorlage aus (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="a0845-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="a0845-137">Nennen Sie die neue Klasse PersonController.cs und klicken Sie auf die Schaltfläche **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="a0845-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="a0845-138">Ändern Sie die resultierende Klassendatei so, dass die Klasse von der System. Web. MVC. Controller-Basisklasse erbt (siehe Codebeispiel 3).</span><span class="sxs-lookup"><span data-stu-id="a0845-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>

<span data-ttu-id="a0845-139">[![Erstellen einer neuen Klasse](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a0845-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="a0845-140">**Abbildung 04**: Erstellen einer neuen Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a0845-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>

<span data-ttu-id="a0845-141">**Codebeispiel 3-controllers\personcontroller.cs**</span><span class="sxs-lookup"><span data-stu-id="a0845-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="a0845-142">Der Controller in der Liste 3 macht eine Aktion mit dem Namen Index () verfügbar, die die Zeichenfolge "Hallo Welt!" zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="a0845-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="a0845-143">Sie können diese Controller Aktion aufrufen, indem Sie die Anwendung ausführen und eine URL wie die folgende anfordern:</span><span class="sxs-lookup"><span data-stu-id="a0845-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="a0845-144">Der ASP.NET Development Server verwendet eine zufällige Portnummer (z. b. 40071).</span><span class="sxs-lookup"><span data-stu-id="a0845-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="a0845-145">Wenn Sie eine URL eingeben, um einen Controller aufzurufen, müssen Sie die richtige Portnummer angeben.</span><span class="sxs-lookup"><span data-stu-id="a0845-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="a0845-146">Sie können die Portnummer ermitteln, indem Sie mit der Maus auf das Symbol für das ASP.NET Development Server im Infobereich von Windows (unten rechts auf dem Bildschirm) zeigen.</span><span class="sxs-lookup"><span data-stu-id="a0845-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="a0845-147">[Zurück](adding-dynamic-content-to-a-cached-page-cs.md)
> [Weiter](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a0845-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
