---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Verteiltes Caching (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Das e-Book zur Entwicklung realer Cloud-apps mit Azure basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Vorgehensweisen erläutert, für die er...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: c66187b990a828c53bd2f8115e3c9660fc6022ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582807"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Verteiltes Caching (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Verfahren erläutert, die Ihnen bei der Entwicklung von Web-Apps für die Cloud helfen können. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Im vorherigen Kapitel wurde die Behandlung vorübergehender Fehler und die erwähnte Zwischenspeicherung als Strategie für einen Trennschalter behandelt. Dieses Kapitel bietet weitere Hintergrundinformationen zur Zwischenspeicherung, einschließlich der Verwendungszwecke, allgemeiner Muster für deren Verwendung und der Implementierung in Azure.

## <a name="what-is-distributed-caching"></a>Was ist verteilte Zwischenspeicherung?

Ein Cache bietet einen hohen Durchsatz und eine geringe Latenzzeit für den Zugriff auf häufig verwendete Anwendungsdaten, indem die Daten im Arbeitsspeicher gespeichert werden. Für eine Cloud-APP ist der nützlichste Cachetyp der verteilte Cache, d. h. die Daten werden nicht im Arbeitsspeicher des einzelnen Webservers, sondern in anderen cloudressourcen gespeichert, und die zwischengespeicherten Daten werden allen Webservern einer Anwendung (oder anderen virtuellen Cloud-Computern) zur Verfügung gestellt, die e von der Anwendung verwendet).

![Diagramm mit mehreren Webservern, die auf die gleichen Cache Server zugreifen](distributed-caching/_static/image1.png)

Wenn die Anwendung durch Hinzufügen oder Entfernen von Servern skaliert wird oder wenn Server aufgrund von Upgrades oder Fehlern ersetzt werden, bleiben die zwischengespeicherten Daten für jeden Server, auf dem die Anwendung ausgeführt wird, zugänglich.

Durch die Vermeidung des Datenzugriffs mit hoher Latenzzeit eines permanenten Datenspeichers kann das Caching die Reaktionsfähigkeit der Anwendung erheblich verbessern. Das Abrufen von Daten aus dem Cache ist beispielsweise viel schneller als das Abrufen aus einer relationalen Datenbank.

Ein neben Vorteil der Zwischenspeicherung ist reduzierter Datenverkehr zum persistenten Datenspeicher, was zu geringeren Kosten führen kann, wenn für den persistenten Datenspeicher Gebühren anfallen.

## <a name="when-to-use-distributed-caching"></a>Verwendungszwecke von verteiltem Caching

Das Caching funktioniert am besten für anwendungsworkloads, die mehr Lesevorgänge als das Schreiben von Daten durchführen, und wenn das Datenmodell die Schlüssel-Wert-Organisation unterstützt, die Sie zum Speichern und Abrufen von Daten im Cache verwenden. Es ist auch nützlicher, wenn Anwendungs Benutzer viele gängige Daten gemeinsam nutzen. beispielsweise bietet der Cache nicht so viele Vorteile, wenn jeder Benutzer in der Regel Daten abruft, die für diesen Benutzer eindeutig sind. Ein Beispiel, bei dem Caching sehr vorteilhaft sein könnte, ist ein Produktkatalog, da sich die Daten nicht häufig ändern und alle Kunden die gleichen Daten betrachten.

Der Vorteil der Zwischenspeicherung erhöht sich immer mehr, je mehr Anwendungen skaliert werden, da die Durchsatz Limits und Latenz Verzögerungen des permanenten Datenspeichers zu einer größeren Einschränkung der Gesamtleistung der Anwendung werden. Sie können das Caching jedoch auch aus anderen Gründen als der Leistung implementieren. Für Daten, die nicht perfekt auf dem neuesten Stand sein müssen, wenn Sie dem Benutzer angezeigt werden, kann der Cache Zugriff als Trennschalter dienen, wenn der persistente Datenspeicher nicht reagiert oder nicht verfügbar ist.

## <a name="popular-cache-population-strategies"></a>Beliebte Strategien für die Cache Population

