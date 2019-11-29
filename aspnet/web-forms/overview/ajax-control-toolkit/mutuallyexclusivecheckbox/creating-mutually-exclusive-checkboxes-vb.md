---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Erstellen von gegenseitig ausschließenden Kontrollkästchen (VB) | Microsoft-Dokumentation
author: wenz
description: 'Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet. Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606459"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="f1f34-104">Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (VB)</span><span class="sxs-lookup"><span data-stu-id="f1f34-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="f1f34-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f1f34-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f1f34-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f1f34-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="f1f34-107">Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet.</span><span class="sxs-lookup"><span data-stu-id="f1f34-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f1f34-108">Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Options Felder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="f1f34-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f1f34-109">Kontrollkästchen können jederzeit deaktiviert werden, schließen sich jedoch nicht gegenseitig aus.</span><span class="sxs-lookup"><span data-stu-id="f1f34-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f1f34-110">Dieses Tutorial bietet das Beste aus beiden Ansätzen: Kontrollkästchen, die sich gegenseitig ausschließen.</span><span class="sxs-lookup"><span data-stu-id="f1f34-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="f1f34-111">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="f1f34-111">Overview</span></span>

<span data-ttu-id="f1f34-112">Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet.</span><span class="sxs-lookup"><span data-stu-id="f1f34-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f1f34-113">Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Options Felder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="f1f34-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f1f34-114">Kontrollkästchen können jederzeit deaktiviert werden, schließen sich jedoch nicht gegenseitig aus.</span><span class="sxs-lookup"><span data-stu-id="f1f34-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f1f34-115">Dieses Tutorial bietet das Beste aus beiden Ansätzen: Kontrollkästchen, die sich gegenseitig ausschließen.</span><span class="sxs-lookup"><span data-stu-id="f1f34-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="f1f34-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="f1f34-116">Steps</span></span>

<span data-ttu-id="f1f34-117">Das ASP.NET AJAX Control Toolkit enthält den MutuallyExclusiveCheckbox-Extender.</span><span class="sxs-lookup"><span data-stu-id="f1f34-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="f1f34-118">Dies ermöglicht es Programmierern, einem Gruppennamen (`Key` Attribut) ein beliebiges Kontrollkästchen zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="f1f34-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="f1f34-119">Aus allen Kontrollkästchen innerhalb derselben Gruppe kann jeweils nur eine ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="f1f34-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="f1f34-120">Beginnen wir damit, zwei Kontrollkästchen auf einer neuen ASP.NET-Seite zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="f1f34-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="f1f34-121">Es können mehr, aber zwei davon ausreichen, um das Prinzip zu veranschaulichen:</span><span class="sxs-lookup"><span data-stu-id="f1f34-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="f1f34-122">Für beide Kontrollkästchen muss ein mutuallyexclusivecheckboxextender-Steuerelement auf der Seite abgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="f1f34-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="f1f34-123">Beide Schlüssel Attribute müssen denselben Wert aufweisen, ebenso wie die Wert Attribute von HTML-Optionsfeld Elementen identisch sein müssen, um die Gruppe anzugeben, zu der Sie gehören.</span><span class="sxs-lookup"><span data-stu-id="f1f34-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="f1f34-124">Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens.</span><span class="sxs-lookup"><span data-stu-id="f1f34-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="f1f34-125">Schließen Sie schließlich den ASP.NET AJAX-`ScriptManager` ein, der für alle Elemente des ASP.NET AJAX Control Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="f1f34-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="f1f34-126">Speichern Sie die Seite, und führen Sie Sie aus: Sie können beide Kontrollkästchen aktivieren und deaktivieren. es können jedoch nicht beide Kontrollkästchen aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="f1f34-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="f1f34-127">[![jeweils nur ein Kontrollkästchen aktiviert werden kann.](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1f34-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="f1f34-128">Es kann jeweils nur ein Kontrollkästchen geprüft werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-mutually-exclusive-checkboxes-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="f1f34-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f1f34-129">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="f1f34-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
