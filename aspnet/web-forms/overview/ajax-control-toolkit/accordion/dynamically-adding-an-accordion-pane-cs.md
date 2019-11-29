---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamisches Hinzufügen eines Accordion-C#Bereichs () | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Panels werden normalerweise als w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607247"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Dynamisches Hinzufügen eines Accordion-C#Bereichs ()

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitiger Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.

## <a name="overview"></a>Übersicht über

Das Steuerelement "Accordion" im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht es dem Benutzer, jeweils ein Element anzuzeigen. Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitiger Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.

## <a name="steps"></a>Schritte

Das Accordion-Steuerelement macht alle wichtigen Eigenschaften für serverseitigen Code verfügbar. Unter anderem ermöglicht die `Panes`-Eigenschaft den Zugriff auf die Sammlung von Bereichen, aus denen das Akkordeon besteht. Jeder Bereich weist den Typ `AccordionPane`auf. Es ist daher trivial, einen solchen Bereich zu erstellen:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

Die `HeaderContainer`-Eigenschaft von `AccordionPane` ermöglicht den Zugriff auf die ASP.NET-Steuerelemente innerhalb des Header Abschnitts des Bereichs. die `ContentContainer`-Eigenschaft von `AccordionPane` ist für den Inhalts Abschnitt des Bereichs identisch. Dadurch kann der ASP.NET-Code den Bereichen Inhalte hinzufügen:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Schließlich müssen der Bereich (n) der `Panes`-Auflistung des Zieh Fensters hinzugefügt werden:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Hier ist ein kompletter serverseitiger Code, der zwei Bereiche zu einem Accordion-Steuerelement hinzufügt:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Das einzige fehlende Element ist das Akkordeon selbst, das vom vorhanden sein des ASP.net-`ScriptManager` Steuer Elements abhängig ist:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Um das Beispiel abzuschließen, stellen die beiden CSS-Klassen, auf die im Accordion-Steuerelement verwiesen wird, Stil Informationen für den Browser bereit:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

[![die Daten im Akkordeon durch serverseitigen Code dynamisch hinzugefügt wurden.](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Die Daten im Akkordeon wurden durch serverseitigen Code dynamisch hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](dynamically-adding-an-accordion-pane-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](databinding-to-an-accordion-cs.md)
> [Weiter](databinding-to-an-accordion-vb.md)