Damit Daten aus dem Cache abgerufen werden können, müssen Sie Sie zuerst speichern. Es gibt mehrere Strategien zum erhalten von Daten, die Sie in einen Cache benötigen:

- Bedarfs gesteuert/Cache

    Die Anwendung versucht, Daten aus dem Cache abzurufen, und wenn der Cache nicht über die Daten verfügt (ein "Fehler"), speichert die Anwendung die Daten im Cache, sodass Sie beim nächsten Mal verfügbar ist. Wenn die Anwendung das nächste Mal versucht, dieselben Daten zu erhalten, findet Sie eine Suche im Cache ("Treffer"). Um das Abrufen von zwischengespeicherten Daten, die sich in der Datenbank geändert haben, zu verhindern, können Sie den Cache beim Vornehmen von Änderungen am Datenspeicher für ungültig erklären.
- Push für Hintergrunddaten

    Hintergrunddienste übertragen Daten regelmäßig in den Cache, und die APP ruft immer aus dem Cache ab. Dieser Ansatz eignet sich hervorragend für Datenquellen mit hoher Latenzzeit, die nicht erfordern, dass Sie immer die neuesten Daten zurückgeben.
- Stromunterbrecher

    Die Anwendung kommuniziert normalerweise direkt mit dem persistenten Datenspeicher, aber wenn der permanente Datenspeicher Verfügbarkeits Probleme aufweist, ruft die Anwendung Daten aus dem Cache ab. Daten wurden möglicherweise entweder mithilfe der Cache-oder der Push-Strategie für die Hintergrunddaten in den Cache eingefügt. Dabei handelt es sich um eine Strategie zur Fehlerbehandlung und nicht um eine leistungssteigernde Strategie.

Um die Daten im Cache aktuell zu halten, können Sie Verwandte Cache Einträge löschen, wenn Ihre Anwendung Daten erstellt, aktualisiert oder löscht. Wenn es für Ihre Anwendung in Ordnung ist, Daten zu erhalten, die etwas veraltet sind, können Sie sich auf eine konfigurierbare Ablaufzeit verlassen, um eine Beschränkung für die Anzahl der alten Cache Daten festzulegen.

Sie können den absoluten Ablauf (Zeitraum seit der Erstellung des Cache Elements) oder den gleitenden Ablauf (Zeitspanne seit dem letzten Zugriff auf ein Cache Element) konfigurieren. Der absolute Ablauf wird verwendet, wenn Sie vom Cache Ablauf Mechanismus abhängig sind, um zu verhindern, dass die Daten zu veraltet werden. In der Lösung für die IT-Behebung werden veraltete Cache Elemente manuell entfernt, und der gleitende Ablauf wird verwendet, um die aktuellsten Daten im Cache zu speichern. Unabhängig von der von Ihnen gewählten Ablauf Richtlinie entfernt der Cache automatisch die ältesten Elemente (zuletzt verwendet oder LRU), wenn das Arbeitsspeicher Limit des Caches erreicht ist.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Beispielcode für den Cache, um die IT-APP zu korrigieren

Im folgenden Beispielcode überprüfen wir den Cache zuerst, wenn Sie einen Task zum Beheben von Tasks abrufen. Wenn die Aufgabe im Cache gefunden wird, wird Sie zurückgegeben. Wenn Sie nicht gefunden werden, wird die Datenbank aus der Datenbank entfernt und im Cache gespeichert. Die Änderungen, die Sie vornehmen, um der `FindTaskByIdAsync`-Methode Caching hinzuzufügen, werden hervorgehoben.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Wenn Sie einen Task zum Reparieren der IT aktualisieren oder löschen, müssen Sie die zwischengespeicherte Aufgabe ungültig machen (entfernen). Andernfalls werden von zukünftigen versuchen, diese Aufgabe zu lesen, weiterhin die alten Daten aus dem Cache angezeigt.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Dies sind Beispiele zum Veranschaulichen von einfachem zwischen Speicherungs Code. Caching wurde nicht in das herunterladbare IT-Projekt für das Herunterladen implementiert.

## <a name="azure-caching-services"></a>Azure Caching-Dienste

