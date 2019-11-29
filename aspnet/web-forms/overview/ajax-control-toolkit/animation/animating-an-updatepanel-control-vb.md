---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animieren eines Update Panel-Steuer Elements (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Für den Inhalt eines...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d66dda923940a328c0757049c9d8bfa3b2d2b9fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607065"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="fc9dd-104">Animieren eines UpdatePanel-Steuerelements (VB)</span><span class="sxs-lookup"><span data-stu-id="fc9dd-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="fc9dd-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fc9dd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fc9dd-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="fc9dd-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="fc9dd-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fc9dd-108">Für den Inhalt eines Update Panel ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="fc9dd-109">In diesem Tutorial wird gezeigt, wie eine solche Animation für ein Update Panel eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="fc9dd-110">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="fc9dd-110">Overview</span></span>

<span data-ttu-id="fc9dd-111">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fc9dd-112">Für den Inhalt einer `UpdatePanel`ist ein spezieller Extender vorhanden, der sich stark auf das Animations Framework stützt: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="fc9dd-113">In diesem Tutorial wird gezeigt, wie Sie eine solche Animation für eine `UpdatePanel`einrichten.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="fc9dd-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="fc9dd-114">Steps</span></span>

<span data-ttu-id="fc9dd-115">Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="fc9dd-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="fc9dd-116">Die Animation in diesem Szenario wird auf ein ASP.net-`Wizard`-websteuer Element angewendet, das sich in einem `UpdatePanel`befindet.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="fc9dd-117">Drei (beliebige) Schritte bieten genügend Optionen, um Postbacks auszublenden:</span><span class="sxs-lookup"><span data-stu-id="fc9dd-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="fc9dd-118">Das für das `UpdatePanelAnimationExtender` Steuerelement erforderliche Markup ähnelt dem Markup, das für die `AnimationExtender`verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="fc9dd-119">Im `TargetControlID`-Attribut werden die `ID` des zu animierenden `UpdatePanel` bereitgestellt. im `UpdatePanelAnimationExtender`-Steuerelement enthält das `<Animations>`-Element XML-Markup für die Animation (en).</span><span class="sxs-lookup"><span data-stu-id="fc9dd-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="fc9dd-120">Es gibt jedoch einen Unterschied: die Anzahl von Ereignissen und Ereignis Handlern ist im Vergleich zu `AnimationExtender`beschränkt.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="fc9dd-121">Bei `UpdatePanels`sind nur zwei davon vorhanden:</span><span class="sxs-lookup"><span data-stu-id="fc9dd-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="fc9dd-122">`<OnUpdated>`, wenn Update Panel aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="fc9dd-123">`<OnUpdating>`, wenn Update Panel mit der Aktualisierung beginnt</span><span class="sxs-lookup"><span data-stu-id="fc9dd-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="fc9dd-124">In diesem Szenario sollte der neue Inhalt des `UpdatePanel` (nach dem Postback) ausgeblendet werden.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="fc9dd-125">Dies ist das erforderliche Markup hierfür:</span><span class="sxs-lookup"><span data-stu-id="fc9dd-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="fc9dd-126">Wenn nun ein Postback innerhalb von Update Panel auftritt, wird der neue Inhalt des Bereichs reibungslos ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="fc9dd-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="fc9dd-127">[![der nächste Schritt des Assistenten wird ausgeblendet.](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc9dd-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="fc9dd-128">Der nächste Schritt des Assistenten wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fc9dd-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc9dd-129">[Zurück](changing-an-animation-using-client-side-code-vb.md)
> [Weiter](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fc9dd-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
