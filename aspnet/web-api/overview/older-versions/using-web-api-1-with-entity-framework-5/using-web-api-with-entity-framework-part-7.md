---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Teil 7: Erstellen der Hauptseite | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599989"
---
# <a name="part-7-creating-the-main-page"></a>Teil 7: Erstellen der Hauptseite

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Erstellen der Hauptseite

In diesem Abschnitt erstellen Sie die Haupt Anwendungsseite. Diese Seite ist komplexer als die Administrator Seite. Daher werden wir Sie in mehreren Schritten betrachten. Dabei werden einige erweiterte Knockout. js-Techniken angezeigt. Im folgenden finden Sie das grundlegende Layout der Seite:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Products" enthält ein Array von Produkten.
- "Cart" enthält ein Array von Produkten mit Mengen. Durch Klicken auf "zu Warenkorb hinzufügen" wird der Warenkorb aktualisiert.
- "Orders" enthält ein Array von Auftrags-IDs.
- "Details" enthält ein Bestell Detail, bei dem es sich um ein Array von Elementen (Produkte mit Mengen) handelt.

Wir beginnen mit dem definieren einiger grundlegender Layouts in HTML, ohne Datenbindung oder Skript. Öffnen Sie die Datei views/Home/Index. cshtml, und ersetzen Sie den gesamten Inhalt durch Folgendes:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Fügen Sie als nächstes einen Skript Abschnitt hinzu, und erstellen Sie ein leeres Ansichts Modell:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Basierend auf dem zuvor skizzierten Entwurf benötigt unser Ansichts Modell Observables für Produkte, Warenkorb, Bestellungen und Details. Fügen Sie dem `AppViewModel` Objekt die folgenden Variablen hinzu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Benutzer können Elemente aus der Liste Produkte zum Warenkorb hinzufügen und Elemente aus dem Warenkorb entfernen. Um diese Funktionen zu kapseln, erstellen wir eine weitere Ansichts Modell Klasse, die ein Produkt darstellt. Fügen Sie den folgenden Code zu `AppViewModel` hinzu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Die `ProductViewModel`-Klasse enthält zwei Funktionen, die verwendet werden, um das Produkt in und aus dem Warenkorb zu verschieben: `addItemToCart` fügt dem Warenkorb eine Einheit des Produkts hinzu, und `removeAllFromCart` entfernt alle Mengen des Produkts.

Benutzer können eine vorhandene Bestellung auswählen und die Bestelldetails erhalten. Wir Kapseln diese Funktionalität in ein anderes Ansichts Modell:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Die `OrderDetailsViewModel` wird mit einer Bestellung initialisiert, und die Bestelldetails werden abgerufen, indem eine AJAX-Anforderung an den Server gesendet wird.

Beachten Sie auch die `total`-Eigenschaft für die `OrderDetailsViewModel`. Diese Eigenschaft ist eine besondere Art Observable, die als [berechnete Observable](http://knockoutjs.com/documentation/computedObservables.html)bezeichnet wird. Wie der Name schon sagt, können Sie mit einem berechneten Observable Daten an einen berechneten&#8212;Wert binden, in diesem Fall die Gesamtkosten der Bestellung.

Fügen Sie als nächstes diese Funktionen `AppViewModel`hinzu:

- `resetCart` entfernt alle Elemente aus dem Warenkorb.
- `getDetails` Ruft die Details für eine Bestellung ab (indem eine neue `OrderDetailsViewModel` auf die `details` Liste übertragen wird).
- `createOrder` erstellt eine neue Bestellung und leert den Warenkorb.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Initialisieren Sie schließlich das Ansichts Modell, indem Sie AJAX-Anforderungen für die Produkte und Bestellungen vornehmen:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, das ist zwar viel Code, aber wir haben das Schritt-für-Schritt-Code erstellt, und ich hoffe, dass der Entwurf eindeutig ist. Nun können dem HTML einige Knockout. js-Bindungen hinzugefügt werden.

**Rodu**

Im folgenden finden Sie die Bindungen für die Produktliste:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Dies durchläuft das Products-Array und zeigt den Namen und den Preis an. Die Schaltfläche "zu Bestellung hinzufügen" ist nur sichtbar, wenn der Benutzer angemeldet ist.

Mit der Schaltfläche "zu Bestellung hinzufügen" werden `addItemToCart` für die `ProductViewModel` Instanz des Produkts aufgerufen. Dies veranschaulicht ein nützliches Feature von Knockout. js: Wenn ein Ansichts Modell andere Ansichts Modelle enthält, können Sie die Bindungen auf das innere Modell anwenden. In diesem Beispiel werden die Bindungen in der `foreach` auf jede der `ProductViewModel` Instanzen angewendet. Diese Vorgehensweise ist viel sauberer, als die gesamte Funktionalität in einem einzigen Ansichts Modell zu platzieren.

**Brust**

Hier sind die Bindungen für den Warenkorb:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Dadurch wird das waren Korb Array durchlaufen, und der Name, der Preis und die Menge werden angezeigt. Beachten Sie, dass der Link "Remove" und die Schaltfläche "Create Order" an View-Model-Funktionen gebunden sind.

**Vergabe**

Im folgenden finden Sie die Bindungen für die Liste Bestellungen:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Dadurch werden die Bestellungen durchlaufen, und die Bestell-ID wird angezeigt. Das Click-Ereignis für den Link ist an die `getDetails` Funktion gebunden.

**Bestell Details**

Im folgenden finden Sie die Bindungen für die Bestelldetails:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Dadurch werden die Elemente in der Reihenfolge durchlaufen, und das Produkt, der Preis und die Menge werden angezeigt. Das umgebende div-Element ist nur sichtbar, wenn das Detail Array mindestens ein Element enthält.

## <a name="conclusion"></a>Schlussfolgerung

In diesem Tutorial haben Sie eine Anwendung erstellt, die Entity Framework verwendet, um mit der Datenbank zu kommunizieren, und ASP.net-Web-API, um eine öffentlich ausgerichtete Schnittstelle oberhalb der Datenschicht bereitzustellen. Wir verwenden ASP.NET MVC 4 zum Rendering der HTML-Seiten und Knockout. js Plus jQuery, um dynamische Interaktionen ohne Seiten Umladungen bereitzustellen.

Zusätzliche Ressourcen:

- [ASP.NET Datenzugriffs-Inhalts Zuordnung](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework Developer Center](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Vorheriges](using-web-api-with-entity-framework-part-6.md)
