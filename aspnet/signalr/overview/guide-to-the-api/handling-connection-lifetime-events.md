---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr | Microsoft-Dokumentation
author: bradygaster
description: In diesem Artikel wird beschrieben, wie die von der Hubs-API verfügbar gemachten Ereignisse verwendet werden.
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 5bdf20549fccab5d644e35fdf4ce351540c8620d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467421"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Überblick und Behandeln von Ereignissen im Zusammenhang mit der Verbindungslebensdauer in SignalR

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Dieser Artikel bietet eine Übersicht über die signalr-Verbindung, die Verbindung wiederherstellen und Verbindungs Ereignisse, die Sie behandeln können, sowie über Timeout-und KeepAlive-Einstellungen, die Sie konfigurieren können.
>
> In diesem Artikel wird davon ausgegangen, dass Sie bereits über Kenntnisse zu signalr und Verbindungs Lebensdauer-Ereignissen verfügen. Eine Einführung in signalr finden Sie unter [Introduction to signalr (Einführung in signalr](../getting-started/introduction-to-signalr.md)). Listen der Verbindungs Lebensdauer-Ereignisse finden Sie in den folgenden Ressourcen:
>
> - [Behandeln von Verbindungs Lebensdauer-Ereignissen in der Hub-Klasse](hubs-api-guide-server.md#connectionlifetime)
> - [Behandeln von Verbindungs Lebensdauer-Ereignissen in JavaScript-Clients](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Behandeln von Verbindungs Lebensdauer-Ereignissen in .NET-Clients](hubs-api-guide-net-client.md#connectionlifetime)
>
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendete Software Versionen
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> - .NET 4.5
> - Signalr Version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Vorherige Versionen dieses Themas
>
> Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

## <a name="overview"></a>Übersicht

Dieser Artikel enthält folgende Abschnitte:

- [Terminologie und Szenarien für die Verbindungs Lebensdauer](#terminology)

    - [Signalr-Verbindungen, Transportverbindungen und physische Verbindungen](#signalrvstransport)
    - [Übertragungs Szenarien für Transport Unterbrechung](#transportdisconnect)
    - [Szenarios zur Trennung von Clients](#clientdisconnect)
    - [Szenarios zur Server Trennung](#serverdisconnect)
- [Timeout-und KeepAlive-Einstellungen](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - ["Disconnecttimeout"](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Ändern der Timeout-und KeepAlive-Einstellungen](#changetimeout)
- [Benachrichtigen des Benutzers über Trennungen](#notifydisconnect)
- [Fortlaufende Wiederherstellung der Verbindung](#continuousreconnect)
- [Trennen einer Verbindung zwischen einem Client und Servercode](#disconnectclientfromserver)
- [Erkennen der Ursache für eine Verbindungs Trennung](#detectingreasonfordisconnection)

Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API. Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologie und Szenarien für die Verbindungs Lebensdauer

Der `OnReconnected`-Ereignishandler in einem signalr-Hub kann direkt nach `OnConnected` ausgeführt werden, aber nicht nach `OnDisconnected` für einen bestimmten Client. Der Grund, warum Sie eine erneute Verbindung ohne eine Verbindung herstellen können, besteht darin, dass es verschiedene Möglichkeiten gibt, wie das Wort "Connection" in signalr verwendet wird.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Signalr-Verbindungen, Transportverbindungen und physische Verbindungen

In diesem Artikel wird zwischen *signalr-Verbindungen*, *Transportverbindungen*und *physischen Verbindungen*unterschieden:

- Die **signalr-Verbindung** bezieht sich auf eine logische Beziehung zwischen einem Client und einer Server-URL, die von der signalr-API verwaltet und durch eine Verbindungs-ID eindeutig identifiziert wird. Die Daten über diese Beziehung werden von signalr verwaltet und zum Herstellen einer Transport Verbindung verwendet. Die Beziehung wird beendet, und signalr gibt die Daten frei, wenn der Client die `Stop` Methode aufruft, oder ein Timeout Limit erreicht wird, während signalr versucht, eine verlorene Transport Verbindung wiederherzustellen.
- **Transport Verbindung** bezieht sich auf eine logische Beziehung zwischen einem Client und einem Server, der von einer der vier Transport-APIs verwaltet wird: websockets, vom Server gesendete Ereignisse, der dauerhafte Frame oder das lange abrufen. Signalr verwendet die Transport-API, um eine Transport Verbindung herzustellen, und die Transport-API hängt davon ab, ob eine physische Netzwerkverbindung vorhanden ist, um die Transport Verbindung herzustellen. Die Transport Verbindung wird beendet, wenn signalr Sie beendet oder wenn die Transport-API erkennt, dass die physische Verbindung getrennt ist.
- Die **physische Verbindung** bezieht sich auf die physischen Netzwerkverbindungen--Drähte, drahtlos Signale, Router usw., die die Kommunikation zwischen einem Client Computer und einem Server Computer vereinfachen. Die physische Verbindung muss vorhanden sein, um eine Transport Verbindung herzustellen, und es muss eine Transport Verbindung hergestellt werden, um eine signalr-Verbindung herzustellen. Das Abbrechen der physischen Verbindung beendet jedoch nicht immer sofort die Transport Verbindung oder die signalr-Verbindung, wie weiter unten in diesem Thema erläutert wird.

Im folgenden Diagramm wird die signalr-Verbindung durch die Hubs-API und die persistentconnection-API signalr-Schicht dargestellt. die Transport Verbindung wird von der Transportschicht dargestellt, und die physische Verbindung wird durch die Linien zwischen dem Server dargestellt. und die Clients.

![Signalr-Architektur Diagramm](handling-connection-lifetime-events/_static/image1.png)

Wenn Sie die `Start`-Methode in einem signalr-Client aufzurufen, stellen Sie dem signalr-Client Code alle Informationen bereit, die er benötigt, um eine physische Verbindung mit einem Server herzustellen. Der signalr-Client Code verwendet diese Informationen, um eine HTTP-Anforderung zu erstellen und eine physische Verbindung herzustellen, die eine der vier Transportmethoden verwendet. Wenn die Transport Verbindung fehlschlägt oder der Server ausfällt, wird die signalr-Verbindung nicht sofort entfernt, da der Client weiterhin über die Informationen verfügt, die er benötigt, um automatisch eine neue Transport Verbindung mit derselben signalr-URL wiederherzustellen. In diesem Szenario ist kein Eingreifen der Benutzeranwendung beteiligt, und wenn der signalr-Client Code eine neue Transport Verbindung herstellt, wird keine neue signalr-Verbindung gestartet. Die Kontinuität der signalr-Verbindung wird in der Tatsache widergespiegelt, dass sich die Verbindungs-ID, die beim aufruft der `Start`-Methode erstellt wird, nicht ändert.

Der `OnReconnected`-Ereignishandler auf dem Hub wird ausgeführt, wenn eine Transport Verbindung nach dem Verlust der Verbindung automatisch wieder hergestellt wird. Der `OnDisconnected`-Ereignishandler wird am Ende einer signalr-Verbindung ausgeführt. Eine signalr-Verbindung kann auf eine der folgenden Arten enden:

- Wenn der Client die `Stop`-Methode aufruft, wird eine Nachricht zum Beenden an den Server gesendet, und sowohl der Client als auch der Server beenden die signalr-Verbindung sofort.
- Nachdem die Konnektivität zwischen Client und Server verloren gegangen ist, versucht der Client, die Verbindung wiederherzustellen, und der Server wartet darauf, dass der Client erneut eine Verbindung herstellt. Wenn die Versuche, die Verbindung wiederherzustellen, nicht erfolgreich sind und das Timeout für die Trennung endet, beenden sowohl der Client als auch der Server die signalr-Verbindung. Der Client versucht nicht mehr, die Verbindung wiederherzustellen, und der Server verwirft seine Darstellung der signalr-Verbindung.
- Wenn der Client nicht mehr ausgeführt werden kann, ohne dass die Möglichkeit besteht, die `Stop`-Methode aufzurufen, wartet der Server, bis der Client erneut eine Verbindung herstellt, und beendet dann die signalr-Verbindung nach dem Timeout Zeitraum für die Verbindung.
- Wenn der Server nicht mehr ausgeführt wird, versucht der Client, die Verbindung wiederherzustellen (die Transport Verbindung neu zu erstellen), und beendet dann die signalr-Verbindung nach dem Timeout Zeitraum für die Verbindung.

Wenn keine Verbindungsprobleme vorliegen und die Benutzeranwendung die signalr-Verbindung beendet, indem Sie die `Stop`-Methode aufrufen, wird die signalr-Verbindung und die Transport Verbindung zum gleichen Zeitpunkt gestartet und beendet. In den folgenden Abschnitten werden die anderen Szenarien ausführlicher beschrieben.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Übertragungs Szenarien für Transport Unterbrechung

Physische Verbindungen können langsam sein, oder es können Unterbrechungen bei der Konnektivität auftreten. Abhängig von Faktoren wie z. b. der Länge der Unterbrechung, wird die Transport Verbindung möglicherweise verworfen. Signalr versucht dann, die Transport Verbindung wiederherzustellen. Manchmal erkennt die Transport Verbindungs-API die Unterbrechung und löscht die Transport Verbindung. signalr findet sofort heraus, dass die Verbindung verloren geht. In anderen Szenarien wird weder die Transport Verbindungs-API noch signalr sofort darauf hingewiesen, dass die Konnektivität verloren gegangen ist. Für alle Transporte außer Long-Abruf verwendet der signalr-Client eine Funktion mit dem Namen *KeepAlive* , um den Verlust der Konnektivität zu überprüfen, die von der Transport-API nicht erkannt werden kann. Weitere Informationen zu langen Abruf Verbindungen finden Sie unter [Timeout-und KeepAlive-Einstellungen](#timeoutkeepalive) weiter unten in diesem Thema.

Wenn eine Verbindung inaktiv ist, sendet der Server in regelmäßigen Abständen ein KeepAlive-Paket an den Client. Ab dem Zeitpunkt, an dem dieser Artikel geschrieben wurde, beträgt die Standard Häufigkeit alle 10 Sekunden. Wenn Sie diese Pakete überwachen, können Clients erkennen, ob ein Verbindungsproblem vorliegt. Wenn ein KeepAlive-Paket nicht erwartungsgemäß empfangen wird, geht der Client nach kurzer Zeit davon aus, dass Verbindungsprobleme vorliegen, z. b. verlangsamtheit oder Unterbrechungen. Wenn KeepAlive nach längerer Zeit immer noch nicht empfangen wird, nimmt der Client an, dass die Verbindung getrennt wurde, und versucht, erneut eine Verbindung herzustellen.

Das folgende Diagramm veranschaulicht die Client-und Server Ereignisse, die in einem typischen Szenario ausgelöst werden, wenn Probleme mit der physischen Verbindung auftreten, die von der Transport-API nicht sofort erkannt werden. Das Diagramm gilt für die folgenden Umstände:

- Der Transport ist ein websockets, ein unbenes Frame oder Ereignisse, die vom Server gesendet werden.
- Es gibt unterschiedliche Zeiträume der Unterbrechung der physischen Netzwerkverbindung.
- Die Transport-API erkennt die Unterbrechungen nicht, sodass signalr die KeepAlive-Funktionalität verwendet, um Sie zu erkennen.

![Transport Trennungen](handling-connection-lifetime-events/_static/image2.png)

Wenn der Client in den Modus zum erneuten Herstellen der Verbindung wechselt, aber keine Transport Verbindung innerhalb des Limits für den Trennungs Timeout herstellen kann, beendet der Server die signalr-Verbindung. Wenn dies der Fall ist, führt der Server die `OnDisconnected` Methode des Hubs aus und fügt eine Disconnect-Nachricht in eine Warteschlange ein, die an den Client gesendet wird, wenn der Client eine spätere Verbindung herstellt. Wenn der Client dann erneut eine Verbindung herstellt, empfängt er den Disconnect-Befehl und ruft die `Stop`-Methode auf. In diesem Szenario wird `OnReconnected` nicht ausgeführt, wenn der Client erneut eine Verbindung herstellt, und `OnDisconnected` wird nicht ausgeführt, wenn der Client `Stop`aufruft. Dieses Szenario wird im folgenden Diagramm veranschaulicht.

![Transport Unterbrechungen-Server Timeout](handling-connection-lifetime-events/_static/image3.png)

Die signalr-Verbindungs Lebensdauer Ereignisse, die möglicherweise auf dem Client ausgelöst werden, lauten wie folgt:

- `ConnectionSlow` Client Ereignis.

    Wird ausgelöst, wenn ein vordefinierter Anteil des KeepAlive-Timeout Zeitraums seit dem Empfang der letzten Nachricht oder KeepAlive-Ping abgelaufen ist. Der standardmäßige KeepAlive-Timeout-Zeitraum ist 2/3 des KeepAlive-Timeouts. Der KeepAlive-Timeout Wert beträgt 20 Sekunden, sodass die Warnung bei ungefähr 13 Sekunden auftritt.

    Standardmäßig sendet der Server Keepalive-Pings alle 10 Sekunden, und der Client prüft ca. 2 Sekunden auf KeepAlive-Pings (ein Drittel der Differenz zwischen dem KeepAlive-Timeout Wert und dem KeepAlive-Timeout-Warn Wert).

    Wenn die Transport-API eine Trennung erkennt, wird signalr möglicherweise über die Trennung informiert, bevor der Zeitraum für das KeepAlive-Timeout überschritten wird. In diesem Fall würde das Ereignis `ConnectionSlow` nicht ausgelöst, und signalr würde direkt zum `Reconnecting` Ereignis gehen.
- `Reconnecting` Client Ereignis.

    Wird ausgelöst, wenn (a) die Transport-API erkennt, dass die Verbindung unterbrochen wurde, oder (b) der KeepAlive-Timeout Zeitraum seit dem Empfang der letzten Nachricht oder KeepAlive-Ping verstrichen ist. Der signalr-Client Code versucht, erneut eine Verbindung herzustellen. Sie können dieses Ereignis behandeln, wenn die Anwendung eine Aktion ausführen soll, wenn eine Transport Verbindung unterbrochen wird. Der standardmäßige KeepAlive-Timeout Zeitraum beträgt derzeit 20 Sekunden.

    Wenn der Client Code versucht, eine Hub-Methode aufzurufen, während signalr sich im Modus für die erneute Verbindungs Herstellung befindet, versucht signalr, den Befehl zu senden. In den meisten Fällen schlagen solche Versuche fehl, aber in einigen Fällen können Sie erfolgreich ausgeführt werden. Für die vom Server gesendeten Ereignisse, den dauerhaften Rahmen und lange Abruf Transporte verwendet signalr zwei Kommunikationskanäle: einen, den der Client zum Senden von Nachrichten verwendet, und einen, den der Client zum Empfangen von Nachrichten verwendet. Der für den Empfang verwendete Kanal ist das dauerhaft geöffnete, und das ist das, das geschlossen wird, wenn die physische Verbindung unterbrochen wird. Der zum Senden verwendete Kanal bleibt verfügbar. Wenn also die physische Konnektivität wieder hergestellt wird, kann ein Methodenaufrufe vom Client zum Server erfolgreich sein, bevor der Empfangskanal wieder hergestellt wird. Der Rückgabewert wird erst empfangen, wenn signalr den Kanal erneut öffnet, der für den Empfang verwendet wird.
- `Reconnected` Client Ereignis.

    Wird ausgelöst, wenn die Transport Verbindung wieder hergestellt wird. Der `OnReconnected`-Ereignishandler im Hub wird ausgeführt.
- `Closed` Client Ereignis (`disconnected`-Ereignis in JavaScript).

    Wird ausgelöst, wenn das Trennungs Timeout abgelaufen ist, während der signalr-Client Code versucht, die Verbindung nach dem Verlust der Transport Verbindung wiederherzustellen. Der Standardwert für das Trennen der Verbindung beträgt 30 Sekunden. (Dieses Ereignis wird auch ausgelöst, wenn die Verbindung beendet wird, da die `Stop`-Methode aufgerufen wird.)

Transport Verbindungsunterbrechungen, die nicht von der Transport-API erkannt werden und den Empfang von KeepAlive-Pings vom Server für einen längeren Zeitraum als das KeepAlive-Timeout nicht verzögern, bewirken möglicherweise nicht, dass Ereignisse der Verbindungs Lebensdauer ausgelöst werden.

In einigen Netzwerkumgebungen werden Verbindungen im Leerlauf absichtlich geschlossen, und eine weitere Funktion der KeepAlive-Pakete besteht darin, dies zu verhindern, da diese Netzwerke wissen, dass eine signalr-Verbindung verwendet wird. In Extremfällen ist die Standard Häufigkeit von KeepAlive-Pings möglicherweise nicht ausreichend, um geschlossene Verbindungen zu verhindern. In diesem Fall können Sie Keepalive-Pings so konfigurieren, dass Sie häufiger gesendet werden. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Timeout-und KeepAlive-Einstellungen](#timeoutkeepalive) .

> [!NOTE]
>
> **Wichtig**: die hier beschriebene Abfolge von Ereignissen ist nicht sichergestellt. Mit signalr wird jeder Versuch unternommen, Verbindungs Lebensdauer-Ereignisse gemäß diesem Schema auf vorhersagbare Weise zu lösen. es gibt jedoch viele Variationen von Netzwerk Ereignissen und viele Möglichkeiten, wie die zugrunde liegenden Kommunikations Frameworks wie Transport-APIs diese verarbeiten. Beispielsweise wird das `Reconnected` Ereignis möglicherweise nicht ausgelöst, wenn der Client erneut eine Verbindung herstellt, oder der `OnConnected` Handler auf dem Server wird möglicherweise ausgeführt, wenn der Versuch, eine Verbindung herzustellen, nicht erfolgreich ist. In diesem Thema werden nur die Auswirkungen beschrieben, die normalerweise in bestimmten typischen Fällen entstehen würden.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Szenarios zur Trennung von Clients

In einem Browser Client wird der signalr-Client Code, der eine signalr-Verbindung verwaltet, im JavaScript-Kontext einer Webseite ausgeführt. Aus diesem Grund muss die signalr-Verbindung beendet werden, wenn Sie von einer Seite zu einer anderen navigieren, und das ist der Grund dafür, dass Sie mehrere Verbindungen mit mehreren Verbindungs-IDs haben, wenn Sie eine Verbindung über mehrere Browserfenster oder-Registerkarten herstellen. Wenn der Benutzer ein Browserfenster oder eine Registerkarte schließt oder zu einer neuen Seite navigiert oder die Seite aktualisiert, wird die signalr-Verbindung sofort beendet, da der signalr-Client Code dieses Browser Ereignis für Sie behandelt und die `Stop` Methode aufruft. In diesen Szenarien oder auf einer beliebigen Client Plattform, wenn Ihre Anwendung die `Stop`-Methode aufruft, wird der `OnDisconnected`-Ereignishandler sofort auf dem Server ausgeführt, und der Client löst das `Closed`-Ereignis aus (das Ereignis wird in JavaScript als `disconnected` bezeichnet).

Wenn eine Client Anwendung oder der Computer, auf dem Sie ausgeführt wird, abstürzen oder in den Standbymodus wechselt (z. b. wenn der Benutzer den Laptop schließt), wird der Server nicht informiert, was passiert ist. Wenn der Server weiß, kann der Verlust des Clients auf eine Verbindungsunterbrechung zurückzuführen sein, und der Client versucht möglicherweise, die Verbindung wiederherzustellen. Daher wartet der Server in diesen Szenarien darauf, dem Client die Möglichkeit zu geben, erneut eine Verbindung herzustellen, und `OnDisconnected` wird erst ausgeführt, wenn das Timeout für die Trennung abläuft (standardmäßig ca. 30 Sekunden). Dieses Szenario wird im folgenden Diagramm veranschaulicht.

![Client Computerfehler](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Szenarios zur Server Trennung

Wenn ein Server offline geschaltet wird, wird er neu gestartet, schlägt fehl, die APP-Domäne wird wieder verwendet usw.--das Ergebnis könnte einer verlorenen Verbindung ähneln, oder die Transport-API und signalr wissen, dass der Server nicht mehr verfügbar ist, und signalr versucht möglicherweise, die Verbindung wiederherzustellen, ohne das `ConnectionSlow` Ereignis zu erhöhen. Wenn der Client in den Modus zum erneuten Verbinden wechselt und der Server wieder hergestellt oder neu gestartet wird oder ein neuer Server vor Ablauf des Trennungs Timeouts online geschaltet wird, stellt der Client erneut eine Verbindung mit dem wiederhergestellten oder neuen Server her. In diesem Fall wird die signalr-Verbindung auf dem Client fortgesetzt, und das `Reconnected`-Ereignis wird ausgelöst. Auf dem ersten Server wird `OnDisconnected` nie ausgeführt, und auf dem neuen Server wird `OnReconnected` ausgeführt, obwohl `OnConnected` zuvor nie für diesen Client auf diesem Server ausgeführt wurde. (Der Effekt ist derselbe, wenn der Client nach einem Neustart oder der Wiederverwendung der APP-Domäne erneut eine Verbindung mit dem gleichen Server herstellt, denn wenn der Server neu gestartet wird, hat er keinen Speicher für vorherige Verbindungs Aktivitäten.) Im folgenden Diagramm wird davon ausgegangen, dass die Transport-API die verlorene Verbindung sofort erkennt, sodass das `ConnectionSlow`-Ereignis nicht ausgelöst wird.

![Server Fehler und erneute Verbindung](handling-connection-lifetime-events/_static/image5.png)

Wenn ein Server innerhalb des Trennungs Timeouts nicht verfügbar wird, wird die signalr-Verbindung beendet. In diesem Szenario wird das `Closed` Ereignis (`disconnected` in JavaScript-Clients) auf dem Client ausgelöst, `OnDisconnected` jedoch nie auf dem Server aufgerufen wird. Im folgenden Diagramm wird davon ausgegangen, dass die Transport-API die verlorene Verbindung nicht erkennt, sodass Sie von der signalr KeepAlive-Funktionalität erkannt wird und das `ConnectionSlow`-Ereignis ausgelöst wird.

![Server Fehler und-Timeout](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Timeout-und KeepAlive-Einstellungen

Die Standardwerte für `ConnectionTimeout`, `DisconnectTimeout`und `KeepAlive` sind für die meisten Szenarien geeignet, können aber geändert werden, wenn Ihre Umgebung besondere Anforderungen hat. Wenn in Ihrer Netzwerkumgebung beispielsweise Verbindungen geschlossen werden, die 5 Sekunden lang im Leerlauf sind, müssen Sie den KeepAlive-Wert möglicherweise verringern.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Diese Einstellung gibt an, wie lange eine Transport Verbindung geöffnet bleiben und auf eine Antwort gewartet werden soll, bevor Sie geschlossen und eine neue Verbindung geöffnet wird. Der Standardwert ist 110 Sekunden.

Diese Einstellung gilt nur, wenn die KeepAlive-Funktionalität deaktiviert ist. Dies gilt normalerweise nur für den langen Abruf Transport. Das folgende Diagramm veranschaulicht die Auswirkung dieser Einstellung auf eine lange Abruf Transport Verbindung.

![Lange Abruf Transport Verbindung](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>"Disconnecttimeout"

Diese Einstellung gibt die Zeitspanne an, die gewartet wird, nachdem eine Transport Verbindung unterbrochen wurde, bevor das `Disconnected` Ereignis erhöht wird. Der Standardwert ist 30 Sekunden. Wenn Sie `DisconnectTimeout`festlegen, wird `KeepAlive` automatisch auf 1/3 des `DisconnectTimeout` Werts festgelegt.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Diese Einstellung gibt die Zeitspanne an, die gewartet werden soll, bevor ein KeepAlive-Paket über eine Verbindung im Leerlauf gesendet wird. Der Standardwert beträgt 10 Sekunden. Dieser Wert darf nicht größer als 1/3 des `DisconnectTimeout` Werts sein.

Wenn Sie sowohl `DisconnectTimeout` als auch `KeepAlive`festlegen möchten, legen Sie `KeepAlive` nach `DisconnectTimeout`fest. Andernfalls wird die `KeepAlive` Einstellung überschrieben, wenn `DisconnectTimeout` automatisch `KeepAlive` auf 1/3 des Timeout Werts festlegt.

Wenn Sie die KeepAlive-Funktion deaktivieren möchten, legen Sie `KeepAlive` auf NULL fest. Die KeepAlive-Funktion wird für den langen Abruf Transport automatisch deaktiviert.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Ändern der Timeout-und KeepAlive-Einstellungen

Um die Standardwerte für diese Einstellungen zu ändern, legen Sie diese in `Application_Start` in der Datei *Global. asax* fest, wie im folgenden Beispiel gezeigt. Die Werte, die im Beispielcode angezeigt werden, entsprechen den Standardwerten.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Benachrichtigen des Benutzers über Trennungen

In einigen Anwendungen können Sie dem Benutzer eine Meldung anzeigen, wenn Konnektivitätsprobleme vorliegen. Sie haben mehrere Möglichkeiten, wie und wann dies zu tun ist. Die folgenden Codebeispiele gelten für einen JavaScript-Client, der den generierten Proxy verwendet.

- Behandeln Sie das `connectionSlow` Ereignis, um eine Meldung anzuzeigen, sobald signalr Verbindungsprobleme erkennt, bevor es in den erneuten Verbindungs Modus wechselt.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Behandeln Sie das `reconnecting`-Ereignis, um eine Meldung anzuzeigen, wenn signalr eine Trennung der Verbindung erkennt und den Modus für die erneute Verbindung wiederherstellt.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Behandeln Sie das `disconnected`-Ereignis, um eine Meldung anzuzeigen, wenn beim Versuch, die Verbindung wiederherzustellen, ein Timeout aufgetreten ist In diesem Szenario ist die einzige Möglichkeit, erneut eine Verbindung mit dem Server herzustellen, die signalr-Verbindung neu zu starten, indem Sie die `Start`-Methode aufrufen, mit der eine neue Verbindungs-ID erstellt wird. Im folgenden Codebeispiel wird mithilfe eines Flags sichergestellt, dass die Benachrichtigung nur nach einem Timeout der erneuten Verbindungs Herstellung ausgegeben wird, nicht nach einem normalen Ende der signalr-Verbindung, die durch den Aufruf der `Stop`-Methode verursacht wurde.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Fortlaufende Wiederherstellung der Verbindung

In einigen Anwendungen möchten Sie möglicherweise automatisch eine Verbindung wiederherstellen, nachdem Sie verloren gegangen ist und der Versuch, eine Verbindung herzustellen, einen Timeout verursacht hat. Zu diesem Zweck können Sie die `Start`-Methode von Ihrem `Closed`-Ereignishandler (`disconnected`-Ereignishandler auf JavaScript-Clients) abrufen. Möglicherweise möchten Sie eine gewisse Zeit warten, bevor Sie `Start` aufrufen, um dies zu vermeiden, wenn der Server oder die physische Verbindung nicht verfügbar ist. Das folgende Codebeispiel gilt für einen JavaScript-Client, der den generierten Proxy verwendet.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Ein potenzielles Problem, das bei mobilen Clients zu beachten ist, besteht darin, dass bei einem kontinuierlichen erneuten Verbindungsversuch, wenn der Server oder die physische Verbindung nicht verfügbar ist, eine unnötige Akku Ableitung verursachen

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Trennen einer Verbindung zwischen einem Client und Servercode

Signalr Version 2 verfügt über keine integrierte Server-API zum Trennen der Verbindung von Clients. Es gibt [Pläne, diese Funktionalität in Zukunft hinzuzufügen](https://github.com/SignalR/SignalR/issues/2101). In der aktuellen signalr-Version ist die einfachste Möglichkeit, einen Client vom Server zu trennen, eine Disconnect-Methode auf dem Client zu implementieren und diese Methode vom Server aus aufzurufen. Das folgende Codebeispiel zeigt eine Disconnect-Methode für einen JavaScript-Client unter Verwendung des generierten Proxys.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Sicherheit: weder diese Methode zum Trennen der Verbindung von Clients noch die vorgeschlagene integrierte API adressiert das Szenario von gehackten Clients, die bösartigen Code ausführen, da die Clients erneut eine Verbindung herstellen können, oder der Hacker Code die `stopClient` Methode entfernen oder ändern kann. Der geeignete Ort zum Implementieren des Zustands behafteten Denial-of-Service (DOS)-Schutzes ist nicht im Framework oder auf der Serverebene, sondern in der Front-End-Infrastruktur.

<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Erkennen der Ursache für eine Verbindungs Trennung

Signalr 2,1 fügt dem Server eine Überladung hinzu `OnDisconnect` Ereignis, das angibt, ob der Client absichtlich getrennt wurde, anstatt ein Timeout festzustellen. Der `StopCalled`-Parameter ist true, wenn die Verbindung vom Client explizit geschlossen wurde. Wenn der Client in JavaScript aufgrund eines Server Fehlers die Verbindung getrennt hat, werden die Fehlerinformationen als `$.connection.hub.lastError`an den Client übermittelt.

**C#Servercode: `stopCalled`-Parameter**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**JavaScript-Client Code: Zugriff auf `lastError` im `disconnect`-Ereignis.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
