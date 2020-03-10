---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Verwenden von Postbacks mit ReorderList (VB) | Microsoft-Dokumentation
author: wenz
description: Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet ist, wird ein Po...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d6075e40df2c32df6c0d801243eff98fa7790b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78445929"
---
# <a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="bac2b-104">Verwenden von Postbacks mit dem ReorderList-Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="bac2b-104">Using Postbacks with ReorderList (VB)</span></span>

<span data-ttu-id="bac2b-105">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bac2b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bac2b-106">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bac2b-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="bac2b-107">Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="bac2b-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="bac2b-108">Wenn die Liste neu angeordnet wird, wird der Server von einem Postback über die Änderung informiert.</span><span class="sxs-lookup"><span data-stu-id="bac2b-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="bac2b-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bac2b-109">Overview</span></span>

<span data-ttu-id="bac2b-110">Das `ReorderList`-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="bac2b-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="bac2b-111">Wenn die Liste neu angeordnet wird, wird der Server von einem Postback über die Änderung informiert.</span><span class="sxs-lookup"><span data-stu-id="bac2b-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="bac2b-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="bac2b-112">Steps</span></span>

<span data-ttu-id="bac2b-113">Es gibt mehrere mögliche Datenquellen für das `ReorderList`-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="bac2b-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="bac2b-114">Eine ist die Verwendung eines `XmlDataSource`-Steuer Elements:</span><span class="sxs-lookup"><span data-stu-id="bac2b-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="bac2b-115">Um diesen XML-Code an ein `ReorderList`-Steuerelement zu binden und Postbacks zu aktivieren, müssen die folgenden Attribute festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="bac2b-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="bac2b-116">`DataSourceID`: die ID der Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="bac2b-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="bac2b-117">`SortOrderField`: die Eigenschaft, nach der sortiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="bac2b-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="bac2b-118">`AllowReorder`: gibt an, ob der Benutzer das Anordnen der Listenelemente gestatten soll.</span><span class="sxs-lookup"><span data-stu-id="bac2b-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="bac2b-119">`PostBackOnReorder`: gibt an, ob ein Postback erstellt werden soll, wenn die Liste neu angeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="bac2b-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="bac2b-120">Im folgenden finden Sie das entsprechende Markup für das-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="bac2b-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="bac2b-121">Innerhalb des `ReorderList`-Steuer Elements können bestimmte Daten aus der Datenquelle mithilfe der `Eval()`-Methode gebunden werden:</span><span class="sxs-lookup"><span data-stu-id="bac2b-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="bac2b-122">An beliebiger Position auf der Seite enthält eine Bezeichnung die Informationen, die bei der letzten Neuanordnung aufgetreten sind:</span><span class="sxs-lookup"><span data-stu-id="bac2b-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="bac2b-123">Diese Bezeichnung ist mit Text im serverseitigen Code gefüllt, der das Postback verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="bac2b-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="bac2b-124">Zum Schluss muss das `ScriptManager`-Steuerelement auf der Seite abgelegt werden, um die Funktionalität von ASP.NET AJAX und dem Control Toolkit zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="bac2b-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

<span data-ttu-id="bac2b-125">[![jede Neuanordnung ein Postback auslöst.](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bac2b-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="bac2b-126">Jede Neuanordnung löst ein Postback aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-postbacks-with-reorderlist-vb/_static/image3.png)).</span><span class="sxs-lookup"><span data-stu-id="bac2b-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bac2b-127">[Zurück](drag-and-drop-via-reorderlist-cs.md)
> [Weiter](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bac2b-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
