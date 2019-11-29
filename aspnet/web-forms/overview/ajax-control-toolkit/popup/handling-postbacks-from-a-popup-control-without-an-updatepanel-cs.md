---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Verarbeiten von Postbacks über ein Popup Steuerelement ohne Update Panel (C#) | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Wenn ein Postback in "su..." auftritt
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598732"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="e802d-104">Verarbeiten von Postbacks über ein Popupsteuerelement ohne ein UpdatePanel-Steuerelement (C#)</span><span class="sxs-lookup"><span data-stu-id="e802d-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>

<span data-ttu-id="e802d-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e802d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e802d-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e802d-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="e802d-107">Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="e802d-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="e802d-108">Wenn ein Postback in einem solchen Bereich stattfindet und mehrere Bereiche auf der Seite vorhanden sind, ist es schwierig, zu ermitteln, auf welchen Panel geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="e802d-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="e802d-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="e802d-109">Overview</span></span>

<span data-ttu-id="e802d-110">Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="e802d-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="e802d-111">Wenn ein Postback in einem solchen Bereich stattfindet und mehrere Bereiche auf der Seite vorhanden sind, ist es schwierig, zu ermitteln, auf welchen Panel geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="e802d-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="e802d-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="e802d-112">Steps</span></span>

<span data-ttu-id="e802d-113">Wenn Sie ein `PopupControl` mit einem Postback verwenden, aber keine `UpdatePanel` auf der Seite haben, bietet das Steuerelement-Toolkit keine Möglichkeit, zu bestimmen, welches Client Element das Popup ausgelöst hat, das wiederum das Postback verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="e802d-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="e802d-114">Ein kleiner Trick bietet jedoch eine Problem Umgehung für dieses Szenario.</span><span class="sxs-lookup"><span data-stu-id="e802d-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="e802d-115">Im folgenden finden Sie zunächst die grundlegende Einrichtung: zwei Textfelder, die beide das gleiche Popup, einen Kalender, auslöst.</span><span class="sxs-lookup"><span data-stu-id="e802d-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="e802d-116">Zwei `PopupControlExtenders` Textfelder und Popup zusammensetzen.</span><span class="sxs-lookup"><span data-stu-id="e802d-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="e802d-117">Die grundlegende Idee ist das Hinzufügen eines ausgeblendeten Formular Felds in der &lt;`form`&gt; Element, das das Textfeld enthält, das das Popup gestartet hat:</span><span class="sxs-lookup"><span data-stu-id="e802d-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="e802d-118">Wenn die Seite geladen wird, fügt JavaScript-Code beide Textfeldern einen Ereignishandler hinzu: Wenn der Benutzer auf ein Textfeld klickt, wird sein Name in das ausgeblendete Formularfeld geschrieben:</span><span class="sxs-lookup"><span data-stu-id="e802d-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="e802d-119">Im serverseitigen Code muss der Wert des ausgeblendeten Felds gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="e802d-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="e802d-120">Da verborgene Formularfelder zu bearbeiten sind, ist ein Whitelist-Ansatz erforderlich, um den verborgenen Wert zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="e802d-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="e802d-121">Nachdem das richtige Textfeld identifiziert wurde, wird das Datum aus dem Kalender in das Textfeld geschrieben.</span><span class="sxs-lookup"><span data-stu-id="e802d-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

<span data-ttu-id="e802d-122">[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e802d-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="e802d-123">Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="e802d-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="e802d-124">[Wenn Sie ![auf ein Datum klicken, wird es in das Textfeld eingefügt.](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e802d-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="e802d-125">Wenn Sie auf ein Datum klicken, wird es in das Textfeld eingefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png)).</span><span class="sxs-lookup"><span data-stu-id="e802d-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e802d-126">[Zurück](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Weiter](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e802d-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
