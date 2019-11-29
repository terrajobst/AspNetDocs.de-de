---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Auslösen einer Animation in einem anderen SteuerC#Element () | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Im Allgemeinen wird das Starten einer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599613"
---
# <a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="0288c-104">Auslösen einer Animation in einem anderen Steuerelement (C#)</span><span class="sxs-lookup"><span data-stu-id="0288c-104">Triggering an Animation in another Control (C#)</span></span>

<span data-ttu-id="0288c-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0288c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0288c-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0288c-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="0288c-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="0288c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0288c-108">Im Allgemeinen wird das Starten einer Animation durch eine Benutzerinteraktion mit demselben Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="0288c-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="0288c-109">Es ist jedoch auch möglich, mit einem Steuerelement zu interagieren und dann ein anderes Steuerelement zu Animation.</span><span class="sxs-lookup"><span data-stu-id="0288c-109">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="overview"></a><span data-ttu-id="0288c-110">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="0288c-110">Overview</span></span>

<span data-ttu-id="0288c-111">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="0288c-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="0288c-112">Im Allgemeinen wird das Starten einer Animation durch eine Benutzerinteraktion mit demselben Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="0288c-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="0288c-113">Es ist jedoch auch möglich, mit einem Steuerelement zu interagieren und dann ein anderes Steuerelement zu Animation.</span><span class="sxs-lookup"><span data-stu-id="0288c-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="0288c-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="0288c-114">Steps</span></span>

<span data-ttu-id="0288c-115">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="0288c-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="0288c-116">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="0288c-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="0288c-117">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="0288c-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="0288c-118">Um mit der Animation des Panels zu beginnen, wird eine HTML-Schaltfläche verwendet.</span><span class="sxs-lookup"><span data-stu-id="0288c-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="0288c-119">Beachten Sie, dass `<input type="button" />` für `<asp:Button />` bevorzugt ist, da wir kein Postback wünschen, wenn der Benutzer auf diese Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="0288c-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="0288c-120">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an.</span><span class="sxs-lookup"><span data-stu-id="0288c-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="0288c-121">Es ist wichtig, `TargetControlID` auf die ID der Schaltfläche (das Element, das die Animation auslöst) festzulegen, nicht auf die ID des Bereichs (das zu animierende Element).</span><span class="sxs-lookup"><span data-stu-id="0288c-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="0288c-122">Platzieren Sie die Animationen innerhalb des `<Animations>` Knotens wie üblich.</span><span class="sxs-lookup"><span data-stu-id="0288c-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="0288c-123">Um den Bereich zu ändern, nicht die Schaltfläche, legen Sie das `AnimationTarget`-Attribut für jedes Animations Element innerhalb `AnimationExtender`fest.</span><span class="sxs-lookup"><span data-stu-id="0288c-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="0288c-124">Der Wert für `AnimationTarget` ist natürlich die ID des Panels.</span><span class="sxs-lookup"><span data-stu-id="0288c-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="0288c-125">Auf diese Weise werden die Animationen mit dem Panel und nicht mit der auslösenden Schaltfläche durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="0288c-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="0288c-126">Im folgenden finden Sie `AnimationExtender` Markup für dieses Szenario:</span><span class="sxs-lookup"><span data-stu-id="0288c-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="0288c-127">Beachten Sie die besondere Reihenfolge, in der die einzelnen Animationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0288c-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="0288c-128">Zuerst wird die Schaltfläche deaktiviert, sobald die Animation ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="0288c-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="0288c-129">Da das `<EnableAction>`-Element kein `AnimationTarget` Attribut enthält, wird diese Animation auf das ursprüngliche Steuerelement angewendet: die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="0288c-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="0288c-130">Die nächsten zwei Animations Schritte müssen parallel ausgeführt werden (`<Parallel>`-Element).</span><span class="sxs-lookup"><span data-stu-id="0288c-130">The next two animation steps shall be carried out in parallel (`<Parallel>` element).</span></span> <span data-ttu-id="0288c-131">Beide Attribute `AnimationTarget` sind auf `"Panel1"`festgelegt, sodass das Panel und nicht die Schaltfläche animiert werden.</span><span class="sxs-lookup"><span data-stu-id="0288c-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>

<span data-ttu-id="0288c-132">[![mit einem Mausklick auf die Schaltfläche startet die Panel-Animation.](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0288c-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="0288c-133">Mit dem Mauszeiger auf die Schaltfläche wird die Panel-Animation gestartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](triggering-an-animation-in-another-control-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="0288c-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0288c-134">[Zurück](disabling-actions-during-animation-cs.md)
> [Weiter](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0288c-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
