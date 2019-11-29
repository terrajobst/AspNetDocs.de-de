---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animation in Abhängigkeit von einer Bedingung (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Gibt an, ob eine Animation ist...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607016"
---
# <a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="1c0e2-104">Von einer Bedingung abhängige Animation (VB)</span><span class="sxs-lookup"><span data-stu-id="1c0e2-104">Animation Depending On a Condition (VB)</span></span>

<span data-ttu-id="1c0e2-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1c0e2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1c0e2-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1c0e2-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="1c0e2-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1c0e2-108">Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="1c0e2-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="1c0e2-109">Overview</span></span>

<span data-ttu-id="1c0e2-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1c0e2-111">Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="1c0e2-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="1c0e2-112">Steps</span></span>

<span data-ttu-id="1c0e2-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="1c0e2-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="1c0e2-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="1c0e2-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="1c0e2-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="1c0e2-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="1c0e2-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="1c0e2-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="1c0e2-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="1c0e2-118">Anstelle einer regulären Animation kommt das `<Condition>`-Element ins Spiel.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="1c0e2-119">Der JavaScript-Code, der als Wert des `ConditionScript` Attributs bereitgestellt wird, wird zur Laufzeit ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="1c0e2-120">Wenn true ausgewertet wird, wird die Animation ausgeführt; andernfalls nicht.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="1c0e2-121">Das folgende Markup stellt zwei Animationen bereit, die jeweils in 50% der Fälle nach dem Zufallsprinzip ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="1c0e2-122">Da es möglicherweise nur eine Animation innerhalb `<OnLoad>`gibt, werden die beiden `<Condition>` Animationen mit dem `<Sequence>`-Element verknüpft:</span><span class="sxs-lookup"><span data-stu-id="1c0e2-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="1c0e2-123">Beachten Sie, dass das kleiner-als-Zeichen (`<`) im `ConditionScript`-Attribut mit Escapezeichen versehen werden muss ().</span><span class="sxs-lookup"><span data-stu-id="1c0e2-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="1c0e2-124">Wenn Sie dieses Skript ausführen, wird entweder weder eine Animation ausgeführt noch eine der beiden Aktionen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1c0e2-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="1c0e2-125">[![der Bereich ausgeblendet wird, ohne die Größe zu ändern, also wird die zweite Animation ausgeführt, der erste nicht](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1c0e2-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="1c0e2-126">Der Bereich wird ausgeblendet, ohne die Größe zu ändern. Daher wird die zweite Animation ausgeführt, der erste nicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animation-depending-on-a-condition-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="1c0e2-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c0e2-127">[Zurück](executing-several-animations-after-each-other-vb.md)
> [Weiter](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1c0e2-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
