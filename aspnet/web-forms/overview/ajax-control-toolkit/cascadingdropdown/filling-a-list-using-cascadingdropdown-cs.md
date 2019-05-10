---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Ausfüllen einer Liste mit CascadingDropDown (c#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 31319e0ad15825acead2b7e8b619985272fb8eaa
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131458"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="bceea-103">Ausfüllen einer Liste mit CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="bceea-103">Filling a List Using CascadingDropDown (C#)</span></span>

<span data-ttu-id="bceea-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bceea-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bceea-105">[Code herunterladen](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bceea-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="bceea-106">Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="bceea-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="bceea-107">(Z. B. eine Liste enthält eine Liste der US-Staaten und die folgenden Liste wird dann mit der größten Städte in diesem Zustand gefüllt.) Die erste Herausforderung lösen werden tatsächlich eine Dropdownliste mit diesem Steuerelement geben.</span><span class="sxs-lookup"><span data-stu-id="bceea-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="bceea-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bceea-108">Overview</span></span>

<span data-ttu-id="bceea-109">Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="bceea-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="bceea-110">(Z. B. eine Liste enthält eine Liste der US-Staaten und die folgenden Liste wird dann mit der größten Städte in diesem Zustand gefüllt.) Die erste Herausforderung lösen werden tatsächlich eine Dropdownliste mit diesem Steuerelement geben.</span><span class="sxs-lookup"><span data-stu-id="bceea-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="bceea-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="bceea-111">Steps</span></span>

<span data-ttu-id="bceea-112">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="bceea-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="bceea-113">Anschließend ist ein DropDownList-Steuerelement erforderlich:</span><span class="sxs-lookup"><span data-stu-id="bceea-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="bceea-114">Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bceea-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="bceea-115">Eine asynchrone Anforderung sendet an einen Webdienst die klicken Sie dann eine Liste mit Einträgen in der Liste anzuzeigende zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bceea-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="bceea-116">Damit dies funktioniert müssen die folgenden CascadingDropDown Attribute festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="bceea-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="bceea-117">`ServicePath`: Die URL eines Webdiensts, der Einträge in der Liste bereitstellen</span><span class="sxs-lookup"><span data-stu-id="bceea-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="bceea-118">`ServiceMethod`: Web-Methode, die Bereitstellung von Einträge in der Liste</span><span class="sxs-lookup"><span data-stu-id="bceea-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="bceea-119">`TargetControlID`: ID der Dropdown-Liste</span><span class="sxs-lookup"><span data-stu-id="bceea-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="bceea-120">`Category`: Kategorieinformationen, die beim Aufruf der Web-Methode übermittelt wird</span><span class="sxs-lookup"><span data-stu-id="bceea-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="bceea-121">`PromptText`: Text, der angezeigt wird, wenn Sie Daten aus der Liste asynchron vom Server geladen</span><span class="sxs-lookup"><span data-stu-id="bceea-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="bceea-122">Dies ist das Markup für die `CascadingDropDown` Element.</span><span class="sxs-lookup"><span data-stu-id="bceea-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="bceea-123">Der einzige Unterschied zwischen c# und VB ist der Name des zugeordneten Webdiensts:</span><span class="sxs-lookup"><span data-stu-id="bceea-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="bceea-124">Der JavaScript-Code, der von der `CascadingDropDown` Extender Aufrufen eine Webdienstmethode mit folgender Signatur:</span><span class="sxs-lookup"><span data-stu-id="bceea-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="bceea-125">Damit der wichtigste Aspekt ist, dass die Methode ein Array vom Typ zurückgeben muss `CascadingDropDownNameValue` (definiert durch die ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="bceea-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="bceea-126">In der `CascadingDropDownNameValue` Konstruktor, der erste Eintrag in der der Text, und klicken Sie dann seinen Wert angegeben werden müssen, ebenso wie `<option value="VALUE">NAME</option>` in HTML tun würden.</span><span class="sxs-lookup"><span data-stu-id="bceea-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="bceea-127">Hier sind einige Beispieldaten:</span><span class="sxs-lookup"><span data-stu-id="bceea-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="bceea-128">Die Seite im Browser laden, löst die Liste mit drei Anbietern gefüllt werden soll.</span><span class="sxs-lookup"><span data-stu-id="bceea-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="bceea-129">[![Die Liste wird automatisch ausgefüllt.](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bceea-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="bceea-130">Die Liste wird automatisch übernommen ([klicken Sie, um das Bild in voller Größe anzeigen](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bceea-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bceea-131">Nächste</span><span class="sxs-lookup"><span data-stu-id="bceea-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
