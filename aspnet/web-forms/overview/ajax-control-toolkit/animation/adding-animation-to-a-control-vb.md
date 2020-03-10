---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Hinzufügen von Animationen zu einem Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484125"
---
# <a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="2d6ad-104">Hinzufügen von Animationen zu einem Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-104">Adding Animation to a Control (VB)</span></span>

<span data-ttu-id="2d6ad-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2d6ad-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="2d6ad-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2d6ad-108">In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-108">This tutorial shows how to set up such an animation.</span></span>

## <a name="overview"></a><span data-ttu-id="2d6ad-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2d6ad-109">Overview</span></span>

<span data-ttu-id="2d6ad-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2d6ad-111">In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="2d6ad-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="2d6ad-112">Steps</span></span>

<span data-ttu-id="2d6ad-113">Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2d6ad-114">Die Animation in diesem Szenario wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2d6ad-115">Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="2d6ad-116">Als nächstes benötigen wir die `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="2d6ad-117">Nachdem Sie eine `ID` und die übliche `runat="server"`bereitgestellt haben, muss das `TargetControlID`-Attribut auf das-Steuerelement festgelegt werden, um in unserem Fall den Bereich zu animieren:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="2d6ad-118">Die gesamte Animation wird deklarativ mithilfe einer XML-Syntax angewendet, die leider nicht vollständig von Visual Studio IntelliSense unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="2d6ad-119">Der Stamm Knoten ist innerhalb dieses Knotens `<Animations>;`. es sind mehrere Ereignisse zulässig, die bestimmen, wann die Animation (n) stattfindet:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="2d6ad-120">`OnClick` (Maus Klick)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="2d6ad-121">`OnHoverOut` (wenn die Maus ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="2d6ad-122">`OnHoverOver` (wenn mit der Maus auf ein Steuerelement gezeigt wird, wird die `OnHoverOut` Animation angehalten)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="2d6ad-123">`OnLoad` (wenn die Seite geladen wurde)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="2d6ad-124">`OnMouseOut` (wenn die Maus ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="2d6ad-125">`OnMouseOver` (wenn mit der Maus auf ein Steuerelement gezeigt wird, wobei die `OnMouseOut` Animation nicht angehalten wird)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="2d6ad-126">Das Framework enthält eine Reihe von Animationen, die jeweils durch ein eigenes XML-Element dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="2d6ad-127">Hier finden Sie eine Auswahl:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-127">Here is a selection:</span></span>

- <span data-ttu-id="2d6ad-128">`<Color>` (Ändern einer Farbe)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="2d6ad-129">`<FadeIn>` (ausblenden)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="2d6ad-130">`<FadeOut>` (ausblenden)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="2d6ad-131">`<Property>` (Ändern der-Eigenschaft eines Steuer Elements)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="2d6ad-132">`<Pulse>` (pulsierende)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="2d6ad-133">`<Resize>` (Ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="2d6ad-134">`<Scale>` (proportional zum Ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="2d6ad-135">In diesem Beispiel muss der Bereich ausgeblendet werden. Die Animation muss 1,5 Sekunden (`Duration`-Attribut) annehmen und 24 Frames (Animations Schritte) pro Sekunde (`Fps` Attribut) anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="2d6ad-136">Hier ist das komplette Markup für das `AnimationExtender`-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="2d6ad-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="2d6ad-137">Wenn Sie dieses Skript ausführen, wird der Bereich innerhalb von 1 bis halbe Sekunden angezeigt und ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="2d6ad-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>

<span data-ttu-id="2d6ad-138">[![der Bereich ausgeblendet wird](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2d6ad-139">Der Bereich wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-animation-to-a-control-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="2d6ad-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d6ad-140">[Zurück](dynamically-controlling-updatepanel-animations-cs.md)
> [Weiter](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2d6ad-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
