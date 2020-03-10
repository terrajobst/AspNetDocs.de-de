---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Aktionen und Funktionen in odata v4 mit ASP.net-Web-API 2,2 | Microsoft-Dokumentation
author: MikeWasson
description: In odata können Aktionen und Funktionen serverseitiges Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. In diesem Tutorial wird gezeigt, wie Sie...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448059"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Aktionen und Funktionen in odata v4 mit ASP.net-Web-API 2,2

von [Mike Wasson](https://github.com/MikeWasson)

> In odata können Aktionen und Funktionen serverseitiges Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. In diesem Tutorial wird gezeigt, wie Sie mithilfe der Web-API 2,2 Aktionen und Funktionen zu einem odata V4-Endpunkt hinzufügen. Das Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md) auf.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
> - Web-API 2,2
> - OData v4
> - Visual Studio 2013 (Visual Studio 2017 [hier](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)herunterladen)
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Tutorialversionen
>
> Informationen zu odata, Version 3, finden Sie [unter odata-Aktionen in ASP.net-Web-API 2](../odata-v3/odata-actions.md).

Der Unterschied zwischen *Aktionen* und *Funktionen* besteht darin, dass Aktionen Nebeneffekte haben können und Funktionen dies nicht tun. Sowohl Aktionen als auch Funktionen können Daten zurückgeben. Einige Verwendungsmöglichkeiten für Aktionen sind:

- Komplexe Transaktionen.
- Gleichzeitige Bearbeitung mehrerer Entitäten.
- Zulassen von Updates nur für bestimmte Eigenschaften einer Entität.
- Senden von Daten, die keine Entität sind.

Funktionen sind nützlich für die Rückgabe von Informationen, die nicht direkt einer Entität oder Auflistung entsprechen.

Eine Aktion (oder Funktion) kann eine einzelne Entität oder eine Auflistung als Ziel haben. In der odata-Terminologie ist dies die *Bindung*. Sie können auch &quot;ungebundene&quot; Aktionen/Funktionen haben, die als statische Vorgänge für den Dienst aufgerufen werden.

## <a name="example-adding-an-action"></a>Beispiel: Hinzufügen einer Aktion

Definieren wir eine Aktion, um ein Produkt zu bewerten.

> [!NOTE]
> Dieses Tutorial baut auf dem Tutorial [Erstellen eines odata V4-Endpunkts mit ASP.net-Web-API 2](create-an-odata-v4-endpoint.md) auf.

Fügen Sie zunächst ein `ProductRating` Modell hinzu, das die Bewertungen darstellt.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Fügen Sie auch ein **dbset** zur `ProductsContext`-Klasse hinzu, damit EF eine Bewertungstabelle in der Datenbank erstellt.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Hinzufügen der Aktion zum EDM

Fügen Sie in WebApiConfig.cs den folgenden Code hinzu:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Die **entitytypeconfiguration. Action** -Methode fügt dem Entity Data Model (EDM) eine Aktion hinzu. Die **Parameter** Methode gibt einen typisierten Parameter für die Aktion an.

Mit diesem Code wird auch der Namespace für das EDM festgelegt. Der Namespace ist wichtig, da der URI für die Aktion den voll qualifizierten Aktions Namen enthält:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> In einer typischen IIS-Konfiguration führt der Punkt in dieser URL dazu, dass IIS den Fehler 404 zurückgibt. Sie können dieses Problem beheben, indem Sie den folgenden Abschnitt zu Ihrer Web. config-Datei hinzufügen:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Fügen Sie eine Controller Methode für die Aktion hinzu.

Fügen Sie `ProductsController`die folgende Methode hinzu, um die &quot;Rate&quot; Aktion zu aktivieren:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Beachten Sie, dass der Methodenname mit dem Aktions Namen übereinstimmt. Das Attribut **[HttpPost]** gibt an, dass die Methode eine HTTP Post-Methode ist.

Zum Aufrufen der Aktion sendet der Client eine HTTP POST-Anforderung wie die folgende:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Die &quot;Rate&quot; Aktion ist an Produkt Instanzen gebunden, sodass der URI für die Aktion der voll qualifizierte Aktionsname ist, der an den Entitäts-URI angehängt ist. (Denken Sie daran, dass der EDM-Namespace auf &quot;productservice&quot;festgelegt wurde, sodass der voll qualifizierte Aktionsname &quot;productservice. Rate&quot;ist.)

Der Anforderungs Text enthält die Aktionsparameter als JSON-Nutzlast. Die Web-API konvertiert die JSON-Nutzlast automatisch in ein **odataaktionparameters** -Objekt, bei dem es sich nur um ein Wörterbuch von Parameterwerten handelt. Verwenden Sie dieses Wörterbuch, um auf die Parameter in ihrer Controller Methode zuzugreifen.

Wenn der Client die Aktionsparameter im falschen Format sendet, ist der Wert von **modelstate. IsValid** false. Aktivieren Sie dieses Flag in ihrer Controller Methode, und geben Sie einen Fehler zurück, wenn **IsValid** den Wert false hat.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Beispiel: Hinzufügen einer Funktion

Nun fügen wir eine odata-Funktion hinzu, die das teuerste Produkt zurückgibt. Wie zuvor besteht der erste Schritt darin, die Funktion dem EDM hinzuzufügen. Fügen Sie in WebApiConfig.cs den folgenden Code hinzu.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

In diesem Fall ist die-Funktion an die Products-Auflistung gebunden, nicht an einzelne Produkt Instanzen. Clients rufen die Funktion auf, indem Sie eine GET-Anforderung senden:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Hier ist die Controller Methode für diese Funktion:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Beachten Sie, dass der Methodenname mit dem Funktionsnamen übereinstimmt. Das Attribut **[HttpGet]** gibt an, dass die Methode eine HTTP Get-Methode ist.

Hier ist die HTTP-Antwort:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Beispiel: Hinzufügen einer ungebundenen Funktion

Das vorherige Beispiel war eine Funktion, die an eine Auflistung gebunden ist. Im nächsten Beispiel erstellen wir eine *ungebundene* Funktion. Ungebundene Funktionen werden als statische Vorgänge für den Dienst aufgerufen. Die-Funktion in diesem Beispiel gibt die Verkaufssteuer für eine bestimmte Postleitzahl zurück.

Fügen Sie in der webapiconfig-Datei dem EDM die-Funktion hinzu:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Beachten Sie, dass die **Funktion** anstelle des Entitäts Typs oder der Auflistung direkt für **odatamodelta Builder**aufgerufen wird. Dadurch wird dem Modell-Generator mitgeteilt, dass die Funktion nicht gebunden ist.

Dies ist die Controller Methode, die die-Funktion implementiert:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Es spielt keine Rolle, in welchem Web-API-Controller Sie diese Methode platzieren. Sie können Sie in `ProductsController`platzieren oder einen separaten Controller definieren. Das **[odataroute]** -Attribut definiert die URI-Vorlage für die Funktion.

Im folgenden finden Sie ein Beispiel für eine Client Anforderung:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Die HTTP-Antwort sieht wie folgt aus:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
