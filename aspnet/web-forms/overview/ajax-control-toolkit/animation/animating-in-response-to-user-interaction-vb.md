---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animieren als Reaktion auf Benutzerinteraktion (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können Stern...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607035"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="e5b4e-104">Animieren als Reaktion auf eine Benutzerinteraktion (VB)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="e5b4e-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e5b4e-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="e5b4e-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="e5b4e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e5b4e-108">Die Animationen können automatisch gestartet werden, oder Sie können durch Benutzerinteraktion ausgelöst werden, z. b. durch Klicken mit der Maus.</span><span class="sxs-lookup"><span data-stu-id="e5b4e-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="overview"></a><span data-ttu-id="e5b4e-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="e5b4e-109">Overview</span></span>

<span data-ttu-id="e5b4e-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="e5b4e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e5b4e-111">Die Animationen können automatisch gestartet werden, oder Sie können durch Benutzerinteraktion ausgelöst werden, z. b. durch Klicken mit der Maus.</span><span class="sxs-lookup"><span data-stu-id="e5b4e-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="e5b4e-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="e5b4e-112">Steps</span></span>

<span data-ttu-id="e5b4e-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="e5b4e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="e5b4e-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="e5b4e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="e5b4e-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="e5b4e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="e5b4e-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="e5b4e-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="e5b4e-117">Innerhalb des `<Animations>` Knotens gibt es fünf Möglichkeiten, um die Animation über eine Benutzerinteraktion zu starten (das fehlende Element ist `<OnLoad>`, das ausgeführt wird, sobald die gesamte Seite vollständig geladen wurde):</span><span class="sxs-lookup"><span data-stu-id="e5b4e-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="e5b4e-118">`<OnClick>` (Klicken Sie auf das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="e5b4e-119">`<OnHoverOut>` (Maus verlässt das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="e5b4e-120">`<OnHoverOver>` (Mauszeiger über ein Steuerelement, Beenden der `<OnHoverOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="e5b4e-121">`<OnMouseOut>` (Maus verlässt ein Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="e5b4e-122">`<OnMouseOver>` (Mauszeiger über ein Steuerelement, das `<OnMouseOut>` Animation wird nicht angehalten)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="e5b4e-123">In diesem Szenario wird `<OnClick>` verwendet.</span><span class="sxs-lookup"><span data-stu-id="e5b4e-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="e5b4e-124">Wenn der Benutzer auf den Bereich klickt, wird die Größe angepasst und gleichzeitig ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="e5b4e-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

<span data-ttu-id="e5b4e-125">[![mit einem Mausklick wird die Animation gestartet.](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="e5b4e-126">Mit einem Mausklick wird die Animation gestartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e5b4e-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5b4e-127">[Zurück](picking-one-animation-out-of-a-list-vb.md)
> [Weiter](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e5b4e-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
