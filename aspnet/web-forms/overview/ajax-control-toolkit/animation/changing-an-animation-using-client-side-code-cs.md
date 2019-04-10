---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Ändern von Animationen mit clientseitigen Code (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es können auch die Animation...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bdee58aa04e1c8217c2a727b96aa8b239fe1aca
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395602"
---
# <a name="changing-an-animation-using-client-side-code-c"></a>Ändern von Animationen mit clientseitigem Code (C#)

durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Die tatsächliche Animation wird durch eine HTML-Schaltfläche gestartet:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Beachten Sie, dass es keine `<Animations>` Knoten innerhalb der `AnimationExtender` Steuerelement. Benutzerdefinierte JavaScript-Code wird verwendet, geben Sie die Animationen, die mit dem Steuerelement verwendet werden.

Wie bei der Server-API von `AnimationExtender`, es gibt keine einfache Möglichkeit, eine Animation für den Extender noch nicht zugewiesen. Mit den verschiedenen Ereignissen registriert jedoch der Extender zur Verfügung stellt mehrere Methoden zum Lesen und Schreiben von Animationen (`OnClick`, `OnLoad`und so weiter). Hier einige Beispiele:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Das Format des Rückgabewerts von der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine objektdarstellung, der das XML-Markup bereitstellen. Derzeit besteht keine Möglichkeit, ein Objekt übergeben, aber es ist möglich, ein Objekt aus einer angegebenen Animation lesen (`get_OnXXXBehavior()` Methoden).

Hier ist eine JSON-Zeichenfolge (ohne die begrenzenden Anführungszeichen und ordentlich), die eine Animation, die von der Schaltfläche ausgelöste darstellt, aber die Animation des Bereichs durch Ändern der Größe und gleichzeitig ausblenden:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Die folgende JavaScript-Code weist diese JSON-descripting auf die `OnClick` Animation des aktuellen Extenders und führt ihn:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![TIE-Animation sofort ausgeführt, ohne ein Mausklick (und mit sehr geringem Markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

Die Animation wird sofort ausgeführt, ohne ein Mausklick (und mit sehr geringem Markup) ([klicken Sie, um das Bild in voller Größe anzeigen](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](executing-animations-using-client-side-code-cs.md)
> [Weiter](animating-an-updatepanel-control-cs.md)
