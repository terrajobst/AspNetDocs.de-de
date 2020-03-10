---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globale Fehlerbehandlung in ASP.net-Web-API 2-ASP.NET 4. x
author: davidmatson
description: Eine Übersicht über die globale Fehlerbehandlung in ASP.net-Web-API 2 für ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 94f2d6d31d0b37f9bb0077e6258c70a2dfb1918d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448965"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Globale Fehlerbehandlung in ASP.net-Web-API 2

von [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Thema bietet einen Überblick über die globale Fehlerbehandlung in ASP.net-Web-API 2 für ASP.NET 4. x. Heute gibt es in der Web-API keine einfache Möglichkeit, Fehler Global zu protokollieren oder zu behandeln. Einige nicht behandelte Ausnahmen können über [Ausnahme Filter](exception-handling.md)verarbeitet werden, aber es gibt eine Reihe von Fällen, die Ausnahme Filter nicht verarbeiten können. Beispiel:

1. Von Controllerkonstruktoren ausgelöste Ausnahmen.
2. Von Meldungshandlern ausgelöste Ausnahmen
3. Während des Routings ausgelöste Ausnahmen.
4. Während der Serialisierung von Antwort Inhalten ausgelöste Ausnahmen.

Wir möchten eine einfache, konsistente Möglichkeit bereitstellen, um diese Ausnahmen zu protokollieren und zu verarbeiten (sofern möglich). 

Es gibt zwei Hauptfälle für die Behandlung von Ausnahmen. Dies ist der Fall, in dem wir eine Fehler Antwort senden können, und der Fall, in dem nur die Ausnahme protokolliert werden kann. Ein Beispiel für den letzteren Fall ist der Zeitpunkt, zu dem eine Ausnahme in der Mitte des Streaming-Antwort Inhalts ausgelöst wird. in diesem Fall ist es zu spät, eine neue Antwortnachricht zu senden, da der Statuscode, die Header und der Teil Inhalt bereits über das Netzwerk übertragen wurden, sodass wir die Verbindung einfach abbrechen. Obwohl die Ausnahme nicht behandelt werden kann, um eine neue Antwortnachricht zu liefern, wird die Protokollierung der Ausnahme weiterhin unterstützt. In Fällen, in denen ein Fehler erkannt werden kann, können wir eine entsprechende Fehler Antwort zurückgeben, wie im folgenden dargestellt:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Vorhandene Optionen

Neben [Ausnahme Filtern](exception-handling.md)können auch heute [Nachrichten Handler](../advanced/http-message-handlers.md) verwendet werden, um alle Antworten auf 500-Ebene zu beobachten, aber das reagieren auf diese Antworten ist schwierig, da Ihnen der Kontext zum ursprünglichen Fehler fehlt. Meldungs Handler haben auch einige der gleichen Einschränkungen wie Ausnahme Filter bezüglich der Fälle, die Sie behandeln können. Die Web-API verfügt zwar über eine Ablauf Verfolgungs Infrastruktur, bei der Fehlerbedingungen aufgezeichnet werden, die Ablauf Verfolgungs Infrastruktur ist jedoch zu Diagnose Zwecken und nicht für die Ausführung in Produktionsumgebungen konzipiert oder geeignet. Die globale Ausnahmebehandlung und Protokollierung sollten Dienste sein, die während der Produktion ausgeführt werden und in vorhandene Überwachungslösungen eingebunden werden können (z. [b. ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Übersicht über die Lösungen

 Wir stellen zwei neue, vom Benutzer ersetzbare Dienste, [iexceptionlogger](../releases/whats-new-in-aspnet-web-api-21.md) und iexceptionhandler, bereit, um nicht behandelte Ausnahmen zu protokollieren und zu behandeln. Die Dienste sind sehr ähnlich, mit zwei Haupt unterschieden:

1. Wir unterstützen das Registrieren mehrerer Ausnahme Protokollierungen, aber nur eines einzelnen Ausnahme Handlers.
2. Ausnahme Protokollierungen werden immer aufgerufen, auch wenn die Verbindung abgebrochen werden soll. Ausnahmehandler werden nur aufgerufen, wenn wir immer noch auswählen können, welche Antwortnachricht gesendet werden soll.

Beide Dienste ermöglichen den Zugriff auf einen Ausnahme Kontext, der relevante Informationen ab dem Punkt enthält, an dem die Ausnahme erkannt wurde. Dies gilt insbesondere für [httprequestmessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [httprequestcontext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), die ausgelöste Ausnahme und die Ausnahme Quelle (Details weiter unten).

### <a name="design-principles"></a>Entwurfsprinzipien

1. **Keine wichtigen Änderungen** Da diese Funktion in einer neben Version hinzugefügt wird, besteht eine wichtige Einschränkung, die sich auf die Lösung auswirkt, darin, dass keine wichtigen Änderungen vorgenommen werden müssen, um Verträge oder Verhalten einzugeben. Diese Einschränkung hat eine Bereinigung ausgeschlossen, die wir im Hinblick auf vorhandene catch-Blöcke durchgeführt haben möchten, wodurch Ausnahmen in 500 Antworten umgewandelt werden. Diese zusätzliche Bereinigung ist etwas, das wir für eine nachfolgende Hauptversion in Erwägung haben. Wenn dies für Sie wichtig ist, stimmen Sie bitte auf [ASP.net-Web-API User Voice](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception)ab.
2. **Beibehalten der Konsistenz mit Web-API-Konstrukten** Die Filter Pipeline der Web-API ist eine hervorragend Möglichkeit, übergreifende Aspekte mit der Flexibilität der Anwendung der Logik auf einen Aktions spezifischen, Controller spezifischen oder globalen Bereich zu verarbeiten. Filter, einschließlich Ausnahme Filter, haben immer Aktions-und Controller Kontexte, auch wenn Sie im globalen Gültigkeitsbereich registriert sind. Dieser Vertrag ist für Filter sinnvoll, bedeutet aber, dass Ausnahme Filter, auch Global weit gültige, für einige Ausnahme Behandlungsfälle geeignet sind, wie z. b. Ausnahmen von Nachrichten Handlern, bei denen keine Aktion oder kein Controller Kontext vorhanden ist. Wenn wir die flexible Bereichs Definition verwenden möchten, die von Filtern für die Ausnahmebehandlung geboten wird, benötigen wir weiterhin Ausnahme Filter. Wenn jedoch eine Ausnahme außerhalb eines Controller Kontexts behandelt werden muss, benötigen wir auch ein separates Konstrukt für die vollständige globale Fehlerbehandlung (etwas ohne Controller Kontext und Aktions Kontext Einschränkungen).

### <a name="when-to-use"></a>Verwendungs Zeitpunkt

- Mit Ausnahme Protokollierungen können alle nicht behandelten Ausnahmen angezeigt werden, die von der Web-API abgefangen wurden.
- Ausnahmehandler sind die Lösung für die Anpassung aller möglichen Antworten auf nicht behandelte Ausnahmen, die von der Web-API abgefangen werden.
- Ausnahme Filter sind die einfachste Lösung für die Verarbeitung der Teilmenge nicht behandelter Ausnahmen, die sich auf eine bestimmte Aktion oder einen bestimmten Controller beziehen.

### <a name="service-details"></a>Dienst Details

 Die Schnittstellen für die Ausnahme Protokollierung und den handlerdienst sind einfache Async-Methoden, die die entsprechenden Kontexte 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Wir stellen auch Basisklassen für beide Schnittstellen bereit. Das Überschreiben der Kern Methoden (Sync oder Async) ist nur erforderlich, um zu den empfohlenen Zeiten zu protokollieren oder zu verarbeiten. Bei der Protokollierung stellt die `ExceptionLogger` Basisklasse sicher, dass die Kern Protokollierungs Methode nur einmal für jede Ausnahme aufgerufen wird (auch wenn Sie später weiter oben in der Aufruf Stapel weitergegeben wird und wieder abgefangen wird). Die `ExceptionHandler`-Basisklasse ruft die Kern Behandlungsmethode nur für Ausnahmen am Anfang der-aufrufsliste auf und ignoriert ältere, geschachtelte catch-Blöcke. (Vereinfachte Versionen dieser Basisklassen finden Sie im Anhang unten.) Sowohl `IExceptionLogger` als auch `IExceptionHandler` erhalten Informationen über die Ausnahme über eine `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Wenn das Framework eine Ausnahme Protokollierung oder einen Ausnahmehandler aufruft, stellt es immer eine `Exception` und eine `Request`bereit. Mit Ausnahme von Unittests wird auch immer eine `RequestContext`bereitgestellt. Sie stellt selten eine `ControllerContext` und `ActionContext` bereit (nur bei Aufrufen aus dem catch-Block für Ausnahme Filter). Sie stellt sehr selten eine `Response`bereit (nur in bestimmten IIS-Fällen, wenn versucht wird, die Antwort zu schreiben). Beachten Sie, dass es für einige dieser Eigenschaften möglicherweise `null` ist, dass der Consumer vor dem Zugriff auf die Member der Ausnahme Klasse auf `null` prüft.`CatchBlock` eine Zeichenfolge, die angibt, welcher catch-Block die Ausnahme gesehen hat. Die Catch-Block Zeichenfolgen lauten wie folgt:

- HTTPServer (SendAsync-Methode)
- Httpcontrollerdispatcher (SendAsync-Methode)
- Httpbatchhandler (SendAsync-Methode)
- Iexceptionfilter (Verarbeitung der Ausnahme Filter Pipeline in "ExecuteAsync" durch apicontroller)
- Owin-Host:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (for buffering output)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (for streaming output)
- Webhost:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (for buffering output)
    - Httpcontrollerhandler. Beschreib testreamedresponmencontentasync (für Streamingausgabe)
    - Httpcontrollerhandler. Beschreib teerrorresponsecontentasync (für Fehler bei der Fehlerwiederherstellung im gepufferten Ausgabemodus)

Die Liste der catch-Block Zeichenfolgen ist auch über statische schreibgeschützte Eigenschaften verfügbar. (Die Kernblock-catch-Block Zeichenfolge befindet sich in den statischen exceptioncatchblocks. der Rest wird in einer statischen Klasse jeweils für owin und Webhost angezeigt.)`IsTopLevelCatchBlock` ist hilfreich, wenn das empfohlene Muster zur Behandlung von Ausnahmen nur am Anfang der-aufrufsstapel folgt. Anstatt Ausnahmen in 500-Antworten zu übernehmen, wenn ein geclusterte catch-Block auftritt, kann ein Ausnahmehandler Ausnahmen weitergeben, bis Sie vom Host angezeigt werden.

Zusätzlich zum `ExceptionContext`erhält eine Protokollierung eine weitere Information über den vollständigen `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Die zweite Eigenschaft (`CanBeHandled`) ermöglicht einem Logger, eine Ausnahme zu identifizieren, die nicht behandelt werden kann. Wenn die Verbindung abgebrochen und keine neue Antwortnachricht gesendet werden kann, werden die Protokollierungen aufgerufen, aber der Handler wird ***nicht*** aufgerufen, und die Protokollierungen können dieses Szenario aus dieser Eigenschaft identifizieren.

Zusätzlich zum `ExceptionContext`erhält ein Handler eine weitere Eigenschaft, die er für die vollständige `ExceptionHandlerContext` festlegen kann, um die Ausnahme zu behandeln:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Ein Ausnahmehandler gibt an, dass er eine Ausnahme behandelt hat, indem er die `Result`-Eigenschaft auf ein Aktions Ergebnis festgelegt hat (z. b. " [exceptionresult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx)", " [internalservererrorresult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx)", " [Statuscode](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)Type" oder ein benutzerdefiniertes Ergebnis). Wenn die `Result`-Eigenschaft NULL ist, wird die Ausnahme nicht behandelt, und die ursprüngliche Ausnahme wird erneut ausgelöst.

Für Ausnahmen am Anfang der Aufruf Stapel haben wir einen zusätzlichen Schritt unternommen, um sicherzustellen, dass die Antwort für API-Aufrufer geeignet ist. Wenn die Ausnahme an den Host weitergegeben wird, wird dem Aufrufer der gelbe Bildschirm angezeigt, oder eine andere vom Host bereitgestellte Antwort, die in der Regel HTML und in der Regel keine geeignete API-Fehler Antwort ist. In diesen Fällen beginnt das Ergebnis nicht-NULL, und nur wenn ein benutzerdefinierter Ausnahmehandler ihn explizit auf `null` (unbehandelte) zurücksetzt, wird die Ausnahme an den Host übermittelt. Das Festlegen von `Result` auf `null` in solchen Fällen kann für zwei Szenarien nützlich sein:

1. Owin-gehostete Web-API mit benutzerdefinierter Middleware für die Ausnahmebehandlung vor/außerhalb der Web-API.
2. Lokales Debuggen über einen Browser, bei dem der gelbe Bildschirm von Tod eine hilfreiche Antwort auf eine nicht behandelte Ausnahme ist.

Bei Ausnahme Protokollierungen und Ausnahme Handlern werden keine wiederherzustellenden Aktionen durchzuführen, wenn die Protokollierung oder der Handler selbst eine Ausnahme auslöst. (Außer wenn Sie die Ausnahme weitergeben lassen, lassen Sie das Feedback unten auf dieser Seite, wenn Sie einen besseren Ansatz haben.) Der Vertrag für die Ausnahme Protokollierung und Handler besteht darin, dass Sie keine Ausnahmen an ihre Aufrufer weitergeben dürfen. Andernfalls wird die Ausnahme einfach weitergegeben. Dies führt häufig zu einem HTML-Fehler (wie z. b. ASP). Der gelbe Bildschirm "NET" wird zurück an den Client gesendet (was in der Regel nicht die bevorzugte Option für API-Aufrufer ist, die JSON oder XML erwarten).

## <a name="examples"></a>Beispiele

### <a name="tracing-exception-logger"></a>Ablauf Verfolgungs Ausnahme Protokollierung

Die nachfolgende Ausnahme Protokollierung sendet Ausnahme Daten an konfigurierte Ablauf Verfolgungs Quellen (einschließlich des debugausgabefensters in Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Ausnahme Handler für benutzerdefinierte Fehlermeldungen

Im folgenden finden Sie eine benutzerdefinierte Fehler Antwort an Clients, einschließlich einer e-Mail-Adresse zum Kontaktieren des Supports.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrieren von Ausnahme Filtern

Wenn Sie das Projekt mit der Projektvorlage "ASP.NET MVC 4-Webanwendung" erstellen, fügen Sie Ihren Web-API-Konfigurations Code in die `WebApiConfig` Klasse im Ordner " *App/_start* " ein:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Anhang: Grundklassen Details

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
