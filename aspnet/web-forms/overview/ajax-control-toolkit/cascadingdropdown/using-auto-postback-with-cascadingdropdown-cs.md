---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Verwenden von automatischem Postback mit CascadingDropDownC#() | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574484"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a>Verwenden von automatischem Postback mit CascadingDropDown (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. Wenn Sie jedoch das CascadingDropDown-Steuerelement verwenden, ASP. Das AutoPostBack-Feature des DropDownList-Steuer Elements von NET funktioniert nicht, da das asynchrone Laden von Daten in die Liste ein (unnötiges) Postback selbst generiert. Mit JavaScript-Code kann dieser Effekt vermieden werden.

## <a name="overview"></a>Übersicht über

Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Wenn Sie jedoch das CascadingDropDown-Steuerelement verwenden, ASP. Das AutoPostBack-Feature des DropDownList-Steuer Elements von NET funktioniert nicht, da das asynchrone Laden von Daten in die Liste ein (unnötiges) Postback selbst generiert. Mit JavaScript-Code kann dieser Effekt vermieden werden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des &lt;`form`&gt; Element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Anschließend ist ein Dropdown List-Steuerelement erforderlich:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt, der Webdienst-URL und Methoden Informationen bereitstellt:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Der "CascadingDropDown"-Extender ruft anschließend asynchron einen Webdienst mit der folgenden Methoden Signatur auf:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Die-Methode gibt ein Array vom Typ CascadingDropDown-Wert zurück. Der Konstruktor des Typs erwartet zuerst die Beschriftung des Listen Eintrags und dann den Wert (HTML `value`-Attribut).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Wenn Sie die Seite im Browser laden, wird die Dropdown Liste mit drei Anbietern aufgefüllt, die zweite ist vorab ausgewählt. Außerdem definiert ASP.net die `__doPostBack()` JavaScript-Methode. Nachdem die Seite geladen wurde, wird dieser JavaScript-Befehl der Dropdown Liste hinzugefügt, jedoch nur, wenn darin Elemente vorhanden sind. Wenn in der Liste keine Elemente vorhanden sind, lädt das steuerungstooltoolkit Sie zurzeit, sodass der JavaScript-Code ein Timeout verwendet und in einer halben Sekunde erneut versucht wird.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Auf diese Weise wird ein Postback nur ausgeführt, wenn tatsächlich Elemente in der Liste vorhanden sind und der Benutzer einen Eintrag auswählt.

[![auswählen eines Listen Elements verursacht ein Postback.](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Wenn Sie ein Listenelement auswählen, wird ein Postback ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Weiter](filling-a-list-using-cascadingdropdown-vb.md)
