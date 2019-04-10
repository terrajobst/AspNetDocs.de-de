---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Verwenden das Schieberegler-Steuerelement mit automatischem Postback (c#) | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, stellen die Schieberegler Autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: a5b858a05470caa244902afbb404adbb2e4761b7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59402725"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a>Verwenden das Schieberegler-Steuerelement mit automatischem Postback (c#)

durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.


## <a name="overview"></a>Übersicht

Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.

## <a name="steps"></a>Schritte

Um den Schieberegler automatisch postback auf eine Änderung vornehmen, benötigen beide Felder das Attribut `AutoPostBack="true"`: Das Textfeld, das den Schieberegler selbst werden soll, und das Textfeld, das den Schieberegler auf die Position enthält. Hier ist das erforderliche Markup für diese:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Die `SliderExtender` Steuerelement von ASP.NET AJAX Control Toolkit weist die beiden Textfelder die Schieberegler-Funktionalität:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Zusätzliche Label-Element wird später verwendet werden, um die Benutzer eines Postbacks zu informieren:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Zum Schluss die `ScriptManager` Steuerelement von ASP.NET AJAX lädt die erforderliche JavaScript für das Steuerelement-Toolkit funktioniert:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Nachdem Sie der Schieberegler wieder veröffentlicht; Dieses Ereignis kann abgefangen und eine Aktion ausgeführt werden, auf der Serverseite:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![MOving des Schiebereglers löst einen Postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Verschieben des Schiebereglers einen Postback auslöst ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![AFterwards, ist das Datum der Änderung in die Bezeichnung geschrieben](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Anschließend wird das Datum der Änderung in die Bezeichnung geschrieben ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Weiter](databinding-the-slider-control-cs.md)
