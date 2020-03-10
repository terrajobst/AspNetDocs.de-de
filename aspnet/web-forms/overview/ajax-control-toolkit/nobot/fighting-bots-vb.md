---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Bekämpfung von Bots (VB) | Microsoft-Dokumentation
author: wenz
description: Automatisierte Bots Verputzen Webprotokolle und andere Websites mit Spam und senden Kommentar Formulare ohne Benutzerinteraktion. Das NOBOT-Steuerelement im ASP.NET AJAX-con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509049"
---
# <a name="fighting-bots-vb"></a>Abwehren von Bots (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Automatisierte Bots Verputzen Webprotokolle und andere Websites mit Spam und senden Kommentar Formulare ohne Benutzerinteraktion. Das NOBOT-Steuerelement im ASP.NET AJAX Control Toolkit kann Ihnen helfen, diese Bots zu bekämpfen.

## <a name="overview"></a>Übersicht

Automatisierte Bots Verputzen Webprotokolle und andere Websites mit Spam und senden Kommentar Formulare ohne Benutzerinteraktion. Das NOBOT-Steuerelement im ASP.NET AJAX Control Toolkit kann Ihnen helfen, diese Bots zu bekämpfen.

## <a name="steps"></a>Schritte

Ein gängiger Ansatz zur Bekämpfung von Bots ist die Verwendung von Captchas vollständig automatisierten öffentlichen Tests, um Computer und Menschen voneinander zu informieren. Ein Turing-Test war ursprünglich ein Test, bei dem jemand entscheiden musste, ob es sich bei einem Kommunikationspartner um einen Menschen oder einen Computer handelt. Im Web besteht eine CAPTCHA in der Regel aus einem Bild mit einigen verzerrten Buchstaben. Die Idee ist, dass nur ein Mensch die Buchstaben auf dem Bild lesen kann, während die OCR-Algorithmen fehlschlagen.

Dieser Ansatz bietet mehrere vor-und Nachteile, aber eine Erörterung dieses Ansatzes geht über den Rahmen dieses Tutorials hinaus. Es gibt jedoch ein Steuerelement im ASP.NET AJAX Control Toolkit, das einen ähnlichen Ansatz bietet: `NoBot`. Sie ist einfacher zu überwinden als eine CAPTCHA, aber Sie ist sehr einfach zu verwenden und eignet sich hervorragend für Websites wie Blogs, bei denen es sich um einen Erfolg handelt, wenn die meisten Spam Versuche besiegt werden, was das `NoBot`-Steuerelement tun kann.

`NoBot` fängt das Postback des aktuellen ASP.net-Webformulars ab, wenn mindestens eine dieser Bedingungen erfüllt ist:

- Der Browser kann ein JavaScript-Rätsel nicht lösen (z.b. wenn JavaScript deaktiviert ist).
- Der Benutzer hat das Formular schnell übermittelt.
- Die Client-IP-Adresse hat das Formular zu häufig in einem bestimmten Zeitraum übermittelt.

Um diese Bedingungen zu überprüfen, benötigt das `NoBot` Steuerelement diese Attribute (alle optional):

- `ResponseMinimumDelaySeconds` minimale Anzahl von Sekunden zwischen Postbacks
- `CutoffWindowSeconds` Länge des Zeitintervalls, in dem Postbacks von einer IP Measures sind
- `CutoffMaximumInstances` maximale Anzahl von Sekunden pro Zeitintervall

Das folgende Markup erfordert, dass mindestens zwei Sekunden zwischen Postbacks Vergehen und dass nur fünf Postbacks oder weniger innerhalb eines Zeitraums von 30 Sekunden vorhanden sind:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Stellen Sie dann wie üblich sicher, dass die `ScriptManager` auf der Seite enthalten ist, damit die ASP.NET AJAX-Bibliothek geladen und das steuerungstooltoolkit verwendet werden kann:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Da die meisten der Überprüfungen `NoBot` auf der Serverseite ausgeführt werden, müssen Sie das Ergebnis dieser Überprüfungen überprüfen. Dies kann durch Aufrufen der `IsValid()`-Methode von `NoBot`erfolgen. Es verfügt über ein Argument (als `out` Parameter/`ByRef` Parameter), das vom Typ `NoBotState`ist. Die Zeichen folgen Darstellung enthält den Grund, in dem die Überprüfung fehlschlägt, und `Valid` andernfalls. Der folgende Code gibt eine Nachricht gemäß `NoBot`Ergebnis aus:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Zum Schluss benötigen Sie ein zu über mittelendes Formular und ein Bezeichnungs Element zum Ausgeben der Nachricht.

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Wenn Sie dieses Skript ausführen und JavaScript deaktivieren oder das Formular innerhalb der ersten zwei Sekunden übermitteln oder das Formular sieben Mal innerhalb von dreißig Sekunden übermitteln, erhalten Sie eine Fehlermeldung. Verwenden Sie dieses Steuerelement jedoch klug, da für nur etwa 90-95% der Benutzer JavaScript aktiviert ist. aus diesem Grund können 5-10% der Benutzer `NoBot`Tests nicht ausgeführt werden.

[![diese Fehlermeldung wurde möglicherweise von einem bot verursacht.](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Diese Fehlermeldung wurde möglicherweise von einem bot verursacht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](fighting-bots-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Previous](fighting-bots-cs.md)
