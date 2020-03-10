---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Ausfüllen einer Liste mit CascadingDropDown (C#) | Microsoft-Dokumentation
author: wenz
description: Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList zugeordnete Werte in Anoth laden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: b5e9874fb5b6d3e55c8af5b85d12bf1ffacc116b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497763"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a>Ausfüllen einer Liste mit CascadingDropDown (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Die erste Herausforderung besteht darin, eine Dropdown Liste mit diesem Steuerelement auszufüllen.

## <a name="overview"></a>Übersicht

Das CascadingDropDown-Steuerelement im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer Dropdown List zugeordnete Werte in eine andere Dropdown List laden. (Eine Liste enthält beispielsweise eine Liste der US-Bundesstaaten, und die nächste Liste wird dann mit den wichtigsten Städten in diesem Zustand aufgefüllt.) Die erste Herausforderung besteht darin, eine Dropdown Liste mit diesem Steuerelement auszufüllen.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und dem Steuerelement-Toolkit zu aktivieren, muss das `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden (innerhalb des `<form>` Elements):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Anschließend ist ein Dropdown List-Steuerelement erforderlich:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown-Extender hinzugefügt. Er sendet eine asynchrone Anforderung an einen Webdienst, der dann eine Liste von Einträgen zurückgibt, die in der Liste angezeigt werden sollen. Damit dies funktioniert, müssen die folgenden CascadingDropDown-Attribute festgelegt werden:

- `ServicePath`: URL eines Webdiensts, der die Listeneinträge bereitgestellt
- `ServiceMethod`: Webmethode, die die Listeneinträge bereitgestellt
- `TargetControlID`: ID der Dropdown Liste
- `Category`: Kategorieinformationen, die beim Aufrufen an die Webmethode übermittelt werden.
- `PromptText`: Text, der beim asynchronen Laden von Listen Daten vom Server angezeigt wird.

Hier ist das Markup für das `CascadingDropDown` Element. Der einzige Unterschied C# zwischen und VB ist der Name des zugeordneten Webdiensts:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Der JavaScript-Code, der vom `CascadingDropDown` Extender stammt, ruft eine Webdienst Methode mit der folgenden Signatur auf:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Der wichtigste Aspekt ist, dass die Methode ein Array vom Typ `CascadingDropDownNameValue` zurückgeben muss (definiert durch das ASP.NET AJAX Control Toolkit). Im `CascadingDropDownNameValue`-Konstruktor muss zuerst der Text des Listen Eintrags und dann der zugehörige Wert bereitgestellt werden, ebenso wie `<option value="VALUE">NAME</option>` in HTML. Im folgenden finden Sie einige Beispiel Daten:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Wenn Sie die Seite im Browser laden, wird die Liste mit drei Anbietern gefüllt.

[![die Liste automatisch aufgefüllt wird](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Die Liste wird automatisch ausgefüllt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](filling-a-list-using-cascadingdropdown-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Weiter](using-cascadingdropdown-with-a-database-cs.md)
