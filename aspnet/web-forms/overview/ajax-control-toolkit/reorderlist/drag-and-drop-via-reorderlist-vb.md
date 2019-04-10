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
ms.openlocfilehash: 1b955c43a0fc95bda87843fc4a5c9e56aef3dfc6
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400996"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="7da80-103">Drag & Drop über ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="7da80-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="7da80-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7da80-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7da80-105">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7da80-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="7da80-106">Die ReorderList-Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="7da80-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="7da80-107">Die Liste der aktuellen Taskreihenfolge muss auf dem Server beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="7da80-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="7da80-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7da80-108">Overview</span></span>

<span data-ttu-id="7da80-109">Die `ReorderList` Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="7da80-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="7da80-110">Die Liste der aktuellen Taskreihenfolge muss auf dem Server beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="7da80-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="7da80-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="7da80-111">Steps</span></span>

<span data-ttu-id="7da80-112">Die `ReorderList` -Steuerelement unterstützt, Binden von Daten aus einer Datenbank in der Liste.</span><span class="sxs-lookup"><span data-stu-id="7da80-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="7da80-113">Das beste daran ist, unterstützt aber auch das Schreiben von Änderungen Sequenznummern Listenelements zurück an den Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="7da80-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="7da80-114">Dieses Beispiel verwendet die Microsoft SQL Server 2005 Express Edition als Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="7da80-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="7da80-115">Die Datenbank ist ein optional (und kostenlosen) Teil einer Installation von Visual Studio, einschließlich der express Edition.</span><span class="sxs-lookup"><span data-stu-id="7da80-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="7da80-116">Es ist auch verfügbar als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="7da80-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="7da80-117">In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung.</span><span class="sxs-lookup"><span data-stu-id="7da80-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="7da80-118">Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.</span><span class="sxs-lookup"><span data-stu-id="7da80-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="7da80-119">Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="7da80-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="7da80-120">Mit dem Server verbinden, doppelklicken Sie auf `Databases` und eine neue Datenbank erstellen (mit der rechten Maustaste, und wählen Sie `New Database`) namens `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="7da80-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="7da80-121">In dieser Datenbank, erstellen Sie eine neue Tabelle namens `AJAX` mit den folgenden vier Spalten:</span><span class="sxs-lookup"><span data-stu-id="7da80-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- `id` <span data-ttu-id="7da80-122">(primärer Schlüssel, Integer, Identität, nicht NULL)</span><span class="sxs-lookup"><span data-stu-id="7da80-122">(primary key, integer, identity, not NULL)</span></span>
- `char` <span data-ttu-id="7da80-123">(char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="7da80-123">(char(1), NULL)</span></span>
- `description` <span data-ttu-id="7da80-124">(varchar(50)-Spalte, NULL)</span><span class="sxs-lookup"><span data-stu-id="7da80-124">(varchar(50), NULL)</span></span>
- `position` <span data-ttu-id="7da80-125">(int, NULL)</span><span class="sxs-lookup"><span data-stu-id="7da80-125">(int, NULL)</span></span>


[![T<span data-ttu-id="7da80-126">IE-Layout der Tabelle AJAX]</span><span class="sxs-lookup"><span data-stu-id="7da80-126">he layout of the AJAX table]</span></span>(drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

<span data-ttu-id="7da80-127">Das Layout der AJAX-Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7da80-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="7da80-128">Füllen Sie dann die Tabelle mit ein Paar von Werten.</span><span class="sxs-lookup"><span data-stu-id="7da80-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="7da80-129">Beachten Sie, dass die `position` -Spalte enthält die Sortierreihenfolge der Elemente.</span><span class="sxs-lookup"><span data-stu-id="7da80-129">Note that the `position` column holds the sort order of the elements.</span></span>


[![T<span data-ttu-id="7da80-130">seine ursprünglichen Daten in der AJAX-Tabelle]</span><span class="sxs-lookup"><span data-stu-id="7da80-130">he initial data in the AJAX table]</span></span>(drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

<span data-ttu-id="7da80-131">Die ersten Daten in der AJAX-Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7da80-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="7da80-132">Der nächste Schritt ist erforderlich, zum Generieren einer `SqlDataSource` Steuerelement für die Kommunikation mit der neuen Datenbank und die Tabelle.</span><span class="sxs-lookup"><span data-stu-id="7da80-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="7da80-133">Die Datenquelle muss unterstützen die `SELECT` und `UPDATE` SQL-Befehle.</span><span class="sxs-lookup"><span data-stu-id="7da80-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="7da80-134">Wenn die Reihenfolge der Listenelemente später geändert wird, die `ReorderList` Steuerelement werden automatisch zwei Werte an der Datenquelle sendet `Update` Befehl: die neue Position und die ID des Elements.</span><span class="sxs-lookup"><span data-stu-id="7da80-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="7da80-135">Aus diesem Grund die Datenquelle muss ein `<UpdateParameters>` Abschnitt für diese beiden Werte:</span><span class="sxs-lookup"><span data-stu-id="7da80-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="7da80-136">Die `ReorderList` Steuerelement muss die folgenden Attribute festzulegen:</span><span class="sxs-lookup"><span data-stu-id="7da80-136">The `ReorderList` control needs to set the following attributes:</span></span>

- `AllowReorder`<span data-ttu-id="7da80-137">: Gibt an, ob Elemente der Liste neu angeordnet werden können</span><span class="sxs-lookup"><span data-stu-id="7da80-137">: Whether the list items may be rearranged</span></span>
- `DataSourceID`<span data-ttu-id="7da80-138">: Die ID der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="7da80-138">: The ID of the data source</span></span>
- `DataKeyField`<span data-ttu-id="7da80-139">: Der Name der Primärschlüsselspalte in der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="7da80-139">: The name of the primary key column in the data source</span></span>
- `SortOrderField`<span data-ttu-id="7da80-140">: Die Datenspalte für die Quelle, die die Sortierreihenfolge für die Listenelemente bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="7da80-140">: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="7da80-141">In der `<DragHandleTemplate>` und `<ItemTemplate>` Abschnitte, das Layout der Liste angepasst werden kann.</span><span class="sxs-lookup"><span data-stu-id="7da80-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="7da80-142">Datenbindung ist auch möglich mit der `Eval()` -Methode, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="7da80-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="7da80-143">Das folgende CSS-Formatinformationen (verwiesen wird, in der `<DragHandleTemplate>` Teil der `ReorderList` Steuerelement) wird sichergestellt, dass sich der Mauszeiger entsprechend ändert, wenn sie über den Ziehpunkt zeigen:</span><span class="sxs-lookup"><span data-stu-id="7da80-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="7da80-144">Zum Schluss eine `ScriptManager` Steuerelement initialisiert ASP.NET AJAX für die Seite:</span><span class="sxs-lookup"><span data-stu-id="7da80-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="7da80-145">Führen Sie in diesem Beispiel wird im Browser aus, und ordnen Sie die Listenelemente etwas.</span><span class="sxs-lookup"><span data-stu-id="7da80-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="7da80-146">Anschließend laden Sie die Seite bzw. verfügen Sie einen Blick auf die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7da80-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="7da80-147">Die geänderten Positionen noch gültig und werden auch übernommen, nach den Werten in der `position` -Spalte in die Datenbank ohne Code, nur mithilfe von Markup.</span><span class="sxs-lookup"><span data-stu-id="7da80-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


[![T<span data-ttu-id="7da80-148">er Daten in der Datenbank ändert sich entsprechend die neue Reihenfolge der Liste Elemente]</span><span class="sxs-lookup"><span data-stu-id="7da80-148">he data in the database changes according to the new list item order]</span></span>(drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

<span data-ttu-id="7da80-149">Das Datenelement in der Datenbank ändert sich gemäß der neuen Liste Reihenfolge ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="7da80-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7da80-150">Vorheriges</span><span class="sxs-lookup"><span data-stu-id="7da80-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
