---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Aktualisieren des TableAdapter für die Verwendung von Joins (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Beim Arbeiten mit einer Datenbank ist es üblich, Daten anzufordern, die auf mehrere Tabellen verteilt sind. Zum Abrufen von Daten aus zwei verschiedenen Tabellen können wir beide...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c94baa99b126cdd24d69afc3d02bfe8b069419b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78427527"
---
# <a name="updating-the-tableadapter-to-use-joins-vb"></a>Aktualisieren des TableAdapter-Steuerelements für die Verwendung von Verknüpfungen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) oder [PDF herunterladen](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Beim Arbeiten mit einer Datenbank ist es üblich, Daten anzufordern, die auf mehrere Tabellen verteilt sind. Zum Abrufen von Daten aus zwei verschiedenen Tabellen können wir entweder eine korrelierte Unterabfrage oder eine Join-Operation verwenden. In diesem Tutorial vergleichen Sie korrelierte Unterabfragen und die joinsyntax, bevor Sie sehen, wie Sie einen TableAdapter erstellen, der einen Join in der Haupt Abfrage enthält.

## <a name="introduction"></a>Einführung

Bei relationalen Datenbanken sind die Daten, mit denen wir arbeiten möchten, häufig auf mehrere Tabellen verteilt. Wenn Sie z. b. Produktinformationen anzeigen, möchten wir wahrscheinlich jeden Produkt die entsprechenden Kategorien und Lieferanten Namen auflisten. Die `Products` Tabelle enthält `CategoryID`-und `SupplierID` Werte, aber die tatsächlichen Kategorie-und Lieferanten Namen befinden sich jeweils in den Tabellen `Categories` und `Suppliers`.

Zum Abrufen von Informationen aus einer anderen verknüpften Tabelle können wir entweder *korrelierte Unterabfragen* oder `JOIN`*s*verwenden. Eine korrelierte Unterabfrage ist eine `SELECT` Abfrage, die auf Spalten in der äußeren Abfrage verweist. Beispielsweise wurden im Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) zwei korrelierte Unterabfragen in der Haupt Abfrage `ProductsTableAdapter` s verwendet, um die Kategorien-und Lieferanten Namen für jedes Produkt zurückzugeben. Ein `JOIN` ist ein SQL-Konstrukt, mit dem verknüpfte Zeilen aus zwei verschiedenen Tabellen zusammengeführt werden. Wir haben eine `JOIN` im Tutorial [Abfragen von Daten mit dem SqlDataSource-Steuer](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) Element verwendet, um neben den einzelnen Produkten Kategorieinformationen anzuzeigen.

