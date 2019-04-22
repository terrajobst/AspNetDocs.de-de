---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (VB) | Microsoft-Dokumentation
author: wenz
description: 'Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet. Es gibt jedoch ein Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 45ea3c3dbcf7816f67081a61230c4b055a90fcf5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59393625"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="d1046-104">Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (VB)</span><span class="sxs-lookup"><span data-stu-id="d1046-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="d1046-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1046-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1046-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1046-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="d1046-107">Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="d1046-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="d1046-108">Es gibt jedoch ein Nachteil: Sobald in einer Gruppe ein Optionsfeld ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="d1046-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="d1046-109">Kontrollkästchen kann jederzeit deaktiviert werden, jedoch sind nicht gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="d1046-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="d1046-110">In diesem Tutorial werden die Vorteile beider Ansätze: Kontrollkästchen, die sich gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="d1046-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="d1046-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="d1046-111">Overview</span></span>

<span data-ttu-id="d1046-112">Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="d1046-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="d1046-113">Es gibt jedoch ein Nachteil: Sobald in einer Gruppe ein Optionsfeld ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="d1046-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="d1046-114">Kontrollkästchen kann jederzeit deaktiviert werden, jedoch sind nicht gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="d1046-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="d1046-115">In diesem Tutorial werden die Vorteile beider Ansätze: Kontrollkästchen, die sich gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="d1046-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="d1046-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="d1046-116">Steps</span></span>

<span data-ttu-id="d1046-117">Das ASP.NET AJAX Control Toolkit enthält die MutuallyExclusiveCheckBox-Extender.</span><span class="sxs-lookup"><span data-stu-id="d1046-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="d1046-118">Programmierer, der Name einer Gruppe alle Kontrollkästchen zuweisen können (`Key` Attribut).</span><span class="sxs-lookup"><span data-stu-id="d1046-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="d1046-119">Aus alle Kontrollkästchen in der gleichen Gruppe kann nur eine gleichzeitig ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="d1046-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="d1046-120">Wir beginnen mit zwei Kontrollkästchen in eine neue ASP.NET-Seite zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="d1046-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="d1046-121">Es können mehrere, aber zwei reicht aus, um das Prinzip zu demonstrieren:</span><span class="sxs-lookup"><span data-stu-id="d1046-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="d1046-122">Für beide Kontrollkästchen muss ein MutuallyExclusiveCheckBoxExtender-Steuerelement auf der Seite eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="d1046-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="d1046-123">Beide Schlüsselattribute müssen den gleichen Wert wie der Wert sich Attribute eines HTML-Radio Button-Elemente befinden, müssen identisch mit die Gruppe zu bezeichnen, in der sie angehören.</span><span class="sxs-lookup"><span data-stu-id="d1046-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="d1046-124">Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens.</span><span class="sxs-lookup"><span data-stu-id="d1046-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="d1046-125">Darüber hinaus enthalten die ASP.NET AJAX `ScriptManager` die alle Elemente des ASP.NET AJAX Control Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="d1046-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="d1046-126">Speichern Sie, und führen Sie die Seite: Sie aktivieren und deaktivieren Sie beide Kontrollkästchen, können jedoch zu keinem Zeitpunkt beide Kontrollkästchen geprüft werden kann.</span><span class="sxs-lookup"><span data-stu-id="d1046-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="d1046-127">[![Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1046-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="d1046-128">Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1046-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d1046-129">Vorherige</span><span class="sxs-lookup"><span data-stu-id="d1046-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
