---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Aufrufen eines odata-Dienstanbieter von einemC#.NET-Client () | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie ein odata-Dienst C# von einer Client Anwendung aufgerufen wird. Im Tutorial verwendete Software Versionen Visual Studio 2013 (funktioniert mit Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498165"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a>Zugreifen auf einen OData-Dienst über einen .NET-Client (C#)

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In diesem Tutorial wird gezeigt, wie ein odata-Dienst C# von einer Client Anwendung aufgerufen wird.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funktioniert mit Visual Studio 2012)
> - [WCF Data Services-Clientbibliothek](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web-API 2. (Der odata-Beispiel Dienst wird mit der Web-API 2 erstellt, aber die Client Anwendung ist nicht von der Web-API abhängig.)

In diesem Tutorial werden die Schritte zum Erstellen einer Client Anwendung erläutert, die einen odata-Dienst aufruft. Der odata-Dienst macht die folgenden Entitäten verfügbar:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

In den folgenden Artikeln wird beschrieben, wie der odata-Dienst in der Web-API implementiert wird. (Sie müssen diese jedoch nicht lesen, um sich mit diesem Tutorial vertraut zu machen.)

- [Erstellen eines odata-Endpunkts in der Web-API 2](creating-an-odata-endpoint.md)
- [Odata-Entitäts Beziehungen in der Web-API 2](working-with-entity-relations.md)
- [OData-Aktionen in der Web-API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generieren des Dienst Proxys

Der erste Schritt besteht im Generieren eines Dienst Proxys. Der Dienst Proxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den odata-Dienst definiert. Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Beginnen Sie, indem Sie das odata-Dienstprojekt in Visual Studio öffnen. Drücken Sie STRG + F5, um den Dienst lokal in IIS Express auszuführen. Notieren Sie sich die lokale Adresse, einschließlich der Portnummer, die Visual Studio zuweist. Sie benötigen diese Adresse beim Erstellen des Proxys.

Öffnen Sie als nächstes eine andere Instanz von Visual Studio, und erstellen Sie ein Konsolen Anwendungsprojekt. Die Konsolenanwendung wird als odata-Client Anwendung verwendet. (Sie können das Projekt auch der gleichen Projekt Mappe wie der Dienst hinzufügen.)

> [!NOTE]
> Die restlichen Schritte beziehen sich auf das Konsolen Projekt.

Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf **Verweise** , und wählen Sie **Dienstverweis hinzufügen**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

Geben Sie im Dialogfeld **Dienstverweis hinzufügen** die Adresse des odata-Dienstanbieter ein:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

Dabei ist *Port* die Portnummer.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Geben Sie als **Namespace**"productservice" ein. Mit dieser Option wird der Namespace der Proxy Klasse definiert.

Klicken Sie auf **Go**. Visual Studio liest das odata-Metadatendokument, um die Entitäten im Dienst zu ermitteln.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Klicken Sie auf **OK** , um die Proxy Klasse zu Ihrem Projekt hinzuzufügen.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Erstellen einer Instanz der Dienst Proxy Klasse

Erstellen Sie in der `Main`-Methode wie folgt eine neue Instanz der Proxy Klasse:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Verwenden Sie erneut die tatsächliche Portnummer, in der der Dienst ausgeführt wird. Wenn Sie den Dienst bereitstellen, verwenden Sie den URI des Live Dienstanbieter. Sie müssen den Proxy nicht aktualisieren.

Der folgende Code fügt einen Ereignishandler hinzu, der die Anforderungs-URIs im Konsolenfenster ausgibt. Dieser Schritt ist nicht erforderlich, aber es ist interessant, die URIs für jede Abfrage anzuzeigen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Abfragen des Dienstanbieter

Der folgende Code Ruft die Liste der Produkte aus dem odata-Dienst ab.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Beachten Sie, dass Sie keinen Code schreiben müssen, um die HTTP-Anforderung zu senden oder die Antwort zu analysieren. Die Proxy Klasse führt dies automatisch aus, wenn Sie die `Container.Products` Auflistung in der **foreach** -Schleife auflisten.

Wenn Sie die Anwendung ausführen, sollte die Ausgabe wie folgt aussehen:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Um eine Entität anhand der ID zu erhalten, verwenden Sie eine `where`-Klausel.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Im restlichen Abschnitt dieses Themas zeige ich nicht die gesamte `Main` Funktion, sondern nur den Code, der zum Anrufen des Dienstanbieter benötigt wird.

## <a name="apply-query-options"></a>Abfrage Optionen anwenden

Odata definiert [Abfrage Optionen](../supporting-odata-query-options.md) , die verwendet werden können, um Daten zu filtern, zu sortieren, abzufragen usw. Im Dienst Proxy können Sie diese Optionen mithilfe verschiedener LINQ-Ausdrücke anwenden.

In diesem Abschnitt werden kurze Beispiele angezeigt. Weitere Informationen finden Sie im Thema über [Legungen zu LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) auf MSDN.

### <a name="filtering-filter"></a>Filtern ($Filter)

Verwenden Sie zum Filtern eine `where`-Klausel. Im folgenden Beispiel wird nach Produktkategorie gefiltert.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Dieser Code entspricht der folgenden odata-Abfrage.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Beachten Sie, dass der Proxy die `where`-Klausel in einen odata-`$filter` Ausdruck konvertiert.

### <a name="sorting-orderby"></a>Sortierung ($OrderBy)

Verwenden Sie zum Sortieren eine `orderby`-Klausel. Das folgende Beispiel sortiert nach Preis, von der höchsten zum niedrigsten.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Im folgenden finden Sie die entsprechende odata-Anforderung.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Client seitiges Paging ($Skip und $Top)

Bei großen Entitätenmengen kann es vorkommen, dass der Client die Anzahl der Ergebnisse einschränkt. Ein Client kann z. b. 10 Einträge gleichzeitig anzeigen. Dies wird als *Client seitiges Paging*bezeichnet. (Es gibt auch [Serverseitiges Paging](../supporting-odata-query-options.md#server-paging), bei dem der Server die Anzahl der Ergebnisse einschränkt.) Verwenden Sie zum Ausführen von Client seitigem Paging die LINQ **Skip** -und **Take** -Methoden. Im folgenden Beispiel werden die ersten 40-Ergebnisse ausgelassen und die nächsten 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Im folgenden finden Sie die entsprechende odata-Anforderung:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($Select) und erweitern ($Expand)

Verwenden Sie die `DataServiceQuery<t>.Expand`-Methode, um Verwandte Entitäten einzubeziehen. Wenn Sie z. b. die `Supplier` für jede `Product`einschließen möchten:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Im folgenden finden Sie die entsprechende odata-Anforderung:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Verwenden Sie die LINQ **Select** -Klausel, um die Form der Antwort zu ändern. Im folgenden Beispiel wird nur der Name jedes Produkts ohne weitere Eigenschaften abgerufen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Im folgenden finden Sie die entsprechende odata-Anforderung:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Eine SELECT-Klausel kann Verwandte Entitäten einschließen. Wenn Sie in diesem Fall " **Expand**;" nicht aufzurufen. der Proxy enthält in diesem Fall automatisch die Erweiterung. Das folgende Beispiel ruft den Namen und den Lieferanten der einzelnen Produkte ab.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Im folgenden finden Sie die entsprechende odata-Anforderung. Beachten Sie, dass Sie die Option **$Expand** enthält.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Weitere Informationen zu $SELECT und $Expand finden Sie unter [Verwenden von $SELECT, $Expand und $Value in der Web-API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Neue Entität hinzufügen

Um eine neue Entität zu einer Entitätenmenge hinzuzufügen, nennen Sie `AddToEntitySet`, wobei *EntitySet* der Name der Entitätenmenge ist. `AddToProducts` fügt z. b. der `Products` Entitätenmenge eine neue `Product` hinzu. Wenn Sie den Proxy generieren, erstellt WCF Data Services diese stark typisierten **AddTo** -Methoden automatisch.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Um einen Link zwischen zwei Entitäten hinzuzufügen, verwenden Sie die **AddLink** -Methode und die **SetLink** -Methode. Der folgende Code fügt einen neuen Lieferanten und ein neues Produkt hinzu und erstellt dann Links zwischen Ihnen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Verwenden Sie **AddLink** , wenn die Navigations Eigenschaft eine Auflistung ist. In diesem Beispiel fügen wir ein Produkt zur `Products`-Sammlung auf dem Lieferanten hinzu.

Verwenden Sie **SetLink** , wenn die Navigations Eigenschaft eine einzelne Entität ist. In diesem Beispiel legen wir die `Supplier`-Eigenschaft für das Produkt fest.

## <a name="update--patch"></a>Aktualisieren/Patch

Um eine Entität zu aktualisieren, müssen Sie die **UpdateObject** -Methode aufrufen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Das Update wird ausgeführt, wenn Sie **SaveChanges**aufzurufen. WCF sendet standardmäßig eine HTTP-Merge-Anforderung. Die **patchonupdate** -Option weist WCF an, stattdessen einen HTTP-Patch zu senden.

> [!NOTE]
> Warum Patchen und Zusammenführen? In der ursprünglichen HTTP 1,1-Spezifikation ([RCF 2616](http://tools.ietf.org/html/rfc2616)) wurde keine HTTP-Methode mit der Semantik "partielle Aktualisierung" definiert. Zur Unterstützung von Teil Updates hat die odata-Spezifikation die Merge-Methode definiert. In 2010 hat [RFC 5789](http://tools.ietf.org/html/rfc5789) die patchmethode für Teil Updates definiert. Einen Teil des Verlaufs finden Sie in diesem [Blogbeitrag](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) im Blogbeitrag WCF Data Services. Heute wird Patch für Merge bevorzugt. Der vom Web-API-Gerüst erstellte odata-Controller unterstützt beide Methoden.

Wenn Sie die gesamte Entität (Put Semantik) ersetzen möchten, geben Sie die **replaceonupdate** -Option an. Dies bewirkt, dass WCF eine HTTP PUT-Anforderung sendet.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Löschen einer Entität

Zum Löschen einer Entität wird **DeleteObject**aufgerufen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Aufrufen einer odata-Aktion

In odata können [Aktionen](odata-actions.md) serverseitiges Verhalten hinzugefügt werden, die nicht einfach als CRUD-Vorgänge für Entitäten definiert werden können.

Obwohl in den odata-Metadatendokumenten die Aktionen beschrieben werden, werden von der Proxy Klasse keine stark typisierten Methoden für Sie erstellt. Sie können eine odata-Aktion weiterhin mithilfe der generischen **Execute** -Methode aufrufen. Sie müssen jedoch die Datentypen der Parameter und den Rückgabewert kennen.

Beispielsweise übernimmt die `RateProduct` Aktion einen Parameter mit dem Namen "Bewertung" vom Typ "`Int32`" und gibt eine `double`zurück. Der folgende Code zeigt, wie diese Aktion aufgerufen wird.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Weitere Informationen finden Sie unter[Aufrufen von Dienst Vorgängen und-Aktionen](https://msdn.microsoft.com/library/hh230677.aspx).

Eine Möglichkeit besteht darin, die **Container** Klasse so zu erweitern, dass Sie eine stark typisierte Methode bereitstellt, die die Aktion aufruft:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
