---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Verarbeiten von Postbacks über ein Popup Steuerelement ohne Update Panel (C#) | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Wenn ein Postback in "su..." auftritt
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496503"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Verarbeiten von Postbacks über ein Popupsteuerelement ohne ein UpdatePanel-Steuerelement (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Wenn ein Postback in einem solchen Bereich stattfindet und mehrere Bereiche auf der Seite vorhanden sind, ist es schwierig, zu ermitteln, auf welchen Panel geklickt wurde.

## <a name="overview"></a>Übersicht

Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Wenn ein Postback in einem solchen Bereich stattfindet und mehrere Bereiche auf der Seite vorhanden sind, ist es schwierig, zu ermitteln, auf welchen Panel geklickt wurde.

## <a name="steps"></a>Schritte

Wenn Sie ein `PopupControl` mit einem Postback verwenden, aber keine `UpdatePanel` auf der Seite haben, bietet das Steuerelement-Toolkit keine Möglichkeit, zu bestimmen, welches Client Element das Popup ausgelöst hat, das wiederum das Postback verursacht hat. Ein kleiner Trick bietet jedoch eine Problem Umgehung für dieses Szenario.

Im folgenden finden Sie zunächst die grundlegende Einrichtung: zwei Textfelder, die beide das gleiche Popup, einen Kalender, auslöst. Zwei `PopupControlExtenders` Textfelder und Popup zusammensetzen.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Die grundlegende Idee ist das Hinzufügen eines ausgeblendeten Formular Felds in der &lt;`form`&gt; Element, das das Textfeld enthält, das das Popup gestartet hat:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Wenn die Seite geladen wird, fügt JavaScript-Code beide Textfeldern einen Ereignishandler hinzu: Wenn der Benutzer auf ein Textfeld klickt, wird sein Name in das ausgeblendete Formularfeld geschrieben:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

Im serverseitigen Code muss der Wert des ausgeblendeten Felds gelesen werden. Da verborgene Formularfelder zu bearbeiten sind, ist ein Whitelist-Ansatz erforderlich, um den verborgenen Wert zu überprüfen. Nachdem das richtige Textfeld identifiziert wurde, wird das Datum aus dem Kalender in das Textfeld geschrieben.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png)).

[Wenn Sie ![auf ein Datum klicken, wird es in das Textfeld eingefügt.](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Wenn Sie auf ein Datum klicken, wird es in das Textfeld eingefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png)).

> [!div class="step-by-step"]
> [Zurück](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Weiter](using-multiple-popup-controls-vb.md)
