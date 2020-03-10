---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Vorab festlegen von Listeneinträgen mit CascadingDropDown (VB) | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483807"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Vorheriges Festlegen von Listeneinträgen mit CascadingDropDown (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. Mit etwas Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.

## <a name="overview"></a>Übersicht

Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Mit etwas Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Anschließend ist ein Dropdown List-Steuerelement erforderlich:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt, der Webdienst-URL und Methoden Informationen bereitstellt:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Der "CascadingDropDown"-Extender ruft anschließend asynchron einen Webdienst mit der folgenden Methoden Signatur auf:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Die-Methode gibt ein Array vom Typ CascadingDropDown-Wert zurück. Der Konstruktor des Typs erwartet zuerst die Beschriftung des Listen Eintrags und dann den Wert (HTML `value`-Attribut). Wenn das dritte Argument auf true festgelegt ist, wird das List-Element automatisch im Browser ausgewählt.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Wenn Sie die Seite im Browser laden, wird die Dropdown Liste mit drei Anbietern aufgefüllt, die zweite ist vorab ausgewählt.

[![die Liste ausgefüllt und automatisch vorab ausgewählt wird.](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

Die Liste wird automatisch ausgefüllt und vorab ausgewählt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](using-cascadingdropdown-with-a-database-vb.md)
> [Weiter](using-auto-postback-with-cascadingdropdown-vb.md)
