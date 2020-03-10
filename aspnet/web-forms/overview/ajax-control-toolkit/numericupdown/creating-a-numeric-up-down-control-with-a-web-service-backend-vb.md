---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Erstellen eines numerischen Steuer Elements nach oben/unten mit einem Webdienst-Back-End (VB) | Microsoft-Dokumentation
author: wenz
description: Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann es sein, dass ein numerisches auf-/ab--Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) mehr als c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bf6e1b27180589d39e308de62b5be1f47fa8fe2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496719"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Erstellen einen numerischen UpAndDown-Steuerelements mit einem Webdienst-Back-End (VB)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden. Standardmäßig erhöht oder verringert das NumericUpDown-Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.

## <a name="overview"></a>Übersicht

Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden. Standardmäßig erhöht oder verringert das `NumericUpDown` Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX-Steuerelement-Toolkit enthält den `NumericUpDown` Extender, der automatisch zwei Schaltflächen zu einem Textfeld hinzufügt: eine zum Erhöhen des Werts, eine zum verringern. Das-Steuerelement unterstützt jedoch auch einen Webdienst-oder Seiten Methodenaufrufe. Wenn Sie auf die Schaltfläche nach oben oder nach unten klicken, stellt der JavaScript-Code eine Verbindung mit dem Webserver her und führt dort eine Methode aus. Die Methoden Signatur ist die folgende:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

Das `current`-Argument ist der aktuelle Wert im Textfeld. Das `tag`-Attribut sind zusätzliche Kontext Daten, die als Eigenschaft des `NumericUpDown` Extender festgelegt werden können (Dies ist jedoch nicht erforderlich).

In diesem Beispiel darf das numerische auf-/ab--Steuerelement nur Werte zulassen, die die beiden folgenden Werte haben: 1, 2, 4, 8, 16, 32, 64 usw. Daher muss die Methode, die ausgeführt wird, wenn der Benutzer den Wert erhöhen möchte, den alten Wert verdoppeln. die andere Methode muss den Wert durch zwei dividieren. Hier ist der komplette Webdienst:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Erstellen Sie abschließend eine neue ASP.NET-Seite. Wie üblich benötigen Sie ein `ScriptManager`-Steuerelement, ein `TextBox`-Steuerelement und ein `NumericUpDownExtender`-Steuerelement. Für Letzteres müssen Sie die Webdienst Informationen bereitstellen:

- `ServiceDownMethod` Name der Down-Webmethode oder Seiten Methode
- `ServiceDownPath` Pfad zum Webdienst mit der Down-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden
- `ServiceUpMethod` Name der up-Webmethode oder-Seiten Methode
- `ServiceUpPath` Pfad zum Webdienst mit der up-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden

Hier ist das komplette Markup für die Seite:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Wenn Sie die Seite ausführen, beachten Sie, dass sich der Wert im Textfeld immer verdoppelt, wenn Sie auf die obere Schaltfläche klicken, und wird halbiert, wenn Sie auf die untere Schaltfläche klicken.

[![werden nur Zahlen angezeigt, die eine Potenz von 2 haben.](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Nur Zahlen, die eine Potenz von 2 sind, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png)).

> [!div class="step-by-step"]
> [Previous](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
