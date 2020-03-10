---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (C#) | Microsoft-Dokumentation
author: wenz
description: 'Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet. Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496785"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="eeb1d-104">Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (C#)</span><span class="sxs-lookup"><span data-stu-id="eeb1d-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>

<span data-ttu-id="eeb1d-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eeb1d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eeb1d-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="eeb1d-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="eeb1d-107">Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="eeb1d-108">Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Options Felder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="eeb1d-109">Kontrollkästchen können jederzeit deaktiviert werden, schließen sich jedoch nicht gegenseitig aus.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="eeb1d-110">Dieses Tutorial bietet das Beste aus beiden Ansätzen: Kontrollkästchen, die sich gegenseitig ausschließen.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="eeb1d-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="eeb1d-111">Overview</span></span>

<span data-ttu-id="eeb1d-112">Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="eeb1d-113">Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Options Felder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="eeb1d-114">Kontrollkästchen können jederzeit deaktiviert werden, schließen sich jedoch nicht gegenseitig aus.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="eeb1d-115">Dieses Tutorial bietet das Beste aus beiden Ansätzen: Kontrollkästchen, die sich gegenseitig ausschließen.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="eeb1d-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="eeb1d-116">Steps</span></span>

<span data-ttu-id="eeb1d-117">Das ASP.NET AJAX Control Toolkit enthält den MutuallyExclusiveCheckbox-Extender.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="eeb1d-118">Dies ermöglicht es Programmierern, einem Gruppennamen (`Key` Attribut) ein beliebiges Kontrollkästchen zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="eeb1d-119">Aus allen Kontrollkästchen innerhalb derselben Gruppe kann jeweils nur eine ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="eeb1d-120">Beginnen wir damit, zwei Kontrollkästchen auf einer neuen ASP.NET-Seite zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="eeb1d-121">Es können mehr, aber zwei davon ausreichen, um das Prinzip zu veranschaulichen:</span><span class="sxs-lookup"><span data-stu-id="eeb1d-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="eeb1d-122">Für beide Kontrollkästchen muss ein mutuallyexclusivecheckboxextender-Steuerelement auf der Seite abgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="eeb1d-123">Beide Schlüssel Attribute müssen denselben Wert aufweisen, ebenso wie die Wert Attribute von HTML-Optionsfeld Elementen identisch sein müssen, um die Gruppe anzugeben, zu der Sie gehören.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="eeb1d-124">Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="eeb1d-125">Schließen Sie schließlich den ASP.NET AJAX-`ScriptManager` ein, der für alle Elemente des ASP.NET AJAX Control Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="eeb1d-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="eeb1d-126">Speichern Sie die Seite, und führen Sie Sie aus: Sie können beide Kontrollkästchen aktivieren und deaktivieren. es können jedoch nicht beide Kontrollkästchen aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="eeb1d-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="eeb1d-127">[![jeweils nur ein Kontrollkästchen aktiviert werden kann.](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eeb1d-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="eeb1d-128">Es kann jeweils nur ein Kontrollkästchen geprüft werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-mutually-exclusive-checkboxes-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="eeb1d-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="eeb1d-129">Weiter</span><span class="sxs-lookup"><span data-stu-id="eeb1d-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
