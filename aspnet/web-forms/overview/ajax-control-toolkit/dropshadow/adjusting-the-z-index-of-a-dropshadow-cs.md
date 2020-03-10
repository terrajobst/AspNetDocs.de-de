---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Anpassen des Z-Index eines DropShadow (C#) | Microsoft-Dokumentation
author: wenz
description: Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Dieser Schatten steht jedoch manchmal in Konflikt mit anderen Steuerelementen, für Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497481"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="72e68-104">Anpassen des Z-Index eines DropShadow-Steuerelements (C#)</span><span class="sxs-lookup"><span data-stu-id="72e68-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>

<span data-ttu-id="72e68-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="72e68-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="72e68-106">[Code herunterladen](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="72e68-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="72e68-107">Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert.</span><span class="sxs-lookup"><span data-stu-id="72e68-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="72e68-108">Dieser Schatten verursacht jedoch manchmal einen Konflikt mit anderen Steuerelementen, z. ASP.net. dem Menü Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="72e68-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="72e68-109">Wenn ein Menüeintrag angezeigt wird, wird er hinter dem Ablage Schatten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="72e68-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="72e68-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="72e68-110">Overview</span></span>

<span data-ttu-id="72e68-111">Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert.</span><span class="sxs-lookup"><span data-stu-id="72e68-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="72e68-112">Dieser Schatten verursacht jedoch manchmal einen Konflikt mit anderen Steuerelementen, z. ASP.net. dem Menü Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="72e68-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="72e68-113">Wenn ein Menüeintrag angezeigt wird, wird er hinter dem Ablage Schatten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="72e68-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="72e68-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="72e68-114">Steps</span></span>

<span data-ttu-id="72e68-115">Der Code beginnt mit dem Panel selbst und enthält ausreichend Text, sodass der Bereich ausreichend Text enthält, damit der Effekt sichtbar ist:</span><span class="sxs-lookup"><span data-stu-id="72e68-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="72e68-116">Ein anderes Panel wird direkt vor dem `panelShadow` Panel platziert.</span><span class="sxs-lookup"><span data-stu-id="72e68-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="72e68-117">Sie enthält ein Menü mit horizontaler Ausrichtung, sodass Menüeinträge über (oder eher unter) dem `dropShadow` Panel) angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="72e68-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="72e68-118">Anschließend wird der `DropShadowExtender` hinzugefügt, um den `panelShadow` Panel mit einem Schlag Schatteneffekt zu erweitern:</span><span class="sxs-lookup"><span data-stu-id="72e68-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="72e68-119">Schließlich ermöglicht das ASP.NET AJAX `ScriptManager`-Steuerelement, dass das Steuerelement-Toolkit funktioniert:</span><span class="sxs-lookup"><span data-stu-id="72e68-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="72e68-120">Wenn Sie dieses Skript ausführen, werden die Menüeinträge unterhalb des Bereichs angezeigt.</span><span class="sxs-lookup"><span data-stu-id="72e68-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="72e68-121">Das Menü verwendet jedoch die CSS-Klasse `panel`, in der Sie lediglich zwei Dinge definieren müssen, damit Elemente vor dem anderen Panel angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="72e68-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="72e68-122">Relative Positionierung</span><span class="sxs-lookup"><span data-stu-id="72e68-122">Relative positioning</span></span>
- <span data-ttu-id="72e68-123">Ein positiver z-index</span><span class="sxs-lookup"><span data-stu-id="72e68-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="72e68-124">Das `DropShadowExtender` Steuerelement kann dann nicht mehr mit dem Menü Steuerelement in Konflikt stehen.</span><span class="sxs-lookup"><span data-stu-id="72e68-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="72e68-125">[![vor: der Menüeintrag ist nicht sichtbar.](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="72e68-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="72e68-126">Vor: der Menüeintrag ist nicht sichtbar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="72e68-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>

<span data-ttu-id="72e68-127">[![nach: der Menüeintrag wird angezeigt.](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="72e68-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="72e68-128">Nach: der Menüeintrag wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png)).</span><span class="sxs-lookup"><span data-stu-id="72e68-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="72e68-129">Weiter</span><span class="sxs-lookup"><span data-stu-id="72e68-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
