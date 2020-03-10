---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Bearbeiten von DropShadow-Eigenschaften aus ClientC#Code () | Microsoft-Dokumentation
author: wenz
description: Anpassen der Bearbeitungs Schnittstelle von DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497427"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Bearbeiten von DropShadow-Eigenschaften über den Clientcode (C#)

von [Christian Wenz](https://github.com/wenz)

[Code herunterladen](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) oder [PDF herunterladen](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Die Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Code des Clients geändert werden.

## <a name="overview"></a>Übersicht

Mit dem DropShadow-Steuerelement im AJAX Control Toolkit wird ein Panel mit einem Schlag Schatten erweitert. Die Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Code des Clients geändert werden.

## <a name="steps"></a>Schritte

Der Code beginnt mit einem Panel, das einige Textzeilen enthält:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Die zugeordnete CSS-Klasse gibt dem Panel eine schöne Hintergrundfarbe:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

Der `DropShadowExtender` wird hinzugefügt, um den Bereich mit einem Schlag Schatteneffekt zu erweitern, Deckkraft auf 50% festgelegt:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Mit dem ASP.NET AJAX-`ScriptManager` Steuerelement kann das Steuerelement Toolkit funktionieren:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Ein anderes Panel enthält zwei JavaScript-Links zum Festlegen der Deckkraft des Schlag Schattens: der minus Link verringert die Deckkraft des Schattens, der Plus Link erhöht ihn.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Die JavaScript-Funktion `changeOpacity()` muss dann zuerst das `DropShadowExtender`-Steuerelement auf der Seite suchen. ASP.NET AJAX definiert die `$find()` Methode genau für diese Aufgabe. Anschließend ruft die `get_Opacity()`-Methode die aktuelle Deckkraft ab, die von der `set_Opacity()`-Methode festgelegt wird. Der JavaScript-Code legt dann den aktuellen Wert für die Deckkraft in das `<label>`-Element ein:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

[![die Deckkraft auf der Clientseite geändert wird.](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Die Deckkraft wird auf der Clientseite geändert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png)).

> [!div class="step-by-step"]
> [Zurück](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Weiter](adjusting-the-z-index-of-a-dropshadow-vb.md)
