---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Master/Detail mithilfe einer Auflistungs Liste mit Master Datensätzen mit einem Detail DataList (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial komprimieren wir den zweiseitigen Master/Detail-Bericht des vorherigen Tutorials in eine einzelne Seite, die eine aufauflistungs Liste mit den Kategorien Amen für "t..." anzeigt.
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 81d72c666925e89729464e7ea696bde8d323e277
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78490689"
---
# <a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Master-/Detailbericht mit einer Aufzählung der Masterdatensätze und einem DataList-Steuerelement für die Details (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) oder [PDF herunterladen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> In diesem Tutorial komprimieren wir den zweiseitigen Master/Detail-Bericht des vorherigen Tutorials in eine einzelne Seite, auf der eine Auflistungs Liste mit den Kategorien Amen auf der linken Seite des Bildschirms und die Produkte der ausgewählten Kategorie auf der rechten Seite des Bildschirms angezeigt werden.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](master-detail-filtering-acess-two-pages-datalist-vb.md) wurde erläutert, wie Sie einen Master-/Detailbericht auf zwei Seiten aufteilen. Auf der Master Seite haben wir ein Repeater-Steuerelement verwendet, um eine aufzurufene Liste von Kategorien zu erzeugen. Jeder Kategoriename war ein Hyperlink, bei dem der Benutzer auf die Detailseite klickt, auf der ein zweispaltige DataList die Produkte zeigte, die zur ausgewählten Kategorie gehören.

In diesem Lernprogramm komprimieren wir das bidirektionale Tutorial in eine einzelne Seite, die eine Auflistungs Liste der Kategorien Amen auf der linken Seite des Bildschirms anzeigt, wobei jeder Kategoriename als LinkButton gerendert wird. Wenn Sie auf eine der Kategorienamen Link Buttons klicken, wird ein Postback ausgelöst, und die ausgewählten kategorieprodukte werden auf der rechten Seite des Bildschirms an einen zwei spaltigen DataList gebunden. Zusätzlich zum Anzeigen der Namen der einzelnen Kategorien zeigt der Repeater auf der linken Seite an, wie viele Produkte für eine bestimmte Kategorie vorhanden sind (siehe Abbildung 1).

[![der Name der Kategorie und die Gesamtzahl der Produkte auf der linken Seite angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Abbildung 1**: der Name der Kategorie und die Gesamtzahl der Produkte werden auf der linken Seite angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))

## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Schritt 1: Anzeigen eines Wiederholungs Moduls im linken Bereich des Bildschirms

Für dieses Lernprogramm muss die aufzurufene Liste der Kategorien links neben den Produkten der ausgewählten Kategorie angezeigt werden. Inhalt innerhalb einer Webseite kann mithilfe von standardmäßigen HTML-Elementen, Absatz Tags, nicht unterbrechenden Leerzeichen, `<table>` s usw. oder über Cascading Stylesheet (CSS)-Techniken positioniert werden. Alle unsere Tutorials haben bisher CSS-Techniken für die Positionierung verwendet. Als wir die Navigations Benutzeroberfläche auf unserer Master Seite im Lernprogramm zu [Masterseiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-vb.md) erstellt haben, wurde die *absolute Positionierung*verwendet, die den exakten Pixel Offset für die Navigationsliste und den Hauptinhalt anzeigt. Alternativ kann CSS verwendet werden, um ein Element nach rechts oder Links von einem anderen zu positionieren *.* Wir können die aufzurufende Liste der Kategorien auf der linken Seite der ausgewählten Category-Produkte anzeigen, indem Sie den Repeater Links vom DataList-Steuerelemente

Öffnen Sie die Seite `CategoriesAndProducts.aspx` aus dem Ordner `DataListRepeaterFiltering`, und fügen Sie der Seite einen Repeater und einen DataList hinzu. Legen Sie die `ID` der Repeater auf `Categories` und die DataList s auf `CategoryProducts`fest. Wechseln Sie zur Quell Ansicht, und platzieren Sie die Steuerelemente Repeater und DataList in ihren eigenen `<div>` Elementen. Das heißt, dass Sie den Repeater zuerst in ein `<div>` Element und dann den DataList in seinem eigenen `<div>` Element direkt nach dem Repeater einschließen. Das Markup sollte in etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Um das Wiederholungs Modul auf der linken Seite des DataList-Steuerelemente zu verwenden, muss das `float` CSS-Format Attribut verwendet werden, wie im folgenden Beispiel:

[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

Der `float: left;` die das erste `<div>` Element links von der zweiten. Die `width`-und `padding-right` Einstellungen geben die ersten `<div>` s `width` an und wie viel Auffüll Zeichen zwischen dem Inhalt des `<div>` Elements und seinem rechten Rand hinzugefügt werden. Weitere Informationen zu unverankerten Elementen in CSS finden Sie unter [floatutorial](http://css.maxdesign.com.au/floatutorial/).

Anstatt die Stileinstellungen direkt über das erste `<p>` Element s `style` Attribut anzugeben, können Sie stattdessen eine neue CSS-Klasse in `Styles.css` namens `FloatLeft`erstellen:

[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Anschließend können wir die `<div>` durch `<div class="FloatLeft">`ersetzen.

Nachdem Sie die CSS-Klasse hinzugefügt und das Markup auf der `CategoriesAndProducts.aspx` Seite konfiguriert haben, wechseln Sie zum Designer. Der Repeater sollte auf der linken Seite des DataList-Steuerelemente enthalten sein (Obwohl zurzeit beide nur als graue Felder angezeigt werden, da wir noch keine Datenquellen oder Vorlagen konfigurieren).

[![der Repeater auf der linken Seite des DataList-](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Abbildung 2**: der Repeater wird auf der linken Seite des DataList-Steuermoduls angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))

## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Schritt 2: Bestimmen der Anzahl von Produkten für jede Kategorie

Nachdem der Repeater und der DataList das Markup abgeschlossen haben, sind wir bereit, die Kategoriedaten an das Repeater-Steuerelement zu binden. In der aufzurufenden Liste der Kategorien in Abbildung 1 wird jedoch zusätzlich zu den einzelnen Kategoriennamen auch die Anzahl der Produkte angezeigt, die der Kategorie zugeordnet sind. Für den Zugriff auf diese Informationen können Sie folgende Aktionen ausführen:

- **Legen Sie diese Informationen aus der Code Behind-Klasse der ASP.net page s fest.** Bei einem bestimmten *`categoryID`* können wir die Anzahl der zugeordneten Produkte ermitteln, indem wir die `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode aufrufen. Diese Methode gibt ein `ProductsDataTable` Objekt zurück, dessen `Count`-Eigenschaft angibt, wie viele `ProductsRow` s vorhanden sind. Dies ist die Anzahl der Produkte für die angegebene *`categoryID`* . Wir können einen `ItemDataBound` Ereignishandler für den Repeater erstellen, der für jede Kategorie, die an den Repeater gebunden ist, die `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode aufruft und die Anzahl in die Ausgabe einschließt.
- **Aktualisieren Sie die `CategoriesDataTable` im typisierten DataSet, um eine `NumberOfProducts` Spalte einzuschließen.** Anschließend können wir die `GetCategories()` Methode im `CategoriesDataTable` aktualisieren, um diese Informationen einzuschließen. Alternativ können Sie `GetCategories()` unverändert belassen und eine neue `CategoriesDataTable` Methode namens `GetCategoriesAndNumberOfProducts()`erstellen.

Sehen Sie sich diese beiden Techniken an. Der erste Ansatz ist einfacher zu implementieren, da wir die Datenzugriffs Schicht nicht aktualisieren müssen. Allerdings ist eine höhere Kommunikation mit der Datenbank erforderlich. Der aufzurufende Aufrufe der `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode im `ItemDataBound`-Ereignishandler fügt für jede im Wiederholungs Modul angezeigte Kategorie einen zusätzlichen Daten Bank Rückruf hinzu. Bei dieser Technik gibt es *n* + 1 Daten Bank Aufrufe, wobei *n* die Anzahl der im Wiederholungs Modul angezeigten Kategorien ist. Beim zweiten Ansatz wird die Produkt Anzahl mit Informationen zu jeder Kategorie aus der `CategoriesBLL` Class `GetCategories()` (oder `GetCategoriesAndNumberOfProducts()`)-Methode zurückgegeben, sodass nur ein Trip zur Datenbank entsteht.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Bestimmen der Anzahl von Produkten im ItemDataBound-Ereignis Handler

Zum Ermitteln der Anzahl von Produkten für jede Kategorie im Wiederholungs-`ItemDataBound` Ereignishandler sind keine Änderungen an der vorhandenen Datenzugriffs Ebene erforderlich. Alle Änderungen können direkt auf der `CategoriesAndProducts.aspx` Seite vorgenommen werden. Fügen Sie zunächst eine neue ObjectDataSource mit dem Namen "`CategoriesDataSource`" über das "Repeater s"-Smarttag hinzu. Konfigurieren Sie als nächstes die `CategoriesDataSource` ObjectDataSource, damit Sie die Daten aus der `CategoriesBLL` Class s `GetCategories()`-Methode abruft.

[![Konfigurieren von ObjectDataSource für die Verwendung der Methode GetCategories () der kategoriesbll-Klasse](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Abbildung 3**: Konfigurieren von ObjectDataSource für die Verwendung der `CategoriesBLL` Class s `GetCategories()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))

Jedes Element im `Categories` Repeater muss klickbar sein und bewirkt, dass der `CategoryProducts` DataList diese Produkte für die ausgewählte Kategorie anzeigt, wenn darauf geklickt wird. Dies kann erreicht werden, indem jede Kategorie zu einem Hyperlink wird, eine Verknüpfung mit der gleichen Seite (`CategoriesAndProducts.aspx`) erfolgt, aber die `CategoryID` über die QueryString übergeben wird, ähnlich wie im vorherigen Tutorial. Der Vorteil dieses Ansatzes besteht darin, dass eine Seite, die ein bestimmtes Produkt der Kategorie anzeigt, mit einem Lesezeichen versehen und von einer Suchmaschine indiziert werden kann.

Alternativ können wir jede Kategorie zu einem LinkButton machen. Dies ist der Ansatz, den wir für dieses Tutorial verwenden werden. Der LinkButton wird im Browser des Benutzers als Hyperlink gerendert. Wenn Sie darauf klicken, wird ein Postback ausgelöst. beim Postback muss die DataList s ObjectDataSource aktualisiert werden, um die Produkte anzuzeigen, die zur ausgewählten Kategorie gehören. In diesem Tutorial ist die Verwendung eines Links sinnvoller als die Verwendung eines Link Button. Es gibt jedoch möglicherweise andere Szenarien, in denen die Verwendung von LinkButton vorteilhafter ist. Während der Hyperlink-Ansatz für dieses Beispiel ideal wäre, sollten Sie stattdessen die Verwendung von LinkButton durchsuchen. Wie wir sehen werden, führt die Verwendung von LinkButton zu einigen Herausforderungen, die sonst nicht mit einem Hyperlink entstehen würden. Daher werden diese Herausforderungen durch die Verwendung eines LinkButton in diesem Tutorial hervorgehoben und Lösungen für Szenarien bereitgestellt, in denen wir möglicherweise einen Link Button anstelle eines Links verwenden möchten.

> [!NOTE]
> Es wird empfohlen, dieses Tutorial mit einem Hyperlink-Steuerelement oder `<a>` Element anstelle von LinkButton zu wiederholen.

Das folgende Markup zeigt die deklarative Syntax für "Repeater" und "ObjectDataSource". Beachten Sie, dass die Repeater s-Vorlagen eine Auflistungs Liste mit jedem Element als LinkButton Rendering:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Für dieses Lernprogramm muss der Ansichts Zustand des Wiederholungs Moduls aktiviert sein (Beachten Sie, dass die `EnableViewState="False"` aus der deklarativen Syntax von Repeater) aktiviert ist. In Schritt 3 erstellen wir einen Ereignishandler für das Repeater s-`ItemCommand` Ereignis, in dem wir die DataList s-Sammlung "ObjectDataSource s `SelectParameters`" aktualisieren. Der Repeater-`ItemCommand`wird jedoch nicht ausgelöst, wenn der Ansichts Zustand deaktiviert ist. Weitere Informationen dazu, warum [der Ansichts](http://scottonwriting.net/sowblog/posts/1263.aspx) Zustand aktiviert werden muss, damit ein Wiederholungs-`ItemCommand` Ereignis ausgelöst wird, finden Sie in der Übersicht über eine ASP.net-Frage und [deren Lösung](http://scottonwriting.net/sowBlog/posts/1268.aspx) .

Für den Link Button mit dem `ID`-Eigenschafts Wert `ViewCategory` ist dessen `Text`-Eigenschaft nicht festgelegt. Wenn wir nur den Kategorien Amen anzeigen wollten, hätten wir die Text-Eigenschaft deklarativ über die Datenbindung-Syntax wie folgt festgelegt:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Wir möchten jedoch sowohl den Namen der Kategorie *als auch* die Anzahl der Produkte anzeigen, die zu dieser Kategorie gehören. Diese Informationen können aus dem Repeater-`ItemDataBound` Ereignishandler abgerufen werden, indem die `ProductBLL` Class s `GetCategoriesByProductID(categoryID)`-Methode aufgerufen und festgelegt wird, wie viele Datensätze in der resultierenden `ProductsDataTable`zurückgegeben werden, wie im folgenden Code veranschaulicht:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Wir beginnen damit, sicherzustellen, dass wir mit einem Datenelement arbeiten (eines, dessen `ItemType` `Item` oder `AlternatingItem`ist) und dann auf die `CategoriesRow` Instanz verweisen, die gerade an den aktuellen `RepeaterItem`gebunden wurde. Als nächstes bestimmen wir die Anzahl der Produkte für diese Kategorie, indem wir eine Instanz der `ProductsBLL`-Klasse erstellen, die `GetCategoriesByProductID(categoryID)`-Methode aufrufen und die Anzahl der zurückgegebenen Datensätze mithilfe der `Count`-Eigenschaft bestimmen. Zum Schluss ist die `ViewCategory` LinkButton in ItemTemplate Verweise, und die `Text`-Eigenschaft ist auf *CategoryName* ("*numofproductsincategory*") festgelegt, wobei " *numofproductsincategory* " als Zahl mit 0 (null) Dezimalstellen formatiert ist.

> [!NOTE]
> Alternativ könnten Sie der Code Behind-Klasse der ASP.net page s eine *Formatierungsfunktion* hinzufügen, die eine Kategorie s `CategoryName` und `CategoryID` Werte akzeptiert und die `CategoryName` zurückgibt, die mit der Anzahl der Produkte in der Kategorie verkettet ist (wie durch Aufrufen der `GetCategoriesByProductID(categoryID)`-Methode festgelegt). Die Ergebnisse einer solchen Formatierungsfunktion können der Text Eigenschaft "LinkButton s" deklarativ zugewiesen werden, da der `ItemDataBound`-Ereignishandler nicht erforderlich ist. Weitere Informationen zur Verwendung von Formatierungsfunktionen finden Sie unter [Verwenden von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Element oder [Formatieren des DataList-und Repeater auf der Grundlage von datentutorials](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) .

Nehmen Sie sich nach dem Hinzufügen dieses Ereignis Handlers einen Moment Zeit, um die Seite über einen Browser zu testen. Beachten Sie, wie jede Kategorie in einer Auflistungs Liste aufgeführt wird, wobei der Name der Kategorie und die Anzahl der der Kategorie zugeordneten Produkte angezeigt wird (siehe Abbildung 4).

[![werden die Kategorien "Name" und "Anzahl der Produkte" angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Abbildung 4**: jeder Name und die Anzahl der Produkte der Kategorie werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))

## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Aktualisieren der`CategoriesDataTable`und`CategoriesTableAdapter`, um die Anzahl der Produkte für jede Kategorie einzuschließen

Anstatt die Anzahl der Produkte für jede Kategorie zu ermitteln, die an den Wiederholungs Dienst gebunden ist, können wir diesen Prozess optimieren, indem wir die `CategoriesDataTable` und `CategoriesTableAdapter` in der Datenzugriffs Schicht so anpassen, dass diese Informationen nativ enthalten sind. Um dies zu erreichen, müssen wir `CategoriesDataTable` eine neue Spalte hinzufügen, die die Anzahl der zugeordneten Produkte enthält. Wenn Sie einer Datentabelle eine neue Spalte hinzufügen möchten, öffnen Sie das typisierte DataSet (`App_Code\DAL\Northwind.xsd`), klicken Sie mit der rechten Maustaste auf die zu ändernde Datentabelle, und wählen Sie hinzufügen/Spalte aus. Fügen Sie dem `CategoriesDataTable` eine neue Spalte hinzu (siehe Abbildung 5).

[![der kategoriesdatasource eine neue Spalte hinzufügen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Abbildung 5**: Hinzufügen einer neuen Spalte zum `CategoriesDataSource` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))

Dadurch wird eine neue Spalte mit dem Namen `Column1`hinzugefügt, die Sie ändern können, indem Sie einfach einen anderen Namen eingeben. Benennen Sie diese neue Spalte in `NumberOfProducts`um. Als nächstes müssen wir diese Spalten Eigenschaften konfigurieren. Klicken Sie auf die neue Spalte, und wechseln Sie zum Eigenschaftenfenster. Ändern Sie die Spalte s `DataType`-Eigenschaft von `System.String` in `System.Int32`, und legen Sie die `ReadOnly`-Eigenschaft auf `True`fest, wie in Abbildung 6 dargestellt.

![Legen Sie die DataType-Eigenschaft und die Read Only-Eigenschaft der neuen Spalte fest.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Abbildung 6**: Festlegen der Eigenschaften für `DataType` und `ReadOnly` der neuen Spalte

Während der `CategoriesDataTable` jetzt über eine `NumberOfProducts` Spalte verfügt, wird sein Wert nicht von einer der entsprechenden TableAdapter s-Abfragen festgelegt. Wir können die `GetCategories()`-Methode aktualisieren, um diese Informationen zurückzugeben, wenn diese Informationen bei jedem Abruf von Kategorieinformationen zurückgegeben werden sollen. Wenn Sie jedoch nur die Anzahl der zugeordneten Produkte für die Kategorien in seltenen Fällen (z. b. nur für dieses Tutorial) erfassen müssen, können wir `GetCategories()` unverändert lassen und eine neue Methode erstellen, die diese Informationen zurückgibt. Verwenden Sie diesen letzteren Ansatz, und erstellen Sie eine neue Methode mit dem Namen `GetCategoriesAndNumberOfProducts()`.

Um diese neue `GetCategoriesAndNumberOfProducts()` Methode hinzuzufügen, klicken Sie mit der rechten Maustaste auf den `CategoriesTableAdapter`, und wählen Sie neue Abfrage aus. Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen geöffnet, der in vorherigen Tutorials mehrmals verwendet wurde. Starten Sie den Assistenten für diese Methode, indem Sie angeben, dass die Abfrage eine Ad-hoc-SQL-Anweisung verwendet, die Zeilen zurückgibt.

[![Erstellen der Methode mit einer Ad-hoc-SQL-Anweisung](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Abbildung 7**: Erstellen der Methode mit einer Ad-hoc-SQL-Anweisung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))

[![die SQL-Anweisung Zeilen zurückgibt](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Abbildung 8**: die SQL-Anweisung gibt Zeilen zurück ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))

Im nächsten Assistenten werden Sie aufgefordert, die Abfrage zu verwenden. Um die einzelnen Kategorien `CategoryID`, `CategoryName`und `Description` sowie die Anzahl der der Kategorie zugeordneten Produkte zurückzugeben, verwenden Sie die folgende `SELECT`-Anweisung:

[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]

[![die zu verwendende Abfrage angeben](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Abbildung 9**: Angeben der zu verwendenden Abfrage ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))

Beachten Sie, dass die Unterabfrage, die die Anzahl der Produkte berechnet, die der Kategorie zugeordnet sind, mit `NumberOfProducts`Alias versehen ist. Diese namens Übereinstimmung bewirkt, dass der Wert, der von dieser Unterabfrage zurückgegeben wird, der `CategoriesDataTable` s `NumberOfProducts` Spalte zugeordnet wird.

Nachdem Sie diese Abfrage eingegeben haben, besteht der letzte Schritt darin, den Namen für die neue Methode auszuwählen. Verwenden Sie `FillWithNumberOfProducts` und `GetCategoriesAndNumberOfProducts` für die Datentabelle ausfüllen und Datentabelle zurückgeben.

[![benennen Sie die neuen TableAdapter s-Methoden fillwithnumofproducts und getcategoriesandnumofproducts.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Abbildung 10**: Benennen der neuen TableAdapter s-Methoden `FillWithNumberOfProducts` und `GetCategoriesAndNumberOfProducts` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))

An diesem Punkt wurde die Datenzugriffs Ebene um die Anzahl der Produkte pro Kategorie erweitert. Da all unsere Präsentationsschicht alle Aufrufe an die DAL über eine separate Geschäftslogik Schicht weiterleitet, müssen wir der `CategoriesBLL` Klasse eine entsprechende `GetCategoriesAndNumberOfProducts` Methode hinzufügen:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Nachdem die DAL und die BLL abgeschlossen sind, können wir diese Daten an den `Categories` Repeater in `CategoriesAndProducts.aspx`binden! Wenn Sie bereits eine ObjectDataSource für den Repeater aus dem Abschnitt Ermitteln der Anzahl der Produkte im `ItemDataBound`-Ereignis Handler erstellt haben, löschen Sie diese ObjectDataSource, und entfernen Sie die `DataSourceID` Eigenschafts Einstellung für Repeater s. Entfernen Sie außerdem das Repeater-`ItemDataBound` Ereignis vom Ereignishandler, indem Sie die `Handles Categories.OnItemDataBound`-Syntax in der ASP.NET-Code Behind-Klasse entfernen.

Fügen Sie mit dem Repeater zurück in seinen ursprünglichen Zustand einen neuen ObjectDataSource mit dem Namen "`CategoriesDataSource`" über das Smarttag "Repeater s" hinzu. Konfigurieren Sie ObjectDataSource so, dass die `CategoriesBLL`-Klasse verwendet wird. Wenn Sie jedoch nicht die `GetCategories()`-Methode verwenden möchten, sollten Sie stattdessen `GetCategoriesAndNumberOfProducts()` verwenden (siehe Abbildung 11).

[![konfigurieren Sie ObjectDataSource für die Verwendung der getcategoriesandnumofproducts-Methode.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Abbildung 11**: Konfigurieren von ObjectDataSource für die Verwendung der `GetCategoriesAndNumberOfProducts`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))

Aktualisieren Sie anschließend die `ItemTemplate` so, dass die Eigenschaft "LinkButton s `Text`" deklarativ mithilfe der Datenbindung-Syntax zugewiesen wird, und schließt die Datenfelder `CategoryName` und `NumberOfProducts` ein. Das komplette deklarative Markup für das Repeater und den `CategoriesDataSource` ObjectDataSource folgendermaßen:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

Die Ausgabe, die durch Aktualisieren der dal zum Einschließen einer `NumberOfProducts` Spalte gerendert wird, ist identisch mit der Verwendung des `ItemDataBound` ereignishandleransatzes (siehe Abbildung 4, um einen Screenshot des Wiederholungs Moduls mit den Kategorien Amen und der Anzahl der Produkte anzuzeigen).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Schritt 3: Anzeigen der Produkte der ausgewählten Kategorie

An diesem Punkt haben wir den `Categories` Repeater, der die Liste der Kategorien zusammen mit der Anzahl der Produkte in jeder Kategorie anzeigt. Der Repeater verwendet für jede Kategorie einen LinkButton, bei dem auf einen Postback geklickt wird. an diesem Punkt müssen diese Produkte für die ausgewählte Kategorie im `CategoryProducts` DataList angezeigt werden.

Eine Herausforderung besteht darin, dass der DataList nur die Produkte für die ausgewählte Kategorie anzeigt. In der [Master-/Detail-Verwendung eines auswählbaren Master GridView-Tutorials mit einem Detail DetailsView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) -Tutorial haben wir gesehen, wie Sie eine GridView erstellen können, deren Zeilen ausgewählt werden können, wobei die Details der ausgewählten Zeile in einer DetailsView auf der gleichen Seite angezeigt werden. Die GridView s ObjectDataSource hat Informationen über alle Produkte zurückgegeben, die die `ProductsBLL` s `GetProducts()`-Methode verwenden, während die DetailsView s ObjectDataSource mithilfe der `GetProductsByProductID(productID)`-Methode Informationen über das ausgewählte Produkt abgerufen hat. Der *`productID`* Parameterwert wurde deklarativ bereitgestellt, indem er dem Wert der GridView s `SelectedValue`-Eigenschaft zugeordnet wurde. Leider verfügt der Repeater nicht über eine `SelectedValue`-Eigenschaft und kann nicht als Parameter Quelle fungieren.

> [!NOTE]
> Dies ist eine der Herausforderungen, die bei der Verwendung von LinkButton in einem Repeater angezeigt werden. Hätten wir stattdessen einen Hyperlink verwendet, um die `CategoryID` über QueryString zu übergeben. wir könnten dieses QueryString-Feld als Quelle für den Wert des Parameters s verwenden.

Bevor wir uns über das Fehlen einer `SelectedValue`-Eigenschaft für den Repeater Gedanken machen, binden Sie den DataList zunächst an eine ObjectDataSource, und geben Sie dessen `ItemTemplate`an.

Wählen Sie aus dem DataList s-Smarttag eine neue ObjectDataSource mit dem Namen `CategoryProductsDataSource` aus, und konfigurieren Sie Sie so, dass Sie die `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode verwendet. Da der DataList in diesem Tutorial eine schreibgeschützte Schnittstelle bietet, können Sie die Dropdown Listen in den Registerkarten einfügen, aktualisieren und löschen auf (keine) festlegen.

[![konfigurieren Sie ObjectDataSource für die Verwendung der Methode getproductbycategoryid (CategoryID) der productbll-Klasse.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Abbildung 12**: Konfigurieren von ObjectDataSource für die Verwendung `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))

Da die `GetProductsByCategoryID(categoryID)`-Methode einen Eingabeparameter erwartet ( *`categoryID`* ), können Sie mit dem Assistenten zum Konfigurieren von Datenquellen den Parameter s Source angeben. Waren die Kategorien in einem GridView-oder DataList-Element aufgelistet, legen wir die Dropdown Liste Parameter Quelle auf Control und die ControlID auf den `ID` des datenweb-Steuer Elements fest. Da der Repeater jedoch keine `SelectedValue` Eigenschaft hat, kann er nicht als Parameter Quelle verwendet werden. Wenn Sie überprüfen, werden Sie feststellen, dass die ControlID-Dropdown Liste nur ein Steuerelement `ID``CategoryProducts`, die `ID` des DataList-Steuer Elements enthält.

Legen Sie vorerst die Dropdown Liste Parameter Quelle auf None fest. Wir werden diesen Parameterwert Programm gesteuert zuweisen, wenn im Wiederholungs Modul auf eine Kategorie-Link Schaltfläche geklickt wird.

[![keine Parameter Quelle für den CategoryID-Parameter angeben.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Abbildung 13**: Geben Sie keine Parameter Quelle für den *`categoryID`* -Parameter an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png)).

Nachdem Sie den Assistenten zum Konfigurieren von Datenquellen abgeschlossen haben, generiert Visual Studio automatisch die DataList-`ItemTemplate`. Ersetzen Sie diese Standard `ItemTemplate` durch die Vorlage, die wir im vorherigen Tutorial verwendet haben. Legen Sie außerdem die DataList s-`RepeatColumns`-Eigenschaft auf 2 fest. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup für Ihren DataList und die zugehörige ObjectDataSource wie folgt aussehen:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Derzeit wird der `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* Parameter nie festgelegt, sodass beim Anzeigen der Seite keine Produkte angezeigt werden. Wir müssen diesen Parameterwert auf der Grundlage des `CategoryID` der im Wiederholungs Modul angeklickten Kategorie festlegen. Dies führt zu zwei Herausforderungen: zuerst wird ermittelt, wann auf einen Link Button in den repeatater s `ItemTemplate` geklickt wurde. und zweitens: wie können wir die `CategoryID` der entsprechenden Kategorie ermitteln, auf deren LinkButton geklickt wurde?

Die LinkButton-Steuerelemente wie die Schaltflächen-und ImageButton-Steuerelemente haben ein `Click` Ereignis und ein [`Command`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). Das `Click` Ereignis ist so konzipiert, dass einfach festzustellen ist, dass auf Link Button geklickt wurde. Manchmal müssen Sie jedoch zusätzlich zu feststellen, dass auf Link Button geklickt wurde, auch einige zusätzliche Informationen an den Ereignishandler übergeben. Wenn dies der Fall ist, können die Eigenschaften von "LinkButton s [`CommandName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) " und " [`CommandArgument`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) " diese zusätzlichen Informationen zugewiesen werden. Wenn dann auf Link Button geklickt wird, wird das `Command` Ereignis ausgelöst (anstelle des `Click` Ereignisses), und dem Ereignishandler werden die Werte der Eigenschaften `CommandName` und `CommandArgument` übermittelt.

Wenn ein `Command` Ereignis aus einer Vorlage im Wiederholungs Modul ausgelöst wird, wird das Ereignis Repeater s [`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) ausgelöst, und die `CommandName` und `CommandArgument` Werte der angeklickten LinkButton (oder Schaltfläche oder ImageButton) werden an Sie übermittelt. Daher müssen Sie Folgendes tun, um zu bestimmen, wann auf eine kategorielinkschaltfläche im Wiederholungs Modul geklickt wurde:

1. Legen Sie die `CommandName`-Eigenschaft des LinkButton in der Repeater-`ItemTemplate` auf einen Wert fest (Ich habe "listproducts" verwendet). Wenn Sie diesen `CommandName` Wert festlegen, wird das Ereignis "LinkButton s `Command`" ausgelöst, wenn auf Link Button geklickt wird.
2. Legen Sie die `CommandArgument`-Eigenschaft von LinkButton auf den Wert des aktuellen Elements fest `CategoryID`.
3. Erstellen Sie einen Ereignishandler für das Ereignis "Repeater s `ItemCommand`". Legen Sie im-Ereignishandler den `CategoryProductsDataSource` ObjectDataSource-`CategoryID` Parameter auf den Wert des übergebenen `CommandArgument`fest.

Im folgenden `ItemTemplate` Markup für den kategorierepeater werden die Schritte 1 und 2 implementiert. Beachten Sie, dass dem `CommandArgument` Wert die Datenelemente `CategoryID` mithilfe der Datenbindung-Syntax zugewiesen werden:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Jedes Mal, wenn ein `ItemCommand` Ereignishandler erstellt wird, ist es ratsam, immer zuerst den Wert für die eingehende `CommandName` zu überprüfen, da *jedes* `Command` Ereignis, das von einer Schaltfläche, einem LinkButton oder *einem* ImageButton im Wiederholungs Modul ausgelöst wird, das `ItemCommand` Ereignis auslöst. Obwohl zurzeit nur ein Link Button vorhanden ist, können wir (oder ein anderer Entwickler in unserem Team) dem Repeater weitere Schaltflächen-websteuer Elemente hinzufügen, die, wenn Sie darauf klicken, denselben `ItemCommand` Ereignishandler auslöst. Daher ist es am besten, immer sicherzustellen, dass Sie die `CommandName`-Eigenschaft überprüfen und nur mit der programmgesteuerten Logik fortfahren, wenn Sie mit dem erwarteten Wert übereinstimmt.

Nachdem sichergestellt wurde, dass der übergebenen `CommandName` Wert listproducts entspricht, weist der Ereignishandler den `CategoryProductsDataSource` ObjectDataSource-`CategoryID`-Parameter dem Wert der übergebenen `CommandArgument`zu. Diese Änderung an der ObjectDataSource s-`SelectParameters` bewirkt, dass sich der DataList automatisch erneut an die Datenquelle bindet und die Produkte für die neu ausgewählte Kategorie anzeigt.

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Mit diesen Ergänzungen ist unser Tutorial fertig! Nehmen Sie sich einen Moment Zeit, um Sie in einem Browser zu testen. In Abbildung 14 wird der Bildschirm beim ersten Besuch der Seite angezeigt. Da eine Kategorie noch ausgewählt werden kann, werden keine Produkte angezeigt. Wenn Sie auf eine Kategorie (z. b. "wird erzeugt") klicken, werden diese Produkte in der Produktkategorie in einer zwei spaltigen Ansicht angezeigt (siehe Abbildung 15).

[beim ersten Besuch der Seite ![keine Produkte angezeigt.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Abbildung 14**: beim ersten Besuch der Seite werden keine Produkte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))

[![klicken auf die Kategorie "Produkte" listet die passenden Produkte auf der rechten Seite auf.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Abbildung 15**: Klicken auf die Kategorie "Produktion" listet die passenden Produkte auf der rechten Seite auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))

## <a name="summary"></a>Zusammenfassung

Wie in diesem Tutorial erläutert, können Master/Detail-Berichte auf zwei Seiten verteilt oder konsolidiert werden. Wenn Sie einen Master-/Detailbericht auf einer einzelnen Seite anzeigen, werden jedoch einige Herausforderungen hinsichtlich der optimalen Anordnung der Master-und Detaildaten Sätze auf der Seite eingeführt. In der *Master-/Details-Verwendung eines auswählbaren Master GridView-Tutorials mit einem Detail DetailsView* -Tutorial haben wir die Detaildaten Sätze oberhalb der Master Datensätze angezeigt. in diesem Tutorial haben wir CSS-Techniken verwendet, damit die Master Datensätze auf der linken Seite der Details verankert werden.

Zusammen mit der Anzeige von Master-/Detailberichten hatten wir auch die Möglichkeit, zu untersuchen, wie die Anzahl der mit jeder Kategorie verknüpften Produkte abgerufen werden kann, und wie serverseitige Logik durchgeführt wird, wenn in einem Repeater auf eine linktaste (oder Schaltfläche oder ImageButton) geklickt wird.

In diesem Tutorial wird die Untersuchung von Master-/Detailberichten mit DataList und Repeater abgeschlossen. Im nächsten Tutorial wird veranschaulicht, wie Sie dem DataList-Steuerelement Bearbeitungs-und Löschfunktionen hinzufügen.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) ein Tutorial zu unverankerten CSS-Elementen mit CSS
- [CSS-Positionierung](http://www.brainjar.com/css/positioning/) Weitere Informationen zum Positionieren von Elementen mit CSS
- Anordnen [von Inhalten mit HTML](http://www.w3schools.com/html/html_layout.asp) mithilfe von `<table>` s und anderen HTML-Elementen für die Positionierung

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial war Zack Jones. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Previous](master-detail-filtering-acess-two-pages-datalist-vb.md)
