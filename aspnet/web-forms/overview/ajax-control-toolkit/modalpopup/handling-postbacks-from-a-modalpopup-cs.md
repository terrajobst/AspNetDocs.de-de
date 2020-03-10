---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Verarbeiten von Postbacks über ein ModalPopupC#-Attribut () | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Es muss besonders darauf geachtet werden, wenn ein POS...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446205"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="a2836-104">Verarbeiten von Postbacks über ein ModalPopup-Steuerelement (C#)</span><span class="sxs-lookup"><span data-stu-id="a2836-104">Handling Postbacks from a ModalPopup (C#)</span></span>

<span data-ttu-id="a2836-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a2836-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a2836-106">[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a2836-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="a2836-107">Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel.</span><span class="sxs-lookup"><span data-stu-id="a2836-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="a2836-108">Es muss besonders darauf geachtet werden, wenn ein Postback aus dem Popup erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="a2836-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="a2836-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a2836-109">Overview</span></span>

<span data-ttu-id="a2836-110">Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel.</span><span class="sxs-lookup"><span data-stu-id="a2836-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="a2836-111">Es muss besonders darauf geachtet werden, wenn ein Postback aus dem Popup erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="a2836-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="a2836-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="a2836-112">Steps</span></span>

<span data-ttu-id="a2836-113">Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):</span><span class="sxs-lookup"><span data-stu-id="a2836-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="a2836-114">Fügen Sie als nächstes ein Panel hinzu, das als modales Popup fungiert.</span><span class="sxs-lookup"><span data-stu-id="a2836-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="a2836-115">Dort kann der Benutzer einen Namen und eine e-Mail-Adresse eingeben.</span><span class="sxs-lookup"><span data-stu-id="a2836-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="a2836-116">Eine Schaltfläche wird verwendet, um das Popup Fenster zu schließen und die Informationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a2836-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="a2836-117">Beachten Sie, dass das `OnClick`-Attribut festgelegt ist, sodass ein Postback auftritt, wenn auf diese Schaltfläche geklickt wird:</span><span class="sxs-lookup"><span data-stu-id="a2836-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="a2836-118">Die Seite selbst besteht aus zwei Bezeichnungen für genau dieselben Informationen: Name und e-Mail-Adresse.</span><span class="sxs-lookup"><span data-stu-id="a2836-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="a2836-119">Eine Schaltfläche wird verwendet, um das modale Popup zu starten:</span><span class="sxs-lookup"><span data-stu-id="a2836-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="a2836-120">Fügen Sie das `ModalPopupExtender`-Steuerelement hinzu, damit das Popup angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a2836-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="a2836-121">Legen Sie das `PopupControlID`-Attribut auf die ID des Panels fest, und `TargetControlID` auf die ID der Schaltfläche:</span><span class="sxs-lookup"><span data-stu-id="a2836-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="a2836-122">Wenn nun auf die Schaltfläche "`Save`" innerhalb des modalen Popups geklickt wird, wird die serverseitige `SaveData()` Methode ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a2836-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="a2836-123">Dort können Sie die eingegebenen Daten in einem Datenspeicher speichern.</span><span class="sxs-lookup"><span data-stu-id="a2836-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="a2836-124">Der Einfachheit halber werden die neuen Daten nur in der Bezeichnung ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="a2836-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="a2836-125">Außerdem sollten die Textfeld-Steuerelemente im modalen Popup mit dem aktuellen Namen und der aktuellen e-Mail-Adresse ausgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="a2836-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="a2836-126">Dies ist jedoch nur erforderlich, wenn kein Postback stattfindet.</span><span class="sxs-lookup"><span data-stu-id="a2836-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="a2836-127">Wenn ein Postback vorliegt, werden die Textfelder von der ASP.NET ViewState-Funktion automatisch mit den entsprechenden Werten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="a2836-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

<span data-ttu-id="a2836-128">[![das modale Popup ein Postback verursacht.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a2836-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="a2836-129">Das modale Popup bewirkt ein Postback ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-modalpopup-cs/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="a2836-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2836-130">[Zurück](using-modalpopup-with-a-repeater-control-cs.md)
> [Weiter](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a2836-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
