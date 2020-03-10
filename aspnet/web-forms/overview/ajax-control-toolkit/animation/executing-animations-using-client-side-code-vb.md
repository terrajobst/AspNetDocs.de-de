---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Ausführen von Animationen mit Client seitigem Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animations Ausführung...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484005"
---
# <a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="cb7ae-104">Ausführen von Animationen mit clientseitigem Code (VB)</span><span class="sxs-lookup"><span data-stu-id="cb7ae-104">Executing Animations Using Client-Side Code (VB)</span></span>

<span data-ttu-id="cb7ae-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cb7ae-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cb7ae-106">[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cb7ae-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="cb7ae-107">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="cb7ae-108">Die Ausführung der Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="cb7ae-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="cb7ae-109">Overview</span></span>

<span data-ttu-id="cb7ae-110">Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="cb7ae-111">Die Ausführung der Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="cb7ae-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="cb7ae-112">Steps</span></span>

<span data-ttu-id="cb7ae-113">Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="cb7ae-114">Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="cb7ae-115">Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="cb7ae-116">Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="cb7ae-117">Verwenden Sie innerhalb des Knotens `<Animations>` `<OnClick>`, um die Animationen auszuführen, sobald der Benutzer auf den Bereich klickt.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="cb7ae-118">Fügen Sie zwei Animationen hinzu, die parallel ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="cb7ae-119">Um dies zu demonstrieren, wird diese Animation (und alle anderen mit dem Steuerelement-Toolkit erstellten Animationen) mithilfe von JavaScript-Code ausgeführt, sobald die Seite ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="cb7ae-120">Zuerst benötigen wir Zugriff auf das `AnimationExtender`-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="cb7ae-121">Die ASP.NET AJAX-Bibliothek stellt die `$find()`-Funktion für diese Aufgabe bereit:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="cb7ae-122">Das `AnimationExtender`-Steuerelement stellt eine umfangreiche API bereit, einschließlich Methoden, deren Namen mit den im XML-Markup verwendeten Ereignis Handlern identisch sind: `OnClick()`, `OnLoad()`usw.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="cb7ae-123">Beispielsweise führt ein Aufrufder `OnClick()`-Methode die Animation innerhalb des `<OnClick>`-Elements des `AnimationExtender`-Steuer Elements aus:</span><span class="sxs-lookup"><span data-stu-id="cb7ae-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="cb7ae-124">Im folgenden finden Sie den vollständigen Client seitigen JavaScript-Code, der das Klicken auf den Bereich emuliert, sobald die Seite vollständig geladen wurde. Beachten Sie, dass der `pageLoad()` Funktionsname verwendet wird, der von ASP.NET AJAX aufgerufen wird, nachdem die Seite und alle enthaltenen JavaScript-Bibliotheken geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="cb7ae-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

<span data-ttu-id="cb7ae-125">[![die Animation sofort ausgeführt wird, ohne einen Mausklick](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb7ae-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="cb7ae-126">Die Animation wird sofort ohne Mausklick ausgeführt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-animations-using-client-side-code-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="cb7ae-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cb7ae-127">[Zurück](modifying-animations-from-the-server-side-vb.md)
> [Weiter](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cb7ae-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
