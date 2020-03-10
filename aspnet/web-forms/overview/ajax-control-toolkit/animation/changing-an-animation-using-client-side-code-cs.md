---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Ändern einer Animation mit Client seitigem Code (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animation kann auch...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484053"
---
# <a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="ed5a0-104">Ändern von Animationen mit clientseitigem Code (C#)</span><span class="sxs-lookup"><span data-stu-id="ed5a0-104">Changing an Animation Using Client-Side Code (C#)</span></span>

<span data-ttu-id="ed5a0-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ed5a0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ed5a0-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ed5a0-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="ed5a0-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ed5a0-108">Die Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="ed5a0-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="ed5a0-109">Overview</span></span>

<span data-ttu-id="ed5a0-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ed5a0-111">Die Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="ed5a0-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="ed5a0-112">Steps</span></span>

<span data-ttu-id="ed5a0-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="ed5a0-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="ed5a0-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="ed5a0-116">Die eigentliche Animation wird von einer HTML-Schaltfläche gestartet:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="ed5a0-117">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="ed5a0-118">Beachten Sie, dass es innerhalb des `AnimationExtender` Steuer Elements keinen `<Animations>` Knoten gibt.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="ed5a0-119">Benutzerdefinierter JavaScript-Code wird verwendet, um die Animationen bereitzustellen, die mit dem Steuerelement verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="ed5a0-120">Wie bei der Server-API von `AnimationExtender`gibt es keine einfache Möglichkeit, dem Extender eine Animation zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="ed5a0-121">Der Extender stellt jedoch mehrere Methoden zum Lesen und Schreiben von Animationen zur Verfügung, die mit den verschiedenen Ereignissen registriert sind (`OnClick`, `OnLoad`usw.).</span><span class="sxs-lookup"><span data-stu-id="ed5a0-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="ed5a0-122">Im Folgenden finden Sie einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="ed5a0-123">Das Format des Rückgabewerts der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine Objektdarstellung des XML-Markups bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="ed5a0-124">Derzeit gibt es keine Möglichkeit, ein Objekt in zu übergeben, aber es ist möglich, ein Objekt aus einer bestimmten Animation (`get_OnXXXBehavior()` Methoden) zu lesen.</span><span class="sxs-lookup"><span data-stu-id="ed5a0-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="ed5a0-125">Im folgenden finden Sie eine JSON-Zeichenfolge (ohne Anführungszeichen und formatierte Anführungszeichen), die eine von der Schaltfläche ausgelöste Animation darstellen, aber eine Animation des Panels durch Ändern der Größe und Ausblenden der Option zur gleichen Zeit ausführen:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="ed5a0-126">Der folgende JavaScript-Code weist diese JSON-Beschreibung der `OnClick` Animation des aktuellen Extenders zu und führt Sie aus:</span><span class="sxs-lookup"><span data-stu-id="ed5a0-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

<span data-ttu-id="ed5a0-127">[![die Animation sofort ausgeführt wird, ohne einen Mausklick (und mit sehr geringem Markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ed5a0-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="ed5a0-128">Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit sehr geringem Markup) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](changing-an-animation-using-client-side-code-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="ed5a0-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ed5a0-129">[Zurück](executing-animations-using-client-side-code-cs.md)
> [Weiter](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ed5a0-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
