---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Strategien für die Daten Partitionierung (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: b8c901ec30b6d37237f80100a2978350ac389b7a
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519166"
---
# <a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Strategien für die Daten Partitionierung (entwickeln realer Cloud-apps mit Azure)

durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu den Reihen finden Sie [im ersten Kapitel](introduction.md).

Früher haben wir gesehen, wie einfach es ist, die webeebene einer cloudanwendung durch Hinzufügen und Entfernen von Webservern zu skalieren. Wenn Sie jedoch alle den gleichen Datenspeicher erreichen, wechselt der Engpass Ihrer Anwendung vom Front-End zum Back-End, und die Datenebene ist am schwierigsten zu skalieren. In diesem Kapitel wird erläutert, wie Sie Ihre Datenebene skalierbar machen, indem Sie Daten in mehreren relationalen Datenbanken partitionieren oder den relationalen Daten Bank Speicher mit anderen Datenspeicher Optionen kombinieren.

Das Einrichten eines Partitionierungs Schemas erfolgt im Voraus am besten aus dem oben erwähnten Grund: Es ist sehr schwierig, Ihre Datenspeicher Strategie zu ändern, nachdem eine app in der Produktionsumgebung eingesetzt wurde. Wenn Sie sich mit unterschiedlichen Ansätzen Gedanken machen, können Sie einen "Twitter-Moment" vermeiden, wenn Ihre APP über einen längeren Zeitraum abstürzt oder ausfällt, während Sie den Daten-und Datenzugriffs Code Ihrer APP neu organisieren.

## <a name="the-three-vs-of-data-storage"></a>Die drei vs der Datenspeicherung

Um zu ermitteln, ob Sie eine Partitionierungs Strategie benötigen und was Sie sein sollte, sollten Sie drei Fragen zu Ihren Daten berücksichtigen:

- Volume – wie viele Daten werden letztendlich gespeichert? Ein paar Gigabytes? Ein paar hundert Gigabyte? Terabyte? Petabytes?
- Geschwindigkeit – wie hoch ist die Rate, mit der die Daten vergrößert werden? Handelt es sich um eine interne APP, die nicht viele Daten erzeugt? Eine externe APP, in die Kunden Bilder und Videos hochladen?
- Vielfalt – welche Art von Daten wird gespeichert? Relationale Bilder, Schlüssel-Wert-Paare, Diagramme für soziale Netzwerke?

Wenn Sie davon ausgehen, dass Sie viel Volumen, Geschwindigkeit oder Vielfalt haben, müssen Sie sorgfältig überlegen, welche Art von Partitionierungsschema Ihre APP am besten effizient und effektiv skalieren kann, wenn Sie wächst, und sicherstellen, dass keine Engpässe entstehen.

Es gibt im Wesentlichen drei Ansätze für die Partitionierung:

- Vertikale Partitionierung
- Horizontale Partitionierung
- Hybrid Partitionierung

## <a name="vertical-partitioning"></a>Vertikale Partitionierung

Die vertikale Portierung ähnelt dem Aufteilen einer Tabelle nach Spalten: ein Satz von Spalten wird in einen Datenspeicher umgewandelt, und ein anderer Spalten Satz wird in einen anderen Datenspeicher übertragen.

Nehmen wir beispielsweise an, dass meine APP Daten über Personen speichert, einschließlich Images:

![Datentabelle](data-partitioning-strategies/_static/image1.png)

Wenn Sie diese Daten als Tabelle darstellen und die unterschiedlichen Arten von Daten betrachten, können Sie sehen, dass die drei Spalten links Zeichen folgen Daten aufweisen, die effizient von einer relationalen Datenbank gespeichert werden können, während die beiden Spalten auf der rechten Seite im wesentlichen Byte Arrays sind, die c aus Bilddateien. Es ist möglich, Abbild Datei Daten in einer relationalen Datenbank zu speichern, und viele Personen müssen dies tun, da Sie die Daten nicht im Dateisystem speichern möchten. Sie verfügen möglicherweise nicht über ein Dateisystem, das die erforderlichen Datenmengen speichern kann, oder Sie möchten möglicherweise kein separates Sicherungs-und Wiederherstellungs System verwalten. Diese Vorgehensweise eignet sich gut für lokale Datenbanken und für kleine Datenmengen in clouddatenbanken. In der lokalen Umgebung ist es möglicherweise einfacher, dass der DBA alles erledigt.

In einer clouddatenbank ist der Speicher jedoch relativ aufwendig, und eine große Anzahl von Images könnte dazu führen, dass die Größe der Datenbank über die Grenzwerte hinausgeht, auf denen Sie effizient agieren kann. Sie können diese Probleme beheben, indem Sie die Daten vertikal partitionieren. Dies bedeutet, dass Sie den am besten geeigneten Datenspeicher für jede Spalte in der Datentabelle auswählen. Das beste an diesem Beispiel ist, die Zeichen folgen Daten in einer relationalen Datenbank und die Images in BLOB Storage zu platzieren.

![Datentabelle vertikal partitioniert](data-partitioning-strategies/_static/image2.png)

Das Speichern von Bildern im BLOB-Speicher und nicht in einer Datenbank ist in der Cloud praktischer als in einer lokalen Umgebung, da Sie sich keine Gedanken über das Einrichten von Dateiservern oder das Verwalten von Sicherung und Wiederherstellung von Daten machen müssen, die außerhalb der relationalen Datenbank gespeichert sind: alle Dies wird automatisch vom BLOB-Speicherdienst verarbeitet.

Dies ist der Partitionierungs Ansatz, den wir in der Lösung zum Beheben von IT-apps implementiert haben, und wir betrachten den Code dafür im [BLOB Storage-Kapitel](unstructured-blob-storage.md). Ohne dieses Partitionierungsschema und der Annahme, dass eine durchschnittliche Abbild Größe von 3 Megabyte besteht, wäre die Korrektur der IT-app nur in der Lage, ungefähr 40.000 Tasks zu speichern, bevor die maximale Datenbankgröße von 150 Gigabyte erreicht wird. Nach dem Entfernen der Images kann die Datenbank zehnmal so viele Aufgaben speichern. Sie können viel länger gehen, bevor Sie sich Gedanken über die Implementierung eines horizontalen Partitionierungs Schemas machen müssen. Und wenn die APP skaliert wird, wachsen ihre Kosten langsamer, da der Großteil der Speicheranforderungen sehr preiswert in den BLOB-Speicher geht.

## <a name="horizontal-partitioning-sharding"></a>Horizontale Partitionierung (Sharding)

Die horizontale Portierung ähnelt dem Aufteilen einer Tabelle nach Zeilen: ein Satz von Zeilen wird in einen Datenspeicher umgewandelt, und ein anderer Satz von Zeilen wird in einen anderen Datenspeicher übertragen.

Eine andere Möglichkeit besteht darin, unterschiedliche Bereiche von Kundennamen in verschiedenen Datenbanken zu speichern.

![Datentabelle horizontal partitioniert](data-partitioning-strategies/_static/image3.png)

Sie sollten mit dem shardingschema sehr vorsichtig sein, um sicherzustellen, dass die Daten gleichmäßig verteilt werden, um Hotspots zu vermeiden. Dieses einfache Beispiel mit dem ersten Buchstaben des Nachnamen erfüllt diese Anforderung nicht, da viele Personen Nachnamen haben, die mit bestimmten häufigen Buchstaben beginnen. Sie würden die Einschränkungen der Tabellengröße früher als erwartet erreichen, da einige Datenbanken sehr groß werden würden, wenn die meisten Datenbanken sehr groß bleiben würden.

Eine Seite der horizontalen Partitionierung ist, dass es möglicherweise schwierig ist, Abfragen für alle Daten durchzuführen. In diesem Beispiel muss eine Abfrage von bis zu 26 unterschiedlichen Datenbanken gezeichnet werden, um alle von der APP gespeicherten Daten zu erhalten.

## <a name="hybrid-partitioning"></a>Hybrid Partitionierung

Sie können die vertikale und horizontale Partitionierung kombinieren. Beispielsweise können Sie in den Beispiel Daten die Images in BLOB Storage speichern und die Zeichen folgen Daten horizontal partitionieren.

![Datentabelle Hybrid partitioniert](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partitionierung einer Produktionsanwendung

Konzeptionell lässt sich leicht erkennen, wie ein Partitionierungsschema funktioniert, aber jedes Partitionierungsschema erhöht die Komplexität des Codes und führt viele neue Komplikationen ein, die Sie behandeln müssen. Wenn Sie Bilder in den BLOB-Speicher verschieben, was tun Sie, wenn der Speicherdienst herunterläuft? Wie behandle ich die BLOB-Sicherheit? Was geschieht, wenn die Datenbank und der BLOB-Speicher nicht synchron sind? Wenn Sie Sharding ausführen, wie werden Abfragen in allen Datenbanken behandelt?

Die Komplikationen können so lange verwaltet werden, wie Sie planen, bevor Sie in die Produktion gehen. Viele Personen, die dies nicht getan haben, hatten Sie später nicht mehr. Im Durchschnitt erhält das Cat-Team (Customer Advisory Team) in etwa einmal pro Monat eine Telefonanruf-Telefonnummer von Kunden, deren apps wirklich sehr groß sind und diese Planung nicht durchlaufen haben. Und Sie sagen etwas wie: "Help! Ich lege alles in einem einzigen Datenspeicher ab, und in 45 Tagen geht es in diesem Bereich nicht mehr. Wenn Sie viel Geschäftslogik in den Zugriff auf Ihren Datenspeicher integriert haben und Sie über Kunden verfügen, die Ihre APP verwenden, gibt es keine gute Zeit für einen Tag, während Sie migrieren. Wir arbeiten am Ende daran, dass der Kunde die Daten im Handumdrehen und ohne Zeit in Betrieb nehmen muss. Das ist sehr spannend und sehr beängstigend, und es ist nicht alles, was Sie tun möchten, wenn Sie es vermeiden können. Wenn Sie dies im Vordergrund tun und in Ihre APP integrieren, wird das Leben erheblich vereinfacht, wenn die APP später wächst.

## <a name="summary"></a>Summary

Ein effektives Partitionierungsschema kann es ihrer Cloud-App ermöglichen, Daten in der Cloud ohne Engpässe zu skalieren. Und Sie müssen nicht für riesige oder umfassende Infrastrukturen bezahlen, wenn Sie die app in einem lokalen Rechenzentrum ausführen. In der Cloud können Sie die Kapazität inkrementell hinzufügen, wenn Sie Sie benötigen, und Sie zahlen nur für die Verwendung bei der Verwendung.

Im [nächsten Kapitel](unstructured-blob-storage.md) wird erläutert, wie die Korrektur der IT-APP die vertikale Partitionierung implementiert, indem Bilder in BLOB Storage gespeichert werden.

## <a name="resources"></a>Ressourcen

Weitere Informationen zu Partitionierungs Strategien finden Sie in den folgenden Ressourcen.

Dokumentation:

- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Windows Azure-Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael thomassy.
- [Microsoft Patterns and Practices: Cloud-Entwurfsmuster](https://msdn.microsoft.com/library/dn568099.aspx). Siehe Leitfaden zur Daten Partitionierung, Sharding-Muster.

Videos:

- [Failsafe: aufbauen skalierbarer, robuster Cloud Services](https://channel9.msdn.com/Series/FailSafe). Neun teilige Serie von Ulrich Homann, Marc Mercuri und Mark Simms. Stellt allgemeine Konzepte und architektonische Prinzipien in einer sehr zugänglichen und interessanten Weise vor, wobei Storys von der Benutzerfreundlichkeit von Microsoft Customer Advisory Team (CAT) mit tatsächlichen Kunden erstellt werden. Weitere Informationen finden Sie unter Partitionierungs Diskussion in Episode 7.
- [Building Big: Erkenntnisse von Windows Azure-Kunden Teil I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms erläutert Partitionierungs Schemas, shardingstrategien, die Implementierung von Sharding und SQL-Daten Bank Verbunde ab 19:49. Vergleichbar mit der Failsafe-Reihe, wird jedoch ausführlicher erläutert.

Beispielcode:

- [Grundlagen des clouddiensts in Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung, die eine Sharding-Datenbank enthält. Eine Beschreibung des implementierten shardingschemas finden Sie unter [dal – Sharding of RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) im Windows Azure-Blog.

> [!div class="step-by-step"]
> [Zurück](data-storage-options.md)
> [Weiter](unstructured-blob-storage.md)
