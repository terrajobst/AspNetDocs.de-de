---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attribut Routing in ASP.net-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446997"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Attribut Routing in ASP.net-Web-API 2

von [Mike Wasson](https://github.com/MikeWasson)

Das *Routing* ist die Art und Weise, wie die Web-API einem URI eine Aktion entspricht Web-API 2 unterstützt eine neue Art von Routing namens *Attribut Routing*. Wie der Name schon sagt, verwendet das Attribut Routing Attribute zum Definieren von Routen. Das Attribut Routing ermöglicht Ihnen mehr Kontrolle über die URIs in Ihrer Web-API. Beispielsweise können Sie ganz einfach URIs erstellen, die Ressourcen Hierarchien beschreiben.

Der frühere Routing Stil, der als Konvention-basiertes Routing bezeichnet wird, wird weiterhin vollständig unterstützt. Tatsächlich können Sie beide Techniken im gleichen Projekt kombinieren.

In diesem Thema wird gezeigt, wie das Attribut Routing aktiviert wird und die verschiedenen Optionen für das Attribut Routing beschrieben werden. Ein End-to-End-Tutorial, in dem Attribut Routing verwendet wird, finden Sie unter [Erstellen einer Rest-API mit Attribut Routing in der Web-API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Voraussetzungen

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition

Verwenden Sie alternativ den nuget-Paket-Manager, um die erforderlichen Pakete zu installieren. Wählen Sie **im Menü Extras** in Visual Studio **nuget-Paket-Manager**und dann **Paket-Manager-Konsole**aus. Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Gründe für Attribut Routing

In der ersten Version der Web-API wurde ein *Konventionen basiertes* Routing verwendet. Bei dieser Art von Routing definieren Sie eine oder mehrere Routen Vorlagen, bei denen es sich im Grunde um parametrisierte Zeichen folgen handelt. Wenn das Framework eine Anforderung empfängt, entspricht es dem URI für die Routen Vorlage. (Weitere Informationen zum Konventionen basierten Routing finden Sie unter [Routing in ASP.net-Web-API](routing-in-aspnet-web-api.md).

Ein Vorteil des Konventionen basierten Routings besteht darin, dass Vorlagen an einer zentralen Stelle definiert werden und die Routing Regeln einheitlich auf alle Controller angewendet werden. Leider ist es bei einem Konventionen basierten Routing schwierig, bestimmte URI-Muster zu unterstützen, die in Rest-APIs häufig vorkommen. Beispielsweise enthalten Ressourcen häufig untergeordnete Ressourcen: Kunden haben Aufträge, Filme haben Actors, Bücher haben Autoren und so weiter. Es ist natürlich, URIs zu erstellen, die diese Beziehungen widerspiegeln:

`/customers/1/orders`

Diese Art von URI ist schwierig zu erstellen, wenn ein Konventionen-basiertes Routing verwendet wird. Obwohl es möglich ist, können die Ergebnisse nicht gut skaliert werden, wenn Sie über viele Controller oder Ressourcentypen verfügen.

Beim Attribut Routing ist es trivial, eine Route für diesen URI zu definieren. Sie fügen der Controller Aktion einfach ein Attribut hinzu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Im folgenden finden Sie einige andere Muster, die das Attribut Routing leicht macht.

**API-Versionsverwaltung**

In diesem Beispiel würde "/API/v1/Products" an einen anderen Controller als "/API/v2/Products" weitergeleitet werden.

`/api/v1/products`
`/api/v2/products`

**Überladene URI Segmente**

In diesem Beispiel ist "1" eine Bestellnummer, aber "Pending" wird einer Auflistung zugeordnet.

`/orders/1`
`/orders/pending`

**Mehrere Parametertypen**

In diesem Beispiel ist "1" eine Bestellnummer, aber "2013/06/16" gibt ein Datum an.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Aktivieren von Attribut Routing

Um das Attribut Routing zu aktivieren, wird **maphttpattributeroutes** während der Konfiguration aufgerufen. Diese Erweiterungsmethode wird in der **System. Web. http. httpconfigurationextensions** -Klasse definiert.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Das Attribut Routing kann mit einem [Konvention-basierten](routing-in-aspnet-web-api.md) Routing kombiniert werden. Um auf Konventionen basierende Routen zu definieren, nennen Sie die **maphttproute** -Methode.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Weitere Informationen zum Konfigurieren der Web-API finden Sie unter [Konfigurieren von ASP.net-Web-API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Hinweis: Migrieren von der Web-API 1

Vor der Web-API 2 hat die Web-API-Projektvorlagen Code wie den folgenden generiert:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Wenn das Attribut Routing aktiviert ist, löst dieser Code eine Ausnahme aus. Wenn Sie ein vorhandenes Web-API-Projekt aktualisieren, um Attribut Routing zu verwenden, stellen Sie sicher, dass Sie diesen Konfigurations Code wie folgt aktualisieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Weitere Informationen finden Sie unter [Konfigurieren der Web-API mit ASP.net-Hosting](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Hinzufügen von Routen Attributen

Im folgenden finden Sie ein Beispiel für eine Route, die mit einem-Attribut definiert wird:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Die Zeichenfolge &quot;Customers/{CustomerID}/Orders&quot; ist die URI-Vorlage für die Route. Die Web-API versucht, den Anforderungs-URI mit der Vorlage zu vergleichen. In diesem Beispiel sind "Customers" und "Orders" literalsegmente, und "{CustomerID}" ist ein Variablen Parameter. Die folgenden URIs entsprechen dieser Vorlage:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Sie können die Übereinstimmung mithilfe von [Einschränkungen](#constraints)einschränken, die weiter unten in diesem Thema beschrieben werden.

Beachten Sie, dass der &quot;{CustomerID}&quot; Parameter in der Routen Vorlage mit dem Namen des *CustomerID-* Parameters in der-Methode übereinstimmt. Wenn die Web-API die Controller Aktion aufruft, versucht Sie, die Routen Parameter zu binden. Wenn der URI z. b. `http://example.com/customers/1/orders`ist, versucht die Web-API, den Wert "1" an den Parameter " *CustomerID* " in der Aktion zu binden.

Eine URI-Vorlage kann mehrere Parameter aufweisen:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Alle Controller Methoden, die nicht über ein Routen Attribut verfügen, verwenden ein auf Konventionen basierendes Routing. Auf diese Weise können Sie beide Routing Typen im selben Projekt kombinieren.

## <a name="http-methods"></a>HTTP-Methoden

Die Web-API wählt auch Aktionen basierend auf der HTTP-Methode der Anforderung aus (Get, Post usw.). Standardmäßig sucht die Web-API nach einer Entsprechung ohne Beachtung der Groß-/Kleinschreibung mit dem Anfang des Controller Methoden namens. Beispielsweise entspricht eine Controller Methode namens `PutCustomers` einer HTTP PUT-Anforderung.

Sie können diese Konvention überschreiben, indem Sie die-Methode mit den folgenden Attributen versehen:

- **HttpDelete**
- **[HttpGet]**
- **[Httphead]**
- **[Httpoptions]**
- **[Httppatch]**
- **HttpPost**
- **HttpPut**

Im folgenden Beispiel wird die Methode "foratebook" http Post-Anforderungen zugeordnet.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Verwenden Sie für alle anderen HTTP-Methoden, einschließlich nicht-standardmäßiger Methoden, das Attribut " **Accept tverbs** ", das eine Liste von HTTP-Methoden annimmt.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Routen Präfixe

Häufig beginnen die Routen in einem Controller mit dem gleichen Präfix. Beispiel:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Sie können ein gemeinsames Präfix für einen gesamten Controller mit dem **[routeprefix]** -Attribut festlegen:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Verwenden Sie eine Tilde (~) für das Method-Attribut, um das Routen Präfix zu überschreiben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Das Routen Präfix kann Parameter enthalten:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Routen Einschränkungen

Mit Routen Einschränkungen können Sie einschränken, wie die Parameter in der Routen Vorlage abgeglichen werden. Die allgemeine Syntax ist &quot;{Parameter: Einschränkung}&quot;. Beispiel:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Hier wird die erste Route nur ausgewählt, wenn die &quot;-ID&quot; Segment des URIs eine ganze Zahl ist. Andernfalls wird die zweite Route ausgewählt.

In der folgenden Tabelle sind die unterstützten Einschränkungen aufgeführt.

| Constraint | Beschreibung | Beispiel |
| --- | --- | --- |
| Alpha | Entspricht groß-oder Kleinbuchstaben lateinische Alphabet Zeichen (a-z, a-z) | {x:alpha} |
| bool | Entspricht einem booleschen Wert. | {x:bool} |
| datetime | Entspricht einem **DateTime** -Wert. | {x:datetime} |
| Decimal | Entspricht einem Dezimalwert. | {x:decimal} |
| Doppelt | Entspricht einem 64-Bit-Gleit Komma Wert. | {x:double} |
| float | Entspricht einem 32-Bit-Gleit Komma Wert. | {x:float} |
| guid | Entspricht einem GUID-Wert. | {x:guid} |
| int | Entspricht einem ganzzahligen Wert von 32 Bit. | {x:int} |
| Länge | Entspricht einer Zeichenfolge mit der angegebenen Länge oder innerhalb eines angegebenen Längen Bereichs. | {x:length (6)} {x:length (1, 20)} |
| long | Entspricht einem ganzzahligen Wert von 64 Bit. | {x:long} |
| max | Entspricht einer ganzen Zahl mit einem maximalen Wert. | {x:max(10)} |
| MaxLength | Entspricht einer Zeichenfolge mit einer maximalen Länge. | {x:maxLength (10)} |
| Min. | Entspricht einem Integer-Wert mit einem Minimalwert. | {x:min (10)} |
| MinLength | Entspricht einer Zeichenfolge mit einer Mindestlänge. | {x:minLength (10)} |
| range | Entspricht einer ganzen Zahl innerhalb eines Wertebereichs. | {x:Range (10, 50)} |
| regex | Entspricht einem regulären Ausdruck. | {x:Regex (^ \d{3}-\d{3}-\d{4}$)} |

Beachten Sie, dass einige der Einschränkungen, z. b. &quot;min&quot;, Argumente in Klammern setzen. Sie können mehrere Einschränkungen auf einen Parameter anwenden, getrennt durch einen Doppelpunkt.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Benutzerdefinierte Routeneinschränkungen

Sie können benutzerdefinierte Routen Einschränkungen erstellen, indem Sie die **ihttprouteeinschränkung** -Schnittstelle implementieren. Beispielsweise schränkt die folgende Einschränkung einen Parameter auf einen ganzzahligen Wert ungleich 0 (null) ein.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Der folgende Code zeigt, wie Sie die Einschränkung registrieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Nun können Sie die Einschränkung in Ihren Routen anwenden:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Sie können auch die gesamte **defaultinlineconstraintresolver** -Klasse ersetzen, indem Sie die **iinlineconstraintresolver** -Schnittstelle implementieren. Dadurch werden alle integrierten Einschränkungen ersetzt, es sei denn, ihre Implementierung von **iinlineconstraintresolver** fügt Sie explizit hinzu.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Optionale URI-Parameter und Standardwerte

Sie können einen URI-Parameter optional machen, indem Sie dem Routen Parameter ein Fragezeichen hinzufügen. Wenn ein Routen Parameter optional ist, müssen Sie einen Standardwert für den Methoden Parameter definieren.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In diesem Beispiel geben `/api/books/locale/1033` und `/api/books/locale` dieselbe Ressource zurück.

Alternativ können Sie einen Standardwert in der Routen Vorlage wie folgt angeben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Dies ist fast identisch mit dem vorherigen Beispiel, aber es gibt einen geringfügigen Unterschied des Verhaltens, wenn der Standardwert angewendet wird.

- Im ersten Beispiel ("{LCID: int?}") wird der Standardwert 1033 direkt dem-Methoden Parameter zugewiesen, sodass der-Parameter diesen exakten Wert erhält.
- Im zweiten Beispiel ("{LCID: int = 1033}") durchläuft der Standardwert "1033" den Modell Bindungsprozess. Der Standardmodell Binder konvertiert "1033" in den numerischen Wert 1033. Allerdings könnten Sie einen benutzerdefinierten Modell Binder einbinden, der etwas anderes Unternehmen kann.

(In den meisten Fällen sind die beiden Formulare äquivalent, es sei denn, Sie haben in ihrer Pipeline benutzerdefinierte Modell Binder.)

<a id="route-names"></a>
## <a name="route-names"></a>Routennamen

In der Web-API weist jede Route einen Namen auf. Routennamen sind nützlich, um Links zu erstellen, sodass Sie einen Link in eine HTTP-Antwort einfügen können.

Um den Routennamen anzugeben, legen Sie die **Name** -Eigenschaft für das-Attribut fest. Im folgenden Beispiel wird gezeigt, wie der Routen Name festgelegt wird und wie der Routen Name beim Erstellen eines Links verwendet wird.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Routen Reihenfolge

Wenn das Framework versucht, einen URI mit einer Route abzugleichen, wertet es die Routen in einer bestimmten Reihenfolge aus. Um die Reihenfolge anzugeben, legen Sie die **Order** -Eigenschaft für das Routen Attribut fest. Niedrigere Werte werden zuerst ausgewertet. Der Standard Reihen folgen Wert ist 0 (null).

So wird die Gesamt Reihenfolge bestimmt:

1. Vergleichen Sie die **Order** -Eigenschaft des Route-Attributs.
2. Sehen Sie sich die einzelnen URI-Segmente in der Routen Vorlage an. Sortieren Sie für jedes Segment folgende Reihenfolge:

    1. Literale Segmente.
    2. Routen Parameter mit Einschränkungen.
    3. Routen Parameter ohne Einschränkungen.
    4. Platzhalter Parameter Segmente mit Einschränkungen.
    5. Platzhalter Parameter Segmente ohne Einschränkungen.
3. Im Fall eines gleich Zeichens werden Routen durch einen Ordinalzeichenfolgenvergleich ohne Berücksichtigung der Groß-/Kleinschreibung ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) der Routen Vorlage geordnet.

Im Folgenden ein Beispiel. Angenommen, Sie definieren den folgenden Controller:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Diese Routen werden wie folgt angeordnet.

1. Bestellungen/Details
2. Orders/{ID}
3. Orders/{CustomerName}
4. Orders/{\*Datum}
5. Aufträge/ausstehend

Beachten Sie, dass "Details" ein literales Segment ist und vor "{ID}" angezeigt wird, aber "Pending" (ausstehend) angezeigt wird, da die **Order** -Eigenschaft 1 ist. (In diesem Beispiel wird davon ausgegangen, dass keine Kunden mit dem Namen "Details" oder "Pending" vorhanden sind. Vermeiden Sie im Allgemeinen, mehrdeutige Routen zu vermeiden. In diesem Beispiel ist eine bessere Routen Vorlage für `GetByCustomer` "Customers/{CustomerName}").