Der Grund für die Verwendung von `JOIN` s mit TableAdapters liegt darin, dass der TableAdapter s-Assistent für die automatische Generierung entsprechender `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen verwendet wird. Genauer gesagt: Wenn die Haupt Abfrage des TableAdapter s `JOIN` s enthält, kann der TableAdapter die Ad-hoc-SQL-Anweisungen oder gespeicherten Prozeduren für die `InsertCommand`-, `UpdateCommand`-und `DeleteCommand` Eigenschaften nicht automatisch erstellen.

In diesem Tutorial vergleichen und vergleichen Sie korrelierte Unterabfragen und `JOIN` en kurz vor dem Erstellen eines TableAdapter, der `JOIN` s in der Haupt Abfrage enthält.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Vergleichen und vergleichen korrelierter Unterabfragen und`JOIN` s

Beachten Sie, dass die `ProductsTableAdapter`, die im ersten Lernprogramm im `Northwind` DataSet erstellt werden, korrelierte Unterabfragen verwendet, um die entsprechenden Kategorie-und Lieferanten Namen für die einzelnen Produkte zurückzuführen. Die Haupt Abfrage `ProductsTableAdapter` s ist unten dargestellt.

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Die beiden korrelierten Unterabfragen-`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` und `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` sind `SELECT` Abfragen, die einen einzelnen Wert pro Produkt als zusätzliche Spalte in der Spaltenliste der äußeren `SELECT` Anweisung zurückgeben.

Alternativ kann ein `JOIN` verwendet werden, um die einzelnen Lieferanten und Kategorien amen der Produkte zurückzugeben. Die folgende Abfrage gibt dieselbe Ausgabe wie die obige aus, verwendet jedoch `JOIN` s anstelle von Unterabfragen:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Mit einem `JOIN` werden die Datensätze aus einer Tabelle mit Datensätzen aus einer anderen Tabelle basierend auf einigen Kriterien zusammengeführt. In der obigen Abfrage weist z. b. die `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` SQL Server an, jeden Produktdaten Satz mit dem Kategoriedatensatz zusammenzuführen, dessen `CategoryID` Wert mit dem `CategoryID` Wert des Produkts übereinstimmt. Mit dem zusammengeführten Ergebnis können wir mit den entsprechenden Kategoriefeldern für jedes Produkt arbeiten (z. b. `CategoryName`).

> [!NOTE]
> `JOIN` s werden häufig beim Abfragen von Daten aus relationalen Datenbanken verwendet. Wenn Sie mit der `JOIN`-Syntax noch nicht vertraut sind oder ein bisschen auf die Verwendung von Pinseln müssen, empfiehlt es sich, das [SQL Join-Tutorial](http://www.w3schools.com/sql/sql_join.asp) bei [W3 Schools](http://www.w3schools.com/)zu verwenden. Lesen Sie außerdem die Abschnitte [`JOIN` Grundlagen](https://msdn.microsoft.com/library/ms191517.aspx) und [Unterabfragen](https://msdn.microsoft.com/library/ms189575.aspx) der [SQL-Online](https://msdn.microsoft.com/library/ms130214.aspx)Dokumentation.

Da `JOIN` s und korrelierte Unterabfragen zum Abrufen von verknüpften Daten aus anderen Tabellen verwendet werden können, bleiben viele Entwickler an den Köpfen und Fragen sich, welcher Ansatz verwendet werden soll. Alle SQL-Gurus, mit denen ich gesprochen habe, haben ungefähr das gleiche gesagt, dass es keine Rolle spielt, da SQL Server ungefähr identische Ausführungspläne erzeugt. Ihre Ratschläge sind dann die Methode, mit der Sie und Ihr Team am bequemsten sind. Es ist zu beachten, dass diese Experten nach dem Durchlaufen dieser Ratschläge sofort Ihre Präferenz `JOIN` s über korrelierte Unterabfragen ausgedrückt haben.

Beim Aufbau einer Datenzugriffs Ebene mithilfe typisierter Datasets funktionieren die Tools besser, wenn Unterabfragen verwendet werden. Der TableAdapter s-Assistent generiert die entsprechenden `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen nicht automatisch, wenn die Haupt Abfrage `JOIN` s enthält. diese Anweisungen werden jedoch automatisch generiert, wenn korrelierte Unterabfragen verwendet werden.

Um dieses Manko zu untersuchen, erstellen Sie ein temporäres typisiertes DataSet im `~/App_Code/DAL` Ordner. Wählen Sie im TableAdapter-Konfigurations-Assistenten aus, dass Sie Ad-hoc-SQL-Anweisungen verwenden, und geben Sie die folgende `SELECT` Abfrage ein (siehe Abbildung 1):

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]

[![geben Sie eine Haupt Abfrage ein, die Joins enthält.](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Abbildung 1**: Geben Sie eine Haupt Abfrage ein, die `JOIN` s enthält ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image3.png)).

