---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Unterstützen von Entitäts Beziehungen in odata V3 mit Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten. Mithilfe von odata können Clients navigieren...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600314"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Unterstützen von Entitäts Beziehungen in odata V3 mit der Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Die meisten Datasets definieren Beziehungen zwischen Entitäten: Kunden haben Aufträge. Bücher haben Autoren. Produkte haben Lieferanten. Mithilfe von odata können Clients durch Entitäts Beziehungen navigieren. Wenn Sie ein Produkt erhalten, finden Sie den Lieferanten. Sie können auch Beziehungen erstellen oder entfernen. Beispielsweise können Sie den Lieferanten für ein Produkt festlegen.
> 
> In diesem Tutorial wird gezeigt, wie diese Vorgänge in ASP.net-Web-API unterstützt werden. Das Tutorial baut auf dem Tutorial zum [Erstellen eines odata V3-Endpunkts mit der Web-API 2](creating-an-odata-endpoint.md)auf.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - Web-API 2
> - Odata, Version 3
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Hinzufügen einer Lieferanten Entität

Zuerst müssen wir dem odata-Feed einen neuen Entitätstyp hinzufügen. Wir fügen eine `Supplier` Klasse hinzu.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Diese Klasse verwendet eine Zeichenfolge für den Entitäts Schlüssel. In der Praxis ist dies möglicherweise weniger häufig als die Verwendung eines ganzzahligen Schlüssels. Es ist jedoch zu sehen, wie odata andere Schlüsseltypen außer Ganzzahlen behandelt.

Als Nächstes erstellen wir eine Beziehung, indem wir der `Product`-Klasse eine `Supplier`-Eigenschaft hinzufügen:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Fügen Sie der `ProductServiceContext`-Klasse ein neues **dbset** hinzu, damit Entity Framework die `Supplier`-Tabelle in der Datenbank einschließt.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

Fügen Sie in WebApiConfig.cs dem EDM-Modell eine Entität "Suppliers" hinzu:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Navigationseigenschaften

Um den Lieferanten für ein Produkt zu erhalten, sendet der Client eine GET-Anforderung:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Hier ist "Supplier" eine Navigations Eigenschaft für den `Product`-Typ. In diesem Fall verweist `Supplier` auf ein einzelnes Element, aber eine Navigations Eigenschaft kann auch eine Auflistung (1: n-oder m:n-Beziehung) zurückgeben.

Fügen Sie der `ProductsController`-Klasse die folgende Methode hinzu, um diese Anforderung zu unterstützen:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Der *Schlüssel* Parameter ist der Schlüssel des Produkts. Die-Methode gibt die zugehörige Entität&#8212;zurück, in diesem Fall eine `Supplier`-Instanz. Der Methodenname und der Parameter Name sind beide wichtig. Im Allgemeinen müssen Sie, wenn die Navigations Eigenschaft den Namen "X" hat, eine Methode mit dem Namen "getX" hinzufügen. Die Methode muss einen Parameter mit dem Namen "*Key*" akzeptieren, der mit dem Datentyp des übergeordneten Schlüssels übereinstimmt.

Es ist auch wichtig, das **[fromudatauri]** -Attribut in den *Key* -Parameter einzubeziehen. Dieses Attribut weist die Web-API an, odata-Syntax Regeln zu verwenden, wenn der Schlüssel aus dem Anforderungs-URI analysiert wird.

## <a name="creating-and-deleting-links"></a>Erstellen und Löschen von Links

Odata unterstützt das Erstellen oder Entfernen von Beziehungen zwischen zwei Entitäten. In der odata-Terminologie ist die Beziehung ein "Link". Jeder Link weist einen URI mit dem Formular *Entität*/$Links/*Entität*auf. Beispielsweise sieht der Link Zwischenprodukt und Lieferant wie folgt aus:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Zum Erstellen eines neuen Links sendet der Client eine Post-Anforderung an den Link-URI. Der Anforderungs Text ist der URI der Ziel Entität. Nehmen wir beispielsweise an, dass es einen Lieferanten mit dem Schlüssel "ctso" gibt. Zum Erstellen einer Verknüpfung zwischen "Produkt (1)" und "Lieferant (" ctso ") sendet der Client eine Anforderung wie die folgende:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Zum Löschen eines Links sendet der Client eine DELETE-Anforderung an den Link-URI.

**Erstellen von Links**

Fügen Sie der `ProductsController`-Klasse den folgenden Code hinzu, um es einem Client zu ermöglichen, Produktlieferanten Links zu erstellen:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Diese Methode nimmt drei Parameter an:

- *Key*: der Schlüssel für die übergeordnete Entität (das Produkt)
- *NavigationProperty*: der Name der Navigations Eigenschaft. In diesem Beispiel ist die einzige gültige Navigations Eigenschaft "Supplier".
- *Link*: der odata-URI der verknüpften Entität. Dieser Wert wird aus dem Anforderungs Text entnommen. Der Link-URI könnte z. b. "`http://localhost/odata/Suppliers('CTSO')`lauten, d. h. der Lieferant mit ID = ' ctso '.

Die-Methode verwendet den Link, um den Lieferanten zu suchen. Wenn der übereinstimmende Lieferant gefunden wird, legt die-Methode die `Product.Supplier`-Eigenschaft fest und speichert das Ergebnis in der Datenbank.

Der schwierigste Teil besteht darin, den Link-URI zu übernehmen. Im Grunde genommen müssen Sie das Ergebnis des Sendens einer GET-Anforderung an diesen URI simulieren. Die folgende Hilfsmethode zeigt, wie dies geschieht. Die-Methode ruft den Routing Prozess der Web-API auf und ruft eine **odatapath** -Instanz ab, die den analysierten odata-Pfad darstellt. Bei einem Link-URI sollte eines der Segmente der Entitäts Schlüssel sein. (Andernfalls sendet der Client einen ungültigen URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Löschen von Links**

Fügen Sie der `ProductsController`-Klasse den folgenden Code hinzu, um einen Link zu löschen:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

In diesem Beispiel ist die Navigations Eigenschaft eine einzelne `Supplier` Entität. Wenn die Navigations Eigenschaft eine Auflistung ist, muss der URI zum Löschen eines Links einen Schlüssel für die zugehörige Entität enthalten. Beispiel:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Diese Anforderung entfernt die Reihenfolge 1 von Kunde 1. In diesem Fall hat die Delta-Ink-Methode die folgende Signatur:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Der *relatedkey* -Parameter gibt den Schlüssel für die zugehörige Entität an. Suchen Sie in der `DeleteLink`-Methode die primäre Entität mit dem *Schlüssel* Parameter, suchen Sie die verknüpfte Entität nach dem Parameter *relatedkey* , und entfernen Sie dann die Zuordnung. Abhängig von Ihrem Datenmodell müssen Sie möglicherweise beide Versionen von `DeleteLink`implementieren. Die Web-API ruft die richtige Version auf Grundlage des Anforderungs-URI auf.
