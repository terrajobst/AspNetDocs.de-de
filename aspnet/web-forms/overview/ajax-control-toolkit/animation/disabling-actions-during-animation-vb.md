---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deaktivieren von Aktionen während der Animation (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie unterstützt auch Aktionen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606835"
---
# <a name="disabling-actions-during-animation-vb"></a>Deaktivieren von Aktionen während der Animation (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Außerdem werden Aktionen wie Mausklicks unterstützt. Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Außerdem werden Aktionen wie Mausklicks unterstützt. Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Die Animation wird wie folgt auf eine HTML-Schaltfläche angewendet:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Beachten Sie, dass ein HTML-Steuerelement anstelle eines websteuer Elements verwendet wird, da es nicht gewünscht werden soll, dass die Schaltfläche ein Postback erstellt. die Client seitige Animation muss für uns einfach gestartet werden.

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Innerhalb des `<Animations>` Knotens ist `<OnClick>` das Rechte Element, das mit dem Maus Klick behandelt werden soll. Während der Animation kann jedoch auch auf die Schaltfläche geklickt werden. Das `<EnableAction>`-Element kann dies berücksichtigen. Wenn Sie `Enabled="false"` festlegen, wird die Schaltfläche als Teil der Animation deaktiviert. Da wir mehrere einzelne Animationen verwenden (deaktivieren Sie die Schaltfläche und die eigentlichen Animationen), ist das `<Parallel>`-Element erforderlich, um die einzelnen Animationen miteinander zu verbinden. Hier ist das komplette Markup für `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Es ist auch möglich, die Schaltfläche nach der Animation erneut zu aktivieren, indem das folgende XML-Element am Ende der Liste verwendet wird:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Im vorliegenden Szenario wäre dies jedoch nutzlos, da die Schaltfläche ausgeblendet wird und am Ende der Animation nicht sichtbar ist.

[![die Schaltfläche deaktiviert ist, sobald die Animation ausgeführt wird](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](disabling-actions-during-animation-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](animating-in-response-to-user-interaction-vb.md)
> [Weiter](triggering-an-animation-in-another-control-vb.md)