Standardmäßig erstellt der TableAdapter automatisch `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen basierend auf der Haupt Abfrage. Wenn Sie auf die Schaltfläche Erweitert klicken, sehen Sie, dass diese Funktion aktiviert ist. Trotz dieser Einstellung kann der TableAdapter die Anweisungen `INSERT`, `UPDATE`und `DELETE` nicht erstellen, da die Haupt Abfrage einen `JOIN`enthält.

![Geben Sie eine Haupt Abfrage ein, die Joins enthält.](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Abbildung 2**: Geben Sie eine Haupt Abfrage ein, die `JOIN` s enthält.

Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen. An diesem Punkt enthält Ihr Dataset s-Designer einen einzelnen TableAdapter mit einer Datentabelle mit Spalten für jedes der Felder, die in der Spaltenliste der `SELECT` Abfrage s zurückgegeben werden. Dies schließt die `CategoryName` und `SupplierName`ein, wie in Abbildung 3 gezeigt.

![Die Datentabelle enthält eine Spalte für jedes Feld, das in der Spaltenliste zurückgegeben wird.](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Abbildung 3**: die Datentabelle enthält eine Spalte für jedes Feld, das in der Spaltenliste zurückgegeben wird.

Obwohl die Datentabelle über die entsprechenden Spalten verfügt, fehlen im TableAdapter Werte für die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand`. Um dies zu bestätigen, klicken Sie im Designer auf den TableAdapter und dann auf den Eigenschaftenfenster. Dort sehen Sie, dass die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` auf (keine) festgelegt sind.

[![die Eigenschaften InsertCommand, UpdateCommand und DeleteCommand auf (keine) festgelegt.](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Abbildung 4**: die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` sind auf (keine) festgelegt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))

Um dieses Problem zu umgehen, können Sie die SQL-Anweisungen und Parameter für die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` über den Eigenschaftenfenster manuell bereitstellen. Alternativ könnten Sie zunächst die Haupt Abfrage des TableAdapter s so konfigurieren, dass *Sie keine `JOIN`* s enthält. Dies ermöglicht es, dass die Anweisungen `INSERT`, `UPDATE`und `DELETE` für uns automatisch generiert werden. Nachdem Sie den Assistenten abgeschlossen haben, können wir die TableAdapter s-`SelectCommand` vom Eigenschaftenfenster manuell aktualisieren, sodass Sie die `JOIN`-Syntax enthält.

Obwohl dieser Ansatz funktioniert, ist er bei der Verwendung von Ad-hoc-SQL-Abfragen sehr anfällig, da jedes Mal, wenn die Haupt Abfrage des TableAdapter s über den Assistenten neu konfiguriert wird, die automatisch generierten `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen neu erstellt werden. Dies bedeutet, dass alle Anpassungen, die wir später vorgenommen haben, verloren gehen würden, wenn wir mit der rechten Maustaste auf den TableAdapter klicken, im Kontextmenü Konfigurieren auswählen und den Assistenten erneut abgeschlossen haben.

