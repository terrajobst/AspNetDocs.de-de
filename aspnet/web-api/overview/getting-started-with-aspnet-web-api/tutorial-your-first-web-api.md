---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Beginnen Sie mit ASP.net-Web-API 2 (C#)-ASP.NET 4. x
author: MikeWasson
description: Tutorial mit Code. Verwenden Sie ASP.net-Web-API, um eine Web-API zu erstellen, die eine Liste von Produkten zurückgibt.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3e35c2bc0e46dfdb4544b772775eddd533f27be3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448551"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a>Beginnen Sie mit ASP.net-Web-API 2 (C#)

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

In diesem Tutorial verwenden Sie ASP.net-Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt.

HTTP dient nicht nur zum Reservieren von Webseiten. HTTP ist auch eine leistungsstarke Plattform zum Entwickeln von APIs, die Dienste und Daten verfügbar machen. HTTP ist einfach, flexibel und allgegenwärtig. Fast jede Plattform, die Sie sich vorstellen können, verfügt über eine HTTP-Bibliothek, sodass HTTP-Dienste eine breite Palette von Clients erreichen können, einschließlich Browsern, mobiler Geräte und herkömmlicher Desktop Anwendungen.

ASP.net-Web-API ist ein Framework zum Entwickeln von Web-APIs auf der .NET Framework. 

## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- Web-API 2

Eine neuere Version dieses Tutorials finden Sie [unter Erstellen einer Web-API mit ASP.net Core und Visual Studio für Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .

## <a name="create-a-web-api-project"></a>Erstellen eines Web-API-Projekts

In diesem Tutorial verwenden Sie ASP.net-Web-API, um eine Web-API zu erstellen, die eine Liste der Produkte zurückgibt. Die Front-End-Webseite verwendet jQuery, um die Ergebnisse anzuzeigen.

![](tutorial-your-first-web-api/_static/image1.png)

Starten Sie Visual Studio, und wählen Sie auf der **Start** Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET Webanwendung**aus. Nennen Sie das Projekt "productapp", und klicken Sie auf **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus. Überprüfen Sie unter &quot;Ordner und Kern Verweise hinzufügen für&quot;die **Web-API**. Klicken Sie auf **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Sie können auch ein Web-API-Projekt mithilfe der &quot;Web-API&quot; Vorlage erstellen. Die Web-API-Vorlage verwendet ASP.NET MVC, um API-Hilfeseiten bereitzustellen. Ich verwende die leere Vorlage für dieses Tutorial, weil ich die Web-API ohne MVC anzeigen möchte. Im Allgemeinen müssen Sie ASP.NET MVC nicht kennen, um die Web-API zu verwenden.

## <a name="adding-a-model"></a>Hinzufügen eines Modells

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. ASP.net-Web-API können das Modell automatisch in JSON, XML oder ein anderes Format serialisieren und dann die serialisierten Daten in den Text der http-Antwortnachricht schreiben. Solange ein Client das Serialisierungsformat lesen kann, kann das Objekt deserialisiert werden. Die meisten Clients können entweder XML oder JSON analysieren. Darüber hinaus kann der Client das gewünschte Format angeben, indem der Accept-Header in der HTTP-Anforderungs Nachricht festgelegt wird.

Erstellen Sie zunächst ein einfaches Modell, das ein Produkt darstellt.

Wenn der Projektmappen-Explorer nicht bereits sichtbar ist, können Sie auf das Menü **Ansicht** klicken und **Projektmappen-Explorer** wählen. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner „Modelle“. Wählen Sie im Kontextmenü die Option **Hinzufügen** und dann **Klasse**.

![](tutorial-your-first-web-api/_static/image4.png)

Benennen Sie die Klasse &quot;Product&quot;. Fügen Sie der `Product`-Klasse die folgenden Eigenschaften hinzu.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Hinzufügen eines Controllers

In der Web-API ist ein *Controller* ein Objekt zum Verarbeiten von HTTP-Anforderungen. Wir fügen einen Controller hinzu, der entweder eine Liste mit Produkten oder ein einzelnes von der ID angegebenes Produkt zurückgeben kann.

> [!NOTE]
> Wenn Sie ASP.NET MVC verwendet haben, sind Sie bereits mit Controllern vertraut. Web-API-Controller ähneln MVC-Controllern, erben aber die **apicontroller** -Klasse anstelle der **Controller** -Klasse.

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner „Controller“. Wählen Sie **Hinzufügen** und dann **Controller**.

![](tutorial-your-first-web-api/_static/image5.png)

Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **Web API Controller - Empty** (Web-API-Controller – Leer). Klicken Sie auf **Hinzufügen**.

![](tutorial-your-first-web-api/_static/image6.png)

Benennen Sie im Dialogfeld " **Controller hinzufügen** " den Controller &quot;ProductController-&quot;. Klicken Sie auf **Hinzufügen**.

![](tutorial-your-first-web-api/_static/image7.png)

Das Gerüst erstellt im Ordner "Controllers" eine Datei mit dem Namen ProductsController.cs.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Sie müssen die Controller nicht in einem Ordner mit dem Namen "Controllers" platzieren. Der Ordnername ist nur eine bequeme Möglichkeit zum Organisieren der Quelldateien.

Doppelklicken Sie auf die Datei, um sie zu öffnen, falls sie noch nicht geöffnet ist. Ersetzen Sie den Code in dieser Datei durch Folgendes:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Um das Beispiel einfach zu halten, werden Produkte in einem Fixed-Array in der Controller Klasse gespeichert. Natürlich würden Sie in einer echten Anwendung eine Datenbank Abfragen oder eine andere externe Datenquelle verwenden.

Der Controller definiert zwei Methoden, die Produkte zurückgeben:

- Die `GetAllProducts`-Methode gibt die gesamte Liste der Produkte als **IEnumerable-&lt;Product&gt;** Type zurück.
- Die `GetProduct`-Methode sucht nach ihrer ID nach einem einzelnen Produkt.

Das ist alles! Sie verfügen über eine funktionierende Web-API. Jede Methode auf dem Controller entspricht einem oder mehreren URIs:

| Controller-Methode | URI |
| --- | --- |
| Getallproducts | /api/products |
| GetProduct | /API/Products/-*ID* |

Bei der `GetProduct`-Methode ist die *ID* im URI ein Platzhalter. Um z. b. das Produkt mit der ID 5 zu erhalten, wird der URI `api/products/5`.

Weitere Informationen zur Weiterleitung von HTTP-Anforderungen an Controller Methoden durch die Web-API finden Sie unter [Routing in ASP.net-Web-API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Aufrufen der Web-API mit JavaScript und jQuery

In diesem Abschnitt fügen wir eine HTML-Seite hinzu, die AJAX verwendet, um die Web-API aufzurufen. Wir verwenden jQuery zum Erstellen der AJAX-Aufrufe und zum Aktualisieren der Seite mit den Ergebnissen.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**und dann **Neues Element**aus.

![](tutorial-your-first-web-api/_static/image9.png)

Wählen Sie im Dialogfeld **Neues Element hinzufügen** unter **Visual C#** den **webknoten** aus, und wählen Sie dann das Element **HTML-Seite** aus. Benennen Sie die Seite &quot;Index. html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Ersetzen Sie alles in dieser Datei durch Folgendes:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Es gibt mehrere Möglichkeiten, um jQuery herunterzuladen. In diesem Beispiel habe ich das [Microsoft AJAX CDN](../../../ajax/cdn/overview.md)verwendet. Sie können Sie auch aus [http://jquery.com/](http://jquery.com/)herunterladen, und die Projektvorlage "Web-API" ASP.NET enthält auch jQuery.

### <a name="getting-a-list-of-products"></a>Eine Liste der Produkte wird erhalten.

Senden Sie eine HTTP GET-Anforderung an &quot;/API/Products-&quot;, um eine Liste der Produkte zu erhalten.

Die jQuery-Funktion [getjson](http://api.jquery.com/jQuery.getJSON/) sendet eine AJAX-Anforderung. Für Response enthält ein Array von JSON-Objekten. Die `done`-Funktion gibt einen Rückruf an, der aufgerufen wird, wenn die Anforderung erfolgreich ist. Im Rückruf aktualisieren wir das DOM mit den Produktinformationen.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Erhalten eines Produkts nach ID

Wenn Sie ein Produkt anhand der ID erhalten möchten, senden Sie eine HTTP GET-Anforderung an &quot;/API/Products/*ID*&quot;, wobei *ID* die Produkt-ID ist.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Es wird weiterhin `getJSON` aufgerufen, um die AJAX-Anforderung zu senden, aber dieses Mal wird die ID in den Anforderungs-URI eingefügt. Die Antwort dieser Anforderung ist eine JSON-Darstellung eines einzelnen Produkts.

## <a name="running-the-application"></a>Ausführen der Anwendung

Drücken Sie die F5-TASTE, um das Debuggen der Anwendung zu starten. Die Webseite sollte wie folgt aussehen:

![](tutorial-your-first-web-api/_static/image11.png)

Geben Sie die ID ein, und klicken Sie auf suchen, um ein Produkt nach ID zu erhalten.

![](tutorial-your-first-web-api/_static/image12.png)

Wenn Sie eine ungültige ID eingeben, gibt der Server einen HTTP-Fehler zurück:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Verwenden von F12 zum Anzeigen der HTTP-Anforderung und-Antwort

Wenn Sie mit einem HTTP-Dienst arbeiten, kann es sehr nützlich sein, die HTTP-Anforderung anzuzeigen und Nachrichten anzufordern. Hierfür können Sie die F12-Entwicklertools in Internet Explorer 9 verwenden. Drücken Sie in Internet Explorer 9 die Taste **F12** , um die Tools zu öffnen. Klicken Sie auf die Registerkarte **Netzwerk** , und drücken Sie **Aufzeichnung starten**. Kehren Sie nun zurück zur Webseite, und drücken Sie **F5** , um die Webseite neu zu laden. Internet Explorer erfasst den HTTP-Datenverkehr zwischen dem Browser und dem Webserver. In der Zusammenfassungs Ansicht wird der gesamte Netzwerk Datenverkehr für eine Seite angezeigt:

![](tutorial-your-first-web-api/_static/image14.png)

Suchen Sie den Eintrag für den relativen URI "API/Products/". Wählen Sie diesen Eintrag aus, und klicken Sie **auf zur detaillierten Ansicht**wechseln. In der Detailansicht gibt es Registerkarten, um die Anforderungs-und Antwortheader und Textkörper anzuzeigen. Wenn Sie z. b. auf die Registerkarte **Anforderungs Header** klicken, sehen Sie, dass der Client &quot;Anwendung/JSON-&quot; im Accept-Header angefordert hat.

![](tutorial-your-first-web-api/_static/image15.png)

Wenn Sie auf die Registerkarte "Antworttext" klicken, können Sie sehen, wie die Produktliste in JSON serialisiert wurde. Andere Browser verfügen über eine ähnliche Funktionalität. Ein weiteres nützliches Tool ist " [fddler](http://www.fiddler2.com/fiddler2/)", ein webdebugproxy. Sie können mit "fddler" den HTTP-Datenverkehr anzeigen und auch HTTP-Anforderungen verfassen, um Ihnen die vollständige Kontrolle über die HTTP-Header in der Anforderung zu bieten.

## <a name="see-this-app-running-on-azure"></a>Diese APP wird in Azure ausgeführt.

Möchten Sie sehen, dass die fertige Site als Live-Web-App ausgeführt wird? Sie können eine vollständige Version der APP für Ihr Azure-Konto bereitstellen, indem Sie einfach auf die folgende Schaltfläche klicken.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Sie benötigen ein Azure-Konto, um diese Lösung in Azure bereitzustellen. Wenn Sie noch nicht über ein Konto verfügen, haben Sie die folgenden Optionen:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) . Sie erhalten Gutschriften, die Sie verwenden können, um kostenpflichtige Azure-Dienste zu testen. selbst nach ihrer Nutzung können Sie das Konto behalten und kostenlose Azure-Dienste nutzen.
- [Aktivieren von MSDN-Abonnenten Vorteilen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : Ihr MSDN-Abonnement bietet Ihnen jeden Monat ein Guthaben, das Sie für kostenpflichtige Azure-Dienste nutzen können.

## <a name="next-steps"></a>Nächste Schritte

- Ein ausführlichere Beispiel für einen HTTP-Dienst, der Post-, Put-und DELETE-Aktionen und Schreibvorgänge in eine Datenbank unterstützt, finden [Sie unter Verwenden der Web-API 2 mit Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Weitere Informationen zum Erstellen von flüssigen und reaktionsfähigen Webanwendungen zusätzlich zu einem HTTP-Dienst finden Sie unter [ASP.net Single Page Application](../../../single-page-application/index.md).
- Weitere Informationen zum Bereitstellen eines Visual Studio-Webprojekts für Azure App Service finden Sie unter [Erstellen einer ASP.net-Web-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