Azure bietet die folgenden Caching-Dienste: [Azure redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) und [Azure Managed Cache](https://msdn.microsoft.com/library/dn386094.aspx). Azure redis Cache basiert auf dem beliebten [Open Source-redis Cache](http://redis.io/) und ist für die meisten zwischen Speicherungs Szenarien die erste Wahl.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>ASP.NET-Sitzungszustand mithilfe eines Cache Anbieters

Wie im Kapitel " [bewährte Methoden für die Webentwicklung](web-development-best-practices.md)" erwähnt, besteht die bewährte Methode darin, die Verwendung des Sitzungs Zustands zu vermeiden. Wenn für Ihre Anwendung der Sitzungszustand erforderlich ist, besteht die nächste bewährte Methode darin, den standardmäßigen in-Memory-Anbieter zu vermeiden, da dadurch das horizontale hochskalieren (mehrere Instanzen des Webservers) nicht aktiviert wird. Der ASP.NET SQL Server-Sitzungs Zustands Anbieter ermöglicht es einer Site, die auf mehreren Webservern ausgeführt wird, den Sitzungs Status zu verwenden, aber im Vergleich zu einem in-Memory-Anbieter entstehen hohe Latenz Kosten. Die beste Lösung, wenn Sie den Sitzungs Status verwenden müssen, ist die Verwendung eines Cache Anbieters, z. b. der [Sitzungs Zustands Anbieter für Azure Cache](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Summary

Sie haben gesehen, wie die korrigierende IT-APP das Caching implementieren kann, um die Reaktionszeit und die Skalierbarkeit zu verbessern und die APP für Lesevorgänge weiterhin zu aktivieren, wenn die Datenbank nicht verfügbar ist. Im [nächsten Kapitel](queue-centric-work-pattern.md) zeigen wir, wie Sie die Skalierbarkeit weiter verbessern und die APP für Schreibvorgänge weiterhin reaktionsfähig machen.

## <a name="resources"></a>Ressourcen

Weitere Informationen zum Zwischenspeichern finden Sie in den folgenden Ressourcen.

Documentation

- [Azure-Cache](https://msdn.microsoft.com/library/gg278356.aspx). Offizielle MSDN-Dokumentation zum Caching in Azure.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Weitere Informationen finden Sie unter Caching-Anleitungen und Cache Abbild.
- [Failsafe: Leitfaden für robuste cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Weitere Informationen finden Sie im Abschnitt zum Zwischenspeichern.
- [Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Löw. Whitepaper von Mark Simms und Michael thomassy. Weitere Informationen finden Sie im Abschnitt verteilte Zwischenspeicherung.
- [Verteiltes Zwischenspeichern auf dem Pfad zur Skalierbarkeit](https://msdn.microsoft.com/magazine/dd942840.aspx). Ein älterer (2009) MSDN Magazine-Artikel, aber eine deutlich geschriebene Einführung in das verteilte Caching im allgemeinen; geht ausführlicher vor als in den Abschnitten zum Zwischenspeichern der Whitepaper "failsafe" und "bewährte Methoden".

Videos

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Serie von Ulrich Homann, Marc Mercuri und Mark Simms. Bietet eine 400-stufige Ansicht der Architektur von Cloud-apps. Diese Reihe konzentriert sich auf Theorie und Gründe, warum; Weitere Informationen zur Vorgehensweise finden Sie unter Building Big Series by Mark Simms. Weitere Informationen finden Sie in der cachingdiskussion in Episode 3 ab 1:24:14.
- [Building Big: Erkenntnisse von Azure-Kunden Teil I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies erläutert das verteilte Caching ab 46:00. Vergleichbar mit der Failsafe-Reihe, wird jedoch ausführlicher erläutert. Die Präsentation wurde am 31. Oktober 2012, sodass der Cache Dienst von Web-Apps in Azure App Service, der in 2013 eingeführt wurde, nicht abgedeckt wird.

Codebeispiel

- [Grundlagen des clouddiensts in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung, die das verteilte Caching implementiert. Weitere Informationen finden Sie im begleitenden Blogbeitrag [Cloud Service Fundamentals – Caching Basics](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Zurück](transient-fault-handling.md)
> [Weiter](queue-centric-work-pattern.md)
