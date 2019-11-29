---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Einführung in signalr | Microsoft-Dokumentation
author: bradygaster
description: In diesem Artikel wird beschrieben, was signalr ist und einige der Lösungen, die für die Erstellung entwickelt wurden.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 11b494b4839c646b018098c76a8a9ae0a2169757
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600488"
---
# <a name="introduction-to-signalr"></a>Einführung in SignalR

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Artikel wird beschrieben, was signalr ist und einige der Lösungen, die für die Erstellung entwickelt wurden. 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)veröffentlichen.

## <a name="what-is-signalr"></a>Was ist signalr?

ASP.net signalr ist eine Bibliothek für ASP.NET-Entwickler, die das Hinzufügen von Echt Zeit Webfunktionen zu Anwendungen vereinfacht. Die Echt Zeit webolle ist die Möglichkeit, dass Servercode Inhalte direkt an verbundene Clients Übertragung, sobald Sie verfügbar werden, anstatt dass der Server darauf wartet, dass ein Client neue Daten anfordert.

Signalr kann verwendet werden, um Ihrer ASP.NET-Anwendung jede Art von "Echt Zeit Web-Funktionalität" hinzuzufügen. Obwohl Chat häufig als Beispiel verwendet wird, können Sie viel mehr tun. Jedes Mal, wenn ein Benutzer eine Webseite aktualisiert, um neue Daten anzuzeigen, oder wenn die Seite eine [lange](http://en.wikipedia.org/wiki/Push_technology#Long_polling) Abfrage zum Abrufen neuer Daten implementiert, ist dies ein Kandidat für die Verwendung von signalr. Beispiele hierfür sind Dashboards und Überwachungsanwendungen, kollaborative Anwendungen (z. b. die gleichzeitige Bearbeitung von Dokumenten), Statusaktualisierungen für Aufträge und Echt Zeit Formulare.

Signalr bietet auch vollständig neue Arten von Webanwendungen, die hoch Häufigkeits Updates vom Server erfordern, z. b. Echt Zeit Spiele.

Signalr bietet eine einfache API zum Erstellen von Remote Prozedur aufrufen (RPC) für Server-zu-Client-Aufrufe, die JavaScript-Funktionen in Client Browsern (und anderen Client Plattformen) aus Server seitigem .NET-Code aufrufen. Signalr umfasst auch die API für die Verbindungs Verwaltung (z.b. Verbindungs-und Trennungs Ereignisse) und das Gruppieren von Verbindungen.

![Aufrufen von Methoden mit signalr](introduction-to-signalr/_static/image1.png)

Signalr übernimmt automatisch die Verbindungs Verwaltung und ermöglicht das Übertragen von Nachrichten an alle verbundenen Clients gleichzeitig, wie z. b. einen Chatraum. Sie können auch Nachrichten an bestimmte Clients senden. Die Verbindung zwischen dem Client und dem Server ist im Gegensatz zu einer klassischen HTTP-Verbindung persistent, die für jede Kommunikation wieder hergestellt wird.

Signalr unterstützt die Funktion "Serverpush", in der Servercode Client Code im Browser mithilfe von Remote Prozedur aufrufen (Remote Procedure Calls, RPC) aufrufen kann, anstelle des im Web gängigen Anforderungs Antwort Modells.

Signalr-Anwendungen können mit Service Bus, SQL Server oder [redis](http://redis.io)auf Tausende von Clients horizontal hochskaliert werden.

Signalr ist Open Source und kann über [GitHub](https://github.com/signalr)aufgerufen werden.

## <a name="signalr-and-websocket"></a>Signalr und WebSocket

Signalr verwendet den neuen WebSocket-Transport, sofern verfügbar, und greift bei Bedarf auf ältere Transporte zurück. Obwohl Sie Ihre APP sicherlich direkt mithilfe von WebSocket schreiben können, bedeutet die Verwendung von signalr, dass viele zusätzliche Funktionen, die Sie implementieren müssen, bereits für Sie erledigt sind. Vor allem bedeutet dies, dass Sie Ihre APP so programmieren können, dass WebSocket genutzt wird, ohne sich Gedanken über das Erstellen eines separaten Codepfads für ältere Clients machen zu müssen. Signalr schützt Sie auch daran, sich über Aktualisierungen von WebSocket Gedanken zu machen, da signalr aktualisiert wurde, um Änderungen am zugrunde liegenden Transport zu unterstützen, sodass Ihre Anwendung eine konsistente Schnittstelle über Versionen von WebSocket hinweg bietet.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transporte und Fallbacks

Signalr ist eine Abstraktion über einige der Transporte, die für die Echt Zeit Arbeit zwischen Client und Server erforderlich sind. Eine signalr-Verbindung wird als http gestartet und anschließend zu einer WebSocket-Verbindung herauf gestuft, wenn Sie verfügbar ist. WebSocket ist der ideale Transport für signalr, da dadurch der Server Arbeitsspeicher am effizientesten genutzt wird, die geringste Latenz und die meisten zugrunde liegenden Features (z. b. die vollständige Duplex Kommunikation zwischen Client und Server) vorhanden sind, aber auch die strengsten Anforderungen: WebSocket erfordert, dass der Server Windows Server 2012 oder Windows 8 und .NET Framework 4,5 verwendet. Wenn diese Anforderungen nicht erfüllt werden, versucht signalr, andere Transporte zu verwenden, um die Verbindungen herzustellen.

### <a name="html-5-transports"></a>HTML 5-Transporte

Diese Transporte sind von der Unterstützung für [HTML 5](http://en.wikipedia.org/wiki/HTML5)abhängig. Wenn der Client Browser den HTML 5-Standard nicht unterstützt, werden ältere Transporte verwendet.

- **WebSocket** (wenn sowohl der Server als auch der Browser angeben, dass WebSocket unterstützt werden kann). WebSocket ist der einzige Transport, der eine echte persistente bidirektionale Verbindung zwischen Client und Server herstellt. WebSocket hat jedoch auch die strengsten Anforderungen. Sie wird nur in den neuesten Versionen von Microsoft Internet Explorer, Google Chrome und Mozilla Firefox vollständig unterstützt und verfügt nur über eine partielle Implementierung in anderen Browsern wie Opera und Safari.
- Vom **Server gesendete Ereignisse**, auch bekannt als eventSource (wenn der Browser vom Server gesendete Ereignisse unterstützt, was im Grunde alle Browser mit Ausnahme von Internet Explorer ist).

### <a name="comet-transports"></a>Comet-Transporte

Die folgenden Transporte basieren auf dem [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) -Webanwendungs Modell, in dem ein Browser oder ein anderer Client eine lange beibehaltene http-Anforderung verwaltet, die der Server verwenden kann, um Daten per Push an den Client zu übermitteln, ohne dass der Client diese explizit anfordert.

- **Forever-Frame** (nur für Internet Explorer). "Forever Frame" erstellt einen ausgeblendeten IFRAME, der eine Anforderung an einen Endpunkt auf dem Server sendet, der nicht beendet wird. Der Server sendet dann kontinuierlich Skripts an den Client, der sofort ausgeführt wird und eine unidirektionale Echtzeitverbindung zwischen Server und Client bereitstellt. Bei der Verbindung zwischen Client und Server wird eine separate Verbindung zwischen dem Server und der Client Verbindung verwendet, und wie bei einer HTTP-Standard Anforderung wird eine neue Verbindung für jedes Datenelement erstellt, das gesendet werden muss.
- **Langer AJAX**-Abruf. Bei einem langen Abruf wird keine permanente Verbindung erstellt, sondern der Server wird mit einer Anforderung abgefragt, die geöffnet bleibt, bis der Server antwortet. zu diesem Zeitpunkt wird die Verbindung geschlossen, und sofort wird eine neue Verbindung angefordert. Dies kann zu einer gewissen Latenz beim Zurücksetzen der Verbindung führen.

Weitere Informationen dazu, welche Transporte unter welchen Konfigurationen unterstützt werden, finden Sie [unter Unterstützte Plattformen](supported-platforms.md).

### <a name="transport-selection-process"></a>Transport Auswahlprozess

In der folgenden Liste sind die Schritte aufgeführt, die signalr verwendet, um zu entscheiden, welcher Transport verwendet werden soll.

1. Wenn es sich bei dem Browser um Internet Explorer 8 oder eine frühere Version handelt, wird ein langer Abruf verwendet.
2. Wenn JSONP konfiguriert ist (d. h., der `jsonp`-Parameter ist auf `true` festgelegt, wenn die Verbindung gestartet wird), wird ein langer Abruf Vorgang verwendet.
3. Wenn eine Domänen übergreifende Verbindung hergestellt wird (d. h., wenn sich der signalr-Endpunkt nicht in derselben Domäne wie die Hostingseite befindet), wird WebSocket verwendet, wenn die folgenden Kriterien erfüllt sind:

   - Der Client unterstützt cors (Cross-Origin Resource Sharing). Ausführliche Informationen zu den Clients, die cors unterstützen, finden Sie unter [cors unter caniuse.com](http://www.caniuse.com/CORS).
   - Der Client unterstützt WebSocket.
   - Der Server unterstützt WebSocket.

     Wenn keines dieser Kriterien erfüllt ist, wird ein langer Abruf Vorgang verwendet. Weitere Informationen zu Domänen übergreifenden Verbindungen finden Sie unter [Einrichten einer Domänen übergreifenden Verbindung](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Wenn JSONP nicht konfiguriert ist und die Verbindung nicht Domänen übergreifend ist, wird WebSocket verwendet, wenn der Client und der Server dies unterstützen.
5. Wenn der Client oder der Server WebSocket nicht unterstützt, werden Server gesendete Ereignisse verwendet, sofern diese verfügbar sind.
6. Wenn vom Server gesendete Ereignisse nicht verfügbar sind, wird der dauerhafte Frame versucht.
7. Bei einem fehlerhaften Frame wird ein langer Abruf Vorgang verwendet.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Überwachen von Transporten

Sie können bestimmen, welcher Transport von Ihrer Anwendung verwendet wird, indem Sie die Protokollierung für Ihren Hub aktivieren und das Konsolenfenster in Ihrem Browser öffnen.

Fügen Sie der Client Anwendung den folgenden Befehl hinzu, um die Protokollierung für die Ereignisse Ihres Hubs in einem Browser zu aktivieren:

`$.connection.hub.logging = true;`

- Öffnen Sie in Internet Explorer die Entwicklertools, indem Sie F12 drücken, und klicken Sie auf die Registerkarte Konsole.

    ![Konsole in Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- Öffnen Sie in Chrome die Konsole, indem Sie STRG + UMSCHALT + J drücken.

    ![Konsole in Google Chrome](introduction-to-signalr/_static/image3.png)

Wenn die Konsole geöffnet ist und die Protokollierung aktiviert ist, können Sie sehen, welcher Transport von signalr verwendet wird.

![Konsole in Internet Explorer, die den WebSocket-Transport anzeigt](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Angeben eines Transports

Das Aushandeln eines Transports dauert eine bestimmte Zeitspanne sowie Client-/Server-Ressourcen. Wenn die Client Funktionen bekannt sind, kann ein Transport angegeben werden, wenn die Client Verbindung gestartet wird. Der folgende Code Ausschnitt veranschaulicht das Starten einer Verbindung mithilfe des AJAX Long-Abruf Transports, wie Sie verwendet wird, wenn bekannt ist, dass der Client kein anderes Protokoll unterstützte:

`connection.start({ transport: 'longPolling' });`

Sie können eine Fall Back Reihenfolge angeben, wenn ein Client bestimmte Transporte in der richtigen Reihenfolge ausprobieren soll. Der folgende Code Ausschnitt veranschaulicht das Testen von WebSocket und das Fehlschlagen des websockets direkt zum langen Abruf Vorgang.

`connection.start({ transport: ['webSockets','longPolling'] });`

Die Zeichen folgen Konstanten zum Angeben von Transporten werden wie folgt definiert:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Verbindungen und Hubs

Die signalr-API enthält zwei Modelle für die Kommunikation zwischen Clients und Servern: persistente Verbindungen und Hubs.

Eine Verbindung stellt einen einfachen Endpunkt zum Senden von Nachrichten mit einem einzelnen Empfänger, gruppierten oder Broadcast Nachrichten dar. Die permanente Verbindungs-API (dargestellt in .NET-Code durch die persistentconnection-Klasse) ermöglicht dem Entwickler den direkten Zugriff auf das von signalr bereitgestellte Kommunikationsprotokoll auf niedriger Ebene. Die Verwendung des Verbindungs Kommunikationsmodells ist Entwicklern vertraut, die Verbindungs basierte APIs wie z. b. Windows Communication Foundation verwendet haben.

Ein Hub ist eine hochstufige Pipeline, die auf der Verbindungs-API basiert und es dem Client und dem Server ermöglicht, Methoden untereinander direkt aufzurufen. Signalr verarbeitet die Verteilung über die Computer Grenzen hinweg, als wäre es von Magic, sodass Clients Methoden auf dem Server so einfach wie lokale Methoden aufrufen können und umgekehrt. Die Verwendung des Hubs-Kommunikationsmodells ist Entwicklern vertraut, die Remote Aufruf-APIs wie z. b. .NET-Remoting verwendet haben. Mithilfe eines Hubs können Sie auch stark typisierte Parameter an Methoden übergeben und so die Modell Bindung aktivieren.

### <a name="architecture-diagram"></a>Architektur Diagramm

Das folgende Diagramm zeigt die Beziehung zwischen Hubs, permanenten Verbindungen und den zugrunde liegenden Technologien, die für Transporte verwendet werden.

![Signalr-Architektur Diagramm, das APIs, Transporte und Clients anzeigt](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Funktionsweise von Hubs

Wenn serverseitiger Code eine Methode auf dem Client aufruft, wird ein Paket über den aktiven Transport gesendet, der den Namen und die Parameter der aufzurufenden Methode enthält (wenn ein Objekt als Methoden Parameter gesendet wird, wird es mit JSON serialisiert). Der Client vergleicht dann den Methodennamen mit den Methoden, die im Client seitigen Code definiert werden. Wenn eine Entsprechung vorliegt, wird die Client Methode mithilfe der deserialisierten Parameterdaten ausgeführt.

Der Methodenaufrufe kann mithilfe von Tools wie " [fddler](http://fiddler2.com/) " überwacht werden. Die folgende Abbildung zeigt einen Methodenaufrufe, der von einem signalr-Server an einen Webbrowser Client im Protokollbereich von "fddler" gesendet wird. Der Methodenaufruf wird von einem Hub mit dem Namen `MoveShapeHub`gesendet, und die aufgerufene Methode wird `updateShape`aufgerufen.

![Ansicht des "f"-Protokolls mit signalr-Datenverkehr](introduction-to-signalr/_static/image6.png)

In diesem Beispiel wird der Name des Hubs mit dem `H`-Parameter identifiziert. der Methodenname wird mit dem `M`-Parameter identifiziert, und die an die-Methode gesendeten Daten werden mit dem `A`-Parameter identifiziert. Die Anwendung, die diese Meldung generiert hat, wird im [echt](tutorial-high-frequency-realtime-with-signalr.md) Zeit Lern-Tutorial erstellt.

### <a name="choosing-a-communication-model"></a>Auswählen eines Kommunikationsmodells

Die meisten Anwendungen sollten die Hubs-API verwenden. Die Verbindungs-API kann in den folgenden Situationen verwendet werden:

- Das Format der tatsächlich gesendeten Nachricht muss angegeben werden.
- Der Entwickler bevorzugt das Arbeiten mit einem Messaging-und dispatchmodell anstelle eines Remote Aufruf Modells.
- Eine vorhandene Anwendung, die ein Messaging Modell verwendet, wird für die Verwendung von signalr portiert.
