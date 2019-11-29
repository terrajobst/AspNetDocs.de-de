---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Aktivieren von CRUD-Vorgängen in ASP.net-Web-API 1-ASP.NET 4. x
author: MikeWasson
description: Tutorial zeigt, wie CRUD-Vorgänge in einem HTTP-Dienst unterstützt werden, indem ASP.net-Web-API für ASP.NET 4. x verwendet wird.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600336"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Aktivieren von CRUD-Vorgängen in ASP.net-Web-API 1

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> In diesem Tutorial wird gezeigt, wie CRUD-Vorgänge in einem HTTP-Dienst mithilfe von ASP.net-Web-API für ASP.NET 4. x unterstützt werden.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Visual Studio 2012
> - Web-API 1 (funktioniert auch mit der Web-API 2)

CRUD steht für &quot;erstellen, lesen, aktualisieren und löschen,&quot; den vier grundlegenden Daten Bank Vorgängen. Viele HTTP-Dienste modellieren auch CRUD-Vorgänge über Rest-oder Rest-ähnliche APIs.

In diesem Tutorial erstellen Sie eine sehr einfache Web-API, um eine Liste von Produkten zu verwalten. Jedes Produkt enthält einen Namen, einen Preis und eine Kategorie (z. b. &quot;Toys&quot; oder &quot;Hardware&quot;) sowie eine Produkt-ID.

Die Products-API macht die folgenden Methoden verfügbar.

| -Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen einer Liste aller Produkte | GET | /api/products |
| Produkt nach ID erhalten | GET | /API/Products/-*ID* |
| Produkt nach Kategorie | GET | /API/Products? Category =*Kategorie* |
| Erstellen eines neuen Produkts | POST | /api/products |
| Aktualisieren eines Produkts | PUT | /API/Products/-*ID* |
| Löschen eines Produkts | LÖSCHEN | /API/Products/-*ID* |

Beachten Sie, dass einige der URIs die Produkt-ID im Pfad enthalten. Um z. b. das Produkt mit der ID 28 zu erhalten, sendet der Client eine GET-Anforderung für `http://hostname/api/products/28`.

### <a name="resources"></a>Ressourcen

Die Products-API definiert URIs für zwei Ressourcentypen:

| Ressource | URI |
| --- | --- |
| Die Liste aller Produkte. | /api/products |
| Ein einzelnes Produkt. | /API/Products/-*ID* |

### <a name="methods"></a>Methoden

Die vier http-Hauptmethoden (Get, Put, Post und DELETE) können CRUD-Vorgängen folgendermaßen zugeordnet werden:

- Get ruft die Darstellung der Ressource an einem angegebenen URI ab. Get sollte keine Nebeneffekte auf dem Server haben.
- Put aktualisiert eine Ressource an einem angegebenen URI. Put kann auch verwendet werden, um eine neue Ressource an einem angegebenen URI zu erstellen, wenn der Server es Clients ermöglicht, neue URIs anzugeben. In diesem Tutorial wird die Erstellung durch Put von der API nicht unterstützt.
- Post erstellt eine neue Ressource. Der Server weist den URI für das neue-Objekt zu und gibt diesen URI als Teil der Antwortnachricht zurück.
- DELETE Löscht eine Ressource an einem angegebenen URI.

Hinweis: die Put-Methode ersetzt die gesamte Product-Entität. Das heißt, der Client wird erwartet, dass er eine komplette Darstellung des aktualisierten Produkts sendet. Wenn Sie Teil Aktualisierungen unterstützen möchten, wird die Patch-Methode bevorzugt. In diesem Tutorial wird Patch nicht implementiert.

## <a name="create-a-new-web-api-project"></a>Erstellen eines neuen Web-API-Projekts

Führen Sie zunächst Visual Studio aus, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus. Nennen Sie das Projekt &quot;productstore-&quot;, und klicken Sie auf **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Web API** aus, und klicken Sie auf **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Hinzufügen eines Modells

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. In ASP.net-Web-API können Sie stark typisierte CLR-Objekte als Modelle verwenden, die automatisch in XML oder JSON für den Client serialisiert werden.

Bei der productstore-API bestehen unsere Daten aus Produkten. Daher erstellen wir eine neue Klasse mit dem Namen "`Product`".

Wenn Projektmappen-Explorer nicht bereits sichtbar ist, klicken Sie auf das Menü **Ansicht** , und wählen Sie **Projektmappen-Explorer**aus. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Modelle** . Klicken Sie im Kontextmenü auf **Hinzufügen**, und wählen Sie dann **Klasse**aus. Benennen Sie die Klasse &quot;Product&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Fügen Sie der `Product`-Klasse die folgenden Eigenschaften hinzu.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Hinzufügen eines Repositorys

