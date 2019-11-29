---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Auswählen einer Animation aus einer Liste (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Das Framework gibt auch die...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 694416532b558291ff6ab57a442058b53d6167e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606740"
---
# <a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="b6d6e-104">Auswählen einer Animation aus einer Liste (VB)</span><span class="sxs-lookup"><span data-stu-id="b6d6e-104">Picking One Animation Out Of a List (VB)</span></span>

<span data-ttu-id="b6d6e-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b6d6e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b6d6e-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b6d6e-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="b6d6e-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b6d6e-108">Das Framework ermöglicht es dem Programmierer auch, eine Animation aus einer Liste von Animationen auszuwählen, abhängig von der Auswertung von JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="b6d6e-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="b6d6e-109">Overview</span></span>

<span data-ttu-id="b6d6e-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b6d6e-111">Das Framework ermöglicht es dem Programmierer auch, eine Animation aus einer Liste von Animationen auszuwählen, abhängig von der Auswertung von JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="b6d6e-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="b6d6e-112">Steps</span></span>

<span data-ttu-id="b6d6e-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="b6d6e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="b6d6e-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="b6d6e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="b6d6e-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="b6d6e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="b6d6e-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="b6d6e-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="b6d6e-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="b6d6e-118">Anstelle einer regulären Animation kommt das `<Case>`-Element ins Spiel.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="b6d6e-119">Der Wert seines selectScript-Attributs wird ausgewertet. der Rückgabewert muss numerisch sein.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="b6d6e-120">Abhängig von dieser Zahl wird eine der untergeordneten Elemente in &lt;Fall&gt; ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b6d6e-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="b6d6e-121">Wenn selectScript beispielsweise zu 2 ausgewertet wird, führt das steuerungstoolkit die dritte Animation innerhalb &lt;Case-&gt; aus (Zählung beginnt bei 0).</span><span class="sxs-lookup"><span data-stu-id="b6d6e-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="b6d6e-122">Das folgende Markup definiert drei untergeordnete Elemente: die Größe der Breite, die Größe der Höhe und das ausblenden. Der JavaScript-Code (`Math.floor(3 * Math.random())`) wählt dann eine Zahl zwischen 0 und 2 aus, sodass einer der drei Animationen ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="b6d6e-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]

<span data-ttu-id="b6d6e-123">[![einer der möglichen drei Animationen: der Bereich wird breiter.](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b6d6e-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="b6d6e-124">Einer der möglichen drei Animationen: der Bereich wird breiter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](picking-one-animation-out-of-a-list-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="b6d6e-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b6d6e-125">[Zurück](animation-depending-on-a-condition-vb.md)
> [Weiter](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b6d6e-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
