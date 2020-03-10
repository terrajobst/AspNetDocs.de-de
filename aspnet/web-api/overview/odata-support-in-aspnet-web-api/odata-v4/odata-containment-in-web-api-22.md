---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Kapselung in odata V4 mithilfe der Web-API 2,2 | Microsoft-Dokumentation
author: rick-anderson
description: 'In der Regel konnte auf eine Entität nur zugegriffen werden, wenn Sie in einer Entitätenmenge gekapselt wurde Odata v4 bietet jedoch zwei zusätzliche Optionen: Singleton und con...'
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421401"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a>Kapselung in odata V4 mithilfe der Web-API 2,2

von jinfu Tan

> In der Regel konnte auf eine Entität nur zugegriffen werden, wenn Sie in einer Entitätenmenge gekapselt wurde Odata v4 bietet jedoch zwei zusätzliche Optionen: Singleton und Containment, die beide von WebAPI 2,2 unterstützt werden.

In diesem Thema wird gezeigt, wie Sie eine Kapselung in einem odata-Endpunkt in WebAPI 2,2 definieren. Weitere Informationen zur Kapselung finden Sie unter [Containment ist mit odata V4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Informationen zum Erstellen eines odata V4-Endpunkts in der Web-API finden [Sie unter Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2,2](create-an-odata-v4-endpoint.md).

Zunächst erstellen wir ein Containment-Domänen Modell im odata-Dienst mit diesem Datenmodell:

![Datenmodell](odata-containment-in-web-api-22/_static/image1.png)

Ein Konto enthält viele paymentinstruments (PI), aber wir definieren keine Entitätenmenge für einen Pi. Stattdessen kann nur über ein Konto auf die PiS zugegriffen werden.

Sie können die in diesem Thema verwendete Lösung von [codeplex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)herunterladen.

## <a name="defining-the-data-model"></a>Definieren des Datenmodells

1. Definieren Sie die CLR-Typen.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    Das `Contained`-Attribut wird für Eigenschaften der Einschluss Navigation verwendet.
2. Generieren Sie das EDM-Modell basierend auf den CLR-Typen.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    Der `ODataConventionModelBuilder` behandelt das EDM-Modell, wenn das `Contained`-Attribut der entsprechenden Navigations Eigenschaft hinzugefügt wird. Wenn die Eigenschaft ein Sammlungstyp ist, wird auch eine `GetCount(string NameContains)` Funktion erstellt.

    Die generierten Metadaten sehen wie folgt aus:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    Das `ContainsTarget`-Attribut gibt an, dass die Navigations Eigenschaft eine Containment ist.

## <a name="define-the-containing-entity-set-controller"></a>Definieren des enthaltenden entitätenmengencontrollers

Enthaltene Entitäten haben keinen eigenen Controller. die Aktion wird im enthaltenden entitätenmengencontroller definiert. In diesem Beispiel gibt es einen accounst Controller, aber keinen paymentinstrumenscontroller.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Wenn der odata-Pfad 4 oder mehr Segmente ist, funktioniert nur das Attribut Routing, wie z. b. `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` im obigen Controller. Andernfalls funktionieren sowohl das Attribut als auch das herkömmliche Routing: beispielsweise `GetPayInPIs(int key)` Übereinstimmungen `GET ~/Accounts(1)/PayinPIs`.

*Vielen Dank für den ursprünglichen Inhalt dieses Artikels.*
