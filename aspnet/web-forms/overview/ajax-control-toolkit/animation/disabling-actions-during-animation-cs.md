---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Deaktivieren von Aktionen während der AnimationC#() | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie unterstützt auch Aktionen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430953"
---
# <a name="disabling-actions-during-animation-c"></a><span data-ttu-id="1c83d-104">Deaktivieren von Aktionen während einer Animation (C#)</span><span class="sxs-lookup"><span data-stu-id="1c83d-104">Disabling Actions during Animation (C#)</span></span>

<span data-ttu-id="1c83d-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1c83d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1c83d-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1c83d-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="1c83d-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="1c83d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1c83d-108">Außerdem werden Aktionen wie Mausklicks unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1c83d-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="1c83d-109">Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="1c83d-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="1c83d-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1c83d-110">Overview</span></span>

<span data-ttu-id="1c83d-111">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="1c83d-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1c83d-112">Außerdem werden Aktionen wie Mausklicks unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1c83d-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="1c83d-113">Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="1c83d-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="1c83d-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="1c83d-114">Steps</span></span>

<span data-ttu-id="1c83d-115">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="1c83d-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="1c83d-116">Die Animation wird wie folgt auf eine HTML-Schaltfläche angewendet:</span><span class="sxs-lookup"><span data-stu-id="1c83d-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="1c83d-117">Beachten Sie, dass ein HTML-Steuerelement anstelle eines websteuer Elements verwendet wird, da es nicht gewünscht werden soll, dass die Schaltfläche ein Postback erstellt. die Client seitige Animation muss für uns einfach gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="1c83d-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="1c83d-118">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="1c83d-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="1c83d-119">Innerhalb des `<Animations>` Knotens ist `<OnClick>` das Rechte Element, das mit dem Maus Klick behandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1c83d-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="1c83d-120">Während der Animation kann jedoch auch auf die Schaltfläche geklickt werden.</span><span class="sxs-lookup"><span data-stu-id="1c83d-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="1c83d-121">Das `<EnableAction>`-Element kann dies berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="1c83d-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="1c83d-122">Wenn Sie `Enabled="false"` festlegen, wird die Schaltfläche als Teil der Animation deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="1c83d-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="1c83d-123">Da wir mehrere einzelne Animationen verwenden (deaktivieren Sie die Schaltfläche und die eigentlichen Animationen), ist das `<Parallel>`-Element erforderlich, um die einzelnen Animationen miteinander zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="1c83d-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="1c83d-124">Hier ist das komplette Markup für `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="1c83d-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="1c83d-125">Es ist auch möglich, die Schaltfläche nach der Animation erneut zu aktivieren, indem das folgende XML-Element am Ende der Liste verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="1c83d-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="1c83d-126">Im vorliegenden Szenario wäre dies jedoch nutzlos, da die Schaltfläche ausgeblendet wird und am Ende der Animation nicht sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="1c83d-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="1c83d-127">[![die Schaltfläche deaktiviert ist, sobald die Animation ausgeführt wird](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1c83d-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="1c83d-128">Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](disabling-actions-during-animation-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="1c83d-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c83d-129">[Zurück](animating-in-response-to-user-interaction-cs.md)
> [Weiter](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1c83d-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
