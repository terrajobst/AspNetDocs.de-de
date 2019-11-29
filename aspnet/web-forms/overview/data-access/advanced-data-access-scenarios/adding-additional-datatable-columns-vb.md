---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Hinzufügen zusätzlicher Daten Tabellenspalten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie mit dem TableAdapter-Assistenten ein typisiertes Dataset erstellen, enthält die entsprechende Datentabelle die Spalten, die von der Hauptdaten Bank Abfrage zurückgegeben werden. Aber hier...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a55f8bc4d3508387927ca81674073a001867de7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74608300"
---
# <a name="adding-additional-datatable-columns-vb"></a>Hinzufügen von zusätzlichen DataTable-Spalten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) oder [PDF herunterladen](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Wenn Sie mit dem TableAdapter-Assistenten ein typisiertes Dataset erstellen, enthält die entsprechende Datentabelle die Spalten, die von der Hauptdaten Bank Abfrage zurückgegeben werden. Es gibt jedoch Situationen, in denen die Datentabelle zusätzliche Spalten enthalten muss. In diesem Tutorial erfahren Sie, warum gespeicherte Prozeduren empfohlen werden, wenn zusätzliche Daten Tabellenspalten erforderlich sind.

## <a name="introduction"></a>Einführung

Wenn Sie einem typisierten Dataset einen TableAdapter hinzufügen, wird das entsprechende datbare s-Schema durch die Haupt Abfrage des TableAdapter s bestimmt. Wenn die Haupt Abfrage z. b. die Datenfelder *a*, *b*und *c*zurückgibt, verfügt die Datentabelle über drei entsprechende Spalten mit den Namen *A*, *b*und *c*. Neben der Haupt Abfrage kann ein TableAdapter zusätzliche Abfragen einschließen, die ggf. eine Teilmenge der Daten auf der Grundlage eines Parameters zurückgeben. Zusätzlich zur Haupt Abfrage `ProductsTableAdapter` s, die Informationen zu allen Produkten zurückgibt, enthält Sie z. b. auch Methoden wie `GetProductsByCategoryID(categoryID)` und `GetProductByProductID(productID)`, die bestimmte Produktinformationen auf der Grundlage eines angegebenen Parameters zurückgeben.

Das Modell, in dem das Datatyp s-Schema die TableAdapter s-Haupt Abfrage widerspiegelt, funktioniert gut, wenn alle TableAdapter s-Methoden die gleichen oder weniger Datenfelder zurückgeben, als in der Haupt Abfrage angegeben sind. Wenn eine TableAdapter-Methode zusätzliche Datenfelder zurückgeben muss, sollte das Schema der Datentabelle entsprechend erweitert werden. In der [Master/Detail-Auflistung mit einer Liste von Master Datensätzen mit einem DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) -Lernprogramm für Details haben wir dem `CategoriesTableAdapter` eine Methode hinzugefügt, die die in der Haupt Abfrage und `NumberOfProducts`definierten Datenfelder `CategoryID`, `CategoryName`und `Description` zurückgegeben hat, ein zusätzliches Datenfeld, das die Anzahl der der jeweiligen Kategorie zugeordneten Produkte gemeldet hat. Wir haben dem `CategoriesDataTable` manuell eine neue Spalte hinzugefügt, um den Wert des `NumberOfProducts` Daten Felds aus dieser neuen Methode zu erfassen.

Wie im Tutorial [Hochladen von Dateien](../working-with-binary-files/uploading-files-vb.md) erläutert, muss mit TableAdapters, die Ad-hoc-SQL-Anweisungen verwenden, sehr sorgfältig vorgegangen werden, und es sind Methoden vorhanden, deren Datenfelder nicht exakt mit der Haupt Abfrage identisch sind. Wenn der TableAdapter-Konfigurations-Assistent erneut ausgeführt wird, werden alle TableAdapter s-Methoden aktualisiert, damit die Daten Feldliste mit der Haupt Abfrage übereinstimmt. Folglich werden alle Methoden mit angepassten Spalten Listen auf die Haupt Spaltenliste der Abfrage zurückgesetzt, und es werden nicht die erwarteten Daten zurückgegeben. Dieses Problem tritt nicht auf, wenn gespeicherte Prozeduren verwendet werden.

