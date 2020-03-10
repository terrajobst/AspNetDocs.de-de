---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Drag & Drop über ReorderList (VB) | Microsoft-Dokumentation
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f7c5749053d8bf587467fb1939fca05ce2872a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446085"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="0aff6-103">Drag & Drop über ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="0aff6-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="0aff6-104">von [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0aff6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0aff6-105">[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0aff6-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="0aff6-106">Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0aff6-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0aff6-107">Die aktuelle Reihenfolge der Liste muss auf dem Server persistent gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="0aff6-107">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="0aff6-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="0aff6-108">Overview</span></span>

<span data-ttu-id="0aff6-109">Das `ReorderList`-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0aff6-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0aff6-110">Die aktuelle Reihenfolge der Liste muss auf dem Server persistent gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="0aff6-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="0aff6-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="0aff6-111">Steps</span></span>

<span data-ttu-id="0aff6-112">Das `ReorderList`-Steuerelement unterstützt das Binden von Daten aus einer Datenbank in die Liste.</span><span class="sxs-lookup"><span data-stu-id="0aff6-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="0aff6-113">Das beste daran ist, dass es auch das Schreiben von Änderungen in der Reihenfolge des Listen Elements in den Datenspeicher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0aff6-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="0aff6-114">In diesem Beispiel wird Microsoft SQL Server 2005 Express Edition als Datenspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="0aff6-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="0aff6-115">Bei der Datenbank handelt es sich um einen optionalen (und kostenlosen) Teil einer Visual Studio-Installation, einschließlich Express Edition.</span><span class="sxs-lookup"><span data-stu-id="0aff6-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="0aff6-116">Es ist auch unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)als separater Download verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0aff6-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0aff6-117">Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup.</span><span class="sxs-lookup"><span data-stu-id="0aff6-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0aff6-118">Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.</span><span class="sxs-lookup"><span data-stu-id="0aff6-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0aff6-119">Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="0aff6-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="0aff6-120">Stellen Sie eine Verbindung mit dem Server her, doppelklicken Sie auf `Databases`, und erstellen Sie eine neue Datenbank (Klicken Sie mit der rechten Maustaste, und wählen Sie `New Database`) `Tutorials`</span><span class="sxs-lookup"><span data-stu-id="0aff6-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="0aff6-121">Erstellen Sie in dieser Datenbank eine neue Tabelle mit dem Namen `AJAX` mit den folgenden vier Spalten:</span><span class="sxs-lookup"><span data-stu-id="0aff6-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="0aff6-122">`id` (Primärschlüssel, Ganzzahl, Identität, nicht null)</span><span class="sxs-lookup"><span data-stu-id="0aff6-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="0aff6-123">`char` (Char (1), null)</span><span class="sxs-lookup"><span data-stu-id="0aff6-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="0aff6-124">`description` (varchar (50), null)</span><span class="sxs-lookup"><span data-stu-id="0aff6-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="0aff6-125">`position` (int, null)</span><span class="sxs-lookup"><span data-stu-id="0aff6-125">`position` (int, NULL)</span></span>

<span data-ttu-id="0aff6-126">[![des Layouts der AJAX-Tabelle](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0aff6-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="0aff6-127">Das Layout der AJAX-Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0aff6-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>

<span data-ttu-id="0aff6-128">Füllen Sie als nächstes die Tabelle mit einigen Werten aus.</span><span class="sxs-lookup"><span data-stu-id="0aff6-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="0aff6-129">Beachten Sie, dass die `position` Spalte die Sortierreihenfolge der Elemente enthält.</span><span class="sxs-lookup"><span data-stu-id="0aff6-129">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="0aff6-130">[![der ursprünglichen Daten in der AJAX-Tabelle](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0aff6-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="0aff6-131">Die ursprünglichen Daten in der AJAX-Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0aff6-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>

<span data-ttu-id="0aff6-132">Der nächste Schritt erfordert, dass ein `SqlDataSource`-Steuerelement generiert wird, um mit der neuen Datenbank und der zugehörigen Tabelle zu kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="0aff6-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="0aff6-133">Die Datenquelle muss die SQL-Befehle `SELECT` und `UPDATE` unterstützen.</span><span class="sxs-lookup"><span data-stu-id="0aff6-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="0aff6-134">Wenn die Reihenfolge der Listenelemente zu einem späteren Zeitpunkt geändert wird, übermittelt das `ReorderList`-Steuerelement automatisch zwei Werte an den `Update`-Befehl der Datenquelle: die neue Position und die ID des Elements.</span><span class="sxs-lookup"><span data-stu-id="0aff6-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="0aff6-135">Daher benötigt die Datenquelle für diese beiden Werte einen `<UpdateParameters>` Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="0aff6-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="0aff6-136">Das `ReorderList`-Steuerelement muss die folgenden Attribute festlegen:</span><span class="sxs-lookup"><span data-stu-id="0aff6-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="0aff6-137">`AllowReorder`: gibt an, ob die Listenelemente neu angeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="0aff6-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="0aff6-138">`DataSourceID`: die ID der Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="0aff6-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="0aff6-139">`DataKeyField`: der Name der Primärschlüssel Spalte in der Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="0aff6-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="0aff6-140">`SortOrderField`: die Datenquellen Spalte, die die Sortierreihenfolge für die Listenelemente bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="0aff6-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="0aff6-141">In den Abschnitten `<DragHandleTemplate>` und `<ItemTemplate>` kann das Layout der Liste optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="0aff6-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="0aff6-142">Außerdem ist Datenbindung mithilfe der `Eval()`-Methode möglich, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0aff6-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="0aff6-143">Die folgenden CSS-Formatinformationen (auf die im `<DragHandleTemplate>` Abschnitt des `ReorderList`-Steuer Elements verwiesen wird) stellen sicher, dass sich der Mauszeiger entsprechend ändert, wenn er auf das Zieh Handle zeigt:</span><span class="sxs-lookup"><span data-stu-id="0aff6-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="0aff6-144">Zum Schluss Initialisiert ein `ScriptManager`-Steuerelement ASP.NET AJAX für die Seite:</span><span class="sxs-lookup"><span data-stu-id="0aff6-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="0aff6-145">Führen Sie dieses Beispiel im Browser aus, und ordnen Sie die Listenelemente mit einem Bit neu an.</span><span class="sxs-lookup"><span data-stu-id="0aff6-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="0aff6-146">Laden Sie dann die Seite neu, und/oder sehen Sie sich die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="0aff6-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="0aff6-147">Die geänderten Positionen wurden beibehalten und werden auch durch die Werte in der `position`-Spalte in der Datenbank und alle ohne Code, nur mithilfe von Markup, reflektiert.</span><span class="sxs-lookup"><span data-stu-id="0aff6-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="0aff6-148">[![die Daten in der Datenbank gemäß der neuen Reihenfolge der Listenelemente geändert werden.](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0aff6-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="0aff6-149">Die Daten in der Datenbank ändern sich entsprechend der Reihenfolge der neuen Listenelemente ([Klicken Sie, um das Bild in voller Größe anzuzeigen](drag-and-drop-via-reorderlist-vb/_static/image9.png)).</span><span class="sxs-lookup"><span data-stu-id="0aff6-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0aff6-150">Previous</span><span class="sxs-lookup"><span data-stu-id="0aff6-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
