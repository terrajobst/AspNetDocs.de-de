---
uid: signalr/overview/performance/signalr-performance
title: Signalr-Leistung | Microsoft-Dokumentation
author: bradygaster
description: SignalR-Leistung
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449859"
---
# <a name="signalr-performance"></a>SignalR-Leistung

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Thema wird beschrieben, wie Sie die Leistung in einer signalr-Anwendung entwerfen, Messen und verbessern.
>
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendete Software Versionen
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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

Eine aktuelle Präsentation der signalr-Leistung und-Skalierung finden Sie unter [Skalieren des Echt Zeit Web mit ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).

Dieses Thema enthält folgende Abschnitte:

- [Überlegungen zum Entwurf](#design)
- [Optimieren des signalr-Servers auf die Leistung](#tuning)
- [Problembehandlung bei Leistungsproblemen](#troubleshooting)
- [Verwenden von signalr-Leistungsindikatoren](#perfcounters)
- [Verwenden anderer Leistungsindikatoren](#othercounters)
- [Weitere Ressourcen](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Überlegungen zum Entwurf

In diesem Abschnitt werden Muster beschrieben, die während des Entwurfs einer signalr-Anwendung implementiert werden können, um sicherzustellen, dass die Leistung nicht beeinträchtigt wird, indem unnötiger Netzwerkverkehr erzeugt wird.

### <a name="throttling-message-frequency"></a>Häufigkeit der Drosselung von Nachrichten

Selbst in einer Anwendung, die Nachrichten mit hoher Häufigkeit sendet (z. b. eine Echtzeit-Gaming-Anwendung), müssen die meisten Anwendungen nicht mehr als ein paar Nachrichten pro Sekunde senden. Um die Menge des von den einzelnen Clients generierten Datenverkehrs zu reduzieren, kann eine Nachrichten Schleife implementiert werden, die Nachrichten nicht häufiger als eine festgelegte Rate, sondern in eine Warteschlange eingereiht und sendet (d. h. bis zu eine bestimmte Anzahl von Nachrichten pro Sekunde gesendet werden, wenn Nachrichten in dieser Zeit vorhanden sind. das zu sendende euerstellungsintervall). Eine Beispielanwendung, die Nachrichten auf eine bestimmte Rate (sowohl vom Client als auch vom Server) drosselt, finden Sie unter [Hochfrequenz in Echtzeit mit signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Verringern der Nachrichtengröße

Sie können die Größe einer signalr-Nachricht verringern, indem Sie die Größe der serialisierten Objekte verringern. Wenn Sie in Servercode ein Objekt senden, das Eigenschaften enthält, die nicht übertragen werden müssen, verhindern Sie, dass diese Eigenschaften mithilfe des `JsonIgnore`-Attributs serialisiert werden. Die Namen von Eigenschaften werden ebenfalls in der Nachricht gespeichert. die Namen von Eigenschaften können mit dem `JsonProperty`-Attribut verkürzt werden. Im folgenden Codebeispiel wird veranschaulicht, wie eine Eigenschaft vom senden an den Client ausgeschlossen wird und wie Eigenschaftsnamen verkürzt werden:

**.Net-Servercode, der das jsonignore-Attribut veranschaulicht, um zu verhindern, dass Daten an den Client gesendet werden, und das jsonproperty-Attribut, um die Nachrichtengröße zu verringern**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Um die Lesbarkeit/Wartbarkeit im Client Code beizubehalten, können die abgekürzten Eigenschaftsnamen nach dem Empfang der Nachricht benutzerfreundlichen Namen zugeordnet werden. Im folgenden Codebeispiel wird eine Möglichkeit veranschaulicht, wie Sie gekürzte Namen in längere Weise neu zuordnen können, indem Sie einen Nachrichten Vertrag (Mapping) definieren und die `reMap`-Funktion verwenden, um den Vertrag auf die optimierte Nachrichten Klasse anzuwenden:

**Client seitiger JavaScript-Code, mit dem gekürzte Eigenschaftsnamen zu lesbaren Namen neu zugeordnet werden**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Namen können auch in Nachrichten vom Client an den Server mit derselben Methode verkürzt werden.

Wenn Sie den Speicherbedarf (d. h. die Menge an Arbeitsspeicher, der für die Nachricht verwendet wird) des Nachrichten Objekts verringern, kann auch die Leistung verbessert werden. Wenn z. b. der vollständige Bereich eines `int` nicht benötigt wird, kann stattdessen ein `short` oder `byte` verwendet werden.

Da Nachrichten im Nachrichtenbus im Server Speicher gespeichert werden, kann das Verringern der Größe der Nachrichten auch Probleme mit dem Server Arbeitsspeicher beheben.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Optimieren des signalr-Servers auf die Leistung

Die folgenden Konfigurationseinstellungen können verwendet werden, um Ihren Server für eine bessere Leistung in einer signalr-Anwendung zu optimieren. Allgemeine Informationen zum Verbessern der Leistung in einer ASP.NET-Anwendung finden Sie unter Verbessern der [Leistung von ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).

**Signalr-Konfigurationseinstellungen**

- **Defaultmessagebuffersize**: Standardmäßig speichert signalr 1000 Nachrichten pro Hub und Verbindung im Arbeitsspeicher. Wenn große Nachrichten verwendet werden, kann dies zu Arbeitsspeicher Problemen führen, die durch verringern dieses Werts verringert werden können. Diese Einstellung kann im `Application_Start` Ereignishandler in einer ASP.NET-Anwendung oder in der `Configuration`-Methode einer owin-Startklasse in einer selbst gehosteten Anwendung festgelegt werden. Im folgenden Beispiel wird veranschaulicht, wie dieser Wert verringert wird, um den Speicherbedarf Ihrer Anwendung zu verringern, um die Menge des verwendeten Server Arbeitsspeichers zu reduzieren:

    **.Net-Servercode in Startup.cs zum Verringern der standardmäßigen Nachrichten Puffergröße**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS-Konfigurationseinstellungen**

- **Maximale Anzahl gleichzeitiger Anforderungen pro Anwendung**: das Erhöhen der Anzahl gleichzeitiger IIS-Anforderungen erhöht die verfügbaren Server Ressourcen für die Bereitstellung von Anforderungen. Der Standardwert ist 5000; um diese Einstellung zu erhöhen, führen Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: Dies ist die maximale Anzahl von Anforderungen, die http. sys für den Anwendungs Pool in die Warteschlange stellt. Wenn die Warteschlange voll ist, erhalten neue Anforderungen die Antwort 503 "Dienst nicht verfügbar". Der Standardwert lautet „1000“.

    Durch Verkürzen der Warteschlangen Länge für den Workerprozess in dem Anwendungs Pool, der Ihre Anwendung gehostet, werden Speicherressourcen gespart. Weitere Informationen finden Sie unter [verwalten, optimieren und Konfigurieren von Anwendungs Pools](https://technet.microsoft.com/library/cc745955.aspx).

**ASP.NET-Konfigurationseinstellungen**

Dieser Abschnitt enthält Konfigurationseinstellungen, die in der `aspnet.config`-Datei festgelegt werden können. Diese Datei befindet sich an einem von zwei Speicherorten, abhängig von der Plattform:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

ASP.NET Einstellungen, die möglicherweise die signalr-Leistung verbessern, umfassen Folgendes:

- **Maximale Anzahl gleichzeitiger Anforderungen pro CPU**: durch das erhöhen dieser Einstellung können Leistungsengpässe verringert werden. Fügen Sie die folgende Konfigurationseinstellung in die `aspnet.config` Datei ein, um diese Einstellung zu erhöhen:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limit für Anforderungs Warteschlange**: Wenn die Gesamtzahl der Verbindungen die `maxConcurrentRequestsPerCPU` Einstellung überschreitet, beginnt ASP.net mit der Drosselung von Anforderungen mithilfe einer Warteschlange. Um die Größe der Warteschlange zu vergrößern, können Sie die `requestQueueLimit` Einstellung erhöhen. Fügen Sie zu diesem Zweck dem Knoten `processModel` in `config/machine.config` (anstatt `aspnet.config`) die folgende Konfigurationseinstellung hinzu:

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Problembehandlung bei Leistungsproblemen

In diesem Abschnitt werden Möglichkeiten zum Auffinden von Leistungs Engpässen in Ihrer Anwendung beschrieben.

### <a name="verifying-that-websocket-is-being-used"></a>Es wird überprüft, ob WebSocket verwendet wird.

Obwohl signalr eine Vielzahl von Transporten für die Kommunikation zwischen Client und Server verwenden kann, bietet WebSocket einen erheblichen Leistungsvorteil und sollte verwendet werden, wenn der Client und der Server dies unterstützen. Informationen zum ermitteln, ob der Client und der Server die Anforderungen für WebSocket erfüllen, finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports). Um zu ermitteln, welcher Transport in der Anwendung verwendet wird, können Sie die Browser Entwicklertools verwenden und die Protokolle überprüfen, um festzustellen, welcher Transport für die Verbindung verwendet wird. Informationen zur Verwendung der Browser-Entwicklungs Tools in Internet Explorer und Chrome finden Sie unter [Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Verwenden von signalr-Leistungsindikatoren

In diesem Abschnitt wird beschrieben, wie Sie signalr-Leistungsindikatoren aktivieren und verwenden, die im `Microsoft.AspNet.SignalR.Utils` Paket enthalten sind.

### <a name="installing-signalrexe"></a>Installieren von signalr. exe

Leistungsindikatoren können dem Server mithilfe eines Hilfsprogramms namens signalr. exe hinzugefügt werden. Um dieses Hilfsprogramm zu installieren, führen Sie die folgenden Schritte aus:

1. **Wählen Sie** in Visual Studio Extras > **nuget-Paket-Manager** > **nuget-Pakete für Projekt Mappe verwalten aus** .
2. Suchen Sie nach **signalr. utils**, und wählen Sie installieren aus.

    ![](signalr-performance/_static/image1.png)
3. Akzeptieren Sie den Lizenzvertrag, um das Paket zu installieren.
4. "Signalr. exe" wird auf `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`installiert.

### <a name="installing-performance-counters-with-signalrexe"></a>Installieren von Leistungsindikatoren mit "signalr. exe"

Um signalr-Leistungsindikatoren zu installieren, führen Sie signalr. exe an einer Eingabeaufforderung mit erhöhten Rechten mit dem folgenden Parameter aus:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Um signalr-Leistungsindikatoren zu entfernen, führen Sie signalr. exe an einer Eingabeaufforderung mit erhöhten Rechten mit dem folgenden Parameter aus:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Signalr-Leistungsindikatoren

Das Hilfsprogrammpaket installiert die folgenden Leistungsindikatoren. Die "Gesamt"-Leistungsindikatoren messen die Anzahl der Ereignisse seit dem letzten Anwendungs Pool oder Server Neustart.

**Verbindungsmetriken**

Die folgenden Metriken messen die auftretenden Ereignisse der Verbindungs Lebensdauer. Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Verbundene Verbindungen**
- **Verbindung wieder hergestellt**
- **Verbindungen getrennt**
- **Verbindungen aktuell**

**Nachrichtenmetriken**

Die folgenden Metriken messen den Nachrichtenverkehr, der von signalr generiert wurde.

- **Empfangene Verbindungs Nachrichten gesamt**
- **Gesendete Verbindungs Nachrichten gesamt**
- **Empfangene Verbindungs Nachrichten/Sek.**
- **Gesendete Verbindungs Nachrichten/Sek.**

**Nachrichtenbus Metriken**

Die folgenden Metriken messen den Datenverkehr über den internen signalr-Nachrichtenbus, die Warteschlange, in der alle eingehenden und ausgehenden signalr-Nachrichten abgelegt werden Eine Meldung wird **veröffentlicht** , wenn Sie gesendet oder übertragen wird. Ein **Abonnent** in diesem Kontext ist ein Abonnement für den Nachrichtenbus. Dies sollte gleich der Anzahl von Clients und dem Server selbst sein. Ein **zugewiesener Worker** ist eine Komponente, die Daten an aktive Verbindungen sendet. ein **ausgelasteter Worker** sendet eine Nachricht aktiv.

- **Empfangene Nachrichtenbus-Nachrichten gesamt**
- **Empfangene Message Bus-Nachrichten/Sek.**
- **Veröffentlichte Message Bus-Nachrichten gesamt**
- **Veröffentlichte Message Bus-Nachrichten/Sek.**
- **Nachrichtenbus Abonnenten aktuell**
- **Nachrichten busabonnenten Gesamt**
- **Nachrichtenbus Abonnenten/Sek.**
- **Von Message Bus zugewiesene Worker**
- **Ausgelastete Worker für Nachrichtenbus**
- **Message Bus-Themen aktuell**

**Fehlermetriken**

Die folgenden Metriken messen die vom signalr-Nachrichten Datenverkehr generierten Fehler. **Hub-Auflösungs** Fehler treten auf, wenn eine Hub-oder Hub-Methode nicht aufgelöst werden kann. **Hubaufruf** Fehler sind Ausnahmen, die beim Aufrufen einer hubmethode ausgelöst werden. **Transport** Fehler sind Verbindungsfehler, die während einer HTTP-Anforderung oder-Antwort ausgelöst werden.

- **Fehler: alle Gesamt**
- **Fehler: alle/Sek.**
- **Fehler: Hub-Auflösung Gesamt**
- **Fehler: Hub-Auflösung/Sek.**
- **Fehler: hubaufruf Gesamt**
- **Fehler: hubaufrufe/Sek.**
- **Fehler: Gesamt Transport**
- **Fehler: Transport/Sek.**

<a id="scaleout_metrics"></a>

**Skalierbare Metriken**

Die folgenden Metriken Messen Datenverkehr und Fehler, die vom Anbieter für horizontales Skalieren generiert werden. Ein Daten **Strom** in diesem Kontext ist eine Skalierungs Einheit, die vom Anbieter für horizontales Skalieren verwendet wird. Dies ist eine Tabelle, wenn SQL Server verwendet wird, ein Thema, wenn Service Bus verwendet wird, und ein Abonnement, wenn redis verwendet wird. Jeder Stream stellt geordnete Lese-und Schreibvorgänge sicher. ein einzelner Stream ist ein potenzieller Skalierungs Engpass. Daher kann die Anzahl der Streams angehoben werden, um den Engpass zu verringern. Wenn mehrere Streams verwendet werden, verteilt signalr (Shard) Nachrichten automatisch auf eine Weise, die sicherstellt, dass die von einer beliebigen Verbindung gesendeten Nachrichten in der richtigen Reihenfolge vorliegen.

Mit der Einstellung [maxqueuelength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) wird die Länge der von signalr verwalteten Sende Warteschlange für horizontales Skalieren gesteuert. Wenn ein Wert größer als 0 festgelegt wird, werden alle Nachrichten in einer Sende Warteschlange nacheinander an die konfigurierte Messaging-Backplane gesendet. Wenn die Größe der Warteschlange die konfigurierte Länge überschreitet, tritt bei nachfolgenden Sende aufrufen sofort ein Fehler mit einer [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) auf, bis die Anzahl der Nachrichten in der Warteschlange kleiner ist als die Einstellung. Die Warteschlange ist standardmäßig deaktiviert, da die implementierten Backplane in der Regel über eine eigene Warteschlange oder Fluss Steuerung verfügen. Im Fall von SQL Server schränkt das Verbindungspooling die Anzahl von gesendeten Sendungen zu einem beliebigen Zeitpunkt ein.

Standardmäßig wird nur ein Stream für SQL Server und redis verwendet, fünf Streams werden für Service Bus verwendet, und die Warteschlange ist deaktiviert. diese Einstellungen können jedoch mithilfe der Konfiguration auf SQL Server und Service Bus geändert werden:

**.NET-Server Code zum Konfigurieren der Tabellen Anzahl und der Warteschlangen Länge für SQL Server Rückwand**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**.NET-Server Code zum Konfigurieren der Themen Anzahl und der Warteschlangen Länge für Service Bus Rückwand**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Ein **Puffer** Datenstrom ist ein Puffer, der in einen Fehlerzustand versetzt wurde. Wenn sich der Stream im Faulted-Zustand befindet, schlagen alle an die Rückwand gesendeten Nachrichten sofort fehl, bis der Datenstrom nicht mehr fehlerhaft ist. Die **Länge der Sende Warteschlange** entspricht der Anzahl der Nachrichten, die gesendet, aber noch nicht gesendet wurden.

- **Horizontal hochskalierte Nachrichtenbus Nachrichten/Sek.**
- **Gesamtanzahl von horizontal skalierbaren Streams**
- **Hochskalierte Streams geöffnet**
- **Skalieren von streamingstreams**
- **ScaleOut-Fehler Gesamt**
- **Skalierungs Fehler/Sek.**
- **Länge der Sende Warteschlange für horizontale Skalierung**

Weitere Informationen zu den Leistungsindikatoren, die von diesen Indikatoren gemessen werden, finden Sie im Thema zum horizontalen hoch [Skalieren mit Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Verwenden anderer Leistungsindikatoren

Die folgenden Leistungsindikatoren können auch beim Überwachen der Leistung Ihrer Anwendung nützlich sein.

**Arbeitsspeicher**

- .NET CLR-Speicher\\# Bytes in allen Heaps (für w3wp)

**ASP.NET**

- ASP. NET\Aktuelle Anforderungen
- ASP.NET\Queued
- ASP. net\abgelehnte

**CPU**

- Prozessorinformationen\prozessorzeit

**TCP/IP**

- TCPv6/Verbindungen hergestellt
- TCPv4/Verbindungen hergestellt

**Webdienst**

- Webdienst\aktuelle Verbindungen
- Webdienst\maximale Verbindungen

**Threading**

- .NET CLR-Sperren und-Threads\\Anzahl der aktuellen logischen Threads
- .NET CLR-Sperren und-Threads\\Anzahl der aktuellen physischen Threads

<a id="otherresources"></a>

## <a name="other-resources"></a>Weitere Ressourcen

Weitere Informationen zur Leistungsüberwachung und-Optimierung in ASP.net finden Sie in den folgenden Themen:

- [ASP.NET Performance Overview (Die Leistung von ASP.NET im Überblick)](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [ASP.net Thread Verwendung auf IIS 7,5, IIS 7,0 und IIS 6,0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;ApplicationPool&gt; Element (Webeinstellungen)](https://msdn.microsoft.com/library/dd560842.aspx)
