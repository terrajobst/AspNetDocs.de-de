---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animieren als Reaktion auf eine Benutzerinteraktion (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können Sternen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: c38160ffa9965384cf4eae2ebda52bd62b766bba
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59396231"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="01762-104">Animieren als Reaktion auf eine Benutzerinteraktion (VB)</span><span class="sxs-lookup"><span data-stu-id="01762-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="01762-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="01762-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="01762-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="01762-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="01762-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="01762-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="01762-108">Die Animationen können automatisch starten, oder es werden möglicherweise ausgelöst durch einen Benutzereingriff, z. B. indem Sie mit der Maus auf.</span><span class="sxs-lookup"><span data-stu-id="01762-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="01762-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="01762-109">Overview</span></span>

<span data-ttu-id="01762-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="01762-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="01762-111">Die Animationen können automatisch starten, oder es werden möglicherweise ausgelöst durch einen Benutzereingriff, z. B. indem Sie mit der Maus auf.</span><span class="sxs-lookup"><span data-stu-id="01762-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="01762-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="01762-112">Steps</span></span>

<span data-ttu-id="01762-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="01762-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="01762-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="01762-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="01762-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="01762-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="01762-116">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="01762-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="01762-117">In der `<Animations>` Knoten stehen fünf Methoden zum Starten der Animation über eine Benutzerinteraktion (das fehlende Element ist `<OnLoad>` die wird ausgeführt, nachdem die gesamte Seite vollständig geladen wurde):</span><span class="sxs-lookup"><span data-stu-id="01762-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- `<OnClick>` <span data-ttu-id="01762-118">(Mausklick auf das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="01762-118">(mouse click on the control)</span></span>
- `<OnHoverOut>` <span data-ttu-id="01762-119">(Mauszeiger das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="01762-119">(mouse leaves the control)</span></span>
- `<OnHoverOver>` <span data-ttu-id="01762-120">(Maus bewegt wird, auf ein Steuerelement, das Beenden der `<OnHoverOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="01762-120">(mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- `<OnMouseOut>` <span data-ttu-id="01762-121">(Mauszeiger ein Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="01762-121">(mouse leaves a control)</span></span>
- `<OnMouseOver>` <span data-ttu-id="01762-122">(Maus bewegt wird, auf ein Steuerelement, das nicht beendet die `<OnMouseOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="01762-122">(mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="01762-123">In diesem Szenario `<OnClick>` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="01762-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="01762-124">Klickt der Benutzer im Bereich angepasst wird und zur gleichen Zeit ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="01762-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![A <span data-ttu-id="01762-125">Mausklick startet die Animation]</span><span class="sxs-lookup"><span data-stu-id="01762-125">mouse click starts the animation]</span></span>(animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

<span data-ttu-id="01762-126">Startet die Animation, klicken mit der Maus ([klicken Sie, um das Bild in voller Größe anzeigen](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="01762-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01762-127">[Zurück](picking-one-animation-out-of-a-list-vb.md)
> [Weiter](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="01762-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
