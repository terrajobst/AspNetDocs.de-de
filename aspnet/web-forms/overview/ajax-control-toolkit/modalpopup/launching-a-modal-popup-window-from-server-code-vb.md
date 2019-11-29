---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Starten eines modalen Popup Fensters über den Server Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Einige Szenarien erfordern jedoch, dass "t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606590"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="5bfb9-104">Starten eines modalen Popupfensters über den Servercode (VB)</span><span class="sxs-lookup"><span data-stu-id="5bfb9-104">Launching a Modal Popup Window from Server Code (VB)</span></span>

<span data-ttu-id="5bfb9-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5bfb9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5bfb9-106">[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5bfb9-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="5bfb9-107">Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5bfb9-108">In einigen Szenarien ist es jedoch erforderlich, dass das modale Popup auf Serverseite ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="5bfb9-109">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="5bfb9-109">Overview</span></span>

<span data-ttu-id="5bfb9-110">Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="5bfb9-111">In einigen Szenarien ist es jedoch erforderlich, dass das modale Popup auf Serverseite ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="5bfb9-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="5bfb9-112">Steps</span></span>

<span data-ttu-id="5bfb9-113">Zunächst einmal ist ein ASP.NET Button-websteuer Element erforderlich, um zu veranschaulichen, wie das ModalPopup-Steuerelement funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="5bfb9-114">Fügen Sie eine solche Schaltfläche innerhalb des &lt;Formulars&gt;-Element auf einer neuen Seite hinzu:</span><span class="sxs-lookup"><span data-stu-id="5bfb9-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="5bfb9-115">Anschließend benötigen Sie das Markup für das Popup, das Sie erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="5bfb9-116">Definieren Sie es als `<asp:Panel>` Steuerelement, und stellen Sie sicher, dass es ein Schaltflächen-Steuerelement enthält.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="5bfb9-117">Das ModalPopup-Steuerelement bietet die Funktionen, mit denen eine solche Schaltfläche das Popup schließen soll. Andernfalls gibt es keine einfache Möglichkeit, diese zu verschwinden.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="5bfb9-118">Fügen Sie anschließend der Seite das ModalPopup-Steuerelement aus dem ASP.NET AJAX-Toolkit hinzu.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="5bfb9-119">Legen Sie die Eigenschaften für die Schaltfläche fest, mit der das Steuerelement geladen wird, die Schaltfläche, die Sie entfernt, und die ID des eigentlichen Popups.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="5bfb9-120">Wie bei allen Webseiten, die auf ASP.NET AJAX basieren; der Skript-Manager muss die erforderlichen JavaScript-Bibliotheken für die verschiedenen Ziel Browser laden:</span><span class="sxs-lookup"><span data-stu-id="5bfb9-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="5bfb9-121">Führen Sie das Beispiel im Browser aus.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-121">Run the example in the browser.</span></span> <span data-ttu-id="5bfb9-122">Wenn Sie auf die Schaltfläche klicken, wird das modale Popup Fenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="5bfb9-123">Um denselben Effekt mithilfe von Server seitigem Code zu erzielen, ist eine neue Schaltfläche erforderlich:</span><span class="sxs-lookup"><span data-stu-id="5bfb9-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="5bfb9-124">Wie Sie sehen können, wird durch Klicken auf die Schaltfläche ein Postback generiert und die `ServerButton_Click()` Methode auf dem Server ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="5bfb9-125">In dieser Methode wird eine JavaScript-Funktion mit dem Namen `launchModal()` ausgeführt, um genau zu sein. die JavaScript-Funktion wird ausgeführt, nachdem die Seite geladen wurde:</span><span class="sxs-lookup"><span data-stu-id="5bfb9-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="5bfb9-126">Der Auftrag `launchModal()` ist das Anzeigen von ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="5bfb9-127">Die `launchModal()`-Funktion wird ausgeführt, nachdem die vollständige HTML-Seite geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="5bfb9-128">Zu diesem Zeitpunkt wurde das ASP.NET AJAX-Framework jedoch noch nicht vollständig geladen.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="5bfb9-129">Daher legt die `launchModal()`-Funktion nur eine Variable fest, die das ModalPopup-Steuerelement zu einem späteren Zeitpunkt anzeigen muss:</span><span class="sxs-lookup"><span data-stu-id="5bfb9-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="5bfb9-130">Die `pageLoad()` JavaScript-Funktion ist eine spezielle Funktion, die ausgeführt wird, nachdem ASP.NET AJAX vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="5bfb9-131">Daher fügen wir dieser Funktion Code hinzu, um das ModalPopup-Steuerelement anzuzeigen. Dies gilt jedoch nur, wenn `launchModal()` vor aufgerufen wurde:</span><span class="sxs-lookup"><span data-stu-id="5bfb9-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="5bfb9-132">Die `$find()`-Funktion sucht nach einem benannten Element auf der Seite und erwartet die serverseitige ID als Parameter.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="5bfb9-133">Daher gibt `$find("mpe")` die Client Darstellung des ModalPopup-Steuer Elements zurück. mit der `show()`-Methode kann das Popup Fenster angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5bfb9-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="5bfb9-134">[![das modale Popup Fenster angezeigt wird, wenn auf eine der Schaltflächen geklickt wird.](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5bfb9-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="5bfb9-135">Das modale Popup Fenster wird angezeigt, wenn auf eine der Schaltflächen geklickt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5bfb9-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5bfb9-136">[Zurück](positioning-a-modalpopup-cs.md)
> [Weiter](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5bfb9-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
