---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Positionieren eines ModalPopup-C#() | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Das Steuerelement bietet jedoch keine...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496917"
---
# <a name="positioning-a-modalpopup-c"></a>Positionieren eines ModalPopup-Steuerelements (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Das-Steuerelement bietet jedoch keine integrierte Funktionalität zum Positionieren des Popups.

## <a name="overview"></a>Übersicht

Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Das-Steuerelement bietet jedoch keine integrierte Funktionalität zum Positionieren des Popups.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Control Toolkit zu aktivieren, wird der `ScriptManager`. das Steuerelement muss an beliebiger Stelle auf der Seite abgelegt werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Fügen Sie als nächstes ein Panel hinzu, das als modales Popup fungiert. Zum Schließen des Popups wird eine Schaltfläche verwendet:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Wenn das Popup Fenster angezeigt wird, muss es an einer bestimmten Stelle auf der Seite positioniert werden. Für diese Aufgabe wird eine Client seitige JavaScript-Funktion erstellt. Zuerst wird versucht, auf den Bereich zuzugreifen. Wenn der Vorgang erfolgreich ist, wird die Position des Panels mithilfe von CSS und JavaScript festgelegt (ändern Sie die Position des Popups bei wird). Das `ModalPopupExtender`-Steuerelement versucht jedoch auch, das Popup Fenster zu positionieren. Aus diesem Grund positioniert der JavaScript-Code jedes Zehntel einer Sekunde wiederholt das Popup.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Wie Sie sehen, wird der Rückgabewert der JavaScript-Methode `setTimeout()` in einer globalen Variablen gespeichert. Dadurch kann die wiederholte Positionierung des Popup Bedarfs bei Bedarf mit der `clearTimeout()`-Methode beendet werden:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Nun müssen Sie nur noch den Browser zum Abrufen dieser Funktionen verwenden, wenn dies angemessen ist. Die `movePanel()` JavaScript-Funktion muss aufgerufen werden, wenn auf die Schaltfläche geklickt wird, mit der das Panel ausgelöst wird:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

Wenn das Popup Fenster geschlossen wird, wird die `stopMoving()` Funktion wiedergegeben, die im `ModalPopupExtender` Steuerelement ausgelöst werden kann:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

[![das modale Popup an der angegebenen Position angezeigt wird.](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Das modale Popup Fenster wird an der angegebenen Position angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](positioning-a-modalpopup-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](handling-postbacks-from-a-modalpopup-cs.md)
> [Weiter](launching-a-modal-popup-window-from-server-code-vb.md)
