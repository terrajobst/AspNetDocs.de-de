---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Ändern von Animationen von Serverseite (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es kann auch die Animationen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d4c786ca10e77353ac8b4746138cb1e314585ae
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59383784"
---
# <a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="7979e-104">Ändern von Animationen von Serverseite (c#)</span><span class="sxs-lookup"><span data-stu-id="7979e-104">Modifying Animations From The Server Side (C#)</span></span>

<span data-ttu-id="7979e-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7979e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7979e-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7979e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="7979e-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7979e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7979e-108">Die Animationen können auch auf der Serverseite geändert werden</span><span class="sxs-lookup"><span data-stu-id="7979e-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="7979e-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7979e-109">Overview</span></span>

<span data-ttu-id="7979e-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7979e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7979e-111">Die Animationen können auch auf der Serverseite geändert werden</span><span class="sxs-lookup"><span data-stu-id="7979e-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="7979e-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="7979e-112">Steps</span></span>

<span data-ttu-id="7979e-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="7979e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="7979e-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="7979e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="7979e-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="7979e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="7979e-116">Der Rest des Codes wird auf der Serverseite ausgeführt und verwendet keine Markup; Stattdessen wird Code zum Erstellen der `AnimationExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="7979e-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="7979e-117">Das Steuerelement-Toolkit bietet derzeit jedoch keine API-Zugriff, um die einzelnen Animationen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7979e-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="7979e-118">Es ist jedoch möglich, legen Sie die `AnimationExtender`die Animations-Eigenschaft in eine Zeichenfolge enthält, des XML-Markups verwendet werden, wenn Sie die Animationen deklarativ zuweisen.</span><span class="sxs-lookup"><span data-stu-id="7979e-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="7979e-119">Um den XML-Code zu erstellen, die nicht enthalten, muss die `<Animations>` Element können Sie die .NET Framework XML unterstützt, oder, wie im folgenden Code ein, geben Sie einfach die Zeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="7979e-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="7979e-120">Fügen Sie abschließend die `AnimationExtender` die Steuerung an die aktuelle Seite innerhalb der `<form runat="server">` -Element, um sicherzustellen, dass die Animation enthalten ist und ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="7979e-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![T<span data-ttu-id="7979e-121">IE-Animation wird mithilfe von serverseitigen erstellt C#/VB-Code]</span><span class="sxs-lookup"><span data-stu-id="7979e-121">he animation is created using server-side C#/VB code]</span></span>(modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

<span data-ttu-id="7979e-122">Die Animation wird erstellt, mit serverseitiger C#/VB-Code ([klicken Sie, um das Bild in voller Größe anzeigen](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7979e-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7979e-123">[Zurück](triggering-an-animation-in-another-control-cs.md)
> [Weiter](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7979e-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
