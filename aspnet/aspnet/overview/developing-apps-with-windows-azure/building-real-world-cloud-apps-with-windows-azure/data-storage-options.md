---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Datenspeicher Optionen (entwickeln realer Cloud-apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 9357ed5aef39bed501cdac9ac26d46c884d4fae0
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457179"
---
# <a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Datenspeicher Optionen (entwickeln realer Cloud-apps mit Azure)

von [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des IT-Projekts](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [herunterladen des E-Books](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Das e-Book zur Entwicklung **realer Cloud-apps mit Azure** basiert auf einer Präsentation von Scott Guthrie. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Informationen zum e-Book finden Sie [im ersten Kapitel](introduction.md).

Die meisten Benutzer werden für relationale Datenbanken verwendet und neigen dazu, beim Entwerfen einer Cloud-App andere Datenspeicher Optionen zu übersehen. Das Ergebnis kann eine suboptimale Leistung, hohe Kosten oder eine schlechtere Leistung sein, weil nicht relationale [nosql](http://en.wikipedia.org/wiki/NoSQL) -Datenbanken einige Aufgaben effizienter verarbeiten können als relationale Datenbanken. Wenn Kunden uns bei der Behebung eines wichtigen Daten Speicherungs Problems Fragen, liegt dies häufig daran, dass Sie eine relationale Datenbank haben, in der eine der nosql-Optionen besser funktionieren würde. In diesen Situationen wäre der Kunde besser gewesen, wenn er die nosql-Lösung implementiert hätte, bevor die app in der Produktionsumgebung bereitgestellt wird.

Andererseits wäre es auch ein Fehler, wenn Sie davon ausgehen, dass eine nosql-Datenbank alles gut oder ausreichend erledigen kann. Es gibt keine optimale Daten Verwaltungs Option für alle Datenspeicher Tasks. verschiedene Daten Verwaltungslösungen sind für verschiedene Aufgaben optimiert. Die meisten realen Cloud-apps verfügen über eine Reihe von Datenspeicher Anforderungen und werden häufig am besten durch eine Kombination aus mehreren Datenspeicherlösungen bedient.

Der Zweck dieses Kapitels besteht darin, Ihnen einen umfassenderen Überblick über die für eine Cloud-App verfügbaren Datenspeicher Optionen zu vermitteln und einige grundlegende Anleitungen zum Auswählen der für Ihr Szenario geeigneten Optionen zu erhalten. Sie sollten sich mit den verfügbaren Optionen vertraut machen und sich über ihre Stärken und Schwächen Gedanken machen, bevor Sie eine Anwendung entwickeln. Das Ändern von Datenspeicher Optionen in einer Produktions-App kann sehr schwierig sein, z. b. Wenn Sie eine Jet-Engine ändern möchten, während die Ebene aktiv ist.

## <a name="data-storage-options-on-azure"></a>Datenspeicher Optionen in Azure

In der Cloud ist es relativ einfach, eine Vielzahl von relationalen und nosql-Daten speichern zu verwenden. Im folgenden finden Sie einige der Datenspeicher Plattformen, die Sie in Azure verwenden können.

![](data-storage-options/_static/image1.png)

Die Tabelle zeigt vier Typen von nosql-Datenbanken:

- [Schlüssel-Wert-Datenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec7) speichern ein einzelnes serialisiertes Objekt für jeden Schlüsselwert. Diese eignen sich gut zum Speichern von großen Datenmengen, bei denen Sie ein Element für einen bestimmten Schlüsselwert abrufen möchten und keine Abfrage basierend auf anderen Eigenschaften des Elements durchführen müssen.

    [Azure BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) ist eine Schlüssel-Wert-Datenbank, die wie File Storage in der Cloud funktioniert. dabei handelt es sich um Schlüsselwerte, die Ordner-und Dateinamen entsprechen. Sie rufen eine Datei anhand des Ordners und des Datei namens ab, nicht durchsuchen nach Werten im Dateiinhalt.

    [Azure Table Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) ist auch eine Schlüssel-Wert-Datenbank. Jeder Wert wird als *Entität* bezeichnet (ähnlich einer Zeile, die durch einen Partitions Schlüssel und einen Zeilen Schlüssel identifiziert wird) und enthält mehrere *Eigenschaften* (ähnlich wie Spalten, aber nicht alle Entitäten in einer Tabelle müssen dieselben Spalten gemeinsam verwenden). Das Abfragen von anderen Spalten als dem Schlüssel ist äußerst ineffizient und sollte vermieden werden. Beispielsweise können Sie Benutzerprofil Daten speichern, wobei eine Partition Informationen zu einem einzelnen Benutzer speichert. Sie können Daten wie z. b. Benutzername, Kenn Wort Hash, Geburtsdatum usw. in separaten Eigenschaften einer Entität oder in separaten Entitäten in derselben Partition speichern. Sie möchten jedoch nicht alle Benutzer mit einem bestimmten Bereich von Geburtsdaten Abfragen, und Sie können keine joinabfrage zwischen ihrer Profil Tabelle und einer anderen Tabelle ausführen. Der Tabellen Speicher ist skalierbarer und kostengünstiger als eine relationale Datenbank, aber er ermöglicht keine komplexen Abfragen oder Joins.
- [Documentdatenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sind Schlüssel-Wert-Datenbanken, in denen die Werte *Dokumente*sind. "Document" wird hier nicht im Sinne eines Word-oder Excel-Dokuments verwendet, sondern eine Auflistung benannter Felder und Werte, von denen jedes ein untergeordnetes Dokument sein könnte. Beispielsweise kann ein Bestelldokument in einer Tabelle mit Bestell Verläufen die Bestellnummer, das Bestelldatum und die Kunden Felder enthalten. und das Feld Customer weist möglicherweise die Felder Name und Adresse auf. In der Datenbank werden Felddaten in einem Format codiert, z. b. XML, YAML, JSON oder bson. oder es kann nur-Text verwendet werden. Eine Funktion, die Dokument Datenbanken von Schlüssel-Wert-Datenbanken unterscheidet, ist die Möglichkeit, nicht Schlüsselfelder abzufragen und sekundäre Indizes zu definieren, um die Abfrage effizienter zu gestalten. Durch diese Fähigkeit ist eine dokumentdatenbank besser für Anwendungen geeignet, die Daten auf der Grundlage von Kriterien abrufen müssen, die komplexer sind als der Wert des Dokument Schlüssels. Beispielsweise können Sie in einer dokumentdatenbank mit dem Verkaufs Auftrags Verlauf Abfragen für verschiedene Felder durchsuchen, z. b. Produkt-ID, Kunden-ID, Kunden Name usw. [MongoDB](http://www.mongodb.org/) ist eine beliebte dokumentdatenbank.
- [Spalten Familien Datenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sind Schlüssel-Wert-Datenspeicher, die es Ihnen ermöglichen, Datenspeicherung in Auflistungen verwandter Spalten zu strukturieren, die als Spalten Familien bezeichnet werden. Beispielsweise kann eine Census-Datenbank eine Gruppe von Spalten für den Namen einer Person (First, Middle, Last), eine Gruppe für die Adresse der Person und eine Gruppe für die Profilinformationen der Person (DOB, Geschlecht usw.) enthalten. In der Datenbank kann dann jede Spalten Familie in einer separaten Partition gespeichert werden, wobei alle Daten für eine Person im Zusammenhang mit dem gleichen Schlüssel aufbewahrt werden. Sie können dann alle Profilinformationen lesen, ohne alle Informationen zu Name und Adresse lesen zu müssen. [Cassandra](http://cassandra.apache.org/) ist eine beliebte Datenbank der Spalten Familie.
- [Graph-Datenbanken](https://msdn.microsoft.com/library/dn313285.aspx#sec10) speichern Informationen als eine Auflistung von Objekten und Beziehungen. Der Zweck einer Graph-Datenbank besteht darin, eine Anwendung für die effiziente Ausführung von Abfragen zu aktivieren, die das Netzwerk von Objekten und die Beziehungen zwischen Ihnen durchlaufen. Die Objekte können beispielsweise Mitarbeiter in einer Personalverwaltungsdatenbank sein, und Sie können Abfragen der Art „Alle Mitarbeiter ermitteln, die direkt oder indirekt für Stephan arbeiten“ durchführen. [Neo4j](http://www.neo4j.org/) ist eine beliebte Graph-Datenbank.

Im Vergleich zu relationalen Datenbanken bieten die nosql-Optionen eine weitaus größere Skalierbarkeit und Kosteneffizienz für die Speicherung und Analyse von unstrukturierten Daten. Der Kompromiss besteht darin, dass Sie nicht die umfangreichen Funktionen für die Abfrage Fähigkeit und die stabile Datenintegrität relationaler Datenbanken bereitstellen. Nosql funktioniert gut für IIS-Protokolldaten, bei denen eine hohe Datenmenge ohne Verknüpfungs Abfragen erforderlich ist. Nosql funktioniert nicht so gut für Banktransaktionen, was eine absolute Datenintegrität erfordert und viele Beziehungen zu anderen Konto bezogenen Daten umfasst.

Es gibt auch eine neuere Kategorie der Daten Bank Plattform mit dem Namen [newsql](http://en.wikipedia.org/wiki/NewSQL) , die die Skalierbarkeit einer nosql-Datenbank mit der Abfrage-und Transaktions Integrität einer relationalen Datenbank kombiniert. Newsql-Datenbanken sind für verteilte Speicher-und Abfrage Verarbeitung konzipiert, die häufig in OldSQL-Datenbanken nicht implementiert werden können. [Nuodb](http://www.nuodb.com/) ist ein Beispiel für eine newsql-Datenbank, die in Azure verwendet werden kann.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop und MapReduce

Die großen Datenmengen, die Sie in nosql-Datenbanken speichern können, können möglicherweise nicht rechtzeitig effizient analysiert werden. Zu diesem Zweck können Sie ein Framework wie [Hadoop](http://hadoop.apache.org/) verwenden, das die [MapReduce](http://en.wikipedia.org/wiki/MapReduce) -Funktionalität implementiert. Der MapReduce-Prozess führt im Wesentlichen die folgenden Schritte aus:

- Begrenzen Sie die Größe der Daten, die verarbeitet werden müssen, indem Sie aus dem Datenspeicher nur die Daten auswählen, die Sie tatsächlich analysieren müssen. Wenn Sie z. b. die Zusammensetzung ihrer Benutzerbasis nach Geburtsjahr kennen möchten, wählen Sie nur Geburtsjahre aus Ihrem Benutzerprofil-Datenspeicher aus.
- Unterteilen Sie die Daten in Teile, und senden Sie Sie zur Verarbeitung an verschiedene Computer. Computer a berechnet die Anzahl der Personen mit 1950-1959 Datumsangaben, Computer B 1960-1969 usw. Diese Gruppe von Computern wird als *Hadoop-Cluster*bezeichnet.
- Fügen Sie die Ergebnisse jedes Teils wieder zusammen, nachdem die Verarbeitung der Teile abgeschlossen ist. Sie haben jetzt eine relativ kurze Liste, wie viele Personen für die einzelnen Geburtsjahre und die Aufgabe der Berechnung von Prozentsätzen in dieser Gesamtliste verwaltbar sind.

In Azure können Sie mit [hdinsight](https://azure.microsoft.com/services/hdinsight/) neue Einblicke aus Big Data mithilfe der Leistungsfähigkeit von Hadoop verarbeiten, analysieren und gewinnen. Beispielsweise können Sie Sie zum Analysieren von Webserver Protokollen verwenden:

- Aktivieren Sie die Webserver Protokollierung für Ihr Speicherkonto. Dadurch wird Azure so eingerichtet, dass Protokolle für jede HTTP-Anforderung an Ihre Anwendung in den BLOB-Dienst geschrieben werden. Der BLOB-Dienst ist im Grunde genommen ein clouddateispeicher und ist problemlos in hdinsight integriert.

    ![Protokolliert an BLOB Storage](data-storage-options/_static/image2.png)
- Wenn die APP Datenverkehr erhält, werden Webserver-IIS-Protokolle in den BLOB-Speicher geschrieben.

    ![Webserverprotokolle](data-storage-options/_static/image3.png)
- Klicken Sie im Portal auf **neu** - **Data Services** - **hdinsight** - **schneller**Fassung, und geben Sie einen hdinsight-Cluster Namen, eine Clustergröße (Anzahl der hdinsight-Cluster Datenknoten) und einen Benutzernamen und ein Kennwort für den hdinsight-Cluster an.

    ![HDInsight](data-storage-options/_static/image4.png)

Sie können nun MapReduce-Aufträge einrichten, um Ihre Protokolle zu analysieren und Antworten auf Fragen zu erhalten, wie z. b.:

- Für welche Tageszeiten erhält meine APP den größten oder geringsten Datenverkehr?
- Aus welchen Ländern kommt mein Datenverkehr?
- Wie hoch ist das Durchschnittseinkommen der Bereiche, von denen der Datenverkehr stammt. (Es gibt ein öffentliches DataSet, mit dem Sie in der Nähe der IP-Adresse nach der IP-Adresse und mit der IP-Adresse in den Webserver Protokollen vergleichen können.)
- Wie verhält sich das Einkommen in der Umgebung mit bestimmten Seiten oder Produkten der Site?

Anschließend können Sie die Antworten auf solche Fragen auf der Grundlage der Wahrscheinlichkeit verwenden, die ein Kunde interessiert oder wahrscheinlich ein bestimmtes Produkt kaufen würde.

Wie im [Kapitel Automatisieren von allem](automate-everything.md)erläutert, können die meisten Funktionen, die Sie im Portal ausführen können, automatisiert werden. Dies umfasst das Einrichten und Ausführen von hdinsight-Analyse Aufträgen. Ein typisches hdinsight-Skript kann die folgenden Schritte enthalten:

- Stellen Sie einen hdinsight-Cluster bereit, und verknüpfen Sie ihn mit Ihrem Speicherkonto für die BLOB Storage-Eingabe.
- Laden Sie die ausführbaren MapReduce-Auftragsdateien (jar-oder exe-Dateien) in den hdinsight-Cluster hoch.
- Übermitteln Sie eine MapReduce, in der die Ausgabedaten in BLOB Storage gespeichert werden.
- Warten Sie, bis der Auftrag beendet wurde.
- Löschen des HDInsight-Clusters
- Greifen Sie auf die Ausgabe aus BLOB Storage zu.

Durch Ausführen eines Skripts, das all dies erledigt, minimieren Sie die Zeitspanne, in der der hdinsight-Cluster bereitgestellt wird, wodurch ihre Kosten minimiert werden.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platform as a Service (PAS) im Vergleich zu Infrastructure-as-a-Service (IaaS)

Zu den zuvor aufgeführten Datenspeicher Optionen zählen sowohl Platform-as-a-Service (PAS)-als auch Infrastructure-as-a-Service (IaaS)-Lösungen. Das bedeutet, dass wir die Hardware-und Software Infrastruktur verwalten und nur den Dienst verwenden. SQL-Datenbank ist ein Feature von Azure. Sie Fragen nach Datenbanken, und im Hintergrund richtet Azure die virtuellen Computer ein und konfiguriert Sie und richtet die Datenbanken auf diesen ein. Sie haben keinen direkten Zugriff auf die VMs und müssen Sie nicht verwalten. IaaS bedeutet, dass Sie virtuelle Computer, die in unserer Rechenzentrumsinfrastruktur ausgeführt werden, einrichten, konfigurieren und verwalten. Wir stellen einen Katalog mit vorkonfigurierten VM-Images für allgemeine VM-Konfigurationen bereit. Beispielsweise können Sie vorkonfigurierte VM-Images für Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database usw. installieren.

Zu den von Azure angebotenen Datenlösungen von Azure gehören:

- Azure SQL-Datenbank (früher als SQL Azure bezeichnet). Eine relationale clouddatenbank, die auf SQL Server basiert.
- Azure Table Storage. Eine Schlüssel-Wert-nosql-Datenbank.
- Azure Blob-Speicher. File Storage in der Cloud.

Für IaaS können Sie alles ausführen, was Sie auf einen virtuellen Computer laden können. Beispiel:

- Relationale Datenbanken, z. b. SQL Server, Oracle, MySQL, SQL Compact, SQLite oder Postgres.
- Schlüssel-Wert-Datenspeicher wie memcached, redis, Cassandra und Riak.
- Spaltendaten Speicher, z. b. HBase.
- Dokument Datenbanken, z. b. MongoDB, ravendb und couchdb.
- Diagramm Datenbanken, z. b. neo4j.

![Datenspeicher Optionen in Azure](data-storage-options/_static/image5.png)

Mit der IaaS-Option erhalten Sie nahezu unbegrenzte Datenspeicher Optionen, und viele davon sind besonders einfach zu verwenden, da Sie VMS mit vorkonfigurierten Images erstellen können. Wechseln Sie beispielsweise im Verwaltungs Portal zu **Virtual Machines**, klicken Sie auf die Registerkarte **Images** , und klicken Sie dann auf **Durchsuchen VM-Depot**.

![VM-Depot durchsuchen](data-storage-options/_static/image6.png)

Daraufhin wird eine Liste mit [Hunderten vorkonfigurierten VM-Images](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)angezeigt, und Sie können einen virtuellen Computer aus einem Image erstellen, auf dem ein Datenbankverwaltungssystem vorinstalliert ist, z. b. MongoDB, Neo4J, redis, Cassandra oder couchdb:

![MongoDB in VM-Depot](data-storage-options/_static/image7.png)

Azure ermöglicht die Verwendung von IaaS-Datenspeicher Optionen so einfach wie möglich, aber die Angebote der Angebote bieten viele Vorteile, die Sie für viele Szenarien kostengünstiger und praktischer machen:

- Sie müssen keine VMs erstellen, sondern nur das Portal oder ein Skript zum Einrichten eines Datenspeicher verwenden. Wenn Sie einen Datenspeicher mit einer Leistung von 200 Terabyte benötigen, können Sie einfach auf eine Schaltfläche klicken oder einen Befehl ausführen, und in Sekunden können Sie ihn verwenden.
- Sie müssen die VMs, die vom Dienst verwendet werden, nicht verwalten oder patchen. Microsoft erledigt dies automatisch. Sie müssen sich keine Gedanken über das Einrichten der Infrastruktur für die Skalierung oder Hochverfügbarkeit machen. Microsoft kümmert sich um diese für Sie.
- Sie müssen keine Lizenzen erwerben. Lizenzgebühren sind in den Dienstgebühren enthalten.
- Sie bezahlen nur für die tatsächliche Nutzung.

Optionen für die Datenspeicherung in Azure sind Angebote von Drittanbietern. Beispielsweise können Sie das [mongolab-Add-on](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) aus dem Azure-Store auswählen, um eine MongoDB-Database as a Service bereitzustellen.

## <a name="choosing-a-data-storage-option"></a>Auswählen einer Datenspeicher Option

Ein Ansatz ist nicht für alle Szenarien geeignet. Wenn jemand sagt, dass diese Technologie die Antwort ist, müssen Sie zuerst Fragen: "Was ist die Frage?", da verschiedene Lösungen für verschiedene Dinge optimiert sind. Es gibt entscheidende Vorteile für das relationale Modell. aus diesem Grund ist es schon so lang. Es gibt aber auch eine Seite zu SQL, die mit einer nosql-Lösung adressiert werden kann.

Häufig sehen wir, dass es sich bei der Arbeit am besten um einen kompositorischen Ansatz handelt, bei dem SQL und nosql in einer einzigen Lösung verwendet werden. Auch wenn man sagt, dass Sie nosql akzeptieren, werden Sie häufig feststellen, dass Sie mehrere verschiedene nosql-Frameworks verwenden: Sie verwenden [couchdb](http://wiki.apache.org/couchdb/Introduction)und [redis](http://redis.io/)und [Riak](http://basho.com/riak/) für verschiedene Dinge. Auch Facebook, das nosql ausgiebig verwendet, verwendet verschiedene nosql-Frameworks für verschiedene Teile des Dienstanbieter. Die Flexibilität, Datenspeicher Ansätze zu kombinieren und abzugleichen, ist eines der Dinge, die für die Cloud von Vorteil sind, da es einfach ist, mehrere Datenlösungen zu verwenden und Sie in eine einzige APP zu integrieren.

Im folgenden finden Sie einige Fragen, die Sie beim Auswählen eines Ansatzes berücksichtigen sollten:

| Daten Semantik | -Was sind die wichtigsten Datenspeicher-und Datenzugriffs Semantik (Speichern Sie relationale oder unstrukturierte Daten)? Unstrukturierte Daten wie z. b. Mediendateien sind am besten in BLOB Storage geeignet. eine Sammlung verwandter Daten, wie z. b. Produkte, Inventuren, Lieferanten, Kunden Bestellungen usw., eignet sich am besten für eine relationale Datenbank. |
| --- | --- |
| Abfrage Unterstützung | -Wie einfach ist es, die Daten abzufragen? Welche Arten von Fragen können effizient gestellt werden? Schlüssel-Wert-Datenspeicher sind sehr gut geeignet, um eine einzelne Zeile mit einem Schlüsselwert zu erhalten, aber nicht so gut für komplexe Abfragen. Bei einem Benutzerprofil-Datenspeicher, in dem Sie die Daten für einen bestimmten Benutzer immer erhalten, kann ein Schlüssel-Wert-Datenspeicher gut funktionieren. für einen Produktkatalog, in dem Sie unterschiedliche Gruppierungen basierend auf verschiedenen Produkt Attributen erhalten möchten, kann eine relationale Datenbank besser funktionieren. Nosql-Datenbanken können große Datenmengen effizient speichern, aber Sie müssen die Datenbank so strukturieren, dass die Daten von der APP abgefragt werden. Dies erschwert Ad-hoc-Abfragen. Mit einer relationalen Datenbank können Sie nahezu jede Art von Abfrage erstellen. |
| Funktionale Projektion | : Können Fragen, Aggregationen usw. serverseitig ausgeführt werden? Wenn ich "SELECT count (\*)" aus einer Tabelle in SQL ausführe, wird die gesamte Arbeit auf dem Server sehr effizient ausgeführt, und die Zahl, nach der ich Suche, wird zurückgegeben. Wenn die gleiche Berechnung aus einem nosql-Datenspeicher gewünscht werden soll, der keine Aggregation unterstützt, handelt es sich hierbei um eine ineffiziente "unbegrenzte Abfrage", die wahrscheinlich ein Timeout hat. Auch wenn die Abfrage erfolgreich ist, muss ich alle Daten vom Server an den Client abrufen und die Zeilen auf dem Client zählen. Welche Sprachen oder Typen von Ausdrücken können verwendet werden? Mit einer relationalen Datenbank kann ich SQL verwenden. Bei einigen nosql-Datenbanken wie Azure Table Storage verwende ich [odata](http://www.odata.org/), und ich kann nur den Primärschlüssel Filtern und Projektionen abrufen (Wählen Sie eine Teilmenge der verfügbaren Felder aus). |
| Einfache Skalierbarkeit | -Wie oft und wie viel müssen die Daten skaliert werden? : Implementiert die Plattform für horizontales Skalieren nativ? -Wie einfach ist es, Kapazität (Größe und Durchsatz) hinzuzufügen/zu entfernen? Relationale Datenbanken und Tabellen werden nicht automatisch partitioniert, um Sie skalierbar zu machen, sodass Sie über bestimmte Einschränkungen hinaus schwierig zu skalieren sind. Nosql-Datenspeicher wie der Azure-Tabellen Speicher Partitionieren alle alles, und es gibt fast keine Beschränkung für das Hinzufügen von Partitionen. Sie können Table Storage problemlos bis zu 200 Terabyte skalieren, aber die maximale Datenbankgröße für Azure SQL-Datenbank beträgt 500 Gigabyte. Sie können relationale Daten skalieren, indem Sie Sie in mehrere Datenbanken aufteilen, aber das Einrichten einer Anwendung zur Unterstützung dieses Modells umfasst viele Programmierungs arbeiten. |
| Instrumentation und Verwaltbarkeit | -Wie einfach ist die Plattform zu instrumentieren, zu überwachen und zu verwalten? Sie müssen sich über die Integrität und Leistung Ihres Daten Stores informieren, damit Sie wissen müssen, welche Metriken eine Plattform Ihnen kostenlos zur Verfügung stellt und was Sie selbst entwickeln müssen. |
| Operationen (Operations) | -Wie einfach ist die Plattform zum Bereitstellen und Ausführen in Azure? PaaS? IaaS? Linux? Table Storage und SQL-Datenbank können in Azure problemlos eingerichtet werden. Plattformen, die nicht in Azure-Weblösungen integriert sind, erfordern mehr Aufwand. |
| API-Unterstützung | : Ist eine API verfügbar, die das Arbeiten mit der Plattform erleichtert? Für den Azure-Tabellen Speicherdienst gibt es ein SDK mit einer .NET-API, das das asynchrone Programmiermodell von .NET 4,5 unterstützt. Wenn Sie eine .net-app schreiben, ist es viel einfacher, Code für den Azure-Tabellen Dienst im Vergleich zu einer anderen Datenspeicher Plattform für Schlüssel-Wert-Spalten zu schreiben und zu testen, die über keine API oder eine weniger umfassende API verfügt. |
| Transaktions Integrität und Datenkonsistenz | : Ist es wichtig, dass die Plattform Transaktionen unterstützt, um die Datenkonsistenz zu gewährleisten? Zum Nachverfolgen von gesendeten Massen-e-Mails können Leistung und niedrige Datenspeicher Kosten wichtiger sein als die automatische Unterstützung von Transaktionen oder der referenziellen Integrität in der Datenplattform, wodurch der Azure-Tabellen Dienst eine gute Wahl ist. Zum Nachverfolgen der Guthaben von Konten oder Bestellungen ist eine relationale Daten Bank Plattform, die starke Transaktions Garantien bietet, eine bessere Wahl. |
| Geschäftskontinuität | -Wie einfach sind Sicherung, Wiederherstellung und Notfall Wiederherstellung? Früher oder später werden Produktionsdaten beschädigt, und Sie benötigen eine Rückgängig-Funktion. Relationale Datenbanken verfügen häufig über differenziertere Wiederherstellungs Funktionen, wie z. b. die Möglichkeit der Wiederherstellung bis zu einem bestimmten Zeitpunkt. Wenn Sie wissen, welche Wiederherstellungs Features auf den einzelnen Plattformen verfügbar sind, ist ein wichtiger Faktor zu berücksichtigen. |
| Kosten | Wenn mehr als eine Plattform Ihre Daten Auslastung unterstützen kann, wie vergleichen Sie die Kosten? Wenn Sie z. b. ASP.net Identity verwenden, können Sie Benutzerprofil Daten im Azure-Tabellen Speicherdienst oder in der Azure SQL-Datenbank speichern. Wenn Sie die umfassenden Abfragefunktionen von SQL-Datenbank nicht benötigen, können Sie ggf. Azure-Tabellen auswählen, da Sie für eine bestimmte Menge an Speicher viel weniger Kosten. |

Wir empfehlen im Allgemeinen die Beantwortung der Fragen in den einzelnen Kategorien, bevor Sie Ihre Datenspeicherlösungen auswählen.

Außerdem kann für ihre Arbeitsauslastung bestimmte Anforderungen erfüllt sein, die von einigen Plattformen besser unterstützt werden. Beispiel:

- Erfordert Ihre Anwendung Überwachungsfunktionen?
- Was sind die Anforderungen an die Daten Langlebigkeit?-benötigen Sie automatisierte Archivierungs-oder Löschfunktionen?
- Verfügen Sie über spezielle Sicherheitsanforderungen? Beispielsweise enthalten die Daten PII (personenbezogene Informationen). Sie müssen jedoch sicherstellen können, dass PII aus den Abfrage Ergebnissen ausgeschlossen wird.
- Wenn Sie über einige Daten verfügen, die aus gesetzlichen oder technologischen Gründen nicht in der Cloud gespeichert werden können, benötigen Sie möglicherweise eine cloudbasierte Datenspeicher Plattform, die die Integration in Ihren lokalen Speicher vereinfacht.

## <a name="demo--using-sql-database-in-azure"></a>Demo – Verwenden von SQL-Datenbank in Azure

Die Korrektur der IT-App verwendet eine relationale Datenbank zum Speichern von Tasks. Das Windows PowerShell-Skript für die Umgebungs Erstellung, das im [Kapitel Automatisieren von allen](automate-everything.md) angezeigt wird, erstellt zwei SQL-Datenbankinstanzen Sie können diese im Portal anzeigen, indem Sie auf die Registerkarte **SQL-Datenbanken** klicken.

![SQL-Datenbanken im Portal](data-storage-options/_static/image8.png)

Es ist auch einfach, Datenbanken über das Portal zu erstellen.

Klicken Sie auf **neu--Data Services** -- **SQL-Datenbank** -- **schneller**Fassung, geben Sie einen Datenbanknamen ein, wählen Sie einen Server aus, den Sie bereits in Ihrem Konto besitzen, oder erstellen Sie einen neuen, und klicken Sie auf **SQL-Datenbank**

![Neue SQL-Datenbank](data-storage-options/_static/image9.png)

Warten Sie einige Sekunden, und Sie haben eine Datenbank in Azure, die Sie verwenden können.

![Neue SQL-Datenbank erstellt](data-storage-options/_static/image10.png)

Azure führt also in wenigen Sekunden eine Ausführung von einem Tag oder einer Woche oder länger aus, um die lokale Umgebung auszuführen. Und da Sie Datenbanken genauso einfach automatisch in einem Skript oder mithilfe einer Verwaltungs-API erstellen können, können Sie dynamisch horizontal hochskalieren, indem Sie Ihre Daten auf mehrere Datenbanken verteilen, solange die Anwendung dafür programmiert wurde.

Dies ist ein Beispiel für unser Platform-as-a-Service-Modell. Sie müssen die Server nicht verwalten. Sie müssen sich keine Gedanken über Sicherungen machen. Der Dienst wird mit hoher Verfügbarkeit ausgeführt – die Daten in der Datenbank werden automatisch auf drei Server repliziert. Wenn ein Computer ausfällt, wird automatisch ein Failover durchgeführt, und es gehen keine Daten verloren. Der Server wird regelmäßig gepatcht, Sie müssen sich damit keine Gedanken machen.

Klicken Sie auf eine Schaltfläche, und Sie erhalten die genaue Verbindungs Zeichenfolge, die Sie benötigen, und können die neue Datenbank sofort verwenden.

![Verbindungszeichenfolgen](data-storage-options/_static/image11.png)

Das Dashboard zeigt den Verbindungs Verlauf und die Menge des verwendeten Speichers an.

![SQL-Datenbank-Dashboard](data-storage-options/_static/image12.png)

Sie können Datenbanken im Portal oder mit SQL Server Tools verwalten, mit denen Sie bereits vertraut sind, einschließlich SQL Server Management Studio (SSMS) und der Visual Studio-Tools SQL Server-Objekt-Explorer (ssox) und Server-Explorer.

![SSOX](data-storage-options/_static/image13.png)

Ein weiteres nützliches ist das Preismodell. Sie können mit der Entwicklung mit einer kostenlosen 20-MB-Datenbank beginnen, und eine Produktionsdatenbank beginnt um $5 Uhr pro Monat. Sie zahlen nur für die Menge der Daten, die Sie tatsächlich in der Datenbank speichern, nicht für die maximale Kapazität. Sie müssen keine Lizenz erwerben.

SQL-Datenbank ist einfach zu skalieren. Bei der Korrektur der IT-APP ist die Datenbank, die wir in unserem Automation-Skript erstellen, auf 1 GB begrenzt. Wenn Sie es bis zu 150 GB skalieren möchten, können Sie einfach in das Portal wechseln und die Einstellung ändern oder einen Rest-API-Befehl ausführen, und in Sekunden verfügen Sie über eine 150-GB-Datenbank, in der Sie Daten bereitstellen können.

![Editionen und Größen von SQL-Datenbanken](data-storage-options/_static/image14.png)

Das ist die Leistungsfähigkeit der Cloud, die Infrastruktur schnell und einfach zu nutzen und sofort mit der Verwendung zu beginnen.

Die Korrektur der IT-App verwendet zwei SQL-Datenbanken, eine für die Mitgliedschaft (Authentifizierung und Autorisierung) und eine für Daten, und das ist alles, was Sie tun müssen, um Sie bereitzustellen und zu skalieren. Sie haben bereits gesehen, wie Sie die Datenbanken mithilfe von Windows PowerShell-Skripts bereitstellen, und nun haben Sie auch gesehen, wie einfach es ist, im Portal zu Vorgehen.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework im Vergleich zum direkten Datenbankzugriff mithilfe von ADO.net

Die Korrektur der IT-App greift mithilfe des Entity Framework auf diese Datenbanken zu, der von Microsoft empfohlenen ORM (Object-Relational Mapper) für .NET-Anwendungen. Ein ORM ist ein großartiges Tool, das die Produktivität des Entwicklers vereinfacht, aber die Produktivität wird in einigen Szenarien zu einer beeinträchtigten Leistung. In einer realen Cloud-APP treffen Sie keine Wahl zwischen der Verwendung von EF oder der direkten Verwendung von ADO.net. Sie verwenden beides. In den meisten Fällen, in denen Sie Code schreiben, der mit der Datenbank funktioniert, ist das erzielen der maximalen Leistung nicht entscheidend, und Sie können die vereinfachten Codierungen und Tests nutzen, die Sie mit dem Entity Framework erhalten. In Situationen, in denen der EF-Overhead zu einer unzulässigen Leistung führen würde, können Sie eigene Abfragen mit ADO.net schreiben und ausführen, idealerweise durch Aufrufen gespeicherter Prozeduren.

Jede Methode, die Sie für den Zugriff auf die Datenbank verwenden, möchten Sie so weit wie möglich mit "chattiness" minimieren. Anders ausgedrückt: Wenn Sie alle benötigten Daten in einem größeren Abfrageresultset und nicht in Dutzenden oder Hunderten von kleineren Abfragen erhalten, ist dies in der Regel vorzuziehen. Wenn Sie z. b. Studenten und die Kurse auflisten müssen, in denen Sie registriert sind, ist es in der Regel besser, alle Daten in einer joinabfrage zu erhalten, statt die Studenten in einer Abfrage zu erhalten und separate Abfragen für die Kurse der einzelnen Kurs Personen auszuführen.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL-Datenbanken und die Entity Framework in der APP zum Reparieren

Die `FixItContext`-Klasse, die von der Entity Framework `DbContext`-Klasse abgeleitet wird, identifiziert die-Datenbank in der App "korrigieren der IT-app" und gibt die Tabellen in der Datenbank an. Der Kontext gibt eine Entitätenmenge (Tabelle) für Aufgaben an, und der Code übergibt an den Kontext den Namen der Verbindungs Zeichenfolge. Dieser Name verweist auf eine Verbindungs Zeichenfolge, die in der Datei "Web. config" definiert ist.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

Die Verbindungs Zeichenfolge in der Datei " *Web. config* " heißt appdb (hier verweist auf die lokale Entwicklungs Datenbank):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Der Entity Framework erstellt eine *fixttasks* -Tabelle basierend auf den Eigenschaften, die in der `FixItTask` Entitäts Klasse enthalten sind. Dies ist eine einfache poco-Klasse (Plain Old CLR Object), was bedeutet, dass Sie nicht von der Entity Framework erbt oder Abhängigkeiten hat. Entity Framework weiß jedoch, wie eine Tabelle auf der Basis erstellt und CRUD-Vorgänge (Create-Read-Update-Delete) damit ausgeführt werden.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabelle "fixttasks"](data-storage-options/_static/image15.png)

Die Korrektur-IT-app enthält eine Repository-Schnittstelle, die für CRUD-Vorgänge verwendet wird, die mit dem Datenspeicher arbeiten.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Beachten Sie, dass die Repository-Methoden alle asynchron sind, sodass der gesamte Datenzugriff auf eine vollständig asynchrone Weise erfolgen kann.

Die Repository-Implementierung ruft Entity Framework Async-Methoden auf, um mit den Daten zu arbeiten, einschließlich LINQ-Abfragen sowie von INSERT-, Update-und DELETE-Vorgängen. Im folgenden finden Sie ein Beispiel für den Code für die Suche nach einer Korrektur-IT-Aufgabe.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Sie werden feststellen, dass hier auch etwas Zeit-und Fehler Protokollierungs Code vorhanden ist. Wir betrachten dies später im [Kapitel Überwachung und Telemetrie](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Auswählen von SQL-Datenbank (PAS) im Vergleich zu SQL Server in einer VM (IaaS) in Azure

Eine gute Sache von SQL Server und Azure SQL-Datenbank ist, dass das Kern Programmiermodell für beide identisch ist. Sie können die meisten der gleichen Fähigkeiten in beiden Umgebungen verwenden. Sie können sogar eine SQL Server Datenbank in der Entwicklung und eine SQL-Daten Bank Instanz in der Cloud verwenden, sodass die Korrektur der IT-App eingerichtet ist.

Als Alternative können Sie die gleiche SQL Server in der Cloud ausführen, die Sie lokal ausführen, indem Sie Sie auf IaaS-VMS installieren. Bei einigen Legacy Anwendungen ist das Ausführen von SQL Server auf einem virtuellen Computer möglicherweise eine bessere Lösung. Da eine SQL Server Datenbank auf einem dedizierten virtuellen Computer ausgeführt wird, sind mehr Ressourcen als eine SQL-DatenbankDatenbank verfügbar, die auf einem freigegebenen Server ausgeführt wird. Dies bedeutet, dass eine SQL Server Datenbank größer sein kann und dennoch eine gute Leistung zur Folge hat. Im Allgemeinen gilt: je geringer die Datenbankgröße und die Tabellengröße, desto besser funktioniert der Anwendungsfall für SQL-Datenbank (PAS).

Im folgenden finden Sie einige Richtlinien für die Wahl zwischen den beiden Modellen.

| Azure SQL-Datenbank (PaaS) | SQL Server auf einem virtuellen Computer (IaaS) |
| --- | --- |
| Professionals: Sie müssen keine VMs erstellen oder verwalten, ein Update-oder **Patch-Betriebs** System oder SQL. Azure erledigt dies für Sie. -Integrierte Hochverfügbarkeit mit einer SLA auf Datenbankebene. -Niedrige Gesamtbetriebskosten (TCO), da Sie nur für das bezahlen, was Sie nutzen (keine Lizenz erforderlich). -Gut für die Verarbeitung einer großen Anzahl kleinerer Datenbanken (&lt;= 500 GB). : Einfaches Erstellen von neuen Datenbanken zum Aktivieren der horizontalen Skalierung. | ***Vorteile*** : featurekompatibel mit lokalem SQL Server. -Kann SQL Server [Hochverfügbarkeit über AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) in zwei virtuellen Computern mit einer SLA auf VM-Ebene implementieren. -Sie haben die umfassende Kontrolle über die Verwaltung von SQL. -Sie können SQL-Lizenzen, die Sie bereits besitzen, wieder verwenden, oder Sie zahlen nach der Stunde für eine. -Gut für die Verarbeitung weniger, aber größerer (1 TB) Datenbanken. |
| **Nachteile** : einige Merkmals Lücken im Vergleich zu lokalen SQL Server (fehlende [CLR-Integration](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [Komprimierungs Unterstützung](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)usw.)-Daten Bank Größenbeschränkung von 500 GB. | ***Nachteile*** : für die Erstellung und Verwaltung von Datenbanken sind die Datenträger-IOPS (e/a-Vorgänge pro Sekunde) auf ungefähr 8000 (über 16 Daten Laufwerke) beschränkt. |

Wenn Sie SQL Server auf einem virtuellen Computer verwenden möchten, können Sie Ihre eigene SQL Server Lizenz verwenden, oder Sie können für eine Stunde bezahlen. Beispielsweise können Sie im Portal oder über die Rest-API einen neuen virtuellen Computer mit einem SQL Server Image erstellen.

![Erstellen einer VM mit SQL Server](data-storage-options/_static/image16.png)

![Liste der SQL Server VM-Images](data-storage-options/_static/image17.png)

Wenn Sie einen virtuellen Computer mit einem SQL Server Abbild erstellen, legen wir die SQL Server Lizenzkosten basierend auf Ihrer Nutzung des virtuellen Computers nach der Stunde ab. Wenn Sie ein Projekt haben, das nur einige Monate lang ausgeführt wird, ist es günstiger, stündlich zu bezahlen. Wenn Sie der Ansicht sind, dass das Projekt für Jahre in der Zukunft läuft, ist es günstiger, die Lizenz auf die gleiche Weise wie gewohnt zu erwerben.

## <a name="summary"></a>Zusammenfassung

Cloud Computing vereinfacht das kombinieren und Abgleichen von Datenspeicher Ansätzen, damit Sie die Anforderungen Ihrer Anwendung am besten erfüllen. Wenn Sie eine neue Anwendung entwickeln, sollten Sie sich die hier aufgeführten Fragen sorgfältig überlegen, um Ansätze auszuwählen, die weiterhin gut funktionieren, wenn Ihre Anwendung wächst. Im [nächsten Kapitel](data-partitioning-strategies.md) werden einige Partitionierungs Strategien erläutert, die Sie verwenden können, um mehrere Daten Speicherungs Ansätze zu kombinieren.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Auswählen einer Daten Bank Plattform:

- [Datenzugriff für hochgradig skalierbare Lösungen: Verwenden von SQL, nosql und Polyglot Persistenz](https://aka.ms/dag-doc). E-Book von Microsoft-Mustern und-Praktiken, die sich ausführlich mit den verschiedenen Arten von Daten speichern befassen, die für cloudanwendungen verfügbar sind.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/ff898430.aspx). Weitere Informationen finden Sie unter Datenkonsistenz, Leitfaden zur Datenreplikation und Synchronisierung, Index Tabellen Muster, materialisiertes Ansichts Muster.
- [Base: eine Acid-Alternative](http://queue.acm.org/detail.cfm?id=1394128). Artikel zu den vor-und Nachteile von Datenkonsistenz und Skalierbarkeit.
- [Sieben Datenbanken in sieben Wochen: eine Anleitung für moderne Datenbanken und die nosql-Bewegung](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Buch von Eric Redmond und Jim R. Wilson. Es wird dringend empfohlen, sich in den Umfang der heute verfügbaren Datenspeicher Plattformen vorzustellen.

Auswählen zwischen SQL Server und SQL-Datenbank:

- [Leitfaden für die Premium-Vorschau für SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Eine Einführung in SQL-Datenbank Premium und eine Anleitung dazu, wann Sie diese über die Web Edition und die Business Edition von SQL-Datenbank auswählen.
- [Richtlinien und Einschränkungen (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Die Portal Seite, die mit der Dokumentation zu den Einschränkungen der SQL-Datenbank verknüpft ist, einschließlich einer, bei der der Schwerpunkt auf SQL Server Features liegt
- [SQL Server in Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Die Portal Seite, die mit der Dokumentation zum Ausführen von SQL Server in Azure verknüpft ist.
- [Scott Guthrie erläutert SQL-Datenbanken in Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6-minütiges Video Einführung in die SQL-Datenbank von Scott Guthrie.
- [Anwendungs Muster und Entwicklungsstrategien für SQL Server in Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Verwenden von Entity Framework und SQL-Datenbank in einer ASP.net-Web-App

- Die ersten Schritte [mit EF 6 unter Verwendung von MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Neun teilige tutorialreihe, die Sie durch das Entwickeln einer MVC-App führt, die EF verwendet und die Datenbank in Azure und SQL-Datenbank bereitstellt.
- [ASP.NET-Webbereitstellung mithilfe von Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)Dabei handelt es sich um eine Lernprogrammserie mit zwölf Teilen, in der die Bereitstellungsaufgaben vollständig vorgestellt werden. Zwölf teilige tutorialreihe, in der die Bereitstellung einer Datenbank mithilfe von EF Code First ausführlicher erläutert wird.
- Stellen [Sie eine sichere ASP.NET MVC 5-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)Website bereit. Schritt-für-Schritt-Lernprogramm, das Sie durch die Erstellung einer Web-App führt, die die-Authentifizierung verwendet, Anwendungs Tabellen in der Mitgliedschafts Datenbank speichert, das Datenbankschema ändert und die app in Azure bereitstellt.
- [ASP.NET Datenzugriffs-Inhalts](https://go.microsoft.com/fwlink/p/?LinkId=282414)Zuordnung. Links zu Ressourcen für die Arbeit mit EF und SQL-Datenbank.

Verwenden von MongoDB in Azure:

- [Mongolab-MongoDB in Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Die Portal Seite für die Dokumentation zur Ausführung von MongoDB in Azure.
- [Erstellen Sie eine Azure-Website, die eine Verbindung mit MongoDB herstellt, die auf einem virtuellen Computer in Azure ausgeführt](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/)wird. Schritt-für-Schritt-Tutorial, das zeigt, wie eine MongoDB-Datenbank in einer ASP.NET-Webanwendung verwendet wird.

Hdinsight (Hadoop in Azure):

- [Hdinsight](https://azure.microsoft.com/documentation/services/hdinsight/): Dokumentation zum hdinsight-Portal auf der [Azure](https://azure.microsoft.com/) -Website.
- [Hadoop und hdinsight: Big Data in Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). MSDN Magazine-Artikel von Bruno terkaly und Ricardo Villalobos, Einführung in Hadoop in Azure.
- [Microsoft Patterns and Practices: Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Siehe MapReduce-Muster.

> [!div class="step-by-step"]
> [Zurück](single-sign-on.md)
> [Weiter](data-partitioning-strategies.md)
