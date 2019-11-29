---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Verwenden des Schieberegler-Steuer Elements mit automatischem Postback (VB) | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, den Schieberegler wiederherzustellen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: e7a3286bcf7ca844f5dcfa4848c15e0bd4767c0f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598562"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="da57f-104">Verwenden des Schieberegler-Steuer Elements mit automatischem Postback (VB)</span><span class="sxs-lookup"><span data-stu-id="da57f-104">Using the Slider Control With Auto-Postback (VB)</span></span>

<span data-ttu-id="da57f-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="da57f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="da57f-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="da57f-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="da57f-107">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="da57f-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="da57f-108">Es ist möglich, den Schieberegler Auto Post Back vorzunehmen, sobald sich der Wert ändert.</span><span class="sxs-lookup"><span data-stu-id="da57f-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="da57f-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="da57f-109">Overview</span></span>

<span data-ttu-id="da57f-110">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="da57f-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="da57f-111">Es ist möglich, den Schieberegler Auto Post Back vorzunehmen, sobald sich der Wert ändert.</span><span class="sxs-lookup"><span data-stu-id="da57f-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="da57f-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="da57f-112">Steps</span></span>

<span data-ttu-id="da57f-113">Damit der Schieberegler bei einer Änderung automatisch Postback wird, benötigen beide Textfelder das-Attribut `AutoPostBack="true"`: das Textfeld, das zum Schieberegler wird, und das Textfeld, das die Position des Schiebereglers enthält.</span><span class="sxs-lookup"><span data-stu-id="da57f-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="da57f-114">Dies ist das erforderliche Markup für dieses:</span><span class="sxs-lookup"><span data-stu-id="da57f-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="da57f-115">Das `SliderExtender`-Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit weist den beiden Textfeldern die Schieberegler-Funktionalität zu:</span><span class="sxs-lookup"><span data-stu-id="da57f-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="da57f-116">Ein zusätzliches Label-Element wird später verwendet, um den Benutzer über ein Postback zu informieren:</span><span class="sxs-lookup"><span data-stu-id="da57f-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="da57f-117">Zum Schluss lädt das `ScriptManager`-Steuerelement von ASP.NET AJAX das erforderliche JavaScript, damit das Steuerelement-Toolkit funktioniert:</span><span class="sxs-lookup"><span data-stu-id="da57f-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="da57f-118">Der Schieberegler wird jetzt zurück gepostet. auf der Serverseite kann dieses Ereignis abgefangen und befolgt werden:</span><span class="sxs-lookup"><span data-stu-id="da57f-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]

<span data-ttu-id="da57f-119">[![Verschieben des Schiebereglers löst ein Postback aus.](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="da57f-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="da57f-120">Durch Verschieben des Schiebereglers wird ein Postback ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="da57f-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>

<span data-ttu-id="da57f-121">[![anschließend wird das Datum dieser Änderung in der Bezeichnung geschrieben.](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="da57f-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="da57f-122">Anschließend wird das Datum dieser Änderung in der Bezeichnung geschrieben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-slider-control-with-auto-postback-vb/_static/image6.png)).</span><span class="sxs-lookup"><span data-stu-id="da57f-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="da57f-123">[Zurück](databinding-the-slider-control-cs.md)
> [Weiter](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="da57f-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
