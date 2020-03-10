---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Aktivieren der signalr-Ablauf Verfolgung | Microsoft-Dokumentation
author: bradygaster
description: In diesem Dokument wird beschrieben, wie die Ablauf Verfolgung für signalr-Server und-Clients aktiviert und konfiguriert wird. Mithilfe der Ablauf Verfolgung können Sie Diagnoseinformationen zu Ereignissen anzeigen...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 34fe2cdb10c4b41a6e8cac7fb1741d53c02dfc80
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467439"
---
# <a name="enabling-signalr-tracing"></a>Aktivieren der Ablaufverfolgung für SignalR

von [Tom fitzmacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Dokument wird beschrieben, wie die Ablauf Verfolgung für signalr-Server und-Clients aktiviert und konfiguriert wird. Mithilfe der Ablauf Verfolgung können Sie Diagnoseinformationen zu Ereignissen in der signalr-Anwendung anzeigen.
>
> Dieses Thema wurde ursprünglich von Patrick Fletcher geschrieben.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - Signalr Version 2
>
>
>
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
>
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

Wenn die Ablauf Verfolgung aktiviert ist, erstellt eine signalr-Anwendung Protokolleinträge für Ereignisse. Sie können Ereignisse sowohl vom Client als auch vom Server protokollieren. Ablauf Verfolgung auf dem Server protokolliert die Verbindung, den Anbieter für horizontales hochskalieren und Nachrichtenbus Ereignisse. Bei der Ablauf Verfolgung auf dem Client werden Verbindungs Ereignisse protokolliert. In signalr 2,1 und höher protokolliert die Ablauf Verfolgung auf dem Client den vollständigen Inhalt von hubaufruf Nachrichten.

## <a name="contents"></a>Inhalt

- [Aktivieren der Ablauf Verfolgung auf dem Server](#server)

    - [Protokollieren von Server Ereignissen in Textdateien](#server_text)
    - [Protokollieren von Server Ereignissen im Ereignisprotokoll](#server_eventlog)
- [Aktivieren der Ablauf Verfolgung im .NET-Client (Windows-Desktop-Apps)](#net_client)

    - [Protokollieren von Desktop Client Ereignissen an der Konsole](#desktop_console)
    - [Protokollieren von Desktop Client Ereignissen in einer Textdatei](#desktop_text)
- [Aktivieren der Ablauf Verfolgung in Windows Phone 8-Clients](#phone)

    - [Protokollieren von Windows Phone Client Ereignissen auf der Benutzeroberfläche](#phone_ui)
    - [Protokollieren von Windows Phone Client Ereignissen in der Debug-Konsole](#phone_debug)
- [Aktivieren der Ablauf Verfolgung im JavaScript-Client](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Aktivieren der Ablauf Verfolgung auf dem Server

Sie aktivieren die Ablauf Verfolgung auf dem Server in der Konfigurationsdatei der Anwendung ("App. config" oder "Web. config", je nach Projekttyp). Sie geben an, welche Kategorien von Ereignissen Sie protokollieren möchten. In der Konfigurationsdatei geben Sie auch an, ob die Ereignisse in einer Textdatei, im Windows-Ereignisprotokoll oder in einem benutzerdefinierten Protokoll mithilfe einer Implementierung von [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx)protokolliert werden sollen.

Die Server Ereignis Kategorien umfassen die folgenden Arten von Nachrichten:

| `Source` | Meldungen |
| --- | --- |
| SignalR.SqlMessageBus | Setup für den SQL-Nachrichtenbus-Anbieter für horizontales Skalieren, Daten Bank Vorgänge, Fehler und Timeout |
| SignalR.ServiceBusMessageBus | Service Bus-Anbieter für horizontales Skalieren: Erstellen von und Abonnement-, Fehler-und Messaging Ereignissen |
| SignalR.RedisMessageBus | Redis-Anbieter Verbindung für horizontales hochskalieren, Verbindungsfehler und Fehlerereignisse |
| SignalR.ScaleoutMessageBus | Horizontales Skalieren von Messaging Ereignissen |
| SignalR.Transports.WebSocketTransport | WebSocket-Transport Verbindung, trennen von Verbindungs-, Messaging-und Fehlerereignissen |
| SignalR.Transports.ServerSentEventsTransport | Serversentevents-Transport Verbindung, trennen von Verbindungs-, Messaging-und Fehlerereignissen |
| SignalR.Transports.ForeverFrameTransport | Foreverframe-Transport Verbindung, trennen von Verbindungs-, Messaging-und Fehlerereignissen |
| SignalR.Transports.LongPollingTransport | Longabruf-Transport Verbindung, trennen von Verbindungs-, Messaging-und Fehlerereignissen |
| SignalR.Transports.TransportHeartBeat | Transport Verbindung, trennen von Verbindungen und KeepAlive-Ereignissen |
| SignalR.ReflectedHubDescriptorProvider | Hub-Ermittlungs Ereignisse |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Protokollieren von Server Ereignissen in Textdateien

Der folgende Code zeigt, wie die Ablauf Verfolgung für jede Ereignis Kategorie aktiviert wird. In diesem Beispiel wird die Anwendung so konfiguriert, dass Ereignisse in Textdateien protokolliert werden.

**XML-Servercode zum Aktivieren der Ablauf Verfolgung**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Im obigen Code gibt der `SignalRSwitch` Eintrag den [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) an, der für Ereignisse verwendet wird, die an das angegebene Protokoll gesendet werden. In diesem Fall wird Sie auf `Verbose` festgelegt, was bedeutet, dass alle Debugging-und Ablauf Verfolgungs Meldungen protokolliert werden.

Die folgende Ausgabe zeigt Einträge aus der `transports.log.txt`-Datei für eine Anwendung, die die oben beschriebene Konfigurationsdatei verwendet. Es werden eine neue Verbindung, eine entfernte Verbindung und Transport Takt Ereignisse angezeigt.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Protokollieren von Server Ereignissen im Ereignisprotokoll

Um Ereignisse anstelle einer Textdatei im Ereignisprotokoll zu protokollieren, ändern Sie die Werte für die Einträge im Knoten `sharedListeners`. Der folgende Code zeigt, wie Server Ereignisse im Ereignisprotokoll protokolliert werden:

**XML-Servercode zum Protokollieren von Ereignissen im Ereignisprotokoll**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Die Ereignisse werden im Anwendungsprotokoll protokolliert und sind über die Ereignisanzeige verfügbar, wie unten dargestellt:

![Ereignisanzeige mit signalr-Protokollen](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Wenn Sie das Ereignisprotokoll verwenden, legen Sie für die **TraceLevel** den Wert " **Error** " fest, um die Anzahl der Nachrichten beizubehalten.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Aktivieren der Ablauf Verfolgung im .NET-Client (Windows-Desktop-Apps)

Der .NET-Client kann mit einer Implementierung von [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)Ereignisse in der Konsole, in einer Textdatei oder in einem benutzerdefinierten Protokoll protokollieren.

Wenn Sie die Protokollierung im .NET-Client aktivieren möchten, legen Sie die `TraceLevel`-Eigenschaft der Verbindung auf einen [Tracelevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) -Wert und die Eigenschaft `TraceWriter` auf eine gültige [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) -Instanz fest.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Protokollieren von Desktop Client Ereignissen an der Konsole

Der folgende C# Code zeigt, wie Ereignisse im .NET-Client in der Konsole protokolliert werden:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Protokollieren von Desktop Client Ereignissen in einer Textdatei

Der folgende C# Code zeigt, wie Ereignisse im .NET-Client in einer Textdatei protokolliert werden:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Die folgende Ausgabe zeigt Einträge aus der `ClientLog.txt`-Datei für eine Anwendung, die die oben beschriebene Konfigurationsdatei verwendet. Es zeigt den Client, der eine Verbindung mit dem Server herstellt, und der Hub, der eine Client Methode mit dem Namen `addMessage`aufruft:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Aktivieren der Ablauf Verfolgung in Windows Phone 8-Clients

Signalr-Anwendungen für Windows Phone-Apps verwenden denselben .NET-Client wie Desktop-Apps, aber [Console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) und das Schreiben in eine Datei mit [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) sind nicht verfügbar. Stattdessen müssen Sie eine benutzerdefinierte Implementierung von [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) für die Ablauf Verfolgung erstellen.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Protokollieren von Windows Phone Client Ereignissen auf der Benutzeroberfläche

Die [signalr-Codebasis](https://github.com/SignalR/SignalR/archive/master.zip) enthält ein Windows Phone Beispiel, das die Ablauf Verfolgungs Ausgabe mithilfe einer benutzerdefinierten [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) -Implementierung namens `TextBlockWriter`in einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) schreibt. Diese Klasse befindet sich im Projekt **Samples/Microsoft. Aspnet. signalr. Client. WP8. Samples** . Wenn Sie eine Instanz von `TextBlockWriter`erstellen, übergeben Sie den aktuellen [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)und einen [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , in dem ein [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) erstellt wird, der für die Ausgabe der Ablauf Verfolgung verwendet wird:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

Die Ausgabe der Ablauf Verfolgung wird dann in einen neuen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) geschrieben, der im [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) erstellt wurde, das Sie eingegeben haben:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Protokollieren von Windows Phone Client Ereignissen in der Debug-Konsole

Um die Ausgabe an die Debugkonsole anstatt an die Benutzeroberfläche zu senden, erstellen Sie eine [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) -Implementierung, die in das Debugfenster schreibt, und weisen Sie Sie der Eigenschaft [traceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) ihrer Verbindung zu:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Ablauf Verfolgungs Informationen werden dann in das Debugfenster in Visual Studio geschrieben:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Aktivieren der Ablauf Verfolgung im JavaScript-Client

Um die Client seitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging`-Eigenschaft für das Verbindungs Objekt fest, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen.

**JavaScript-Client Code zum Aktivieren der Ablauf Verfolgung für die Browser Konsole (mit dem generierten Proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**JavaScript-Client Code zum Aktivieren der Ablauf Verfolgung in der Browser Konsole (ohne den generierten Proxy)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Wenn die Ablauf Verfolgung aktiviert ist, protokolliert der JavaScript-Client Ereignisse in der Browser Konsole. Informationen zum Zugriff auf die Browser Konsole finden Sie unter über [Wachen von Transporten](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Der folgende Screenshot zeigt einen signalr-JavaScript-Client mit aktivierter Ablauf Verfolgung. Es werden Verbindungs-und hubaufruf Ereignisse in der Browser Konsole angezeigt:

![Signalr-Ablauf Verfolgungs Ereignisse in der Browser Konsole](enabling-signalr-tracing/_static/image4.png)
