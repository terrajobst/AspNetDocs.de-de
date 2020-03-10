---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Per Drag & Drop über ReorderListC#() | Microsoft-Dokumentation
author: wenz
description: Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Die aktuelle Reihenfolge der Liste soll...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 2fc6d55a290cbb58bea36d8145d814e337bbd931
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446007"
---
# <a name="drag-and-drop-via-reorderlist-c"></a>Drag & Drop über ReorderList (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Die aktuelle Reihenfolge der Liste muss auf dem Server persistent gespeichert werden.

## <a name="overview"></a>Übersicht

Das `ReorderList`-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Die aktuelle Reihenfolge der Liste muss auf dem Server persistent gespeichert werden.

## <a name="steps"></a>Schritte

Das `ReorderList`-Steuerelement unterstützt das Binden von Daten aus einer Datenbank in die Liste. Das beste daran ist, dass es auch das Schreiben von Änderungen in der Reihenfolge des Listen Elements in den Datenspeicher unterstützt.

In diesem Beispiel wird Microsoft SQL Server 2005 Express Edition als Datenspeicher verwendet. Bei der Datenbank handelt es sich um einen optionalen (und kostenlosen) Teil einer Visual Studio-Installation, einschließlich Express Edition. Es ist auch unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)als separater Download verfügbar. Bei diesem Beispiel wird davon ausgegangen, dass die Instanz des SQL Server 2005 Express Edition `SQLEXPRESS` ist und sich auf demselben Computer wie der Webserver befindet. Dies ist auch das Standard Setup. Wenn das Setup abweicht, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung des Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Stellen Sie eine Verbindung mit dem Server her, doppelklicken Sie auf `Databases`, und erstellen Sie eine neue Datenbank (Klicken Sie mit der rechten Maustaste, und wählen Sie `New Database`) `Tutorials`

Erstellen Sie in dieser Datenbank eine neue Tabelle mit dem Namen `AJAX` mit den folgenden vier Spalten:

- `id` (Primärschlüssel, Ganzzahl, Identität, nicht null)
- `char` (Char (1), null)
- `description` (varchar (50), null)
- `position` (int, null)

[![des Layouts der AJAX-Tabelle](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Das Layout der AJAX-Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](drag-and-drop-via-reorderlist-cs/_static/image3.png))

Füllen Sie als nächstes die Tabelle mit einigen Werten aus. Beachten Sie, dass die `position` Spalte die Sortierreihenfolge der Elemente enthält.

[![der ursprünglichen Daten in der AJAX-Tabelle](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

Die ursprünglichen Daten in der AJAX-Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](drag-and-drop-via-reorderlist-cs/_static/image6.png))

Der nächste Schritt erfordert, dass ein `SqlDataSource`-Steuerelement generiert wird, um mit der neuen Datenbank und der zugehörigen Tabelle zu kommunizieren. Die Datenquelle muss die SQL-Befehle `SELECT` und `UPDATE` unterstützen. Wenn die Reihenfolge der Listenelemente zu einem späteren Zeitpunkt geändert wird, übermittelt das `ReorderList`-Steuerelement automatisch zwei Werte an den `Update`-Befehl der Datenquelle: die neue Position und die ID des Elements. Daher benötigt die Datenquelle für diese beiden Werte einen `<UpdateParameters>` Abschnitt:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

Das `ReorderList`-Steuerelement muss die folgenden Attribute festlegen:

- `AllowReorder`: gibt an, ob die Listenelemente neu angeordnet werden können.
- `DataSourceID`: die ID der Datenquelle.
- `DataKeyField`: der Name der Primärschlüssel Spalte in der Datenquelle.
- `SortOrderField`: die Datenquellen Spalte, die die Sortierreihenfolge für die Listenelemente bereitstellt.

In den Abschnitten `<DragHandleTemplate>` und `<ItemTemplate>` kann das Layout der Liste optimiert werden. Außerdem ist Datenbindung mithilfe der `Eval()`-Methode möglich, wie hier gezeigt:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Die folgenden CSS-Formatinformationen (auf die im `<DragHandleTemplate>` Abschnitt des `ReorderList`-Steuer Elements verwiesen wird) stellen sicher, dass sich der Mauszeiger entsprechend ändert, wenn er auf das Zieh Handle zeigt:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Zum Schluss Initialisiert ein `ScriptManager`-Steuerelement ASP.NET AJAX für die Seite:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Führen Sie dieses Beispiel im Browser aus, und ordnen Sie die Listenelemente mit einem Bit neu an. Laden Sie dann die Seite neu, und/oder sehen Sie sich die Datenbank an. Die geänderten Positionen wurden beibehalten und werden auch durch die Werte in der `position`-Spalte in der Datenbank und alle ohne Code, nur mithilfe von Markup, reflektiert.

[![die Daten in der Datenbank gemäß der neuen Reihenfolge der Listenelemente geändert werden.](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Die Daten in der Datenbank ändern sich entsprechend der Reihenfolge der neuen Listenelemente ([Klicken Sie, um das Bild in voller Größe anzuzeigen](drag-and-drop-via-reorderlist-cs/_static/image9.png)).

> [!div class="step-by-step"]
> [Zurück](using-postbacks-with-reorderlist-cs.md)
> [Weiter](using-postbacks-with-reorderlist-vb.md)