Die `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen des TableAdapter s, die automatisch generiert werden, sind glücklicherweise auf Ad-hoc-SQL-Anweisungen beschränkt. Wenn der TableAdapter gespeicherte Prozeduren verwendet, können Sie die `SelectCommand`, die `InsertCommand`, `UpdateCommand`oder `DeleteCommand` gespeicherten Prozeduren anpassen und den TableAdapter-Konfigurations-Assistenten erneut ausführen, ohne zu befürchten, dass die gespeicherten Prozeduren geändert werden.

In den nächsten Schritten erstellen wir einen TableAdapter, der anfänglich eine Haupt Abfrage verwendet, in der alle `JOIN` en ausgelassen werden, damit die entsprechenden gespeicherten Prozeduren INSERT, Update und DELETE automatisch generiert werden. Anschließend aktualisieren wir die `SelectCommand`, sodass ein `JOIN` verwendet, das zusätzliche Spalten aus verknüpften Tabellen zurückgibt. Schließlich erstellen wir eine zugehörige Klasse für die Geschäftslogik Schicht und veranschaulichen die Verwendung des TableAdapter auf einer ASP.NET-Webseite.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Schritt 1: Erstellen des TableAdapters mit einer vereinfachten Haupt Abfrage

Für dieses Tutorial fügen wir einen TableAdapter und eine stark typisierte Datentabelle für die `Employees` Tabelle im `NorthwindWithSprocs` Dataset hinzu. Die `Employees` Tabelle enthält ein `ReportsTo` Feld, in dem die `EmployeeID` des Mitarbeiter-Managers angegeben wurde. Beispielsweise hat der Mitarbeiter Anne dodsvalue den `ReportTo` Wert 5, der `EmployeeID` von Steven Buchanan. Folglich berichtet er an Steven, seinen Vorgesetzten. Zusammen mit der Berichterstellung für die einzelnen Mitarbeiter `ReportsTo` Wert können wir auch den Namen des Vorgesetzten abrufen. Dies kann mithilfe einer `JOIN`erreicht werden. Wenn Sie jedoch eine `JOIN` verwenden, wenn der TableAdapter erstmalig erstellt wird, verhindert der Assistent automatisch die entsprechenden INSERT-, Update-und DELETE-Funktionen. Daher wird zunächst ein TableAdapter erstellt, dessen Haupt Abfrage keine `JOIN` s enthält. Anschließend aktualisieren wir in Schritt 2 die gespeicherte Prozedur der Haupt Abfrage, um den Namen der Manager-s über eine `JOIN`abzurufen.

Öffnen Sie zunächst das `NorthwindWithSprocs` DataSet im Ordner `~/App_Code/DAL`. Klicken Sie mit der rechten Maustaste auf den Designer, wählen Sie im Kontextmenü die Option hinzufügen aus, und wählen Sie das Menü Element TableAdapter aus. Dadurch wird der TableAdapter-Konfigurations-Assistent gestartet. Wie in Abbildung 5 dargestellt, lassen Sie den Assistenten neue gespeicherte Prozeduren erstellen, und klicken Sie auf Weiter. Ein Aktualisierungs Programm für das Erstellen neuer gespeicherter Prozeduren mit dem TableAdapter-Assistenten finden Sie unter [Erstellen neuer gespeicherter Prozeduren für das Lernprogramm für typisierte Datasets](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

[Wählen Sie ![die Option neue gespeicherte Prozeduren erstellen.](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Abbildung 5**: Wählen Sie die Option neue gespeicherte Prozeduren erstellen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))

Verwenden Sie die folgende `SELECT`-Anweisung für die Haupt Abfrage des TableAdapter s:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Da diese Abfrage keine `JOIN` s enthält, erstellt der TableAdapter-Assistent automatisch gespeicherte Prozeduren mit entsprechenden `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen sowie eine gespeicherte Prozedur zum Ausführen der Haupt Abfrage.

Mit dem folgenden Schritt können wir den TableAdapter-gespeicherten Prozeduren benennen. Verwenden Sie die Namen `Employees_Select`, `Employees_Insert`, `Employees_Update`und `Employees_Delete`, wie in Abbildung 6 dargestellt.

[![die gespeicherten Prozeduren der TableAdapter-Prozedur.](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Abbildung 6**: Benennen der TableAdapter s-gespeicherten Prozeduren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))

Im letzten Schritt werden wir aufgefordert, die TableAdapter s-Methoden zu benennen. Verwenden Sie `Fill` und `GetEmployees` als Methodennamen. Stellen Sie außerdem sicher, dass das Kontrollkästchen zum Senden von Updates direkt an die Datenbank (GenerateDBDirectMethods) aktiviert ist.

[![benennen Sie die TableAdapter s-Methoden Fill und GetEmployees.](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Abbildung 7**: Benennen der TableAdapter s-Methoden `Fill` und `GetEmployees` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))

