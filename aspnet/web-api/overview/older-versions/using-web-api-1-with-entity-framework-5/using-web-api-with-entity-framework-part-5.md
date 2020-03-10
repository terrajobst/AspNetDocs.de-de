---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout. js | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447861"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout. js

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js

In diesem Abschnitt verwenden wir Knockout. js zum Hinzufügen von Funktionen zur Administrator Ansicht.

[Knockout. js](http://knockoutjs.com/) ist eine JavaScript-Bibliothek, die das Binden von HTML-Steuerelementen an Daten vereinfacht. Knockout. js verwendet das Model-View-ViewModel (MVVM)-Muster.

- Das *Modell* ist die serverseitige Darstellung der Daten in der Geschäftsdomäne (in unserem Fall Produkte und Aufträge).
- Die *Ansicht* ist die Darstellungs Schicht (HTML).
- Das *View-Model* ist ein JavaScript-Objekt, das die Modelldaten enthält. Das Ansichts Modell ist eine Code Abstraktion der Benutzeroberfläche. Die HTML-Darstellung ist nicht bekannt. Stattdessen stellt Sie abstrakte Features der Sicht dar, z. b. "eine Liste von Elementen".

Die Ansicht ist Daten gebunden an das Ansichts Modell. Updates für das Ansichts Modell werden automatisch in der Ansicht widergespiegelt. Das View-Model ruft auch Ereignisse aus der Ansicht ab, z. b. Schaltflächen Klicks, und führt Vorgänge für das Modell aus, z. b. das Erstellen eines Auftrags.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Zuerst definieren wir das Ansichts Modell. Danach binden wir das HTML-Markup an das Ansichts Modell.

Fügen Sie admin. cshtml den folgenden Razor-Abschnitt hinzu:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Sie können diesen Abschnitt an beliebiger Stelle in der Datei hinzufügen. Wenn die Ansicht gerendert wird, wird der Abschnitt unten auf der HTML-Seite direkt vor dem schließenden &lt;/Body&gt; Tag angezeigt.

Alle Skripts für diese Seite werden innerhalb des Skripttags angezeigt, das durch den Kommentar angezeigt wird:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Definieren Sie zunächst eine Ansichts Modell Klasse:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko. observablearray** ist eine besondere Art von Objekt in Knockout, das als *Observable*bezeichnet wird. Aus der [Knockout. js-Dokumentation](http://knockoutjs.com/documentation/observables.html): ein Observable-Objekt ist ein JavaScript-Objekt, mit dem Abonnenten über Änderungen benachrichtigt werden können. Wenn sich der Inhalt einer wahrnehmbaren Änderung ändert, wird die Ansicht automatisch entsprechend aktualisiert.

Um das `products` Array aufzufüllen, stellen Sie eine AJAX-Anforderung an die Web-API. Erinnern Sie sich daran, dass wir den Basis-URI für die API im Ansichts Behälter gespeichert haben (siehe [Teil 4](using-web-api-with-entity-framework-part-4.md) des Tutorials).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Fügen Sie als nächstes dem Ansichts Modell Funktionen zum Erstellen, aktualisieren und Löschen von Produkten hinzu. Diese Funktionen übermitteln AJAX-Aufrufe an die Web-API und verwenden die Ergebnisse, um das Ansichts Modell zu aktualisieren.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Der wichtigste Teil: Wenn das DOM geladen ist, wird die Funktion " **ko. applybinding** " aufgerufen und eine neue Instanz der `ProductsViewModel`übergeben:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Die Methode " **ko. applybinding** " aktiviert Knockout und verbindet das Ansichts Modell mit der Ansicht.

Nachdem wir nun über ein Ansichts Modell verfügen, können wir die Bindungen erstellen. In Knockout. js fügen Sie den HTML-Elementen `data-bind` Attribute hinzu. Um z. b. eine HTML-Liste an ein Array zu binden, verwenden Sie die `foreach` Bindung:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Die `foreach` Bindung durchläuft das Array und erstellt untergeordnete Elemente für jedes Objekt im Array. Bindungen für die untergeordneten Elemente können auf Eigenschaften der Array Objekte verweisen.

Fügen Sie der Liste "Update-Products" die folgenden Bindungen hinzu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Das `<li>`-Element tritt innerhalb des Gültigkeits Bereichs der **foreach** -Bindung auf. Das bedeutet, dass Knockout das Element einmal für jedes Produkt im `products` Array Rendering wird. Alle Bindungen im `<li>`-Element verweisen auf diese Produkt Instanz. `$data.Name` bezieht sich z. b. auf die `Name`-Eigenschaft des Produkts.

Um die Werte der Texteingaben festzulegen, verwenden Sie die `value` Bindung. Die Schaltflächen werden mithilfe der `click` Bindung an Funktionen in der Modell Ansicht gebunden. Die Product-Instanz wird als Parameter an jede Funktion übergeben. Weitere Informationen finden Sie in der [Knockout. js-Dokumentation](http://knockoutjs.com/documentation/observables.html) mit guten Beschreibungen der verschiedenen Bindungen.

Fügen Sie als nächstes eine Bindung für das **Sende Ereignis auf dem Formular** "Produkt hinzufügen" hinzu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Diese Bindung Ruft die `create`-Funktion für das Ansichts Modell auf, um ein neues Produkt zu erstellen.

Im folgenden finden Sie den gesamten Code für die Administrator Ansicht:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Führen Sie die Anwendung aus, melden Sie sich mit dem Administrator Konto an, und klicken Sie auf den Link "admin". Sie sollten die Liste der Produkte sehen und Produkte erstellen, aktualisieren oder löschen können.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-4.md)
> [Weiter](using-web-api-with-entity-framework-part-6.md)
