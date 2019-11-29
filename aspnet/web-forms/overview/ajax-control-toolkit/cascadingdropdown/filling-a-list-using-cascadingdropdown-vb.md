---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Ausfüllen einer Liste mit CascadingDropDown (VB) | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599601"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="cc468-103">Ausfüllen einer Liste mit CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="cc468-103">Filling a List Using CascadingDropDown (VB)</span></span>

<span data-ttu-id="cc468-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cc468-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cc468-105">[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="cc468-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="cc468-106">Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden.</span><span class="sxs-lookup"><span data-stu-id="cc468-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="cc468-107">(Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Die erste Herausforderung besteht darin, eine Dropdown Liste mit diesem Steuerelement auszufüllen.</span><span class="sxs-lookup"><span data-stu-id="cc468-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="cc468-108">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="cc468-108">Overview</span></span>

<span data-ttu-id="cc468-109">Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden.</span><span class="sxs-lookup"><span data-stu-id="cc468-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="cc468-110">(Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Die erste Herausforderung besteht darin, eine Dropdown Liste mit diesem Steuerelement auszufüllen.</span><span class="sxs-lookup"><span data-stu-id="cc468-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="cc468-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="cc468-111">Steps</span></span>

<span data-ttu-id="cc468-112">Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):</span><span class="sxs-lookup"><span data-stu-id="cc468-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="cc468-113">Anschließend ist ein Dropdown List-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="cc468-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="cc468-114">Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="cc468-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="cc468-115">Er sendet eine asynchrone Anforderung an einen Webdienst, der dann eine Liste von Einträgen zurückgibt, die in der Liste angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="cc468-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="cc468-116">Damit dies funktioniert, müssen die folgenden CascadingDropDown-Attribute festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="cc468-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="cc468-117">`ServicePath`: URL eines Webdiensts, der die Listeneinträge bereitgestellt</span><span class="sxs-lookup"><span data-stu-id="cc468-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="cc468-118">`ServiceMethod`: Webmethode, die die Listeneinträge bereitgestellt</span><span class="sxs-lookup"><span data-stu-id="cc468-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="cc468-119">`TargetControlID`: ID der Dropdown Liste</span><span class="sxs-lookup"><span data-stu-id="cc468-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="cc468-120">`Category`: Kategorieinformationen, die beim Aufrufen an die Webmethode übermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="cc468-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="cc468-121">`PromptText`: Text, der beim asynchronen Laden von Listen Daten vom Server angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="cc468-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="cc468-122">Hier ist das Markup für das `CascadingDropDown` Element.</span><span class="sxs-lookup"><span data-stu-id="cc468-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="cc468-123">Der einzige Unterschied C# zwischen und VB ist der Name des zugeordneten Webdiensts:</span><span class="sxs-lookup"><span data-stu-id="cc468-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="cc468-124">Der JavaScript-Code, der vom `CascadingDropDown` Extender stammt, ruft eine Webdienst Methode mit der folgenden Signatur auf:</span><span class="sxs-lookup"><span data-stu-id="cc468-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="cc468-125">Der wichtigste Aspekt ist, dass die Methode ein Array vom Typ `CascadingDropDownNameValue` zurückgeben muss (definiert durch das ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="cc468-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="cc468-126">Im `CascadingDropDownNameValue`-Konstruktor muss zuerst der Text des Listen Eintrags und dann der zugehörige Wert bereitgestellt werden, ebenso wie `<option value="VALUE">NAME</option>` in HTML.</span><span class="sxs-lookup"><span data-stu-id="cc468-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="cc468-127">Im folgenden finden Sie einige Beispiel Daten:</span><span class="sxs-lookup"><span data-stu-id="cc468-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="cc468-128">Wenn Sie die Seite im Browser laden, wird die Liste mit drei Anbietern gefüllt.</span><span class="sxs-lookup"><span data-stu-id="cc468-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="cc468-129">[![die Liste automatisch aufgefüllt wird](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cc468-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="cc468-130">Die Liste wird automatisch ausgefüllt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](filling-a-list-using-cascadingdropdown-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="cc468-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc468-131">[Zurück](using-auto-postback-with-cascadingdropdown-cs.md)
> [Weiter](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cc468-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