Nehmen Sie sich nach Abschluss des Assistenten einen Moment Zeit, um die gespeicherten Prozeduren in der Datenbank zu untersuchen. Es sollten vier neue angezeigt werden: `Employees_Select`, `Employees_Insert`, `Employees_Update`und `Employees_Delete`. Überprüfen Sie als nächstes die `EmployeesDataTable`, und `EmployeesTableAdapter` Sie soeben erstellt haben. Die Datentabelle enthält eine Spalte für jedes Feld, das von der Haupt Abfrage zurückgegeben wird. Klicken Sie auf den TableAdapter, und wechseln Sie dann zum Eigenschaftenfenster. Dort sehen Sie, dass die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` ordnungsgemäß konfiguriert sind, um die entsprechenden gespeicherten Prozeduren aufzurufen.

[![der TableAdapter Einfügungs-, Aktualisierungs-und Löschfunktionen enthält.](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Abbildung 8**: der TableAdapter umfasst Einfüge-, Aktualisierungs-und Löschfunktionen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))

Nachdem die gespeicherten Prozeduren INSERT, Update und DELETE automatisch erstellt und die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` ordnungsgemäß konfiguriert wurden, können wir die gespeicherte Prozedur `SelectCommand` s anpassen, um zusätzliche Informationen zu den einzelnen Vorgesetzten der Mitarbeiter zurückzugeben. Insbesondere müssen Sie die gespeicherte Prozedur `Employees_Select` aktualisieren, um eine `JOIN` zu verwenden, und die `FirstName` und `LastName` Werte des Managers zurückgeben. Nachdem die gespeicherte Prozedur aktualisiert wurde, müssen wir die Datentabelle aktualisieren, damit Sie diese zusätzlichen Spalten enthält. Wir behandeln diese beiden Aufgaben in den Schritten 2 und 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Schritt 2: Anpassen der gespeicherten Prozedur zum Einschließen einer`JOIN`

Beginnen Sie mit dem Server-Explorer, und starten Sie den Ordner "gespeicherte Prozeduren" der Northwind-Datenbank, und öffnen Sie die gespeicherte Prozedur `Employees_Select`. Wenn diese gespeicherte Prozedur nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie aktualisieren aus. Aktualisieren Sie die gespeicherte Prozedur so, dass Sie eine `LEFT JOIN` verwendet, um den vor-und Nachnamen des Vorgesetzten zurückzugeben:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Nachdem Sie die `SELECT`-Anweisung aktualisiert haben, speichern Sie die Änderungen, indem Sie im Menü Datei auf Speichern `Employees_Select`klicken. Alternativ dazu können Sie auf das Symbol Speichern auf der Symbolleiste klicken oder STRG + S drücken. Nachdem Sie die Änderungen gespeichert haben, klicken Sie mit der rechten Maustaste auf die gespeicherte Prozedur `Employees_Select` im Server-Explorer, und wählen Sie ausführen aus. Dadurch wird die gespeicherte Prozedur ausgeführt, und die Ergebnisse werden im Ausgabefenster angezeigt (siehe Abbildung 9).

[![werden die Ergebnisse gespeicherter Prozeduren in der Ausgabefenster angezeigt.](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Abbildung 9**: die Ergebnisse gespeicherter Prozeduren werden in der Ausgabefenster angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))

## <a name="step-3-updating-the-datatable-s-columns"></a>Schritt 3: Aktualisieren der Spalten der Datentabelle

An diesem Punkt gibt die gespeicherte Prozedur `Employees_Select` `ManagerFirstName` und `ManagerLastName` Werte zurück, aber der `EmployeesDataTable` fehlen diese Spalten. Diese fehlenden Spalten können auf eine von zwei Arten der Datentabelle hinzugefügt werden:

