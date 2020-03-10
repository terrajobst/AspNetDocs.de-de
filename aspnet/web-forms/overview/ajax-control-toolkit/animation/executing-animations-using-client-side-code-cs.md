---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Ausführen von Animationen mit Client seitigem Code (C#) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Animations Ausführung...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b6ba1553b9c8c51d5d6ae1679e53f9cc1d17b769
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484023"
---
# <a name="executing-animations-using-client-side-code-c"></a>Ausführen von Animationen mit clientseitigem Code (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Ausführung der Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code ausgelöst werden.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. Die Ausführung der Animation kann auch mit benutzerdefiniertem Client seitigem JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Fügen Sie zunächst den `ScriptManager` auf der Seite ein. Anschließend wird die ASP.NET AJAX-Bibliothek geladen, sodass Sie das steuerungstooltoolkit verwenden können:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Die Animation wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Definieren Sie in der zugeordneten CSS-Klasse für den Bereich eine schöne Hintergrundfarbe, und legen Sie außerdem eine festgelegte Breite für den Bereich fest:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Fügen Sie dann der Seite die `AnimationExtender` hinzu, und geben Sie dabei eine `ID`, das `TargetControlID`-Attribut und den obligatorischen `runat="server"`an:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

Verwenden Sie innerhalb des Knotens `<Animations>` `<OnClick>`, um die Animationen auszuführen, sobald der Benutzer auf den Bereich klickt. Fügen Sie zwei Animationen hinzu, die parallel ausgeführt werden:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Um dies zu demonstrieren, wird diese Animation (und alle anderen mit dem Steuerelement-Toolkit erstellten Animationen) mithilfe von JavaScript-Code ausgeführt, sobald die Seite ausgeführt wird. Zuerst benötigen wir Zugriff auf das `AnimationExtender`-Steuerelement. Die ASP.NET AJAX-Bibliothek stellt die `$find()`-Funktion für diese Aufgabe bereit:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

Das `AnimationExtender`-Steuerelement stellt eine umfangreiche API bereit, einschließlich Methoden, deren Namen mit den im XML-Markup verwendeten Ereignis Handlern identisch sind: `OnClick()`, `OnLoad()`usw. Beispielsweise führt ein Aufrufder `OnClick()`-Methode die Animation innerhalb des `<OnClick>`-Elements des `AnimationExtender`-Steuer Elements aus:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Im folgenden finden Sie den vollständigen Client seitigen JavaScript-Code, der das Klicken auf den Bereich emuliert, sobald die Seite vollständig geladen wurde. Beachten Sie, dass der `pageLoad()` Funktionsname verwendet wird, der von ASP.NET AJAX aufgerufen wird, nachdem die Seite und alle enthaltenen JavaScript-Bibliotheken geladen wurden.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]

[![die Animation sofort ausgeführt wird, ohne einen Mausklick](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Die Animation wird sofort ohne Mausklick ausgeführt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](executing-animations-using-client-side-code-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](modifying-animations-from-the-server-side-cs.md)
> [Weiter](changing-an-animation-using-client-side-code-cs.md)
