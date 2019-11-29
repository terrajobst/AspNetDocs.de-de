---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animieren als Reaktion auf Benutzerinteraktion (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können Stern...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607035"
---
# <a name="animating-in-response-to-user-interaction-vb"></a>Animieren als Reaktion auf eine Benutzerinteraktion (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können automatisch gestartet werden, oder Sie können durch Benutzerinteraktion ausgelöst werden, z. b. durch Klicken mit der Maus.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animationen können automatisch gestartet werden, oder Sie können durch Benutzerinteraktion ausgelöst werden, z. b. durch Klicken mit der Maus.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

Innerhalb des `<Animations>` Knotens gibt es fünf Möglichkeiten, um die Animation über eine Benutzerinteraktion zu starten (das fehlende Element ist `<OnLoad>`, das ausgeführt wird, sobald die gesamte Seite vollständig geladen wurde):

- `<OnClick>` (Klicken Sie auf das Steuerelement)
- `<OnHoverOut>` (Maus verlässt das Steuerelement)
- `<OnHoverOver>` (Mauszeiger über ein Steuerelement, Beenden der `<OnHoverOut>` Animation)
- `<OnMouseOut>` (Maus verlässt ein Steuerelement)
- `<OnMouseOver>` (Mauszeiger über ein Steuerelement, das `<OnMouseOut>` Animation wird nicht angehalten)

In diesem Szenario wird `<OnClick>` verwendet. Wenn der Benutzer auf den Bereich klickt, wird die Größe angepasst und gleichzeitig ausgeblendet.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

[![mit einem Mausklick wird die Animation gestartet.](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Mit einem Mausklick wird die Animation gestartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](picking-one-animation-out-of-a-list-vb.md)
> [Weiter](disabling-actions-during-animation-vb.md)
