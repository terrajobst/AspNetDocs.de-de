---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animation in Abhängigkeit von einer Bedingung (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Gibt an, ob eine Animation ist...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497877"
---
# <a name="animation-depending-on-a-condition-vb"></a>Von einer Bedingung abhängige Animation (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde. Anstelle einer regulären Animation kommt das `<Condition>`-Element ins Spiel. Der JavaScript-Code, der als Wert des `ConditionScript` Attributs bereitgestellt wird, wird zur Laufzeit ausgeführt. Wenn true ausgewertet wird, wird die Animation ausgeführt; andernfalls nicht. Das folgende Markup stellt zwei Animationen bereit, die jeweils in 50% der Fälle nach dem Zufallsprinzip ausgeführt werden. Da es möglicherweise nur eine Animation innerhalb `<OnLoad>`gibt, werden die beiden `<Condition>` Animationen mit dem `<Sequence>`-Element verknüpft:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Beachten Sie, dass das kleiner-als-Zeichen (`<`) im `ConditionScript`-Attribut mit Escapezeichen versehen werden muss (). Wenn Sie dieses Skript ausführen, wird entweder weder eine Animation ausgeführt noch eine der beiden Aktionen ausgeführt.

[![der Bereich ausgeblendet wird, ohne die Größe zu ändern, also wird die zweite Animation ausgeführt, der erste nicht](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Der Bereich wird ausgeblendet, ohne die Größe zu ändern. Daher wird die zweite Animation ausgeführt, der erste nicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animation-depending-on-a-condition-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-after-each-other-vb.md)
> [Weiter](picking-one-animation-out-of-a-list-vb.md)
