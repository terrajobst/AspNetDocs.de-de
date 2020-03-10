---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animieren als Reaktion auf Benutzerinteraktion (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können Stern...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497901"
---
# <a name="animating-in-response-to-user-interaction-c"></a>Animieren als Reaktion auf eine Benutzerinteraktion (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können automatisch gestartet werden, oder Sie können durch Benutzerinteraktion ausgelöst werden, z. b. durch Klicken mit der Maus.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können automatisch gestartet werden, oder Sie können durch Benutzerinteraktion ausgelöst werden, z. b. durch Klicken mit der Maus.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

Innerhalb des `<Animations>` Knotens gibt es fünf Möglichkeiten, um die Animation über eine Benutzerinteraktion zu starten (das fehlende Element ist `<OnLoad>`, das ausgeführt wird, sobald die gesamte Seite vollständig geladen wurde):

- `<OnClick>` (Klicken Sie auf das Steuerelement)
- `<OnHoverOut>` (Maus verlässt das Steuerelement)
- `<OnHoverOver>` (Mauszeiger über ein Steuerelement, Beenden der `<OnHoverOut>` Animation)
- `<OnMouseOut>` (Maus verlässt ein Steuerelement)
- `<OnMouseOver>` (Mauszeiger über ein Steuerelement, das `<OnMouseOut>` Animation wird nicht angehalten)

In diesem Szenario wird `<OnClick>` verwendet. Wenn der Benutzer auf den Bereich klickt, wird die Größe angepasst und gleichzeitig ausgeblendet.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

[![mit einem Mausklick wird die Animation gestartet.](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Mit einem Mausklick wird die Animation gestartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](picking-one-animation-out-of-a-list-cs.md)
> [Weiter](disabling-actions-during-animation-cs.md)
