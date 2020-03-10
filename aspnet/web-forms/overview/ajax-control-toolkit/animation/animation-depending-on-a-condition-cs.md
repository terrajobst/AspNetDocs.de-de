---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animation in Abhängigkeit von einer BedingungC#() | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Gibt an, ob eine Animation ist...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484167"
---
# <a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="33060-104">Von einer Bedingung abhängige Animation (C#)</span><span class="sxs-lookup"><span data-stu-id="33060-104">Animation Depending On a Condition (C#)</span></span>

<span data-ttu-id="33060-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="33060-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="33060-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="33060-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="33060-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="33060-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="33060-108">Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.</span><span class="sxs-lookup"><span data-stu-id="33060-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="33060-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="33060-109">Overview</span></span>

<span data-ttu-id="33060-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="33060-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="33060-111">Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.</span><span class="sxs-lookup"><span data-stu-id="33060-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="33060-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="33060-112">Steps</span></span>

<span data-ttu-id="33060-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="33060-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="33060-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="33060-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="33060-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="33060-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="33060-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="33060-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="33060-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="33060-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="33060-118">Anstelle einer regulären Animation kommt das `<Condition>`-Element ins Spiel.</span><span class="sxs-lookup"><span data-stu-id="33060-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="33060-119">Der JavaScript-Code, der als Wert des `ConditionScript` Attributs bereitgestellt wird, wird zur Laufzeit ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="33060-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="33060-120">Wenn true ausgewertet wird, wird die Animation ausgeführt; andernfalls nicht.</span><span class="sxs-lookup"><span data-stu-id="33060-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="33060-121">Das folgende Markup stellt zwei Animationen bereit, die jeweils in 50% der Fälle nach dem Zufallsprinzip ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="33060-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="33060-122">Da es möglicherweise nur eine Animation innerhalb `<OnLoad>`gibt, werden die beiden `<Condition>` Animationen mit dem `<Sequence>`-Element verknüpft:</span><span class="sxs-lookup"><span data-stu-id="33060-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="33060-123">Beachten Sie, dass das kleiner-als-Zeichen (`<`) im `ConditionScript`-Attribut mit Escapezeichen versehen werden muss ().</span><span class="sxs-lookup"><span data-stu-id="33060-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="33060-124">Wenn Sie dieses Skript ausführen, wird entweder weder eine Animation ausgeführt noch eine der beiden Aktionen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="33060-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="33060-125">[![der Bereich ausgeblendet wird, ohne die Größe zu ändern, also wird die zweite Animation ausgeführt, der erste nicht](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="33060-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="33060-126">Der Bereich wird ausgeblendet, ohne die Größe zu ändern. Daher wird die zweite Animation ausgeführt, der erste nicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animation-depending-on-a-condition-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="33060-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="33060-127">[Zurück](executing-several-animations-after-each-other-cs.md)
> [Weiter](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="33060-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
