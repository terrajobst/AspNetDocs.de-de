---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Anpassen des Z-Index eines DropShadow (C#) | Microsoft-Dokumentation
author: wenz
description: Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Dieser Schatten steht jedoch manchmal in Konflikt mit anderen Steuerelementen, für Insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574305"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Anpassen des Z-Index eines DropShadow-Steuerelements (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Dieser Schatten verursacht jedoch manchmal einen Konflikt mit anderen Steuerelementen, z. ASP.net. dem Menü Steuerelement. Wenn ein Menüeintrag angezeigt wird, wird er hinter dem Ablage Schatten angezeigt.

## <a name="overview"></a>Übersicht über

Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Dieser Schatten verursacht jedoch manchmal einen Konflikt mit anderen Steuerelementen, z. ASP.net. dem Menü Steuerelement. Wenn ein Menüeintrag angezeigt wird, wird er hinter dem Ablage Schatten angezeigt.

## <a name="steps"></a>Schritte

Der Code beginnt mit dem Panel selbst und enthält ausreichend Text, sodass der Bereich ausreichend Text enthält, damit der Effekt sichtbar ist:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Ein anderes Panel wird direkt vor dem `panelShadow` Panel platziert. Sie enthält ein Menü mit horizontaler Ausrichtung, sodass Menüeinträge über (oder eher unter) dem `dropShadow` Panel) angezeigt werden:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Anschließend wird der `DropShadowExtender` hinzugefügt, um den `panelShadow` Panel mit einem Schlag Schatteneffekt zu erweitern:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Schließlich ermöglicht das ASP.NET AJAX `ScriptManager`-Steuerelement, dass das Steuerelement-Toolkit funktioniert:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Wenn Sie dieses Skript ausführen, werden die Menüeinträge unterhalb des Bereichs angezeigt. Das Menü verwendet jedoch die CSS-Klasse `panel`, in der Sie lediglich zwei Dinge definieren müssen, damit Elemente vor dem anderen Panel angezeigt werden:

- Relative Positionierung
- Ein positiver z-index

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Das `DropShadowExtender` Steuerelement kann dann nicht mehr mit dem Menü Steuerelement in Konflikt stehen.

[![vor: der Menüeintrag ist nicht sichtbar.](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Vor: der Menüeintrag ist nicht sichtbar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png)).

[![nach: der Menüeintrag wird angezeigt.](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Nach: der Menüeintrag wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png)).

> [!div class="step-by-step"]
> [Nächste](manipulating-dropshadow-properties-from-client-code-cs.md)
