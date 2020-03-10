---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Datenbindung des Schieberegler-SteuerC#Elements () | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, die aktuelle Positio zu binden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78445911"
---
# <a name="databinding-the-slider-control-c"></a><span data-ttu-id="e6d3b-104">Datenbindung des Schieberegler-Steuerelements (C#)</span><span class="sxs-lookup"><span data-stu-id="e6d3b-104">Databinding the Slider Control (C#)</span></span>

<span data-ttu-id="e6d3b-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e6d3b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e6d3b-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e6d3b-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="e6d3b-107">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="e6d3b-108">Es ist möglich, die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="e6d3b-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="e6d3b-109">Overview</span></span>

<span data-ttu-id="e6d3b-110">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="e6d3b-111">Es ist möglich, die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="e6d3b-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="e6d3b-112">Steps</span></span>

<span data-ttu-id="e6d3b-113">Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):</span><span class="sxs-lookup"><span data-stu-id="e6d3b-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="e6d3b-114">Fügen Sie dann der Seite zwei `TextBox`-Steuerelemente hinzu.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="e6d3b-115">Eine wird in einen grafischen Schieberegler transformiert, und die andere wird die Position des Schiebereglers enthalten.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="e6d3b-116">Der nächste Schritt ist bereits der letzte Schritt.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-116">The next step is already the final step.</span></span> <span data-ttu-id="e6d3b-117">Mit dem `SliderExtender`-Steuerelement aus dem ASP.NET AJAX Control Toolkit wird ein Schieberegler aus dem ersten Textfeld entfernt. das zweite Textfeld wird automatisch aktualisiert, wenn sich die Position des Schiebereglers ändert.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="e6d3b-118">Damit dies funktioniert, muss das `TargetControlID` Attribut des `SliderExtender`auf die ID des ersten Textfelds festgelegt werden. Das `BoundControlID`-Attribut muss auf die ID des zweiten Textfelds festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="e6d3b-119">Wie Sie im Browser sehen können, funktioniert die Datenbindung in beide Richtungen: Wenn Sie einen neuen Wert in das Textfeld eingeben, wird die Position des Schiebereglers aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="e6d3b-120">Wenn Sie das zweite Textfeld schreibgeschützt machen, können Sie dem Textfeld einen schwachen Schutz hinzufügen, damit der Benutzer den Wert in dort nicht manuell aktualisieren kann.</span><span class="sxs-lookup"><span data-stu-id="e6d3b-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="e6d3b-121">[![Schieberegler und Textfeld sind synchron.](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e6d3b-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="e6d3b-122">Schieberegler und Textfeld sind synchron ([Klicken Sie, um das Bild in voller Größe anzuzeigen](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e6d3b-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e6d3b-123">[Zurück](using-the-slider-control-with-auto-postback-cs.md)
> [Weiter](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e6d3b-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
