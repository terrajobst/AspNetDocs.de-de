---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Batch Löschung (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie mehrere Datenbankdaten Sätze in einem einzigen Vorgang löschen. Auf der Benutzeroberflächen Ebene bauen wir auf einer erweiterten GridView auf, die in einem früheren tut erstellt wurde...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0974a16764eee2ef03cf36b4b15f9ef41f99982b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78476457"
---
# <a name="batch-deleting-vb"></a>Löschen in Batches (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) oder [PDF herunterladen](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Erfahren Sie, wie Sie mehrere Datenbankdaten Sätze in einem einzigen Vorgang löschen. Auf der Benutzeroberflächen Ebene bauen wir auf einer erweiterten GridView auf, die in einem früheren Tutorial erstellt wurde. In der Datenzugriffs Ebene werden mehrere Löschvorgänge in eine Transaktion eingeschlossen, um sicherzustellen, dass alle Löschvorgänge erfolgreich sind oder für alle Löschungen ein Rollback ausgeführt wird.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](batch-updating-vb.md) wurde erläutert, wie eine Batch Bearbeitungs Schnittstelle mithilfe einer vollständig bearbeitbaren GridView erstellt wird. In Fällen, in denen Benutzer häufig viele Datensätze gleichzeitig bearbeiten, benötigt eine Batch Bearbeitungs Schnittstelle weitaus weniger Postbacks und Tastatur-zu-Maus-Kontextwechsel, wodurch die Effizienz der Endbenutzer verbessert wird. Dieses Verfahren ist auf ähnliche Weise nützlich für Seiten, bei denen Benutzer häufig viele Datensätze in einem go löschen.

Jeder Benutzer, der einen Online-e-Mail-Client verwendet hat, ist bereits mit einer der gängigsten Schnittstellen zum Löschen von Batches vertraut: ein Kontrollkästchen in jeder Zeile in einem Raster mit der entsprechenden Schaltfläche alle aktivierten Elemente löschen (siehe Abbildung 1). Dieses Lernprogramm ist recht kurz, da wir bereits alle harten arbeiten in vorherigen Tutorials unter Erstellen der webbasierten Schnittstelle und einer Methode zum Löschen einer Reihe von Datensätzen als einzelner atomarer Vorgang durchgeführt haben. Im Tutorial [Hinzufügen einer GridView-Spalte mit Kontrollkästchen](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) haben wir ein GridView-Objekt mit einer Spalte mit Kontrollkästchen erstellt, und im Tutorial zum [Umpacken von Daten Bank Änderungen in einer Transaktion](wrapping-database-modifications-within-a-transaction-vb.md) haben wir eine Methode in der BLL erstellt, die eine Transaktion zum Löschen einer `List<T>` von `ProductID` Werten verwenden würde. In diesem Tutorial erstellen wir unsere vorherigen Erfahrungen und führen Sie zusammen, um ein funktionierendes Beispiel für einen Batch Löschvorgang zu erstellen.

[![jede Zeile ein Kontrollkästchen enthält.](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Abbildung 1**: jede Zeile enthält ein Kontrollkästchen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-deleting-vb/_static/image2.png))

## <a name="step-1-creating-the-batch-deleting-interface"></a>Schritt 1: Erstellen der Schnittstelle zum Löschen von Batches

Da wir die Schnittstelle zum Löschen von Batches bereits im Tutorial [Hinzufügen einer GridView-Spalte mit Kontrollkästchen](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) erstellt haben, können wir Sie einfach in `BatchDelete.aspx` kopieren, anstatt Sie von Grund auf neu zu erstellen. Öffnen Sie zunächst die Seite `BatchDelete.aspx` im Ordner `BatchData` und die Seite `CheckBoxField.aspx` im Ordner `EnhancedGridView`. Wechseln Sie auf der Seite "`CheckBoxField.aspx`" zur Quell Ansicht, und kopieren Sie das Markup zwischen den `<asp:Content>` Tags, wie in Abbildung 2 dargestellt.

[![das deklarative Markup von CheckBoxField. aspx in die Zwischenablage kopieren.](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Abbildung 2**: Kopieren des deklarativen Markups der `CheckBoxField.aspx` in die Zwischenablage ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-deleting-vb/_static/image4.png))

Navigieren Sie als nächstes zur Quell Ansicht in `BatchDelete.aspx`, und fügen Sie den Inhalt der Zwischenablage in die `<asp:Content>` Tags ein. Kopieren Sie außerdem den Code aus der Code Behind-Klasse in `CheckBoxField.aspx.vb` in die Code-Behind-Klasse in `BatchDelete.aspx.vb` (der `DeleteSelectedProducts` Schaltflächen `Click` Ereignishandler, die `ToggleCheckState`-Methode und die `Click`-Ereignishandler für die Schaltflächen `CheckAll` und `UncheckAll`). Nachdem Sie den Inhalt kopiert haben, sollte die Code Behind-Klasse der `BatchDelete.aspx` Page s den folgenden Code enthalten:

[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Nehmen Sie sich nach dem Kopieren des deklarativen Markups und des Quellcodes einen Moment Zeit, um `BatchDelete.aspx` zu testen, indem Sie ihn über einen Browser anzeigen. Es sollte eine GridView angezeigt werden, in der die ersten zehn Produkte in einer GridView aufgelistet sind, wobei jede Zeile den Namen, die Kategorie und den Preis des Produkts zusammen mit einem Kontrollkästchen auflistet. Es sollten drei Schaltflächen vorhanden sein: alle überprüfen, alle deaktivieren und ausgewählte Produkte löschen. Wenn Sie auf die Schaltfläche Alle überprüfen klicken, werden alle Kontrollkästchen aktiviert, während alle Kontrollkästchen deaktiviert werden. Wenn Sie auf ausgewählte Produkte löschen klicken, wird eine Meldung angezeigt, in der die `ProductID` Werte der ausgewählten Produkte aufgeführt sind. die Produkte werden jedoch nicht gelöscht.

[![die Schnittstelle von CheckBoxField. aspx in batchlösch. aspx verschoben wurde.](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Abbildung 3**: die Schnittstelle aus `CheckBoxField.aspx` wurde in `BatchDeleting.aspx` verschoben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-deleting-vb/_static/image6.png))

## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Schritt 2: Löschen der überprüften Produkte mithilfe von Transaktionen

Wenn die Schnittstelle zum Löschen des Batches erfolgreich in `BatchDeleting.aspx`kopiert wurde, besteht nur noch die Aktualisierung des Codes, sodass die Schaltfläche ausgewählte Produkte löschen die überprüften Produkte mithilfe der Methode `DeleteProductsWithTransaction` in der `ProductsBLL` Klasse löscht. Diese Methode, die im Tutorial zum [Umpacken von Daten Bank Änderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-vb.md) hinzugefügt wird, akzeptiert als Eingabe eine `List(Of T)` `ProductID` Werte und löscht alle entsprechenden `ProductID` innerhalb des Gültigkeits Bereichs einer Transaktion.

Der `DeleteSelectedProducts` Schaltfläche s `Click` Ereignishandler verwendet derzeit die folgende `For Each` Schleife zum Durchlaufen der einzelnen GridView-Zeilen:

[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Für jede Zeile wird auf das websteuer Element für den `ProductSelector` CheckBox Programm gesteuert verwiesen. Wenn diese Option aktiviert ist, wird die Zeile s `ProductID` aus der `DataKeys` Auflistung abgerufen, und die `DeleteResults` Bezeichnung s `Text` Eigenschaft wird aktualisiert, um eine Meldung zu enthalten, die angibt, dass die Zeile zum Löschen ausgewählt wurde.

Der obige Code löscht keine Datensätze, da der aufkommentierung der `ProductsBLL` Klasse `Delete`-Methode auskommentiert wird. Wenn diese Lösch Logik angewendet werden soll, würde der Code die Produkte löschen, jedoch nicht innerhalb einer atomaren Operation. Das heißt, wenn die ersten Löschvorgänge in der Sequenz erfolgreich waren, aber eine spätere fehlgeschlagen ist (möglicherweise aufgrund einer Verletzung der Fremdschlüssel Einschränkung), wird eine Ausnahme ausgelöst, aber bereits gelöschte Produkte werden gelöscht.

Um die Atomizität sicherzustellen, müssen wir stattdessen die `ProductsBLL` Class s `DeleteProductsWithTransaction`-Methode verwenden. Da diese Methode eine Liste mit `ProductID` Werten akzeptiert, müssen wir diese Liste zuerst aus dem Raster kompilieren und Sie dann als Parameter übergeben. Zuerst erstellen wir eine Instanz eines `List(Of T)` vom Typ `Integer`. Innerhalb der `For Each` Schleife müssen die ausgewählten Produkte `ProductID` Werte dieser `List(Of T)`hinzugefügt werden. Nach der Schleife müssen diese `List(Of T)` an die `ProductsBLL` Class-Klasse `DeleteProductsWithTransaction`-Methode übermittelt werden. Aktualisieren Sie die `DeleteSelectedProducts` Schaltfläche s `Click` Ereignishandler durch den folgenden Code:

[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

Der aktualisierte Code erstellt einen `List(Of T)` vom Typ `Integer` (`productIDsToDelete`) und füllt ihn mit den zu löschenden `ProductID` Werten. Wenn nach der `For Each`-Schleife mindestens ein Produkt ausgewählt ist, wird die `ProductsBLL` Class s `DeleteProductsWithTransaction`-Methode aufgerufen und an diese Liste übermittelt. Außerdem wird die `DeleteResults` Bezeichnung angezeigt, und die Daten werden an die GridView-Ansicht zurückgebracht (sodass die neu gelöschten Datensätze nicht mehr als Zeilen im Raster angezeigt werden).

Abbildung 4 zeigt die GridView, nachdem eine Reihe von Zeilen zum Löschen ausgewählt wurde. In Abbildung 5 wird der Bildschirm unmittelbar nach dem Klicken auf die Schaltfläche ausgewählte Produkte löschen angezeigt. Beachten Sie, dass in Abbildung 5 die `ProductID` Werte der gelöschten Datensätze in der Bezeichnung unterhalb der GridView angezeigt werden und diese Zeilen nicht mehr in der GridView angezeigt werden.

[![die ausgewählten Produkte gelöscht werden.](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Abbildung 4**: die ausgewählten Produkte werden gelöscht ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-deleting-vb/_static/image8.png))

[![der ProductID-Werte gelöschter Produkte werden unterhalb der GridView aufgeführt.](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Abbildung 5**: die gelöschten Produkte `ProductID` Werte werden unterhalb der GridView aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-deleting-vb/_static/image10.png))

> [!NOTE]
> Fügen Sie zum Testen der `DeleteProductsWithTransaction` Method s Atomicity manuell einen Eintrag für ein Produkt in der `Order Details` Tabelle hinzu, und versuchen Sie dann, das Produkt (zusammen mit anderen) zu löschen. Wenn Sie versuchen, das Produkt mit einer zugeordneten Bestellung zu löschen, erhalten Sie eine Verletzung der Fremdschlüssel Einschränkung, aber beachten Sie, dass für die anderen ausgewählten Produkt Löschungen ein Rollback ausgeführt wird.

## <a name="summary"></a>Zusammenfassung

Das Erstellen einer Schnittstelle zum Löschen von Batches umfasst das Hinzufügen einer GridView-Spalte mit einer Spalte mit Kontrollkästchen und einem Schaltflächen-websteuer Element, mit dem alle ausgewählten Zeilen als einzelner atomarer Vorgang gelöscht werden. In diesem Tutorial haben wir eine solche Schnittstelle erstellt, indem wir die Arbeit in zwei vorherigen Tutorials zusammengefasst, [eine GridView-Spalte mit Kontrollkästchen hinzugefügt](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) und [Daten Bank Änderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-vb.md)Umtragen. Im ersten Tutorial haben wir eine GridView mit einer Spalte mit Kontrollkästchen erstellt, und in der letzteren Zeit haben wir eine Methode in der BLL implementiert, bei der eine `List(Of T)` `ProductID` Werte übertragen wurde, die alle innerhalb des Gültigkeits Bereichs einer Transaktion gelöscht wurden.

Im nächsten Tutorial erstellen wir eine Schnittstelle zum Ausführen von Batch Einfügungen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Hilton giesanow und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](batch-updating-vb.md)
> [Weiter](batch-inserting-vb.md)
