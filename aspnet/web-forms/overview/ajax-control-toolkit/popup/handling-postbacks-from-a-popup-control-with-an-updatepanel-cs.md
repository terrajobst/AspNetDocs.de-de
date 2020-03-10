---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Verarbeiten von Postbacks über ein Popup Steuerelement mit einem Update Panel (C#) | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es muss besonders darauf geachtet werden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b9e58d68b3d6c5d01ceaba6c01653e9574b541b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496665"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="aa38a-104">Verarbeiten von Postbacks über ein Popupsteuerelement mit einem UpdatePanel-Steuerelement (C#)</span><span class="sxs-lookup"><span data-stu-id="aa38a-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>

<span data-ttu-id="aa38a-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="aa38a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="aa38a-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="aa38a-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="aa38a-107">Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="aa38a-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="aa38a-108">Es muss besonders darauf geachtet werden, dass ein Postback innerhalb eines solchen Popups erfolgt.</span><span class="sxs-lookup"><span data-stu-id="aa38a-108">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="overview"></a><span data-ttu-id="aa38a-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aa38a-109">Overview</span></span>

<span data-ttu-id="aa38a-110">Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="aa38a-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="aa38a-111">Es muss besonders darauf geachtet werden, dass ein Postback innerhalb eines solchen Popups erfolgt.</span><span class="sxs-lookup"><span data-stu-id="aa38a-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="aa38a-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="aa38a-112">Steps</span></span>

<span data-ttu-id="aa38a-113">Wenn eine `PopupControl` mit einem Postback verwendet wird, kann ein `UpdatePanel` die durch das Postback verursachte Seiten Aktualisierung verhindern.</span><span class="sxs-lookup"><span data-stu-id="aa38a-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="aa38a-114">Das folgende Markup definiert einige wichtige Elemente:</span><span class="sxs-lookup"><span data-stu-id="aa38a-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="aa38a-115">Ein `ScriptManager`-Steuerelement, sodass das ASP.NET AJAX Control Toolkit funktioniert.</span><span class="sxs-lookup"><span data-stu-id="aa38a-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="aa38a-116">Zwei `TextBox` Steuerelemente, die beide ein Popup-Element auslöst</span><span class="sxs-lookup"><span data-stu-id="aa38a-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="aa38a-117">Ein `Panel` Steuerelement, das als Popup fungiert.</span><span class="sxs-lookup"><span data-stu-id="aa38a-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="aa38a-118">Innerhalb des Panels ist ein `Calendar`-Steuerelement in ein `UpdatePanel` Steuerelement eingebettet.</span><span class="sxs-lookup"><span data-stu-id="aa38a-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="aa38a-119">Zwei `PopupControlExtender` Steuerelemente, die den Bereich den Textfeldern zuweisen</span><span class="sxs-lookup"><span data-stu-id="aa38a-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="aa38a-120">Beachten Sie, dass das `OnSelectionChanged`-Attribut des `Calendar` Steuer Elements festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="aa38a-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="aa38a-121">Wenn der Benutzer also ein Datum im Kalender auswählt, erfolgt ein Postback, und die serverseitige Methode `c1_SelectionChanged()` wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="aa38a-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="aa38a-122">Innerhalb dieser Methode muss das aktuelle Datum abgerufen und in das Textfeld zurückgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="aa38a-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="aa38a-123">Die Syntax für lautet wie folgt: Zunächst muss ein Proxy Objekt für die `PopupControlExtender` auf der Seite generiert werden.</span><span class="sxs-lookup"><span data-stu-id="aa38a-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="aa38a-124">Das ASP.NET AJAX Control Toolkit bietet die `GetProxyForCurrentPopup()`-Methode.</span><span class="sxs-lookup"><span data-stu-id="aa38a-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="aa38a-125">Das von dieser Methode zurückgegebene Objekt unterstützt die `Commit()`-Methode, die einen Wert zurück an das Steuerelement sendet, das das Popup ausgelöst hat (nicht das Steuerelement, das den Methoden Befehl ausgelöst hat!).</span><span class="sxs-lookup"><span data-stu-id="aa38a-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="aa38a-126">Der folgende Code stellt das ausgewählte Datum als Argument für die `Commit()`-Methode bereit und bewirkt, dass der Code das ausgewählte Datum wieder in das Textfeld schreibt:</span><span class="sxs-lookup"><span data-stu-id="aa38a-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="aa38a-127">Wenn Sie nun auf ein Kalenderdatum klicken, wird das ausgewählte Datum im zugeordneten Textfeld angezeigt, und es wird ein Datumsauswahl-Steuerelement erstellt, das sich derzeit auf vielen Websites befindet.</span><span class="sxs-lookup"><span data-stu-id="aa38a-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>

<span data-ttu-id="aa38a-128">[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aa38a-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="aa38a-129">Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="aa38a-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="aa38a-130">[Wenn Sie ![auf ein Datum klicken, wird es in das Textfeld eingefügt.](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="aa38a-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="aa38a-131">Wenn Sie auf ein Datum klicken, wird es in das Textfeld eingefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png)).</span><span class="sxs-lookup"><span data-stu-id="aa38a-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aa38a-132">[Zurück](using-multiple-popup-controls-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="aa38a-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
