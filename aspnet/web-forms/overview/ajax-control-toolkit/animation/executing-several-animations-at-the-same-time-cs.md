---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Ausführen mehrerer Animationen zur gleichen Zeit (C#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Er ermöglicht es, durch Fallenlassen ausführen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: f228ca600691ea4820ecdb79dea5e70054975f32
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58422947"
---
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="633ec-104">Ausführen mehrerer Animationen zur gleichen Zeit (C#)</span><span class="sxs-lookup"><span data-stu-id="633ec-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="633ec-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="633ec-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="633ec-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="633ec-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="633ec-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="633ec-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="633ec-108">Sie können mehrere Animationen auf eine Weise parallel ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="633ec-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="633ec-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="633ec-109">Overview</span></span>

<span data-ttu-id="633ec-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="633ec-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="633ec-111">Sie können mehrere Animationen auf eine Weise parallel ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="633ec-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="633ec-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="633ec-112">Steps</span></span>

<span data-ttu-id="633ec-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="633ec-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="633ec-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="633ec-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="633ec-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="633ec-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="633ec-116">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="633ec-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="633ec-117">In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="633ec-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="633ec-118">Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation.</span><span class="sxs-lookup"><span data-stu-id="633ec-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="633ec-119">Der Animation-Framework können Sie mehrere Animationen in einer mit join die `<Parallel>` Element.</span><span class="sxs-lookup"><span data-stu-id="633ec-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="633ec-120">Alle Animationen in `<Parallel>` zur gleichen Zeit ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="633ec-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="633ec-121">Hier ist die einem möglichen Markup für die `AnimationExtender` Steuerelement ausgeblendet wird, und Ändern der Größe des Bereichs zur gleichen Zeit:</span><span class="sxs-lookup"><span data-stu-id="633ec-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="633ec-122">Und tatsächlich: Wenn Sie dieses Skript ausführen, wird im Bereich angezeigt wird, klicken Sie dann die Größe (mehr als verdreifacht Sie seine Breite sich und seine Höhe halbiert) und zur gleichen Zeit ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="633ec-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>


<span data-ttu-id="633ec-123">[![Der Bereich ausgeblendet wird, und Ändern der Größe (einschließlich seines Inhalts Dank des Browsers Rendering-Engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="633ec-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="633ec-124">Der Bereich ausgeblendet wird, und Ändern der Größe (einschließlich seines Inhalts Dank des Browsers Rendering-Engine) ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="633ec-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="633ec-125">[Zurück](adding-animation-to-a-control-cs.md)
> [Weiter](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="633ec-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
