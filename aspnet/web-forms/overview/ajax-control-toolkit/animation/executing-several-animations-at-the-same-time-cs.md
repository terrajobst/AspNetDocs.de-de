---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Gleich zeidas Ausführen mehrerer Animationen (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie ermöglicht das Ausführen von "Schweregrad"...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: fe71feccbcbc4ee8e9cdc09d6220de6a53dd2d2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483969"
---
# <a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="c5d6b-104">Gleich zeidas Ausführen mehrerer Animationen (C#)</span><span class="sxs-lookup"><span data-stu-id="c5d6b-104">Executing Several Animations at The Same Time (C#)</span></span>

<span data-ttu-id="c5d6b-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c5d6b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c5d6b-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c5d6b-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="c5d6b-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c5d6b-108">Dadurch können mehrere Animationen parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-108">It allows to run several animations in a parallel fashion.</span></span>

## <a name="overview"></a><span data-ttu-id="c5d6b-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c5d6b-109">Overview</span></span>

<span data-ttu-id="c5d6b-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c5d6b-111">Dadurch können mehrere Animationen parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="c5d6b-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="c5d6b-112">Steps</span></span>

<span data-ttu-id="c5d6b-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="c5d6b-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="c5d6b-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="c5d6b-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="c5d6b-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="c5d6b-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="c5d6b-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="c5d6b-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="c5d6b-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="c5d6b-118">Im Allgemeinen akzeptiert `<OnLoad>` nur eine Animation.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="c5d6b-119">Das Animations Framework ermöglicht es Ihnen, mehrere Animationen mit dem `<Parallel>`-Element in ein Element einzubinden.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="c5d6b-120">Alle Animationen in `<Parallel>` werden gleichzeitig ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="c5d6b-121">Im folgenden finden Sie ein mögliches Markup für das `AnimationExtender`-Steuerelement, wobei der Bereich gleichzeitig ausgeblendet und seine Größe geändert wird:</span><span class="sxs-lookup"><span data-stu-id="c5d6b-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="c5d6b-122">Und tatsächlich: Wenn Sie dieses Skript ausführen, wird der Bereich angezeigt, und die Größe wird geändert (mehr als die Breite und die Höhe der Höhe) und gleichzeitig ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="c5d6b-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>

<span data-ttu-id="c5d6b-123">[![der Bereich ausgeblendet wird und seine Größe ändert (einschließlich seines Inhalts, dank der Rendering-Engine des Browsers)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c5d6b-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="c5d6b-124">Der Bereich wird ausgeblendet und seine Größe angepasst (einschließlich seines Inhalts, dank der Rendering-Engine des Browsers) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-at-the-same-time-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="c5d6b-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c5d6b-125">[Zurück](adding-animation-to-a-control-cs.md)
> [Weiter](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c5d6b-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
