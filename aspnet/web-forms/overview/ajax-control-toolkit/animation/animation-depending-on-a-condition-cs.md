---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animation in Abhängigkeit von einer BedingungC#() | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Gibt an, ob eine Animation ist...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599857"
---
# <a name="animation-depending-on-a-condition-c"></a>Von einer Bedingung abhängige Animation (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Ob eine Animation ausgeführt wird oder nicht, kann auch von einer Bedingung in Form von JavaScript-Code abhängen.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde. Anstelle einer regulären Animation kommt das `<Condition>`-Element ins Spiel. Der JavaScript-Code, der als Wert des `ConditionScript` Attributs bereitgestellt wird, wird zur Laufzeit ausgeführt. Wenn true ausgewertet wird, wird die Animation ausgeführt; andernfalls nicht. Das folgende Markup stellt zwei Animationen bereit, die jeweils in 50% der Fälle nach dem Zufallsprinzip ausgeführt werden. Da es möglicherweise nur eine Animation innerhalb `<OnLoad>`gibt, werden die beiden `<Condition>` Animationen mit dem `<Sequence>`-Element verknüpft:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Beachten Sie, dass das kleiner-als-Zeichen (`<`) im `ConditionScript`-Attribut mit Escapezeichen versehen werden muss (). Wenn Sie dieses Skript ausführen, wird entweder weder eine Animation ausgeführt noch eine der beiden Aktionen ausgeführt.

[![der Bereich ausgeblendet wird, ohne die Größe zu ändern, also wird die zweite Animation ausgeführt, der erste nicht](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Der Bereich wird ausgeblendet, ohne die Größe zu ändern. Daher wird die zweite Animation ausgeführt, der erste nicht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](animation-depending-on-a-condition-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-after-each-other-cs.md)
> [Weiter](picking-one-animation-out-of-a-list-cs.md)
