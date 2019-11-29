---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Verwenden von mehreren Popup Steuerelementen (VB) | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611604"
---
# <a name="using-multiple-popup-controls-vb"></a>Verwenden von mehreren Popupsteuerelementen (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, mehr als ein Popup-Steuerelement auf einer Seite zu verwenden.

## <a name="overview"></a>Übersicht über

Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, mehr als ein Popup-Steuerelement auf einer Seite zu verwenden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Fügen Sie als nächstes ein Panel hinzu, das als Popup fungiert. Im aktuellen Szenario enthält der Bereich ein `Calendar` Steuerelement. Um die Seiten Aktualisierungen zu vermeiden, die durch die Postbacks des Kalenders verursacht werden, wird der Bereich in einem `UpdatePanel` Steuerelement abgelegt:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Die Seite enthält auch zwei Textfelder. Für jedes Textfeld wird das Kalender Popup angezeigt, sobald das Textfeld aktiviert ist.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Erweitern Sie nun die beiden Textfelder mit einem `PopupControlExtender`. Das `TargetControlID`-Attribut stellt die ID des Steuer Elements bereit, das an den Extender gebunden ist. Das `PopupControlID`-Attribut enthält die ID des Popup Panels. In diesem Fall zeigen beide Extender denselben Bereich an, aber auch unterschiedliche Bereiche sind möglich.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Wenn Sie nun in ein Textfeld klicken, wird unter dem Feld ein Kalender angezeigt, in dem Sie ein Datum auswählen können. (Das ausgewählte Datum wieder in die Textfelder wird in einem anderen Tutorial behandelt.)

[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-multiple-popup-controls-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
