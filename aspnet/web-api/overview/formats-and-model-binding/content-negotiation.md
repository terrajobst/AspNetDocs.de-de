---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Inhaltsaushandlung in ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: Beschreibt, wie ASP.net-Web-API die http-Inhaltsaushandlung für ASP.NET 4. x implementiert.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504657"
---
# <a name="content-negotiation-in-aspnet-web-api"></a>Inhalts Aushandlung in ASP.net-Web-API

von [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.net-Web-API die Inhaltsaushandlung für ASP.NET 4. x implementiert.

Die HTTP-Spezifikation (RFC 2616) definiert die Inhaltsaushandlung als "der Prozess der Auswahl der optimalen Darstellung für eine bestimmte Antwort, wenn mehrere Darstellungen verfügbar sind". Der primäre Mechanismus für die Inhaltsaushandlung in http sind folgende Anforderungs Header:

- **Akzeptieren:** Welche Medientypen für die Antwort zulässig sind, z. b. "application/json", "Application/XML" oder ein benutzerdefinierter Medientyp, z. b. &quot;application/vnd. example + XML&quot;
- **Accept-Charset:** Die zulässigen Zeichensätze, wie z. b. UTF-8 oder ISO 8859-1.
- **Accept-Encoding:** Welche Inhalts Codierungen zulässig sind, z. b. gzip.
- **Accept-Language:** Die bevorzugte natürliche Sprache, wie z. b. "en-US".

Der Server kann auch andere Teile der HTTP-Anforderung überprüfen. Wenn die Anforderung z. b. einen X-angeforderten-with-Header enthält, der eine AJAX-Anforderung angibt, wird der Server möglicherweise standardmäßig JSON verwendet, wenn kein Accept-Header vorhanden ist.

In diesem Artikel wird erläutert, wie die Web-API die Accept-und Accept-Charset-Header verwendet. (Zu diesem Zeitpunkt gibt es keine integrierte Unterstützung für Accept-Encoding oder Accept-Language.)

## <a name="serialization"></a>Serialisierung

Wenn ein Web-API-Controller eine Ressource als CLR-Typ zurückgibt, serialisiert die Pipeline den Rückgabewert und schreibt ihn in den HTTP-Antworttext.

Sehen Sie sich beispielsweise die folgende Controller Aktion an:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Ein Client kann diese HTTP-Anforderung senden:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Als Antwort sendet der Server möglicherweise Folgendes:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

In diesem Beispiel hat der Client entweder JSON, JavaScript oder "Anything" (\*/\*) angefordert. Der Server hat mit einer JSON-Darstellung des `Product` Objekts geantwortet. Beachten Sie, dass der Content-Type-Header in der Antwort auf &quot;Application/JSON-&quot;festgelegt ist.

Ein Controller kann auch ein **HttpResponseMessage** -Objekt zurückgeben. Um ein CLR-Objekt für den Antworttext anzugeben, müssen Sie die Erweiterungsmethode " **samateresponse** " aufrufen:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Diese Option ermöglicht Ihnen mehr Kontrolle über die Details der Antwort. Sie können den Statuscode festlegen, HTTP-Header hinzufügen usw.

Das Objekt, das die Ressource serialisiert, wird als *medienformatierer*bezeichnet. Medien Formatierer werden von der **mediatypeer Formatter** -Klasse abgeleitet. Die Web-API stellt medienformatierer für XML und JSON bereit, und Sie können benutzerdefinierte Formatierer erstellen, um andere Medientypen zu unterstützen. Weitere Informationen zum Schreiben eines benutzerdefinierten Formatierers finden Sie unter [medienformatierer](media-formatters.md).

## <a name="how-content-negotiation-works"></a>So funktioniert die Inhalts Aushandlung

Zuerst Ruft die Pipeline den **icontentvermittler** -Dienst aus dem **httpconfiguration** -Objekt ab. Außerdem wird die Liste der Medien Formatierer aus der **httpconfiguration. Formatierungs Programme** -Auflistung abgerufen.

Als nächstes Ruft die Pipeline **icontentverhandlers. aushandeln**auf und übergibt Folgendes:

- Der Typ des zu serialisierenden Objekts.
- Die Auflistung der Medien Formatierer.
- Die HTTP-Anforderung

Von der **Aushandlungs** Methode werden zwei Informationen zurückgegeben:

- Zu verwendende Formatierer
- Der Medientyp für die Antwort.

Wenn kein Formatierer gefunden wird, gibt die **Aushandlungs** Methode **null**zurück, und der Client empfängt den HTTP-Fehler 406 (nicht akzeptabel).

Der folgende Code zeigt, wie ein Controller die Inhaltsaushandlung direkt aufrufen kann:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Dieser Code entspricht dem automatischen Funktionsumfang der Pipeline.

## <a name="default-content-negotiator"></a>Standard inhaltsverhandler

Die **defaultcontentverhandlerklasse** stellt die Standard Implementierung von **icontentvermittler**bereit. Es werden verschiedene Kriterien verwendet, um einen Formatierer auszuwählen.

Zuerst muss der Formatierer den Typ serialisieren können. Dies wird durch den Aufruf von **mediatypformatter. CanWrite tetype**überprüft.

Als Nächstes untersucht der inhaltsverhandler jeden Formatierer und wertet aus, wie gut er mit der HTTP-Anforderung übereinstimmt. Zum Auswerten der Übereinstimmung prüft der inhaltsverhandler zwei Dinge im Formatierer:

- Die **supportedmediatypes** -Auflistung, die eine Liste der unterstützten Medientypen enthält. Der inhaltsverhander versucht, diese Liste mit dem Accept-Header der Anforderung abzugleichen. Beachten Sie, dass der Accept-Header Bereiche enthalten kann. "Text/Plain" ist z. b. eine Entsprechung für Text/\* oder \*/\*.
- Die **mediatypeer Mappings** -Auflistung, die eine Liste von **mediatypeer Mapping** -Objekten enthält. Die **mediatypeer Mapping** -Klasse stellt eine generische Methode zum Zuordnen von HTTP-Anforderungen mit Medientypen bereit. Beispielsweise kann ein benutzerdefinierter HTTP-Header einem bestimmten Medientyp zugeordnet werden.

Wenn mehrere Übereinstimmungen vorhanden sind, gewinnt die Übereinstimmung mit dem höchsten Qualitätsfaktor. Beispiel:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

In diesem Beispiel hat Application/JSON einen impliziten Qualitätsfaktor von 1,0, daher wird er gegenüber Application/XML bevorzugt.

Wenn keine Übereinstimmungen gefunden werden, versucht der inhaltsverhandler, ggf. mit dem Medientyp des Anforderungs Texts abzugleichen. Wenn die Anforderung beispielsweise JSON-Daten enthält, sucht der inhaltsverhandler nach einem JSON-Formatierer.

Wenn noch keine Übereinstimmungen vorhanden sind, wählt der Inhalts Vermittler einfach das erste formatierungprogramm aus, das den Typ serialisieren kann.

## <a name="selecting-a-character-encoding"></a>Auswählen einer Zeichencodierung

Nachdem ein Formatierungs Programm ausgewählt wurde, wählt der inhaltsverhandler die beste Zeichencodierung aus, indem er die **supportedencodings** -Eigenschaft des Formatierungs Programms prüft und ihn mit dem Accept-Charset-Header in der Anforderung abruft (sofern vorhanden).
