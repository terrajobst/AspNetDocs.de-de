---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Ausführen mehrerer Animationen zur gleichen Zeit (C#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Er ermöglicht es, durch Fallenlassen ausführen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: f228ca600691ea4820ecdb79dea5e70054975f32
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58422947"
---
<a name="executing-several-animations-at-the-same-time-c"></a>Ausführen mehrerer Animationen zur gleichen Zeit (C#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Sie können mehrere Animationen auf eine Weise parallel ausgeführt.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Sie können mehrere Animationen auf eine Weise parallel ausgeführt.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation. Der Animation-Framework können Sie mehrere Animationen in einer mit join die `<Parallel>` Element. Alle Animationen in `<Parallel>` zur gleichen Zeit ausgeführt werden.

Hier ist die einem möglichen Markup für die `AnimationExtender` Steuerelement ausgeblendet wird, und Ändern der Größe des Bereichs zur gleichen Zeit:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

Und tatsächlich: Wenn Sie dieses Skript ausführen, wird im Bereich angezeigt wird, klicken Sie dann die Größe (mehr als verdreifacht Sie seine Breite sich und seine Höhe halbiert) und zur gleichen Zeit ausgeblendet wird.


[![Der Bereich ausgeblendet wird, und Ändern der Größe (einschließlich seines Inhalts Dank des Browsers Rendering-Engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Der Bereich ausgeblendet wird, und Ändern der Größe (einschließlich seines Inhalts Dank des Browsers Rendering-Engine) ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](adding-animation-to-a-control-cs.md)
> [Weiter](executing-several-animations-after-each-other-cs.md)
