---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Teil 2: Erstellen der Domänen Modelle | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504273"
---
# <a name="part-2-creating-the-domain-models"></a>Teil 2: Erstellen der Domänen Modelle

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Hinzufügen von Modellen

Es gibt drei Möglichkeiten, um Entity Framework zu umgehen:

- Database-First: Sie beginnen mit einer Datenbank, und Entity Framework generiert den Code.
- Model-First: Sie beginnen mit einem visuellen Modell, und Entity Framework generiert sowohl die Datenbank als auch den Code.
- Code First: Sie beginnen mit Code, und Entity Framework generiert die Datenbank.

Wir verwenden den Code First-Ansatz. daher definieren wir zunächst unsere Domänen Objekte als POCOS (Plain-Old CLR Objects). Mit dem Code First-Ansatz benötigen Domänen Objekte keinen zusätzlichen Code zur Unterstützung der Datenbankebene, z. b. Transaktionen oder Persistenz. (Insbesondere müssen Sie nicht von der [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) -Klasse erben.) Sie können weiterhin Daten Anmerkungen verwenden, um zu steuern, wie Entity Framework das Datenbankschema erstellt.

Da POCOS keine zusätzlichen Eigenschaften enthalten, die den [Daten Bank Status](https://msdn.microsoft.com/library/system.data.entitystate.aspx)beschreiben, können Sie problemlos in JSON oder XML serialisiert werden. Dies bedeutet jedoch nicht, dass Sie Ihre Entity Framework Modelle immer direkt für Clients verfügbar machen sollten, wie später in diesem Tutorial zu sehen ist.

Wir erstellen die folgenden POCOS:

- Produkt
- Auftrag
- OrderDetail

Um jede Klasse zu erstellen, klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Modelle. Klicken Sie im Kontextmenü auf **Hinzufügen** , und wählen Sie dann **Klasse aus.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Fügen Sie mit der folgenden Implementierung eine `Product` Klasse hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Gemäß der Konvention verwendet Entity Framework die `Id`-Eigenschaft als Primärschlüssel und ordnet Sie einer Identitäts Spalte in der Datenbanktabelle zu. Wenn Sie eine neue `Product` Instanz erstellen, wird kein Wert für `Id`festgelegt, da die Datenbank den Wert generiert.

Das **Gerüst Column** -Attribut weist ASP.NET MVC an, die `Id`-Eigenschaft zu überspringen, wenn ein Editor-Formular erzeugt wird. Das **erforderliche** -Attribut wird verwendet, um das Modell zu validieren. Gibt an, dass die `Name`-Eigenschaft eine nicht leere Zeichenfolge sein muss.

Fügen Sie die `Order`-Klasse hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Fügen Sie die `OrderDetail`-Klasse hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Fremdschlüssel Beziehungen

Eine Bestellung enthält viele Bestelldetails, und jedes Bestell Detail bezieht sich auf ein einzelnes Produkt. Um diese Beziehungen darzustellen, definiert die `OrderDetail`-Klasse Eigenschaften mit dem Namen `OrderId` und `ProductId`. Entity Framework wird ableiten, dass diese Eigenschaften Fremdschlüssel darstellen, und fügt der Datenbank Foreign Key-Einschränkungen hinzu.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Die Klassen `Order` und `OrderDetail` enthalten außerdem Navigations Eigenschaften, die Verweise auf die verknüpften Objekte enthalten. Wenn Sie eine Bestellung haben, können Sie zu den Produkten in der Bestellung navigieren, indem Sie den Navigations Eigenschaften folgen.

Kompilieren Sie das Projekt jetzt. Entity Framework verwendet Reflektion, um die Eigenschaften der Modelle zu ermitteln. Daher ist eine kompilierte Assembly erforderlich, um das Datenbankschema zu erstellen.

## <a name="configure-the-media-type-formatters"></a>Konfigurieren der Medientyp-Formatierer

Ein [Medientyp-Formatierer](../../formats-and-model-binding/media-formatters.md) ist ein Objekt, das die Daten serialisiert, wenn die Web-API den HTTP-Antworttext schreibt. Die integrierten Formatierer unterstützen die JSON-und XML-Ausgabe. Standardmäßig serialisieren beide Formatierer alle Objekte nach Wert.

Bei der Serialisierung nach Wert wird ein Problem erstellt, wenn ein Objekt Diagramm zirkuläre Verweise enthält. Das ist genau der Fall bei den Klassen `Order` und `OrderDetail`, da jeder einen Verweis auf den anderen enthält. Der Formatierer befolgt die Verweise, schreibt jedes Objekt nach Wert und geht in Kreise. Daher müssen wir das Standardverhalten ändern.

Erweitern Sie in Projektmappen-Explorer den Ordner App\_Start, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs. Fügen Sie der `WebApiConfig`-Klasse folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Mit diesem Code wird der JSON-Formatierer für die Beibehaltung von Objekt verweisen festgelegt, und der XML-Formatierer wird vollständig aus der Pipeline entfernt. (Sie können den XML-Formatierer so konfigurieren, dass er Objekt Verweise beibehält, aber es ist ein wenig mehr Arbeit, und wir benötigen nur JSON für diese Anwendung. Weitere Informationen finden Sie unter [Behandeln von zirkulären Objekt verweisen](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-1.md)
> [Weiter](using-web-api-with-entity-framework-part-3.md)
