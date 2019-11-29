---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Arbeiten mit berechneten Spalten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie eine Datenbanktabelle erstellen, können Sie mit Microsoft SQL Server eine berechnete Spalte definieren, deren Wert aus einem Ausdruck berechnet wird, der normalerweise refgen...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: e425d7363c2cdea6efb0ba51f3fc2b6a5330bf2a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74602859"
---
# <a name="working-with-computed-columns-vb"></a>Arbeiten mit berechneten Spalten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) oder [PDF herunterladen](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Wenn Sie eine Datenbanktabelle erstellen, können Sie mit Microsoft SQL Server eine berechnete Spalte definieren, deren Wert aus einem Ausdruck berechnet wird, der in der Regel auf andere Werte im selben Datenbankdaten Satz verweist. Diese Werte sind in der Datenbank schreibgeschützt, was bei der Arbeit mit TableAdapters besondere Überlegungen erfordert. In diesem Tutorial erfahren Sie, wie Sie die Herausforderungen der berechneten Spalten erfüllen.

## <a name="introduction"></a>Einführung

Microsoft SQL Server ermöglicht *[berechnete Spalten](https://msdn.microsoft.com/library/ms191250.aspx)* , bei denen es sich um Spalten handelt, deren Werte aus einem Ausdruck berechnet werden, der in der Regel auf die Werte aus anderen Spalten in derselben Tabelle verweist. Beispielsweise kann ein Zeit Verfolgungs Datenmodell eine Tabelle mit dem Namen `ServiceLog` mit Spalten enthalten, einschließlich `ServicePerformed`, `EmployeeID`, `Rate`und `Duration`. Während der Betrag pro Dienst Element (mit der Rate multipliziert mit der Dauer) über eine Webseite oder eine andere programmgesteuerte Schnittstelle berechnet werden kann, kann es nützlich sein, eine Spalte in die `ServiceLog` Tabelle mit dem Namen `AmountDue` einzuschließen, die diese Informationen gemeldet hat. Diese Spalte könnte als normale Spalte erstellt werden, Sie muss jedoch aktualisiert werden, wenn die Werte für `Rate` oder `Duration` Spalten geändert werden. Ein besserer Ansatz besteht darin, die `AmountDue` Spalte mithilfe des Ausdrucks `Rate * Duration`zu einer berechneten Spalte zu machen. Dies würde dazu führen, dass SQL Server automatisch den Wert der `AmountDue` Spalte berechnet, wenn in einer Abfrage darauf verwiesen wird.

Da der Wert einer berechneten Spalte durch einen Ausdruck bestimmt wird, sind solche Spalten schreibgeschützt und können daher keinen Werten in `INSERT`-oder `UPDATE` Anweisungen zugewiesen werden. Wenn berechnete Spalten jedoch Teil der Haupt Abfrage für einen TableAdapter sind, der Ad-hoc-SQL-Anweisungen verwendet, werden Sie automatisch in die automatisch generierten `INSERT`-und `UPDATE`-Anweisungen eingeschlossen. Folglich müssen die Tabellen `INSERT`-und `UPDATE` Abfragen sowie `InsertCommand`-und `UpdateCommand` Eigenschaften aktualisiert werden, um Verweise auf alle berechneten Spalten zu entfernen.

Eine Herausforderung bei der Verwendung berechneter Spalten mit einem TableAdapter, der Ad-hoc-SQL-Anweisungen verwendet, besteht darin, dass die TableAdapter s-`INSERT` und `UPDATE` Abfragen immer dann erneut generiert werden, wenn der TableAdapter-Konfigurations-Assistent abgeschlossen ist. Aus diesem Grund werden die berechneten Spalten, die manuell aus dem `INSERT` entfernt wurden, und `UPDATE` Abfragen erneut angezeigt, wenn der Assistent erneut ausgeführt wird. Obwohl TableAdapters, die gespeicherte Prozeduren verwenden, von dieser Brüchigkeit leiden, verfügen Sie über eigene Quirlen, die wir in Schritt 3 ansprechen werden.

In diesem Tutorial fügen wir der `Suppliers` Tabelle in der Northwind-Datenbank eine berechnete Spalte hinzu und erstellen dann einen entsprechenden TableAdapter, um mit dieser Tabelle und der berechneten Spalte zu arbeiten. Der TableAdapter verwendet gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen, damit unsere Anpassungen nicht verloren gehen, wenn der TableAdapter-Konfigurations-Assistent verwendet wird.

Legen Sie Los!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Schritt 1: Hinzufügen einer berechneten Spalte zur`Suppliers`Tabelle

Die Datenbank Northwind verfügt nicht über berechnete Spalten, daher müssen wir eine selbst hinzufügen. In diesem Tutorial fügen Sie der `Suppliers` Tabelle `FullContactName` eine berechnete Spalte hinzu, die den Namen des Kontakts, den Titel und das Unternehmen, für das Sie verwendet werden, im folgenden Format zurückgibt: `ContactName` (`ContactTitle`, `CompanyName`). Diese berechnete Spalte kann in Berichten verwendet werden, wenn Informationen zu Zulieferern angezeigt werden.

Öffnen Sie zunächst die Definition der `Suppliers` Tabelle, indem Sie mit der rechten Maustaste auf die `Suppliers` Tabelle im Server-Explorer klicken und im Kontextmenü die Option Tabellendefinition öffnen auswählen. Dadurch werden die Spalten der Tabelle und ihre Eigenschaften angezeigt, z. b. der Datentyp, ob Sie `NULL` s zulassen usw. Um eine berechnete Spalte hinzuzufügen, geben Sie zunächst den Namen der Spalte in die Tabellendefinition ein. Geben Sie als nächstes den Ausdruck in das Textfeld (Formel) im Abschnitt berechnete Spalten Angabe in der Spalte Eigenschaftenfenster ein (siehe Abbildung 1). Benennen Sie die berechnete Spalte `FullContactName`, und verwenden Sie den folgenden Ausdruck:

[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Beachten Sie, dass Zeichen folgen in SQL mithilfe des `+`-Operators verkettet werden können. Die `CASE`-Anweisung kann in einer herkömmlichen Programmiersprache wie eine bedingte verwendet werden. Im obigen Ausdruck kann die `CASE`-Anweisung wie folgt gelesen werden: Wenn `ContactTitle` nicht `NULL` ist, wird der `ContactTitle` Wert ausgegeben, der mit einem Komma verkettet ist. andernfalls wird nichts ausgegeben. Weitere Informationen zur Nützlichkeit der `CASE`-Anweisung finden Sie in [der Leistungsfähigkeit von SQL `CASE`-Anweisungen](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Anstatt hier eine `CASE`-Anweisung zu verwenden, können wir alternativ `ISNULL(ContactTitle, '')`verwenden. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) gibt *checkexpression* zurück, wenn es ungleich NULL ist; andernfalls wird " *replacementvalue*" zurückgegeben. Obwohl entweder `ISNULL` oder `CASE` in dieser Instanz funktionieren, gibt es komplexere Szenarios, in denen die Flexibilität der `CASE`-Anweisung nicht durch `ISNULL`abgeglichen werden kann.

Nachdem Sie diese berechnete Spalte hinzugefügt haben, sollte der Bildschirm wie der Screenshot in Abbildung 1 aussehen.

[![der Tabelle Suppliers eine berechnete Spalte mit dem Namen fullcontactname hinzufügen](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer berechneten Spalte mit dem Namen `FullContactName` zur `Suppliers` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image3.png))

Nachdem Sie die berechnete Spalte benannt und den Ausdruck eingegeben haben, speichern Sie die Änderungen in der Tabelle, indem Sie auf das Symbol Speichern auf der Symbolleiste klicken. Drücken Sie dazu STRG + S, oder klicken Sie auf das Menü Datei, und wählen Sie dann `Suppliers`speichern aus.

Wenn Sie die Tabelle speichern, sollten Sie die Server-Explorer aktualisieren, einschließlich der soeben hinzugefügten Spalte in der Spaltenliste der `Suppliers` Tabelle. Außerdem wird der in das Textfeld (Formel) eingegebene Ausdruck automatisch an einen entsprechenden Ausdruck angepasst, der unnötige Leerräume entfernt, Spaltennamen in eckige Klammern (`[]`) umgibt und Klammern enthält, um die Reihenfolge der Vorgänge explizit anzuzeigen:

[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Weitere Informationen zu berechneten Spalten in Microsoft SQL Server finden Sie in der [technischen Dokumentation](https://msdn.microsoft.com/library/ms191250.aspx). Sehen Sie sich auch die Vorgehens [Weise: Angeben berechneter Spalten](https://msdn.microsoft.com/library/ms188300.aspx) für eine schrittweise exemplarische Vorgehensweise zum Erstellen berechneter Spalten an.

> [!NOTE]
> Standardmäßig werden berechnete Spalten nicht physisch in der Tabelle gespeichert, sondern stattdessen jedes Mal neu berechnet, wenn in einer Abfrage auf Sie verwiesen wird. Durch Aktivieren des Kontrollkästchens ist persistent können Sie jedoch SQL Server anweisen, die berechnete Spalte physisch in der Tabelle zu speichern. Dadurch kann ein Index für die berechnete Spalte erstellt werden, wodurch die Leistung von Abfragen verbessert werden kann, die den berechneten Spaltenwert in ihren `WHERE`-Klauseln verwenden. Weitere Informationen finden Sie [unter Erstellen von Indizes für berechnete Spalten](https://msdn.microsoft.com/library/ms189292.aspx) .

## <a name="step-2-viewing-the-computed-column-s-values"></a>Schritt 2: Anzeigen der Werte der berechneten Spalte

Bevor wir mit der Arbeit auf der Datenzugriffs Ebene beginnen, kann es eine Minute dauern, die `FullContactName` Werte anzuzeigen. Klicken Sie im Server-Explorer mit der rechten Maustaste auf den `Suppliers` Tabellennamen, und wählen Sie im Kontextmenü Neue Abfrage aus. Dadurch wird ein Abfragefenster angezeigt, in dem Sie aufgefordert werden, die Tabellen auszuwählen, die in die Abfrage eingeschlossen werden sollen. Fügen Sie die `Suppliers` Tabelle hinzu, und klicken Sie auf Schließen Überprüfen Sie als nächstes die Spalten `CompanyName`, `ContactName`, `ContactTitle`und `FullContactName` aus der Tabelle Suppliers. Klicken Sie abschließend auf das rote Ausrufezeichen Symbol auf der Symbolleiste, um die Abfrage auszuführen und die Ergebnisse anzuzeigen.

Wie in Abbildung 2 gezeigt, enthalten die Ergebnisse `FullContactName`, das die `CompanyName`, `ContactName`und `ContactTitle` Spalten im Format `ContactName` (`ContactTitle`, `CompanyName`) auflistet.

[![fullcontactname das Format ContactName (ContactTitle, CompanyName) verwendet.](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Abbildung 2**: die `FullContactName` verwendet das Format `ContactName` (`ContactTitle`, `CompanyName`) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image6.png))

## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Schritt 3: Hinzufügen der`SuppliersTableAdapter`zur Datenzugriffs Ebene

Um mit den Lieferanteninformationen in unserer Anwendung arbeiten zu können, müssen wir zunächst einen TableAdapter und eine Datentabelle in unserer dal erstellen. Im Idealfall würde dies mithilfe der gleichen einfachen Schritte erreicht werden, die in den vorherigen Tutorials untersucht wurden. Beim Arbeiten mit berechneten Spalten werden jedoch einige Falten eingeführt, die eine Diskussion verdienen.

Wenn Sie einen TableAdapter verwenden, der Ad-hoc-SQL-Anweisungen verwendet, können Sie die berechnete Spalte einfach über den TableAdapter-Konfigurations-Assistenten in die Haupt Abfrage von TableAdapter einschließen. Dadurch werden jedoch automatisch `INSERT`-und `UPDATE`-Anweisungen generiert, die die berechnete Spalte enthalten. Wenn Sie versuchen, eine dieser Methoden auszuführen, kann ein `SqlException` mit der Meldung der Spalten Spalten *Name* nicht geändert werden, weil es sich entweder um eine berechnete Spalte handelt oder wenn das Ergebnis eines Union-Operators ausgelöst wird. Obwohl die `INSERT`-und `UPDATE`-Anweisung manuell über die Eigenschaften `InsertCommand` und `UpdateCommand` von TableAdapter) angepasst werden können, gehen diese Anpassungen immer verloren, wenn der TableAdapter-Konfigurations-Assistent erneut ausgeführt wird.

Aufgrund der Brüchigkeit von TableAdapters, die Ad-hoc-SQL-Anweisungen verwenden, empfiehlt es sich, beim Arbeiten mit berechneten Spalten gespeicherte Prozeduren zu verwenden. Wenn Sie vorhandene gespeicherte Prozeduren verwenden, konfigurieren Sie einfach den TableAdapter, wie im Tutorial [Verwenden vorhandener gespeicherter Prozeduren für das TableAdapters für typisierte Datasets](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) beschrieben. Wenn Sie mit dem TableAdapter-Assistenten die gespeicherten Prozeduren erstellt haben, ist es jedoch wichtig, dass Sie zunächst alle berechneten Spalten aus der Haupt Abfrage weglassen. Wenn Sie eine berechnete Spalte in die Haupt Abfrage einschließen, werden Sie vom TableAdapter-Konfigurations-Assistenten nach Abschluss des Abschlusses informiert, dass die entsprechenden gespeicherten Prozeduren nicht erstellt werden können. Kurz gesagt, wir müssen zunächst den TableAdapter mithilfe einer berechneten, Spalten freien Haupt Abfrage konfigurieren und dann die zugehörige gespeicherte Prozedur manuell aktualisieren, und der TableAdapter s-`SelectCommand`, um die berechnete Spalte einzubeziehen. Diese Vorgehensweise ähnelt der Vorgehensweise, die im Tutorial [Aktualisieren des TableAdapters für die Verwendung](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* verwendet wird.

Fügen Sie für dieses Tutorial einen neuen TableAdapter hinzu, und lassen Sie die gespeicherten Prozeduren für uns automatisch erstellen. Folglich müssen wir zunächst die berechnete Spalte `FullContactName` aus der Haupt Abfrage weglassen.

Öffnen Sie zunächst das `NorthwindWithSprocs` DataSet im Ordner `~/App_Code/DAL`. Klicken Sie mit der rechten Maustaste in den Designer, und wählen Sie im Kontextmenü einen neuen TableAdapter hinzufügen aus. Dadurch wird der TableAdapter-Konfigurations-Assistent gestartet. Geben Sie die Datenbank an, aus der Daten abgefragt werden sollen (`NORTHWNDConnectionString` `Web.config`), und klicken Sie auf weiter Da wir noch keine gespeicherten Prozeduren zum Abfragen oder Ändern der `Suppliers` Tabelle erstellt haben, aktivieren Sie die Option neue gespeicherte Prozeduren erstellen, damit Sie vom Assistenten für uns erstellt wird, und klicken Sie auf Weiter.

[Wählen Sie ![die Option neue gespeicherte Prozeduren erstellen.](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Abbildung 3**: Auswählen der Option "neue gespeicherte Prozeduren erstellen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image9.png))

Der nachfolgende Schritt fordert uns zur Haupt Abfrage auf. Geben Sie die folgende Abfrage ein, die die Spalten `SupplierID`, `CompanyName`, `ContactName`und `ContactTitle` für jeden Lieferanten zurückgibt. Beachten Sie, dass diese Abfrage die berechnete Spalte (`FullContactName`) absichtlich abschließt. Wir aktualisieren die entsprechende gespeicherte Prozedur, um diese Spalte in Schritt 4 einzubeziehen.

[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Nachdem Sie die Haupt Abfrage eingegeben und auf "weiter" geklickt haben, können wir die vier gespeicherten Prozeduren, die generiert werden, mit dem Assistenten benennen. Benennen Sie diese gespeicherten Prozeduren `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`und `Suppliers_Delete`, wie in Abbildung 4 veranschaulicht.

[![passen Sie die Namen der automatisch generierten gespeicherten Prozeduren an.](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Abbildung 4**: Anpassen der Namen der automatisch generierten gespeicherten Prozeduren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image12.png))

Im nächsten Schritt des Assistenten können wir die TableAdapter s-Methoden benennen und die Muster angeben, die für den Zugriff auf und das Aktualisieren von Daten verwendet werden. Lassen Sie alle drei Kontrollkästchen aktiviert, benennen Sie aber die `GetData`-Methode in `GetSuppliers`um. Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen.

[![die GetData-Methode in getsuppliers umbenennen](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Abbildung 5**: Umbenennen der `GetData` Methode in `GetSuppliers` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image15.png))

Nachdem Sie auf "Fertigstellen" geklickt haben, erstellt der Assistent die vier gespeicherten Prozeduren und fügt dem typisierten Dataset den TableAdapter und die entsprechende Datentabelle hinzu.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Schritt 4: einschließen der berechneten Spalte in die Haupt Abfrage des TableAdapter s

Wir müssen nun den in Schritt 3 erstellten TableAdapter und die Datentabelle aktualisieren, um die `FullContactName` berechnete Spalte einzubeziehen. Dieser Vorgang umfasst zwei Schritte:

1. Aktualisieren der gespeicherten Prozedur `Suppliers_Select`, um die berechnete Spalte `FullContactName` zurückzugeben, und
2. Aktualisieren der Datentabelle, um eine entsprechende `FullContactName` Spalte einzuschließen.

Navigieren Sie zunächst zum Server-Explorer und Drilldown in den Ordner gespeicherte Prozeduren. Öffnen Sie die gespeicherte Prozedur `Suppliers_Select`, und aktualisieren Sie die `SELECT` Abfrage so, dass Sie die `FullContactName` berechnete Spalte enthält:

[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Speichern Sie die Änderungen an der gespeicherten Prozedur, indem Sie auf das Symbol Speichern auf der Symbolleiste klicken. Drücken Sie dazu STRG + S, oder wählen Sie im Menü Datei die Option `Suppliers_Select` speichern aus.

Kehren Sie anschließend zum DataSet-Designer zurück, klicken Sie mit der rechten Maustaste auf die `SuppliersTableAdapter`, und wählen Sie im Kontextmenü die Option konfigurieren aus. Beachten Sie, dass die Spalte `Suppliers_Select` nun die Spalte `FullContactName` in der Datenspalten Auflistung enthält.

[![führen Sie den Konfigurations-Assistenten für TableAdapter s aus, um die Spalten der Datentabelle zu aktualisieren.](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Abbildung 6**: Ausführen des Konfigurations-Assistenten für TableAdapter s zum Aktualisieren der Spalten der Datentabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image18.png))

Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen. Dadurch wird dem `SuppliersDataTable`automatisch eine entsprechende Spalte hinzugefügt. Der TableAdapter-Assistent ist intelligent genug, um zu erkennen, dass die `FullContactName` Spalte eine berechnete Spalte und daher schreibgeschützt ist. Folglich wird die `ReadOnly`-Eigenschaft der Spalte auf `true`festgelegt. Um dies zu überprüfen, wählen Sie die Spalte aus der `SuppliersDataTable` aus, und navigieren Sie dann zum Eigenschaftenfenster (siehe Abbildung 7). Beachten Sie, dass die `FullContactName` Spalten `DataType` und `MaxLength` Eigenschaften ebenfalls entsprechend festgelegt werden.

[![die fullcontactname-Spalte als schreibgeschützt gekennzeichnet ist.](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Abbildung 7**: die `FullContactName` Spalte ist schreibgeschützt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image21.png))

## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Schritt 5: Hinzufügen einer`GetSupplierBySupplierID`Methode zum TableAdapter

In diesem Tutorial erstellen wir eine ASP.NET-Seite, auf der die Lieferanten in einem aktualisierbaren Raster angezeigt werden. In den vorherigen Tutorials haben wir einen einzelnen Datensatz von der Geschäftslogik Schicht aktualisiert, indem wir diesen Datensatz aus der dal als stark typisierte Datentabelle abrufen, seine Eigenschaften aktualisieren und dann die aktualisierte Datentabelle an die DAL zurücksenden, um die Änderungen an zu übermitteln. die Datenbank. Zum Ausführen dieses ersten Schritts: Abrufen des Datensatzes, der von der dal aktualisiert wird, müssen Sie zunächst der dal eine `GetSupplierBySupplierID(supplierID)`-Methode hinzufügen.

Klicken Sie mit der rechten Maustaste auf die `SuppliersTableAdapter` im Datasetentwurf, und wählen Sie im Kontextmenü die Option Abfrage hinzufügen aus. Lassen Sie den Assistenten wie in Schritt 3 beschrieben eine neue gespeicherte Prozedur für uns generieren, indem Sie die Option neue gespeicherte Prozedur erstellen auswählen (Weitere Informationen finden Sie in Abbildung 3). Da diese Methode einen Datensatz mit mehreren Spalten zurückgibt, geben Sie an, dass Sie eine SQL-Abfrage verwenden möchten, die eine SELECT-Abfrage ist, die Zeilen zurückgibt, und klicken Sie auf weiter

[![wählen Sie die Option Select What Returns Rows aus.](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Abbildung 8**: Auswählen der Option Select What Returns Rows ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image24.png))

Im nachfolgenden Schritt werden wir aufgefordert, die Abfrage für diese Methode zu verwenden. Geben Sie Folgendes ein, das dieselben Datenfelder zurückgibt wie die Haupt Abfrage, jedoch für einen bestimmten Lieferanten.

[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Im nächsten Bildschirm werden wir aufgefordert, die gespeicherte Prozedur zu benennen, die automatisch generiert wird. Benennen Sie diese gespeicherte Prozedur `Suppliers_SelectBySupplierID` und klicken Sie dann auf Weiter.

[![der gespeicherten Prozedur den Namen Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Abbildung 9**: Benennen der gespeicherten Prozedur `Suppliers_SelectBySupplierID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image27.png))

Schließlich werden Sie vom Assistenten aufgefordert, die Datenzugriffs Muster und Methodennamen einzugeben, die für den TableAdapter verwendet werden sollen. Lassen Sie beide Kontrollkästchen aktiviert, benennen Sie jedoch die Methoden `FillBy` und `GetDataBy` in `FillBySupplierID` bzw. `GetSupplierBySupplierID`um.

[![benennen Sie die TableAdapter-Methoden fillbysupplierid und getsupplierbysupplierid.](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Abbildung 10**: Benennen der TableAdapter-Methoden `FillBySupplierID` und `GetSupplierBySupplierID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image30.png))

Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen.

## <a name="step-6-creating-the-business-logic-layer"></a>Schritt 6: Erstellen der Geschäftslogik Ebene

Bevor wir eine ASP.NET-Seite erstellen, die die berechnete Spalte verwendet, die Sie in Schritt 1 erstellt haben, müssen wir zunächst die entsprechenden Methoden in der BLL hinzufügen. Unsere ASP.NET-Seite, die wir in Schritt 7 erstellen werden, ermöglicht Benutzern das Anzeigen und Bearbeiten von Lieferanten. Daher ist es erforderlich, dass unsere BLL mindestens eine Methode zur Verfügung stellt, um alle Lieferanten und eine andere zum Aktualisieren eines bestimmten Lieferanten zu erhalten.

Erstellen Sie eine neue Klassendatei mit dem Namen `SuppliersBLLWithSprocs` im Ordner `~/App_Code/BLL`, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Wie die anderen BLL-Klassen verfügt `SuppliersBLLWithSprocs` über eine `Protected` `Adapter`-Eigenschaft, die eine Instanz der `SuppliersTableAdapter`-Klasse zusammen mit zwei `Public` Methoden zurückgibt: `GetSuppliers` und `UpdateSupplier`. Die `GetSuppliers`-Methode ruft den von der entsprechenden `GetSupplier` Methode in der Datenzugriffs Ebene zurückgegebenen `SuppliersDataTable` auf und gibt diesen zurück. Die `UpdateSupplier`-Methode ruft Informationen über den bestimmten zu aktualisierenden Lieferanten mithilfe eines aufgerufenen der dal s `GetSupplierBySupplierID(supplierID)`-Methode ab. Anschließend werden die Eigenschaften `CategoryName`, `ContactName`und `ContactTitle` aktualisiert und die Änderungen an die Datenbank übergeben, indem die `Update`-Methode der Datenzugriffs Schicht aufgerufen und das geänderte `SuppliersRow` Objekt übergeben wird.

> [!NOTE]
> Mit Ausnahme von `SupplierID` und `CompanyName`lassen alle Spalten in der Tabelle Suppliers `NULL` Werte zu. Wenn die Parameter für die Übergabe `contactName` oder `contactTitle` `Nothing` sind, müssen wir die entsprechenden `ContactName`-und `ContactTitle`-Eigenschaften auf einen `NULL` Daten Bank Wert festlegen, indem Sie die `SetContactNameNull`-Methode bzw. die `SetContactTitleNull`-Methode verwenden.

## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Schritt 7: Arbeiten mit der berechneten Spalte aus der Präsentationsebene

Wenn die berechnete Spalte der `Suppliers`-Tabelle hinzugefügt wurde und die DAL und BLL entsprechend aktualisiert wurden, können wir eine ASP.NET-Seite erstellen, die mit der `FullContactName` berechneten Spalte funktioniert. Öffnen Sie zunächst die Seite `ComputedColumns.aspx` im Ordner `AdvancedDAL`, und ziehen Sie eine GridView aus der Toolbox auf den Designer. Legen Sie die Eigenschaft GridView s `ID` auf `Suppliers` fest, und binden Sie das Smarttag an eine neue ObjectDataSource mit dem Namen `SuppliersDataSource`. Konfigurieren Sie die ObjectDataSource so, dass Sie die `SuppliersBLLWithSprocs`-Klasse verwendet, die wir in Schritt 6 hinzugefügt haben, und klicken Sie

[![Konfigurieren von ObjectDataSource für die Verwendung der suppliersbllwithsprocs-Klasse](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Abbildung 11**: Konfigurieren von ObjectDataSource für die Verwendung der `SuppliersBLLWithSprocs`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image33.png))

In der `SuppliersBLLWithSprocs`-Klasse sind nur zwei Methoden definiert: `GetSuppliers` und `UpdateSupplier`. Stellen Sie sicher, dass diese beiden Methoden auf den Registerkarten auswählen und aktualisieren angegeben sind, und klicken Sie auf Fertigstellen, um die Konfiguration von ObjectDataSource abzuschließen.

Nach Abschluss des Assistenten zum Konfigurieren von Datenquellen wird von Visual Studio ein BoundField-Element für jedes der zurückgegebenen Datenfelder hinzugefügt. Entfernen Sie die `SupplierID` BoundField, und ändern Sie die `HeaderText` Eigenschaften der `CompanyName`, `ContactName`, `ContactTitle`und `FullContactName` boundfields in Company, Name, Title und Full Contact Name. Aktivieren Sie im Smarttags das Kontrollkästchen "Bearbeiten aktivieren", um die integrierten Funktionen der GridView-Funktion zu aktivieren.

Zusätzlich zum Hinzufügen von boundfields zur GridView bewirkt die Beendigung des Datenquellen-Assistenten auch, dass Visual Studio die ObjectDataSource s `OldValuesParameterFormatString`-Eigenschaft auf die ursprüngliche\_{0}festgelegt hat. Legen Sie diese Einstellung auf den Standardwert zurück, {0}.

Nachdem Sie diese Änderungen an GridView und ObjectDataSource vorgenommen haben, sollte Ihr deklaratives Markup in etwa wie folgt aussehen:

[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Besuchen Sie anschließend diese Seite über einen Browser. Wie in Abbildung 12 gezeigt, wird jeder Lieferant in einem Raster aufgelistet, das die `FullContactName` Spalte enthält, deren Wert einfach die Verkettung der anderen drei Spalten ist, die als `ContactName` (`ContactTitle`, `CompanyName`) formatiert sind.

[![werden die einzelnen Lieferanten im Raster aufgelistet.](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Abbildung 12**: jeder Lieferant ist im Raster aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image36.png))

Wenn Sie auf die Schaltfläche Bearbeiten für einen bestimmten Lieferanten klicken, wird ein Postback ausgelöst, und die Zeile wird in der Bearbeitungs Schnittstelle gerendert (siehe Abbildung 13) Die ersten drei Spalten werden in Ihrer Standard Bearbeitungs Schnittstelle dargestellt-ein TextBox-Steuerelement, dessen `Text`-Eigenschaft auf den Wert des Datenfelds festgelegt ist. Die `FullContactName` Spalte bleibt jedoch als Text erhalten. Beim Hinzufügen von boundfields zur GridView zum Abschluss des Assistenten zum Konfigurieren von Datenquellen wurde die Eigenschaft `FullContactName` BoundField s `ReadOnly` auf `True` festgelegt, da die `SuppliersDataTable`-Eigenschaft der entsprechenden `FullContactName` Spalte `ReadOnly` auf `True`festgelegt ist. Wie in Schritt 4 erwähnt, wurde die Eigenschaft `FullContactName` s `ReadOnly` auf `True` festgelegt, da der TableAdapter erkannt hat, dass es sich bei der Spalte um eine berechnete Spalte handelt.

[![die fullcontactname-Spalte kann nicht bearbeitet werden.](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Abbildung 13**: die `FullContactName` Spalte kann nicht bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](working-with-computed-columns-vb/_static/image39.png))

Aktualisieren Sie den Wert von mindestens einer der bearbeitbaren Spalten, und klicken Sie auf aktualisieren. Beachten Sie, dass der Wert der `FullContactName` s automatisch aktualisiert wird, um die Änderung widerzuspiegeln.

> [!NOTE]
> Die GridView verwendet derzeit boundfields für die bearbeitbaren Felder, was zur Standard Bearbeitungs Schnittstelle führt. Da das `CompanyName` Feld erforderlich ist, sollte es in ein TemplateField-Element konvertiert werden, das ein "Requirements dfieldvalidator" enthält. Ich lasse dies als Übung für den interessierten Reader aus. Ausführliche Anweisungen zum Konvertieren eines BoundField in ein TemplateField und zum Hinzufügen von Validierungs Steuerelementen finden [Sie unter Hinzufügen von Validierungs Steuerelementen zum Bearbeitungs-und einfügeschnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) -Tutorial.

## <a name="summary"></a>Summary

Beim Definieren des Schemas für eine Tabelle ermöglicht Microsoft SQL Server das einschließen berechneter Spalten. Dabei handelt es sich um Spalten, deren Werte anhand eines Ausdrucks berechnet werden, der in der Regel auf die Werte aus anderen Spalten im gleichen Datensatz verweist. Da die Werte für berechnete Spalten auf einem Ausdruck basieren, sind Sie schreibgeschützt und können nicht in einer `INSERT`-oder `UPDATE` Anweisung einem Wert zugewiesen werden. Dies führt zu Problemen, wenn eine berechnete Spalte in der Haupt Abfrage eines TableAdapter verwendet wird, der versucht, automatisch entsprechende `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen zu generieren.

In diesem Tutorial haben wir Techniken zum Umgehen der Probleme erläutert, die für berechnete Spalten auftreten. Insbesondere haben wir in unserem TableAdapter gespeicherte Prozeduren verwendet, um die Brüchigkeit zu überwinden, die in TableAdapters mit Ad-hoc-SQL-Anweisungen verankert ist. Wenn der TableAdapter-Assistent neue gespeicherte Prozeduren erstellt, ist es wichtig, dass die Haupt Abfrage zuerst berechnete Spalten auslassen, da ihre Anwesenheit verhindert, dass die gespeicherten Prozeduren für die Datenänderung generiert werden. Nachdem der TableAdapter anfänglich konfiguriert wurde, kann die `SelectCommand` gespeicherte Prozedur neu erstellt werden, um berechnete Spalten einzuschließen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Hilton geisenow und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](adding-additional-datatable-columns-vb.md)
> [Weiter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
