---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Entitäts Beziehungen in odata V4 mithilfe von ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten. Mithilfe von odata können Clients navigieren...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484461"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Entitäts Beziehungen in odata V4 mithilfe von ASP.net-Web-API 2,2

von [Mike Wasson](https://github.com/MikeWasson)

> Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten. Mithilfe von odata können Clients durch Entitäts Beziehungen navigieren. Wenn Sie ein Produkt erhalten, finden Sie den Lieferanten. Sie können auch Beziehungen erstellen oder entfernen. Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.
>
> In diesem Tutorial wird gezeigt, wie diese Vorgänge in odata V4 mithilfe von ASP.net-Web-API unterstützt werden. Das Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md)auf.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - Web-API 2,1
> - OData v4
> - Visual Studio 2013 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)herunterladen)
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Tutorialversionen
>
> Informationen zu odata, Version 3, finden Sie [unter unterstützen von Entitäts Beziehungen in odata V3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Hinzufügen einer Lieferanten Entität

> [!NOTE]
> Das Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md)auf.

Zuerst benötigen wir eine verwandte Entität. Fügen Sie im Ordner Models eine Klasse mit dem Namen `Supplier` hinzu.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Fügen Sie eine Navigations Eigenschaft zur `Product`-Klasse hinzu:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Fügen Sie der `ProductsContext`-Klasse ein neues **dbset** hinzu, damit Entity Framework die Lieferanten Tabelle in der Datenbank enthält.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Fügen Sie in WebApiConfig.cs dem Entity Data Model eine &quot;Suppliers&quot; Entitätenmenge hinzu:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Hinzufügen eines Suppliers-Controllers

Fügen Sie dem Ordner Controllers eine `SuppliersController` Klasse hinzu.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Ich zeige Ihnen nicht, wie CRUD-Vorgänge für diesen Controller hinzugefügt werden. Die Schritte sind identisch mit denen für den Produkte Controller (siehe [Erstellen eines odata V4-Endpunkts](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Beziehen verwandter Entitäten

Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Fügen Sie der `ProductsController`-Klasse die folgende Methode hinzu, um diese Anforderung zu unterstützen:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Diese Methode verwendet eine Standard Benennungs Konvention.

- Methodenname: getX, wobei X die Navigations Eigenschaft ist.
- Parameter Name: *Schlüssel*

Wenn Sie diese Benennungs Konvention befolgen, ordnet die Web-API die HTTP-Anforderung automatisch der Controller-Methode zu.

HTTP-Beispiel Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

HTTP-Beispiel Antwort:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Beziehen einer verknüpften Sammlung

Im vorherigen Beispiel hat ein Produkt einen Lieferanten. Eine Navigations Eigenschaft kann auch eine Auflistung zurückgeben. Der folgende Code Ruft die Produkte für einen Lieferanten ab:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

In diesem Fall gibt die Methode eine **iquervable** anstelle eines **singleresult-&lt;t zurück&gt;**

HTTP-Beispiel Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

HTTP-Beispiel Antwort:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Erstellen einer Beziehung zwischen Entitäten

Odata unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei vorhandenen Entitäten. In der odata V4-Terminologie ist die Beziehung eine &quot;Referenz&quot;. (In odata V3 wurde die Beziehung als *Link*bezeichnet. Die Protokoll Unterschiede sind für dieses Tutorial nicht von Bedeutung.)

Ein Verweis hat seinen eigenen URI, wobei das Formular `/Entity/NavigationProperty/$ref`ist. Hier ist beispielsweise der URI, der den Verweis zwischen einem Produkt und seinem Lieferanten adressiert:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Zum Hinzufügen einer Beziehung sendet der Client eine Post-oder PUT-Anforderung an diese Adresse.

- Put, wenn die Navigations Eigenschaft eine einzelne Entität ist, z. b. `Product.Supplier`.
- Post, wenn die Navigations Eigenschaft eine Auflistung ist, z. b. `Supplier.Products`.

Der Anforderungs Text enthält den URI der anderen Entität in der Beziehung. Hier ist eine Beispiel Anforderung:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

In diesem Beispiel sendet der Client eine PUT-Anforderung an `/Products(6)/Supplier/$ref`. Hierbei handelt es sich um den $ref-URI für die `Supplier` des Produkts mit der ID = 6. Wenn die Anforderung erfolgreich ausgeführt wird, sendet der Server eine Antwort vom Typ "204 (No Content)":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Hier ist die Controller Methode zum Hinzufügen einer Beziehung zu einem `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Der *NavigationProperty* -Parameter gibt an, welche Beziehung festgelegt werden soll. (Wenn für die Entität mehr als eine Navigations Eigenschaft vorhanden ist, können Sie weitere `case`-Anweisungen hinzufügen.)

Der *Link* -Parameter enthält den URI des Lieferanten. Die Web-API analysiert automatisch den Anforderungs Text, um den Wert für diesen Parameter zu erhalten.

Zum Nachschlagen des Lieferanten benötigen wir die ID (oder den Schlüssel), die Teil des *Link* Parameters ist. Verwenden Sie hierzu die folgende Hilfsmethode:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Diese Methode verwendet im Grunde die odata-Bibliothek, um den URI-Pfad in Segmente aufzuteilen, das Segment zu suchen, das den Schlüssel enthält, und den Schlüssel in den richtigen Typ zu konvertieren.

## <a name="deleting-a-relationship-between-entities"></a>Löschen einer Beziehung zwischen Entitäten

Zum Löschen einer Beziehung sendet der Client eine HTTP DELETE-Anforderung an den $ref-URI:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Hier ist die Controller Methode zum Löschen der Beziehung zwischen einem Produkt und einem Lieferanten:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

In diesem Fall ist `Product.Supplier` das &quot;1&quot; Ende einer 1: n-Beziehung, sodass Sie die Beziehung entfernen können, indem Sie `Product.Supplier` auf `null`festlegen.

Im &quot;viele&quot; Ende einer Beziehung muss der Client angeben, welche verwandte Entität entfernt werden soll. Hierzu sendet der Client den URI der verknüpften Entität in der Abfrage Zeichenfolge der Anforderung. Um z. b. "Product 1" aus "Supplier 1" zu entfernen:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Um dies in der Web-API zu unterstützen, müssen wir einen zusätzlichen Parameter in die `DeleteRef`-Methode einschließen. Hier ist die Controller Methode, mit der ein Produkt aus der `Supplier.Products` Beziehung entfernt wird.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Der *Schlüssel* Parameter ist der Schlüssel für den Lieferanten, und der *relatedkey* -Parameter ist der Schlüssel für das Produkt, das aus der `Products` Beziehung entfernt werden soll. Beachten Sie, dass Web-API automatisch den Schlüssel aus der Abfrage Zeichenfolge abruft.
