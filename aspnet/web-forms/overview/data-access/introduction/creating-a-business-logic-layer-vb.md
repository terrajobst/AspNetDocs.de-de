---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Erstellen einer Geschäftslogik Schicht (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie Sie Ihre Geschäftsregeln in eine Geschäftslogik Schicht (Business Logic Layer, BLL) zentralisieren, die als Vermittler für den Datenaustausch zwischen t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ee4789ea9567b7bcd70eb63695e0b1d73076dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78490197"
---
# <a name="creating-a-business-logic-layer-vb"></a>Erstellen einer Geschäftslogikebene (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) oder [PDF herunterladen](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> In diesem Tutorial erfahren Sie, wie Sie Ihre Geschäftsregeln in eine Geschäftslogik Schicht (Business Logic Layer, BLL) zentralisieren, die als Vermittler für den Datenaustausch zwischen der Darstellungs Schicht und der dal fungiert.

## <a name="introduction"></a>Einführung

Die im [ersten Tutorial](creating-a-data-access-layer-vb.md) erstellte Datenzugriffs Schicht (Data Access Layer, DAL) trennt die Datenzugriffs Logik von der Präsentationslogik. Obwohl die DAL die Datenzugriffs Details von der Darstellungs Schicht ordnungsgemäß trennt, erzwingt Sie keine Geschäftsregeln, die angewendet werden können. Beispielsweise möchten wir für die Anwendung möglicherweise nicht zulassen, dass die `CategoryID`-oder `SupplierID` Felder der `Products` Tabelle geändert werden, wenn das `Discontinued` Feld auf 1 festgelegt ist, oder es empfiehlt sich, Regeln für die Rangfolge zu erzwingen. Dies verbietet Situationen, in denen ein Mitarbeiter von einem Mitarbeiter verwaltet wird, der nach dem Mitarbeiter eingestellt wurde. Ein weiteres häufiges Szenario ist die Autorisierung, bei der möglicherweise nur Benutzer in einer bestimmten Rolle Produkte löschen oder den `UnitPrice` Wert ändern können.

In diesem Tutorial erfahren Sie, wie Sie diese Geschäftsregeln in eine Geschäftslogik Schicht (Business Logic Layer, BLL) zentralisieren, die als Vermittler für den Datenaustausch zwischen der Darstellungs Schicht und der dal fungiert. In einer realen Anwendung sollte die BLL als separates Klassen Bibliotheksprojekt implementiert werden. für diese Tutorials implementieren wir jedoch die BLL als eine Reihe von Klassen in unserem `App_Code` Ordner, um die Projektstruktur zu vereinfachen. Abbildung 1 zeigt die architektonischen Beziehungen zwischen der Darstellungs Schicht, der BLL und der DAL.

![Die BLL trennt die Präsentationsebene von der Datenzugriffs Ebene und erzwingt Geschäftsregeln.](creating-a-business-logic-layer-vb/_static/image1.png)

**Abbildung 1**: die BLL trennt die Präsentationsebene von der Datenzugriffs Ebene und legt Geschäftsregeln fest.

Anstatt separate Klassen zu erstellen, um unsere [Geschäftslogik](http://en.wikipedia.org/wiki/Business_logic)zu implementieren, können wir diese Logik wahlweise direkt im typisierten DataSet mit partiellen Klassen platzieren. Ein Beispiel für das Erstellen und Erweitern eines typisierten Datasets finden Sie im ersten Tutorial.

## <a name="step-1-creating-the-bll-classes"></a>Schritt 1: Erstellen der BLL-Klassen

Unsere BLL besteht aus vier Klassen, eine für jeden TableAdapter in der DAL. Jede dieser BLL-Klassen verfügt über Methoden zum Abrufen, einfügen, aktualisieren und Löschen von dem jeweiligen TableAdapter in der dal und zum Anwenden der entsprechenden Geschäftsregeln.

Um die DAL-und BLL-bezogenen Klassen genauer voneinander zu trennen, erstellen wir zwei Unterordner im Ordner "`App_Code`", `DAL` und `BLL`. Klicken Sie einfach mit der rechten Maustaste auf den Ordner `App_Code` im Projektmappen-Explorer, und wählen Sie neuer Ordner aus. Nachdem Sie diese beiden Ordner erstellt haben, verschieben Sie das typisierte DataSet, das Sie im ersten Tutorial erstellt haben, in den `DAL` Unterordner.

Erstellen Sie als nächstes die vier Dateien der BLL-Klasse im `BLL` Unterordner. Klicken Sie hierzu mit der rechten Maustaste auf den Unterordner `BLL`, wählen Sie neues Element hinzufügen aus, und wählen Sie die Klassen Vorlage aus. Benennen Sie die vier Klassen `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`und `EmployeesBLL`.

![Fügen Sie dem Ordner "App_Code" vier neue Klassen hinzu.](creating-a-business-logic-layer-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen von vier neuen Klassen zum Ordner "`App_Code`"

Als Nächstes fügen wir jeder Klasse Methoden hinzu, um die Methoden, die für die TableAdapters definiert sind, aus dem ersten Tutorial zu wrappen. Vorerst werden diese Methoden einfach direkt in der dal aufgerufen. Wir werden später zurückkehren, um die erforderliche Geschäftslogik hinzuzufügen.

> [!NOTE]
> Wenn Sie Visual Studio Standard Edition oder höher verwenden (d. h. Sie verwenden *nicht* Visual Web Developer), können Sie die Klassen optional mithilfe der [Klassen-Designer](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp)visuell entwerfen. Weitere Informationen zu diesem neuen Feature in Visual Studio finden Sie im [Klassen-Designer-Blog](https://blogs.msdn.com/classdesigner/default.aspx) .

Für die `ProductsBLL`-Klasse müssen wir insgesamt sieben Methoden hinzufügen:

- `GetProducts()` gibt alle Produkte zurück.
- `GetProductByProductID(productID)` gibt das Produkt mit der angegebenen Produkt-ID zurück.
- `GetProductsByCategoryID(categoryID)` gibt alle Produkte aus der angegebenen Kategorie zurück.
- `GetProductsBySupplier(supplierID)` gibt alle Produkte des angegebenen Lieferanten zurück.
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` fügt ein neues Produkt mithilfe der von Ihnen über gebenden Werte in die Datenbank ein. Gibt den `ProductID` Wert des neu eingefügten Datensatzes zurück.
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` aktualisiert ein vorhandenes Produkt in der Datenbank mit den bestandenen Werten. gibt `True` zurück, wenn genau eine Zeile aktualisiert wurde `False` andernfalls.
- `DeleteProduct(productID)` löscht das angegebene Produkt aus der Datenbank.

ProductsBLL.vb

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Die Methoden, die einfach Daten `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`und `GetProductBySuppliersID` zurückgeben, sind recht unkompliziert, da Sie einfach die DAL aufzurufen. In einigen Szenarien können Geschäftsregeln vorhanden sein, die auf dieser Ebene implementiert werden müssen (z. b. Autorisierungs Regeln basierend auf dem aktuell angemeldeten Benutzer oder der Rolle, zu der der Benutzer gehört). wir lassen diese Methoden einfach unverändert. Bei diesen Methoden fungiert die BLL nur als Proxy, über den die Präsentationsschicht auf die zugrunde liegenden Daten von der Datenzugriffs Ebene zugreift.

Die Methoden `AddProduct` und `UpdateProduct` verwenden beide als Parameter die Werte für die verschiedenen Produktfelder und fügen ein neues Produkt hinzu bzw. aktualisieren ein vorhandenes Produkt. Da viele Spalten der `Product` Tabelle `NULL` Werte akzeptieren können (`CategoryID`, `SupplierID`und `UnitPrice`, um nur einige zu nennen), werden die Eingabeparameter für `AddProduct` und `UpdateProduct`, die diesen Spalten zugeordnet sind, auf NULL festleg [Bare Typen](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx)verwendet. Typen, die NULL-Werte zulassen, sind neu in .NET 2,0 und stellen ein Verfahren bereit, mit dem angegeben wird, ob ein Werttyp stattdessen `Nothing`werden soll. Weitere Informationen finden Sie im Blogeintrag von [Paul Vick](http://www.panopticoncentral.net/) [der Wahrheit zu Typen, die NULL-Werte](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) zulassen, und in der technischen Dokumentation für die Struktur, die [NULL-Werte](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) zulässt.

Alle drei Methoden geben einen booleschen Wert zurück, der angibt, ob eine Zeile eingefügt, aktualisiert oder gelöscht wurde, da der Vorgang möglicherweise nicht zu einer betroffenen Zeile führt. Wenn z. b. der Seiten Entwickler `DeleteProduct` die Übergabe eines `ProductID` für ein nicht vorhandenes Produkt aufruft, hat die für die Datenbank ausgegebene `DELETE`-Anweisung keine Auswirkung, weshalb die `DeleteProduct` Methode `False`zurückgibt.

Beachten Sie, dass beim Hinzufügen eines neuen Produkts oder beim Aktualisieren eines vorhandenen Produkts die Feldwerte der neuen oder geänderten Produkte als Liste von skalaren angenommen werden, anstatt eine `ProductsRow` Instanz zu akzeptieren. Diese Vorgehensweise wurde gewählt, da die `ProductsRow`-Klasse von der `DataRow` ADO.NET-Klasse abgeleitet wird, die nicht über einen standardmäßigen Parameter losen Konstruktor verfügt. Um eine neue `ProductsRow` Instanz zu erstellen, müssen wir zuerst eine `ProductsDataTable` Instanz erstellen und dann die zugehörige `NewProductRow()`-Methode aufrufen (was wir in `AddProduct`tun). Dieses Manko gibt den Anfang zurück, wenn wir das Einfügen und Aktualisieren von Produkten mithilfe von ObjectDataSource aktivieren. Kurz gesagt, versucht ObjectDataSource, eine Instanz der Eingabeparameter zu erstellen. Wenn die BLL-Methode eine `ProductsRow` Instanz erwartet, versucht ObjectDataSource, eine Instanz zu erstellen, schlägt jedoch fehl, weil ein standardmäßiger Parameter loser Konstruktor fehlt. Weitere Informationen zu diesem Problem finden Sie in den folgenden zwei ASP.NET Forums Beiträgen: [Aktualisieren von objectdatasources mit stark typisierten Datasets](https://forums.asp.net/1098630/ShowPost.aspx)und [Problem mit ObjectDataSource und stark typisiertem DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Im nächsten Schritt erstellt der Code sowohl in `AddProduct` als auch `UpdateProduct`eine `ProductsRow`-Instanz und füllt diese mit den soeben weiter gegebenen Werten. Beim Zuweisen von Werten zu datacolenns einer DataRow können verschiedene Validierungs Überprüfungen auf Feldebene durchgeführt werden. Daher wird die Gültigkeit der an die BLL-Methode übergebenen Daten durch manuelles Zurücksetzen der übergebenen Werte in eine DataRow sichergestellt. Leider verwenden die stark typisierten DataRow-Klassen, die von Visual Studio generiert werden, keine Typen, die NULL-Werte zulassen. Stattdessen müssen Sie die `SetColumnNameNull()`-Methode verwenden, um anzugeben, dass eine bestimmte datacolenumn in einer DataRow einem `NULL`-Daten Bank Wert entsprechen soll.

In `UpdateProduct` wir zuerst das Produkt laden, das mithilfe `GetProductByProductID(productID)`aktualisiert werden soll. Dies mag ein unnötiger Weg zur Datenbank sein, aber diese zusätzliche Fahrt wird in zukünftigen Tutorials, in denen optimistische Parallelität untersucht wird, als lohnenswert erweisen. Optimistische Parallelität ist eine Technik, mit der sichergestellt wird, dass zwei Benutzer, die gleichzeitig an denselben Daten arbeiten, nicht versehentlich die Änderungen überschreiben. Das Abrufen des gesamten Datensatzes erleichtert auch das Erstellen von Update Methoden in der BLL, die nur eine Teilmenge der Spalten von DataRow ändern. Wenn wir uns die `SuppliersBLL`-Klasse ansehen, sehen wir ein Beispiel.

Beachten Sie schließlich, dass die `ProductsBLL`-Klasse das [DataObject-Attribut](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) enthält (die `[System.ComponentModel.DataObject]`-Syntax direkt vor der Class-Anweisung am Anfang der Datei) und die-Methoden über [DataObjectMethodAttribute-Attribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx)verfügen. Das `DataObject` Attribut markiert die Klasse als ein Objekt, das für die Bindung an ein [ObjectDataSource-Steuer](https://msdn.microsoft.com/library/9a4kyhcx.aspx)Element geeignet ist, während der `DataObjectMethodAttribute` den Zweck der Methode angibt. Wie wir in zukünftigen Tutorials sehen werden, erleichtert die ObjectDataSource von ASP.NET 2.0 den deklarativen Zugriff auf Daten aus einer Klasse. Um die Liste der möglichen Klassen zu filtern, die im Assistenten von ObjectDataSource an BIND gebunden werden sollen, werden standardmäßig nur die als `DataObjects` markierten Klassen in der Dropdown Liste des Assistenten angezeigt. Die `ProductsBLL`-Klasse funktioniert auch ohne diese Attribute. durch das Hinzufügen werden Sie jedoch einfacher im Assistenten von ObjectDataSource verwendet.

## <a name="adding-the-other-classes"></a>Hinzufügen der anderen Klassen

Wenn die `ProductsBLL`-Klasse fertiggestellt ist, müssen wir noch die Klassen zum Arbeiten mit Kategorien, Lieferanten und Mitarbeitern hinzufügen. Nehmen Sie sich einen Moment Zeit, um die folgenden Klassen und Methoden mit den Konzepten aus dem obigen Beispiel zu erstellen:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Die einzige Methode, die beachtet werden muss, ist die `UpdateSupplierAddress`-Methode der `SuppliersBLL`-Klasse. Diese Methode stellt eine Schnittstelle zum Aktualisieren der Adressinformationen des Lieferanten bereit. Intern liest diese Methode das `SupplierDataRow`-Objekt für den angegebenen `supplierID` (mithilfe `GetSupplierBySupplierID`), legt die Adress bezogenen Eigenschaften fest und ruft dann die `Update`-Methode des `SupplierDataTable`auf. Die `UpdateSupplierAddress`-Methode folgt:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Die gesamte Implementierung der BLL-Klassen finden Sie in diesem Artikel.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Schritt 2: Zugreifen auf die typisierten Datasets über die BLL-Klassen

Im ersten Tutorial haben wir Beispiele für die programmgesteuerte Arbeit mit dem typisierten DataSet gesehen, aber mit dem Hinzufügen unserer BLL-Klassen sollte die Präsentationsebene stattdessen mit der BLL funktionieren. Im `AllProducts.aspx` Beispiel aus dem ersten Tutorial wurde das `ProductsTableAdapter` verwendet, um die Liste der Produkte an eine GridView zu binden, wie im folgenden Code gezeigt:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Um die neuen BLL-Klassen verwenden zu können, muss nur die erste Codezeile geändert werden. ersetzen Sie dabei einfach das `ProductsTableAdapter` Objekt durch ein `ProductBLL` Objekt:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

Der Zugriff auf die BLL-Klassen kann auch deklarativ (wie das typisierte DataSet) mithilfe von ObjectDataSource erfolgen. In den folgenden Tutorials wird die ObjectDataSource ausführlicher erläutert.

[![die Liste der Produkte in einer GridView angezeigt wird](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Abbildung 3**: die Liste der Produkte wird in einer GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-business-logic-layer-vb/_static/image5.png))

## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Schritt 3: Hinzufügen der Validierung auf Feldebene zu den DataRow-Klassen

Bei der Validierung auf Feldebene handelt es sich um Überprüfungen, die sich auf die Eigenschaftswerte der Geschäftsobjekte beim Einfügen oder aktualisieren beziehen. Einige Validierungsregeln auf Feldebene für Produkte sind:

- Das `ProductName` Feld darf nicht länger als 40 Zeichen sein.
- Das `QuantityPerUnit` Feld darf höchstens 20 Zeichen lang sein.
- Die Felder `ProductID`, `ProductName`und `Discontinued` sind erforderlich, aber alle anderen Felder sind optional.
- Die Felder `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`und `ReorderLevel` müssen größer oder gleich 0 (null) sein.

Diese Regeln können und auf Datenbankebene ausgedrückt werden. Das Zeichenlimit für die Felder `ProductName` und `QuantityPerUnit` wird von den Datentypen dieser Spalten in der `Products` Tabelle (`nvarchar(40)` bzw. `nvarchar(20)`) aufgezeichnet. Ob Felder erforderlich und optional sind, wird durch ausgedrückt, wenn die Datenbanktabellen Spalte `NULL` s zulässt. Es gibt vier [Check-Einschränkungen](https://msdn.microsoft.com/library/ms188258.aspx) , die sicherstellen, dass nur Werte größer oder gleich NULL in die Spalten `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`oder `ReorderLevel` umgewandelt werden können.

Zusätzlich zum Erzwingen dieser Regeln in der Datenbank sollten Sie auch auf Datasetebene erzwungen werden. Die Feldlänge und ob ein Wert erforderlich oder optional ist, werden für die datasetsätze der einzelnen Datentypen bereits aufgezeichnet. Um die vorhandene Überprüfung auf Feldebene automatisch bereitzustellen, wechseln Sie zum DataSet-Designer, wählen Sie ein Feld aus einem der DataTables aus, und wechseln Sie dann zum Eigenschaftenfenster. Wie in Abbildung 4 gezeigt, hat der `QuantityPerUnit` datacolenumn im `ProductsDataTable` eine maximale Länge von 20 Zeichen und lässt `NULL` Werte zu. Wenn Sie versuchen, die `QuantityPerUnit`-Eigenschaft des `ProductsDataRow`auf einen Zeichen folgen Wert mit einer Länge von mehr als 20 Zeichen festzulegen, wird eine `ArgumentException` ausgelöst.

[![die datacolbin eine grundlegende Validierung auf Feldebene bereitstellt.](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Abbildung 4**: datacolenn stellt grundlegende Validierung auf Feldebene bereit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-business-logic-layer-vb/_static/image8.png))

Leider können keine Begrenzungen überprüft werden, z. b. der `UnitPrice` Wert muss größer oder gleich 0 (null) sein, über den Eigenschaftenfenster. Um diese Art von Validierung auf Feldebene bereitzustellen, müssen wir einen Ereignishandler für das [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) -Ereignis der Datentabelle erstellen. Wie im [vorherigen Tutorial](creating-a-data-access-layer-vb.md)erwähnt, können die vom typisierten DataSet erstellten Datasets, DataTables und DataRow-Objekte durch die Verwendung von partiellen Klassen erweitert werden. Mit dieser Technik können wir einen `ColumnChanging` Ereignishandler für die `ProductsDataTable`-Klasse erstellen. Erstellen Sie zunächst eine Klasse im Ordner "`App_Code`" mit dem Namen `ProductsDataTable.ColumnChanging.vb`.

[![dem Ordner "App_Code" eine neue Klasse hinzufügen](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Abbildung 5**: Hinzufügen einer neuen Klasse zum `App_Code` Ordner ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-a-business-logic-layer-vb/_static/image11.png))

Erstellen Sie als nächstes einen Ereignishandler für das `ColumnChanging` Ereignis, mit dem sichergestellt wird, dass die Spaltenwerte `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`und `ReorderLevel` (falls nicht `NULL`) größer oder gleich 0 (null) sind. Wenn eine solche Spalte außerhalb des gültigen Bereichs liegt, lösen Sie eine `ArgumentException`aus.

ProductsDataTable.ColumnChanging.vb

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Schritt 4: Hinzufügen von benutzerdefinierten Geschäftsregeln zu den Klassen von BLL

Zusätzlich zur Validierung auf Feldebene können auch benutzerdefinierte Geschäftsregeln auf hoher Ebene vorhanden sein, die verschiedene Entitäten oder Konzepte enthalten, die auf der Ebene einzelner Spalten nicht ausdrucksfähig sind, wie z. b.:

- Wenn ein Produkt nicht mehr unterstützt wird, kann die `UnitPrice` nicht aktualisiert werden.
- Das Wohnsitzland eines Mitarbeiters muss mit dem Wohnsitzland des Vorgesetzten identisch sein.
- Ein Produkt kann nicht eingestellt werden, wenn es sich um das einzige vom Lieferanten bereitgestellte Produkt handelt.

Die BLL-Klassen sollten Überprüfungen enthalten, um sicherzustellen, dass die Geschäftsregeln der Anwendung eingehalten werden. Diese Überprüfungen können direkt den Methoden hinzugefügt werden, auf die Sie angewendet werden.

Stellen Sie sich vor, dass unsere Geschäftsregeln vorschreiben, dass ein Produkt nicht mehr unterstützt werden konnte, wenn es das einzige Produkt eines bestimmten Lieferanten war. Das heißt, wenn Product *X* das einzige Produkt war, das wir von Lieferanten *Y*gekauft haben, konnte *X* nicht als eingestellt gekennzeichnet werden. Wenn uns der Lieferant *Y* jedoch drei Produkte ( *A*, *B*und *C*) zur Verfügung gestellt hat, können wir beliebige und alle diese als nicht mehr unterstützt markieren. Eine ungerade Geschäftsregel, aber Geschäftsregeln und allgemeine Sense sind nicht immer ausgerichtet!

Wenn Sie diese Geschäftsregel in der `UpdateProducts`-Methode erzwingen möchten, überprüfen Sie zunächst, ob `Discontinued` auf `True` festgelegt wurde. wenn dies der Fall ist, wenden wir `GetProductsBySupplierID` an, um zu bestimmen, wie viele Produkte der Lieferant dieses Produkts gekauft hat. Wenn nur ein Produkt von diesem Lieferanten erworben wird, wird ein `ApplicationException`ausgelöst.

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Reagieren auf Validierungs Fehler in der Präsentationsebene

Beim Aufrufen der BLL von der Präsentationsebene können wir entscheiden, ob Sie versuchen, Ausnahmen zu behandeln, die möglicherweise ausgelöst werden, oder Sie in ASP.net hinein Blasen (wodurch das `Error` Ereignis des `HttpApplication`ausgelöst wird). Um eine Ausnahme zu behandeln, wenn Sie mit der BLL Programm gesteuert arbeiten, können wir einen [try... Catch](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) -Block, wie im folgenden Beispiel gezeigt:

[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Wie wir in zukünftigen Tutorials sehen werden, kann das Behandeln von Ausnahmen, die beim Verwenden eines datenweb-Steuer Elements zum Einfügen, aktualisieren oder Löschen von Daten aus der BLL erfolgen, direkt in einem Ereignishandler behandelt werden, anstatt Code in `Try...Catch` Blöcken umschließen zu müssen.

## <a name="summary"></a>Zusammenfassung

Eine gut strukturierte Anwendung wird in unterschiedliche Ebenen integriert, von denen jede eine bestimmte Rolle kapselt. Im ersten Tutorial dieser Artikel Reihe haben wir eine Datenzugriffs Schicht mithilfe typisierter Datasets erstellt. in diesem Tutorial haben wir eine Geschäftslogik Schicht als eine Reihe von Klassen in den `App_Code` Ordner der Anwendung erstellt, die die DAL aufzurufen. Die BLL implementiert die Logik auf Feldebene und auf Geschäfts Ebene für die Anwendung. Zusätzlich zum Erstellen einer separaten BLL, wie in diesem Tutorial, besteht auch die Möglichkeit, die TableAdapters-Methoden durch die Verwendung von partiellen Klassen zu erweitern. Mit dieser Technik können wir jedoch weder vorhandene Methoden außer Kraft setzen noch die DAL und unsere BLL so sauber wie den Ansatz, den wir in diesem Artikel durchgeführt haben.

Wenn dal und BLL abgeschlossen sind, sind wir bereit, auf der Präsentationsschicht zu beginnen. Im [nächsten Tutorial](master-pages-and-site-navigation-vb.md) führen wir eine kurze Umleitung von Datenzugriffs Themen durch und definieren ein konsistentes Seitenlayout für die Verwendung in den Lernprogrammen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Liz shulok, Dennis Patterson, Carlos Santos und Hilton Giesenow. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-a-data-access-layer-vb.md)
> [Weiter](master-pages-and-site-navigation-vb.md)
