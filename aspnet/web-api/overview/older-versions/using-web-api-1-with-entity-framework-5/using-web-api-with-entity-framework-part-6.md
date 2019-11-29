---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Teil 6: Erstellen von Produkt-und Bestell Controllern | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600031"
---
# <a name="part-6-creating-product-and-order-controllers"></a>Teil 6: Erstellen von Produkt-und Bestell Controllern

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Hinzufügen eines Products-Controllers

Der Administrator Controller ist für Benutzer, die über Administratorrechte verfügen. Kunden können dagegen Produkte anzeigen, jedoch nicht erstellen, aktualisieren oder löschen.

Der Zugriff auf die Post-, Put-und Delete-Methoden kann problemlos eingeschränkt werden, während die Get-Methoden geöffnet lassen. Sehen Sie sich jedoch die Daten an, die für ein Produkt zurückgegeben werden:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Die `ActualCost`-Eigenschaft sollte für Kunden nicht sichtbar sein. Die Lösung besteht darin, ein *Datenübertragungs Objekt (Data Transfer Object* , dto) zu definieren, das eine Teilmenge der Eigenschaften enthält, die für Kunden sichtbar sein sollten. Wir verwenden LINQ to Project `Product` Instanzen, um Instanzen `ProductDTO`.

Fügen Sie dem Ordner Models eine Klasse mit dem Namen `ProductDTO` hinzu.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Fügen Sie jetzt den Controller hinzu. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller. Wählen Sie **Hinzufügen**und dann **Controller**aus. Benennen Sie im Dialogfeld " **Controller hinzufügen** " den Controller &quot;ProductController-&quot;. Wählen Sie unter **Vorlage**die Option **leerer API-Controller**aus.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Ersetzen Sie alles in der Quelldatei durch den folgenden Code:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Der Controller verwendet weiterhin den `OrdersContext`, um die Datenbank abzufragen. Anstatt `Product` Instanzen direkt zurückzugeben, wird `MapProducts` aufgerufen, um Sie auf `ProductDTO` Instanzen zu projizieren:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Die `MapProducts`-Methode gibt einen **iquerable**-Wert zurück, sodass das Ergebnis mit anderen Abfrage Parametern zusammengesetzt werden kann. Dies wird in der `GetProduct`-Methode angezeigt, mit der der Abfrage eine **Where** -Klausel hinzugefügt wird:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Hinzufügen eines Orders-Controllers

Fügen Sie als nächstes einen Controller hinzu, mit dem Benutzer Bestellungen erstellen und anzeigen können.

Wir beginnen mit einem weiteren dto. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle, und fügen Sie eine Klasse mit dem Namen `OrderDTO` der folgenden Implementierung hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Fügen Sie jetzt den Controller hinzu. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller. Wählen Sie **Hinzufügen**und dann **Controller**aus. Legen Sie im Dialogfeld **Controller hinzufügen** die folgenden Optionen fest:

- Geben Sie unter **Controller Name den Namen**orderscontroller ein.
- Wählen Sie unter **Vorlage**die Option "API-Controller mit Lese-/Schreibaktionen mit Entity Framework" aus.
- Wählen Sie unter **Modell Klasse**die Option &quot;Order (productstore. Models)&quot;aus.
- Wählen Sie unter **Datenkontext Klasse**die Option &quot;orderscontext (productstore. Models)&quot;aus.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Klicken Sie auf **Hinzufügen**. Dadurch wird eine Datei mit dem Namen OrdersController.cs hinzugefügt. Als nächstes müssen wir die Standard Implementierung des Controllers ändern.

Löschen Sie zunächst die Methoden `PutOrder` und `DeleteOrder`. In diesem Beispiel können Kunden vorhandene Aufträge nicht ändern oder löschen. In einer echten Anwendung benötigen Sie viel Back-End-Logik, um diese Fälle zu behandeln. (Z. b. wurde die Bestellung bereits versendet?)

Ändern Sie die `GetOrders`-Methode so, dass nur die Bestellungen zurückgegeben werden, die zum Benutzer gehören:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Ändern Sie die `GetOrder`-Methode wie folgt:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Im folgenden finden Sie die Änderungen, die wir an der-Methode vorgenommen haben:

- Der Rückgabewert ist eine `OrderDTO` Instanz anstelle eines `Order`.
- Wenn die Datenbank für die Bestellung abgefragt wird, verwenden wir die [dbquery. include](https://msdn.microsoft.com/library/gg696395) -Methode, um die zugehörigen `OrderDetail` und `Product` Entitäten abzurufen.
- Wir vereinfachen das Ergebnis mithilfe einer Projektion.

Die HTTP-Antwort enthält ein Array von Produkten mit Mengen:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Dieses Format ist für Clients einfacher zu verwenden als das ursprüngliche Objekt Diagramm, das die Entitäten (Reihenfolge, Details und Produkte) enthält.

Die letzte Methode, die als `PostOrder`betrachtet werden soll. Derzeit nimmt diese Methode eine `Order`-Instanz an. Beachten Sie jedoch, was geschieht, wenn ein Client einen Anforderungs Text wie den folgenden sendet:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Dies ist eine gut strukturierte Reihenfolge, und Entity Framework wird sie glücklicherweise in die Datenbank einfügen. Sie enthält jedoch eine Product-Entität, die zuvor noch nicht vorhanden war. Der Client hat soeben ein neues Produkt in unserer Datenbank erstellt. Dies ist eine Überraschung für die Auftrags Erfüllungs Abteilung, wenn Ihnen eine Bestellung für Koala-Bären angezeigt wird. Die Moral besteht darin, die Daten, die Sie in einer Post-oder PUT-Anforderung akzeptieren, wirklich vorsichtig zu sein.

Um dieses Problem zu vermeiden, ändern Sie die `PostOrder`-Methode, um eine `OrderDTO`-Instanz zu verwenden. Verwenden Sie die `OrderDTO`, um den `Order`zu erstellen.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Beachten Sie, dass die Eigenschaften `ProductID` und `Quantity` verwendet werden und alle Werte ignoriert werden, die der Client entweder für den Produktnamen oder den Preis gesendet hat. Wenn die Produkt-ID ungültig ist, verstößt sie gegen die FOREIGN KEY-Einschränkung in der Datenbank, und die Einfügung schlägt wie folgt fehl.

Dies ist die gesamte `PostOrder`-Methode:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Fügen Sie schließlich dem Controller das Attribut " **autorisieren** " hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Jetzt können nur registrierte Benutzer Bestellungen erstellen oder anzeigen.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-5.md)
> [Weiter](using-web-api-with-entity-framework-part-7.md)
