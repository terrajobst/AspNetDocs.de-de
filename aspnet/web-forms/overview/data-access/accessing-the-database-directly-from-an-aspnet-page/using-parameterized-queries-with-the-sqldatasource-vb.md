---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Verwenden parametrisierter Abfragen mit SqlDataSource (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird der Blick auf das SqlDataSource-Steuerelement weitergeleitet, und es wird beschrieben, wie parametrisierte Abfragen definiert werden. Die Parameter können mit "decla..." angegeben werden.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 19b93ff6c0878ae6ed546d347cafef95fd2a01e6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598049"
---
# <a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Verwenden von parametrisierten Abfragen mit dem SqlDataSource-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) oder [PDF herunterladen](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> In diesem Tutorial wird der Blick auf das SqlDataSource-Steuerelement weitergeleitet, und es wird beschrieben, wie parametrisierte Abfragen definiert werden. Die Parameter können sowohl deklarativ als auch Programm gesteuert angegeben werden und können aus mehreren Speicherorten (z. b. QueryString, Sitzungszustand, anderen Steuerelementen usw.) abgerufen werden.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde erläutert, wie das SqlDataSource-Steuerelement verwendet wird, um Daten direkt aus einer Datenbank abzurufen. Mithilfe des Assistenten zum Konfigurieren von Datenquellen könnten wir die Datenbank auswählen und dann entweder die Spalten auswählen, die aus einer Tabelle oder Sicht zurückgegeben werden sollen. Geben Sie eine benutzerdefinierte SQL-Anweisung ein. oder verwenden Sie eine gespeicherte Prozedur. Unabhängig davon, ob Sie Spalten aus einer Tabelle oder Sicht auswählen oder eine benutzerdefinierte SQL-Anweisung eingeben, wird der `SelectCommand`-Eigenschaft des SqlDataSource-Steuer Elements die resultierende Ad-hoc-SQL-`SELECT`-Anweisung zugewiesen. dabei handelt es sich um eine `SELECT`-Anweisung, die ausgeführt wird, wenn die SqlDataSource s-`Select()`-Methode aufgerufen wird

In den SQL `SELECT`-Anweisungen, die in den vorherigen Tutorial s verwendet wurden, fehlten `WHERE` Klauseln. In einer `SELECT`-Anweisung kann die `WHERE`-Klausel verwendet werden, um die zurückgegebenen Ergebnisse einzuschränken. Wenn Sie z. b. die Namen von Produkten anzeigen möchten, die mehr als $50,00 enthalten, können wir die folgende Abfrage verwenden:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

In der Regel werden die in einer `WHERE`-Klausel verwendeten Werte durch eine externe Quelle, z. b. einen QueryString-Wert, eine Sitzungs Variable oder eine Benutzereingabe von einem websteuer Element auf der Seite, bestimmt. Im Idealfall werden solche Eingaben durch die Verwendung von *Parametern*angegeben. Bei Microsoft SQL Server werden Parameter mit `@parameterName`bezeichnet, wie in:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

SqlDataSource unterstützt parametrisierte Abfragen, sowohl für `SELECT`-Anweisungen als auch für die Anweisungen `INSERT`, `UPDATE`und `DELETE`. Darüber hinaus können die Parameterwerte automatisch aus einer Vielzahl von Quellen abgerufen werden: QueryString, Sitzungs Status, Steuerelemente auf der Seite usw. oder können Programm gesteuert zugewiesen werden. In diesem Tutorial erfahren Sie, wie Sie parametrisierte Abfragen definieren und die Parameterwerte sowohl deklarativ als auch Programm gesteuert angeben.

> [!NOTE]
> Im vorherigen Tutorial haben wir die ObjectDataSource verglichen, bei der es sich um die ersten 46-Tutorials mit der SqlDataSource handelt, die Ihre konzeptionellen Ähnlichkeiten notieren. Diese Ähnlichkeiten sind auch auf-Parameter ausgedehnt. Die ObjectDataSource s-Parameter, die den Eingabe Parametern für die Methoden in der Geschäftslogik Schicht zugeordnet sind. Mit SqlDataSource werden die Parameter direkt in der SQL-Abfrage definiert. Beide Steuerelemente verfügen über Auflistungen von Parametern für Ihre `Select()`-, `Insert()`-, `Update()`-und `Delete()`-Methoden, und beide können diese Parameterwerte aus vordefinierten Quellen (QueryString-Werte, Sitzungsvariablen usw.) füllen oder Programm gesteuert zugewiesen werden.

## <a name="creating-a-parameterized-query"></a>Erstellen einer parametrisierten Abfrage

Der Assistent zum Konfigurieren von Datenquellen von SqlDataSource Control s bietet drei Möglichkeiten zum Definieren des Befehls, der zum Abrufen von Datenbankdaten Sätzen ausgeführt werden soll:

- Durch Auswählen der Spalten aus einer vorhandenen Tabelle oder Sicht
- Durch Eingabe einer benutzerdefinierten SQL-Anweisung oder
- Durch Auswählen einer gespeicherten Prozedur

Beim Auswählen von Spalten aus einer vorhandenen Tabelle oder Sicht müssen die Parameter für die `WHERE`-Klausel über das Dialogfeld `WHERE` Klausel hinzufügen angegeben werden. Wenn Sie jedoch eine benutzerdefinierte SQL-Anweisung erstellen, können Sie die Parameter direkt in die `WHERE`-Klausel eingeben (wobei Sie `@parameterName` verwenden, um die einzelnen Parameter anzugeben). Eine [gespeicherte Prozedur](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) besteht aus einer oder mehreren SQL-Anweisungen, und diese Anweisungen können parametrisiert werden. Die in den SQL-Anweisungen verwendeten Parameter müssen jedoch als Eingabeparameter an die gespeicherte Prozedur übermittelt werden.

Da das Erstellen einer parametrisierten Abfrage davon abhängt, wie die SqlDataSource s-`SelectCommand` angegeben wird, sehen Sie sich die folgenden drei Ansätze an. Öffnen Sie zunächst die Seite `ParameterizedQueries.aspx` im Ordner `SqlDataSource`, ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox auf den Designer, und legen Sie dessen `ID` auf `Products25BucksAndUnderDataSource`fest. Klicken Sie anschließend auf den Link Datenquelle konfigurieren des Smarttags für Steuerelemente. Wählen Sie die zu verwendende Datenbank aus (`NORTHWINDConnectionString`), und klicken Sie auf Weiter.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Schritt 1: Hinzufügen einer`WHERE`-Klausel beim Auswählen der Spalten aus einer Tabelle oder Sicht

Wenn Sie die Daten auswählen, die aus der Datenbank mit dem SqlDataSource-Steuerelement zurückgegeben werden sollen, können Sie mit dem Assistenten zum Konfigurieren von Datenquellen einfach die Spalten auswählen, die aus einer vorhandenen Tabelle oder Sicht zurückgegeben werden sollen (siehe Abbildung 1). Dadurch wird automatisch eine SQL `SELECT`-Anweisung erstellt, die an die Datenbank gesendet wird, wenn die SqlDataSource s `Select()`-Methode aufgerufen wird. Wählen Sie wie im vorherigen Tutorial in der Dropdown Liste die Tabelle Products aus, und überprüfen Sie die Spalten `ProductID`, `ProductName`und `UnitPrice`.

[![die Spalten auszuwählen, die aus einer Tabelle oder Sicht zurückgegeben werden sollen.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Abbildung 1**: Auswählen der Spalten, die aus einer Tabelle oder Sicht zurückgegeben werden sollen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))

Wenn Sie eine `WHERE`-Klausel in die `SELECT` Anweisung einschließen möchten, klicken Sie auf die Schaltfläche `WHERE`, die das Dialogfeld `WHERE` Klausel hinzufügen öffnet (siehe Abbildung 2). Um einen Parameter hinzuzufügen, um die von der `SELECT` Abfrage zurückgegebenen Ergebnisse einzuschränken, wählen Sie zuerst die Spalte aus, nach der die Daten gefiltert werden sollen. Wählen Sie als nächstes den Operator aus, der zum Filtern verwendet werden soll (=, &lt;, &lt;=, &gt;usw.). Wählen Sie schließlich die Quelle des Werts des Parameters s aus, z. b. aus dem QueryString-oder Sitzungszustand. Nachdem Sie den Parameter konfiguriert haben, klicken Sie auf die Schaltfläche hinzufügen, um Sie in die `SELECT` Abfrage einzufügen.

Geben Sie in diesem Beispiel nur die Ergebnisse zurück, bei denen der `UnitPrice` Wert kleiner oder gleich $25,00 ist. Wählen Sie aus der Dropdown Liste Spalte `UnitPrice` aus, und &lt;= aus der Dropdown Liste Operator aus. Wenn Sie einen hart codierten Parameterwert verwenden (z. b. $25,00) oder wenn der Parameterwert Programm gesteuert angegeben werden soll, wählen Sie in der Dropdown Liste Quelle die Option keine aus. Geben Sie als nächstes den hart codierten Parameterwert im Textfeld Wert 25,00 ein, und schließen Sie den Prozess ab, indem Sie auf die Schaltfläche Hinzufügen klicken.

[![die im Dialog Feld WHERE-Klausel hinzufügen zurückgegebenen Ergebnisse einschränken.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Abbildung 2**: Begrenzen der Ergebnisse, die im Dialog Feld "`WHERE` Klausel hinzufügen" zurückgegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))

Klicken Sie nach dem Hinzufügen des Parameters auf OK, um zum Assistenten zum Konfigurieren von Datenquellen zurückzukehren. Die `SELECT`-Anweisung am Ende des Assistenten sollte nun eine `WHERE`-Klausel mit einem Parameter mit dem Namen `@UnitPrice`einschließen:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Wenn Sie im Dialogfeld `WHERE` Klausel hinzufügen mehrere Bedingungen in der `WHERE`-Klausel angeben, werden Sie vom Assistenten mit dem `AND`-Operator verbunden. Wenn Sie eine `OR` in die `WHERE`-Klausel (z. b. `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) einschließen müssen, müssen Sie die `SELECT`-Anweisung über den benutzerdefinierten SQL-Anweisungs Bildschirm erstellen.

Vervollständigen Sie die Konfiguration von SqlDataSource (Klicken Sie auf weiter und dann auf Fertigstellen), und überprüfen Sie dann das deklarative Markup von SqlDataSource s. Das Markup enthält jetzt eine `<SelectParameters>`-Auflistung, die die Quellen für die Parameter in der `SelectCommand`auslöst.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Wenn die SqlDataSource s-`Select()`-Methode aufgerufen wird, wird der `UnitPrice` Parameterwert (25,00) auf den `@UnitPrice`-Parameter in der `SelectCommand` angewendet, bevor er an die Datenbank gesendet wird. Das Ergebnis ist, dass nur die Produkte, die kleiner oder gleich $25,00 sind, aus der `Products` Tabelle zurückgegeben werden. Um dies zu bestätigen, fügen Sie der Seite eine GridView hinzu, binden Sie an diese Datenquelle, und zeigen Sie dann die Seite über einen Browser an. Es sollten nur die aufgeführten Produkte angezeigt werden, die kleiner oder gleich $25,00 sind, wie in Abbildung 3 zu sehen.

[![nur die Produkte, die kleiner oder gleich $25,00 sind, angezeigt.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Abbildung 3**: nur die Produkte, die kleiner oder gleich $25,00 sind, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))

## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Schritt 2: Hinzufügen von Parametern zu einer benutzerdefinierten SQL-Anweisung

Wenn Sie eine benutzerdefinierte SQL-Anweisung hinzufügen, können Sie die `WHERE`-Klausel explizit eingeben oder in der Filter Zelle des Abfrage-Generator einen Wert angeben. Um dies zu veranschaulichen, lassen Sie es nur die Produkte in einer GridView anzeigen, deren Preise kleiner als ein bestimmter Schwellenwert sind. Fügen Sie zunächst der Seite "`ParameterizedQueries.aspx`" ein Textfeld hinzu, um diesen Schwellenwert für den Benutzer zu erfassen. Legen Sie für die Eigenschaft TextBox s `ID` den Wert `MaxPrice`fest. Fügen Sie ein Schaltflächen-websteuer Element hinzu, und legen Sie dessen `Text`-Eigenschaft auf passende Produkte anzeigen

Ziehen Sie als nächstes eine GridView auf die Seite, und wählen Sie aus dem Smarttag aus, um eine neue SqlDataSource namens `ProductsFilteredByPriceDataSource`zu erstellen. Wechseln Sie im Assistenten zum Konfigurieren von Datenquellen mit dem Bildschirm benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben (siehe Abbildung 4), und geben Sie die folgende Abfrage ein:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Klicken Sie nach dem Eingeben der Abfrage (entweder manuell oder über die Abfrage-Generator) auf Weiter.

[![nur solche Produkte zurückgeben, die kleiner oder gleich einem Parameter Wert sind.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Abbildung 4**: nur solche Produkte zurückgeben, die kleiner oder gleich einem Parameter Wert sind ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))

Da die Abfrage Parameter enthält, fordert der nächste Bildschirm im Assistenten die Quelle der Parameterwerte an. Wählen Sie in der Dropdown Liste Parameter Quelle die Option Steuerelement aus, und `MaxPrice` (das Textfeld-Steuerelement s `ID` Wert) aus der ControlID-Dropdown Liste. Sie können auch einen optionalen Standardwert eingeben, der verwendet werden soll, wenn der Benutzer keinen Text in das Textfeld `MaxPrice` eingegeben hat. Geben Sie für den Zeitpunkt keinen Standardwert ein.

[![die maxprice s Text-Eigenschaft als Parameter Quelle verwendet.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Abbildung 5**: die `MaxPrice` TextBox s `Text` Eigenschaft wird als Parameter Quelle verwendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))

Klicken Sie zum Abschließen des Assistenten zum Konfigurieren von Datenquellen auf weiter und dann auf Fertigstellen. Das deklarative Markup für GridView, TextBox, Button und SqlDataSource folgt:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Beachten Sie, dass der Parameter im Abschnitt SqlDataSource s `<SelectParameters>` ein `ControlParameter`ist, der zusätzliche Eigenschaften wie `ControlID` und `PropertyName`enthält. Wenn die SqlDataSource s-`Select()`-Methode aufgerufen wird, greift der `ControlParameter` den Wert aus der angegebenen websteuer Element Eigenschaft ab und weist ihn dem entsprechenden Parameter in der `SelectCommand`zu. In diesem Beispiel wird die Text-Eigenschaft `MaxPrice` s als `@MaxPrice` Parameterwert verwendet.

Nehmen Sie sich eine Minute Zeit, um diese Seite in einem Browser anzuzeigen. Wenn Sie zum ersten Mal die Seite aufrufen oder wenn im Textfeld `MaxPrice` ein Wert fehlt, werden in der GridView keine Datensätze angezeigt.

[Wenn das Textfeld maxprice leer ist, ![keine Datensätze angezeigt.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Abbildung 6**: Es werden keine Datensätze angezeigt, wenn das `MaxPrice` Textfeld leer ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))

Der Grund, warum keine Produkte angezeigt werden, liegt darin, dass standardmäßig eine leere Zeichenfolge für einen Parameterwert in eine Datenbank `NULL` Wert konvertiert wird. Da der Vergleich von `[UnitPrice] <= NULL` immer als false ausgewertet wird, werden keine Ergebnisse zurückgegeben.

Geben Sie einen Wert in das Textfeld ein, z. b. 5,00, und klicken Sie auf die Schaltfläche passende Produkte anzeigen. Beim Postback teilt SqlDataSource dem GridView mit, dass sich einer seiner Parameter Quellen geändert hat. Folglich wird die GridView an die SqlDataSource zurückgebunden und zeigt diese Produkte an, die kleiner oder gleich $5,00 sind.

[Es werden ![Produkte angezeigt, die kleiner oder gleich $5,00 sind.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Abbildung 7**: Produkte, die kleiner oder gleich $5,00 sind, werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))

## <a name="initially-displaying-all-products"></a>Anfänglich alle Produkte anzeigen

Anstatt beim ersten Laden der Seite keine Produkte anzuzeigen, möchten wir möglicherweise *alle* Produkte anzeigen. Eine Möglichkeit, alle Produkte aufzulisten, wenn das `MaxPrice` Textfeld leer ist, besteht darin, den Standardwert des Parameters auf einen sehr hohen Wert, wie z. b. 1 Million, festzulegen, da es unwahrscheinlich ist, dass Northwind Traders jemals Inventuren haben wird, deren Einheitspreis $1 Million überschreitet. Dieser Ansatz ist jedoch kurzsichtig und funktioniert möglicherweise nicht in anderen Fällen.

In vorherigen Tutorials: [deklarative Parameter](../basic-reporting/declarative-parameters-vb.md) und die [Master/Detail-Filterung mit einer Dropdown List](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) haben wir ein ähnliches Problem festgestellt. Unsere Lösung bestand darin, diese Logik in die Geschäftslogik Schicht zu versetzen. Insbesondere untersuchte der BLL den eingehenden Wert, und wenn er `NULL` oder ein reservierter Wert war, wurde der-Befehl an die DAL-Methode weitergeleitet, die alle Datensätze zurückgegeben hat. Wenn der eingehende Wert ein normaler Filter Wert war, wurde an der dal-Methode ein-Befehl ausgeführt, der eine SQL-Anweisung ausgeführt hat, die eine parametrisierte `WHERE`-Klausel mit dem angegebenen Wert verwendet hat.

Leider umgehen wir die Architektur bei Verwendung von SqlDataSource. Stattdessen muss die SQL-Anweisung angepasst werden, um alle Datensätze intelligent zu erfassen, wenn der `@MaximumPrice` Parameter `NULL` oder ein reservierter Wert ist. Bei dieser Übung lassen Sie es für den Fall, dass der `@MaximumPrice` Parameter gleich `-1.0`ist, dass *alle* Datensätze zurückgegeben werden (`-1.0` funktioniert als reservierter Wert, da kein Produkt einen negativen `UnitPrice` Wert aufweisen kann). Um dies zu erreichen, können wir die folgende SQL-Anweisung verwenden:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Diese `WHERE`-Klausel gibt *alle* Datensätze zurück, wenn der `@MaximumPrice` Parameter `-1.0`ist. Wenn der Parameterwert nicht `-1.0`ist, werden nur die Produkte zurückgegeben, deren `UnitPrice` kleiner oder gleich dem `@MaximumPrice` Parameterwert ist. Wenn Sie den Standardwert des `@MaximumPrice`-Parameters auf `-1.0`festlegen, wird beim ersten Laden der Seite (oder wenn das `MaxPrice` Textfeld leer ist) `@MaximumPrice` der Wert `-1.0` angezeigt, und alle Produkte werden angezeigt.

[![jetzt werden alle Produkte angezeigt, wenn das Textfeld maxprice leer ist.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Abbildung 8**: jetzt werden alle Produkte angezeigt, wenn das `MaxPrice` Textfeld leer ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))

Bei diesem Ansatz sind einige Einschränkungen zu beachten. Beachten Sie zunächst, dass der Datentyp des Parameters s durch die Verwendung in der SQL-Abfrage abgeleitet wird. Wenn Sie die `WHERE`-Klausel von `@MaximumPrice = -1.0` in `@MaximumPrice = -1`ändern, behandelt die Laufzeit den-Parameter als ganze Zahl. Wenn Sie dann versuchen, das `MaxPrice` Textfeld einem Dezimalwert zuzuweisen (z. b. 5,00), tritt ein Fehler auf, weil der Wert von 5,00 nicht in eine ganze Zahl konvertiert werden kann. Um dieses Problem zu beheben, stellen Sie sicher, dass Sie `@MaximumPrice = -1.0` in der `WHERE`-Klausel verwenden, oder legen Sie für die `ControlParameter` Objekt s `Type`-Eigenschaft den Wert Decimal fest.

Durch das Hinzufügen der `OR @MaximumPrice = -1.0` zur `WHERE`-Klausel kann die Abfrage-Engine einen Index für `UnitPrice` nicht verwenden (sofern vorhanden), was zu einem Tabellenscan führt. Dies kann sich auf die Leistung auswirken, wenn in der `Products` Tabelle eine ausreichend große Anzahl von Datensätzen vorhanden ist. Ein besserer Ansatz wäre, diese Logik in eine gespeicherte Prozedur zu verschieben, in der eine `IF`-Anweisung entweder eine `SELECT` Abfrage aus der `Products` Tabelle ohne eine `WHERE`-Klausel ausführen würde, wenn alle Datensätze zurückgegeben werden müssen oder eine, deren `WHERE`-Klausel nur die `UnitPrice` Kriterien enthält, sodass ein Index verwendet werden kann.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Schritt 3: Erstellen und Verwenden von parametrisierten gespeicherten Prozeduren

Gespeicherte Prozeduren können einen Satz von Eingabe Parametern enthalten, die dann in den SQL-Anweisungen verwendet werden können, die in der gespeicherten Prozedur definiert werden. Beim Konfigurieren von SqlDataSource für die Verwendung einer gespeicherten Prozedur, die Eingabeparameter akzeptiert, können diese Parameterwerte mit denselben Techniken wie bei Ad-hoc-SQL-Anweisungen angegeben werden.

Um die Verwendung gespeicherter Prozeduren in SqlDataSource zu veranschaulichen, erstellen Sie eine neue gespeicherte Prozedur in der Northwind-Datenbank mit dem Namen `GetProductsByCategory`, die einen Parameter mit dem Namen `@CategoryID` akzeptiert und alle Spalten der Produkte zurückgibt, deren `CategoryID` Spalte `@CategoryID`entspricht. Um eine gespeicherte Prozedur zu erstellen, wechseln Sie zum Server-Explorer, und führen Sie einen Drilldown in die `NORTHWND.MDF` Datenbank aus. (Wenn Ihnen die Server-Explorer angezeigt wird, klicken Sie auf das Menü Ansicht, und wählen Sie die Option Server-Explorer aus.)

Klicken Sie in der `NORTHWND.MDF` Datenbank mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, wählen Sie neue gespeicherte Prozedur hinzufügen aus, und geben Sie die folgende Syntax ein:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Klicken Sie auf das Symbol speichern (oder drücken Sie STRG + S), um die gespeicherte Prozedur zu speichern. Sie können die gespeicherte Prozedur testen, indem Sie im Ordner gespeicherte Prozeduren mit der rechten Maustaste darauf klicken und ausführen auswählen. Dadurch werden Sie zur Eingabe der Parameter der gespeicherten Prozedur (`@CategoryID`in dieser Instanz) aufgefordert. Anschließend werden die Ergebnisse im Fenster Ausgabe angezeigt.

[![Sie die gespeicherte Prozedur getproducungbycategory bei Ausführung mit einer @CategoryID von 1.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Abbildung 9**: die gespeicherte Prozedur `GetProductsByCategory` bei Ausführung mit einem `@CategoryID` von 1 ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))

Verwenden Sie diese gespeicherte Prozedur, um alle Produkte in der Kategorie "Getränke" in einer GridView anzuzeigen. Fügen Sie der Seite eine neue GridView hinzu, und binden Sie Sie an eine neue SqlDataSource mit dem Namen `BeverageProductsDataSource`. Fahren Sie mit dem Bildschirm benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben fort, aktivieren Sie das Optionsfeld gespeicherte Prozedur, und wählen Sie in der Dropdown Liste die gespeicherte Prozedur `GetProductsByCategory` aus.

[![die gespeicherte Prozedur getproductionbycategory aus der Dropdown Liste auswählen.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Abbildung 10**: Wählen Sie in der Dropdown Liste die gespeicherte Prozedur `GetProductsByCategory` aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png)).

Da die gespeicherte Prozedur einen Eingabeparameter (`@CategoryID`) akzeptiert, werden wir durch Klicken auf "weiter" aufgefordert, die Quelle für diesen Parameterwert anzugeben. Der `CategoryID` Getränke ist 1. lassen Sie daher die Dropdown Liste Parameter Quelle bei None aus, und geben Sie im Textfeld DefaultValue den Wert 1 ein.

[![einen hart codierten Wert 1 verwenden, um die Produkte in der Kategorie "Getränke" zurückzugeben.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Abbildung 11**: Verwenden eines hart codierten Werts von 1 zum Zurückgeben der Produkte in der Kategorie "Getränke" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))

Wie das folgende deklarative Markup zeigt, wird beim Verwenden einer gespeicherten Prozedur die SqlDataSource s-`SelectCommand`-Eigenschaft auf den Namen der gespeicherten Prozedur festgelegt, und die [`SelectCommandType`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) wird auf `StoredProcedure`festgelegt, was bedeutet, dass der `SelectCommand` der Name einer gespeicherten Prozedur anstelle einer Ad-hoc-SQL-Anweisung ist.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Testen Sie die Seite in einem Browser. Nur die Produkte, die zur Kategorie Getränke gehören, werden angezeigt, obwohl *alle* Produktfelder angezeigt werden, da die gespeicherte Prozedur `GetProductsByCategory` alle Spalten aus der `Products` Tabelle zurückgibt. Wir könnten natürlich die Felder, die in der GridView angezeigt werden, im Dialogfeld "Spalten bearbeiten" in GridView einschränken oder anpassen.

[![alle Getränke angezeigt werden](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Abbildung 12**: alle Getränke werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))

## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Schritt 4: Programm gesteuertes Aufrufen einer SqlDataSource s`Select()`-Anweisung

Die Beispiele, die wir im vorherigen Tutorial und in diesem Tutorial gesehen haben, haben die SqlDataSource-Steuerelemente direkt an eine GridView gebunden. Die Daten des SqlDataSource-Steuer Elements können jedoch Programm gesteuert aufgerufen und im Code aufgelistet werden. Dies kann besonders nützlich sein, wenn Sie Daten Abfragen müssen, um Sie zu überprüfen, aber Sie müssen Sie nicht anzeigen. Anstatt den gesamten Code Bausteine ADO.net zu schreiben, um eine Verbindung mit der Datenbank herzustellen, den Befehl anzugeben und die Ergebnisse abzurufen, kann die SqlDataSource diesen monoton Code verarbeiten lassen.

Um das programmgesteuerte arbeiten mit den SqlDataSource-Daten zu veranschaulichen, stellen Sie sich vor, dass Ihr Chef Sie mit einer Anforderung zum Erstellen einer Webseite kontaktiert hat, die den Namen einer zufällig ausgewählten Kategorie und der zugehörigen Produkte anzeigt. Das heißt, wenn ein Benutzer diese Seite besucht, möchten wir eine Kategorie aus der `Categories` Tabelle auswählen, den Kategorien Amen anzeigen und dann die Produkte auflisten, die zu dieser Kategorie gehören.

Um dies zu erreichen, benötigen wir zwei SqlDataSource-Steuerelemente, um eine Zufalls Kategorie aus der `Categories` Tabelle abzurufen, und eine weitere, um die Produkte der Kategorie zu erhalten. Wir erstellen die SqlDataSource, die in diesem Schritt einen Datensatz für eine Zufalls Kategorie abruft. Schritt 5 befasst sich mit dem Erstellen der SqlDataSource, die die Produkte der Kategorie "s" abruft.

Fügen Sie zunächst ein SqlDataSource-`ParameterizedQueries.aspx` hinzu, und legen Sie dessen `ID` auf `RandomCategoryDataSource`fest. Konfigurieren Sie Sie so, dass die folgende SQL-Abfrage verwendet wird:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` gibt die in zufälliger Reihenfolge sortierten Datensätze zurück (siehe [Verwenden von `NEWID()` zum Zufalls Sortieren von Datensätzen](http://www.sqlteam.com/item.asp?ItemID=8747)) `SELECT TOP 1` gibt den ersten Datensatz aus dem Resultset zurück. Diese Abfrage gibt die `CategoryID`-und `CategoryName` Spaltenwerte aus einer einzelnen, zufällig ausgewählten Kategorie zurück.

Um die Kategorie s `CategoryName` Wert anzuzeigen, fügen Sie der Seite ein Label-websteuer Element hinzu, legen Sie dessen `ID`-Eigenschaft auf `CategoryNameLabel`fest, und löschen Sie die `Text`-Eigenschaft. Zum programmgesteuerten Abrufen von Daten aus einem SqlDataSource-Steuerelement müssen wir dessen `Select()`-Methode aufrufen. Die [`Select()`-Methode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) erwartet einen einzelnen Eingabeparameter vom Typ " [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)", der angibt, wie die Daten vor dem Zurückgeben der Daten übertragen werden sollen. Dies kann Anweisungen zum Sortieren und Filtern der Daten beinhalten und wird von den datenweb Steuerelementen beim Sortieren oder Paging durch die Daten eines SqlDataSource-Steuer Elements verwendet. In unserem Beispiel benötigen wir jedoch keine Daten, die geändert werden müssen, bevor Sie zurückgegeben werden. Daher wird das `DataSourceSelectArguments.Empty` Objekt übergeben.

Die `Select()`-Methode gibt ein Objekt zurück, das `IEnumerable`implementiert. Der genaue Typ, der zurückgegeben wird, hängt vom Wert der [`DataSourceMode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)des SqlDataSource-Steuer Elements ab. Wie bereits im vorherigen Tutorial erläutert, kann diese Eigenschaft auf einen Wert von `DataSet` oder `DataReader`festgelegt werden. Wenn `DataSet`festgelegt ist, gibt die `Select()`-Methode ein [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) -Objekt zurück. Wenn `DataReader`festgelegt ist, wird ein Objekt zurückgegeben, das [`IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx)implementiert. Da für den `RandomCategoryDataSource` SqlDataSource die Eigenschaft `DataSourceMode` auf `DataSet` (Standard) festgelegt ist, werden wir mit einem DataView-Objekt arbeiten.

Der folgende Code veranschaulicht, wie Sie die Datensätze aus der `RandomCategoryDataSource` SqlDataSource als DataView abrufen und den Wert der `CategoryName` Spalte aus der ersten DataView-Zeile lesen können:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` gibt den ersten `DataRowView` in der DataView zurück. `randomCategoryView(0)("CategoryName")` gibt den Wert der `CategoryName` Spalte in dieser ersten Zeile zurück. Beachten Sie, dass DataView lose typisiert ist. Um auf einen bestimmten Spaltenwert zu verweisen, müssen wir den Namen der Spalte als Zeichenfolge (in diesem Fall CategoryName) übergeben. Abbildung 13 zeigt die Meldung, die beim Anzeigen der Seite in der `CategoryNameLabel` angezeigt wird. Natürlich wird der tatsächliche Kategoriename nach dem Zufallsprinzip von der `RandomCategoryDataSource` SqlDataSource bei jedem Besuch der Seite (einschließlich Postbacks) ausgewählt.

[![der zufällig ausgewählte Name der Kategorie s angezeigt wird.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Abbildung 13**: der zufällig ausgewählte Name der Kategorie s wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))

> [!NOTE]
> Wenn die `DataSourceMode`-Eigenschaft des SqlDataSource-Steuer Elements auf `DataReader`festgelegt wurde, musste der Rückgabewert der `Select()`-Methode in `IDataReader`umgewandelt werden. Um den `CategoryName` Spaltenwert aus der ersten Zeile zu lesen, verwenden wir Code wie den folgenden:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Nachdem Sie SqlDataSource nach dem Zufallsprinzip eine Kategorie ausgewählt haben, können Sie das GridView-Elemente hinzufügen, das die Produkte der Kategorie auflistet.

> [!NOTE]
> Anstatt ein Label-websteuer Element zum Anzeigen des Namens der Kategorie zu verwenden, könnten wir der Seite eine FormView oder DetailsView hinzufügen, um Sie an SqlDataSource zu binden. Mithilfe der Bezeichnung konnten wir jedoch untersuchen, wie Sie die SqlDataSource s-`Select()`-Anweisung Programm gesteuert aufrufen und mit den resultierenden Daten im Code arbeiten können.

## <a name="step-5-assigning-parameter-values-programmatically"></a>Schritt 5: Programm gesteuertes Zuweisen von Parameter Werten

Alle bisher in diesem Tutorial genannten Beispiele haben entweder einen hart codierten Parameterwert oder einen, der aus einer der vordefinierten Parameter Quellen entnommen wurde (ein QueryString-Wert, ein websteuer Element auf der Seite usw.) verwendet. Die Parameter des SqlDataSource-Steuer Elements können jedoch auch Programm gesteuert festgelegt werden. Um das aktuelle Beispiel abzuschließen, benötigen wir eine SqlDataSource, die alle Produkte zurückgibt, die zu einer bestimmten Kategorie gehören. Diese SqlDataSource verfügt über einen `CategoryID` Parameter, dessen Wert auf der Grundlage des `CategoryID` Spaltenwerts festgelegt werden muss, der von der `RandomCategoryDataSource` SqlDataSource im `Page_Load` Ereignishandler zurückgegeben wird.

Fügen Sie zunächst der Seite eine GridView hinzu, und binden Sie Sie an eine neue SqlDataSource mit dem Namen `ProductsByCategoryDataSource`. Ähnlich wie in Schritt 3, konfigurieren Sie die SqlDataSource so, dass Sie die gespeicherte Prozedur `GetProductsByCategory` aufruft. Lassen Sie die Dropdown Liste Parameter Quelle auf keine festgelegt, aber geben Sie keinen Standardwert ein, da dieser Standardwert Programm gesteuert festgelegt wird.

[![keine Parameter Quelle oder keinen Standardwert angeben.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Abbildung 14**: keine Parameter Quelle oder keinen Standardwert angeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))

Nachdem der SqlDataSource-Assistent abgeschlossen wurde, sollte das resultierende deklarative Markup in etwa wie folgt aussehen:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

Wir können die `DefaultValue` des Parameters "`CategoryID`" Programm gesteuert im `Page_Load`-Ereignishandler zuweisen:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Mit dieser Addition enthält die Seite eine GridView, in der die Produkte angezeigt werden, die der zufällig ausgewählten Kategorie zugeordnet sind.

[![keine Parameter Quelle oder keinen Standardwert angeben.](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Abbildung 15**: keine Parameter Quelle oder keinen Standardwert angeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))

## <a name="summary"></a>Summary

Die SqlDataSource ermöglicht Seiten Entwicklern, parametrisierte Abfragen zu definieren, deren Parameterwerte hart codiert, aus vordefinierten Parameter Quellen abgerufen oder Programm gesteuert zugewiesen werden können. In diesem Tutorial wurde erläutert, wie Sie eine parametrisierte Abfrage aus dem Assistenten zum Konfigurieren von Datenquellen für Ad-hoc-SQL-Abfragen und gespeicherte Prozeduren erstellen. Außerdem haben wir uns mit der Verwendung von hart codierten Parameter Quellen, einem websteuer Element als Parameter Quelle und der programmgesteuerten Angabe des Parameter Werts beschäftigt.

Wie bei ObjectDataSource bietet auch SqlDataSource Funktionen zum Ändern der zugrunde liegenden Daten. Im nächsten Tutorial wird erläutert, wie Sie `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen mit SqlDataSource definieren. Nachdem diese Anweisungen hinzugefügt wurden, können wir die integrierten Funktionen zum Einfügen, bearbeiten und Löschen verwenden, die den Steuerelementen GridView, DetailsView und FormView zugrunde liegen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Scott Clyde, Randell Schmidt und Ken pespisa. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](querying-data-with-the-sqldatasource-control-vb.md)
> [Weiter](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
