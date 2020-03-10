---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (C#) | Microsoft-Dokumentation
author: wenz
description: 'Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet. Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496785"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a>Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet. Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Options Felder zu deaktivieren. Kontrollkästchen können jederzeit deaktiviert werden, schließen sich jedoch nicht gegenseitig aus. Dieses Tutorial bietet das Beste aus beiden Ansätzen: Kontrollkästchen, die sich gegenseitig ausschließen.

## <a name="overview"></a>Übersicht

Wenn nur eine Reihe von Optionen ausgewählt werden kann, werden normalerweise Options Felder verwendet. Es gibt jedoch einen Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Options Felder zu deaktivieren. Kontrollkästchen können jederzeit deaktiviert werden, schließen sich jedoch nicht gegenseitig aus. Dieses Tutorial bietet das Beste aus beiden Ansätzen: Kontrollkästchen, die sich gegenseitig ausschließen.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX Control Toolkit enthält den MutuallyExclusiveCheckbox-Extender. Dies ermöglicht es Programmierern, einem Gruppennamen (`Key` Attribut) ein beliebiges Kontrollkästchen zuzuweisen. Aus allen Kontrollkästchen innerhalb derselben Gruppe kann jeweils nur eine ausgewählt werden.

Beginnen wir damit, zwei Kontrollkästchen auf einer neuen ASP.NET-Seite zu platzieren. Es können mehr, aber zwei davon ausreichen, um das Prinzip zu veranschaulichen:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Für beide Kontrollkästchen muss ein mutuallyexclusivecheckboxextender-Steuerelement auf der Seite abgelegt werden. Beide Schlüssel Attribute müssen denselben Wert aufweisen, ebenso wie die Wert Attribute von HTML-Optionsfeld Elementen identisch sein müssen, um die Gruppe anzugeben, zu der Sie gehören. Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Schließen Sie schließlich den ASP.NET AJAX-`ScriptManager` ein, der für alle Elemente des ASP.NET AJAX Control Toolkit erforderlich ist:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Speichern Sie die Seite, und führen Sie Sie aus: Sie können beide Kontrollkästchen aktivieren und deaktivieren. es können jedoch nicht beide Kontrollkästchen aktiviert werden.

[![jeweils nur ein Kontrollkästchen aktiviert werden kann.](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Es kann jeweils nur ein Kontrollkästchen geprüft werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-mutually-exclusive-checkboxes-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Weiter](creating-mutually-exclusive-checkboxes-vb.md)
