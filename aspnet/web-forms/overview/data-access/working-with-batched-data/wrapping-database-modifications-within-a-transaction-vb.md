---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Umpacken von Daten Bank Änderungen innerhalb einer Transaktion (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Tutorial ist das erste von vier, das das Aktualisieren, löschen und Einfügen von Daten Batches untersucht. In diesem Tutorial erfahren Sie, wie Datenbanktransaktionen zulassen...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: dee95ee2789a69aac5aa79b8358e58e3ee99e1b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78475887"
---
# <a name="wrapping-database-modifications-within-a-transaction-vb"></a>Umschließen von Datenbankänderungen innerhalb einer Transaktion (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) oder [PDF herunterladen](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Dieses Tutorial ist das erste von vier, das das Aktualisieren, löschen und Einfügen von Daten Batches untersucht. In diesem Tutorial erfahren Sie, wie Datenbanktransaktionen Batch Änderungen als atomarer Vorgang ausführen können, wodurch sichergestellt wird, dass entweder alle Schritte erfolgreich sind oder alle Schritte fehlschlagen.

## <a name="introduction"></a>Einführung

Wie bereits im Tutorial zum [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) erläutert wurde, bietet die GridView integrierte Unterstützung für das Bearbeiten und löschen auf Zeilenebene. Mit wenigen Mausklicks ist es möglich, eine umfangreiche Daten Änderungs Schnittstelle zu erstellen, ohne eine Codezeile schreiben zu müssen, solange Sie Inhalt mit Bearbeitung und Löschung auf Zeilen Basis haben. In bestimmten Szenarien reicht dies jedoch nicht aus, und wir müssen Benutzern die Möglichkeit geben, einen Batch von Datensätzen zu bearbeiten oder zu löschen.

Die meisten webbasierten e-Mail-Clients verwenden z. b. ein Raster, um jede Nachricht aufzulisten, wobei jede Zeile ein Kontrollkästchen zusammen mit den e-Mail-Informationen (Betreff, Absender usw.) enthält. Diese Schnittstelle ermöglicht dem Benutzer das Löschen mehrerer Nachrichten durch überprüfen und anschließendes Klicken auf die Schaltfläche ausgewählte Meldungen löschen. Eine Schnittstelle für die Batch Bearbeitung ist ideal für Situationen, in denen Benutzer häufig viele verschiedene Datensätze bearbeiten. Anstatt zu erzwingen, dass der Benutzer auf Bearbeiten klickt, nehmen Sie die Änderung vor, und klicken Sie dann für jeden Datensatz, der geändert werden muss, eine Batch Bearbeitungs Schnittstelle, die jede Zeile mit der Bearbeitungs Schnittstelle rendert. Der Benutzer kann schnell den Satz von Zeilen ändern, der geändert werden muss, und dann diese Änderungen speichern, indem er auf die Schaltfläche Alle aktualisieren klickt. In diesen Tutorials erfahren Sie, wie Sie Schnittstellen zum Einfügen, bearbeiten und Löschen von Daten Batches erstellen.

Beim Ausführen von Batch Vorgängen ist es wichtig zu bestimmen, ob es möglich ist, dass einige der Vorgänge im Batch erfolgreich sind, während andere fehlschlagen. Stellen Sie sich eine Schnittstelle zum Löschen von Batches vor: Was geschieht, wenn der erste ausgewählte Datensatz erfolgreich gelöscht wurde, aber der zweite fehlschlägt, beispielsweise aufgrund einer Verletzung der FOREIGN KEY-Einschränkung? Soll für den ersten Datensatz ein Rollback ausgeführt werden, oder ist es akzeptabel, dass der erste Datensatz gelöscht wird?

Wenn der Batch Vorgang als [atomarer Vorgang](http://en.wikipedia.org/wiki/Atomic_operation)behandelt werden soll, bei dem entweder alle Schritte erfolgreich sind oder alle Schritte fehlschlagen, muss die Datenzugriffs Ebene erweitert werden, um Unterstützung für [Datenbanktransaktionen](http://en.wikipedia.org/wiki/Database_transaction)zu bieten. Datenbanktransaktionen garantieren eine Atomizität für den Satz von `INSERT`-, `UPDATE`-und `DELETE` Anweisungen, die im Rahmen der Transaktion ausgeführt werden, und sind eine Funktion, die von den meisten modernen Datenbanksystemen unterstützt wird.

In diesem Tutorial wird erläutert, wie Sie die DAL für die Verwendung von Datenbanktransaktionen erweitern. In den nachfolgenden Tutorials wird die Implementierung von Webseiten für das Einfügen, aktualisieren und Löschen von Schnittstellen untersucht. Legen Sie Los!

> [!NOTE]
> Wenn Sie Daten in einer Batch Transaktion ändern, ist Atomizität nicht immer erforderlich. In einigen Szenarien ist es möglicherweise akzeptabel, dass einige Datenänderungen erfolgreich sind und andere im gleichen Batch fehlschlagen, z. b. beim Löschen eines Satzes von e-Mails von einem webbasierten e-Mail-Client. Wenn ein Datenbankfehler in der Mitte des Löschvorgangs aufgetreten ist, ist es wahrscheinlich akzeptabel, dass diese Datensätze, die ohne Fehler verarbeitet werden, gelöscht werden. In solchen Fällen muss die DAL nicht geändert werden, um Datenbanktransaktionen zu unterstützen. Es gibt jedoch noch andere Batch Vorgangs Szenarios, in denen Atomizität von entscheidender Bedeutung ist. Wenn ein Kunde ihren Betrag von einem Bankkonto in ein anderes Konto verschiebt, müssen zwei Vorgänge ausgeführt werden: die Beträge müssen vom ersten Konto abgezogen und dann der zweiten hinzugefügt werden. Obwohl die Bank keine Meinung hat, dass der erste Schritt erfolgreich war, der zweite Schritt jedoch fehlschlägt, sind die Kunden verständlicherweise verärgert. Ich empfehle Ihnen, dieses Tutorial zu durcharbeiten und die Erweiterungen der dal zu implementieren, um Datenbanktransaktionen zu unterstützen, auch wenn Sie nicht beabsichtigen, Sie in den folgenden drei Tutorials zu verwenden: einfügen, aktualisieren und Löschen von Schnittstellen.

## <a name="an-overview-of-transactions"></a>Eine Übersicht über Transaktionen

Die meisten Datenbanken unterstützen *Transaktionen*, die es ermöglichen, mehrere Daten Bank Befehle in einer einzelnen logischen Arbeitseinheit zu gruppieren. Die Daten Bank Befehle, aus denen eine Transaktion besteht, sind garantiert atomarisch, was bedeutet, dass entweder alle Befehle fehlschlagen oder alle erfolgreich ausgeführt werden.

Im Allgemeinen werden Transaktionen mithilfe von SQL-Anweisungen in folgendem Muster implementiert:

1. Geben Sie den Start einer Transaktion an.
2. Führen Sie die SQL-Anweisungen aus, die die Transaktion umfassen.
3. Wenn in einer der Anweisungen aus Schritt 2 ein Fehler auftritt, führen Sie ein Rollback für die Transaktion aus.
4. Wenn alle Anweisungen aus Schritt 2 ohne Fehler abgeschlossen werden, führen Sie einen Commit für die Transaktion aus.

Die SQL-Anweisungen, die zum Erstellen, Ausführen eines Commits und Rollbacks der Transaktion verwendet werden, können beim Schreiben von SQL-Skripts oder bei der Erstellung gespeicherter Prozeduren manuell eingegeben werden. Sie können auch Programm gesteuert mithilfe von ADO.net oder den Klassen im [`System.Transactions` Namespace](https://msdn.microsoft.com/library/system.transactions.aspx) In diesem Tutorial untersuchen wir nur die Verwaltung von Transaktionen mithilfe von ADO.net. In einem zukünftigen Tutorial wird erläutert, wie gespeicherte Prozeduren in der Datenzugriffs Ebene verwendet werden. zu diesem Zeitpunkt werden die SQL-Anweisungen zum Erstellen, Rollback und Commit von Transaktionen untersucht. Weitere Informationen finden Sie in der Zwischenzeit unter [Verwalten von Transaktionen in SQL Server gespeicherten Prozeduren](http://www.4guysfromrolla.com/webtech/080305-1.shtml) .

> [!NOTE]
> Die [`TransactionScope`-Klasse](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) im `System.Transactions`-Namespace ermöglicht Entwicklern, eine Reihe von Anweisungen innerhalb des Bereichs einer Transaktion Programm gesteuert zu umschließen, und bietet Unterstützung für komplexe Transaktionen, die mehrere Quellen umfassen, z. b. zwei verschiedene Datenbanken oder sogar heterogene Typen von Daten speichern, wie z. b. eine Microsoft SQL Server Datenbank, eine Oracle-Datenbank und ein Webdienst. Ich entschied mich für die Verwendung von ADO.NET-Transaktionen für dieses Tutorial anstelle der `TransactionScope`-Klasse, da ADO.net spezifischer für Datenbanktransaktionen ist und in vielen Fällen weitaus weniger ressourcenintensiv ist. Außerdem verwendet die `TransactionScope`-Klasse in bestimmten Szenarien die Microsoft-Distributed Transaction Coordinator (MSDTC). Die Konfigurations-, Implementierungs-und Leistungsprobleme im Zusammenhang mit MSDTC machen es zu einem Recht spezialisierten und fortgeschrittenen Thema, das über den Rahmen dieser Tutorials hinausgeht.

Beim Arbeiten mit dem SqlClient-Anbieter in ADO.net werden Transaktionen durch einen-aufrufbefehl der [`SqlConnection`-Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) [`BeginTransaction`-Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx)initiiert, die ein`SqlTransaction`- [Objekt](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx)zurückgibt. Die Daten Änderungs Anweisungen, mit denen die Transaktion zusammengesetzt wird, werden in einem `try...catch`-Block platziert. Wenn ein Fehler in einer Anweisung im `try`-Block auftritt, wird die Ausführung an den `catch` Block übertragen, in dem für die Transaktion über die [`Rollback`-Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)des `SqlTransaction`-Objekts ein Rollback ausgeführt werden kann. Wenn alle-Anweisungen erfolgreich abgeschlossen werden, führt ein aufrufungs Vorgang des `SqlTransaction` Objekt s [`Commit` Methode](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) am Ende des `try` Blocks einen Commit für die Transaktion aus. Der folgende Code Ausschnitt veranschaulicht dieses Muster. Weitere Syntax und Beispiele für die Verwendung von Transaktionen mit ADO.net finden Sie unter Verwalten der [Daten Bank Konsistenz mit Transaktionen](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) .

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Standardmäßig verwenden die TableAdapters in einem typisierten DataSet keine Transaktionen. Um Unterstützung für Transaktionen bereitzustellen, müssen wir die TableAdapter-Klassen erweitern, um zusätzliche Methoden aufzunehmen, die das oben beschriebene Muster verwenden, um eine Reihe von Daten Änderungs Anweisungen innerhalb des Gültigkeits Bereichs einer Transaktion auszuführen. In Schritt 2 erfahren Sie, wie Sie mit partiellen Klassen diese Methoden hinzufügen.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Schritt 1: Erstellen der Webseiten für das Arbeiten mit Batch Daten

Bevor wir uns mit der Erweiterung der dal zur Unterstützung von Datenbanktransaktionen beschäftigen, nehmen Sie sich zunächst einen Moment Zeit, um die ASP.NET-Webseiten zu erstellen, die wir für dieses Tutorial benötigen, und die drei folgenden. Fügen Sie zunächst einen neuen Ordner mit dem Namen `BatchData` hinzu, und fügen Sie dann die folgenden ASP.NET Seiten hinzu, wobei Sie jede Seite der `Site.master` Master Seite zuordnen.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`

![Fügen Sie die ASP.NET-Seiten für die SqlDataSource-bezogenen Tutorials hinzu.](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Abbildung 1**: Hinzufügen der ASP.NET-Seiten für die auf SqlDataSource bezogenen Tutorials

Wie bei den anderen Ordnern verwendet `Default.aspx` das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement, um die Lernprogramme in diesem Abschnitt aufzulisten. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))

Fügen Sie schließlich diese vier Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem Anpassen der Site Map-`<siteMapNode>`das folgende Markup hinzu:

[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials arbeiten mit Daten im Batch Modus.

![Die Site Übersicht enthält jetzt Einträge für Lernprogramme zum Arbeiten mit Daten in Batch Verarbeitung.](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Abbildung 3**: die Site Übersicht enthält jetzt Einträge für Lernprogramme zum Arbeiten mit Daten im Batch Modus.

## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Schritt 2: Aktualisieren der Datenzugriffs Ebene zur Unterstützung von Datenbanktransaktionen

Wie bereits im ersten Tutorial, dem [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md), erläutert, besteht das typisierte DataSet in unserer dal aus DataTables und TableAdapters. Die DataTables enthalten Daten, während die TableAdapters die Funktionalität zum Lesen von Daten aus der Datenbank in die DataTables bereitstellen, um die Datenbank mit an den DataTables vorgenommenen Änderungen zu aktualisieren, usw. Beachten Sie, dass die TableAdapters zwei Muster zum Aktualisieren von Daten bereitstellen, die als Batch Update und DB-Direct bezeichnet werden. Beim Batch Aktualisierungs Muster wird dem TableAdapter ein DataSet, eine Datentabelle oder eine Auflistung von DataRows übermittelt. Diese Daten werden aufgelistet und für jede eingefügte, geänderte oder gelöschte Zeile, die `InsertCommand`, die `UpdateCommand`oder die `DeleteCommand` ausgeführt. Beim DB-Direct-Muster werden dem TableAdapter stattdessen die Werte der Spalten, die zum Einfügen, aktualisieren oder Löschen eines einzelnen Datensatzes erforderlich sind, übermittelt. Die direkte Daten Bank Muster Methode verwendet dann diese über gebenden Werte, um die entsprechende `InsertCommand`, `UpdateCommand`oder `DeleteCommand` Anweisung auszuführen.

Unabhängig vom verwendeten Aktualisierungs Muster werden von den automatisch generierten TableAdapters-Methoden keine Transaktionen verwendet. Standardmäßig wird jede vom TableAdapter ausgeführte INSERT-, Update-oder DELETE-Operation als einzelner einzelner Vorgang behandelt. Stellen Sie sich beispielsweise vor, dass das DB-Direct-Muster von Code in der BLL verwendet wird, um zehn Datensätze in die Datenbank einzufügen. Mit diesem Code wird die TableAdapter s-`Insert`-Methode zehnmal aufgerufen. Wenn die ersten fünf Einfügungen erfolgreich sind, aber der sechste Vorgang zu einer Ausnahme geführt hat, verbleiben die ersten fünf eingefügten Datensätze in der Datenbank. Analog gilt: Wenn das Batch Aktualisierungs Muster verwendet wird, um eingefügte, geänderte und gelöschte Zeilen in einer Datentabelle einzufügen, zu aktualisieren und zu löschen, wenn die ersten einzelnen Änderungen erfolgreich waren, aber eine spätere Änderung einen Fehler festgestellt hat, werden diese früheren Änderungen durchgeführt. abgeschlossen würde in der Datenbank verbleiben.

In bestimmten Szenarien möchten wir eine Atomizität über eine Reihe von Änderungen hinweg sicherstellen. Um dies zu erreichen, müssen Sie den TableAdapter manuell erweitern, indem Sie neue Methoden hinzufügen, die die `InsertCommand`, `UpdateCommand`und `DeleteCommand` en unter dem Rahmen einer Transaktion ausführen. Beim [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) haben wir uns mit der Verwendung von [partiellen Klassen](http://en.wikipedia.org/wiki/Partial_type) beschäftigt, um die Funktionalität der DataTables im typisierten DataSet zu erweitern. Diese Technik kann auch mit TableAdapters verwendet werden.

Das typisierte DataSet `Northwind.xsd` befindet sich im Ordner "`App_Code`" `DAL` Unterordner "". Erstellen Sie im Ordner "`DAL`" einen Unterordner mit dem Namen `TransactionSupport`, und fügen Sie eine neue Klassendatei mit dem Namen `ProductsTableAdapter.TransactionSupport.vb` hinzu (siehe Abbildung 4). Diese Datei enthält die partielle Implementierung der `ProductsTableAdapter`, die Methoden zum Ausführen von Datenänderungen mithilfe einer Transaktion enthält.

![Fügen Sie einen Ordner mit dem Namen Transaktionsunterstützung und eine Klassendatei mit dem Namen ProductsTableAdapter. transaktionsupport. vb hinzu.](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Abbildung 4**: Hinzufügen eines Ordners namens `TransactionSupport` und einer Klassendatei mit dem Namen `ProductsTableAdapter.TransactionSupport.vb`

Geben Sie in der `ProductsTableAdapter.TransactionSupport.vb`-Datei den folgenden Code ein:

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

Das `Partial`-Schlüsselwort in der Klassen Deklaration gibt dem Compiler an, dass die Elemente, die in hinzugefügt wurden, der `ProductsTableAdapter`-Klasse im `NorthwindTableAdapters`-Namespace hinzugefügt werden sollen. Notieren Sie sich die `Imports System.Data.SqlClient`-Anweisung am Anfang der Datei. Da der TableAdapter so konfiguriert wurde, dass er den SqlClient-Anbieter verwendet, wird intern ein [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) Objekt verwendet, um seine Befehle an die Datenbank auszugeben. Folglich müssen wir die `SqlTransaction`-Klasse verwenden, um die Transaktion zu starten und anschließend einen Commit für die Transaktion auszuführen oder einen Rollback auszuführen. Wenn Sie einen anderen Datenspeicher als Microsoft SQL Server verwenden, müssen Sie den entsprechenden Anbieter verwenden.

Diese Methoden stellen die Bausteine bereit, die zum Starten, Rollback und Commit einer Transaktion erforderlich sind. Sie sind als `Public`gekennzeichnet und ermöglichen die Verwendung innerhalb der `ProductsTableAdapter`, von einer anderen Klasse in der dal oder von einer anderen Ebene in der Architektur, wie z. b. der BLL. `BeginTransaction` öffnet die interne `SqlConnection` TableAdapter s (falls erforderlich), beginnt die Transaktion und weist Sie der `Transaction`-Eigenschaft zu und fügt die Transaktion den internen `SqlDataAdapter` s `SqlCommand` Objekten an. `CommitTransaction` und `RollbackTransaction` vor dem Schließen des internen `Rollback` Objekts die `Commit`-und `Connection` Methoden des `Transaction`-Objekts aufzurufen.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Schritt 3: Hinzufügen von Methoden zum Aktualisieren und Löschen von Daten im Rahmen einer Transaktion

Wenn diese Methoden abgeschlossen sind, können Sie `ProductsDataTable` oder der BLL, die eine Reihe von Befehlen ausführen, eine Reihe von Befehlen hinzufügen. Die folgende Methode verwendet das Batch Aktualisierungs Muster, um eine `ProductsDataTable` Instanz mithilfe einer Transaktion zu aktualisieren. Eine Transaktion wird gestartet, indem die `BeginTransaction`-Methode aufgerufen und dann ein `Try...Catch`-Block verwendet wird, um die Daten Änderungs Anweisungen auszugeben. Wenn der aufzurufende `Adapter` `Update` Methode eine Ausnahme auslöst, wird die Ausführung an den `catch` Block übertragen, in dem für die Transaktion ein Rollback ausgeführt wird und die Ausnahme erneut ausgelöst wird. Erinnern Sie sich daran, dass die `Update`-Methode das Batch Aktualisierungs Muster implementiert, indem Sie die Zeilen der angegebenen `ProductsDataTable` auflisten und die erforderlichen `InsertCommand`, `UpdateCommand`und `DeleteCommand` s ausführen. Wenn einer dieser Befehle zu einem Fehler führt, wird für die Transaktion ein Rollback ausgeführt, und die vorherigen Änderungen, die während der Lebensdauer der Transaktion vorgenommen wurden, werden rückgängig gemacht. Wenn die `Update`-Anweisung ohne Fehler abgeschlossen wird, wird die Transaktion vollständig committet.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Fügen Sie die `UpdateWithTransaction`-Methode über die partielle Klasse in `ProductsTableAdapter.TransactionSupport.vb`der `ProductsTableAdapter`-Klasse hinzu. Alternativ kann diese Methode der Business Logic Layer s `ProductsBLL`-Klasse mit einigen geringfügigen syntaktischen Änderungen hinzugefügt werden. Das Schlüsselwort `Me` in `Me.BeginTransaction()`, `Me.CommitTransaction()`und `Me.RollbackTransaction()` muss durch `Adapter` ersetzt werden (denken Sie daran, dass `Adapter` der Name einer Eigenschaft in `ProductsBLL` vom Typ `ProductsTableAdapter`ist).

Die `UpdateWithTransaction`-Methode verwendet das Batch Aktualisierungs Muster, aber eine Reihe von DB-Direct-aufrufen kann auch innerhalb des Gültigkeits Bereichs einer Transaktion verwendet werden, wie die folgende Methode zeigt. Die `DeleteProductsWithTransaction`-Methode akzeptiert als Eingabe eine `List(Of T)` vom Typ `Integer`, bei der es sich um die `ProductID` s handelt, die gelöscht werden sollen. Die-Methode initiiert die Transaktion über einen Aufruf von `BeginTransaction` und durchläuft dann im `Try`-Block die angegebene Liste, die das DB-Direct-Muster `Delete`-Methode für jeden `ProductID` Wert aufruft. Wenn einer der Aufrufe von `Delete` fehlschlägt, wird die Steuerung an den `Catch` Block übertragen, in dem für die Transaktion ein Rollback ausgeführt wird und die Ausnahme erneut ausgelöst wird. Wenn alle Aufrufe von `Delete` erfolgreich sind, wird für die Transaktion ein Commit ausgeführt. Fügen Sie der `ProductsBLL`-Klasse diese Methode hinzu.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Anwenden von Transaktionen auf mehrere TableAdapters

Der in diesem Tutorial untersuchte Transaktionscode ermöglicht, dass mehrere Anweisungen für die `ProductsTableAdapter` als atomarer Vorgang behandelt werden. Aber was geschieht, wenn mehrere Änderungen an unterschiedlichen Datenbanktabellen atomarisch ausgeführt werden müssen? Wenn Sie beispielsweise eine Kategorie löschen, möchten wir möglicherweise zuerst die aktuellen Produkte einer anderen Kategorie zuweisen. Diese beiden Schritte zum erneuten Zuweisen der Produkte und zum Löschen der Kategorie sollten als atomarer Vorgang ausgeführt werden. Die `ProductsTableAdapter` enthält jedoch nur Methoden zum Ändern der `Products` Tabelle, und die `CategoriesTableAdapter` enthält nur Methoden zum Ändern der `Categories` Tabelle. Wie kann eine Transaktion also beide TableAdapters umfassen?

Eine Möglichkeit ist das Hinzufügen einer Methode zum `CategoriesTableAdapter` mit dem Namen `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` und die Methode dazu, eine gespeicherte Prozedur aufzurufen, die die Produkte neu zuweist und die Kategorie innerhalb des Gültigkeits Bereichs einer Transaktion löscht, die in der gespeicherten Prozedur definiert ist. In einem zukünftigen Tutorial wird erläutert, wie Transaktionen in gespeicherten Prozeduren gestartet, übernommen und zurückgesetzt werden.

Eine andere Möglichkeit besteht darin, eine Hilfsklasse in der dal zu erstellen, die die `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` Methode enthält. Diese Methode erstellt eine Instanz des `CategoriesTableAdapter` und des `ProductsTableAdapter` und legt diese beiden TableAdapters `Connection` Eigenschaften auf dieselbe `SqlConnection` Instanz fest. An diesem Punkt initiiert entweder einer der beiden TableAdapters die Transaktion mit einem `BeginTransaction`. Die TableAdapters-Methoden zum erneuten Zuweisen der Produkte und zum Löschen der Kategorie werden in einem `Try...Catch` Block mit der Transaktion, für die ein Commit oder ein Rollback ausgeführt wurde, aufgerufen.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Schritt 4: Hinzufügen der`UpdateWithTransaction`Methode zur Geschäftslogik Schicht

In Schritt 3 haben wir der `ProductsTableAdapter` in der dal eine `UpdateWithTransaction` Methode hinzugefügt. Wir sollten der BLL eine entsprechende Methode hinzufügen. Obwohl die Darstellungs Schicht direkt zur dal aufrufen kann, um die `UpdateWithTransaction`-Methode aufzurufen, haben diese Tutorials eine mehrschichtige Architektur definiert, die die DAL von der Präsentationsschicht isoliert. Daher wird dieser Ansatz durch ihn fortgesetzt.

Öffnen Sie die Datei `ProductsBLL`-Klasse, und fügen Sie eine Methode mit dem Namen `UpdateWithTransaction` hinzu, die einfach die entsprechende dal-Methode aufruft. In `ProductsBLL`gibt es nun zwei neue Methoden: `UpdateWithTransaction`, die Sie soeben hinzugefügt haben, und `DeleteProductsWithTransaction`, das in Schritt 3 hinzugefügt wurde.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Diese Methoden schließen nicht das `DataObjectMethodAttribute`-Attribut ein, das den meisten anderen Methoden in der `ProductsBLL`-Klasse zugewiesen ist, da wir diese Methoden direkt aus den ASP.net Pages-Code-Behind-Klassen aufrufen. Beachten Sie, dass `DataObjectMethodAttribute` verwendet wird, um die Methoden zu markieren, die im Assistenten zum Konfigurieren von Datenquellen von ObjectDataSource und auf der Registerkarte (auswählen, aktualisieren, einfügen oder löschen) angezeigt werden sollen. Da in der GridView keine integrierte Unterstützung für das Bearbeiten oder Löschen von Batches vorhanden ist, müssen diese Methoden Programm gesteuert aufgerufen werden, anstatt den Code losen deklarativen Ansatz zu verwenden.

## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Schritt 5: atomisch Aktualisieren von Datenbankdaten von der Darstellungs Schicht

Um die Auswirkung zu veranschaulichen, die die Transaktion beim Aktualisieren eines Daten Satz Batches hat, können Sie eine Benutzeroberfläche erstellen, die alle Produkte in einer GridView auflistet und ein Schaltflächen-websteuer Element enthält, das, wenn Sie darauf klicken, die Produkte `CategoryID` Werte neu zuweist. Insbesondere wird die Neuzuweisung der Kategorie fortgesetzt, sodass den ersten mehreren Produkten ein gültiger `CategoryID` Wert zugewiesen wird, während anderen Personen gezielt ein nicht vorhandener `CategoryID` Wert zugewiesen wird. Wenn Sie versuchen, die Datenbank mit einem Produkt zu aktualisieren, dessen `CategoryID` nicht mit der `CategoryID`einer vorhandenen Kategorie identisch ist, tritt eine Verletzung der Fremdschlüssel Einschränkung auf, und es wird eine Ausnahme ausgelöst. In diesem Beispiel werden wir sehen, dass bei der Verwendung einer Transaktion die von der Foreign Key-Einschränkungs Verletzung ausgelöste Ausnahme dazu führt, dass die vorherigen gültigen `CategoryID` Änderungen zurückgesetzt werden. Wenn Sie jedoch keine Transaktion verwenden, bleiben die Änderungen an den ursprünglichen Kategorien erhalten.

Öffnen Sie zunächst die Seite `Transactions.aspx` im Ordner `BatchData`, und ziehen Sie eine GridView aus der Toolbox auf den Designer. Legen Sie den `ID` auf `Products`, und binden Sie ihn aus dem Smarttag an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren Sie ObjectDataSource so, dass die Daten aus der `ProductsBLL` Klasse `GetProducts` Methode abgerufen werden. Dabei handelt es sich um eine schreibgeschützte GridView. Legen Sie daher die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest, und klicken Sie auf Fertigstellen.

[![konfigurieren Sie ObjectDataSource für die Verwendung der GetProducts-Methode der productbll-Klasse.](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Abbildung 5**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL` Class s `GetProducts`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Abbildung 6**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen erstellt Visual Studio boundfields und ein CheckBoxField für die Product Data-Felder. Entfernen Sie alle diese Felder außer `ProductID`, `ProductName`, `CategoryID`und `CategoryName`, und benennen Sie `ProductName` Eigenschaften `CategoryName` und `HeaderText` boundfields in Product bzw. Category um. Aktivieren Sie im Smarttags die Option Paging aktivieren. Nachdem Sie diese Änderungen vorgenommen haben, sollten das deklarative Markup der GridView-und ObjectDataSource-Datei wie folgt aussehen:

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Fügen Sie als nächstes drei Schaltflächen-websteuer Elemente oberhalb der GridView hinzu. Legen Sie die Text-Eigenschaft der ersten Schaltfläche auf Aktualisierungs Raster, die zweiten s zum Ändern von Kategorien (mit Transaction) und die dritte zum Ändern von Kategorien (ohne Transaktion) fest.

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

An diesem Punkt sollte das Designansicht in Visual Studio ähnlich dem Screenshot in Abbildung 7 aussehen.

[![die Seite eine GridView-und drei Button-websteuer Elemente enthält.](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Abbildung 7**: die Seite enthält ein GridView-Steuerelement und drei Schaltflächen-websteuer Elemente ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))

Erstellen Sie Ereignishandler für jede der drei Schaltflächen `Click` Ereignisse, und verwenden Sie den folgenden Code:

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Die Schaltfläche "Aktualisieren" `Click` Ereignis Handlers bindet die Daten einfach an die GridView weiter, indem Sie die `Products` GridView s `DataBind`-Methode aufrufen.

Der zweite Ereignishandler weist die Produkte `CategoryID` en neu zu und verwendet die neue Transaktions Methode aus der BLL, um die Datenbankaktualisierungen unter dem Rahmen einer Transaktion auszuführen. Beachten Sie, dass jedes Produkt s `CategoryID` willkürlich auf denselben Wert wie die `ProductID`festgelegt ist. Dies funktioniert gut für die ersten Produkte, da diese Produkte `ProductID` Werte aufweisen, die den gültigen `CategoryID` en zugeordnet werden. Wenn die `ProductID` s jedoch zu groß werden, wird diese zufällige Überschneidung von `ProductID` s und `CategoryID` s nicht mehr angewendet.

Der dritte `Click`-Ereignishandler aktualisiert die Produkte `CategoryID` s auf die gleiche Weise, sendet das Update jedoch mithilfe der Standard `Update`-Methode der `ProductsTableAdapter` s an die Datenbank. Diese `Update` Methode umschließt nicht die Reihe von Befehlen in einer Transaktion, sodass diese Änderungen vor dem ersten gefundenen Fremdschlüssel-Einschränkungs Fehler beibehalten werden.

Um dieses Verhalten zu veranschaulichen, besuchen Sie diese Seite über einen Browser. Zunächst sollten Sie die erste Seite mit den Daten sehen, wie in Abbildung 8 dargestellt. Klicken Sie anschließend auf die Schaltfläche Kategorien ändern (mit Transaktion). Dies führt zu einem Postback und versucht, alle Produkte `CategoryID` Werte zu aktualisieren, führt jedoch zu einer Verletzung der Fremdschlüssel Einschränkung (siehe Abbildung 9).

[![die Produkte in einer kostenpflichtigen GridView angezeigt werden.](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Abbildung 8**: die Produkte werden in einer kostenpflichtigen GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))

[![erneute Zuweisung der Kategorien führt zu einer Verletzung der Fremdschlüssel Einschränkung.](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Abbildung 9**: Neuzuweisen der Kategorien führt zu einer Verletzung der Fremdschlüssel Einschränkung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))

Klicken Sie nun auf die Schaltfläche zurück, und klicken Sie dann auf die Schaltfläche Raster aktualisieren. Beim Aktualisieren der Daten sollte genau dieselbe Ausgabe wie in Abbildung 8 dargestellt angezeigt werden. Das heißt, obwohl einige der Produkte `CategoryID` s in zulässige Werte geändert und in der Datenbank aktualisiert wurden, wurde ein Rollback ausgeführt, als die Foreign Key-Einschränkungs Verletzung aufgetreten ist.

Klicken Sie jetzt auf die Schaltfläche Kategorien ändern (ohne Transaktion). Dies führt zu dem gleichen Verstoß gegen den Fremdschlüssel Einschränkungs Verstoß (siehe Abbildung 9), aber diesmal werden die Produkte, deren `CategoryID` Werte in einen gültigen Wert geändert wurden, nicht zurückgesetzt. Klicken Sie auf die Schaltfläche zurück, und klicken Sie dann auf die Schaltfläche Raster aktualisieren. Wie in Abbildung 10 gezeigt, wurden die `CategoryID` s der ersten acht Produkte neu zugewiesen. In Abbildung 8 hatte chanz. b. einen `CategoryID` von 1, aber in Abbildung 10 wurde er zu 2 neu zugewiesen.

[![die CategoryID-Werte einiger Produkte aktualisiert wurden, während andere nicht](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Abbildung 10**: einige Produkte `CategoryID` Werte wurden aktualisiert, während andere nicht waren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))

## <a name="summary"></a>Zusammenfassung

Standardmäßig wrappen die TableAdapter s-Methoden nicht die ausgeführten Daten Bank Anweisungen innerhalb des Bereichs einer Transaktion, aber mit wenigen Aufgaben können wir Methoden hinzufügen, die eine Transaktion erstellen, Committe und zurücksetzen. In diesem Tutorial haben wir drei Methoden in der `ProductsTableAdapter`-Klasse erstellt: `BeginTransaction`, `CommitTransaction`und `RollbackTransaction`. Wir haben gesehen, wie diese Methoden zusammen mit einem `Try...Catch`-Block verwendet werden, um eine Reihe von Daten Änderungs Anweisungen atomarisch zu machen. Insbesondere haben wir die `UpdateWithTransaction` Methode in der `ProductsTableAdapter`erstellt, die das Batch Aktualisierungs Muster verwendet, um die erforderlichen Änderungen an den Zeilen eines angegebenen `ProductsDataTable`auszuführen. Wir haben auch die `DeleteProductsWithTransaction`-Methode zur `ProductsBLL`-Klasse in der BLL hinzugefügt, die eine `List` von `ProductID` Werten als Eingabe akzeptiert und die DB-Direct Pattern-Methode `Delete` für jede `ProductID`aufruft. Beide Methoden beginnen, indem Sie eine Transaktion erstellen und dann die Daten Änderungs Anweisungen in einem `Try...Catch`-Block ausführen. Wenn eine Ausnahme auftritt, wird ein Rollback für die Transaktion ausgeführt; andernfalls wird ein Commit ausgeführt.

In Schritt 5 wurden die Auswirkungen von transaktionalen Batch Updates im Vergleich zu Batch Updates veranschaulicht, die die Verwendung einer Transaktion vernachlässigt haben. In den nächsten drei Tutorials bauen wir auf der Grundlage in diesem Tutorial auf und erstellen Benutzeroberflächen zum Ausführen von Batch Aktualisierungen, Löschungen und Einfügungen.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Beibehalten der Daten Bank Konsistenz mit Transaktionen](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Verwalten von Transaktionen in SQL Server gespeicherten Prozeduren](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Einfache Transaktionen: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [Transaktionscope und DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Verwenden von Oracle Database Transaktionen in .net](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Dave Gardner, Hilton giesanow und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](batch-inserting-cs.md)
> [Weiter](batch-updating-vb.md)
