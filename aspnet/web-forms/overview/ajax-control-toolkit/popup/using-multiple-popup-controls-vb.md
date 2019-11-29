---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Verwenden von mehreren Popup Steuerelementen (VB) | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611604"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="2191b-104">Verwenden von mehreren Popupsteuerelementen (VB)</span><span class="sxs-lookup"><span data-stu-id="2191b-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="2191b-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2191b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2191b-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2191b-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="2191b-107">Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="2191b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="2191b-108">Es ist auch möglich, mehr als ein Popup-Steuerelement auf einer Seite zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2191b-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="2191b-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="2191b-109">Overview</span></span>

<span data-ttu-id="2191b-110">Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="2191b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="2191b-111">Es ist auch möglich, mehr als ein Popup-Steuerelement auf einer Seite zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2191b-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="2191b-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="2191b-112">Steps</span></span>

<span data-ttu-id="2191b-113">Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):</span><span class="sxs-lookup"><span data-stu-id="2191b-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="2191b-114">Fügen Sie als nächstes ein Panel hinzu, das als Popup fungiert.</span><span class="sxs-lookup"><span data-stu-id="2191b-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="2191b-115">Im aktuellen Szenario enthält der Bereich ein `Calendar` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="2191b-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="2191b-116">Um die Seiten Aktualisierungen zu vermeiden, die durch die Postbacks des Kalenders verursacht werden, wird der Bereich in einem `UpdatePanel` Steuerelement abgelegt:</span><span class="sxs-lookup"><span data-stu-id="2191b-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="2191b-117">Die Seite enthält auch zwei Textfelder.</span><span class="sxs-lookup"><span data-stu-id="2191b-117">The page also contains two text boxes.</span></span> <span data-ttu-id="2191b-118">Für jedes Textfeld wird das Kalender Popup angezeigt, sobald das Textfeld aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="2191b-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="2191b-119">Erweitern Sie nun die beiden Textfelder mit einem `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="2191b-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="2191b-120">Das `TargetControlID`-Attribut stellt die ID des Steuer Elements bereit, das an den Extender gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="2191b-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="2191b-121">Das `PopupControlID`-Attribut enthält die ID des Popup Panels.</span><span class="sxs-lookup"><span data-stu-id="2191b-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="2191b-122">In diesem Fall zeigen beide Extender denselben Bereich an, aber auch unterschiedliche Bereiche sind möglich.</span><span class="sxs-lookup"><span data-stu-id="2191b-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="2191b-123">Wenn Sie nun in ein Textfeld klicken, wird unter dem Feld ein Kalender angezeigt, in dem Sie ein Datum auswählen können.</span><span class="sxs-lookup"><span data-stu-id="2191b-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="2191b-124">(Das ausgewählte Datum wieder in die Textfelder wird in einem anderen Tutorial behandelt.)</span><span class="sxs-lookup"><span data-stu-id="2191b-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="2191b-125">[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2191b-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="2191b-126">Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-multiple-popup-controls-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="2191b-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2191b-127">[Zurück](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2191b-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
