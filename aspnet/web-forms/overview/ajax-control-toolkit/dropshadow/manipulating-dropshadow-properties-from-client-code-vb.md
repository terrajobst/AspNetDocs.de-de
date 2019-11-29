---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Bearbeiten von DropShadow-Eigenschaften aus Client Code (VB) | Microsoft-Dokumentation
author: wenz
description: Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Die Eigenschaften dieses Extenders können auch mithilfe des Client-JavaScrip geändert werden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574030"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="e7414-104">Bearbeiten von DropShadow-Eigenschaften über den Clientcode (VB)</span><span class="sxs-lookup"><span data-stu-id="e7414-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>

<span data-ttu-id="e7414-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e7414-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e7414-106">[Code herunterladen](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e7414-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="e7414-107">Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert.</span><span class="sxs-lookup"><span data-stu-id="e7414-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="e7414-108">Die Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Code des Clients geändert werden.</span><span class="sxs-lookup"><span data-stu-id="e7414-108">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="e7414-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="e7414-109">Overview</span></span>

<span data-ttu-id="e7414-110">Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert.</span><span class="sxs-lookup"><span data-stu-id="e7414-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="e7414-111">Die Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Code des Clients geändert werden.</span><span class="sxs-lookup"><span data-stu-id="e7414-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="e7414-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="e7414-112">Steps</span></span>

<span data-ttu-id="e7414-113">Der Code beginnt mit einem Panel, das einige Textzeilen enthält:</span><span class="sxs-lookup"><span data-stu-id="e7414-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="e7414-114">Die zugeordnete CSS-Klasse gibt dem Panel eine schöne Hintergrundfarbe:</span><span class="sxs-lookup"><span data-stu-id="e7414-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="e7414-115">Der `DropShadowExtender` wird hinzugefügt, um den Bereich mit einem Schlag Schatteneffekt zu erweitern, Deckkraft auf 50% festgelegt:</span><span class="sxs-lookup"><span data-stu-id="e7414-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="e7414-116">Mit dem ASP.NET AJAX-`ScriptManager` Steuerelement kann das Steuerelement Toolkit funktionieren:</span><span class="sxs-lookup"><span data-stu-id="e7414-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="e7414-117">Ein anderes Panel enthält zwei JavaScript-Links zum Festlegen der Deckkraft des Schlag Schattens: der minus Link verringert die Deckkraft des Schattens, der Plus Link erhöht ihn.</span><span class="sxs-lookup"><span data-stu-id="e7414-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="e7414-118">Die JavaScript-Funktion `changeOpacity()` muss dann zuerst das `DropShadowExtender`-Steuerelement auf der Seite suchen.</span><span class="sxs-lookup"><span data-stu-id="e7414-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="e7414-119">ASP.NET AJAX definiert die `$find()` Methode genau für diese Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="e7414-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="e7414-120">Anschließend ruft die `get_Opacity()`-Methode die aktuelle Deckkraft ab, die von der `set_Opacity()`-Methode festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e7414-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="e7414-121">Der JavaScript-Code legt dann den aktuellen Wert für die Deckkraft in das `<label>`-Element ein:</span><span class="sxs-lookup"><span data-stu-id="e7414-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

<span data-ttu-id="e7414-122">[![die Deckkraft auf der Clientseite geändert wird.](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e7414-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="e7414-123">Die Deckkraft wird auf der Clientseite geändert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="e7414-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e7414-124">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="e7414-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
