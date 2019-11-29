---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamisches Hinzufügen eines Accordion-Bereichs (VB) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Panels werden normalerweise als w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607206"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="f5880-104">Dynamisches Hinzufügen eines Accordion-Bereichs (VB)</span><span class="sxs-lookup"><span data-stu-id="f5880-104">Dynamically Adding An Accordion Pane (VB)</span></span>

<span data-ttu-id="f5880-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f5880-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f5880-106">[Code herunterladen](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f5880-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="f5880-107">Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5880-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="f5880-108">Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitiger Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="f5880-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="f5880-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="f5880-109">Overview</span></span>

<span data-ttu-id="f5880-110">Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5880-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="f5880-111">Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitiger Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="f5880-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="f5880-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="f5880-112">Steps</span></span>

<span data-ttu-id="f5880-113">Das Accordion-Steuerelement macht alle wichtigen Eigenschaften für serverseitigen Code verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f5880-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="f5880-114">Unter anderem ermöglicht die `Panes`-Eigenschaft den Zugriff auf die Sammlung von Bereichen, aus denen das Akkordeon besteht.</span><span class="sxs-lookup"><span data-stu-id="f5880-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="f5880-115">Jeder Bereich weist den Typ `AccordionPane`auf.</span><span class="sxs-lookup"><span data-stu-id="f5880-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="f5880-116">Es ist daher trivial, einen solchen Bereich zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="f5880-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="f5880-117">Die `HeaderContainer`-Eigenschaft von `AccordionPane` ermöglicht den Zugriff auf die ASP.NET-Steuerelemente innerhalb des Header Abschnitts des Bereichs. die `ContentContainer`-Eigenschaft von `AccordionPane` ist für den Inhalts Abschnitt des Bereichs identisch.</span><span class="sxs-lookup"><span data-stu-id="f5880-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="f5880-118">Dadurch kann der ASP.NET-Code den Bereichen Inhalte hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="f5880-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="f5880-119">Schließlich müssen der Bereich (n) der `Panes`-Auflistung des Zieh Fensters hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="f5880-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="f5880-120">Hier ist ein kompletter serverseitiger Code, der zwei Bereiche zu einem Accordion-Steuerelement hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="f5880-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="f5880-121">Das einzige fehlende Element ist das Akkordeon selbst, das vom vorhanden sein des ASP.net-`ScriptManager` Steuer Elements abhängig ist:</span><span class="sxs-lookup"><span data-stu-id="f5880-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="f5880-122">Um das Beispiel abzuschließen, stellen die beiden CSS-Klassen, auf die im Accordion-Steuerelement verwiesen wird, Stil Informationen für den Browser bereit:</span><span class="sxs-lookup"><span data-stu-id="f5880-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

<span data-ttu-id="f5880-123">[![die Daten im Akkordeon durch serverseitigen Code dynamisch hinzugefügt wurden.](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f5880-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="f5880-124">Die Daten im Akkordeon wurden durch serverseitigen Code dynamisch hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-adding-an-accordion-pane-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="f5880-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f5880-125">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="f5880-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
