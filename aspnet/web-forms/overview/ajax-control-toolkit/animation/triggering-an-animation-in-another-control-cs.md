---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Auslösen einer Animation in einem anderen SteuerC#Element () | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Im Allgemeinen wird das Starten einer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599613"
---
# <a name="triggering-an-animation-in-another-control-c"></a>Auslösen einer Animation in einem anderen Steuerelement (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Im Allgemeinen wird das Starten einer Animation durch eine Benutzerinteraktion mit demselben Steuerelement ausgelöst. Es ist jedoch auch möglich, mit einem Steuerelement zu interagieren und dann ein anderes Steuerelement zu Animation.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Im Allgemeinen wird das Starten einer Animation durch eine Benutzerinteraktion mit demselben Steuerelement ausgelöst. Es ist jedoch auch möglich, mit einem Steuerelement zu interagieren und dann ein anderes Steuerelement zu Animation.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Um mit der Animation des Panels zu beginnen, wird eine HTML-Schaltfläche verwendet. Beachten Sie, dass `<input type="button" />` für `<asp:Button />` bevorzugt ist, da wir kein Postback wünschen, wenn der Benutzer auf diese Schaltfläche klickt.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an. Es ist wichtig, `TargetControlID` auf die ID der Schaltfläche (das Element, das die Animation auslöst) festzulegen, nicht auf die ID des Bereichs (das zu animierende Element).

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Platzieren Sie die Animationen innerhalb des `<Animations>` Knotens wie üblich. Um den Bereich zu ändern, nicht die Schaltfläche, legen Sie das `AnimationTarget`-Attribut für jedes Animations Element innerhalb `AnimationExtender`fest. Der Wert für `AnimationTarget` ist natürlich die ID des Panels. Auf diese Weise werden die Animationen mit dem Panel und nicht mit der auslösenden Schaltfläche durchgeführt. Im folgenden finden Sie `AnimationExtender` Markup für dieses Szenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Beachten Sie die besondere Reihenfolge, in der die einzelnen Animationen angezeigt werden. Zuerst wird die Schaltfläche deaktiviert, sobald die Animation ausgeführt wird. Da das `<EnableAction>`-Element kein `AnimationTarget` Attribut enthält, wird diese Animation auf das ursprüngliche Steuerelement angewendet: die Schaltfläche. Die nächsten zwei Animations Schritte müssen parallel ausgeführt werden (`<Parallel>`-Element). Beide Attribute `AnimationTarget` sind auf `"Panel1"`festgelegt, sodass das Panel und nicht die Schaltfläche animiert werden.

[![mit einem Mausklick auf die Schaltfläche startet die Panel-Animation.](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Mit dem Mauszeiger auf die Schaltfläche wird die Panel-Animation gestartet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](triggering-an-animation-in-another-control-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](disabling-actions-during-animation-cs.md)
> [Weiter](modifying-animations-from-the-server-side-cs.md)
