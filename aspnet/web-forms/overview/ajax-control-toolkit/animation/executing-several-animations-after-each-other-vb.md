---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Aufeinander folgende Ausführung mehrerer Animationen (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie ermöglicht das Ausführen von "Schweregrad"...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483981"
---
# <a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="5dcbe-104">Sequenzielles Ausführen mehrerer Animationen (VB)</span><span class="sxs-lookup"><span data-stu-id="5dcbe-104">Executing Several Animations after Each Other (VB)</span></span>

<span data-ttu-id="5dcbe-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5dcbe-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5dcbe-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5dcbe-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="5dcbe-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5dcbe-108">Dadurch können mehrere Animationen nacheinander ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-108">It allows to run several animations one after the other.</span></span>

## <a name="overview"></a><span data-ttu-id="5dcbe-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5dcbe-109">Overview</span></span>

<span data-ttu-id="5dcbe-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5dcbe-111">Dadurch können mehrere Animationen nacheinander ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="5dcbe-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="5dcbe-112">Steps</span></span>

<span data-ttu-id="5dcbe-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="5dcbe-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="5dcbe-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="5dcbe-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="5dcbe-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="5dcbe-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="5dcbe-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="5dcbe-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="5dcbe-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="5dcbe-118">Im Allgemeinen akzeptiert `<OnLoad>` nur eine Animation.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="5dcbe-119">Das Animations Framework ermöglicht es Ihnen, mehrere Animationen mit dem `<Sequence>`-Element in ein Element einzubinden.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="5dcbe-120">Alle Animationen in `<Sequence>` werden nacheinander ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="5dcbe-121">Im folgenden finden Sie ein mögliches Markup für das `AnimationExtender`-Steuerelement, wobei zunächst der Bereich breiter wird und die Höhe dann verringert wird:</span><span class="sxs-lookup"><span data-stu-id="5dcbe-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="5dcbe-122">Wenn Sie dieses Skript ausführen, wird der Panel zuerst breiter und dann kleiner.</span><span class="sxs-lookup"><span data-stu-id="5dcbe-122">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="5dcbe-123">[![zuerst wird die Breite erweitert.](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5dcbe-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="5dcbe-124">Zuerst wird die Breite erweitert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5dcbe-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>

<span data-ttu-id="5dcbe-125">[![dann wird die Höhe reduziert.](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="5dcbe-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="5dcbe-126">Die Höhe wird dann verringert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-after-each-other-vb/_static/image6.png)).</span><span class="sxs-lookup"><span data-stu-id="5dcbe-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5dcbe-127">[Zurück](executing-several-animations-at-the-same-time-vb.md)
> [Weiter](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5dcbe-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