Wir müssen eine Sammlung von Produkten speichern. Es empfiehlt sich, die Sammlung von unserer Dienst Implementierung zu trennen. Auf diese Weise können wir den Sicherungs Speicher ändern, ohne die Dienstklasse neu zu schreiben. Diese Art von Entwurf wird als *Repository* -Muster bezeichnet. Definieren Sie zunächst eine generische Schnittstelle für das Repository.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Modelle** . Wählen Sie **Hinzufügen**und dann **Neues Element**aus.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, C# und erweitern Sie den Knoten. Wählen C#Sie unter die Option **Code**aus. Wählen Sie in der Liste der Code Vorlagen die Option **Schnittstelle**aus. Benennen Sie die Schnittstelle &quot;iproductrepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Fügen Sie die folgende Implementierung hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Fügen Sie nun dem Ordner Models eine weitere Klasse mit dem Namen &quot;productrepository hinzu.&quot; diese Klasse implementiert die `IProductRepository`-Schnittstelle. Fügen Sie die folgende Implementierung hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Das Repository speichert die Liste im lokalen Arbeitsspeicher. Dies ist für ein Tutorial in Ordnung, aber in einer echten Anwendung würden Sie die Daten extern speichern. dabei handelt es sich entweder um eine Datenbank oder um einen cloudspeicher. Das Repository-Muster vereinfacht das spätere Ändern der Implementierung.

## <a name="adding-a-web-api-controller"></a>Hinzufügen eines Web-API-Controllers

Wenn Sie mit ASP.NET MVC gearbeitet haben, sind Sie bereits mit Controllern vertraut. In ASP.net-Web-API ist ein *Controller* eine Klasse, die HTTP-Anforderungen vom Client verarbeitet. Der Assistent für neue Projekte hat bei der Erstellung des Projekts zwei Controller erstellt. Um diese anzuzeigen, erweitern Sie den Ordner Controllers in Projektmappen-Explorer.

- HomeController ist ein herkömmlicher ASP.NET-MVC-Controller. Er ist verantwortlich für die Bereitstellung von HTML-Seiten für die Website und ist nicht direkt mit unserer Web-API verknüpft.
- Valuescontroller ist ein Beispiel für einen WebAPI-Controller.

Löschen Sie valuescontroller, indem Sie in Projektmappen-Explorer mit der rechten Maustaste auf die Datei klicken und dann **Löschen auswählen.** Fügen Sie nun wie folgt einen neuen Controller hinzu:

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner Controller. Wählen Sie **Hinzufügen** , und wählen Sie dann **Controller**aus.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

Benennen Sie im Assistenten zum **Hinzufügen von Controllern** den Controller &quot;ProductController-&quot;. Wählen Sie in der Dropdown Liste **Vorlage** die Option **leerer API-Controller**aus. Klicken Sie anschließend auf **Hinzufügen**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Die Controller müssen nicht in einem Ordner mit dem Namen "Controllers" abgelegt werden. Der Ordnername ist nicht wichtig. Es ist einfach eine bequeme Methode zum Organisieren der Quelldateien.

Der Assistent zum **Hinzufügen** von Controllern erstellt eine Datei namens ProductsController.cs im Ordner Controllers. Wenn diese Datei nicht bereits geöffnet ist, doppelklicken Sie auf die Datei, um Sie zu öffnen. Fügen Sie die folgende **using** -Anweisung hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Fügen Sie ein Feld hinzu, das eine **iproductrepository** -Instanz enthält.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Das Aufrufen von `new ProductRepository()` im Controller ist nicht das beste Design, da es den Controller an eine bestimmte Implementierung von `IProductRepository`bindet. Einen besseren Ansatz finden Sie unter [Verwenden des Web-API-Abhängigkeits Konflikt Lösers](../advanced/dependency-injection.md).

## <a name="getting-a-resource"></a>Eine Ressource wird erhalten.

Die productstore-API macht mehrere &quot;Lese&quot; Aktionen als HTTP Get-Methoden verfügbar. Jede Aktion entspricht einer Methode in der `ProductsController`-Klasse.

| -Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen einer Liste aller Produkte | GET | /api/products |
| Produkt nach ID erhalten | GET | /API/Products/-*ID* |
| Produkt nach Kategorie | GET | /API/Products? Category =*Kategorie* |

Fügen Sie der `ProductsController`-Klasse die folgende Methode hinzu, um die Liste aller Produkte zu erhalten:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Der Methodenname beginnt mit &quot;Get-&quot;, sodass er den Get-Anforderungen entspricht. Da die Methode über keine Parameter verfügt, wird Sie auch einem URI zugeordnet, der keine *&quot;ID&quot;* Segment im Pfad enthält.

