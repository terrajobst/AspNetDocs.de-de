---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Entwerfen, um Fehler zu überstehen (erstellen realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 348232af531b5d53dc3cb46d6d2c7931d95a572d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500763"
---
# <a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Entwerfen, um Fehler zu überstehen (erstellen realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Eine der Dinge, die Sie berücksichtigen müssen, wenn Sie einen beliebigen Anwendungstyp erstellen, aber vor allem einen, der in der Cloud ausgeführt wird, in der viele Personen ihn verwenden werden, ist das Entwerfen der APP, damit Fehler ordnungsgemäß behandelt werden können, und die Bereitstellung des Werts weiterhin so viel wie man. Bei ausreichender Zeit geht es in jeder Umgebung oder jedem beliebigen Softwaresystem zu Fehlern. Wie Ihre APP diese Situationen behandelt, hängt davon ab, wie stark Sie Ihre Kunden erhalten und wie viel Zeit Sie für die Analyse und Behebung von Problemen aufwenden müssen.

## <a name="types-of-failures"></a>Fehlertypen

Es gibt zwei grundlegende Kategorien von Fehlern, die Sie unterschiedlich behandeln sollten:

- Vorübergehende Fehler bei der Selbstreparatur, wie z. b. vorübergehende Probleme mit der Netzwerk Konnektivität.
- Andauernde Ausfälle, für die ein Eingriff erforderlich ist.

Bei vorübergehenden Fehlern können Sie eine Wiederholungs Richtlinie implementieren, um sicherzustellen, dass die app in den meisten Fällen schnell und automatisch wieder hergestellt wird. Ihre Kunden bemerken möglicherweise eine etwas längere Antwortzeit, aber andernfalls werden Sie nicht beeinträchtigt. Wir zeigen einige Möglichkeiten, diese Fehler im [Kapitel zur Behandlung vorübergehender Fehler](transient-fault-handling.md)zu behandeln.

Bei dauerhaften Fehlern können Sie Überwachungs-und Protokollierungsfunktionen implementieren, die Sie umgehend benachrichtigen, wenn Probleme auftreten und die Fehlerursachen Analyse erleichtert. Wir zeigen Ihnen einige Möglichkeiten, wie Sie im [Kapitel Überwachung und Telemetrie](monitoring-and-telemetry.md)auf diese Arten von Fehlern achten können.

## <a name="failure-scope"></a>Fehlerbereich

Außerdem müssen Sie sich Gedanken über den Fehlerbereich machen – ob ein einzelner Computer betroffen ist, ein ganzer Dienst, z. b. SQL-Datenbank oder Speicher oder eine gesamte Region.

![Fehlerbereich](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Computerfehler

In Azure wird ein fehlerhafter Server automatisch durch einen neuen ersetzt, und eine gut entworfene Cloud-APP wird von dieser Art von Fehler automatisch und schnell wieder hergestellt. Früher haben wir die Vorteile der Skalierbarkeit einer Zustands losen webeebene hervorgehoben, und die einfache Wiederherstellung von einem fehlerhaften Server ist ein weiterer Vorteil der Zustands losigkeit. Die einfache Wiederherstellung ist auch einer der Vorteile von Platform-as-a-Service-Features (PAS), wie z. b. SQL-Datenbank und Azure App Service-Web-Apps. Hardware Fehler sind selten, aber wenn Sie auftreten, behandeln diese Dienste Sie automatisch. Sie müssen nicht einmal Code schreiben, um Computerfehler zu behandeln, wenn Sie einen dieser Dienste verwenden.

### <a name="service-failures"></a>Dienst Fehler

Cloud-Apps verwenden in der Regel mehrere Dienste. Beispielsweise werden die Fehlerbehebung für die IT-App verwendet, um den SQL-Datenbankdienst, den Speicherdienst und die Web-App für Azure App Service bereitzustellen. Wie wird Ihre APP ausgeführt, wenn einer der Dienste, von denen Sie abhängen, fehlschlägt? Bei einigen Dienst Fehlern ist es möglicherweise am besten, wenn Sie versuchen, es später erneut zu versuchen. In vielen Szenarien können Sie aber auch besser Vorgehen. Wenn der Back-End-Datenspeicher nicht verfügbar ist, können Sie beispielsweise Benutzereingaben akzeptieren, "Ihre Anforderung wurde empfangen" anzeigen und die Eingabe an einem beliebigen Ort temporär speichern. Wenn der Dienst, den Sie benötigen, wieder funktionsfähig ist, können Sie die Eingabe abrufen und verarbeiten.

Das Kapitel für [Warteschlangen zentrierte Arbeitsmuster](queue-centric-work-pattern.md) zeigt eine Möglichkeit, dieses Szenario zu behandeln. Die Korrektur der IT-App speichert Aufgaben in der SQL-Datenbank, aber Sie muss nicht beendet werden, wenn die SQL-Datenbank nicht ausgeführt wird. In diesem Kapitel wird erläutert, wie Benutzereingaben für eine Aufgabe in einer Warteschlange gespeichert werden und wie ein Arbeitsprozess verwendet wird, um die Warteschlange zu lesen und die Aufgabe zu aktualisieren. Wenn SQL nicht ausgeführt wird, ist die Möglichkeit zum Erstellen von IT-Aufgaben nicht betroffen. der Arbeitsprozess kann neue Tasks warten und verarbeiten, wenn die SQL-Datenbank verfügbar ist.

### <a name="region-failures"></a>Regions Fehler

Alle Regionen können fehlschlagen. Eine Naturkatastrophe könnte ein Rechenzentrum zerstören, es könnte durch einen Meteor vereinfacht werden, die trunk Linie in das Rechenzentrum könnte von einem Landwirt, der eine Kuh mit einer Backhoe hat, usw. abgeschnitten werden. Was tun Sie, wenn Ihre APP im betroffenen Rechenzentrum gehostet wird? Sie können Ihre APP in Azure so einrichten, dass Sie in mehreren Regionen gleichzeitig ausgeführt wird, sodass Sie bei einem Notfall in einer anderen Region weiter ausgeführt werden. Solche Fehler sind sehr selten, und die meisten apps durchlaufen nicht die Unterbrechungen, die notwendig sind, um einen ununterbrochenen Dienst durch Ausfälle dieser Art sicherzustellen. Informationen dazu, wie Sie Ihre APP auch über einen Regions Ausfall verfügbar halten, finden Sie im Abschnitt "Ressourcen" am Ende des Kapitels.

Ein Ziel von Azure besteht darin, die Behandlung all dieser Arten von Fehlern zu vereinfachen, und Sie sehen einige Beispiele dafür, wie wir dies in den folgenden Kapiteln tun.

## <a name="slas"></a>SLAs

Häufig werden die Vereinbarungen zum Service Level (Service Level Agreements, SLAs) in der cloudumgebung angezeigt. Im Grunde sind dies Zusagen, die Unternehmen über die Zuverlässigkeit Ihres Dienstanbieter treffen. Eine 99,9%-SLA bedeutet, dass der Dienst 99,9% der Zeit ordnungsgemäß funktioniert. Das ist ein ziemlich typischer Wert für eine SLA und klingt wie eine sehr hohe Zahl, aber Sie werden vielleicht nicht feststellen, wie viel weniger als 1 bis 1% beträgt. Im folgenden finden Sie eine Tabelle, die zeigt, wie viele Ausfallzeiten verschiedene SLA-Prozentsätze auf über ein Jahr, einen Monat und eine Woche betragen.

![SLA-Tabelle](design-to-survive-failures/_static/image2.png)

Eine 99,9%-SLA bedeutet also, dass der Dienst 8,76 Stunden oder 43,2 Minuten im Monat herunter eingestellt werden kann. Das ist umso mehr Zeit als die meisten Menschen. Als Entwickler möchten Sie also wissen, dass eine bestimmte Zeitspanne möglich ist, und Sie können es auf eine ordnungsgemäße Weise verarbeiten. An einem bestimmten Punkt wird Ihre APP verwendet, und ein Dienst wird Herunterfahren, und Sie möchten die negativen Auswirkungen der Kunden minimieren.

Sie sollten sich über eine SLA informieren, auf welche Zeitrahmen Sie verweist: wird die Uhr jede Woche, jeden Monat oder jedes Jahr zurückgesetzt? In Azure wird die Uhr jeden Monat zurückgesetzt. Dies ist für Sie besser als eine jährliche SLA, da eine jährliche SLA durch eine Reihe von guten Monaten schlechte Monate verbergen könnte.

Natürlich wollen wir immer besser Vorgehen als die SLA. in der Regel sind Sie wesentlich kleiner als das. Die Zusage besteht darin, dass Sie, wenn wir immer länger ausfallen, als die maximale Zeitdauer, die Sie Geld zurückstellen können. Die Menge des Geldes, das Sie zurückerhalten, wäre wahrscheinlich nicht vollständig für die geschäftlichen Auswirkungen der überschreit Zeit ausgeglichen, aber dieser Aspekt der SLA fungiert als Erzwingungs Richtlinie und lässt Sie wissen, dass wir Sie sehr ernst nehmen.

### <a name="composite-slas"></a>Zusammengesetzte SLAs

Wenn Sie SLAs betrachten, ist es wichtig, sich mit der Verwendung mehrerer Dienste in einer APP vertraut zu machen, wobei jeder Dienst über eine separate SLA verfügt. Beispielsweise verwendet die Behebung, die die IT-App Azure App Service Web-Apps, Azure Storage und SQL-Datenbank verwendet. Im folgenden finden Sie die SLA-Nummern ab dem Datum, an dem das e-Book im Dezember 2013 geschrieben wurde:

![SLA-Website, Storage, SQL-Datenbank](design-to-survive-failures/_static/image3.png)

Wie hoch ist die maximale Zeitdauer, die Sie für die App basierend auf diesen SLAs des diensslas erwarten? Sie denken möglicherweise, dass die Abfallzeit gleich dem schlechtesten SLA-Prozentsatz oder 99,9% in diesem Fall wäre. Das würde dann der Fall sein, wenn alle drei Dienste immer gleichzeitig fehlgeschlagen sind, aber das ist nicht unbedingt tatsächlich der Fall. Jeder Dienst kann zu unterschiedlichen Zeitpunkten unabhängig ausfallen, sodass Sie die zusammengesetzte SLA berechnen müssen, indem Sie die einzelnen SLA-Nummern multiplizieren.

![Zusammengesetzte Vereinbarung zum Servicelevel](design-to-survive-failures/_static/image4.png)

Ihre APP könnte also nicht nur 43,2 Minuten im Monat, sondern dreimal so hoch sein, 108 Minuten pro Monat – und liegt weiterhin innerhalb der Azure-SLA-Limits.

Dieses Problem gilt nicht für Azure. Wir stellen tatsächlich die besten Cloud-SLAs für jeden verfügbaren clouddienst bereit, und Sie haben ähnliche Probleme, wenn Sie die Cloud-Dienste eines Anbieters nutzen. Dies ist die Wichtigkeit, mit der Sie sich Gedanken darüber machen können, wie Sie Ihre APP so entwerfen können, dass die unvermeidlichen Dienst Fehler ordnungsgemäß verarbeitet werden, da Sie häufig genug vorkommen, um Ihre Kunden oder Benutzer zu beeinträchtigen.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Cloud-SLAs im Vergleich zur Betriebszeit des Unternehmens

Manchmal sagen wir: "in meiner Unternehmens-App treten diese Probleme nie auf." Wenn Sie feststellen, wie viel Zeit in einem Monat tatsächlich liegt, sagen Sie in der Regel, dass es gelegentlich passiert. Und wenn Sie Fragen, wie oft Sie feststellen, dass Sie einen neuen Server sichern oder installieren oder Software aktualisieren müssen. Natürlich zählt dies als Zeitüberschreitungen. Die meisten Unternehmens-apps, sofern Sie nicht besonders Unternehmens kritisch sind, sind nicht länger als die von unseren Service-SLAs zulässige Zeitspanne. Wenn es sich jedoch um Ihren Server und ihre Infrastruktur handelt und Sie dafür verantwortlich sind und die Kontrolle darüber haben, sind Sie eher weniger Angst bei Ausfallzeiten. In einer cloudumgebung sind Sie von einer anderen Person abhängig, und Sie wissen nicht, was passiert, sodass Sie mehr darüber besorgt werden.

Wenn ein Unternehmen eine höhere Betriebszeit in Prozent erreicht, als Sie aus einer Cloud-SLA kommen, wird dies erreicht, indem die Hardware viel mehr Geld ausgibt. Ein Cloud-Dienst könnte dies tun, müsste jedoch für seine Dienste viel mehr berechnen. Stattdessen profitieren Sie von einem Kosten effektiven Dienst und entwerfen Ihre Software so, dass die unvermeidlichen Ausfälle zu einer minimalen Unterbrechung der Kunden führen. Ihr Job als Cloud-App-Designer ist nicht so viel, um einen Fehler zu vermeiden, um eine Katastrophe zu vermeiden, und Sie erreichen dies, indem Sie sich auf die Software konzentrieren, nicht auf die Hardware. Während Unternehmens-apps eine Maximierung der durchschnittlichen Zeit zwischen Ausfällen anstreben, bemühen sich Cloud-apps, die durchschnittliche Zeit für die Wiederherstellung zu verringern

### <a name="not-all-cloud-services-have-slas"></a>Nicht alle Clouddienste haben SLAs

Beachten Sie auch, dass nicht jeder clouddienst auch über eine SLA verfügt. Wenn Ihre APP von einem Dienst ohne Zeit Garantie abhängig ist, können Sie viel länger ausfallen, als Sie sich vorstellen können. Wenn Sie z. b. die Anmeldung an Ihrer Website mithilfe eines sozialen Anbieters wie Facebook oder Twitter aktivieren, wenden Sie sich an den Dienstanbieter, um herauszufinden, ob eine SLA vorhanden ist, und Sie können feststellen, dass es keine SLA gibt. Wenn der Authentifizierungsdienst jedoch ausfällt oder die Menge der von Ihnen ausgerichteten Anforderungen nicht unterstützt, werden Ihre Kunden von ihrer App gesperrt. Sie können für Tage oder länger ausfallen. Die Ersteller einer neuen App haben hundert Millionen Downloads erwartet und eine Abhängigkeit von der Facebook-Authentifizierung getroffen – aber nicht mit Facebook kommuniziert, bevor Sie leben, und es wurde zu spät erkannt, dass es für diesen Dienst keine SLA gab.

### <a name="not-all-downtime-counts-toward-slas"></a>Nicht alle Ausfallzeiten werden in Bezug auf SLAs gezählt.

Einige Clouddienste können den Dienst absichtlich ablehnen, wenn Sie von Ihrer APP verwendet werden. Dies wird als *Drosselung*bezeichnet. Wenn ein Dienst über eine SLA verfügt, sollte er die Bedingungen angeben, unter denen Sie möglicherweise gedrosselt werden, und Ihr App-Entwurf sollte diese Bedingungen vermeiden und entsprechend auf die Drosselung reagieren, wenn dies geschieht. Wenn beispielsweise bei Anforderungen an einen Dienst ein Fehler auftritt, wenn Sie eine bestimmte Anzahl pro Sekunde überschreiten, sollten Sie sicherstellen, dass automatische Wiederholungen nicht so schnell erfolgen, dass die Drosselung fortgesetzt wird. Im [Kapitel zur Behandlung vorübergehender Fehler](transient-fault-handling.md)erfahren Sie mehr über die Drosselung.

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel wurde versucht, zu erkennen, warum eine echte Cloud-app entwickelt werden muss, um Fehler ordnungsgemäß zu überstehen. Beginnend mit dem [nächsten Kapitel](monitoring-and-telemetry.md)werden die verbleibenden Muster in dieser Reihe ausführlicher über einige Strategien erläutert, die Sie hierfür verwenden können:

- Sie verfügen über eine gute [Überwachung und Telemetrie](monitoring-and-telemetry.md), sodass Sie sich schnell über Fehler informieren können, die eingreifen müssen, und dass Sie über ausreichende Informationen verfügen, um diese zu beheben.
- [Behandeln vorübergehender Fehler](transient-fault-handling.md) durch Implementieren intelligenter [Wiederholungs Logik,](transient-fault-handling.md#circuitbreakers) sodass Ihre APP automatisch wieder hergestellt wird, wenn Sie nicht möglich ist.
- Verwenden Sie [verteilte](distributed-caching.md) Zwischenspeicherung, um Durchsatz, Latenz und Verbindungsprobleme beim Datenbankzugriff zu minimieren.
- Implementieren Sie eine lose Kopplung über das [Warteschlangen zentrierte Arbeitsmuster](queue-centric-work-pattern.md), sodass das App-Front-End weiterhin funktionieren kann, wenn das Back-End nicht ausgeführt wird.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in späteren Kapiteln dieses e-Books und in den folgenden Ressourcen.

Dokumentation:

- [Failsafe: Leitfaden für robuste cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Webseiten Version der Failsafe-Videoserie.
- [Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael thomassy.
- [Technische Anleitung zur Geschäftskontinuität in Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Whitepaper von Patrick Wickline und Jason Roth.
- Notfall [Wiederherstellung und Hochverfügbarkeit für Azure-Anwendungen](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Whitepaper von Michael McKeown, Hanu Kommalapati und Jason Roth.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Weitere Informationen finden Sie unter Leitfaden zur Bereitstellung von mehreren Rechenzentren, Trennschalter
- [Azure-Support: Vereinbarungen zum Service Level](https://azure.microsoft.com/support/legal/sla/).
- [Geschäftskontinuität in Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentation zu den Features für Hochverfügbarkeit und Notfall Wiederherstellung in SQL-Datenbank.
- [Hohe Verfügbarkeit und Notfall Wiederherstellung für SQL Server in Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videos:

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Serie von Ulrich Homann, Marc Mercuri und Mark Simms. Stellt allgemeine Konzepte und architektonische Prinzipien in einer sehr zugänglichen und interessanten Weise vor, wobei Storys von der Benutzerfreundlichkeit von Microsoft Customer Advisory Team (CAT) mit tatsächlichen Kunden erstellt werden. In den Vorfällen 1 und 8 werden die Gründe für das Entwerfen von Cloud-Apps für das überstehen von Fehlern ausführlich aufgeführt. Weitere Informationen finden Sie in der nachfolgenden Erörterung der Drosselung in Folge 2 ab 49:57, der Erörterung von Fehler-und Fehlermodi in Folge 2 ab 56:05 und der Erörterung von Schutz Schaltern in Episode 3 ab 40:55.
- [Building Big: Erkenntnisse von Azure-Kunden (Teil II](https://channel9.msdn.com/Events/Build/2012/3-030)). Mark Simms spricht über das Entwerfen von Fehlern und Instrumentieren von allem. Vergleichbar mit der Failsafe-Reihe, wird jedoch ausführlicher erläutert.

> [!div class="step-by-step"]
> [Zurück](unstructured-blob-storage.md)
> [Weiter](monitoring-and-telemetry.md)
