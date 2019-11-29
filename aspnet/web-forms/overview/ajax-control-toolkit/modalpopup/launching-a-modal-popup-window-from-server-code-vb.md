---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Starten eines modalen Popup Fensters über den Server Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. Einige Szenarien erfordern jedoch, dass "t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606590"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a>Starten eines modalen Popupfensters über den Servercode (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. In einigen Szenarien ist es jedoch erforderlich, dass das modale Popup auf Serverseite ausgelöst wird.

## <a name="overview"></a>Übersicht über

Das ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit zum Erstellen eines modalen Popups mithilfe Client seitiger Mittel. In einigen Szenarien ist es jedoch erforderlich, dass das modale Popup auf Serverseite ausgelöst wird.

## <a name="steps"></a>Schritte

Zunächst einmal ist ein ASP.NET Button-websteuer Element erforderlich, um zu veranschaulichen, wie das ModalPopup-Steuerelement funktioniert. Fügen Sie eine solche Schaltfläche innerhalb des &lt;Formulars&gt;-Element auf einer neuen Seite hinzu:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Anschließend benötigen Sie das Markup für das Popup, das Sie erstellen möchten. Definieren Sie es als `<asp:Panel>` Steuerelement, und stellen Sie sicher, dass es ein Schaltflächen-Steuerelement enthält. Das ModalPopup-Steuerelement bietet die Funktionen, mit denen eine solche Schaltfläche das Popup schließen soll. Andernfalls gibt es keine einfache Möglichkeit, diese zu verschwinden.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Fügen Sie anschließend der Seite das ModalPopup-Steuerelement aus dem ASP.NET AJAX-Toolkit hinzu. Legen Sie die Eigenschaften für die Schaltfläche fest, mit der das Steuerelement geladen wird, die Schaltfläche, die Sie entfernt, und die ID des eigentlichen Popups.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Wie bei allen Webseiten, die auf ASP.NET AJAX basieren; der Skript-Manager muss die erforderlichen JavaScript-Bibliotheken für die verschiedenen Ziel Browser laden:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Führen Sie das Beispiel im Browser aus. Wenn Sie auf die Schaltfläche klicken, wird das modale Popup Fenster angezeigt. Um denselben Effekt mithilfe von Server seitigem Code zu erzielen, ist eine neue Schaltfläche erforderlich:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Wie Sie sehen können, wird durch Klicken auf die Schaltfläche ein Postback generiert und die `ServerButton_Click()` Methode auf dem Server ausgeführt. In dieser Methode wird eine JavaScript-Funktion mit dem Namen `launchModal()` ausgeführt, um genau zu sein. die JavaScript-Funktion wird ausgeführt, nachdem die Seite geladen wurde:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Der Auftrag `launchModal()` ist das Anzeigen von ModalPopup. Die `launchModal()`-Funktion wird ausgeführt, nachdem die vollständige HTML-Seite geladen wurde. Zu diesem Zeitpunkt wurde das ASP.NET AJAX-Framework jedoch noch nicht vollständig geladen. Daher legt die `launchModal()`-Funktion nur eine Variable fest, die das ModalPopup-Steuerelement zu einem späteren Zeitpunkt anzeigen muss:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

Die `pageLoad()` JavaScript-Funktion ist eine spezielle Funktion, die ausgeführt wird, nachdem ASP.NET AJAX vollständig geladen wurde. Daher fügen wir dieser Funktion Code hinzu, um das ModalPopup-Steuerelement anzuzeigen. Dies gilt jedoch nur, wenn `launchModal()` vor aufgerufen wurde:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

Die `$find()`-Funktion sucht nach einem benannten Element auf der Seite und erwartet die serverseitige ID als Parameter. Daher gibt `$find("mpe")` die Client Darstellung des ModalPopup-Steuer Elements zurück. mit der `show()`-Methode kann das Popup Fenster angezeigt werden.

[![das modale Popup Fenster angezeigt wird, wenn auf eine der Schaltflächen geklickt wird.](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Das modale Popup Fenster wird angezeigt, wenn auf eine der Schaltflächen geklickt wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](positioning-a-modalpopup-cs.md)
> [Weiter](using-modalpopup-with-a-repeater-control-vb.md)
