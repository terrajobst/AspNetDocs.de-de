---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Unterstützende odata-Aktionen in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'In odata können Aktionen serverseitiges Verhalten hinzugefügt werden, die nicht einfach als CRUD-Vorgänge für Entitäten definiert werden können. Einige Verwendungsmöglichkeiten für Aktionen sind: implementieren...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600349"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Unterstützende odata-Aktionen in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In odata können *Aktionen* serverseitiges Verhalten hinzugefügt werden, die nicht einfach als CRUD-Vorgänge für Entitäten definiert werden können. Einige Verwendungsmöglichkeiten für Aktionen sind:
> 
> - Implementieren komplexer Transaktionen.
> - Gleichzeitige Bearbeitung mehrerer Entitäten.
> - Zulassen von Updates nur für bestimmte Eigenschaften einer Entität.
> - Senden von Informationen an den Server, der nicht in einer Entität definiert ist.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Web-API 2
> - Odata, Version 3
> - Entity Framework 6

## <a name="example-rating-a-product"></a>Beispiel: bewerten eines Produkts

In diesem Beispiel möchten wir, dass Benutzer Produkte bewerten und dann die durchschnittlichen Bewertungen für die einzelnen Produkte verfügbar machen. In der-Datenbank speichern wir eine Liste mit Bewertungen, die in Produkte eingegeben werden.

Dies ist das Modell, mit dem die Bewertungen in Entity Framework dargestellt werden können:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Wir möchten jedoch nicht, dass Clients ein `ProductRating` Objekt in einer "Bewertungs Sammlung" veröffentlichen. Die Bewertung ist intuitiv mit der Produktsammlung verknüpft, und der Client sollte nur den Bewertungs Wert veröffentlichen.

Anstatt die normalen CRUD-Vorgänge zu verwenden, wird daher eine Aktion definiert, die ein Client für ein Produkt aufrufen kann. In der odata-Terminologie ist die-Aktion an Product-Entitäten *gebunden* .

>Aktionen haben Nebeneffekte auf dem Server. Aus diesem Grund werden Sie mithilfe von HTTP POST-Anforderungen aufgerufen. Aktionen können über Parameter und Rückgabe Typen verfügen, die in den Dienst Metadaten beschrieben werden. Der Client sendet die Parameter im Anforderungs Text, und der Server sendet den Rückgabewert im Antworttext. Zum Aufrufen der Aktion "Produkt Raten" sendet der Client wie folgt einen Post-Vorgang an einen URI:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Bei den Daten in der Post-Anforderung handelt es sich lediglich um die Produktbewertung:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Deklarieren Sie die Aktion im Entity Data Model

Fügen Sie die Aktion in Ihrer Web-API-Konfiguration dem Entity Data Model (EDM) hinzu:

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

In diesem Code wird "rateproduct" als eine Aktion definiert, die für Product-Entitäten ausgeführt werden kann. Außerdem wird deklariert, dass die Aktion einen **int** -Parameter mit dem Namen "Rating" annimmt und einen **int** -Wert zurückgibt.

## <a name="add-the-action-to-the-controller"></a>Hinzufügen der Aktion zum Controller

Die Aktion "rateproduct" ist an Product Entities gebunden. Fügen Sie dem Products-Controller eine Methode mit dem Namen `RateProduct` hinzu, um die Aktion zu implementieren:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Beachten Sie, dass der Methodenname mit dem Namen der Aktion im EDM übereinstimmt. Die-Methode verfügt über zwei Parameter:

- *Key*: der Schlüssel, der vom Produkt zu bewerten ist.
- *Parameter*: ein Wörterbuch mit Aktionsparameter Werten.

Wenn Sie die standardmäßigen Routing Konventionen verwenden, muss der Schlüsselparameter "Key" lauten. Es ist auch wichtig, das Attribut **[fromudatauri]** wie dargestellt einzubeziehen. Dieses Attribut weist die Web-API an, odata-Syntax Regeln zu verwenden, wenn der Schlüssel aus dem Anforderungs-URI analysiert wird.

Verwenden Sie das *Parameter* Wörterbuch, um die Aktionsparameter zu erhalten:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Wenn der Client die Aktionsparameter im richtigen Format sendet, ist der Wert von **modelstate. IsValid** "true". In diesem Fall können Sie das **odataaktionparameters** -Wörterbuch verwenden, um die Parameterwerte zu erhalten. In diesem Beispiel nimmt die `RateProduct` Aktion einen einzelnen Parameter mit dem Namen "Rating" an.

## <a name="action-metadata"></a>Aktions Metadaten

