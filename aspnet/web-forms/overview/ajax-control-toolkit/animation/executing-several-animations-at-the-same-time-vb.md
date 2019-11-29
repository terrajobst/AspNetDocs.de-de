---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Gleich zeidas Ausführen mehrerer Animationen (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie ermöglicht das Ausführen von "Schweregrad"...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: fc11debd06c897c29db56d42167d483f5608cdf8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575309"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a>Gleich zeidas Ausführen mehrerer Animationen (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Dadurch können mehrere Animationen parallel ausgeführt werden.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Dadurch können mehrere Animationen parallel ausgeführt werden.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde. Im Allgemeinen akzeptiert `<OnLoad>` nur eine Animation. Das Animations Framework ermöglicht es Ihnen, mehrere Animationen mit dem `<Parallel>`-Element in ein Element einzubinden. Alle Animationen in `<Parallel>` werden gleichzeitig ausgeführt.

Im folgenden finden Sie ein mögliches Markup für das `AnimationExtender`-Steuerelement, wobei der Bereich gleichzeitig ausgeblendet und seine Größe geändert wird:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

Und tatsächlich: Wenn Sie dieses Skript ausführen, wird der Bereich angezeigt, und die Größe wird geändert (mehr als die Breite und die Höhe der Höhe) und gleichzeitig ausgeblendet.

[![der Bereich ausgeblendet wird und seine Größe ändert (einschließlich seines Inhalts, dank der Rendering-Engine des Browsers)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

Der Bereich wird ausgeblendet und seine Größe angepasst (einschließlich seines Inhalts, dank der Rendering-Engine des Browsers) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-at-the-same-time-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](adding-animation-to-a-control-vb.md)
> [Weiter](executing-several-animations-after-each-other-vb.md)
