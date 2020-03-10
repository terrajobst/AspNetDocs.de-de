---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Gleich zeidas Ausführen mehrerer Animationen (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie ermöglicht das Ausführen von "Schweregrad"...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: fc11debd06c897c29db56d42167d483f5608cdf8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497799"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="ad0dd-104">Gleich zeidas Ausführen mehrerer Animationen (VB)</span><span class="sxs-lookup"><span data-stu-id="ad0dd-104">Executing Several Animations at The Same Time (VB)</span></span>

<span data-ttu-id="ad0dd-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ad0dd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ad0dd-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ad0dd-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="ad0dd-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ad0dd-108">Dadurch können mehrere Animationen parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-108">It allows to run several animations in a parallel fashion.</span></span>

## <a name="overview"></a><span data-ttu-id="ad0dd-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="ad0dd-109">Overview</span></span>

<span data-ttu-id="ad0dd-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ad0dd-111">Dadurch können mehrere Animationen parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="ad0dd-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="ad0dd-112">Steps</span></span>

<span data-ttu-id="ad0dd-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="ad0dd-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="ad0dd-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="ad0dd-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="ad0dd-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="ad0dd-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="ad0dd-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="ad0dd-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="ad0dd-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="ad0dd-118">Im Allgemeinen akzeptiert `<OnLoad>` nur eine Animation.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="ad0dd-119">Das Animations Framework ermöglicht es Ihnen, mehrere Animationen mit dem `<Parallel>`-Element in ein Element einzubinden.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="ad0dd-120">Alle Animationen in `<Parallel>` werden gleichzeitig ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="ad0dd-121">Im folgenden finden Sie ein mögliches Markup für das `AnimationExtender`-Steuerelement, wobei der Bereich gleichzeitig ausgeblendet und seine Größe geändert wird:</span><span class="sxs-lookup"><span data-stu-id="ad0dd-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="ad0dd-122">Und tatsächlich: Wenn Sie dieses Skript ausführen, wird der Bereich angezeigt, und die Größe wird geändert (mehr als die Breite und die Höhe der Höhe) und gleichzeitig ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="ad0dd-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>

<span data-ttu-id="ad0dd-123">[![der Bereich ausgeblendet wird und seine Größe ändert (einschließlich seines Inhalts, dank der Rendering-Engine des Browsers)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ad0dd-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="ad0dd-124">Der Bereich wird ausgeblendet und seine Größe angepasst (einschließlich seines Inhalts, dank der Rendering-Engine des Browsers) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-at-the-same-time-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="ad0dd-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ad0dd-125">[Zurück](adding-animation-to-a-control-vb.md)
> [Weiter](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ad0dd-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
