---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Verwenden des Schieberegler-Steuer Elements mit automatischemC#Postback () | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, den Schieberegler wiederherzustellen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78445827"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a>Verwenden des Schieberegler-Steuer Elements mit automatischemC#Postback ()

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, den Schieberegler Auto Post Back vorzunehmen, sobald sich der Wert ändert.

## <a name="overview"></a>Übersicht

Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafischen Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, den Schieberegler Auto Post Back vorzunehmen, sobald sich der Wert ändert.

## <a name="steps"></a>Schritte

Damit der Schieberegler bei einer Änderung automatisch Postback wird, benötigen beide Textfelder das-Attribut `AutoPostBack="true"`: das Textfeld, das zum Schieberegler wird, und das Textfeld, das die Position des Schiebereglers enthält. Dies ist das erforderliche Markup für dieses:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Das `SliderExtender`-Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit weist den beiden Textfeldern die Schieberegler-Funktionalität zu:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Ein zusätzliches Label-Element wird später verwendet, um den Benutzer über ein Postback zu informieren:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Zum Schluss lädt das `ScriptManager`-Steuerelement von ASP.NET AJAX das erforderliche JavaScript, damit das Steuerelement-Toolkit funktioniert:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Der Schieberegler wird jetzt zurück gepostet. auf der Serverseite kann dieses Ereignis abgefangen und befolgt werden:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

[![Verschieben des Schiebereglers löst ein Postback aus.](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Durch Verschieben des Schiebereglers wird ein Postback ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-slider-control-with-auto-postback-cs/_static/image3.png))

[![anschließend wird das Datum dieser Änderung in der Bezeichnung geschrieben.](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Anschließend wird das Datum dieser Änderung in der Bezeichnung geschrieben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-the-slider-control-with-auto-postback-cs/_static/image6.png)).

> [!div class="step-by-step"]
> [Weiter](databinding-the-slider-control-cs.md)
