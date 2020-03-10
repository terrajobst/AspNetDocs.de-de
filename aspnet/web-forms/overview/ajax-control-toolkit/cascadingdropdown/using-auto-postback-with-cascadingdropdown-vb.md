---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Verwenden von automatischem Postback mit CascadingDropDown (VB) | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5dea23a20aba00af5109f05f18365b89e409a131
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430659"
---
# <a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="c6da5-103">Verwenden von automatischem Postback mit CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="c6da5-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>

<span data-ttu-id="c6da5-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c6da5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c6da5-105">[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c6da5-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="c6da5-106">Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden.</span><span class="sxs-lookup"><span data-stu-id="c6da5-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c6da5-107">Wenn Sie jedoch das CascadingDropDown-Steuerelement verwenden, ASP. Das AutoPostBack-Feature des DropDownList-Steuer Elements von NET funktioniert nicht, da das asynchrone Laden von Daten in die Liste ein (unnötiges) Postback selbst generiert.</span><span class="sxs-lookup"><span data-stu-id="c6da5-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c6da5-108">Mit JavaScript-Code kann dieser Effekt vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="c6da5-108">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="overview"></a><span data-ttu-id="c6da5-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c6da5-109">Overview</span></span>

<span data-ttu-id="c6da5-110">Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden.</span><span class="sxs-lookup"><span data-stu-id="c6da5-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="c6da5-111">(Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Wenn Sie jedoch das CascadingDropDown-Steuerelement verwenden, ASP. Das AutoPostBack-Feature des DropDownList-Steuer Elements von NET funktioniert nicht, da das asynchrone Laden von Daten in die Liste ein (unnötiges) Postback selbst generiert.</span><span class="sxs-lookup"><span data-stu-id="c6da5-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="c6da5-112">Mit JavaScript-Code kann dieser Effekt vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="c6da5-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="c6da5-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="c6da5-113">Steps</span></span>

<span data-ttu-id="c6da5-114">Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des &lt;`form`&gt; Element):</span><span class="sxs-lookup"><span data-stu-id="c6da5-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="c6da5-115">Anschließend ist ein Dropdown List-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="c6da5-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="c6da5-116">Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt, der Webdienst-URL und Methoden Informationen bereitstellt:</span><span class="sxs-lookup"><span data-stu-id="c6da5-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="c6da5-117">Der "CascadingDropDown"-Extender ruft anschließend asynchron einen Webdienst mit der folgenden Methoden Signatur auf:</span><span class="sxs-lookup"><span data-stu-id="c6da5-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="c6da5-118">Die-Methode gibt ein Array vom Typ CascadingDropDown-Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="c6da5-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="c6da5-119">Der Konstruktor des Typs erwartet zuerst die Beschriftung des Listen Eintrags und dann den Wert (HTML `value`-Attribut).</span><span class="sxs-lookup"><span data-stu-id="c6da5-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="c6da5-120">Wenn Sie die Seite im Browser laden, wird die Dropdown Liste mit drei Anbietern aufgefüllt, die zweite ist vorab ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="c6da5-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="c6da5-121">Außerdem definiert ASP.net die `__doPostBack()` JavaScript-Methode.</span><span class="sxs-lookup"><span data-stu-id="c6da5-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="c6da5-122">Nachdem die Seite geladen wurde, wird dieser JavaScript-Befehl der Dropdown Liste hinzugefügt, jedoch nur, wenn darin Elemente vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="c6da5-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="c6da5-123">Wenn in der Liste keine Elemente vorhanden sind, lädt das steuerungstooltoolkit Sie zurzeit, sodass der JavaScript-Code ein Timeout verwendet und in einer halben Sekunde erneut versucht wird.</span><span class="sxs-lookup"><span data-stu-id="c6da5-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="c6da5-124">Auf diese Weise wird ein Postback nur ausgeführt, wenn tatsächlich Elemente in der Liste vorhanden sind und der Benutzer einen Eintrag auswählt.</span><span class="sxs-lookup"><span data-stu-id="c6da5-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>

<span data-ttu-id="c6da5-125">[![auswählen eines Listen Elements verursacht ein Postback.](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c6da5-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="c6da5-126">Wenn Sie ein Listenelement auswählen, wird ein Postback ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c6da5-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c6da5-127">Previous</span><span class="sxs-lookup"><span data-stu-id="c6da5-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
