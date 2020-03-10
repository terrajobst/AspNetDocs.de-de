---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Erstellen der Sicht (UI) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448983"
---
# <a name="create-the-view-ui"></a>Erstellen der Ansicht (Benutzeroberfläche)

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt definieren Sie zunächst den HTML-Code für die APP und fügen die Datenbindung zwischen dem HTML-und dem Ansichts Modell hinzu.

Öffnen Sie die Datei views/Home/Index. cshtml. Ersetzen Sie den gesamten Inhalt dieser Datei durch Folgendes:

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Die meisten `div` Elemente sind für die [Bootstrap](http://getbootstrap.com/) -Formatierung vorhanden. Die wichtigen Elemente sind diejenigen mit `data-bind` Attributen. Dieses Attribut verknüpft den HTML-Code mit dem Ansichts Modell.

Beispiel:

[!code-html[Main](part-7/samples/sample2.html)]

In diesem Beispiel bewirkt die &quot;`text`&quot; Bindung, dass das `<p>`-Element den Wert der `error`-Eigenschaft aus dem Ansichts Modell anzeigt. Beachten Sie, dass `error` als `ko.observable`deklariert wurde:

[!code-javascript[Main](part-7/samples/sample3.js)]

Wenn `error`ein neuer Wert zugewiesen wird, aktualisiert Knockout den Text im `<p>` Element.

Die `foreach` Bindung weist Knockout an, den Inhalt des `books` Arrays zu durchlaufen. Für jedes Element im Array erstellt Knockout ein neues &lt;Li&gt;-Element. Bindungen innerhalb des Kontexts des `foreach` verweisen auf Eigenschaften des Array Elements. Beispiel:

[!code-html[Main](part-7/samples/sample4.html)]

Hier liest die `text` Bindung die Author-Eigenschaft jedes Buchs.

Wenn Sie die Anwendung jetzt ausführen, sollte Sie wie folgt aussehen:

![](part-7/_static/image1.png)

Die Liste der Bücher wird asynchron geladen, nachdem die Seite geladen wurde. Derzeit sind die &quot;Details&quot; Verknüpfungen nicht funktionsfähig. Diese Funktion wird im nächsten Abschnitt hinzugefügt.

> [!div class="step-by-step"]
> [Zurück](part-6.md)
> [Weiter](part-8.md)
