---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Erstellen eines odata V4-Endpunkts mithilfe von ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: Der Open Data Protocol (odata) ist ein Datenzugriffs Protokoll für das Web. Odata bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datasets mithilfe von CRUD-Vorgängen...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484497"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Erstellen eines odata V4-Endpunkts mithilfe von ASP.net-Web-API 

> Der Open Data Protocol (odata) ist ein Datenzugriffs Protokoll für das Web. Odata bietet eine einheitliche Möglichkeit zum Abfragen und Bearbeiten von Datasets mithilfe von CRUD-Vorgängen (erstellen, lesen, aktualisieren und löschen).
>
> ASP.net-Web-API unterstützt sowohl V3 als auch V4 des Protokolls. Sie können sogar einen V4-Endpunkt haben, der parallel mit einem v3-Endpunkt ausgeführt wird.
>
> In diesem Tutorial wird gezeigt, wie Sie einen odata V4-Endpunkt erstellen, der CRUD-Vorgänge unterstützt.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - Web-API 5,2
> - OData v4
> - Visual Studio 2017 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/)herunterladen)
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Tutorialversionen
>
> Informationen zu odata, Version 3, finden Sie unter [Erstellen eines odata-V3-Endpunkts](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

Wählen Sie in Visual Studio im Menü **Datei** die Option **neu** &gt; **Projekt**aus.

Erweitern Sie **installierte** &gt;  **C# Visual** &gt; **Web**, und wählen Sie die Vorlage **ASP.net Web Application (.NET Framework)** aus. Nennen Sie das Projekt &quot;productservice&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Klicken Sie auf **OK**.

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Wählen Sie die Vorlage **Leer** aus. Wählen Sie unter **Ordner und Kern Verweise hinzufügen für:** die Option **Web-API**aus. Klicken Sie auf **OK**.

## <a name="install-the-odata-packages"></a>Installieren der odata-Pakete

Wählen Sie im Menü **Tools** **NuGet-Paket-Manager** &gt; **Paket-Manager-Konsole** aus. Geben Sie im Fenster der Paket-Manager-Konsole Folgendes ein:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Mit diesem Befehl werden die neuesten odata-nuget-Pakete installiert.

## <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Ein *Modell* ist ein Objekt, das eine Daten Entität in der Anwendung darstellt.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner „Modelle“. Wählen Sie im Kontextmenü &gt; **Klasse** **Hinzufügen** aus.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Gemäß der Konvention werden Modellklassen in den Ordner "Models" eingefügt, aber Sie müssen diese Konvention nicht in ihren eigenen Projekten befolgen.

Geben Sie der Klassen den Namen `Product`. Ersetzen Sie den Code in der Datei Product.cs durch den folgenden Code:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

Die `Id`-Eigenschaft ist der Entitäts Schlüssel. Clients können Entitäten nach Schlüssel Abfragen. Um z. b. das Produkt mit der ID 5 zu erhalten, wird der URI `/Products(5)`. Die `Id`-Eigenschaft ist auch der Primärschlüssel in der Back-End-Datenbank.

## <a name="enable-entity-framework"></a>Aktivieren von Entity Framework

In diesem Tutorial verwenden wir Entity Framework (EF) Code First, um die Back-End-Datenbank zu erstellen.

> [!NOTE]
> Die Web-API-odata erfordert EF nicht. Verwenden Sie eine beliebige Datenzugriffs Ebene, die Daten Bank Entitäten in Modelle übersetzen kann.

Installieren Sie zunächst das nuget-Paket für EF. Wählen Sie im Menü **Tools** **NuGet-Paket-Manager** &gt; **Paket-Manager-Konsole** aus. Geben Sie im Fenster der Paket-Manager-Konsole Folgendes ein:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Öffnen Sie die Datei Web. config, und fügen Sie den folgenden Abschnitt innerhalb des **Konfigurations** Elements nach dem **configabschnitts** -Element hinzu.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Mit dieser Einstellung wird eine Verbindungs Zeichenfolge für eine localdb-Datenbank hinzugefügt. Diese Datenbank wird verwendet, wenn Sie die APP lokal ausführen.

Fügen Sie als nächstes dem Ordner Models eine Klasse mit dem Namen `ProductsContext` hinzu:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Im Konstruktor gibt `"name=ProductsContext"` den Namen der Verbindungs Zeichenfolge an.

## <a name="configure-the-odata-endpoint"></a>Konfigurieren des odata-Endpunkts

Öffnen Sie die Datei-App\_Start/webapiconfig. cs. Fügen Sie die folgenden **using** -Anweisungen hinzu:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Fügen Sie dann der **Register** -Methode den folgenden Code hinzu:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Dieser Code führt zwei Aufgaben aus:

- Erstellt ein Entity Data Model (EDM).
- Fügt eine Route hinzu.

Ein EDM ist ein abstraktes Modell der Daten. Das EDM wird verwendet, um das dienstmetadatendokument zu erstellen. Die **odataconaconmodelbuilder** -Klasse erstellt ein EDM mithilfe von Standard Benennungs Konventionen. Diese Vorgehensweise erfordert den geringsten Code. Wenn Sie mehr Kontrolle über das EDM haben möchten, können Sie das EDM mit der **odatamodelta Builder** -Klasse erstellen, indem Sie Eigenschaften, Schlüssel und Navigations Eigenschaften explizit hinzufügen.

Eine *Route* teilt der Web-API mit, wie HTTP-Anforderungen an den Endpunkt weitergeleitet werden. Um eine odata V4-Route zu erstellen, rufen Sie die **mapodataserviceroute** -Erweiterungsmethode auf.

Wenn Ihre Anwendung über mehrere odata-Endpunkte verfügt, erstellen Sie jeweils eine separate Route. Übergeben Sie jeder Route einen eindeutigen Routennamen und ein Präfix.

## <a name="add-the-odata-controller"></a>Hinzufügen des odata-Controllers

Ein *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet. Sie erstellen einen separaten Controller für jede Entitätenmenge in Ihrem odata-Dienst. In diesem Tutorial erstellen Sie einen Controller für die `Product`-Entität.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controllers, und wählen Sie &gt; **Klasse** **Hinzufügen** aus. Geben Sie der Klassen den Namen `ProductsController`.

> [!NOTE]
> In der Version dieses Tutorials für odata V3 wird das **Hinzufügen eines Controller** Gerüsts verwendet. Derzeit gibt es kein Gerüst für odata v4.

Ersetzen Sie den Code in ProductsController.cs durch den folgenden Code.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Der Controller verwendet die `ProductsContext`-Klasse, um mithilfe von EF auf die Datenbank zuzugreifen. Beachten Sie, **dass der Controller** die verwerfen-Methode überschreibt, um den **produczcontext**zu verwerfen.

Dies ist der Ausgangspunkt für den Controller. Als Nächstes fügen wir Methoden für alle CRUD-Vorgänge hinzu.

## <a name="query-the-entity-set"></a>Abfragen der Entitätenmenge

Fügen Sie `ProductsController`die folgenden Methoden hinzu.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Die Parameter lose Version der `Get`-Methode gibt die gesamte Products-Auflistung zurück. Die `Get`-Methode mit einem *Key* -Parameter sucht ein Produkt nach seinem Schlüssel (in diesem Fall die `Id`-Eigenschaft).

Das **[enablequery]** -Attribut ermöglicht es Clients, die Abfrage mithilfe von Abfrage Optionen wie $Filter, $Sort und $Page zu ändern. Weitere Informationen finden Sie [unter unterstützen von odata-Abfrage Optionen](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Hinzufügen einer Entität zur Entitätenmenge

Fügen Sie `ProductsController`die folgende Methode hinzu, damit Clients der Datenbank ein neues Produkt hinzufügen können.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Odata unterstützt zwei verschiedene Semantik zum Aktualisieren von Entitäten: Patch und Put.

- Patch führt ein teilweises Update aus. Der Client gibt nur die Eigenschaften an, die aktualisiert werden sollen.
- Put ersetzt die gesamte Entität.

Der Nachteil von Put besteht darin, dass der Client Werte für alle Eigenschaften in der Entität senden muss, einschließlich der Werte, die nicht geändert werden. Die [odata-Spezifikation](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) gibt an, dass Patch bevorzugt wird.

In jedem Fall ist dies der Code für die Patch-und Put-Methoden:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Im Fall von Patch verwendet der Controller den **Delta&lt;t&gt;** Typ, um die Änderungen zu überprüfen.

## <a name="delete-an-entity"></a>Löschen einer Entität

Um Clients das Löschen eines Produkts aus der Datenbank zu ermöglichen, fügen Sie `ProductsController`die folgende Methode hinzu.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
