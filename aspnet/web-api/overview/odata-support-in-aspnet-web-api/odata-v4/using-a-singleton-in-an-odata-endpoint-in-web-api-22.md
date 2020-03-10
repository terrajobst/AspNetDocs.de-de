---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Erstellen eines Singleton in odata V4 mithilfe der Web-API 2,2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Thema wird gezeigt, wie ein Singleton in einem odata-Endpunkt in der Web-API 2,2 definiert wird.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504501"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Erstellen eines Singleton in odata V4 mithilfe der Web-API 2,2

von Zoe Luo

> In der Regel konnte auf eine Entität nur zugegriffen werden, wenn Sie in einer Entitätenmenge gekapselt wurde Odata v4 bietet jedoch zwei zusätzliche Optionen: Singleton und Containment, die beide von WebAPI 2,2 unterstützt werden.

In diesem Artikel wird gezeigt, wie ein Singleton in einem odata-Endpunkt in der Web-API 2,2 definiert wird. Informationen dazu, was ein Singleton ist und wie Sie davon profitieren können, finden Sie unter [Verwenden eines Singleton zum Definieren Ihrer speziellen Entität](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Informationen zum Erstellen eines odata V4-Endpunkts in der Web-API finden [Sie unter Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2,2](create-an-odata-v4-endpoint.md). 

Wir erstellen einen Singleton in Ihrem Web-API-Projekt mit dem folgenden Datenmodell:

![Datenmodell](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Ein Singleton namens `Umbrella` wird basierend auf dem Typ `Company`definiert, und eine Entitätenmenge mit dem Namen `Employees` wird basierend auf dem Typ `Employee`definiert.

Die in diesem Tutorial verwendete Lösung kann von [codeplex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)heruntergeladen werden.

## <a name="define-the-data-model"></a>Definieren des Datenmodells

1. Definieren Sie die CLR-Typen.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generieren Sie das EDM-Modell basierend auf den CLR-Typen.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Hier weist `builder.Singleton<Company>("Umbrella")` den Modell-Generator an, im EDM-Modell einen Singleton mit dem Namen `Umbrella` zu erstellen.

    Die generierten Metadaten sehen wie folgt aus:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    In den Metadaten sehen wir, dass die Navigations Eigenschaft `Company` in der `Employees` Entitätenmenge an den Singleton-`Umbrella`gebunden ist. Die Bindung wird automatisch durch `ODataConventionModelBuilder`durchgeführt, da nur `Umbrella` den `Company`-Typ aufweist. Wenn im Modell Mehrdeutigkeiten vorliegen, können Sie `HasSingletonBinding` verwenden, um eine Navigations Eigenschaft explizit an einen Singleton zu binden. `HasSingletonBinding` hat dieselbe Auswirkung wie das Verwenden des `Singleton`-Attributs in der CLR-Typdefinition:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definieren des Singleton-Controllers

Wie der EntitySet-Controller erbt der Singleton-Controller von `ODataController`, und der Name des Singleton Controllers sollte `[singletonName]Controller`sein.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Um verschiedene Arten von Anforderungen verarbeiten zu können, müssen Aktionen im Controller vordefiniert werden. **Attribut Routing** ist in WebAPI 2,2 standardmäßig aktiviert. Um z. b. eine Aktion zu definieren, mit der die Abfrage `Revenue` von `Company` mithilfe des Attribut Routings behandelt werden soll, verwenden Sie Folgendes:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Wenn Sie nicht bereit sind, Attribute für jede Aktion zu definieren, definieren Sie einfach Ihre Aktionen nach [odata-Routing Konventionen](../odata-routing-conventions.md). Da ein Schlüssel für das Abfragen eines Singleton nicht erforderlich ist, unterscheiden sich die im Singleton-Controller definierten Aktionen geringfügig von den Aktionen, die im EntitySet-Controller definiert sind.

Zur Referenz sind die Methoden Signaturen für jede Aktions Definition im Singleton-Controller unten aufgeführt.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Im Grunde ist dies alles, was Sie auf der Dienst Seite tun müssen. Das [Beispiel Projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) enthält den gesamten Code für die Lösung und den odata-Client, der die Verwendung des Singleton veranschaulicht. Der-Client wird erstellt, indem die Schritte unter [Erstellen einer odata V4-Client-App](create-an-odata-v4-client-app.md)ausgeführt werden.

erforderlich. 

*Vielen Dank für den ursprünglichen Inhalt dieses Artikels.*
