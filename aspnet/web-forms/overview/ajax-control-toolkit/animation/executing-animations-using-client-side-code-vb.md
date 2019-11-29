---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Ausführen von Animationen mit Client seitigem Code (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animations Ausführung...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575473"
---
# <a name="executing-animations-using-client-side-code-vb"></a>Ausführen von Animationen mit clientseitigem Code (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Ausführung der Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code ausgelöst werden.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Ausführung der Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnClick>`, um die Animationen auszuführen, sobald der Benutzer auf den Bereich klickt. Fügen Sie zwei Animationen hinzu, die parallel ausgeführt werden:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Um dies zu demonstrieren, wird diese Animation (und alle anderen mit dem Steuerelement-Toolkit erstellten Animationen) mithilfe von JavaScript-Code ausgeführt, sobald die Seite ausgeführt wird. Zuerst benötigen wir Zugriff auf das `AnimationExtender`-Steuerelement. Die ASP.NET AJAX-Bibliothek stellt die `$find()`-Funktion für diese Aufgabe bereit:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

Das `AnimationExtender`-Steuerelement stellt eine umfangreiche API bereit, einschließlich Methoden, deren Namen mit den im XML-Markup verwendeten Ereignis Handlern identisch sind: `OnClick()`, `OnLoad()`usw. Beispielsweise führt ein Aufrufder `OnClick()`-Methode die Animation innerhalb des `<OnClick>`-Elements des `AnimationExtender`-Steuer Elements aus:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Im folgenden finden Sie den vollständigen Client seitigen JavaScript-Code, der das Klicken auf den Bereich emuliert, sobald die Seite vollständig geladen wurde. Beachten Sie, dass der `pageLoad()` Funktionsname verwendet wird, der von ASP.NET AJAX aufgerufen wird, nachdem die Seite und alle enthaltenen JavaScript-Bibliotheken geladen wurden.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

[![die Animation sofort ausgeführt wird, ohne einen Mausklick](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Die Animation wird sofort ohne Mausklick ausgeführt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-animations-using-client-side-code-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](modifying-animations-from-the-server-side-vb.md)
> [Weiter](changing-an-animation-using-client-side-code-vb.md)
