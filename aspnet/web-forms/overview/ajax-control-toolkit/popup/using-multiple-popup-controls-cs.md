---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Verwenden von mehreren Popup SteuerC#Elementen () | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8700fe89af591e8b481e853580b0efa0cddbf1bc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496425"
---
# <a name="using-multiple-popup-controls-c"></a>Verwenden von mehreren Popupsteuerelementen (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, mehr als ein Popup-Steuerelement auf einer Seite zu verwenden.

## <a name="overview"></a>Übersicht

Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es ist auch möglich, mehr als ein Popup-Steuerelement auf einer Seite zu verwenden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Fügen Sie als nächstes ein Panel hinzu, das als Popup fungiert. Im aktuellen Szenario enthält der Bereich ein `Calendar` Steuerelement. Um die Seiten Aktualisierungen zu vermeiden, die durch die Postbacks des Kalenders verursacht werden, wird der Bereich in einem `UpdatePanel` Steuerelement abgelegt:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

Die Seite enthält auch zwei Textfelder. Für jedes Textfeld wird das Kalender Popup angezeigt, sobald das Textfeld aktiviert ist.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Erweitern Sie nun die beiden Textfelder mit einem `PopupControlExtender`. Das `TargetControlID`-Attribut stellt die ID des Steuer Elements bereit, das an den Extender gebunden ist. Das `PopupControlID`-Attribut enthält die ID des Popup Panels. In diesem Fall zeigen beide Extender denselben Bereich an, aber auch unterschiedliche Bereiche sind möglich.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Wenn Sie nun in ein Textfeld klicken, wird unter dem Feld ein Kalender angezeigt, in dem Sie ein Datum auswählen können. (Das ausgewählte Datum wieder in die Textfelder wird in einem anderen Tutorial behandelt.)

[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-multiple-popup-controls-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Weiter](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
