---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Verwenden von Postbacks mit ReorderListC#() | Microsoft-Dokumentation
author: wenz
description: Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet ist, wird ein Po...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611392"
---
# <a name="using-postbacks-with-reorderlist-c"></a>Verwenden von Postbacks mit dem ReorderList-Steuerelement (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, wird der Server von einem Postback über die Änderung informiert.

## <a name="overview"></a>Übersicht über

Das `ReorderList`-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, wird der Server von einem Postback über die Änderung informiert.

## <a name="steps"></a>Schritte

Es gibt mehrere mögliche Datenquellen für das `ReorderList`-Steuerelement. Eine ist die Verwendung eines `XmlDataSource`-Steuer Elements:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Um diesen XML-Code an ein `ReorderList`-Steuerelement zu binden und Postbacks zu aktivieren, müssen die folgenden Attribute festgelegt werden:

- `DataSourceID`: die ID der Datenquelle.
- `SortOrderField`: die Eigenschaft, nach der sortiert werden soll.
- `AllowReorder`: gibt an, ob der Benutzer das Anordnen der Listenelemente gestatten soll.
- `PostBackOnReorder`: gibt an, ob ein Postback erstellt werden soll, wenn die Liste neu angeordnet wird.

Im folgenden finden Sie das entsprechende Markup für das-Steuerelement:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

Innerhalb des `ReorderList`-Steuer Elements können bestimmte Daten aus der Datenquelle mithilfe der `Eval()`-Methode gebunden werden:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

An beliebiger Position auf der Seite enthält eine Bezeichnung die Informationen, die bei der letzten Neuanordnung aufgetreten sind:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Diese Bezeichnung ist mit Text im serverseitigen Code gefüllt, der das Postback verarbeitet:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Zum Schluss muss das `ScriptManager`-Steuerelement auf der Seite abgelegt werden, um die Funktionalität von ASP.NET AJAX und dem Control Toolkit zu aktivieren:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

[![jede Neuanordnung ein Postback auslöst.](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Jede Neuanordnung löst ein Postback aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-postbacks-with-reorderlist-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Nächste](drag-and-drop-via-reorderlist-cs.md)
