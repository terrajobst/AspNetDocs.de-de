---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Positionieren eines ModalPopup-C#() | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Das Steuerelement bietet jedoch keine...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496917"
---
# <a name="positioning-a-modalpopup-c"></a><span data-ttu-id="94168-104">Positionieren eines ModalPopup-Steuerelements (C#)</span><span class="sxs-lookup"><span data-stu-id="94168-104">Positioning a ModalPopup (C#)</span></span>

<span data-ttu-id="94168-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="94168-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="94168-106">[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="94168-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="94168-107">Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel.</span><span class="sxs-lookup"><span data-stu-id="94168-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="94168-108">Das-Steuerelement bietet jedoch keine integrierte Funktionalität zum Positionieren des Popups.</span><span class="sxs-lookup"><span data-stu-id="94168-108">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="94168-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="94168-109">Overview</span></span>

<span data-ttu-id="94168-110">Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel.</span><span class="sxs-lookup"><span data-stu-id="94168-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="94168-111">Das-Steuerelement bietet jedoch keine integrierte Funktionalität zum Positionieren des Popups.</span><span class="sxs-lookup"><span data-stu-id="94168-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="94168-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="94168-112">Steps</span></span>

<span data-ttu-id="94168-113">Um die Funktionalität von ASP.NET AJAX und dem Control Toolkit zu aktivieren, wird der `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="94168-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="94168-114">das Steuerelement muss an beliebiger Stelle auf der Seite abgelegt werden (innerhalb des `<form>` Elements):</span><span class="sxs-lookup"><span data-stu-id="94168-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="94168-115">Fügen Sie als nächstes ein Panel hinzu, das als modales Popup fungiert.</span><span class="sxs-lookup"><span data-stu-id="94168-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="94168-116">Zum Schließen des Popups wird eine Schaltfläche verwendet:</span><span class="sxs-lookup"><span data-stu-id="94168-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="94168-117">Wenn das Popup Fenster angezeigt wird, muss es an einer bestimmten Stelle auf der Seite positioniert werden.</span><span class="sxs-lookup"><span data-stu-id="94168-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="94168-118">Für diese Aufgabe wird eine Client seitige JavaScript-Funktion erstellt.</span><span class="sxs-lookup"><span data-stu-id="94168-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="94168-119">Zuerst wird versucht, auf den Bereich zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="94168-119">It first tries to access the panel.</span></span> <span data-ttu-id="94168-120">Wenn der Vorgang erfolgreich ist, wird die Position des Panels mithilfe von CSS und JavaScript festgelegt (ändern Sie die Position des Popups bei wird).</span><span class="sxs-lookup"><span data-stu-id="94168-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="94168-121">Das `ModalPopupExtender`-Steuerelement versucht jedoch auch, das Popup Fenster zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="94168-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="94168-122">Aus diesem Grund positioniert der JavaScript-Code jedes Zehntel einer Sekunde wiederholt das Popup.</span><span class="sxs-lookup"><span data-stu-id="94168-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="94168-123">Wie Sie sehen, wird der Rückgabewert der JavaScript-Methode `setTimeout()` in einer globalen Variablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="94168-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="94168-124">Dadurch kann die wiederholte Positionierung des Popup Bedarfs bei Bedarf mit der `clearTimeout()`-Methode beendet werden:</span><span class="sxs-lookup"><span data-stu-id="94168-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="94168-125">Nun müssen Sie nur noch den Browser zum Abrufen dieser Funktionen verwenden, wenn dies angemessen ist.</span><span class="sxs-lookup"><span data-stu-id="94168-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="94168-126">Die `movePanel()` JavaScript-Funktion muss aufgerufen werden, wenn auf die Schaltfläche geklickt wird, mit der das Panel ausgelöst wird:</span><span class="sxs-lookup"><span data-stu-id="94168-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="94168-127">Wenn das Popup Fenster geschlossen wird, wird die `stopMoving()` Funktion wiedergegeben, die im `ModalPopupExtender` Steuerelement ausgelöst werden kann:</span><span class="sxs-lookup"><span data-stu-id="94168-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

<span data-ttu-id="94168-128">[![das modale Popup an der angegebenen Position angezeigt wird.](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="94168-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="94168-129">Das modale Popup Fenster wird an der angegebenen Position angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](positioning-a-modalpopup-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="94168-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="94168-130">[Zurück](handling-postbacks-from-a-modalpopup-cs.md)
> [Weiter](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="94168-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
