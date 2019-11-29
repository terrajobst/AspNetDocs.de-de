---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deaktivieren von Aktionen während der Animation (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie unterstützt auch Aktionen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606835"
---
# <a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="847c8-104">Deaktivieren von Aktionen während der Animation (VB)</span><span class="sxs-lookup"><span data-stu-id="847c8-104">Disabling Actions during Animation (VB)</span></span>

<span data-ttu-id="847c8-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="847c8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="847c8-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="847c8-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="847c8-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="847c8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="847c8-108">Außerdem werden Aktionen wie Mausklicks unterstützt.</span><span class="sxs-lookup"><span data-stu-id="847c8-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="847c8-109">Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="847c8-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="847c8-110">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="847c8-110">Overview</span></span>

<span data-ttu-id="847c8-111">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="847c8-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="847c8-112">Außerdem werden Aktionen wie Mausklicks unterstützt.</span><span class="sxs-lookup"><span data-stu-id="847c8-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="847c8-113">Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="847c8-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="847c8-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="847c8-114">Steps</span></span>

<span data-ttu-id="847c8-115">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="847c8-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="847c8-116">Die Animation wird wie folgt auf eine HTML-Schaltfläche angewendet:</span><span class="sxs-lookup"><span data-stu-id="847c8-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="847c8-117">Beachten Sie, dass ein HTML-Steuerelement anstelle eines websteuer Elements verwendet wird, da es nicht gewünscht werden soll, dass die Schaltfläche ein Postback erstellt. die Client seitige Animation muss für uns einfach gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="847c8-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="847c8-118">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="847c8-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="847c8-119">Innerhalb des `<Animations>` Knotens ist `<OnClick>` das Rechte Element, das mit dem Maus Klick behandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="847c8-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="847c8-120">Während der Animation kann jedoch auch auf die Schaltfläche geklickt werden.</span><span class="sxs-lookup"><span data-stu-id="847c8-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="847c8-121">Das `<EnableAction>`-Element kann dies berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="847c8-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="847c8-122">Wenn Sie `Enabled="false"` festlegen, wird die Schaltfläche als Teil der Animation deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="847c8-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="847c8-123">Da wir mehrere einzelne Animationen verwenden (deaktivieren Sie die Schaltfläche und die eigentlichen Animationen), ist das `<Parallel>`-Element erforderlich, um die einzelnen Animationen miteinander zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="847c8-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="847c8-124">Hier ist das komplette Markup für `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="847c8-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="847c8-125">Es ist auch möglich, die Schaltfläche nach der Animation erneut zu aktivieren, indem das folgende XML-Element am Ende der Liste verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="847c8-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="847c8-126">Im vorliegenden Szenario wäre dies jedoch nutzlos, da die Schaltfläche ausgeblendet wird und am Ende der Animation nicht sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="847c8-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="847c8-127">[![die Schaltfläche deaktiviert ist, sobald die Animation ausgeführt wird](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="847c8-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="847c8-128">Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](disabling-actions-during-animation-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="847c8-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="847c8-129">[Zurück](animating-in-response-to-user-interaction-vb.md)
> [Weiter](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="847c8-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
