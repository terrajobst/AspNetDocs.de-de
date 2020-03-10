---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Routing Konventionen in ASP.net-Web-API 2 odata-ASP.NET 4. x
author: MikeWasson
description: Beschreibt die Routing Konventionen, die von der Web-API 2 in ASP.NET 4. x für odata-Endpunkte verwendet werden.
ms.author: riande
ms.date: 07/31/2013
ms.custom: seoapril2019
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63df4a82cd8df92631485b2544117844cfd0ca56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498195"
---
# <a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Routing Konventionen in ASP.net-Web-API 2 odata

von [Mike Wasson](https://github.com/MikeWasson)

> In diesem Artikel werden die Routing Konventionen beschrieben, die von der Web-API 2 in ASP.NET 4. x für odata-Endpunkte verwendet werden.

Wenn die Web-API eine odata-Anforderung abruft, ordnet Sie die Anforderung einem Controller Namen und einem Aktions Namen zu. Die Zuordnung basiert auf der HTTP-Methode und dem URI. Beispielsweise wird `GET /odata/Products(1)` `ProductsController.GetProduct`zugeordnet.

In Teil 1 dieses Artikels beschreibe ich die integrierten odata-Routing Konventionen. Diese Konventionen wurden speziell für odata-Endpunkte entwickelt und ersetzen das Standard Routing System der Web-API. (Die Ersetzung erfolgt beim Abrufen von **mapodataroute**.)

In Teil 2 zeige ich Ihnen, wie benutzerdefinierte Routing Konventionen hinzugefügt werden. Derzeit werden die integrierten Konventionen nicht den gesamten Bereich von odata-URIs abdecken, Sie können Sie jedoch erweitern, um zusätzliche Fälle zu behandeln.

- [Integrierte Routing Konventionen](#conventions)
- [Benutzerdefinierte Routing Konventionen](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Integrierte Routing Konventionen

Bevor ich die odata-Routing Konventionen in der Web-API beschreibe, ist es hilfreich, odata-URIs zu verstehen. Ein [odata-URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) besteht aus folgendem:

- Der Dienst Stamm
- Der Ressourcen Pfad.
- Abfrageoptionen

![](odata-routing-conventions/_static/image1.png)

Der wichtigste Teil für das Routing ist der Ressourcen Pfad. Der Ressourcen Pfad ist in Segmente unterteilt. `/Products(1)/Supplier` hat z. b. drei Segmente:

- `Products` verweist auf eine Entitätenmenge mit dem Namen "Products".
- `1` ist ein Entitäts Schlüssel, wobei eine einzelne Entität aus dem Satz ausgewählt wird.
- `Supplier` ist eine Navigations Eigenschaft, die eine verknüpfte Entität auswählt.

Mit diesem Pfad wird der Lieferant von Produkt 1 ausgewählt.

> [!NOTE]
> Odata-Pfadsegmente entsprechen nicht immer URI-Segmenten. Beispielsweise wird "1" als Pfad Segment betrachtet.

**Controller Namen.** Der Controller Name wird immer von der Entitätenmenge im Stammverzeichnis des Ressourcen Pfads abgeleitet. Wenn der Ressourcen Pfad z. b. `/Products(1)/Supplier`ist, sucht die Web-API nach einem Controller mit dem Namen `ProductsController`.

**Aktions Namen.** Aktions Namen werden von den Pfad Segmenten und dem Entity Data Model (EDM) abgeleitet, wie in den folgenden Tabellen aufgeführt. In einigen Fällen haben Sie zwei Möglichkeiten für den Aktions Namen. Beispiel: "Get" oder &quot;GetProducts&quot;.

**Abfragen von Entitäten**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel Aktion |
| --- | --- | --- | --- |
| Get/EntitySet | /Products | Getentityset oder Get | GetProducts |
| Get/EntitySet (Key) | /Products (1) | Getentitytype oder Get | GetProduct |
| Get/EntitySet (Key)/Cast | /Products(1)/Models.Book | Getentitytype oder Get | Getbook |

Weitere Informationen finden Sie unter [Erstellen eines schreibgeschützten odata-Endpunkts](odata-v3/creating-an-odata-endpoint.md).

**Erstellen, aktualisieren und Löschen von Entitäten**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel Aktion |
| --- | --- | --- | --- |
| Nach/EntitySet | /Products | Postentitytype oder Post | PostProduct |
| Put/EntitySet (Key) | /Products (1) | Putentitytype oder Put | Putproduct |
| Put/EntitySet (Key)/Cast | /Products(1)/Models.Book | Putentitytype oder Put | Putbook |
| Patch-/EntitySet (Schlüssel) | /Products (1) | Patchentitytype oder Patch | Patchprodukt |
| Patch/EntitySet (Schlüssel)/Cast | /Products(1)/Models.Book | Patchentitytype oder Patch | PatchBook |
| DELETE/EntitySet (Schlüssel) | /Products (1) | Deleteentitytype oder DELETE | DeleteProduct |
| DELETE/EntitySet (Key)/Cast | /Products(1)/Models.Book | Deleteentitytype oder DELETE | Deletebook |

**Abfragen einer Navigations Eigenschaft**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel Aktion |
| --- | --- | --- | --- |
| Get/EntitySet (Key)/Navigation | /Products (1)/Supplier | Getnavigationfromentitytype oder getnavigation | GetSupplierFromProduct |
| Get/EntitySet (Key)/Cast/Navigation | /Products(1)/Models.Book/Author | Getnavigationfromentitytype oder getnavigation | GetAuthorFromBook |

Weitere Informationen finden Sie unter [Arbeiten mit Entitäts Beziehungen](odata-v3/working-with-entity-relations.md).

**Erstellen und Löschen von Links**

| Anforderung | Beispiel-URI | Aktionsname |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /Products (1)/$Links/Supplier | CreateLink |
| Put/EntitySet (Key)/$Links/Navigation | /Products (1)/$Links/Supplier | CreateLink |
| DELETE /entityset(key)/$links/navigation | /Products (1)/$Links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers (1) | DeleteLink |

Weitere Informationen finden Sie unter [Arbeiten mit Entitäts Beziehungen](odata-v3/working-with-entity-relations.md).

**Eigenschaften**

*Erfordert Web-API 2*

| Anforderung | Beispiel-URI | Aktionsname | Beispiel Aktion |
| --- | --- | --- | --- |
| Get/EntitySet (Key)/Property | /Products (1)/Name | Getpropertyfromentitytype oder GetProperty | GetNameFromProduct |
| Get/EntitySet (Key)/Cast/Property | /Products(1)/Models.Book/Author | Getpropertyfromentitytype oder GetProperty | GetTitleFromBook |

**Aktionen**

| Anforderung | Beispiel-URI | Aktionsname | Beispiel Aktion |
| --- | --- | --- | --- |
| Post/EntitySet (Schlüssel)/Action | /Products (1)/Rate | Aktions nameonentitytype oder Aktionsname | Rateonproduct |
| Post/EntitySet (Schlüssel)/Cast/Action | /Products(1)/Models.Book/CheckOut | Aktions nameonentitytype oder Aktionsname | CheckOutOnBook |

Weitere Informationen finden Sie unter [odata-Aktionen](odata-v3/odata-actions.md).

**Methoden Signaturen**

Im folgenden finden Sie einige Regeln für die Methoden Signaturen:

- Wenn der Pfad einen Schlüssel enthält, sollte die Aktion über einen Parameter mit dem Namen *Key*verfügen.
- Wenn der Pfad einen Schlüssel in einer Navigations Eigenschaft enthält, sollte die Aktion einen Parameter namens *relatedkey*aufweisen.
- Ergänzen Sie *Key* -und *relatedkey* -Parameter mit dem Parameter **[fromudatauri]** .
- Post-und Put-Anforderungen nehmen einen Parameter des Entitäts Typs an.
- Patchanforderungen nehmen einen Parameter vom Typ **Delta&lt;t&gt;** auf, wobei *t* der Entitätstyp ist.

Im folgenden finden Sie ein Beispiel, das Methoden Signaturen für jede integrierte odata-Routing Konvention anzeigt.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Benutzerdefinierte Routing Konventionen

Derzeit decken die integrierten Konventionen nicht alle möglichen odata-URIs ab. Sie können neue Konventionen hinzufügen, indem Sie die **iodataroutingconvention** -Schnittstelle implementieren. Diese Schnittstelle verfügt über zwei Methoden:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **Selectcontroller** gibt den Namen des Controllers zurück.
- **SelectAction** gibt den Namen der Aktion zurück.

Wenn die Konvention für beide Methoden nicht auf diese Anforderung angewendet wird, sollte die Methode NULL zurückgeben.

Der **odatapath** -Parameter stellt den analysierten odata-Ressourcen Pfad dar. Sie enthält eine Liste von **[odatapathsegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** -Instanzen, eine für jedes Segment des Ressourcen Pfads. **Odatapathsegment** ist eine abstrakte Klasse. jeder Segmenttyp wird durch eine Klasse dargestellt, die von **odatapathsegment**abgeleitet ist.

Die **odatapath. TemplatePath** -Eigenschaft ist eine Zeichenfolge, die die Verkettung aller Pfadsegmente darstellt. Wenn der URI z. b. `/Products(1)/Supplier`ist, ist die Pfad Vorlage &quot;~/EntitySet/Key/Navigation&quot;. Beachten Sie, dass die Segmente nicht direkt den URI-Segmenten entsprechen. Beispielsweise wird der Entitäts Schlüssel (1) als sein eigenes **odatapathsegment**dargestellt.

In der Regel führt eine Implementierung von **iodataroutingconvention** Folgendes aus:

1. Vergleichen Sie die Pfad Vorlage, um festzustellen, ob diese Konvention auf die aktuelle Anforderung angewendet wird. Wenn dies nicht zutrifft, wird NULL zurückgegeben.
2. Wenn die Konvention zutrifft, verwenden Sie die Eigenschaften der **odatapathsegment** -Instanzen zum Ableiten von Controller-und Aktions Namen.
3. Fügen Sie für Aktionen dem Routen Wörterbuch alle Werte hinzu, die an die Aktionsparameter gebunden werden sollen (normalerweise Entitäts Schlüssel).

Sehen wir uns ein bestimmtes Beispiel an. Die integrierten Routing Konventionen unterstützen keine Indizierung in eine Navigations Auflistung. Anders ausgedrückt: Es gibt keine Konvention für URIs wie die folgenden:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Im folgenden finden Sie eine benutzerdefinierte Routing Konvention, die diesen Abfragetyp behandelt.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notizen:

1. Ich führe von **entityabtroutingconvention**aus, da die **selectcontroller** -Methode in dieser Klasse für diese neue Routing Konvention geeignet ist. Dies bedeutet, dass **selectcontroller**nicht erneut implementiert werden muss.
2. Die Konvention gilt nur für Get-Anforderungen und nur, wenn die Pfad Vorlage &quot;~/EntitySet/Key/Navigation/Key&quot;ist.
3. Der Aktionsname ist &quot;Get {EntityType}&quot;, wobei *{EntityType}* der Typ der Navigations Auflistung ist. Beispielsweise &quot;getsupplier&quot;. Sie können eine beliebige Benennungs Konvention verwenden, &#8212; um nur sicherzustellen, dass die Controller Aktionen übereinstimmen.
4. Die Aktion nimmt zwei Parameter mit dem Namen *Key* und *relatedkey*an. (Eine Liste mit einigen vordefinierten Parameternamen finden Sie unter [odatarouteconstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Der nächste Schritt besteht darin, die neue Konvention zur Liste der Routing Konventionen hinzuzufügen. Dies geschieht während der Konfiguration, wie im folgenden Code gezeigt:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Im folgenden finden Sie einige weitere Beispiel-Routing Konventionen, die für die Untersuchung nützlich sind:

- [Compositekeyroutingconvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [Customnavigationroutingconvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [Nonbindableaktionroutingconvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [Odataversionrouteeinschränkung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Natürlich ist die Web-API selbst Open Source, sodass Sie den [Quellcode](http://aspnetwebstack.codeplex.com/) für die integrierten Routing Konventionen sehen können. Diese werden im Namespace **System. Web. http. odata. Routing. Conventions** definiert.
