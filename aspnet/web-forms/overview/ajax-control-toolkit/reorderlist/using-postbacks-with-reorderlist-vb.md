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
# <a name="using-postbacks-with-reorderlist-vb"></a>Verwenden von Postbacks mit dem ReorderList-Steuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Das ReorderList-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, wird der Server von einem Postback über die Änderung informiert.

## <a name="overview"></a>Übersicht

Das `ReorderList`-Steuerelement im AJAX Control Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, wird der Server von einem Postback über die Änderung informiert.

## <a name="steps"></a>Schritte

Es gibt mehrere mögliche Datenquellen für das `ReorderList`-Steuerelement. Eine ist die Verwendung eines `XmlDataSource`-Steuer Elements:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Um diesen XML-Code an ein `ReorderList`-Steuerelement zu binden und Postbacks zu aktivieren, müssen die folgenden Attribute festgelegt werden:

- `DataSourceID`: die ID der Datenquelle.
- `SortOrderField`: die Eigenschaft, nach der sortiert werden soll.
- `AllowReorder`: gibt an, ob der Benutzer das Anordnen der Listenelemente gestatten soll.
- `PostBackOnReorder`: gibt an, ob ein Postback erstellt werden soll, wenn die Liste neu angeordnet wird.

Im folgenden finden Sie das entsprechende Markup für das-Steuerelement:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

Innerhalb des `ReorderList`-Steuer Elements können bestimmte Daten aus der Datenquelle mithilfe der `Eval()`-Methode gebunden werden:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

An beliebiger Position auf der Seite enthält eine Bezeichnung die Informationen, die bei der letzten Neuanordnung aufgetreten sind:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Diese Bezeichnung ist mit Text im serverseitigen Code gefüllt, der das Postback verarbeitet:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Zum Schluss muss das `ScriptManager`-Steuerelement auf der Seite abgelegt werden, um die Funktionalität von ASP.NET AJAX und dem Control Toolkit zu aktivieren:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

[![jede Neuanordnung ein Postback auslöst.](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Jede Neuanordnung löst ein Postback aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-postbacks-with-reorderlist-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](drag-and-drop-via-reorderlist-cs.md)
> [Weiter](drag-and-drop-via-reorderlist-vb.md)
