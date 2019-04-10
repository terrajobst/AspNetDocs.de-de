---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Verarbeiten von Postbacks über ein Popupsteuerelement mit einem UpdatePanel-Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Besondere Sorgfalt verfügt, die ausgeführt werden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: dff813f0f3e4da26a32fd6305e476d24484e0e7c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59415088"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="08f67-104">Verarbeiten von Postbacks über ein Popupsteuerelement mit einem UpdatePanel-Steuerelement (C#)</span><span class="sxs-lookup"><span data-stu-id="08f67-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>

<span data-ttu-id="08f67-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="08f67-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="08f67-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="08f67-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="08f67-107">Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="08f67-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="08f67-108">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.</span><span class="sxs-lookup"><span data-stu-id="08f67-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="08f67-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="08f67-109">Overview</span></span>

<span data-ttu-id="08f67-110">Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="08f67-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="08f67-111">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.</span><span class="sxs-lookup"><span data-stu-id="08f67-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="08f67-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="08f67-112">Steps</span></span>

<span data-ttu-id="08f67-113">Bei Verwendung einer `PopupControl` mit einem Postback, ein `UpdatePanel` können verhindern, dass die seitenaktualisierung, die durch das Postback verursacht.</span><span class="sxs-lookup"><span data-stu-id="08f67-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="08f67-114">Das folgende Markup definiert ein paar wichtige Elemente:</span><span class="sxs-lookup"><span data-stu-id="08f67-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="08f67-115">Ein `ScriptManager` steuern, das ASP.NET AJAX Control Toolkit funktioniert</span><span class="sxs-lookup"><span data-stu-id="08f67-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="08f67-116">Zwei `TextBox` steuert, welche sowohl ein Popup ausgelöst werden</span><span class="sxs-lookup"><span data-stu-id="08f67-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="08f67-117">Ein `Panel` -Steuerelement, das als das Popup verwendet wird</span><span class="sxs-lookup"><span data-stu-id="08f67-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="08f67-118">Innerhalb des Bereichs eine `Calendar` eingebettetes Steuerelement ist in einer `UpdatePanel` Steuerelement</span><span class="sxs-lookup"><span data-stu-id="08f67-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="08f67-119">Zwei `PopupControlExtender` Steuerelemente, die den Inhalt der Textfelder im Bereich zuweisen</span><span class="sxs-lookup"><span data-stu-id="08f67-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="08f67-120">Beachten Sie, dass die `OnSelectionChanged` Attribut der `Calendar` -Steuerelement so eingestellt ist.</span><span class="sxs-lookup"><span data-stu-id="08f67-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="08f67-121">Daher ein Postback tritt auf, wenn der Benutzer ein Datum innerhalb des Kalenders auswählt, und die serverseitige Methode `c1_SelectionChanged()` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="08f67-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="08f67-122">In dieser Methode muss das aktuelle Datum abgerufen und in das Textfeld zurückgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="08f67-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="08f67-123">Die Syntax dafür lautet wie folgt aus: -Objekt als erstes einen Proxy für die `PopupControlExtender` auf der Seite generiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="08f67-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="08f67-124">Das ASP.NET AJAX Control Toolkit bietet die `GetProxyForCurrentPopup()` Methode.</span><span class="sxs-lookup"><span data-stu-id="08f67-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="08f67-125">Das Objekt, das Beenden dieser Methode unterstützt die `Commit()` Methode, der einen Wert an das Steuerelement gesendet, der das Popup (nicht das Steuerelement, das den Methodenaufruf ausgelöst!) ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="08f67-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="08f67-126">Der folgende Code stellt das ausgewählte Datum als Argument für die `Commit()` -Methode, sodass des Codes, die das ausgewählte Datum in das Textfeld schreiben:</span><span class="sxs-lookup"><span data-stu-id="08f67-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="08f67-127">Jetzt, wenn Sie in einem Kalenderdatum klicken Sie auf das ausgewählte Datum angezeigt wird, in der zugeordneten Textfeld ein Datumsauswahl-Steuerelement zu erstellen, die derzeit auf vielen Websites finden.</span><span class="sxs-lookup"><span data-stu-id="08f67-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


[![T<span data-ttu-id="08f67-128">er Kalender wird angezeigt, in das Textfeld klickt der Benutzer]</span><span class="sxs-lookup"><span data-stu-id="08f67-128">he Calendar appears when the user clicks into the textbox]</span></span>(handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

<span data-ttu-id="08f67-129">Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="08f67-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


[![C<span data-ttu-id="08f67-130">Licking an einem Datum in das Textfeld eingefügt]</span><span class="sxs-lookup"><span data-stu-id="08f67-130">licking on a date puts it in the textbox]</span></span>(handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

<span data-ttu-id="08f67-131">Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="08f67-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="08f67-132">[Zurück](using-multiple-popup-controls-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="08f67-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
