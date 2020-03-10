---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Öffnen von Typen in odata v4 mit ASP.net-Web-API | Microsoft-Dokumentation
author: microsoft
description: In odata V4 ist ein offener Typ ein strukturierter Typ, der neben den in der Typdefinition deklarierten Eigenschaften auch dynamische Eigenschaften enthält. Öffnen...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504579"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Öffnen von Typen in odata v4 mit ASP.net-Web-API

von [Microsoft](https://github.com/microsoft)

> In odata V4 ist ein *offener Typ* ein strukturierter Typ, der neben den in der Typdefinition deklarierten Eigenschaften auch dynamische Eigenschaften enthält. Mit offenen Typen können Sie Ihren Datenmodellen Flexibilität hinzufügen. In diesem Tutorial wird gezeigt, wie Sie Open types in ASP.net-Web-API odata verwenden.
> 
> In diesem Tutorial wird davon ausgegangen, dass Sie bereits wissen, wie ein odata-Endpunkt in ASP.net-Web-API erstellt wird. Wenn dies nicht der Anfang ist, lesen Sie zuerst [Erstellen eines odata V4-Endpunkts](create-an-odata-v4-endpoint.md) .
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Web-API-odata 5,3
> - OData v4

Zuerst einige odata-Terminologie:

- Entitätstyp: ein strukturierter Typ mit einem Schlüssel.
- Komplexer Typ: ein strukturierter Typ ohne Schlüssel.
- Open Type: ein Typ mit dynamischen Eigenschaften. Beide Entitäts Typen und komplexe Typen können geöffnet werden.

Der Wert einer dynamischen Eigenschaft kann ein primitiver Typ, ein komplexer Typ oder ein Enumerationstyp sein. oder eine Auflistung von diesen Typen. Weitere Informationen zu offenen Typen finden Sie in der [odata V4-Spezifikation](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installieren der webodata-Bibliotheken

Verwenden Sie den nuget-Paket-Manager zum Installieren der neuesten Web-API-odata-Bibliotheken. Im Fenster der Paket-Manager-Konsole:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definieren der CLR-Typen

Definieren Sie zunächst die EDM-Modelle als CLR-Typen.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Wenn die Entity Data Model (EDM) erstellt wird,

- `Category` ist ein Enumerationstyp.
- `Address` ist ein komplexer Typ. (Er verfügt nicht über einen Schlüssel, daher handelt es sich nicht um einen Entitätstyp.)
- `Customer` ist ein Entitätstyp. (Er verfügt über einen Schlüssel.)
- `Press` ist ein offener komplexer Typ.
- `Book` ist ein offener Entitätstyp.

Um einen geöffneten Typ zu erstellen, muss der CLR-Typ über eine Eigenschaft vom Typ `IDictionary<string, object>`verfügen, die die dynamischen Eigenschaften enthält.

## <a name="build-the-edm-model"></a>Erstellen des EDM-Modells

Wenn Sie **odataconaconmodelbuilder** zum Erstellen des EDM verwenden, werden `Press` und `Book` automatisch als offene Typen hinzugefügt, basierend auf dem vorhanden sein einer `IDictionary<string, object>`-Eigenschaft.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Außerdem können Sie das EDM mithilfe von **odatamodelta Builder**explizit erstellen.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Hinzufügen eines odata-Controllers

Fügen Sie als nächstes einen odata-Controller hinzu. Für dieses Tutorial verwenden wir einen vereinfachten Controller, der nur Get-und Post-Anforderungen unterstützt und eine in-Memory-Liste zum Speichern von Entitäten verwendet.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Beachten Sie, dass die erste `Book` Instanz keine dynamischen Eigenschaften hat. Die zweite `Book` Instanz verfügt über die folgenden dynamischen Eigenschaften:

- "Veröffentlicht": primitiver Typ
- "Authors": Auflistung primitiver Typen
- "Othercategories": Sammlung von Enumerationstypen.

Außerdem weist die `Press`-Eigenschaft dieser `Book` Instanz die folgenden dynamischen Eigenschaften auf:

- "Blog": primitiver Typ
- "Address": komplexer Typ

## <a name="query-the-metadata"></a>Abfragen der Metadaten

Um das odata-Metadatendokument zu erhalten, senden Sie eine GET-Anforderung an `~/$metadata`. Der Antworttext sollte in etwa wie folgt aussehen:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Im Metadatendokument können Sie Folgendes sehen:

- Für die `Book`-und `Press` Typen ist der Wert des `OpenType`-Attributs "true". Die `Customer`-und `Address` Typen haben dieses Attribut nicht.
- Der `Book` Entitätstyp verfügt über drei deklarierte Eigenschaften: ISBN, Title und drücken. Die odata-Metadaten enthalten nicht die `Book.Properties`-Eigenschaft aus der CLR-Klasse.
- Entsprechend verfügt der `Press` Complex-Typ nur über zwei deklarierte Eigenschaften: "Name" und "Category". Die Metadaten enthalten nicht die `Press.DynamicProperties`-Eigenschaft aus der CLR-Klasse.

## <a name="query-an-entity"></a>Abfragen einer Entität

Um das Buch mit ISBN gleich "978-0-7356-7942-9" zu erhalten, senden Sie eine GET-Anforderung an `~/Books('978-0-7356-7942-9')`. Der Antworttext sollte in etwa wie folgt aussehen. (Eingerückt, um Sie besser lesbar zu machen.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Beachten Sie, dass die dynamischen Eigenschaften Inline mit den deklarierten Eigenschaften enthalten sind.

## <a name="post-an-entity"></a>Eine Entität Posten

Um eine Buch Entität hinzuzufügen, senden Sie eine Post-Anforderung an `~/Books`. Der Client kann dynamische Eigenschaften in der Anforderungs Nutzlast festlegen.

Im folgenden finden Sie ein Beispiel für eine Anforderung. Beachten Sie die Eigenschaften "Price" und "veröffentlicht".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Wenn Sie in der Controller Methode einen Haltepunkt festlegen, sehen Sie, dass die Web-API diese Eigenschaften dem `Properties` Wörterbuch hinzugefügt hat.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Odata-Open-Type-Beispiel](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