In diesem Tutorial wird erläutert, wie ein Datentabelle-Schema erweitert wird, um zusätzliche Spalten einzuschließen. Aufgrund der Brüchigkeit des TableAdapter bei der Verwendung von Ad-hoc-SQL-Anweisungen werden in diesem Tutorial gespeicherte Prozeduren verwendet. Weitere Informationen zum Konfigurieren eines TableAdapters für die Verwendung gespeicherter Prozeduren finden Sie unter [Erstellen neuer gespeicherter Prozeduren für die TableAdapters für typisierte Datasets](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) und [Verwenden vorhandener gespeicherter Prozeduren für das typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Schritt 1: Hinzufügen einer`PriceQuartile`Spalte zum`ProductsDataTable`

Im Tutorial *Erstellen neuer gespeicherter Prozeduren für das typisierte DataSet s TableAdapters* haben wir ein typisiertes DataSet mit dem Namen "`NorthwindWithSprocs`" erstellt. Dieses DataSet enthält derzeit zwei DataTables: `ProductsDataTable` und `EmployeesDataTable`. Der `ProductsTableAdapter` verfügt über die folgenden drei Methoden:

- `GetProducts`-die Haupt Abfrage, die alle Datensätze aus der `Products` Tabelle zurückgibt
- `GetProductsByCategoryID(categoryID)`: gibt alle Produkte mit der angegebenen *CategoryID*zurück.
- `GetProductByProductID(productID)`: gibt das jeweilige Produkt mit der angegebenen *ProductID*zurück.

Die Haupt Abfrage und die beiden zusätzlichen Methoden geben alle denselben Satz von Datenfeldern zurück, nämlich alle Spalten aus der `Products` Tabelle. Es sind keine korrelierten Unterabfragen vorhanden, oder es `JOIN` s, die zugehörige Daten aus den Tabellen `Categories` oder `Suppliers` abrufen. Daher verfügt die `ProductsDataTable` über eine entsprechende Spalte für jedes Feld in der `Products` Tabelle.

Fügen Sie in diesem Tutorial dem `ProductsTableAdapter` mit dem Namen `GetProductsWithPriceQuartile` eine Methode hinzu, die alle Produkte zurückgibt. Zusätzlich zu den standardmäßigen Product Data-Feldern enthält `GetProductsWithPriceQuartile` auch ein `PriceQuartile` Datenfeld, das angibt, in welchem Quartil der Preis des Produkts liegt. Beispielsweise haben die Produkte, deren Preise sich in den teuersten 25% befinden, den `PriceQuartile` Wert 1, während diejenigen, deren Preise den unteren 25% überschreiten, den Wert 4 haben. Bevor wir uns mit der Erstellung der gespeicherten Prozedur beschäftigen, um diese Informationen zurückzugeben, müssen wir zunächst die `ProductsDataTable` aktualisieren, um eine Spalte einzuschließen, die die `PriceQuartile` Ergebnisse enthält, wenn die `GetProductsWithPriceQuartile`-Methode verwendet wird.

Öffnen Sie das `NorthwindWithSprocs` DataSet, und klicken Sie mit der rechten Maustaste auf die `ProductsDataTable`. Klicken Sie im Kontextmenü auf Hinzufügen, und wählen Sie dann Spalte aus.

[![der productsdatabel eine neue Spalte hinzufügen](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Spalte zum `ProductsDataTable` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image3.png))

Dadurch wird der Datentabelle eine neue Spalte mit dem Namen column1 vom Typ `System.String`hinzugefügt. Wir müssen den Namen dieser Spalte auf "preiquartile" und deren Typ auf "`System.Int32`" aktualisieren, da Sie für eine Zahl zwischen 1 und 4 verwendet wird. Wählen Sie die neu hinzugefügte Spalte in der `ProductsDataTable` aus, und legen Sie aus dem Eigenschaftenfenster die Eigenschaft `Name` auf "preiquartile" und die Eigenschaft "`DataType`" auf `System.Int32`fest.

[![die Eigenschaften "Name" und "DataType" der neuen Spalte festlegen.](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Abbildung 2**: Festlegen der neuen Spalten `Name` und `DataType` Eigenschaften ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image6.png))

Wie in Abbildung 2 dargestellt, gibt es zusätzliche Eigenschaften, die festgelegt werden können, z. b. ob die Werte in der Spalte eindeutig sein müssen, ob die Spalte eine Spalte mit automatischer Inkrement ist, ob Daten Bank `NULL` Werte zulässig sind usw. Lassen Sie diese Werte auf ihre Standardwerte fest.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Schritt 2: Erstellen der`GetProductsWithPriceQuartile`Methode

Nachdem nun die `ProductsDataTable` aktualisiert wurde, um die `PriceQuartile` Spalte einzuschließen, können wir die `GetProductsWithPriceQuartile`-Methode erstellen. Klicken Sie zunächst mit der rechten Maustaste auf den TableAdapter, und wählen Sie im Kontextmenü Abfrage hinzufügen aus. Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen geöffnet, der uns zuerst auffordert, ob Sie Ad-hoc-SQL-Anweisungen oder eine neue oder vorhandene gespeicherte Prozedur verwenden möchten. Da wir noch keine gespeicherte Prozedur haben, die die Price Quartil-Daten zurückgibt, gestatten Sie es dem TableAdapter, diese gespeicherte Prozedur für uns zu erstellen. Wählen Sie die Option neue gespeicherte Prozedur erstellen aus, und klicken Sie auf Weiter.

[![den TableAdapter-Assistenten anzuweisen, die gespeicherte Prozedur für uns zu erstellen](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Abbildung 3**: anweisen des TableAdapter-Assistenten zum Erstellen der gespeicherten Prozedur für uns ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image9.png))

Auf dem nachfolgenden Bildschirm, der in Abbildung 4 gezeigt wird, fragt der Assistent uns, welche Art von Abfrage hinzugefügt werden soll. Da die `GetProductsWithPriceQuartile`-Methode alle Spalten und Datensätze aus der `Products` Tabelle zurückgibt, wählen Sie die Option select this Returns Rows aus, und klicken Sie auf Next.

[![es sich bei der Abfrage um eine SELECT-Anweisung, die mehrere Zeilen zurückgibt.](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Abbildung 4**: bei der Abfrage handelt es sich um eine `SELECT` Anweisung, die mehrere Zeilen zurückgibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image12.png)).

Als nächstes werden wir aufgefordert, die `SELECT` Abfrage zu erhalten. Geben Sie die folgende Abfrage in den Assistenten ein:

[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

In der obigen Abfrage wird die neue [`NTILE`-Funktion](https://msdn.microsoft.com/library/ms175126.aspx) SQL Server 2005 s verwendet, um die Ergebnisse in vier Gruppen aufzuteilen, in denen die Gruppen durch die `UnitPrice` Werte in absteigender Reihenfolge sortiert werden.

Leider weiß die Abfrage-Generator nicht, wie das `OVER`-Schlüsselwort analysiert werden kann, und zeigt beim Analysieren der obigen Abfrage einen Fehler an. Geben Sie daher die oben genannte Abfrage direkt in das Textfeld des Assistenten ein, ohne die Abfrage-Generator zu verwenden.

> [!NOTE]
> Weitere Informationen zu NTILE-und SQL Server 2005 s-Rang Folge Funktionen finden Sie unter Zurückgeben von [Rang Ergebnissen mit Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) und im [Abschnitt Rang Folge Funktionen](https://msdn.microsoft.com/library/ms189798.aspx) aus der [SQL Server 2005-Online](https://msdn.microsoft.com/library/ms189798.aspx)Dokumentation.

Nachdem Sie die `SELECT` Abfrage eingegeben und auf "weiter" geklickt haben, werden Sie vom Assistenten aufgefordert, einen Namen für die gespeicherte Prozedur anzugeben, die erstellt wird. Benennen Sie die neue gespeicherte Prozedur `Products_SelectWithPriceQuartile` und klicken Sie dann auf Weiter.

[![der gespeicherten Prozedur den Namen Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Abbildung 5**: Benennen der gespeicherten Prozedur `Products_SelectWithPriceQuartile` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image15.png))

Abschließend werden wir aufgefordert, die TableAdapter-Methoden zu benennen. Lassen Sie das Kontrollkästchen Datentabelle ausfüllen und Datentabelle zurückgeben aktiviert, und benennen Sie die Methoden `FillWithPriceQuartile` und `GetProductsWithPriceQuartile`.

[![den TableAdapter-Methoden benennen und auf Fertigstellen klicken.](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Abbildung 6**: Benennen der TableAdapter s-Methoden und klicken auf "Fertigstellen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image18.png))

Klicken Sie bei angegebener `SELECT` Abfrage und der gespeicherten Prozedur und TableAdapter-Methoden auf Fertigstellen, um den Assistenten abzuschließen. An diesem Punkt erhalten Sie möglicherweise eine Warnung oder zwei vom Assistenten, die besagt, dass die `OVER` SQL-Konstrukt oder-Anweisung nicht unterstützt wird. Diese Warnungen können ignoriert werden.

Nachdem Sie den Assistenten abgeschlossen haben, sollte der TableAdapter die Methoden `FillWithPriceQuartile` und `GetProductsWithPriceQuartile` enthalten, und die Datenbank sollte eine gespeicherte Prozedur mit dem Namen `Products_SelectWithPriceQuartile`enthalten. Nehmen Sie sich einen Moment Zeit, um zu überprüfen, ob der TableAdapter diese neue Methode tatsächlich enthält und dass die gespeicherte Prozedur ordnungsgemäß zur Datenbank hinzugefügt wurde. Wenn beim Überprüfen der Datenbank die gespeicherte Prozedur nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie aktualisieren aus.

![Stellen Sie sicher, dass dem TableAdapter eine neue Methode hinzugefügt wurde.](adding-additional-datatable-columns-vb/_static/image19.png)

**Abbildung 7**: überprüfen, ob dem TableAdapter eine neue Methode hinzugefügt wurde

[![stellen Sie sicher, dass die Datenbank die gespeicherte Prozedur Products_SelectWithPriceQuartile enthält.](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Abbildung 8**: sicherstellen, dass die Datenbank die gespeicherte Prozedur `Products_SelectWithPriceQuartile` enthält ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image22.png))

> [!NOTE]
> Einer der Vorteile der Verwendung von gespeicherten Prozeduren anstelle von Ad-hoc-SQL-Anweisungen besteht darin, dass das erneute Ausführen des TableAdapter-Konfigurations-Assistenten die Spalten Listen der gespeicherten Prozeduren nicht ändert. Überprüfen Sie dies, indem Sie mit der rechten Maustaste auf den TableAdapter klicken und im Kontextmenü die Option konfigurieren auswählen, um den Assistenten zu starten, und dann auf Fertigstellen klicken, um den Assistenten abzuschließen. Navigieren Sie als nächstes zur Datenbank, und zeigen Sie die gespeicherte Prozedur `Products_SelectWithPriceQuartile` an. Beachten Sie, dass die Spaltenliste nicht geändert wurde. Wenn wir Ad-hoc-SQL-Anweisungen verwendet haben, hätte der TableAdapter-Konfigurations-Assistent durch erneutes Ausführen des TableAdapter-Konfigurations-Assistenten die Spaltenliste der Abfrage mit der Liste der Haupt Abfrage Spalten zurückgesetzt, wodurch die NTILE-Anweisung aus der von der `GetProductsWithPriceQuartile`-Methode verwendeten Abfrage entfernt wird.

Wenn die Datenzugriffs Schicht s `GetProductsWithPriceQuartile`-Methode aufgerufen wird, führt der TableAdapter die gespeicherte Prozedur `Products_SelectWithPriceQuartile` aus und fügt der `ProductsDataTable` für jeden zurückgegebenen Datensatz eine Zeile hinzu. Die von der gespeicherten Prozedur zurückgegebenen Datenfelder werden den `ProductsDataTable` s-Spalten zugeordnet. Da ein `PriceQuartile` Datenfeld von der gespeicherten Prozedur zurückgegeben wird, wird der Wert der `ProductsDataTable` s `PriceQuartile` Spalte zugewiesen.

Für TableAdapter-Methoden, deren Abfragen kein `PriceQuartile` Datenfeld zurückgeben, ist der Wert der `PriceQuartile` Spalte der Wert, der von seiner `DefaultValue`-Eigenschaft angegeben wird. Wie in Abbildung 2 gezeigt, wird dieser Wert auf `DBNull`festgelegt, die Standardeinstellung. Wenn Sie einen anderen Standardwert bevorzugen, legen Sie einfach die `DefaultValue`-Eigenschaft entsprechend fest. Stellen Sie nur sicher, dass der `DefaultValue` Wert gültig ist, wenn die Spalte s `DataType` ist (d. h. `System.Int32` für die Spalte `PriceQuartile`).

An diesem Punkt haben wir die erforderlichen Schritte zum Hinzufügen einer zusätzlichen Spalte zu einer Datentabelle durchgeführt. Um zu überprüfen, ob diese zusätzliche Spalte erwartungsgemäß funktioniert, können Sie eine ASP.NET-Seite erstellen, auf der die Namen, der Preis und das Preis Quartil der einzelnen Produkte angezeigt werden. Bevor wir dies tun, müssen wir jedoch zuerst die Geschäftslogik Schicht aktualisieren, um eine Methode aufzunehmen, die die DAL s-`GetProductsWithPriceQuartile` Methode aufruft. Wir aktualisieren die nächste BLL-Datei in Schritt 3 und erstellen dann die ASP.NET-Seite in Schritt 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Schritt 3: Erweitern der Geschäftslogik Ebene

Bevor wir die neue `GetProductsWithPriceQuartile`-Methode aus der Darstellungs Schicht verwenden, sollten wir zuerst der BLL eine entsprechende Methode hinzufügen. Öffnen Sie die Datei `ProductsBLLWithSprocs`-Klasse, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Wie bei den anderen Datenabruf Methoden in `ProductsBLLWithSprocs`Ruft die `GetProductsWithPriceQuartile`-Methode einfach die entsprechende `GetProductsWithPriceQuartile`-Methode der DAL auf und gibt Ihre Ergebnisse zurück.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Schritt 4: Anzeigen der Price Quartile-Informationen auf einer ASP.NET-Webseite

Mit dem Abschluss der BLL-Addition können Sie eine ASP.NET-Seite erstellen, auf der das Preis Quartil für die einzelnen Produkte angezeigt wird. Öffnen Sie die Seite `AddingColumns.aspx` im Ordner `AdvancedDAL`, und ziehen Sie eine GridView aus der Toolbox auf den Designer, und legen Sie die Eigenschaft `ID` auf `Products`fest. Binden Sie das GridView s-Smarttag an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren Sie die ObjectDataSource so, dass Sie die `ProductsBLLWithSprocs` Class s `GetProductsWithPriceQuartile`-Methode verwendet. Da dies ein Schreib geschütztes Raster ist, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbllwithsprocs-Klasse.](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Abbildung 9**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLLWithSprocs`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image25.png))

[![Abrufen von Produktinformationen von der getproductwithpreiquartile-Methode](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Abbildung 10**: Abrufen von Produktinformationen aus der `GetProductsWithPriceQuartile`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image28.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen wird von Visual Studio automatisch ein BoundField-oder CheckBoxField-Element für jedes der von der-Methode zurückgegebenen Datenfelder zur GridView hinzugefügt. Eines dieser Datenfelder ist `PriceQuartile`. Dies ist die Spalte, die der `ProductsDataTable` in Schritt 1 hinzugefügt wurde.

Bearbeiten Sie die GridView s-Felder, und entfernen Sie alle außer den `ProductName`, `UnitPrice`und `PriceQuartile` boundfields. Konfigurieren Sie das `UnitPrice` BoundField, um seinen Wert als Währung zu formatieren, und legen Sie die `UnitPrice` und `PriceQuartile` boundfields rechts-bzw. zentriert ausgerichtet. Aktualisieren Sie abschließend die verbleibenden boundfields-`HeaderText` Eigenschaften auf Product, Price und Price Quartile. Aktivieren Sie außerdem das Kontrollkästchen Sortierung aktivieren im GridView s-Smarttag.

Nach diesen Änderungen sollten das deklarative Markup der GridView-und ObjectDataSource-Objekte wie folgt aussehen:

[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

In Abbildung 11 wird diese Seite angezeigt, wenn Sie über einen Browser besucht werden. Beachten Sie, dass die Produkte anfänglich nach ihrem Preis in absteigender Reihenfolge sortiert werden, wobei jedem Produkt ein entsprechender `PriceQuartile` Wert zugewiesen wird. Selbstverständlich können diese Daten nach anderen Kriterien sortiert werden, wobei der Wert für den Wert "Price Quartile" immer noch der Rangfolge der Produkte in Bezug auf den Preis entspricht (siehe Abbildung 12).

[![die Produkte nach Ihren Preisen geordnet sind](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Abbildung 11**: die Produkte werden nach Ihren Preisen geordnet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image31.png))

[![die Produkte nach Ihren Namen geordnet sind.](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Abbildung 12**: die Produkte werden nach Ihren Namen geordnet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-additional-datatable-columns-vb/_static/image34.png))

> [!NOTE]
> Mit einigen wenigen Codezeilen könnten wir die GridView erweitern, sodass Sie die Produkt Zeilen auf Grundlage ihres `PriceQuartile` Werts färbt. Wir könnten diese Produkte im ersten Quartil als hell grün, die im zweiten Quartil als hell gelb usw. Einfärben. Ich empfehle Ihnen, sich einen Moment Zeit zu nehmen, um diese Funktionalität hinzuzufügen. Wenn Sie ein Aktualisierungs Programm zum Formatieren von GridView benötigen, lesen Sie das Tutorial [benutzerdefinierte Formatierung basierend auf den Daten](../custom-formatting/custom-formatting-based-upon-data-vb.md) .

## <a name="an-alternative-approach---creating-another-tableadapter"></a>Eine alternative Vorgehensweise: Erstellen eines weiteren TableAdapter

Wie in diesem Tutorial gezeigt, können wir beim Hinzufügen einer Methode zu einem TableAdapter, der andere Datenfelder zurückgibt als die von der Haupt Abfrage geschriebenen Datenfelder, der Datentabelle entsprechende Spalten hinzufügen. Ein solcher Ansatz funktioniert jedoch nur, wenn eine kleine Anzahl von Methoden im TableAdapter vorhanden ist, die unterschiedliche Datenfelder zurückgeben und die alternativen Datenfelder nicht zu stark von der Haupt Abfrage abweichen.

Anstatt Spalten der Datentabelle hinzuzufügen, können Sie stattdessen einen weiteren TableAdapter zum DataSet hinzufügen, das die Methoden aus dem ersten TableAdapter enthält, die unterschiedliche Datenfelder zurückgeben. In diesem Tutorial können Sie dem DataSet mit dem Namen `ProductsWithPriceQuartileTableAdapter`, von dem die gespeicherte Prozedur `Products_SelectWithPriceQuartile` als Haupt Abfrage verwendet wurde, einen zusätzlichen TableAdapter hinzufügen, anstatt dem `ProductsDataTable` die `PriceQuartile` Spalte hinzuzufügen (wo Sie nur von der `GetProductsWithPriceQuartile`-Methode verwendet wird). ASP.NET Seiten, die benötigt werden, um Produktinformationen mit dem Price Quartil zu erhalten, verwenden die `ProductsWithPriceQuartileTableAdapter`, während diejenigen, die die `ProductsTableAdapter`nicht weiter verwenden konnten.

Wenn Sie einen neuen TableAdapter hinzufügen, bleiben die DataTables unbeschädigt, und ihre Spalten spiegeln genau die Datenfelder wider, die von den TableAdapter s-Methoden zurückgegeben werden. Zusätzliche TableAdapters können jedoch wiederkehrende Aufgaben und Funktionen mit sich bringen. Wenn z. b. ASP.NET Seiten, die die `PriceQuartile` Spalte angezeigt haben, auch für die Unterstützung von INSERT-, Update-und DELETE-Unterstützung benötigt werden, müssen die `InsertCommand`, `UpdateCommand`und `DeleteCommand` Eigenschaften der `ProductsWithPriceQuartileTableAdapter` ordnungsgemäß konfiguriert sein. Während diese Eigenschaften die `ProductsTableAdapter` s spiegeln, führt diese Konfiguration einen zusätzlichen Schritt ein. Außerdem gibt es jetzt zwei Möglichkeiten, um der Datenbank ein Produkt zu aktualisieren, zu löschen oder hinzuzufügen: über die Klassen `ProductsTableAdapter` und `ProductsWithPriceQuartileTableAdapter`.

Der Download für dieses Tutorial enthält eine `ProductsWithPriceQuartileTableAdapter`-Klasse im `NorthwindWithSprocs` DataSet, die diese alternative Vorgehensweise veranschaulicht.

## <a name="summary"></a>Summary

In den meisten Szenarien geben alle Methoden in einem TableAdapter denselben Satz von Datenfeldern zurück, aber es gibt Situationen, in denen eine bestimmte Methode oder zwei möglicherweise ein zusätzliches Feld zurückgeben müssen. Beispielsweise wurde im [Master/Detail mithilfe einer aufzurufenden Liste von Master Datensätzen mit einem DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) -Lernprogramm "Details" eine Methode zum `CategoriesTableAdapter` hinzugefügt, die zusätzlich zu den Hauptdaten Feldern der Abfrage ein `NumberOfProducts` Feld zurückgegeben hat, das die Anzahl der der jeweiligen Kategorie zugeordneten Produkte zurückgegeben hat. In diesem Tutorial haben wir das Hinzufügen einer Methode in der `ProductsTableAdapter` untersucht, die zusätzlich zu den Hauptdaten Feldern der Abfrage ein `PriceQuartile` Feld zurückgegeben hat. Um zusätzliche Datenfelder aufzuzeichnen, die von den TableAdapter s-Methoden zurückgegeben werden, müssen die entsprechenden Spalten der Datentabelle hinzugefügt werden.

Wenn Sie das manuelle Hinzufügen von Spalten zur Datentabelle planen, wird empfohlen, dass der TableAdapter gespeicherte Prozeduren verwendet. Wenn der TableAdapter Ad-hoc-SQL-Anweisungen verwendet, werden bei jedem Ausführen des TableAdapter-Konfigurations-Assistenten alle Methoden Daten Feld Listen auf die von der Haupt Abfrage zurückgegebenen Datenfelder zurückgesetzt. Dieses Problem wird nicht auf gespeicherte Prozeduren ausgedehnt, weshalb Sie empfohlen werden und in diesem Tutorial verwendet wurden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Prüfer für dieses Tutorial waren Randy Schmidt, Jacky Goor, Bernadette Leigh und Hilton giesreow. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](updating-the-tableadapter-to-use-joins-vb.md)
> [Weiter](working-with-computed-columns-vb.md)
