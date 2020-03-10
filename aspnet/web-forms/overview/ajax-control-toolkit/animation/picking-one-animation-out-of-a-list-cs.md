---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Auswählen einer Animation aus einer Liste (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Das Framework gibt auch die...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2bc203d694792716bbf4f7d7b8d152c589bf99b1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483843"
---
# <a name="picking-one-animation-out-of-a-list-c"></a>Auswählen einer Animation aus einer Liste (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Das Framework ermöglicht es dem Programmierer auch, eine Animation aus einer Liste von Animationen auszuwählen, abhängig von der Auswertung von JavaScript-Code.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Das Framework ermöglicht es dem Programmierer auch, eine Animation aus einer Liste von Animationen auszuwählen, abhängig von der Auswertung von JavaScript-Code.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnLoad>`, um die Animationen auszuführen, nachdem die Seite vollständig geladen wurde. Anstelle einer regulären Animation kommt das `<Case>`-Element ins Spiel. Der Wert seines selectScript-Attributs wird ausgewertet. der Rückgabewert muss numerisch sein. Abhängig von dieser Zahl wird eine der untergeordneten Elemente in &lt;Fall&gt; ausgeführt. Wenn selectScript beispielsweise zu 2 ausgewertet wird, führt das steuerungstoolkit die dritte Animation innerhalb &lt;Case-&gt; aus (Zählung beginnt bei 0).

Das folgende Markup definiert drei untergeordnete Elemente: die Größe der Breite, die Größe der Höhe und das ausblenden. Der JavaScript-Code (`Math.floor(3 * Math.random())`) wählt dann eine Zahl zwischen 0 und 2 aus, sodass einer der drei Animationen ausgeführt wird:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

[![einer der möglichen drei Animationen: der Bereich wird breiter.](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Einer der möglichen drei Animationen: der Bereich wird breiter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](picking-one-animation-out-of-a-list-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](animation-depending-on-a-condition-cs.md)
> [Weiter](animating-in-response-to-user-interaction-cs.md)
