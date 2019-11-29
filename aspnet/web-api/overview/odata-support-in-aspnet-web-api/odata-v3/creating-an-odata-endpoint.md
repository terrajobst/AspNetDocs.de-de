---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Erstellen eines odata-V3-Endpunkts mit der Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Der Open Data Protocol (odata) ist ein Datenzugriffs Protokoll für das Web. Odata bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, Abfragen der Daten und Bearbeiten der Daten...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600434"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Erstellen eines odata-V3-Endpunkts mit der Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Der [Open Data Protocol](http://www.odata.org/) (odata) ist ein Datenzugriffs Protokoll für das Web. Odata bietet eine einheitliche Möglichkeit zum Strukturieren von Daten, Abfragen der Daten und Bearbeiten des Datasets mithilfe von CRUD-Vorgängen (erstellen, lesen, aktualisieren und löschen). Odata unterstützt die Formate AtomPub (XML) und JSON. Odata definiert auch eine Möglichkeit, Metadaten zu den Daten verfügbar zu machen. Clients können die Metadaten zum Ermitteln der Typinformationen und Beziehungen für das DataSet verwenden.
>
> ASP.net-Web-API erleichtert das Erstellen eines odata-Endpunkts für ein DataSet. Sie können genau steuern, welche odata-Vorgänge der Endpunkt unterstützt. Sie können mehrere odata-Endpunkte neben nicht-odata-Endpunkten hosten. Sie haben vollständige Kontrolle über das Datenmodell, die Back-End-Geschäftslogik und die Datenschicht.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web-API 2
> - Odata, Version 3
> - Entity Framework 6
> - [Webdebugproxy für das webdebugging (optional)](http://www.fiddler2.com)
>
> Die Web-API-odata-Unterstützung wurde in [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=282650)hinzugefügt In diesem Tutorial wird jedoch Gerüstbau verwendet, der in Visual Studio 2013 hinzugefügt wurde.

In diesem Tutorial erstellen Sie einen einfachen odata-Endpunkt, der von Clients abgefragt werden kann. Außerdem erstellen Sie einen C# Client für den Endpunkt. Nachdem Sie dieses Lernprogramm durchgeführt haben, zeigen Sie im nächsten Satz von Tutorials, wie Sie weitere Funktionen hinzufügen, einschließlich Entitäts Beziehungen, Aktionen und $Expand/$SELECT.

- [Erstellen des Visual Studio-Projekts](#create-project)
- [Hinzufügen eines Entitäts Modells](#add-model)
- [Hinzufügen eines odata-Controllers](#add-controller)
- [Hinzufügen des EDM und der Route](#edm)
- [Ausgangsdaten der Datenbank (optional)](#seed-db)
- [Untersuchen des odata-Endpunkts](#explore)
- [Odata-Serialisierungsformate](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

In diesem Tutorial erstellen Sie einen odata-Endpunkt, der grundlegende CRUD-Vorgänge unterstützt. Der Endpunkt macht eine einzelne Ressource, eine Liste mit Produkten, verfügbar. In späteren Tutorials werden weitere Funktionen hinzugefügt.

Starten Sie Visual Studio, und wählen Sie auf der Start Seite die Option **Neues Projekt** aus. Oder wählen Sie im Menü **Datei** die Option **neu** und dann **Projekt**aus.

Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und C# erweitern Sie den Knoten visuelle Knoten. Wählen Sie unter **Visualisierung C#** die Option **Web**aus. Wählen Sie **die Vorlage ASP.NET Webanwendung** aus.

![](creating-an-odata-endpoint/_static/image1.png)

Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus. Überprüfen Sie unter &quot;Ordner und Kern Verweise hinzufügen für...&quot;die **Web-API**. Klicken Sie auf **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Hinzufügen eines Entitäts Modells

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. Für dieses Tutorial benötigen wir ein Modell, das ein Produkt darstellt. Das Modell entspricht dem odata-Entitätstyp.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle. Klicken Sie im Kontextmenü auf **Hinzufügen** , und wählen Sie dann **Klasse**aus.

![](creating-an-odata-endpoint/_static/image3.png)

Benennen Sie die Klasse im Dialogfeld **Neues Element hinzufügen** &quot;Product&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Gemäß der Konvention werden Modellklassen in den Ordner "Models" eingefügt. Sie müssen diese Konvention nicht in ihren eigenen Projekten befolgen, aber wir verwenden Sie für dieses Tutorial.

Fügen Sie in der Datei Product.cs die folgende Klassendefinition hinzu:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Die ID-Eigenschaft wird als Entitäts Schlüssel verwendet. Clients können Produkte nach ID Abfragen. Dieses Feld wäre auch der Primärschlüssel in der Back-End-Datenbank.

Erstellen Sie jetzt das Projekt. Im nächsten Schritt verwenden wir ein Visual Studio-Gerüst, das Reflektion verwendet, um den Produkttyp zu suchen.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Hinzufügen eines odata-Controllers

Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Sie definieren einen separaten Controller für jede Entitätenmenge in Ihrem odata-Dienst. In diesem Tutorial erstellen wir einen einzelnen Controller.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller. Wählen Sie **Hinzufügen** , und wählen Sie dann **Controller**aus.

![](creating-an-odata-endpoint/_static/image5.png)

Wählen Sie im Dialogfeld **Gerüst hinzufügen** &quot;Web-API 2-odata-Controller mit Aktionen aus, indem Sie Entity Framework&quot;verwenden.

![](creating-an-odata-endpoint/_static/image6.png)

Benennen Sie den Controller im Dialogfeld **Controller hinzufügen** mit "ProductController". Aktivieren Sie das Kontrollkästchen &quot;Aktionen für Async-Controller verwenden&quot;. Wählen Sie in der Dropdown Liste **Modell** die Product-Klasse aus.

![](creating-an-odata-endpoint/_static/image7.png)

Klicken Sie auf die Schaltfläche **neuer Datenkontext...** . Belassen Sie den Standardnamen für den Daten Kontexttyp, und klicken Sie auf **Hinzufügen**.

![](creating-an-odata-endpoint/_static/image8.png)

Klicken Sie im Dialogfeld Controller hinzufügen auf Hinzufügen, um den Controller hinzuzufügen.

![](creating-an-odata-endpoint/_static/image9.png)

Hinweis: Wenn Sie eine Fehlermeldung erhalten, die &quot;besagt, dass beim Eingeben des&quot;Typs ein Fehler aufgetreten ist, stellen Sie sicher, dass Sie das Visual Studio-Projekt erstellt haben, nachdem Sie die Product-Klasse hinzugefügt haben. Das Gerüst verwendet Reflektion, um die Klasse zu finden.

![](creating-an-odata-endpoint/_static/image10.png)

Das Gerüst fügt dem Projekt zwei Code Dateien hinzu:

- Products.cs definiert den Web-API-Controller, der den odata-Endpunkt implementiert.
- ProductServiceContext.cs stellt Methoden zum Abfragen der zugrunde liegenden Datenbank mithilfe Entity Framework bereit.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Hinzufügen des EDM und der Route

Erweitern Sie in Projektmappen-Explorer den Ordner App\_Start, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs. Diese Klasse enthält Konfigurations Code für die Web-API. Ersetzen Sie diesen Code durch Folgendes:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Dieser Code führt zwei Aufgaben aus:

- Erstellt ein Entity Data Model (EDM) für den odata-Endpunkt.
- Fügt eine Route für den Endpunkt hinzu.

Ein EDM ist ein abstraktes Modell der Daten. Das EDM wird verwendet, um das Metadatendokument zu erstellen und die URIs für den Dienst zu definieren. Der **odataconaconmodelbuilder** erstellt mithilfe eines Satzes von Standard Benennungs Konventionen EDM ein EDM. Diese Vorgehensweise erfordert den geringsten Code. Wenn Sie mehr Kontrolle über das EDM haben möchten, können Sie das EDM mit der **odatamodelta Builder** -Klasse erstellen, indem Sie Eigenschaften, Schlüssel und Navigations Eigenschaften explizit hinzufügen.

Die **EntitySet** -Methode fügt dem EDM eine Entitätenmenge hinzu:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Die Zeichenfolge "Products" definiert den Namen der Entitätenmenge. Der Name des Controllers muss mit dem Namen der Entitätenmenge identisch sein. In diesem Tutorial heißt die Entitätenmenge "Products", und der Controller hat den Namen "`ProductsController`". Wenn Sie die Entitätenmenge "productset" genannt haben, benennen Sie den Controller `ProductSetController`. Beachten Sie, dass ein Endpunkt über mehrere Entitätenmengen verfügen kann. Nennen Sie **EntitySet&lt;t&gt;** für jede Entitätenmenge, und definieren Sie dann einen entsprechenden Controller.

Die **mapodataroute** -Methode fügt eine Route für den odata-Endpunkt hinzu.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Der erste Parameter ist ein Anzeige Name für die Route. Clients Ihres Dienstanbieter sehen diesen Namen nicht. Der zweite Parameter ist das URI-Präfix für den Endpunkt. Bei diesem Code ist der URI für die Products-Entitätenmenge http://<em>Hostname</em>/odata/Products. Die Anwendung kann mehr als einen odata-Endpunkt aufweisen. Nennen Sie für jeden Endpunkt <strong>mapodataroute</strong> , und geben Sie einen eindeutigen Routennamen und ein eindeutiges URI-Präfix an.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Ausgangsdaten der Datenbank (optional)

In diesem Schritt verwenden Sie Entity Framework, um für die Datenbank einen Ausgangswert für einige Testdaten zu erhalten. Dieser Schritt ist optional, aber Sie können den odata-Endpunkt sofort testen.

Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Dadurch wird ein Ordner namens Migrationen und eine Codedatei mit dem Namen Configuration.cs hinzugefügt.

![](creating-an-odata-endpoint/_static/image12.png)

Öffnen Sie diese Datei, und fügen Sie der `Configuration.Seed`-Methode den folgenden Code hinzu.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Diese Befehle generieren Code, der die Datenbank erstellt, und führt dann den Code aus.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Untersuchen des odata-Endpunkts

In diesem Abschnitt verwenden wir den [webdebugproxy von "fddler](http://www.fiddler2.com) ", um Anforderungen an den Endpunkt zu senden und die Antwort Nachrichten zu untersuchen. Auf diese Weise können Sie die Funktionen eines odata-Endpunkts verstehen.

Drücken Sie in Visual Studio F5, um das Debuggen zu starten. Standardmäßig öffnet Visual Studio Ihren Browser, um zu `http://localhost:*port*`, wobei *Port* die in den Projekteinstellungen konfigurierte Portnummer ist.

Sie können die Portnummer in den Projekteinstellungen ändern. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**aus. Wählen Sie im Fenster Eigenschaften die Option **Web**aus. Geben Sie Unterprojekt- **URL**die Portnummer ein.

### <a name="service-document"></a>Dienst Dokument

Das *Dienst Dokument* enthält eine Liste der Entitätenmengen für den odata-Endpunkt. Um das Dienst Dokument zu erhalten, senden Sie eine GET-Anforderung an den Stamm-URI des Dienstanbieter.

Geben Sie auf der Registerkarte **Composer** den folgenden URI ein: `http://localhost:port/odata/`, wobei *Port* die Portnummer ist.

![](creating-an-odata-endpoint/_static/image13.png)

Klicken Sie auf die Schaltfläche **Ausführen** . Der "fddler" sendet eine HTTP GET-Anforderung an Ihre Anwendung. Die Antwort sollte in der Liste Websitzungen angezeigt werden. Wenn alles funktioniert, lautet der Statuscode 200.

![](creating-an-odata-endpoint/_static/image14.png)

Doppelklicken Sie auf die Antwort in der Liste Websitzungen, um die Details der Antwortnachricht auf der Registerkarte Inspektoren anzuzeigen.

![](creating-an-odata-endpoint/_static/image15.png)

Die unformatierte http-Antwortnachricht sollte in etwa wie folgt aussehen:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Standardmäßig gibt die Web-API das Dienst Dokument im AtomPub-Format zurück. Fügen Sie der HTTP-Anforderung den folgenden Header hinzu, um JSON anzufordern:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Nun enthält die HTTP-Antwort eine JSON-Nutzlast:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dienstmetadatendokument

Das *dienstmetadatendokument* beschreibt das Datenmodell des Dienstanbieter mithilfe einer XML-Sprache, die als konzeptionelle Schema Definitions Sprache (CSDL) bezeichnet wird. Das Metadatendokument zeigt die Struktur der Daten im Dienst und kann zum Generieren von Client Code verwendet werden.

Um das Metadatendokument zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/$metadata`. Hier finden Sie die Metadaten für den in diesem Tutorial gezeigten Endpunkt.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Entitätenmenge

Um die Products-Entitätenmenge zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entität

Um ein einzelnes Produkt zu erhalten, senden Sie eine GET-Anforderung an `http://localhost:port/odata/Products(1)`, wobei "1" die Produkt-ID ist.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Odata-Serialisierungsformate

Odata unterstützt mehrere Serialisierungsformate:

- Atom-Pub (XML)
- JSON "Light" (eingeführt in odata v3)
- JSON "Verbose" (odata v2)

Standardmäßig verwendet die Web-API atompubjson "Light"-Format.

Um das AtomPub-Format zu erhalten, legen Sie den Accept-Header auf "Application/Atom + XML" fest. Im folgenden finden Sie ein Beispiel für einen Antworttext:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Sie können einen offensichtlichen Nachteil des Atom-Formats sehen: Es ist viel ausführlicher als das JSON-Licht. Wenn Sie jedoch über einen Client verfügen, der AtomPub versteht, könnte der Client dieses Format über JSON bevorzugen.

Hier ist die JSON Light-Version der gleichen Entität:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Das JSON Light-Format wurde in Version 3 des odata-Protokolls eingeführt. Aus Gründen der Abwärtskompatibilität kann ein Client das ältere JSON-Format "Verbose" anfordern. Um ausführliche JSON anzufordern, legen Sie den Accept-Header auf `application/json;odata=verbose`fest. Hier ist die ausführliche Version:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Dieses Format vermittelt mehr Metadaten im Antworttext, was einen beträchtlichen Aufwand für eine gesamte Sitzung verursachen kann. Außerdem wird eine Dereferenzierungsebene hinzugefügt, indem das Objekt in eine Eigenschaft mit dem Namen "d" umwickelt wird.

## <a name="next-steps"></a>Nächste Schritte

- [Entitäts Beziehungen hinzufügen](working-with-entity-relations.md)
- [Odata-Aktionen hinzufügen](odata-actions.md)
- [Odata-Dienst über einen .NET-Client aufzurufen](calling-an-odata-service-from-a-net-client.md)