- Klicken Sie **manuell** mit der rechten Maustaste auf die Datentabelle im DataSet-Designer, und wählen Sie im Menü hinzufügen die Option Spalte aus. Anschließend können Sie die Spalte benennen und deren Eigenschaften entsprechend festlegen.
- **Automatisch** : der TableAdapter-Konfigurations-Assistent aktualisiert die Spalten der Datentabelle, um die Felder widerzuspiegeln, die von der gespeicherten Prozedur `SelectCommand` zurückgegeben werden. Wenn Sie Ad-hoc-SQL-Anweisungen verwenden, werden vom Assistenten auch die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` entfernt, da das `SelectCommand` nun eine `JOIN`enthält. Wenn Sie jedoch gespeicherte Prozeduren verwenden, bleiben diese Befehls Eigenschaften intakt.

Wir haben das manuelle Hinzufügen von Daten Tabellenspalten in vorherigen Tutorials untersucht, einschließlich [Master/Detail mithilfe einer Auflistungs Liste von Master Datensätzen mit einem DataList-DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) und [Hochladen von Dateien](../working-with-binary-files/uploading-files-vb.md). dieser Prozess wird im nächsten Tutorial ausführlicher erläutert. In diesem Tutorial können Sie jedoch die automatische Vorgehensweise über den TableAdapter-Konfigurations-Assistenten verwenden.

Klicken Sie zunächst mit der rechten Maustaste auf die `EmployeesTableAdapter`, und wählen Sie im Kontextmenü die Option konfigurieren aus. Dadurch wird der TableAdapter-Konfigurations-Assistent geöffnet, der die gespeicherten Prozeduren auflistet, die zum auswählen, einfügen, aktualisieren und Löschen verwendet werden, zusammen mit den zugehörigen Rückgabe Werten und Parametern (sofern vorhanden). Abbildung 10 zeigt den Assistenten. Hier sehen Sie, dass die gespeicherte Prozedur `Employees_Select` nun die Felder `ManagerFirstName` und `ManagerLastName` zurückgibt.

[![der Assistent die aktualisierte Spaltenliste für die gespeicherte Prozedur Employees_Select anzeigt](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Abbildung 10**: der Assistent zeigt die aktualisierte Spaltenliste für die gespeicherte Prozedur `Employees_Select` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))

Beenden Sie den Assistenten, indem Sie auf Fertigstellen klicken. Wenn Sie zum DataSet-Designer zurückkehren, enthält die `EmployeesDataTable` zwei zusätzliche Spalten: `ManagerFirstName` und `ManagerLastName`.

[![die Mitarbeiterdaten Tabelle zwei neue Spalten enthält.](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Abbildung 11**: die `EmployeesDataTable` enthält zwei neue Spalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))

Um zu veranschaulichen, dass die aktualisierte gespeicherte Prozedur `Employees_Select` in Kraft ist und die Einfüge-, Aktualisierungs-und Löschfunktionen des TableAdapters weiterhin funktionsfähig sind, können Sie eine Webseite erstellen, die es Benutzern ermöglicht, Mitarbeiter anzuzeigen und zu löschen. Bevor wir eine solche Seite erstellen, müssen wir jedoch zunächst eine neue Klasse in der Geschäftslogik Schicht erstellen, um mit Mitarbeitern aus dem `NorthwindWithSprocs` DataSet zu arbeiten. In Schritt 4 erstellen wir eine `EmployeesBLLWithSprocs`-Klasse. In Schritt 5 wird diese Klasse von einer ASP.NET-Seite verwendet.

## <a name="step-4-implementing-the-business-logic-layer"></a>Schritt 4: Implementieren der Geschäftslogik Ebene

Erstellen Sie eine neue Klassendatei im Ordner "`~/App_Code/BLL`" mit dem Namen `EmployeesBLLWithSprocs.vb`. Diese Klasse imitiert die Semantik der vorhandenen `EmployeesBLL`-Klasse, nur diese neue stellt weniger Methoden bereit und verwendet das `NorthwindWithSprocs` DataSet (anstelle des `Northwind` Datasets). Fügen Sie der `EmployeesBLLWithSprocs` -Klasse den folgenden Code hinzu.

[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

Die `EmployeesBLLWithSprocs` Class s `Adapter`-Eigenschaft gibt eine Instanz der `EmployeesTableAdapter``NorthwindWithSprocs` Datasets zurück. Diese wird von der `GetEmployees`-und `DeleteEmployee`-Methode der-Klasse verwendet. Die `GetEmployees`-Methode ruft die `EmployeesTableAdapter` s der entsprechenden `GetEmployees`-Methode auf, die die gespeicherte Prozedur `Employees_Select` aufruft und die Ergebnisse in einer `EmployeeDataTable`auffüllt. Die `DeleteEmployee`-Methode ruft auf ähnliche Weise die `EmployeesTableAdapter` s `Delete`-Methode auf, die die gespeicherte Prozedur `Employees_Delete` aufruft.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Schritt 5: Arbeiten mit den Daten in der Darstellungs Schicht

Wenn die `EmployeesBLLWithSprocs`-Klasse abgeschlossen ist, können wir mit den Mitarbeiterdaten über eine ASP.NET Seite wieder arbeiten. Öffnen Sie die Seite `JOINs.aspx` im Ordner `AdvancedDAL`, und ziehen Sie eine GridView aus der Toolbox auf den Designer, und legen Sie die Eigenschaft `ID` auf `Employees`fest. Binden Sie anschließend aus dem GridView s-Smarttag das Raster an ein neues ObjectDataSource-Steuerelement mit dem Namen `EmployeesDataSource`.

Konfigurieren Sie ObjectDataSource so, dass die `EmployeesBLLWithSprocs`-Klasse verwendet wird, und stellen Sie auf der Registerkarte auswählen und löschen sicher, dass die Methoden `GetEmployees` und `DeleteEmployee` in den Dropdown Listen ausgewählt sind. Klicken Sie auf Fertigstellen, um die Konfiguration von ObjectDataSource abzuschließen.

[![Konfigurieren von ObjectDataSource für die Verwendung der Klasse Mitarbeiter bllwithsprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Abbildung 12**: Konfigurieren von ObjectDataSource für die Verwendung der `EmployeesBLLWithSprocs`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))

[![die ObjectDataSource die GetEmployees-Methode und die DeleteEmployee-Methode verwenden.](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Abbildung 13**: Verwenden von ObjectDataSource zum `GetEmployees` und `DeleteEmployee` Methoden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))

Visual Studio fügt der GridView für jede der `EmployeesDataTable` s-Spalten ein BoundField-Element hinzu. Entfernen Sie alle diese boundfields-Objekte mit Ausnahme von `Title`, `LastName`, `FirstName`, `ManagerFirstName`und `ManagerLastName`, und benennen Sie die `HeaderText` Eigenschaften für die letzten vier boundfields in Nachname, Vorname, Manager-Vorname und Nachname des Vorgesetzten um.

Um Benutzern das Löschen von Mitarbeitern auf dieser Seite zu gestatten, müssen Sie zwei Dinge tun. Weisen Sie zunächst die GridView zum Bereitstellen von Löschfunktionen an, indem Sie die Option zum Aktivieren der Löschung von Ihrem Smarttag aktivieren. Ändern Sie anschließend die ObjectDataSource s `OldValuesParameterFormatString`-Eigenschaft von dem Wert, der vom ObjectDataSource-Assistenten (`original_{0}`) festgelegt wurde, auf den Standardwert (`{0}`). Nachdem Sie diese Änderungen vorgenommen haben, sollten das deklarative Markup von GridView und ObjectDataSource in etwa wie folgt aussehen:

[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Testen Sie die Seite, indem Sie Sie über einen Browser aufrufen. Wie in Abbildung 14 gezeigt, werden auf der Seite die einzelnen Mitarbeiter und deren Name (sofern vorhanden) aufgeführt.

[![der Join in der Employees_Select gespeicherten Prozedur den Namen des Vorgesetzten zurück.](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Abbildung 14**: der `JOIN` in der `Employees_Select` gespeicherten Prozedur gibt den Namen des Vorgesetzten zurück ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))

Wenn Sie auf die Schaltfläche Löschen klicken, wird der Lösch Workflow gestartet, der in der Ausführung der gespeicherten Prozedur `Employees_Delete` gipfiert. Allerdings schlägt die versuchte `DELETE` Anweisung in der gespeicherten Prozedur aufgrund einer Verletzung der FOREIGN KEY-Einschränkung fehl (siehe Abbildung 15). Insbesondere verfügt jeder Mitarbeiter über einen oder mehrere Datensätze in der `Orders` Tabelle, wodurch der Löschvorgang fehlschlägt.

[![Löschen eines Mitarbeiters, der über entsprechende Aufträge verfügt, führt zu einer Verletzung der FOREIGN KEY-Einschränkung.](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Abbildung 15**: Löschen eines Mitarbeiters, der über entsprechende Bestellungen verfügt, führt zu einer Verletzung der Fremdschlüssel Einschränkung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))

Damit ein Mitarbeiter gelöscht werden kann, können Sie folgende Aktionen ausführen:

- Aktualisieren Sie die FOREIGN KEY-Einschränkung in Kaskadierte Löschvorgänge.
- Löschen Sie die Datensätze manuell aus der `Orders` Tabelle für die Mitarbeiter, die Sie löschen möchten, oder
- Aktualisieren Sie die gespeicherte Prozedur `Employees_Delete`, um zuerst die zugehörigen Datensätze aus der `Orders` Tabelle zu löschen, bevor Sie den `Employees` Datensatz löschen. Diese Vorgehensweise wurde im Tutorial [Verwenden vorhandener gespeicherter Prozeduren für das typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) erläutert.

Ich lasse dies als Übung für den Reader aus.

## <a name="summary"></a>Zusammenfassung

Beim Arbeiten mit relationalen Datenbanken kommt es häufig vor, dass Abfragen Ihre Daten aus mehreren verknüpften Tabellen abrufen. Korrelierte Unterabfragen und `JOIN` s bieten zwei verschiedene Techniken für den Zugriff auf Daten aus verknüpften Tabellen in einer Abfrage. In vorherigen Tutorials haben wir am häufigsten korrelierte Unterabfragen verwendet, da der TableAdapter `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen für Abfragen, die `JOIN` s betreffen, nicht automatisch generieren kann. Diese Werte können bei Verwendung von Ad-hoc-SQL-Anweisungen manuell bereitgestellt werden, wenn der TableAdapter-Konfigurations-Assistent abgeschlossen ist.

