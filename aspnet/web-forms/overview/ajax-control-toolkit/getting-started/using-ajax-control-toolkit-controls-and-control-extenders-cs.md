---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Mithilfe von AJAX Control Toolkit-Steuerelementen und Steuerelement-Extendern (c#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie ASP.NET-Seiten AJAX Control Toolkit-Steuerelemente und Extender hinzugefügt.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 82fae91e40ec2f1508fe5c82992eeef4abc4e19a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59419222"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="9d836-103">Verwenden von AJAX Control Toolkit-Steuerelementen und -Steuerelement-Extendern (C#)</span><span class="sxs-lookup"><span data-stu-id="9d836-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>

<span data-ttu-id="9d836-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9d836-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="9d836-105">Erfahren Sie, wie ASP.NET-Seiten AJAX Control Toolkit-Steuerelemente und Extender hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9d836-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="9d836-106">Das AJAX Control Toolkit enthält eine Reihe von Steuerelementen und Steuerelement-Extendern.</span><span class="sxs-lookup"><span data-stu-id="9d836-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="9d836-107">In diesem kurzen Tutorial erfahren Sie, wie Steuerelemente und Extender hinzuzufügen, zu einer ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="9d836-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9d836-108">Anweisungen zum Installieren von AJAX Control Toolkit, und Hinzufügen von AJAX Control Toolkit zur Toolbox Visual Studio/Visual Web Developer, finden Sie im Tutorial [erste Schritte mit dem AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="9d836-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="9d836-109">Mithilfe von AJAX Control Toolkit-Steuerelementen</span><span class="sxs-lookup"><span data-stu-id="9d836-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="9d836-110">Ein AJAX Control Toolkit-Steuerelement funktioniert genauso wie ein normaler ASP.NET-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="9d836-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="9d836-111">Sie können das Steuerelement aus der Toolbox auf einer ASP.NET-Seite ziehen.</span><span class="sxs-lookup"><span data-stu-id="9d836-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="9d836-112">Sie können das Steuerelement auf der Seite in der Entwurfsansicht oder die Datenquellensicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9d836-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="9d836-113">Es ist eine spezielle Anforderung, bei der Verwendung der Steuerelemente aus dem AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="9d836-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="9d836-114">Die Seite muss ein ScriptManager-Steuerelement enthalten.</span><span class="sxs-lookup"><span data-stu-id="9d836-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="9d836-115">Das ScriptManager-Steuerelement ist dafür verantwortlich, alle notwendigen JavaScript das AJAX Control Toolkit-Steuerelemente erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="9d836-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="9d836-116">Die Registerkarte "AJAX Control Toolkit" enthält beispielsweise ein Steuerelement mit dem Namen der Editor-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="9d836-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="9d836-117">Dieses Steuerelement zeigt einen umfassenden HTML-Editor.</span><span class="sxs-lookup"><span data-stu-id="9d836-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="9d836-118">Um das Editor-Steuerelement zu einer Seite hinzufügen, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="9d836-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="9d836-119">Erstellen Sie eine neue ASP.NET-Seite mit dem Namen ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="9d836-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="9d836-120">Wählen Sie das ScriptManager-Steuerelement vom der Registerkarte "AJAX-Erweiterungen" in der Toolbox, und ziehen Sie das Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="9d836-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="9d836-121">Wählen Sie die Editor-Steuerelement vom dem AJAX Control Toolkit-Registerkarte in der Toolbox, und ziehen Sie das Steuerelement auf der Seite (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="9d836-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="9d836-122">Der Designer sollte wie in Abbildung 2 aussehen.</span><span class="sxs-lookup"><span data-stu-id="9d836-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="9d836-123">Führen Sie die Website, indem Sie durch Auswählen der Menüoption **Debuggen, Debugging starten** oder F5 drücken.</span><span class="sxs-lookup"><span data-stu-id="9d836-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="9d836-124">Die Seite in Abbildung 3 sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="9d836-124">You should see the page in Figure 3.</span></span>


[![S<span data-ttu-id="9d836-125">Wahl das Steuerelement der HTML-Editor]</span><span class="sxs-lookup"><span data-stu-id="9d836-125">electing the HTML Editor control]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

<span data-ttu-id="9d836-126">**Abbildung 01**: Auswählen des HTML-Editor-Steuerelements ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


[![V<span data-ttu-id="9d836-127">Isual Studio-Designer mit ScriptManager "und" Bearbeiten-Steuerelement]</span><span class="sxs-lookup"><span data-stu-id="9d836-127">isual Studio Designer with ScriptManager and Edit control]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

<span data-ttu-id="9d836-128">**Abbildung 02**: Visual Studio-Designer mit ScriptManager "und" Bearbeiten-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


[![T<span data-ttu-id="9d836-129">HE DisplayEditor.aspx Page]</span><span class="sxs-lookup"><span data-stu-id="9d836-129">he DisplayEditor.aspx page]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

<span data-ttu-id="9d836-130">**Abbildung 03**: Die Seite "DisplayEditor.aspx" ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="9d836-131">Mithilfe von AJAX Control Toolkit-Steuerelement-Extendern</span><span class="sxs-lookup"><span data-stu-id="9d836-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="9d836-132">Das AJAX Control Toolkit enthält auch die Steuerelement-Extendern.</span><span class="sxs-lookup"><span data-stu-id="9d836-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="9d836-133">Wie der Name schon sagt, wird ein Extender-Steuerelements die Funktionalität eines vorhandenen Steuerelements erweitert.</span><span class="sxs-lookup"><span data-stu-id="9d836-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="9d836-134">Beispielsweise erweitert einen ConfirmButton-Extender-Steuerelement das standardmäßige ASP.NET Schaltflächen-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="9d836-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="9d836-135">Der Extender ändert das Verhalten des Schaltfläche Steuerelements s, damit die Schaltfläche ein Bestätigungsdialogfeld angezeigt, wenn Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="9d836-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="9d836-136">Ein Extender-Steuerelements, wie ein AJAX Control Toolkit-Steuerelement erfordert ein ScriptManager-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="9d836-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="9d836-137">Sie müssen ein ScriptManager-Steuerelement zu einer Seite hinzufügen, bevor Sie Steuerelement-Extendern auf der Seite verwenden.</span><span class="sxs-lookup"><span data-stu-id="9d836-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="9d836-138">Um einen ConfirmButton-Extender-Steuerelement verwenden, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="9d836-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="9d836-139">Erstellen Sie eine neue ASP.NET-Seite mit dem Namen ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="9d836-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="9d836-140">Fügen Sie ein ScriptManager-Steuerelement auf der Seite durch Ziehen des Steuerelements auf der Seite vom der Registerkarte "AJAX-Erweiterungen".</span><span class="sxs-lookup"><span data-stu-id="9d836-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="9d836-141">Fügen Sie eine standardmäßige Schaltflächen-Steuerelement auf der Seite vom die Registerkarte "Standard" die Schaltfläche mit den in der Toolbox auf die Entwurfsoberfläche ziehen.</span><span class="sxs-lookup"><span data-stu-id="9d836-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="9d836-142">Klicken Sie auf die **Extender hinzufügen** aufgabenoption (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="9d836-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="9d836-143">Wählen Sie im Dialogfeld für die auswählen-Extender ConfirmButtonExtender (siehe Abbildung 5), und klicken Sie auf die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="9d836-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="9d836-144">Wählen Sie das Schaltflächen-Steuerelement im Designer, und erweitern Sie den Extender, "Button1"\_ConfirmButtonExtender-Knotens im Eigenschaftenfenster (siehe Abbildung 6).</span><span class="sxs-lookup"><span data-stu-id="9d836-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="9d836-145">Weisen Sie den Wert *wirklich?* der ConfirmText-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="9d836-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="9d836-146">Führen Sie die Seite im Menü die Option **Debuggen, Debugging starten** oder die F5-Taste drücken.</span><span class="sxs-lookup"><span data-stu-id="9d836-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


[![T<span data-ttu-id="9d836-147">er registerkartenoption Extender hinzufügen]</span><span class="sxs-lookup"><span data-stu-id="9d836-147">he Add Extender task option]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

<span data-ttu-id="9d836-148">**Abbildung 04**: Die registerkartenoption Extender hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


[![S<span data-ttu-id="9d836-149">Wahl des ConfirmButton-Extenders-Steuerelements]</span><span class="sxs-lookup"><span data-stu-id="9d836-149">electing the ConfirmButton control extender]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

<span data-ttu-id="9d836-150">**Abbildung 05**: Auswählen des ConfirmButton-Extenders-Steuerelements ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


[![S<span data-ttu-id="9d836-151">Skala-Einstellung einer Eigenschaft ConfirmButton-Steuerelements]</span><span class="sxs-lookup"><span data-stu-id="9d836-151">etting a ConfirmButton property]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

<span data-ttu-id="9d836-152">**Abbildung 06**: Festlegen einer Eigenschaft ConfirmButton-Steuerelements ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="9d836-153">Wenn die Seite geöffnet wird, sollte eine Schaltfläche angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="9d836-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="9d836-154">Wenn Sie die Schaltfläche klicken, erhalten Sie im Bestätigungsdialogfeld in Abbildung 7.</span><span class="sxs-lookup"><span data-stu-id="9d836-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


[![D<span data-ttu-id="9d836-155">Das Dialogfeld "Bestätigung" IsPlaying den Wert]</span><span class="sxs-lookup"><span data-stu-id="9d836-155">isplaying the confirmation dialog]</span></span>(using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

<span data-ttu-id="9d836-156">**Abbildung 07**: Das Dialogfeld "Bestätigung" angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="9d836-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="9d836-157">Beachten Sie, dass Sie normalerweise nicht den einen Extender-Steuerelements auf einer Seite ziehen.</span><span class="sxs-lookup"><span data-stu-id="9d836-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="9d836-158">Verwenden Sie stattdessen die **Extender hinzufügen** aufgabenoption Extender an ein Steuerelement hinzufügen, die Sie bereits zu einer Seite hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="9d836-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="9d836-159">Beachten Sie außerdem, dass Sie Control Extender-Eigenschaften festzulegen, öffnen das Eigenschaftenblatt für das Steuerelement erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="9d836-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="9d836-160">Ein einzelnes ASP.NET-Steuerelement kann durch mehrere Steuerelement-Extendern erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="9d836-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="9d836-161">Das Eigenschaftenblatt für das Steuerelement erweitert wird, werden alle der Steuerelement-Extendern, die dem Steuerelement zugeordnete aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="9d836-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d836-162">[Zurück](get-started-with-the-ajax-control-toolkit-cs.md)
> [Weiter](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9d836-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
