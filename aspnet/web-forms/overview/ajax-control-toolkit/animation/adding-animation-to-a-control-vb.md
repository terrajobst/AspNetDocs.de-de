---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Hinzufügen von Animationen zu einem Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484125"
---
# <a name="adding-animation-to-a-control-vb"></a>Hinzufügen von Animationen zu einem Steuerelement (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.

## <a name="overview"></a>Übersicht

Das Animations Steuerelement im ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, sondern ein ganzes Framework zum Hinzufügen von Animationen zu einem Steuerelement. In diesem Tutorial wird gezeigt, wie eine solche Animation eingerichtet wird.

## <a name="steps"></a>Schritte

Der erste Schritt besteht darin, die `ScriptManager` auf der Seite einzuschließen, sodass die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf einen Textbereich angewendet, der wie folgt aussieht:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Als nächstes benötigen wir die `AnimationExtender`. Nachdem Sie eine `ID` und die übliche `runat="server"`bereitgestellt haben, muss das `TargetControlID`-Attribut auf das-Steuerelement festgelegt werden, um in unserem Fall den Bereich zu animieren:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

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

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird der Bereich innerhalb von 1 bis halbe Sekunden angezeigt und ausgeblendet.

[![der Bereich ausgeblendet wird](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Der Bereich wird ausgeblendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-animation-to-a-control-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](dynamically-controlling-updatepanel-animations-cs.md)
> [Weiter](executing-several-animations-at-the-same-time-vb.md)
