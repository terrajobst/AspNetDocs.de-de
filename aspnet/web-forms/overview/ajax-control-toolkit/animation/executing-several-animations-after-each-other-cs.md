---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Ausführen mehrerer Animationen nach einander (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie ermöglicht das Ausführen von "Schweregrad"...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: e378ac038796dbd44e89d099532b9e186dcf673f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497823"
---
# <a name="executing-several-animations-after-each-other-c"></a>Sequenzielles Ausführen mehrerer Animationen (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Dadurch können mehrere Animationen nacheinander ausgeführt werden.

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Dadurch können mehrere Animationen nacheinander ausgeführt werden.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde. Im Allgemeinen akzeptiert `<OnLoad>` nur eine Animation. Das Animations Framework ermöglicht es Ihnen, mehrere Animationen mit dem `<Sequence>`-Element in ein Element einzubinden. Alle Animationen in `<Sequence>` werden nacheinander ausgeführt. Im folgenden finden Sie ein mögliches Markup für das `AnimationExtender`-Steuerelement, wobei zunächst der Bereich breiter wird und die Höhe dann verringert wird:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird der Panel zuerst breiter und dann kleiner.

[![zuerst wird die Breite erweitert.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Zuerst wird die Breite erweitert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-after-each-other-cs/_static/image3.png))

[![dann wird die Höhe reduziert.](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Die Höhe wird dann verringert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-several-animations-after-each-other-cs/_static/image6.png)).

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-at-the-same-time-cs.md)
> [Weiter](animation-depending-on-a-condition-cs.md)