Um die Dienst Metadaten anzuzeigen, senden Sie eine GET-Anforderung an/odata/-$Metadata. Dies ist der Teil der Metadaten, der die `RateProduct` Aktion deklariert:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Das **FunctionImport** -Element deklariert die Aktion. Die meisten Felder sind selbsterklärend, aber zwei sind erwähnenswert:

- **Isbindable** bedeutet, dass die Aktion für die Ziel Entität mindestens einige Zeit aufgerufen werden kann.
- **Isalwaysbindable** bedeutet, dass die Aktion immer für die Ziel Entität aufgerufen werden kann.

Der Unterschied besteht darin, dass einige Aktionen für Clients immer verfügbar sind, andere Aktionen jedoch möglicherweise vom Zustand der Entität abhängen. Nehmen Sie beispielsweise an, dass Sie eine Aktion zum Kauf definieren. Sie können nur ein Element erwerben, das sich in der Aktienliste befindet. Wenn das Element nicht vorrätig ist, kann ein Client diese Aktion nicht aufrufen.

Wenn Sie das EDM definieren, erstellt die **Aktions** Methode eine immer bindbare Aktion:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Ich werde weiter unten in diesem Thema über nicht immer bindbare Aktionen (auch als *vorübergehende* Aktionen bezeichnet) sprechen.

## <a name="invoking-the-action"></a>Aufrufen der Aktion

Sehen wir uns nun an, wie ein Client diese Aktion aufruft. Angenommen, der Client möchte eine Bewertung von 2 für das Produkt mit der ID "4" abgeben. Im folgenden finden Sie ein Beispiel für eine Anforderungs Nachricht mit dem JSON-Format für den Anforderungs Text:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Die Antwortnachricht lautet wie folgt:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Binden einer Aktion an eine Entitätenmenge

Im vorherigen Beispiel ist die Aktion an eine einzelne Entität gebunden: der Client bewertet ein einzelnes Produkt. Sie können eine Aktion auch an eine Auflistung von Entitäten binden. Nehmen Sie einfach die folgenden Änderungen vor:

Fügen Sie im EDM die **Aktion der Auflistungs Eigenschaft der** Entität hinzu.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Lassen Sie in der Controller Methode den *Key* -Parameter Weg.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Nun ruft der Client die Aktion für die Products-Entitätenmenge auf:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Aktionen mit Sammlungs Parametern

Aktionen können über Parameter verfügen, die eine Auflistung von Werten annehmen. Verwenden Sie im EDM **collectionparameter&lt;t-&gt;** , um den-Parameter zu deklarieren.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Dadurch wird ein Parameter mit dem Namen "Ratings" deklariert, der eine Auflistung von **int** -Werten annimmt. In der Controller Methode erhalten Sie immer noch den Parameterwert aus dem **odataaktionparameters** -Objekt, aber der Wert ist nun ein **ICollection-&lt;int&gt;** Wert:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Vorübergehende Aktionen

Im Beispiel "rateproduct" können Benutzer immer ein Produkt bewerten, sodass die Aktion immer verfügbar ist. Einige Aktionen hängen jedoch vom Zustand der Entität ab. Beispielsweise ist in einem videovermietungs Dienst die Aktion "Auschecken" nicht immer verfügbar. (Es hängt davon ab, ob eine Kopie des Videos verfügbar ist.) Diese Art von Aktion wird als *vorübergehende* Aktion bezeichnet.

In den Dienst Metadaten ist **isalwaysbindable** für eine vorübergehende Aktion gleich false. Das ist eigentlich der Standardwert, sodass die Metadaten wie folgt aussehen:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Dies ist der Grund, warum dies wichtig ist: Wenn eine Aktion vorübergehend ist, muss der Server dem Client mitteilen, wann die Aktion verfügbar ist. Dies erfolgt durch Einschließen eines Links zur Aktion in der Entität. Im folgenden finden Sie ein Beispiel für eine Movie-Entität:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Die Eigenschaft "#Checkout" enthält einen Link zur Checkout-Aktion. Wenn die Aktion nicht verfügbar ist, lässt der Server den Link aus.

Rufen Sie zum Deklarieren einer vorübergehenden Aktion im EDM die **transientaction** -Methode auf:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Außerdem müssen Sie eine Funktion bereitstellen, die einen Aktions Link für eine bestimmte Entität zurückgibt. Legen Sie diese Funktion durch Aufrufen von **hasaction Link**fest. Sie können die Funktion als Lambda-Ausdruck schreiben:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Wenn die Aktion verfügbar ist, gibt der Lambda-Ausdruck einen Link zur Aktion zurück. Der odata-Serialisierer schließt diesen Link ein, wenn die Entität serialisiert wird. Wenn die Aktion nicht verfügbar ist, gibt die Funktion `null`zurück.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Beispiel für odata-Aktionen](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
