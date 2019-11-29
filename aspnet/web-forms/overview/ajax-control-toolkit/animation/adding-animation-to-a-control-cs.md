---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Hinzufügen von Animationen zu einemC#Steuerelement () | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: dd63157fe616c5f6874b7cca11f4ede15018df04
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607130"
---
# <a name="adding-animation-to-a-control-c"></a>Hinzufügen von Animationen zu einem Steuerelement (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.

## <a name="overview"></a>Übersicht über

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.

## <a name="steps"></a>Schritte

Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Als nächstes benötigen wir die `AnimationExtender`. Nachdem Sie eine `ID` und die übliche `runat="server"`bereitgestellt haben, muss das `TargetControlID`-Attribut auf das-Steuerelement festgelegt werden, um in unserem Fall den Bereich zu animieren:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Die gesamte Animation wird deklarativ mithilfe einer XML-Syntax angewendet, die leider nicht vollständig von Visual Studio IntelliSense unterstützt wird. Der Stamm Knoten ist innerhalb dieses Knotens `<Animations>;`. es sind mehrere Ereignisse zulässig, die bestimmen, wann die Animation (n) stattfindet:

- `OnClick` (Maus Klick)
- `OnHoverOut` (wenn die Maus ein Steuerelement verlässt)
- `OnHoverOver` (wenn mit der Maus auf ein Steuerelement gezeigt wird, wird die `OnHoverOut` Animation angehalten)
- `OnLoad` (wenn die Seite geladen wurde)
- `OnMouseOut` (wenn die Maus ein Steuerelement verlässt)
- `OnMouseOver` (wenn mit der Maus auf ein Steuerelement gezeigt wird, wobei die `OnMouseOut` Animation nicht angehalten wird)

Das Framework enthält eine Reihe von Animationen, die jeweils durch ein eigenes XML-Element dargestellt werden. Hier finden Sie eine Auswahl:

- `<Color>` (Ändern einer Farbe)
- `<FadeIn>` (ausblenden)
- `<FadeOut>` (ausblenden)
- `<Property>` (Ändern der-Eigenschaft eines Steuer Elements)
- `<Pulse>` (pulsierende)
- `<Resize>` (Ändern der Größe)
- `<Scale>` (proportional zum Ändern der Größe)

In diesem Beispiel muss der Bereich ausgeblendet werden. Die Animation muss 1,5 Sekunden (`Duration`-Attribut) annehmen und 24 Frames (Animations Schritte) pro Sekunde (`Fps` Attribut) anzeigen. Hier ist das komplette Markup für das `AnimationExtender`-Steuerelement:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird der Bereich innerhalb von 1 bis halbe Sekunden angezeigt und ausgeblendet.

[![der Bereich ausgeblendet wird](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Der Bereich wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-animation-to-a-control-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Nächste](executing-several-animations-at-the-same-time-cs.md)