Glücklicherweise haben TableAdapters, die mithilfe gespeicherter Prozeduren erstellt wurden, nicht denselben Platzhalter wie solche, die mithilfe von Ad-hoc-SQL-Anweisungen erstellt wurden. Daher ist es möglich, einen TableAdapter zu erstellen, dessen Haupt Abfrage eine `JOIN` bei der Verwendung gespeicherter Prozeduren verwendet. In diesem Tutorial haben Sie erfahren, wie Sie einen solchen TableAdapter erstellen. Wir haben mit einer `JOIN`losen `SELECT` Abfrage für die Haupt Abfrage des TableAdapter s begonnen, sodass die entsprechenden gespeicherten INSERT-, Update-und DELETE-Prozeduren automatisch erstellt werden. Nach Abschluss der Erstkonfiguration von TableAdapter wurde die gespeicherte Prozedur `SelectCommand` so erweitert, dass ein `JOIN` verwendet wird, und der TableAdapter-Konfigurations-Assistent wird erneut ausgeführt, um die `EmployeesDataTable` s-Spalten zu aktualisieren.

Beim erneuten Ausführen des TableAdapter-Konfigurations-Assistenten wurden die `EmployeesDataTable` Spalten automatisch aktualisiert, um die von der gespeicherten Prozedur `Employees_Select` zurückgegebenen Datenfelder widerzuspiegeln. Alternativ können wir diese Spalten der Datentabelle manuell hinzufügen. Im nächsten Tutorial wird erläutert, wie Sie der Datentabelle manuell Spalten hinzufügen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Hilton geisenow, David suru und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Weiter](adding-additional-datatable-columns-vb.md)
