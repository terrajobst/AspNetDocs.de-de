---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Verarbeiten von Postbacks über ein Popup Steuerelement mit einem Update Panel (VB) | Microsoft-Dokumentation
author: wenz
description: Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es muss besonders darauf geachtet werden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: dd045ae56696c7944df98cf805ba812fde1bb4ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496635"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Verarbeiten von Postbacks über ein Popupsteuerelement mit einem UpdatePanel-Steuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es muss besonders darauf geachtet werden, dass ein Postback innerhalb eines solchen Popups erfolgt.

## <a name="overview"></a>Übersicht

Der popupcontrol-Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup aufzurufende, wenn ein beliebiges anderes Steuerelement aktiviert ist. Es muss besonders darauf geachtet werden, dass ein Postback innerhalb eines solchen Popups erfolgt.

## <a name="steps"></a>Schritte

Wenn eine `PopupControl` mit einem Postback verwendet wird, kann ein `UpdatePanel` die durch das Postback verursachte Seiten Aktualisierung verhindern. Das folgende Markup definiert einige wichtige Elemente:

- Ein `ScriptManager`-Steuerelement, sodass das ASP.NET AJAX Control Toolkit funktioniert.
- Zwei `TextBox` Steuerelemente, die beide ein Popup-Element auslöst
- Ein `Panel` Steuerelement, das als Popup fungiert.
- Innerhalb des Panels ist ein `Calendar`-Steuerelement in ein `UpdatePanel` Steuerelement eingebettet.
- Zwei `PopupControlExtender` Steuerelemente, die den Bereich den Textfeldern zuweisen

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Beachten Sie, dass das `OnSelectionChanged`-Attribut des `Calendar` Steuer Elements festgelegt ist. Wenn der Benutzer also ein Datum im Kalender auswählt, erfolgt ein Postback, und die serverseitige Methode `c1_SelectionChanged()` wird ausgeführt. Innerhalb dieser Methode muss das aktuelle Datum abgerufen und in das Textfeld zurückgeschrieben werden.

Die Syntax für lautet wie folgt: Zunächst muss ein Proxy Objekt für die `PopupControlExtender` auf der Seite generiert werden. Das ASP.NET AJAX Control Toolkit bietet die `GetProxyForCurrentPopup()`-Methode. Das von dieser Methode zurückgegebene Objekt unterstützt die `Commit()`-Methode, die einen Wert zurück an das Steuerelement sendet, das das Popup ausgelöst hat (nicht das Steuerelement, das den Methoden Befehl ausgelöst hat!). Der folgende Code stellt das ausgewählte Datum als Argument für die `Commit()`-Methode bereit und bewirkt, dass der Code das ausgewählte Datum wieder in das Textfeld schreibt:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Wenn Sie nun auf ein Kalenderdatum klicken, wird das ausgewählte Datum im zugeordneten Textfeld angezeigt, und es wird ein Datumsauswahl-Steuerelement erstellt, das sich derzeit auf vielen Websites befindet.

[![der Kalender angezeigt wird, wenn der Benutzer auf das Textfeld klickt.](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer auf das Textfeld klickt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png)).

[Wenn Sie ![auf ein Datum klicken, wird es in das Textfeld eingefügt.](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Wenn Sie auf ein Datum klicken, wird es in das Textfeld eingefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png)).

> [!div class="step-by-step"]
> [Zurück](using-multiple-popup-controls-vb.md)
> [Weiter](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
