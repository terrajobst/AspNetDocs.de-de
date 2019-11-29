---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Behandlung vorübergehender Fehler (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Das e-Book zur Entwicklung realer Cloud-apps mit Azure basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Vorgehensweisen erläutert, für die er...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: fc281e3d8f7c9edd4d98b029a67e58113132a8b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583655"
---
# <a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Behandlung vorübergehender Fehler (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es werden 13 Muster und Verfahren erläutert, die Ihnen bei der Entwicklung von Web-Apps für die Cloud helfen können. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Wenn Sie eine echte Cloud-App entwerfen, müssen Sie sich mit der Behandlung temporärer Dienstunterbrechungen beschäftigen. Dieses Problem ist in Cloud-apps eindeutig, da Sie von Netzwerkverbindungen und externen Diensten abhängig sind. Sie können häufig geringfügige Ausfälle erzielen, bei denen es sich in der Regel um eine Selbstreparatur handelt, und wenn Sie nicht darauf vorbereitet sind, Sie auf intelligente Weise zu verarbeiten, führt dies zu einem schlechten Eindruck für Ihre Kunden.

## <a name="causes-of-transient-failures"></a>Ursache vorübergehender Fehler

In der cloudumgebung werden Sie feststellen, dass fehlerhafte und gelöschte Datenbankverbindungen regelmäßig auftreten. Dies liegt teilweise daran, dass Sie im Vergleich zur lokalen Umgebung, in der der Webserver und der Datenbankserver über eine direkte physische Verbindung verfügen, mehr Lasten Ausgleichs Module durchlaufen. Auch wenn Sie von einem mehrinstanzfähigen Dienst abhängig sind, sehen Sie, dass Aufrufe an den Dienst langsamer oder zeitaufwändiger werden, weil eine andere Person, die den Dienst verwendet, Sie stark trifft. In anderen Fällen können Sie der Benutzer sein, der den Dienst zu häufig trifft, und der Dienst schränkt Sie absichtlich ein – verweigert Verbindungen –, um zu verhindern, dass andere Mandanten des Dienstanbieter beeinträchtigt werden.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Verwenden intelligenter Wiederholungs-/Backoff-Logik zum mindern der Auswirkung vorübergehender Fehler

Anstatt eine Ausnahme auszulösen und eine nicht verfügbare oder Fehlerseite für den Kunden anzuzeigen, können Sie Fehler erkennen, die in der Regel vorübergehend sind, und den Vorgang, der zu dem Fehler geführt hat, automatisch wiederholen. In den meisten Fällen wird der Vorgang beim zweiten Versuch erfolgreich ausgeführt, und Sie werden nach dem Fehler wieder hergestellt, ohne dass der Kunde jemals wusste, dass ein Problem aufgetreten ist.

Es gibt mehrere Möglichkeiten, intelligente Wiederholungs Logik zu implementieren.

- Die Microsoft Patterns &amp; Practices-Gruppe verfügt über einen [Anwendungs Block zur Behandlung vorübergehender Fehler](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) , der alles für Sie erledigt, wenn Sie ADO.net für den Zugriff auf SQL-Datenbanken verwenden (nicht über Entity Framework). Sie legen nur eine Richtlinie für Wiederholungen fest – gibt an, wie oft eine Abfrage oder einen Befehl wiederholt werden soll und wie lange zwischen den Versuchen gewartet werden soll – und umschließen den SQL-Code in einem *using* -Block.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH unterstützt auch [Azure in-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) und [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Wenn Sie die Entity Framework verwenden, die normalerweise nicht direkt mit SQL-Verbindungen arbeiten, können Sie dieses Muster und dieses Verfahren nicht verwenden, aber Entity Framework 6 erstellt diese Art von Wiederholungs Logik direkt im Framework. Auf ähnliche Weise wird die Wiederholungs Strategie festgelegt, und EF verwendet diese Strategie immer dann, wenn Sie auf die Datenbank zugreift.

    Um dieses Feature in der APP zum Beheben von IT-apps zu verwenden, müssen Sie lediglich eine Klasse hinzufügen, die von *dbconfiguration* abgeleitet ist, und die Wiederholungs Logik aktivieren.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Bei SQL-Daten Bank Ausnahmen, die das Framework in der Regel als vorübergehende Fehler identifiziert, weist der gezeigte Code EF an, den Vorgang bis zu drei Mal zu wiederholen, wobei eine exponentielle Backoff-Verzögerung zwischen den Wiederholungen und eine maximale Verzögerung von 5 Sekunden auftritt. Das exponentielle Backoff bedeutet, dass nach jedem fehlgeschlagenen Wiederholungsversuch für einen längeren Zeitraum gewartet wird, bevor versucht wird, den Vorgang zu wiederholen. Wenn in einer Zeile drei Versuche fehlschlagen, wird eine Ausnahme ausgelöst. Im folgenden Abschnitt zu Schutz Schaltern wird erläutert, warum Sie exponentielle Backoff und eine begrenzte Anzahl an Wiederholungen wünschen.

    Wenn Sie den Azure Storage-Dienst verwenden, können Sie ähnliche Probleme haben, wie es bei der Behebung der IT-App für BLOB der Fall ist, und die .NET-Speicher Client-API implementiert bereits dieselbe Art von Logik. Sie geben einfach die Wiederholungs Richtlinie an, oder Sie müssen dies nicht einmal tun, wenn Sie mit den Standardeinstellungen zufrieden sind.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Trennschalter

Es gibt mehrere Gründe, warum Sie nicht zu viele Male versuchen möchten, den Zeitraum zu lange zu überschreiten:

- Zu viele Benutzer, die fehlgeschlagene Anforderungen wiederholen, können die Benutzeroberflächen der anderen Benutzer beeinträchtigen. Wenn Millionen von Benutzern alle wiederholten Wiederholungs Anforderungen treffen, können Sie IIS-dispatchwarteschlangen binden und verhindern, dass Ihre APP Anforderungen verarbeitet, die andernfalls erfolgreich verarbeitet werden könnten.
- Wenn jeder Benutzer aufgrund eines Dienst Fehlers einen Wiederholungsversuch durchführt, können so viele Anforderungen in die Warteschlange eingereiht werden, dass der Dienst beim Starten der Wiederherstellung überflutet wird.
- Wenn der Fehler auf eine Drosselung zurückzuführen ist, und es ein Zeitfenster gibt, das der Dienst für die Drosselung verwendet, könnten fortgesetzte Wiederholungen dieses Fenster verschieben und bewirken, dass die Drosselung fortgesetzt wird.
- Möglicherweise haben Sie einen Benutzer, der darauf wartet, dass eine Webseite angezeigt wird. Wenn die Benutzer zu lange warten, ist es möglicherweise sehr lästig, Sie später später erneut zu versuchen.

Beim exponentiellen Backoff werden einige dieser Probleme behoben, indem die Häufigkeit der Wiederholungen, die ein Dienst von der Anwendung erhalten kann, eingeschränkt wird. Sie müssen jedoch auch Trenn *Schalter*haben: Dies bedeutet, dass Ihre APP bei einem bestimmten Wiederholungs Schwellenwert den Wiederholungsversuch beendet und einige andere Aktionen durchführt, z. b. eine der folgenden Aktionen:

- Benutzerdefinierter Fallback. Wenn Sie keinen Aktienkurs von Reuters erhalten können, können Sie ihn möglicherweise von Bloomberg erhalten. Wenn Sie keine Daten aus der Datenbank erhalten können, können Sie Sie möglicherweise aus dem Cache erhalten.
- Nicht unbeaufsichtigt. Wenn das, was Sie von einem Dienst benötigen, nicht alles-oder-nichts für Ihre APP ist, geben Sie einfach NULL zurück, wenn Sie die Daten nicht erhalten können. Wenn Sie eine Korrektur-IT-Aufgabe anzeigen und der BLOB-Dienst nicht antwortet, können Sie die Aufgaben Details ohne das Bild anzeigen.
- Schneller Fehler. Fehler beim Benutzer, um zu verhindern, dass der Dienst mit Wiederholungs Anforderungen überflutet wird, was zu einer Dienst Unterbrechung für andere Benutzer oder zum Erweitern eines Drosselungs Fensters führen könnte. Sie können eine benutzerfreundliche Meldung "später erneut versuchen" anzeigen.

Es gibt keine wiederholen Sie-Richtlinie mit einer einzelnen Größe. Sie können mehrmals versuchen, den Vorgang zu wiederholen und in einem asynchronen Hintergrund Arbeitsprozess länger zu warten, als in einer synchronen Web-App, in der ein Benutzer auf eine Antwort wartet. Sie können länger zwischen den Wiederholungen für einen relationalen Datenbankdienst warten, als dies für einen Cache Dienst der Fall wäre. Hier finden Sie einige Beispiele für empfohlene Wiederholungs Richtlinien, um Ihnen eine Vorstellung davon zu geben, wie sich die Zahlen unterscheiden können. ("Fast First" bedeutet, dass vor dem ersten Wiederholungsversuch keine Verzögerung vorliegt.

![Beispiele für Wiederholungs Richtlinien](transient-fault-handling/_static/image1.png)

Eine Anleitung für die Wiederholungs Richtlinie für SQL-Datenbank finden Sie unter [beheben vorübergehender Fehler und Verbindungsfehler in SQL-Datenbank](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Summary

Eine Wiederholungs-/Backoff-Strategie kann dazu beitragen, temporäre Fehler in den meisten Fällen für den Kunden unsichtbar zu machen, und Microsoft stellt Frameworks zur Verfügung, mit denen Sie Ihre Arbeit bei der Implementierung einer Strategie minimieren können, unabhängig davon, ob Sie ADO.net, Entity Framework oder den Azure Storage-Dienst verwenden.

Im [nächsten Kapitel](distributed-caching.md)wird erläutert, wie Sie die Leistung und Zuverlässigkeit mithilfe von verteiltem Caching verbessern.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter:

Documentation

- [Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael thomassy. Vergleichbar mit der Failsafe-Reihe, wird jedoch ausführlicher erläutert. Weitere Informationen finden Sie im Abschnitt Telemetrie und Diagnose.
- [Failsafe: Leitfaden für robuste cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Webseiten Version der Failsafe-Videoserie.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Siehe Wiederholungsmuster, Scheduler-Agent-Supervisor-Muster.
- [Fehlertoleranz in Azure SQL-Datenbank](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Blog Beitrag von Tony Petrossian.
- [Resilienz/Wiederholungs Logik für Entity Framework Verbindung](https://msdn.microsoft.com/data/dn456835). Verwenden und Anpassen des Features zur Behandlung vorübergehender Fehler von Entity Framework 6.
- [Verbindungsresilienz und Befehls Abfang Funktion mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Viertens in einer neun teiligen tutorialreihe wird gezeigt, wie Sie das Feature zur Resilienz von EF 6 in SQL-Datenbank einrichten.

Videos

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Serie von Ulrich Homann, Marc Mercuri und Mark Simms. Stellt allgemeine Konzepte und architektonische Prinzipien in einer sehr zugänglichen und interessanten Weise vor, wobei Storys von der Benutzerfreundlichkeit von Microsoft Customer Advisory Team (CAT) mit tatsächlichen Kunden erstellt werden. Weitere Informationen finden Sie in der Erörterung von Schutz Schaltern in Episode 3 ab 40:55.
- [Building Big: Erkenntnisse von Azure-Kunden (Teil II](https://channel9.msdn.com/Events/Build/2012/3-030)). Mark Simms spricht über das Entwerfen von Fehlern, die Behandlung vorübergehender Fehler und das Instrumentieren von allem.

Codebeispiel

- [Grundlagen des clouddiensts in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung, die vom Microsoft Azure Customer Advisory Team erstellt wurde und veranschaulicht, wie der [Enterprise Library-Block zur Behandlung vorübergehender Fehler](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH) verwendet wird. Weitere Informationen finden Sie unter [Grundlagen der Datenzugriffs Schicht für Clouddienste – Behandlung vorübergehender Fehler](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH wird für den Datenbankzugriff mithilfe von ADO.net direkt (ohne Entity Framework) empfohlen.

> [!div class="step-by-step"]
> [Zurück](monitoring-and-telemetry.md)
> [Weiter](distributed-caching.md)
