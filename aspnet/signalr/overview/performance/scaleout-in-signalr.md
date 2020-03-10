---
uid: signalr/overview/performance/scaleout-in-signalr
title: Einführung in horizontales hochskalieren in signalr | Microsoft-Dokumentation
author: bradygaster
description: In den in diesem Thema verwendeten Software Versionen Visual Studio 2013 .NET 4,5 signalr Version 2 in früheren Versionen dieses Themas Informationen zu früheren Versionen von...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14dc22f99a43b566903c59fb23b7d419350f4a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467781"
---
# <a name="introduction-to-scaleout-in-signalr"></a>Einführung zur horizontalen Skalierung in SignalR

von [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

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

Im Allgemeinen gibt es zwei Möglichkeiten zum Skalieren einer Webanwendung: zentrales *hoch* -und herunter *skalieren*.

- Zentrales hochskalieren bedeutet, dass Sie einen größeren Server (oder einen größeren virtuellen Computer) mit mehr RAM, CPUs usw. verwenden.
- Durch horizontales hochskalieren werden weitere Server zum Verarbeiten der Last hinzugefügt.

Das Problem beim zentralen hochskalieren besteht darin, dass Sie schnell eine Beschränkung für die Größe des Computers erreichen. Darüber hinaus müssen Sie horizontal hochskalieren. Wenn Sie jedoch horizontal hochskalieren, können Clients an andere Server weitergeleitet werden. Ein Client, der mit einem Server verbunden ist, empfängt keine Nachrichten, die von einem anderen Server gesendet werden.

![](scaleout-in-signalr/_static/image1.png)

Eine Lösung besteht darin, Nachrichten zwischen Servern mithilfe einer Komponente weiterzuleiten, die als *Rückwand*bezeichnet wird. Wenn eine Rückwand aktiviert ist, sendet jede Anwendungs Instanz Nachrichten an die Rückwand, und die Rückwand leitet Sie an die anderen Anwendungs Instanzen weiter. (In der Elektronik ist eine Rückwand eine Gruppe paralleler Connectors. Analog dazu verbindet eine signalr-Rückwand mehrere Server.)

![](scaleout-in-signalr/_static/image2.png)

Signalr bietet zurzeit drei Backplane:

- **Azure Service Bus** Service Bus ist eine Messaging Infrastruktur, die es Komponenten ermöglicht, Nachrichten in einer lose gekoppelten Weise zu senden.
- **Redis**. Redis ist ein Schlüssel-Wert-Speicher im Arbeitsspeicher. Redis unterstützt ein Veröffentlichen/Abonnieren-Muster ("Pub/Sub") zum Senden von Nachrichten.
- **SQL Server**. Die SQL Server Rückwand schreibt Nachrichten in SQL-Tabellen. Die Rückwand verwendet Service Broker für effizientes Messaging. Es funktioniert jedoch auch, wenn Service Broker nicht aktiviert ist.

Wenn Sie Ihre Anwendung in Azure bereitstellen, sollten Sie die redis-Rückwand mithilfe von [Azure redis Cache](https://azure.microsoft.com/services/cache/)verwenden. Wenn Sie in ihrer eigenen Serverfarm bereitstellen, sollten Sie die SQL Server-oder redis-Backplane in Erwägung gezogen.

Die folgenden Themen enthalten Schritt-für-Schritt-Tutorials für jede Backplane:

- [Horizontale Skalierung in SignalR mit dem Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Horizontale Skalierung in SignalR mit Redis](scaleout-with-redis.md)
- [Horizontale Skalierung in SignalR mit SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementierung

In signalr wird jede Nachricht über einen Nachrichtenbus gesendet. Ein Nachrichtenbus implementiert die [imessagebus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) -Schnittstelle, die eine Veröffentlichung/Abonnement-Abstraktion bereitstellt. Die Rückstände funktionieren, indem Sie den standardmäßigen **imessagebus** durch einen Bus ersetzen, der für diese Backplane konzipiert ist. Der Nachrichtenbus für redis lautet beispielsweise [redismessagebus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)und verwendet den redis [-Pub/Sub-](http://redis.io/topics/pubsub) Mechanismus, um Nachrichten zu senden und zu empfangen.

Jede Serverinstanz stellt über den Bus eine Verbindung mit der Rückwand her. Wenn eine Nachricht gesendet wird, wird Sie an die Rückwand gesendet, und die Rückwand sendet Sie an jeden Server. Wenn ein Server eine Nachricht von der Backplane erhält, wird die Nachricht in den lokalen Cache eingefügt. Der Server übergibt dann Nachrichten aus seinem lokalen Cache an Clients.

Für jede Client Verbindung wird der Fortschritt des Clients beim Lesen des Nachrichten Datenstroms mithilfe eines Cursors nachverfolgt. (Ein Cursor stellt eine Position im Nachrichtenstream dar.) Wenn ein Client die Verbindung trennt und dann erneut eine Verbindung herstellt, fragt er den Bus nach Nachrichten ab, die nach dem Cursor Wert des Clients eingetroffen sind. Dasselbe passiert, wenn eine Verbindung eine [lange](../getting-started/introduction-to-signalr.md#transports)Abfrage verwendet. Nachdem eine lange Abruf Anforderung abgeschlossen wurde, öffnet der Client eine neue Verbindung und fragt nach Nachrichten, die nach dem Cursor eingetroffen sind.

Der Cursor Mechanismus funktioniert auch dann, wenn ein Client bei der erneuten Verbindungs Herstellung an einen anderen Server weitergeleitet wird. Die Rückwand kennt alle Server, und es spielt keine Rolle, mit welchem Server ein Client eine Verbindung herstellt.

## <a name="limitations"></a>Einschränkungen

Bei Verwendung einer Backplane ist der maximale Nachrichten Durchsatz niedriger als der, wenn Clients direkt mit einem einzelnen Server Knoten kommunizieren. Das liegt daran, dass die Rück Ebene jede Nachricht an jeden Knoten weiterleitet, sodass die Rückwand zu einem Engpass werden kann. Ob diese Einschränkung ein Problem ist, hängt von der Anwendung ab. Im folgenden finden Sie einige typische signalr-Szenarien:

- [Server Broadcast](../getting-started/tutorial-server-broadcast-with-signalr.md) (z. b. Börsen Ticker): Backplane funktionieren für dieses Szenario gut, da der Server die Rate steuert, mit der Nachrichten gesendet werden.
- [Client-zu-Client](../getting-started/tutorial-getting-started-with-signalr.md) (z. b. Chat): in diesem Szenario kann die Rückwand einen Engpass darstellen, wenn die Anzahl der Nachrichten mit der Anzahl der Clients skaliert wird. Das heißt, wenn die Rate der Nachrichten proportional zunimmt, wenn mehr Clients beitreten.
- [Hochfrequenz in Echtzeit](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (z. b. Echt Zeit Spiele): für dieses Szenario wird keine Rückwand empfohlen.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Aktivieren der Ablauf Verfolgung für die horizontale Skalierung von signalr

Fügen Sie der Datei "Web. config" unter dem Stamm **Konfigurations** Element die folgenden Abschnitte hinzu, um die Ablauf Verfolgung für die Rück Ebenen zu aktivieren:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
