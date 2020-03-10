---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Deaktivieren von Aktionen während der AnimationC#() | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Sie unterstützt auch Aktionen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430953"
---
# <a name="disabling-actions-during-animation-c"></a>Deaktivieren von Aktionen während einer Animation (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Außerdem werden Aktionen wie Mausklicks unterstützt. Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Außerdem werden Aktionen wie Mausklicks unterstützt. Wenn jedoch mit einem Mausklick eine Animation gestartet wird, ist es wünschenswert, während der Animation Mausklicks zu deaktivieren.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Die Animation wird wie folgt auf eine HTML-Schaltfläche angewendet:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Beachten Sie, dass ein HTML-Steuerelement anstelle eines websteuer Elements verwendet wird, da es nicht gewünscht werden soll, dass die Schaltfläche ein Postback erstellt. die Client seitige Animation muss für uns einfach gestartet werden.

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

Innerhalb des `<Animations>` Knotens ist `<OnClick>` das Rechte Element, das mit dem Maus Klick behandelt werden soll. Während der Animation kann jedoch auch auf die Schaltfläche geklickt werden. Das `<EnableAction>`-Element kann dies berücksichtigen. Wenn Sie `Enabled="false"` festlegen, wird die Schaltfläche als Teil der Animation deaktiviert. Da wir mehrere einzelne Animationen verwenden (deaktivieren Sie die Schaltfläche und die eigentlichen Animationen), ist das `<Parallel>`-Element erforderlich, um die einzelnen Animationen miteinander zu verbinden. Hier ist das komplette Markup für `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Es ist auch möglich, die Schaltfläche nach der Animation erneut zu aktivieren, indem das folgende XML-Element am Ende der Liste verwendet wird:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Im vorliegenden Szenario wäre dies jedoch nutzlos, da die Schaltfläche ausgeblendet wird und am Ende der Animation nicht sichtbar ist.

[![die Schaltfläche deaktiviert ist, sobald die Animation ausgeführt wird](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](disabling-actions-during-animation-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](animating-in-response-to-user-interaction-cs.md)
> [Weiter](triggering-an-animation-in-another-control-cs.md)
