---
uid: signalr/overview/getting-started/supported-platforms
title: Unterstützte Plattformen | Microsoft-Dokumentation
author: bradygaster
description: In diesem Artikel wird beschrieben, welche Clients und Server von signalr unterstützt werden.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 89730e591bb94022d16fe1a78a4350b38e0bf7a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450123"
---
# <a name="supported-platforms"></a>Unterstützte Plattformen

von [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In diesem Artikel wird beschrieben, welche Clients und Server von signalr unterstützt werden. 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten. Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.

Signalr wird unter einer Vielzahl von Server-und Client Konfigurationen unterstützt. Außerdem verfügt jede Transport Option über einen eigenen Satz von Anforderungen. Wenn die Systemanforderungen für einen Transport nicht verfügbar sind, führt signalr ein ordnungsgemäßes Failover auf andere Transporte durch. Weitere Informationen zu den von signalr unterstützten Transporten finden Sie unter [Transporte und Fallbacks](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Systemanforderungen für Server

Die signalr-Serverkomponente kann auf einer Vielzahl von Serverkonfigurationen gehostet werden. In diesem Abschnitt werden die unterstützten Versionen von Betriebssystemen, .NET Framework, Internet Informations Server und anderen Komponenten beschrieben.

### <a name="supported-server-operating-systems"></a>Unterstützte Serverbetriebssysteme

Die signalr-Serverkomponente kann auf den folgenden Server-oder Client Betriebssystemen gehostet werden. Beachten Sie, dass für signalr websockets verwendet werden muss, Windows Server 2012, Windows Server 2016 oder Windows 8 erforderlich ist (WebSocket kann auf Windows Azure-Websites verwendet werden, solange die .NET Framework-Version der Website auf 4,5 festgelegt ist und websockets im Konfigurationsseite).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 R2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Unterstützte Server .NET Framework Version

Signalr 2 wird nur auf .NET Framework 4,5 unterstützt. Weitere Informationen zu Updates, die die Zuverlässigkeit, Kompatibilität, Stabilität und Leistung verbessern, finden Sie im Abschnitt [Empfohlene Updates](#updates) .

### <a name="supported-server-iis-versions"></a>Unterstützte Server-IIS-Versionen

Wenn signalr in IIS gehostet wird, werden die folgenden Versionen unterstützt. Beachten Sie Folgendes: Wenn ein Client Betriebssystem verwendet wird, z. b. für die Entwicklung (Windows 8 oder Windows 7), sollten vollständige Versionen von IIS oder Cassini nicht verwendet werden, da es eine Beschränkung auf 10 gleichzeitige Verbindungen gibt, die seit der Verbindungs Herstellung sehr schnell erreicht werden. sind flüchtig, häufig wieder hergestellt und werden nicht sofort freigegeben, wenn Sie nicht mehr verwendet werden. IIS Express sollte auf Client Betriebssystemen verwendet werden.

Beachten Sie außerdem, dass für signalr WebSocket verwendet werden muss, IIS 8 oder IIS 8 Express verwendet werden muss, der Server Windows 8, Windows Server 2012 oder höher verwenden muss und dass WebSocket in IIS aktiviert sein muss. Informationen zum Aktivieren von WebSocket in IIS finden Sie [unter IIS 8,0 WebSocket-Protokoll Unterstützung](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 oder IIS 8 Express.
- IIS 7 und 7,5. Unterstützung für [Erweiterungs lose URLs](https://support.microsoft.com/kb/980368) ist erforderlich.
- IIS muss im integrierten Modus ausgeführt werden. der klassische Modus wird nicht unterstützt. Nachrichten Verzögerungen von bis zu 30 Sekunden können auftreten, wenn IIS im klassischen Modus ausgeführt wird, indem der Transport für Server gesendete Ereignisse verwendet wird.
- Die Host Anwendung muss im Modus mit vollständiger Vertrauenswürdigkeit ausgeführt werden.

## <a name="client-system-requirements"></a>Systemanforderungen für Clients

Signalr kann auf verschiedenen Client Plattformen verwendet werden. In diesem Abschnitt werden die Systemanforderungen für die Verwendung von signalr in Webbrowsern, Windows-Desktop Anwendungen, Silverlight-Anwendungen und mobilen Geräten beschrieben.

### <a name="web-browsers"></a>Webbrowser

Signalr kann in einer Vielzahl von Webbrowsern verwendet werden, aber in der Regel werden nur die letzten beiden Versionen unterstützt.

Anwendungen, die signalr in Browsern verwenden, müssen jQuery-Version 1.6.4 oder größere spätere Versionen (z. b. 1.7.2, 1.8.2 oder 1.9.1) verwenden.

Signalr kann in den folgenden Browsern verwendet werden:

- Microsoft Internet Explorer, Version 8, 9, 10 und 11. Moderne, Desktop-und Mobile Versionen werden unterstützt.
- Mozilla Firefox: aktuelle Version-1, Windows-und Mac-Versionen.
- Google Chrome: aktuelle Version-1, Windows-und Mac-Versionen.
- Safari: aktuelle Version-1, Mac-und IOS-Versionen.
- Opera: aktuelle Version-1, nur Windows.
- Android-Browser

Die verschiedenen von signalr verwendeten Transporte sind nicht nur für bestimmte Browser erforderlich, sondern auch für Ihre eigenen Anforderungen. Die folgenden Transporte werden unter den folgenden Konfigurationen unterstützt:

<a id="browser"></a>

**Webbrowser-Transport Anforderungen**

| Transport | Internet Explorer | Chrome (Windows oder IOS) | Firefox | Safari (OSX oder IOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10 und höher | aktuell-1 | aktuell-1 | aktuell-1 | N/V |
| Vom Server gesendete Ereignisse | N/V | aktuell-1 | aktuell-1 | aktuell-1 | N/V |
| ForeverFrame | 8 und höher | N/V | N/V | N/V | 4,1 |
| Langer Abruf | 8 und höher | aktuell-1 | aktuell-1 | aktuell-1 | 4,1 |

\*: 6 + erforderlich für die vollständige Funktionalität.

#### <a name="unsupported-browsers"></a>Nicht unterstützte Browser

Obwohl signalr ohne größere Probleme in älteren Browserversionen ausgeführt werden *kann* , testen wir signalr nicht aktiv in Ihnen und beheben in der Regel keine Fehler, die möglicherweise in ihnen auftreten.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows-Desktop-und Silverlight-Anwendungen

Zusätzlich zur Ausführung in einem Webbrowser kann signalr in eigenständigen Windows-Client-oder Silverlight-Anwendungen gehostet werden. Für Windows Desktop-und Silverlight-signalr-Anwendungen gelten die folgenden Systemanforderungen.

- Anwendungen, die .NET 4 verwenden, werden unter Windows XP SP3 oder höher unterstützt.
- Anwendungen, die .NET Framework 4,5 verwenden, werden unter Windows Vista oder höher unterstützt.

Zusätzlich zu den Anforderungen an das Betriebssystem und .NET Framework haben die für signalr verfügbaren Transportanforderungen eigene Anforderungen. Die folgenden Transporte werden unter den folgenden Konfigurationen unterstützt:

**Windows-Desktop-und Silverlight-Transport Anforderungen**

| Transport | .NET-Anwendung | Silverlight |
| --- | --- | --- |
| Websockets | Windows 8 und höher und .NET 4.5 und höher | N/V |
| Forever-Frame | N/V | N/V |
| Vom Server gesendete Ereignisse | .NET 4+ | 5+ |
| Langer Abruf | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store-und Windows Phone-Anwendungen

Signalr kann in Windows Store-Anwendungen und Windows Phone 8-Anwendungen verwendet werden. Die folgenden Transporte werden unter den folgenden Konfigurationen unterstützt:

**Windows Store-und Windows Phone Transport Anforderungen**

| Transport | Windows Store/.net | Windows Store/JavaScript | Windows Phone/IE | Windows phone/.net |
| --- | --- | --- | --- | --- |
| WebSockets | N/V | Win8+ | 8 und höher | N/V |
| Forever-Frame | N/V | Win8+ | 7.5 und höher | N/V |
| Vom Server gesendete Ereignisse | Win8+ | N/V | N/V | 8 und höher |
| Langer Abruf | Win8+ | Win8+ | 7.5 und höher | 8 und höher |

<a id="updates"></a>

## <a name="recommended-updates"></a>Empfohlene Updates

Die folgenden Updates werden für signalr-Server empfohlen:

- Ein Update für .NET Framework 4,5 ist [hier](https://support.microsoft.com/kb/2750149)verfügbar.
- Microsoft wird in regelmäßigen Abständen QFEs für ASP.net freigeben. Diese sollten als verfügbar angewendet werden.
