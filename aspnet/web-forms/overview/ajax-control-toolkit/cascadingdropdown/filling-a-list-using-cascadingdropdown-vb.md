---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Ausfüllen einer Liste mit CascadingDropDown (VB) | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599601"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a>Ausfüllen einer Liste mit CascadingDropDown (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Die erste Herausforderung besteht darin, eine Dropdown Liste mit diesem Steuerelement auszufüllen.

## <a name="overview"></a>Übersicht über

Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Die erste Herausforderung besteht darin, eine Dropdown Liste mit diesem Steuerelement auszufüllen.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Anschließend ist ein Dropdown List-Steuerelement erforderlich:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt. Er sendet eine asynchrone Anforderung an einen Webdienst, der dann eine Liste von Einträgen zurückgibt, die in der Liste angezeigt werden sollen. Damit dies funktioniert, müssen die folgenden CascadingDropDown-Attribute festgelegt werden:

- `ServicePath`: URL eines Webdiensts, der die Listeneinträge bereitgestellt
- `ServiceMethod`: Webmethode, die die Listeneinträge bereitgestellt
- `TargetControlID`: ID der Dropdown Liste
- `Category`: Kategorieinformationen, die beim Aufrufen an die Webmethode übermittelt werden.
- `PromptText`: Text, der beim asynchronen Laden von Listen Daten vom Server angezeigt wird.

Hier ist das Markup für das `CascadingDropDown` Element. Der einzige Unterschied C# zwischen und VB ist der Name des zugeordneten Webdiensts:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Der JavaScript-Code, der vom `CascadingDropDown` Extender stammt, ruft eine Webdienst Methode mit der folgenden Signatur auf:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Der wichtigste Aspekt ist, dass die Methode ein Array vom Typ `CascadingDropDownNameValue` zurückgeben muss (definiert durch das ASP.NET AJAX Control Toolkit). Im `CascadingDropDownNameValue`-Konstruktor muss zuerst der Text des Listen Eintrags und dann der zugehörige Wert bereitgestellt werden, ebenso wie `<option value="VALUE">NAME</option>` in HTML. Im folgenden finden Sie einige Beispiel Daten:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Wenn Sie die Seite im Browser laden, wird die Liste mit drei Anbietern gefüllt.

[![die Liste automatisch aufgefüllt wird](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

Die Liste wird automatisch ausgefüllt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](filling-a-list-using-cascadingdropdown-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](using-auto-postback-with-cascadingdropdown-cs.md)
> [Weiter](using-cascadingdropdown-with-a-database-vb.md)
