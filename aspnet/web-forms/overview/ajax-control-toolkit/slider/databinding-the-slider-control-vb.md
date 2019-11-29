---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Datenbindung des Schieberegler-Steuer Elements (VB) | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, die aktuelle Positio zu binden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14373bdfdead9916950b8a1cf61f427f7ba50b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598296"
---
# <a name="databinding-the-slider-control-vb"></a>Datenbindung des Schieberegler-Steuerelements (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.

## <a name="overview"></a>Übersicht über

Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Fügen Sie dann der Seite zwei `TextBox`-Steuerelemente hinzu. Eine wird in einen grafischen Schieberegler transformiert, und die andere wird die Position des Schiebereglers enthalten.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Der nächste Schritt ist bereits der letzte Schritt. Mit dem `SliderExtender`-Steuerelement aus dem ASP.NET AJAX Control Toolkit wird ein Schieberegler aus dem ersten Textfeld entfernt. das zweite Textfeld wird automatisch aktualisiert, wenn sich die Position des Schiebereglers ändert. Damit dies funktioniert, muss das `TargetControlID` Attribut des `SliderExtender`auf die ID des ersten Textfelds festgelegt werden. Das `BoundControlID`-Attribut muss auf die ID des zweiten Textfelds festgelegt werden.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Wie Sie im Browser sehen können, funktioniert die Datenbindung in beide Richtungen: Wenn Sie einen neuen Wert in das Textfeld eingeben, wird die Position des Schiebereglers aktualisiert. Wenn Sie das zweite Textfeld schreibgeschützt machen, können Sie dem Textfeld einen schwachen Schutz hinzufügen, damit der Benutzer den Wert in dort nicht manuell aktualisieren kann.

[![Schieberegler und Textfeld sind synchron.](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Schieberegler und Textfeld sind synchron ([Klicken Sie, um das Bild in voller Größe anzuzeigen](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorheriges](using-the-slider-control-with-auto-postback-vb.md)
