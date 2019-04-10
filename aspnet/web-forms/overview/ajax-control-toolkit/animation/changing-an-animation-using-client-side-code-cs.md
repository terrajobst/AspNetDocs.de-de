---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Ändern von Animationen mit clientseitigen Code (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es können auch die Animation...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bdee58aa04e1c8217c2a727b96aa8b239fe1aca
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395602"
---
# <a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="b2a94-104">Ändern von Animationen mit clientseitigem Code (C#)</span><span class="sxs-lookup"><span data-stu-id="b2a94-104">Changing an Animation Using Client-Side Code (C#)</span></span>

<span data-ttu-id="b2a94-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b2a94-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b2a94-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b2a94-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="b2a94-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2a94-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b2a94-108">Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="b2a94-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="b2a94-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b2a94-109">Overview</span></span>

<span data-ttu-id="b2a94-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2a94-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b2a94-111">Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="b2a94-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="b2a94-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="b2a94-112">Steps</span></span>

<span data-ttu-id="b2a94-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="b2a94-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="b2a94-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="b2a94-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="b2a94-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="b2a94-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="b2a94-116">Die tatsächliche Animation wird durch eine HTML-Schaltfläche gestartet:</span><span class="sxs-lookup"><span data-stu-id="b2a94-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="b2a94-117">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="b2a94-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="b2a94-118">Beachten Sie, dass es keine `<Animations>` Knoten innerhalb der `AnimationExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="b2a94-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="b2a94-119">Benutzerdefinierte JavaScript-Code wird verwendet, geben Sie die Animationen, die mit dem Steuerelement verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b2a94-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="b2a94-120">Wie bei der Server-API von `AnimationExtender`, es gibt keine einfache Möglichkeit, eine Animation für den Extender noch nicht zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="b2a94-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="b2a94-121">Mit den verschiedenen Ereignissen registriert jedoch der Extender zur Verfügung stellt mehrere Methoden zum Lesen und Schreiben von Animationen (`OnClick`, `OnLoad`und so weiter).</span><span class="sxs-lookup"><span data-stu-id="b2a94-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="b2a94-122">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="b2a94-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="b2a94-123">Das Format des Rückgabewerts von der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine objektdarstellung, der das XML-Markup bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b2a94-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="b2a94-124">Derzeit besteht keine Möglichkeit, ein Objekt übergeben, aber es ist möglich, ein Objekt aus einer angegebenen Animation lesen (`get_OnXXXBehavior()` Methoden).</span><span class="sxs-lookup"><span data-stu-id="b2a94-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="b2a94-125">Hier ist eine JSON-Zeichenfolge (ohne die begrenzenden Anführungszeichen und ordentlich), die eine Animation, die von der Schaltfläche ausgelöste darstellt, aber die Animation des Bereichs durch Ändern der Größe und gleichzeitig ausblenden:</span><span class="sxs-lookup"><span data-stu-id="b2a94-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="b2a94-126">Die folgende JavaScript-Code weist diese JSON-descripting auf die `OnClick` Animation des aktuellen Extenders und führt ihn:</span><span class="sxs-lookup"><span data-stu-id="b2a94-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![T<span data-ttu-id="b2a94-127">IE-Animation sofort ausgeführt, ohne ein Mausklick (und mit sehr geringem Markup)]</span><span class="sxs-lookup"><span data-stu-id="b2a94-127">he animation runs immediately, without a mouse click (and with very little markup)]</span></span>(changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

<span data-ttu-id="b2a94-128">Die Animation wird sofort ausgeführt, ohne ein Mausklick (und mit sehr geringem Markup) ([klicken Sie, um das Bild in voller Größe anzeigen](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b2a94-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2a94-129">[Zurück](executing-animations-using-client-side-code-cs.md)
> [Weiter](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b2a94-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
