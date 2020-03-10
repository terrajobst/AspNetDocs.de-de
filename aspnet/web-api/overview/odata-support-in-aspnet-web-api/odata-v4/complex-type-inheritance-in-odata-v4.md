---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Vererbung komplexer Typen in odata v4 mit ASP.net-Web-API | Microsoft-Dokumentation
author: microsoft
description: Gemäß der odata V4-Spezifikation kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein komplexer Typ ist ein strukturierter Typ ohne Schlüssel.) Web-API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448131"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Vererbung komplexer Typen in odata v4 mit ASP.net-Web-API

von [Microsoft](https://github.com/microsoft)

> Gemäß der odata V4- [Spezifikation](http://www.odata.org/documentation/odata-version-4-0/)kann ein komplexer Typ von einem anderen komplexen Typ erben. (Ein *komplexer* Typ ist ein strukturierter Typ ohne Schlüssel.) Die Web-API odata 5,3 unterstützt die Vererbung komplexer Typen.
> 
> In diesem Thema wird gezeigt, wie ein Entity Data Model (EDM) mit komplexen Vererbungs Typen erstellt wird. Den gesamten Quellcode finden Sie unter [odata-Beispiel für komplexe Typvererbung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Web-API-odata 5,3
> - OData v4

## <a name="model-hierarchy"></a>Modell Hierarchie

Um die komplexe Typvererbung zu veranschaulichen, verwenden wir die folgende Klassenhierarchie.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` ist ein abstrakter komplexer Typ. `Rectangle`, `Triangle`und `Circle` sind komplexe Typen, die von `Shape`abgeleitet sind, und `RoundRectangle` von `Rectangle`abgeleitet. `Window` ist ein Entitätstyp und enthält eine `Shape` Instanz.

Hier sind die CLR-Klassen, die diese Typen definieren.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Erstellen des EDM-Modells

Zum Erstellen des EDM können Sie **odataconaconmodelbuilder**verwenden, das die Vererbungs Beziehungen von den CLR-Typen leitet.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Außerdem können Sie das EDM mithilfe von **odatamodelta Builder**explizit erstellen. Dies erfordert mehr Code, bietet Ihnen jedoch mehr Kontrolle über das EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

In diesen beiden Beispielen wird das gleiche EDM-Schema erstellt.

## <a name="metadata-document"></a>Metadatendokument

Hier finden Sie das odata-Metadatendokument, das eine komplexe Typvererbung zeigt.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Im Metadatendokument können Sie Folgendes sehen:

- Der `Shape` komplexer Typ ist abstrakt.
- Die `Rectangle`, `Triangle`und `Circle` komplexen Typs verfügen über den Basistyp `Shape`.
- Der `RoundRectangle` Typ verfügt über den Basistyp `Rectangle`.

## <a name="casting-complex-types"></a>Umwandeln komplexer Typen

Das umwandeln komplexer Typen wird jetzt unterstützt. Mit der folgenden Abfrage wird z. b. ein `Shape` in eine `Rectangle`umgewandelt.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Hier ist die Antwort Nutzlast:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
