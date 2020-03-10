---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Reduzieren und Erweitern eines Panels von JavaScript (C#) | Microsoft-Dokumentation
author: wenz
description: Das Reduzier Bare Panel-Steuerelement im ASP.NET AJAX Control Toolkit erweitert ein Panel und bietet ihm die Möglichkeit, seinen Inhalt zu reduzieren und zu erweitern...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497715"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="953f7-103">Reduzieren und Erweitern eines Bereichs über JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="953f7-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>

<span data-ttu-id="953f7-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="953f7-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="953f7-105">[Code herunterladen](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="953f7-105">[Download Code](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="953f7-106">Das Reduzier Bare Panel-Steuerelement im ASP.NET AJAX Control Toolkit erweitert ein Panel und bietet ihm die Möglichkeit, seinen Inhalt zu reduzieren und erneut zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="953f7-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="953f7-107">Diese beiden Aktionen können auch über benutzerdefinierten JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="953f7-107">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="953f7-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="953f7-108">Overview</span></span>

<span data-ttu-id="953f7-109">Das Reduzier Bare Panel-Steuerelement im ASP.NET AJAX Control Toolkit erweitert ein Panel und bietet ihm die Möglichkeit, seinen Inhalt zu reduzieren und erneut zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="953f7-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="953f7-110">Diese beiden Aktionen können auch über benutzerdefinierten JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="953f7-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="953f7-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="953f7-111">Steps</span></span>

<span data-ttu-id="953f7-112">Erstellen Sie zuerst eine neue ASP.NET-Seite, und fügen Sie die `ScriptManager` innerhalb des One `<form>`-Elements ein.</span><span class="sxs-lookup"><span data-stu-id="953f7-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="953f7-113">Dadurch wird die ASP.NET AJAX-Bibliothek geladen, die für das Control Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="953f7-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="953f7-114">Erstellen Sie dann einen Bereich mit Text, sodass der Reduzierungs-/Erweiterungs Effekt angezeigt werden kann:</span><span class="sxs-lookup"><span data-stu-id="953f7-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="953f7-115">Wie Sie sehen können, verweist der Bereich auf eine CSS-Klasse, die hier gezeigt wird (und definiert im Grunde eine Hintergrundfarbe und die Breite des Bereichs):</span><span class="sxs-lookup"><span data-stu-id="953f7-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="953f7-116">Das `CollapsiblePanelExtender` Steuerelement benötigt das `TargetControlID`-Attribut, damit das Toolkit weiß, welcher Bereich auf Anforderung reduziert oder erweitert werden soll:</span><span class="sxs-lookup"><span data-stu-id="953f7-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="953f7-117">Leider macht der Extender derzeit keine bestimmte API zum reduzieren oder Erweitern des Panels verfügbar, aber einige nicht dokumentierte Methoden werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="953f7-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="953f7-118">Fügen Sie zunächst der Seite drei HTML-Schaltflächen hinzu, die das Client seitige JavaScript auslöst, um den Inhalt des Bereichs zu reduzieren oder zu erweitern:</span><span class="sxs-lookup"><span data-stu-id="953f7-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="953f7-119">Im Client seitigen JavaScript-Code (gestartet mit `<script type="text/javascript">`) muss die `$find()`-Methode für den Zugriff auf die `CollapsiblePanelExtender`verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="953f7-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="953f7-120">`$find("cpe")` gibt einen Verweis darauf zurück.</span><span class="sxs-lookup"><span data-stu-id="953f7-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="953f7-121">Von dort aus lösen bestimmte Methoden die vorliegende Aufgabe aus.</span><span class="sxs-lookup"><span data-stu-id="953f7-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="953f7-122">Die Methode zum Öffnen (erweitern) des Panels wird `_doOpen()`; der folgende Code implementiert die `doOpen()`-Funktion, die aufgerufen wird, wenn auf die erste Schaltfläche geklickt wird:</span><span class="sxs-lookup"><span data-stu-id="953f7-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="953f7-123">Um das Panel zu schließen oder zu reduzieren, muss die `_doClose()` Methode ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="953f7-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="953f7-124">Wenn der Benutzer auf die zweite Schaltfläche klickt, wird der folgende JavaScript-Code aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="953f7-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="953f7-125">Die dritte Schaltfläche schaltet den Status des Panels um: von reduziert zu erweitert und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="953f7-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="953f7-126">Der `CollapsiblePanelExtender` macht die `toggle()`-Methode verfügbar, die genau diese Methode ausführt: kehrt den Zustand des Panels um.</span><span class="sxs-lookup"><span data-stu-id="953f7-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="953f7-127">Es gibt jedoch auch einen anderen Ansatz (der intern von der `toggle()`-Methode verwendet wird): die `get_Collapsed()`-Methode der `CollapsiblePanelExtender()` gibt Aufschluss darüber, ob der Bereich reduziert wird.</span><span class="sxs-lookup"><span data-stu-id="953f7-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="953f7-128">Abhängig vom Rückgabewert dieser Funktion wird der Bereich dann entweder erweitert (`_doOpen()` Methode) oder reduziert (`_doClose()`)-Methode:</span><span class="sxs-lookup"><span data-stu-id="953f7-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

<span data-ttu-id="953f7-129">[![die dritte Schaltfläche den Status des Panels ändert: von "reduziert" zu "Erweitert" und "zurück"](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="953f7-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="953f7-130">Die dritte Schaltfläche ändert den Status des Panels: von "reduziert" zu "Erweitert" und "zurück" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="953f7-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="953f7-131">Weiter</span><span class="sxs-lookup"><span data-stu-id="953f7-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
