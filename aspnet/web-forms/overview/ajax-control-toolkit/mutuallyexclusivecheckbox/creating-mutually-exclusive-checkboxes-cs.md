---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (c#) | Microsoft-Dokumentation
author: wenz
description: 'Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet. Es gibt jedoch ein Nachteil: Sobald ein Optionsfeld in einer Gruppe ausgewählt ist...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d80771081a7aefefa115e68827c52092c6559bb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57052597"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (C#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet. Es gibt jedoch ein Nachteil: Sobald in einer Gruppe ein Optionsfeld ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktiviert. Kontrollkästchen kann jederzeit deaktiviert werden, jedoch sind nicht gegenseitig. In diesem Tutorial werden die Vorteile beider Ansätze: Kontrollkästchen, die sich gegenseitig.


## <a name="overview"></a>Übersicht

Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet. Es gibt jedoch ein Nachteil: Sobald in einer Gruppe ein Optionsfeld ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktiviert. Kontrollkästchen kann jederzeit deaktiviert werden, jedoch sind nicht gegenseitig. In diesem Tutorial werden die Vorteile beider Ansätze: Kontrollkästchen, die sich gegenseitig.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX Control Toolkit enthält die MutuallyExclusiveCheckBox-Extender. Programmierer, der Name einer Gruppe alle Kontrollkästchen zuweisen können (`Key` Attribut). Aus alle Kontrollkästchen in der gleichen Gruppe kann nur eine gleichzeitig ausgewählt werden.

Wir beginnen mit zwei Kontrollkästchen in eine neue ASP.NET-Seite zu platzieren. Es können mehrere, aber zwei reicht aus, um das Prinzip zu demonstrieren:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Für beide Kontrollkästchen muss ein MutuallyExclusiveCheckBoxExtender-Steuerelement auf der Seite eingefügt werden. Beide Schlüsselattribute müssen den gleichen Wert wie der Wert sich Attribute eines HTML-Radio Button-Elemente befinden, müssen identisch mit die Gruppe zu bezeichnen, in der sie angehören. Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Darüber hinaus enthalten die ASP.NET AJAX `ScriptManager` die alle Elemente des ASP.NET AJAX Control Toolkit erforderlich ist:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Speichern Sie, und führen Sie die Seite: Sie aktivieren und deaktivieren Sie beide Kontrollkästchen, können jedoch zu keinem Zeitpunkt beide Kontrollkästchen geprüft werden kann.


[![Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](creating-mutually-exclusive-checkboxes-vb.md)
