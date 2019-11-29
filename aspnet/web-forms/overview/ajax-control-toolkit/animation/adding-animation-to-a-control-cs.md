---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Hinzufügen von Animationen zu einemC#Steuerelement () | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: dd63157fe616c5f6874b7cca11f4ede15018df04
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607130"
---
# <a name="adding-animation-to-a-control-c"></a><span data-ttu-id="ff804-104">Hinzufügen von Animationen zu einem Steuerelement (C#)</span><span class="sxs-lookup"><span data-stu-id="ff804-104">Adding Animation to a Control (C#)</span></span>

<span data-ttu-id="ff804-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ff804-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ff804-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ff804-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="ff804-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ff804-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff804-108">In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="ff804-108">This tutorial shows how to set up such an animation.</span></span>

## <a name="overview"></a><span data-ttu-id="ff804-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="ff804-109">Overview</span></span>

<span data-ttu-id="ff804-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ff804-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff804-111">In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="ff804-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ff804-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="ff804-112">Steps</span></span>

<span data-ttu-id="ff804-113">Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="ff804-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ff804-114">Die Animation in diesem Szenario wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="ff804-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ff804-115">Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:</span><span class="sxs-lookup"><span data-stu-id="ff804-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="ff804-116">Als nächstes benötigen wir die `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="ff804-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="ff804-117">Nachdem Sie eine `ID` und die übliche `runat="server"`bereitgestellt haben, muss das `TargetControlID`-Attribut auf das-Steuerelement festgelegt werden, um in unserem Fall den Bereich zu animieren:</span><span class="sxs-lookup"><span data-stu-id="ff804-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ff804-118">Die gesamte Animation wird deklarativ mithilfe einer XML-Syntax angewendet, die leider nicht vollständig von Visual Studio IntelliSense unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="ff804-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="ff804-119">Der Stamm Knoten ist innerhalb dieses Knotens `<Animations>;`. es sind mehrere Ereignisse zulässig, die bestimmen, wann die Animation (n) stattfindet:</span><span class="sxs-lookup"><span data-stu-id="ff804-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="ff804-120">`OnClick` (Maus Klick)</span><span class="sxs-lookup"><span data-stu-id="ff804-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="ff804-121">`OnHoverOut` (wenn die Maus ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="ff804-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ff804-122">`OnHoverOver` (wenn mit der Maus auf ein Steuerelement gezeigt wird, wird die `OnHoverOut` Animation angehalten)</span><span class="sxs-lookup"><span data-stu-id="ff804-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="ff804-123">`OnLoad` (wenn die Seite geladen wurde)</span><span class="sxs-lookup"><span data-stu-id="ff804-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="ff804-124">`OnMouseOut` (wenn die Maus ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="ff804-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ff804-125">`OnMouseOver` (wenn mit der Maus auf ein Steuerelement gezeigt wird, wobei die `OnMouseOut` Animation nicht angehalten wird)</span><span class="sxs-lookup"><span data-stu-id="ff804-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="ff804-126">Das Framework enthält eine Reihe von Animationen, die jeweils durch ein eigenes XML-Element dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ff804-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="ff804-127">Hier finden Sie eine Auswahl:</span><span class="sxs-lookup"><span data-stu-id="ff804-127">Here is a selection:</span></span>

- <span data-ttu-id="ff804-128">`<Color>` (Ändern einer Farbe)</span><span class="sxs-lookup"><span data-stu-id="ff804-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="ff804-129">`<FadeIn>` (ausblenden)</span><span class="sxs-lookup"><span data-stu-id="ff804-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="ff804-130">`<FadeOut>` (ausblenden)</span><span class="sxs-lookup"><span data-stu-id="ff804-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="ff804-131">`<Property>` (Ändern der-Eigenschaft eines Steuer Elements)</span><span class="sxs-lookup"><span data-stu-id="ff804-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="ff804-132">`<Pulse>` (pulsierende)</span><span class="sxs-lookup"><span data-stu-id="ff804-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="ff804-133">`<Resize>` (Ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="ff804-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="ff804-134">`<Scale>` (proportional zum Ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="ff804-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="ff804-135">In diesem Beispiel muss der Bereich ausgeblendet werden. Die Animation muss 1,5 Sekunden (`Duration`-Attribut) annehmen und 24 Frames (Animations Schritte) pro Sekunde (`Fps` Attribut) anzeigen.</span><span class="sxs-lookup"><span data-stu-id="ff804-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="ff804-136">Hier ist das komplette Markup für das `AnimationExtender`-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="ff804-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="ff804-137">Wenn Sie dieses Skript ausführen, wird der Bereich innerhalb von 1 bis halbe Sekunden angezeigt und ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="ff804-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>

<span data-ttu-id="ff804-138">[![der Bereich ausgeblendet wird](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ff804-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="ff804-139">Der Bereich wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-animation-to-a-control-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="ff804-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ff804-140">Nächste</span><span class="sxs-lookup"><span data-stu-id="ff804-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
