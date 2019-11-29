---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Erstellen eines numerischen auf-/ab--Steuerelements mit einemC#Webdienst-Back-End () | Microsoft-Dokumentation
author: wenz
description: Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann es sein, dass ein numerisches auf-/ab--Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) mehr als c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598877"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Erstellen einen numerischen UpAndDown-Steuerelements mit einem Webdienst-Back-End (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden. Standardmäßig erhöht oder verringert das NumericUpDown-Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.

## <a name="overview"></a>Übersicht über

Anstatt einem Benutzer einen Wert in ein Kontrollkästchen einzugeben, kann ein numerisches auf-/ab-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) als komfortabler angesehen werden. Standardmäßig erhöht oder verringert das `NumericUpDown` Steuerelement einen Wert immer um 1, aber ein Webdienst erweist sich als mehr Flexibilität.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX-Steuerelement-Toolkit enthält den `NumericUpDown` Extender, der automatisch zwei Schaltflächen zu einem Textfeld hinzufügt: eine zum Erhöhen des Werts, eine zum verringern. Das-Steuerelement unterstützt jedoch auch einen Webdienst-oder Seiten Methodenaufrufe. Wenn Sie auf die Schaltfläche nach oben oder nach unten klicken, stellt der JavaScript-Code eine Verbindung mit dem Webserver her und führt dort eine Methode aus. Die Methoden Signatur ist die folgende:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

Das `current`-Argument ist der aktuelle Wert im Textfeld. Das `tag`-Attribut sind zusätzliche Kontext Daten, die als Eigenschaft des `NumericUpDown` Extender festgelegt werden können (Dies ist jedoch nicht erforderlich).

In diesem Beispiel darf das numerische auf-/ab--Steuerelement nur Werte zulassen, die die beiden folgenden Werte haben: 1, 2, 4, 8, 16, 32, 64 usw. Daher muss die Methode, die ausgeführt wird, wenn der Benutzer den Wert erhöhen möchte, den alten Wert verdoppeln. die andere Methode muss den Wert durch zwei dividieren. Hier ist der komplette Webdienst:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Erstellen Sie abschließend eine neue ASP.NET-Seite. Wie üblich benötigen Sie ein `ScriptManager`-Steuerelement, ein `TextBox`-Steuerelement und ein `NumericUpDownExtender`-Steuerelement. Für Letzteres müssen Sie die Webdienst Informationen bereitstellen:

- `ServiceDownMethod` Name der Down-Webmethode oder Seiten Methode
- `ServiceDownPath` Pfad zum Webdienst mit der Down-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden
- `ServiceUpMethod` Name der up-Webmethode oder-Seiten Methode
- `ServiceUpPath` Pfad zum Webdienst mit der up-Dienst Methode. weglassen, wenn Sie eine Seiten Methode verwenden

Hier ist das komplette Markup für die Seite:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Wenn Sie die Seite ausführen, beachten Sie, dass sich der Wert im Textfeld immer verdoppelt, wenn Sie auf die obere Schaltfläche klicken, und wird halbiert, wenn Sie auf die untere Schaltfläche klicken.

[![werden nur Zahlen angezeigt, die eine Potenz von 2 haben.](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Nur Zahlen, die eine Potenz von 2 sind, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Nächste](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
