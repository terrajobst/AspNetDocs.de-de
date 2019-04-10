---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animieren eines UpdatePanel-Steuerelements (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a7c40ebe359e21602d9f1de8205e1a7c808acc85
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384252"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="1fead-104">Animieren eines UpdatePanel-Steuerelements (VB)</span><span class="sxs-lookup"><span data-stu-id="1fead-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="1fead-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1fead-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1fead-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1fead-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="1fead-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1fead-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1fead-108">Für den Inhalt von einem UpdatePanel-Steuerelement vorhanden ist, ein spezieller Extender, der stark auf die Animationsframework basiert: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="1fead-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="1fead-109">In diesem Tutorial veranschaulicht, wie Sie solche eine Animation für ein UpdatePanel einrichten.</span><span class="sxs-lookup"><span data-stu-id="1fead-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="1fead-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1fead-110">Overview</span></span>

<span data-ttu-id="1fead-111">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1fead-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1fead-112">Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sehr auf die Animationsframework basiert: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="1fead-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="1fead-113">In diesem Tutorial wird gezeigt, wie solche eine Animation für Einrichten einer `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="1fead-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="1fead-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="1fead-114">Steps</span></span>

<span data-ttu-id="1fead-115">Der erste Schritt ist wie üblich, enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="1fead-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="1fead-116">Die Animation in diesem Szenario gelten für eine ASP.NET `Wizard` Websteuerelement Wohnsitz in einem `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="1fead-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="1fead-117">Drei (beliebiger) Schritte stellen genügend Optionen zum Auslösen von Postbacks bereit:</span><span class="sxs-lookup"><span data-stu-id="1fead-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="1fead-118">Das Markup für die `UpdatePanelAnimationExtender` Steuerelement ist sehr ähnlich, mit das Markup für die `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="1fead-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="1fead-119">In der `TargetControlID` Attribut wir bieten die `ID` von der `UpdatePanel` animieren; innerhalb der `UpdatePanelAnimationExtender` -Steuerelement, die `<Animations>` -Element enthält XML-Markup für die Animation(s).</span><span class="sxs-lookup"><span data-stu-id="1fead-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="1fead-120">Es gibt jedoch einen Unterschied: Die Größe der Ereignisse und Ereignishandler ist im Vergleich zur beschränkt `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="1fead-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="1fead-121">Für `UpdatePanels`, nur zwei davon vorhanden:</span><span class="sxs-lookup"><span data-stu-id="1fead-121">For `UpdatePanels`, only two of them exist:</span></span>

- `<OnUpdated>` <span data-ttu-id="1fead-122">Wenn UpdatePanel aktualisiert wurde</span><span class="sxs-lookup"><span data-stu-id="1fead-122">when the UpdatePanel has been updated</span></span>
- `<OnUpdating>` <span data-ttu-id="1fead-123">Beim Starten der UpdatePanel aktualisiert</span><span class="sxs-lookup"><span data-stu-id="1fead-123">when the UpdatePanel starts updating</span></span>

<span data-ttu-id="1fead-124">In diesem Szenario wird der neue Inhalt, der die `UpdatePanel` (nach dem Postback) eingeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="1fead-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="1fead-125">Dies ist die notwendigen Markups dafür:</span><span class="sxs-lookup"><span data-stu-id="1fead-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="1fead-126">Nun, wenn ein Postback in UpdatePanel, was erfolgt, den neuen Inhalt des Bereichs reibungslos eingeblendet.</span><span class="sxs-lookup"><span data-stu-id="1fead-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


[![T<span data-ttu-id="1fead-127">HE Nächstes-Assistent wird eingeblendet,]</span><span class="sxs-lookup"><span data-stu-id="1fead-127">he next wizard step is fading in]</span></span>(animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

<span data-ttu-id="1fead-128">Der nächste Assistentenschritt wird eingeblendet, ([klicken Sie, um das Bild in voller Größe anzeigen](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1fead-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1fead-129">[Zurück](changing-an-animation-using-client-side-code-vb.md)
> [Weiter](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1fead-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