Fügen Sie der `ProductsController`-Klasse die folgende Methode hinzu, um ein Produkt über die ID zu erhalten:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Dieser Methodenname beginnt auch mit &quot;Get&quot;, aber die Methode hat einen Parameter mit dem Namen " *ID*". Dieser Parameter wird der &quot;ID&quot; Segment des URI-Pfads zugeordnet. Das ASP.net-Web-API Framework konvertiert die ID automatisch in den richtigen Datentyp (**int**) für den-Parameter.

Die GetProduct-Methode löst eine Ausnahme vom Typ " **httpresponcexception** " aus, wenn die *ID* ungültig ist. Diese Ausnahme wird vom Framework in einen 404-Fehler (nicht gefunden) übersetzt.

Fügen Sie abschließend eine Methode zum Suchen nach Produkten nach Kategorie hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Wenn der Anforderungs-URI eine Abfrage Zeichenfolge aufweist, versucht die Web-API, die Abfrage Parameter mit Parametern für die Controller Methode abzugleichen. Daher wird dieser Methode ein URI der Form "API/Products? Category =*Category*" zugeordnet.

## <a name="creating-a-resource"></a>Erstellen einer Ressource

Als Nächstes fügen wir der `ProductsController`-Klasse eine Methode hinzu, um ein neues Produkt zu erstellen. Im folgenden finden Sie eine einfache Implementierung der-Methode:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Beachten Sie zwei Dinge zu dieser Methode:

- Der Methodenname beginnt mit &quot;Post...&quot;. Zum Erstellen eines neuen Produkts sendet der Client eine HTTP POST-Anforderung.
- Die-Methode nimmt einen Parameter vom Typ "Product" an. In der Web-API werden Parameter mit komplexen Typen aus dem Anforderungs Text deserialisiert. Daher erwarten wir, dass der Client eine serialisierte Darstellung eines Product-Objekts entweder im XML-oder im JSON-Format sendet.

Diese Implementierung funktioniert, ist jedoch nicht ganz fertig. Im Idealfall möchten wir, dass die HTTP-Antwort folgendes einschließt:

- **Antwort Code:** Standardmäßig legt das Web-API-Framework den Antwortstatus Code auf 200 (OK) fest. Nach dem HTTP/1.1-Protokoll sollte der Server jedoch mit dem Status 201 (erstellt) Antworten, wenn eine Post-Anforderung zur Erstellung einer Ressource führt.
- **Speicherort:** Wenn der Server eine Ressource erstellt, sollte er den URI der neuen Ressource im Location-Header der Antwort enthalten.

ASP.net-Web-API vereinfacht das Bearbeiten der http-Antwortnachricht. Hier ist die verbesserte Implementierung:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Beachten Sie, dass der Rückgabetyp der Methode jetzt **httpresponsmessage**ist. Durch das Zurückgeben einer **HttpResponseMessage** anstelle eines Produkts können wir die Details der http-Antwortnachricht steuern, einschließlich des Statuscodes und des Location-Headers.

Die Methode " **samateresponse** " erstellt eine **HttpResponseMessage** und schreibt automatisch eine serialisierte Darstellung des Product-Objekts in den Text der Antwortnachricht.

> [!NOTE]
> In diesem Beispiel werden die `Product`nicht überprüft. Weitere Informationen zur Modell Validierung finden Sie [Untermodell Validierung in ASP.net-Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

## <a name="updating-a-resource"></a>Aktualisieren einer Ressource

Das Aktualisieren eines Produkts mit Put ist einfach:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Der Methodenname beginnt mit &quot;Put...&quot;, sodass die Web-API mit Put-Anforderungen übereinstimmt. Die-Methode erfordert zwei Parameter: die Produkt-ID und das aktualisierte Produkt. Der *ID* -Parameter wird aus dem URI-Pfad entnommen, und der *Product* -Parameter wird aus dem Anforderungs Text deserialisiert. Standardmäßig übernimmt das ASP.net-Web-API Framework Einfache Parametertypen aus der Route und komplexen Typen aus dem Anforderungs Text.

## <a name="deleting-a-resource"></a>Löschen einer Ressource

Um eine Ressource zu löschen, definieren Sie ein "löschen..." anzuwenden.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Wenn eine DELETE-Anforderung erfolgreich ist, kann Sie den Status 200 (OK) mit einem Entitäts Text zurückgeben, der den Status beschreibt. Status 202 (akzeptiert), wenn der Löschvorgang noch aussteht. oder Status 204 (kein Inhalt) ohne Entitäts Text. In diesem Fall verfügt die `DeleteProduct`-Methode über einen `void` Rückgabetyp, sodass ASP.net-Web-API diese automatisch in den Statuscode 204 (kein Inhalt) übersetzt.
