---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Ändern einer Animation mit Client seitigem Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animation kann auch...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cce0a5a901f71edd40eada59ac7eeba93222e2b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606928"
---
# <a name="changing-an-animation-using-client-side-code-vb"></a>Ändern von Animationen mit clientseitigem Code (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code geändert werden.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code geändert werden.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Die eigentliche Animation wird von einer HTML-Schaltfläche gestartet:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Beachten Sie, dass es innerhalb des `AnimationExtender` Steuer Elements keinen `<Animations>` Knoten gibt. Benutzerdefinierter JavaScript-Code wird verwendet, um die Animationen bereitzustellen, die mit dem Steuerelement verwendet werden sollen.

Wie bei der Server-API von `AnimationExtender`gibt es keine einfache Möglichkeit, dem Extender eine Animation zuzuweisen. Der Extender stellt jedoch mehrere Methoden zum Lesen und Schreiben von Animationen zur Verfügung, die mit den verschiedenen Ereignissen registriert sind (`OnClick`, `OnLoad`usw.). Hier einige Beispiele:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Das Format des Rückgabewerts der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine Objektdarstellung des XML-Markups bereitstellt. Derzeit gibt es keine Möglichkeit, ein Objekt in zu übergeben, aber es ist möglich, ein Objekt aus einer bestimmten Animation (`get_OnXXXBehavior()` Methoden) zu lesen.

Im folgenden finden Sie eine JSON-Zeichenfolge (ohne Anführungszeichen und formatierte Anführungszeichen), die eine von der Schaltfläche ausgelöste Animation darstellen, aber eine Animation des Panels durch Ändern der Größe und Ausblenden der Option zur gleichen Zeit ausführen:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Der folgende JavaScript-Code weist diese JSON-Beschreibung der `OnClick` Animation des aktuellen Extenders zu und führt Sie aus:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]

[![die Animation sofort ausgeführt wird, ohne einen Mausklick (und mit sehr geringem Markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit sehr geringem Markup) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](changing-an-animation-using-client-side-code-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](executing-animations-using-client-side-code-vb.md)
> [Weiter](animating-an-updatepanel-control-vb.md)
