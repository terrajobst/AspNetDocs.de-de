---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementieren von optimistischer Parallelität (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Für eine Webanwendung, die es mehreren Benutzern ermöglicht, Daten zu bearbeiten, besteht das Risiko, dass zwei Benutzer die gleichen Daten gleichzeitig bearbeiten können. In diesem tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 28c39fe2a290cc3a5b093fdd09de341630606137
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628741"
---
# <a name="implementing-optimistic-concurrency-vb"></a>Implementieren von optimistischer Parallelität (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) oder [PDF herunterladen](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Für eine Webanwendung, die es mehreren Benutzern ermöglicht, Daten zu bearbeiten, besteht das Risiko, dass zwei Benutzer die gleichen Daten gleichzeitig bearbeiten können. In diesem Tutorial implementieren wir die Steuerung für optimistische Parallelität, um dieses Risiko zu bewältigen.

## <a name="introduction"></a>Einführung

Bei Webanwendungen, die nur Benutzern das Anzeigen von Daten erlauben, oder für diejenigen, die nur einen einzelnen Benutzer enthalten, der Daten ändern kann, besteht keine Gefahr, dass zwei gleichzeitige Benutzer versehentlich die Änderungen einer anderen Person überschreiben. Bei Webanwendungen, die mehreren Benutzern das Aktualisieren oder Löschen von Daten ermöglichen, besteht jedoch die Möglichkeit, dass die Änderungen eines Benutzers mit anderen gleichzeitigen Benutzern in Konflikt stehen. Wenn zwei Benutzer einen einzelnen Datensatz gleichzeitig bearbeiten, überschreibt der Benutzer, der die Änderungen zuletzt ausführt, keine Parallelitäts Richtlinie, sondern überschreibt die von der ersten vorgenommenen Änderungen.

Nehmen wir beispielsweise an, dass zwei Benutzer, jisun und Sam, eine Seite in unserer Anwendung besuchen, die es Besuchern gestattet hat, die Produkte mithilfe eines GridView-Steuer Elements zu aktualisieren und zu löschen. Beide klicken auf die Bearbeitungs Schaltfläche in der GridView um die gleiche Zeit. Jisun ändert den Produktnamen in "Chai Tea" und klickt auf die Schaltfläche "Aktualisieren". Das Ergebnis ist eine `UPDATE`-Anweisung, die an die Datenbank gesendet wird, die *alle* aktualisierbaren Felder des Produkts festlegt (obwohl jisun nur ein Feld `ProductName`) aktualisiert hat. Zu diesem Zeitpunkt hat die Datenbank die Werte "Chai Tea", die Kategorie "Getränke", die Lieferanten Exotik usw. für dieses spezielle Produkt. Die GridView auf dem Sam-Bildschirm zeigt jedoch weiterhin den Produktnamen in der bearbeitbaren GridView-Zeile als "Chai" an. Einige Sekunden nach dem Commit von jisun-Änderungen aktualisiert Sam die Kategorie in Bedingungen und klickt auf aktualisieren. Dies führt dazu, dass eine `UPDATE`-Anweisung an die Datenbank gesendet wird, mit der der Produktname auf "Chai" festgelegt wird, die `CategoryID` auf die zugehörige Getränkekategorie-ID usw. Die Änderungen des Produkt namens von jisun wurden überschrieben. In Abbildung 1 wird diese Ereignis Reihe grafisch dargestellt.

[![wenn zwei Benutzer gleichzeitig einen Datensatz aktualisieren, ist es möglich, dass die Änderungen der anderen Benutzer von einem Benutzer überschrieben werden.](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Abbildung 1**: Wenn zwei Benutzer gleichzeitig einen Datensatz aktualisieren, ist es möglich, dass ein Benutzer die Änderungen des anderen Benutzers überschreibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image3.png)).

Wenn zwei Benutzer eine Seite besuchen, kann es auch vorkommen, dass ein Benutzer einen Datensatz aktualisiert, wenn er von einem anderen Benutzer gelöscht wird. Oder zwischen dem Zeitpunkt, zu dem ein Benutzer eine Seite lädt, und beim Klicken auf die Schaltfläche "Löschen" hat ein anderer Benutzer möglicherweise den Inhalt dieses Datensatzes geändert.

Es stehen drei Strategien für die Parallelitäts [Steuerung](http://en.wikipedia.org/wiki/Concurrency_control) zur Verfügung:

- **Nichts Unternehmen** : Wenn gleichzeitige Benutzer denselben Datensatz ändern, lassen Sie den letzten Commit (Standardverhalten) zu.
- Vollständige Parallelität [ **: nehmen**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) Sie an, dass es in diesem Fall möglicherweise zu Parallelitäts Konflikten kommt und der Großteil der Zeit Konflikte dieser Konflikte nicht besteht. Wenn also ein Konflikt auftritt, informieren Sie den Benutzer einfach darüber, dass die Änderungen nicht gespeichert werden können, weil ein anderer Benutzer die gleichen Daten geändert hat.
- **Pessimistische** Parallelität: nehmen Sie an, dass Parallelitäts Konflikte üblich sind und dass Benutzer nicht tolerieren, dass Ihre Änderungen aufgrund der gleichzeitigen Aktivität eines anderen Benutzers nicht gespeichert wurden. Wenn ein Benutzer damit beginnt, einen Datensatz zu aktualisieren, wird er daher gesperrt. Dadurch wird verhindert, dass andere Benutzer diesen Datensatz bearbeiten oder löschen, bis der Benutzer die Änderungen durchführt.

Alle unsere Tutorials haben bisher die Standardstrategie für die Parallelitäts Auflösung verwendet, und wir haben den letzten Schreib Gewinn ermöglicht. In diesem Tutorial wird erläutert, wie die Steuerung der vollständigen Parallelität implementiert wird.

> [!NOTE]
> In dieser tutorialreihe werden die Beispiele für die pessimistische Parallelität nicht betrachtet. Die pessimistische Parallelität wird nur selten verwendet, da solche Sperren, wenn Sie nicht ordnungsgemäß aufgegeben wurden, andere Benutzer daran hindern können, Daten zu aktualisieren. Wenn ein Benutzer beispielsweise einen Datensatz für die Bearbeitung sperrt und dann den Tag vor dem entsperren verlässt, kann kein anderer Benutzer diesen Datensatz aktualisieren, bis der ursprüngliche Benutzer zurückkehrt und seine Aktualisierung abgeschlossen hat. In Fällen, in denen die pessimistische Parallelität verwendet wird, gibt es in der Regel einen Timeout, bei dem die Sperre abgebrochen wird. Tickets Sales Websites, die eine bestimmte Sitzungsort für kurze Zeit sperren, während der Benutzer den Bestellprozess abschließt, ist ein Beispiel für die Steuerung der pessimistischen Parallelität.

## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Schritt 1: Überprüfen der Implementierung der optimistischen Parallelität

Durch die Steuerung der vollständigen Parallelität wird sichergestellt, dass der zu Aktualisier Ende oder zu löschende Datensatz dieselben Werte hat wie beim Start des Aktualisierungs-oder Löschvorgangs. Wenn Sie z. b. in einem bearbeitbaren GridView-Steuerelement auf die Schaltfläche Bearbeiten klicken, werden die Werte des Datensatzes aus der Datenbank gelesen und in Textfeldern und anderen websteuer Elementen angezeigt. Diese ursprünglichen Werte werden von der GridView gespeichert. Nachdem der Benutzer die Änderungen vorgenommen und auf die Schaltfläche Aktualisieren geklickt hat, werden die ursprünglichen Werte und die neuen Werte an die Geschäftslogik Schicht und dann an die Datenzugriffs Ebene weitergeleitet. Die Datenzugriffs Ebene muss eine SQL-Anweisung ausgeben, die nur den Datensatz aktualisiert, wenn die ursprünglichen Werte, die der Benutzer mit der Bearbeitung begonnen hat, mit den Werten in der Datenbank identisch sind. Abbildung 2 zeigt diese Abfolge von Ereignissen.

[![, dass das Update oder der Löschvorgang erfolgreich ist, müssen die ursprünglichen Werte den aktuellen Daten bankwerten entsprechen.](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Abbildung 2**: damit das Update oder der Löschvorgang erfolgreich ausgeführt werden kann, müssen die ursprünglichen Werte den aktuellen Daten bankwerten entsprechen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image6.png)).

Es gibt verschiedene Ansätze für die Implementierung von optimistischer Parallelität (siehe die [Aktualisierungs Logik für optimistische](http://www.eggheadcafe.com/articles/20050719.asp) Parallelität von [Peter A. Bromberg](http://peterbromberg.net/)für einen kurzen Blick auf eine Reihe von Optionen). Das ADO.net typisierte DataSet bietet eine Implementierung, die nur mit dem Tick eines Kontrollkästchens konfiguriert werden kann. Durch das Aktivieren der optimistischen Parallelität für einen TableAdapter im typisierten Dataset werden die `UPDATE`-und `DELETE` Anweisungen des TableAdapters erweitert, um einen Vergleich aller ursprünglichen Werte in der `WHERE`-Klausel einzuschließen. Mit der folgenden `UPDATE` Anweisung werden z. b. der Name und der Preis eines Produkts nur aktualisiert, wenn die aktuellen Daten Bank Werte den Werten entsprechen, die ursprünglich beim Aktualisieren des Datensatzes in der GridView abgerufen wurden. Die Parameter `@ProductName` und `@UnitPrice` enthalten die neuen Werte, die vom Benutzer eingegeben wurden, während `@original_ProductName` und `@original_UnitPrice` die Werte enthalten, die beim Klicken auf die Schaltfläche Bearbeiten ursprünglich in die GridView geladen wurden:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Diese `UPDATE` Anweisung wurde zur besseren Lesbarkeit vereinfacht. In der Praxis wäre die `UnitPrice` Überprüfung in der `WHERE`-Klausel eher beteiligt, da `UnitPrice` `NULL` s enthalten und überprüfen kann, ob `NULL = NULL` immer false zurückgibt (Stattdessen müssen Sie `IS NULL`verwenden).

Zusätzlich zur Verwendung einer anderen zugrunde liegenden `UPDATE`-Anweisung wird durch das Konfigurieren eines TableAdapter für die Verwendung der optimistischen Parallelität auch die Signatur seiner direkten DB-Methoden geändert. Erinnern Sie sich aus unserem ersten Lernprogramm, das [*Erstellen einer Datenzugriffs Ebene*](../introduction/creating-a-data-access-layer-cs.md), dass es sich bei DB Direct-Methoden um solche handelt, die eine Liste von skalaren Werten als Eingabeparameter akzeptieren (anstatt eine stark typisierte DataRow oder eine Datentabelle). Bei der Verwendung optimistischer Parallelität enthalten die Methoden für die direkte DB-`Update()` und die `Delete()` auch Eingabeparameter für die ursprünglichen Werte. Außerdem muss der Code in der BLL für die Verwendung des Batch Aktualisierungs Musters (die `Update()` Methoden Überladungen, die DataRows und DataTables anstelle von skalaren Werten akzeptieren) ebenfalls geändert werden.

Anstatt die TableAdapters unserer vorhandenen dal so zu erweitern, dass Sie die vollständige Parallelität verwenden (was das Ändern der BLL erfordert), erstellen wir stattdessen ein neues typisiertes DataSet mit dem Namen `NorthwindOptimisticConcurrency`, dem wir einen `Products` TableAdapter hinzufügen, der optimistische Parallelität verwendet. Danach erstellen wir eine `ProductsOptimisticConcurrencyBLL` Business Logic Layer-Klasse, die über die entsprechenden Änderungen zur Unterstützung der optimistischen Parallelitäts dal verfügt. Nachdem diese Grundlagen festgelegt wurde, können wir die ASP.NET-Seite erstellen.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Schritt 2: Erstellen einer Datenzugriffs Ebene, die optimistische Parallelität unterstützt

Wenn Sie ein neues typisiertes Dataset erstellen möchten, klicken Sie mit der rechten Maustaste auf den Ordner `DAL` im Ordner `App_Code`, und fügen Sie ein neues Dataset mit dem Namen `NorthwindOptimisticConcurrency`hinzu. Wie im ersten Tutorial zu sehen ist, wird dem typisierten Dataset ein neuer TableAdapter hinzugefügt, und der TableAdapter-Konfigurations-Assistent wird automatisch gestartet. Auf dem ersten Bildschirm werden Sie aufgefordert, die Datenbank anzugeben, mit der eine Verbindung hergestellt werden soll: Stellen Sie eine Verbindung mit derselben Northwind-Datenbank her, indem Sie die `NORTHWNDConnectionString` Einstellung aus `Web.config`

[![Herstellen einer Verbindung mit derselben Northwind-Datenbank](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Abbildung 3**: Herstellen einer Verbindung mit derselben Northwind-Datenbank ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image9.png))

Als nächstes werden Sie aufgefordert, die Daten abzufragen: über eine Ad-hoc-SQL-Anweisung, eine neue gespeicherte Prozedur oder eine vorhandene gespeicherte Prozedur. Da wir Ad-hoc-SQL-Abfragen in unserer ursprünglichen dal verwendet haben, verwenden Sie diese Option auch hier.

[![angeben der Daten, die mit einer Ad-hoc-SQL-Anweisung abgerufen werden sollen](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Abbildung 4**: Angeben der abzurufenden Daten mit einer Ad-hoc-SQL-Anweisung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image12.png))

Geben Sie auf dem folgenden Bildschirm die SQL-Abfrage ein, die zum Abrufen der Produktinformationen verwendet werden soll. Wir verwenden genau dieselbe SQL-Abfrage, die für die `Products` TableAdapter aus unserer ursprünglichen dal verwendet wurde, die alle `Product` Spalten zusammen mit den Lieferanten-und Kategorienamen des Produkts zurückgibt:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]

[![dieselbe SQL-Abfrage aus dem TableAdapter für Produkte in der ursprünglichen dal verwenden](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Abbildung 5**: Verwenden der gleichen SQL-Abfrage aus der `Products` TableAdapter in der ursprünglichen Dal ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image15.png))

Bevor Sie zum nächsten Bildschirm wechseln, klicken Sie auf die Schaltfläche Erweiterte Optionen. Damit dieser TableAdapter die Steuerung der vollständigen Parallelität anwendet, aktivieren Sie einfach das Kontrollkästchen "Verwendung optimistischer Parallelität".

[![Aktivieren der Steuerung der vollständigen Parallelität durch Aktivieren des Kontrollkästchens &quot;vollständige Parallelität verwenden&quot;](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Abbildung 6**: Aktivieren der Steuerung der optimistischen Parallelität durch Aktivieren des Kontrollkästchens "vollständige Parallelität verwenden" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image18.png)

Geben Sie schließlich an, dass der TableAdapter die Datenzugriffs Muster verwenden soll, die eine Datentabelle ausfüllen und eine Datentabelle zurückgeben. Geben Sie außerdem an, dass die direkten DB-Methoden erstellt werden sollen. Ändern Sie den Methodennamen für das Muster "Datentabelle zurückgeben" von "GetData" in "GetProducts", um die Benennungs Konventionen zu spiegeln, die wir in unserer ursprünglichen dal verwendet haben.

[![den TableAdapter alle Datenzugriffs Muster verwenden](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Abbildung 7**: lassen Sie den TableAdapter alle Datenzugriffs Muster nutzen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image21.png))

Nachdem Sie den Assistenten abgeschlossen haben, enthält der DataSet-Designer eine stark typisierte `Products` Datentabelle und TableAdapter. Nehmen Sie sich einen Moment Zeit, um die Datentabelle von `Products` in `ProductsOptimisticConcurrency`umzubenennen. Sie können dies tun, indem Sie mit der rechten Maustaste auf die Titelleiste der Datentabelle klicken und im Kontextmenü die Option Umbenennen auswählen.

[![eine Datentabelle und ein TableAdapter dem typisierten DataSet hinzugefügt wurden.](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Abbildung 8**: dem typisierten DataSet wurden eine Datentabelle und ein TableAdapter hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image24.png))

Um die Unterschiede zwischen den `UPDATE` und `DELETE` Abfragen zwischen dem `ProductsOptimisticConcurrency` TableAdapter (der optimistische Parallelität verwendet) und dem TableAdapter für Produkte (der nicht entspricht) anzuzeigen, klicken Sie auf den TableAdapter und wechseln zum Eigenschaftenfenster. In den `DeleteCommand`-und `UpdateCommand` Eigenschaften `CommandText` unter Eigenschaften können Sie die tatsächliche SQL-Syntax anzeigen, die an die Datenbank gesendet wird, wenn die Aktualisierungs-oder Lösch bezogenen Methoden der dal aufgerufen werden. Für den `ProductsOptimisticConcurrency` TableAdapter wird folgende `DELETE` Anweisung verwendet:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Die `DELETE`-Anweisung für den Product TableAdapter in unserer ursprünglichen Dal ist zwar sehr viel einfacher:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Wie Sie sehen können, enthält die `WHERE`-Klausel in der `DELETE`-Anweisung für den TableAdapter, der optimistische Parallelität verwendet, einen Vergleich zwischen den vorhandenen Spaltenwerten der `Product` Tabelle und den ursprünglichen Werten zum Zeitpunkt der letzten Auffüllung der GridView (oder DetailsView oder FormView). Da alle anderen Felder als `ProductID`, `ProductName`und `Discontinued` `NULL` Werte aufweisen können, sind zusätzliche Parameter und Überprüfungen enthalten, um `NULL` Werte in der `WHERE`-Klausel ordnungsgemäß zu vergleichen.

Wir fügen dem optimistischen Parallelitäts fähigen Dataset für dieses Tutorial keine zusätzlichen Datenelemente hinzu, da unsere ASP.NET-Seite nur Aktualisierungs-und Lösch Produktinformationen bereitstellt. Wir müssen jedoch noch die `GetProductByProductID(productID)` Methode zum `ProductsOptimisticConcurrency` TableAdapter hinzufügen.

Um dies zu erreichen, klicken Sie mit der rechten Maustaste auf die Titelleiste des TableAdapter (der Bereich rechts oberhalb der `Fill`-und `GetProducts` Methodennamen), und wählen Sie im Kontextmenü Abfrage hinzufügen aus. Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen gestartet. Wie bei der Erstkonfiguration des TableAdapters sollten Sie die `GetProductByProductID(productID)` Methode mithilfe einer Ad-hoc-SQL-Anweisung erstellen (siehe Abbildung 4). Da die `GetProductByProductID(productID)`-Methode Informationen zu einem bestimmten Produkt zurückgibt, geben Sie an, dass diese Abfrage ein `SELECT` Abfragetyp ist, der Zeilen zurückgibt.

[![markieren Sie den Abfragetyp als &quot;SELECT, der Zeilen zurückgibt&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Abbildung 9**: Markieren des Abfrage Typs als "`SELECT`, die Zeilen zurückgibt" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image27.png))

Auf dem nächsten Bildschirm werden wir aufgefordert, die SQL-Abfrage zu verwenden, wobei die Standard Abfrage des TableAdapter vorab geladen wurde. Erweitern Sie die vorhandene Abfrage so, dass Sie die-Klausel `WHERE ProductID = @ProductID`enthält, wie in Abbildung 10 dargestellt.

[![der vorab geladenen Abfrage eine WHERE-Klausel hinzufügen, um einen bestimmten Produktdaten Satz zurückzugeben.](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Abbildung 10**: Hinzufügen einer `WHERE`-Klausel zur vorab geladenen Abfrage, um einen bestimmten Produktdaten Satz zurückzugeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image30.png))

Ändern Sie abschließend die generierten Methodennamen in `FillByProductID` und `GetProductByProductID`.

[![die Methoden in fillbyproductid und getproductbyproductid umbenennen.](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Abbildung 11**: Umbenennen der Methoden in `FillByProductID` und `GetProductByProductID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image33.png))

Nachdem dieser Assistent vollständig ist, enthält der TableAdapter nun zwei Methoden zum Abrufen von Daten: `GetProducts()`, die *alle* Produkte zurückgibt. und `GetProductByProductID(productID)`, die das angegebene Produkt zurückgibt.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Schritt 3: Erstellen einer Geschäftslogik Schicht für die DAL mit optimistischer Parallelität

Unsere vorhandene `ProductsBLL`-Klasse enthält Beispiele für die Verwendung von Batch Update-und DB Direct-Mustern. Die `AddProduct`-Methode und die `UpdateProduct` Überladungen verwenden das Batch Aktualisierungs Muster und übergeben eine `ProductRow`-Instanz an die Update-Methode des TableAdapter. Die `DeleteProduct`-Methode hingegen verwendet das direkte DB-Muster, das die `Delete(productID)`-Methode des TableAdapter aufruft.

Mit dem neuen `ProductsOptimisticConcurrency` TableAdapter erfordern die direkten DB-Methoden nun, dass die ursprünglichen Werte ebenfalls weitergegeben werden. Die `Delete`-Methode erwartet nun z. b. zehn Eingabeparameter: die ursprünglichen `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`und `Discontinued`. Diese zusätzlichen Eingabeparameter Werte werden in `WHERE`-Klausel der `DELETE` Anweisung verwendet, die an die Datenbank gesendet wird. der angegebene Datensatz wird nur gelöscht, wenn die aktuellen Werte der Datenbank den ursprünglichen Werten zugeordnet sind.

Obwohl die Methoden Signatur für die `Update` Methode des TableAdapter, die im Batch Aktualisierungs Muster verwendet wird, nicht geändert wurde, hat der Code, der zum Aufzeichnen der ursprünglichen und neuen Werte erforderlich ist, den Wert. Anstatt zu versuchen, die optimistische Parallelitäts aktivierte dal mit unserer vorhandenen `ProductsBLL`-Klasse zu verwenden, erstellen wir daher eine neue Geschäftslogik Schicht-Klasse für die Arbeit mit unserer neuen dal.

Fügen Sie dem Ordner `BLL` im Ordner `App_Code` eine Klasse mit dem Namen `ProductsOptimisticConcurrencyBLL` hinzu.

![Fügen Sie die Klasse "productoptimisticconfiguration Configuration Manager" zum Ordner "BLL" hinzu.](implementing-optimistic-concurrency-vb/_static/image34.png)

**Abbildung 12**: Hinzufügen der `ProductsOptimisticConcurrencyBLL` Klasse zum Ordner "BLL"

Fügen Sie als nächstes der `ProductsOptimisticConcurrencyBLL`-Klasse den folgenden Code hinzu:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Beachten Sie die using `NorthwindOptimisticConcurrencyTableAdapters`-Anweisung oberhalb des Starts der Klassen Deklaration. Der `NorthwindOptimisticConcurrencyTableAdapters`-Namespace enthält die `ProductsOptimisticConcurrencyTableAdapter`-Klasse, die die Methoden der dal bereitstellt. Vor der Klassen Deklaration finden Sie auch das `System.ComponentModel.DataObject`-Attribut, das Visual Studio anweist, diese Klasse in die Dropdown Liste des ObjectDataSource-Assistenten einzubeziehen.

Die `Adapter`-Eigenschaft des `ProductsOptimisticConcurrencyBLL`ermöglicht den schnellen Zugriff auf eine Instanz der `ProductsOptimisticConcurrencyTableAdapter`-Klasse und folgt dem Muster, das in den ursprünglichen BLL-Klassen (`ProductsBLL`, `CategoriesBLL`usw.) verwendet wird. Schließlich ruft die `GetProducts()`-Methode einfach die `GetProducts()`-Methode der DAL auf und gibt ein `ProductsOptimisticConcurrencyDataTable` Objekt zurück, das mit einer `ProductsOptimisticConcurrencyRow` Instanz für jeden Produktdaten Satz in der Datenbank aufgefüllt wird.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Löschen eines Produkts mithilfe des direkten Daten Bank Musters mit optimistischer Parallelität

Wenn Sie das direkte DB-Muster für eine dal verwenden, die optimistische Parallelität verwendet, müssen die Methoden die neuen und ursprünglichen Werte aufweisen. Zum Löschen gibt es keine neuen Werte, sodass nur die ursprünglichen Werte übermittelt werden müssen. In unserer BLL müssen wir alle ursprünglichen Parameter als Eingabeparameter akzeptieren. Wir wollen, dass die `DeleteProduct`-Methode in der `ProductsOptimisticConcurrencyBLL`-Klasse die direkte DB-Methode verwendet. Dies bedeutet, dass diese Methode alle zehn Produktdaten Felder als Eingabeparameter übernehmen und diese an die DAL übergeben muss, wie im folgenden Code gezeigt:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Wenn die ursprünglichen Werte (die Werte, die zuletzt in die GridView (oder DetailsView oder FormView) geladen wurden, von den Werten in der Datenbank abweichen, wenn der Benutzer auf die Schaltfläche "Löschen" klickt, wird die `WHERE`-Klausel nicht mit einem Datenbankdaten Satz übereinstimmen, und es sind keine Datensätze betroffen. Daher gibt die `Delete`-Methode des TableAdapter `0` zurück, und die `DeleteProduct`-Methode der BLL gibt `false`zurück.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Aktualisieren eines Produkts mit dem Batch Aktualisierungs Muster mit optimistischer Parallelität

Wie bereits erwähnt, hat die `Update`-Methode des TableAdapter für das Batch Aktualisierungs Muster unabhängig davon, ob die optimistische Parallelität verwendet wird, dieselbe Methoden Signatur. Die `Update`-Methode erwartet eine DataRow, ein Array von DataRows, eine Datentabelle oder ein typisiertes DataSet. Es gibt keine zusätzlichen Eingabeparameter zum Angeben der ursprünglichen Werte. Dies ist möglich, da die Datentabelle die ursprünglichen und geänderten Werte für Ihre DataRow (s) nachverfolgt. Wenn die DAL die `UPDATE`-Anweisung ausgibt, werden die `@original_ColumnName` Parameter mit den ursprünglichen Werten der DataRow aufgefüllt, während die `@ColumnName`-Parameter mit den geänderten Werten der DataRow aufgefüllt werden.

In der `ProductsBLL`-Klasse (in der unsere ursprüngliche, nicht vollständige Parallelitäts dal verwendet wird), führt der Code bei Verwendung des Batch Aktualisierungs Musters zum Aktualisieren von Produktinformationen die folgende Abfolge von Ereignissen aus:

1. Lesen der aktuellen Datenbankprodukt Informationen in eine `ProductRow` Instanz mithilfe der `GetProductByProductID(productID)`-Methode des TableAdapters
2. Weisen Sie die neuen Werte der `ProductRow` Instanz aus Schritt 1 zu.
3. Ruft die `Update`-Methode des TableAdapter auf und übergibt die `ProductRow` Instanz.

Diese schrittweise unterstützt jedoch die vollständige Parallelität nicht ordnungsgemäß, da das in Schritt 1 aufgefüllte `ProductRow` direkt aus der Datenbank aufgefüllt wird, was bedeutet, dass die ursprünglichen Werte, die von der DataRow verwendet werden, diejenigen sind, die derzeit in der Datenbank vorhanden sind, und nicht diejenigen, die zu Beginn des Bearbeitungsvorgangs an die GridView gebunden wurden. Stattdessen müssen wir bei Verwendung einer optimistischen Parallelitäts aktivierten dal die `UpdateProduct`-Methoden Überladungen ändern, um die folgenden Schritte auszuführen:

1. Lesen der aktuellen Datenbankprodukt Informationen in eine `ProductsOptimisticConcurrencyRow` Instanz mithilfe der `GetProductByProductID(productID)`-Methode des TableAdapters
2. Weisen Sie die *ursprünglichen* Werte der `ProductsOptimisticConcurrencyRow` Instanz aus Schritt 1 zu.
3. Aufrufen der `AcceptChanges()`-Methode der `ProductsOptimisticConcurrencyRow` Instanz, die die DataRow anweist, dass die aktuellen Werte den "ursprünglichen" Werten entsprechen
4. Zuweisen der *neuen* Werte zur `ProductsOptimisticConcurrencyRow` Instanz
5. Ruft die `Update`-Methode des TableAdapter auf und übergibt die `ProductsOptimisticConcurrencyRow` Instanz.

Schritt 1 liest alle aktuellen Daten Bankwerte für den angegebenen Produktdaten Satz. Dieser Schritt ist in der `UpdateProduct` Überladung überflüssig, die *alle* Produkt Spalten aktualisiert (da diese Werte in Schritt 2 überschrieben werden), ist aber für die über Ladungen, die nur eine Teilmenge der Spaltenwerte als Eingabeparameter erhalten, von entscheidender Bedeutung. Nachdem die ursprünglichen Werte der `ProductsOptimisticConcurrencyRow` Instanz zugewiesen wurden, wird die `AcceptChanges()`-Methode aufgerufen, mit der die aktuellen DataRow-Werte als ursprüngliche Werte gekennzeichnet werden, die in den `@original_ColumnName`-Parametern in der `UPDATE` Anweisung verwendet werden sollen. Anschließend werden die neuen Parameterwerte dem `ProductsOptimisticConcurrencyRow` zugewiesen, und schließlich wird die `Update`-Methode aufgerufen, wobei die DataRow übergeben wird.

Der folgende Code zeigt die `UpdateProduct` Überladung, die alle Product Data-Felder als Eingabeparameter akzeptiert. Die `ProductsOptimisticConcurrencyBLL`-Klasse, die im Download für dieses Tutorial enthalten ist, enthält auch eine `UpdateProduct` Überladung, die nur den Namen des Produkts und den Preis als Eingabeparameter akzeptiert.

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Schritt 4: übergeben der ursprünglichen und neuen Werte von der ASP.NET-Seite an die BLL-Methoden

Wenn dal und BLL vollständig sind, müssen Sie nur noch eine ASP.NET-Seite erstellen, die die in das System integrierte optimistische Parallelitäts Logik verwenden kann. Insbesondere das datenweb Steuerelement (GridView, DetailsView oder FormView) muss seine ursprünglichen Werte merken, und ObjectDataSource muss beide Sätze von Werten an die Geschäftslogik Schicht übergeben. Außerdem muss die Seite ASP.net so konfiguriert werden, dass Parallelitäts Verletzungen ordnungsgemäß behandelt werden.

Öffnen Sie zunächst die Seite `OptimisticConcurrency.aspx` im Ordner `EditInsertDelete`, und fügen Sie dem Designer eine GridView hinzu. Legen Sie dessen `ID`-Eigenschaft auf `ProductsGrid`fest. Wählen Sie aus dem Smarttag von GridView eine neue ObjectDataSource mit dem Namen `ProductsOptimisticConcurrencyDataSource`. Da diese ObjectDataSource die DAL verwenden soll, die optimistische Parallelität unterstützt, konfigurieren Sie Sie für die Verwendung des `ProductsOptimisticConcurrencyBLL` Objekts.

[![von ObjectDataSource das productoptimisticconfiguration-Objekt verwenden](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Abbildung 13**: Verwenden von ObjectDataSource zum Verwenden des `ProductsOptimisticConcurrencyBLL` Objekts ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image37.png))

Wählen Sie die `GetProducts`-, `UpdateProduct`-und `DeleteProduct` Methoden aus den Dropdown Listen im Assistenten aus. Verwenden Sie für die UpdateProduct-Methode die-Überladung, die alle Datenfelder des Produkts akzeptiert.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Konfigurieren der Eigenschaften des ObjectDataSource-Steuer Elements

Nachdem Sie den Assistenten abgeschlossen haben, sollte das deklarative Markup von ObjectDataSource wie folgt aussehen:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Wie Sie sehen können, enthält die `DeleteParameters` Auflistung eine `Parameter` Instanz für jeden der zehn Eingabeparameter in der `DeleteProduct`-Methode der `ProductsOptimisticConcurrencyBLL`-Klasse. Entsprechend enthält die `UpdateParameters`-Auflistung für jeden Eingabeparameter in `UpdateProduct`eine `Parameter` Instanz.

In den vorherigen Tutorials, in denen die Datenänderung beteiligt war, würden wir an dieser Stelle die `OldValuesParameterFormatString` Eigenschaft von ObjectDataSource entfernen, da diese Eigenschaft angibt, dass die BLL-Methode erwartet, dass die alten (oder ursprünglichen) Werte und die neuen Werte übergeben werden. Außerdem gibt dieser Eigenschafts Wert die Eingabeparameter Namen für die ursprünglichen Werte an. Da wir die ursprünglichen Werte an die BLL übergeben, dürfen Sie diese Eigenschaft *nicht* entfernen.

> [!NOTE]
> Der Wert der `OldValuesParameterFormatString`-Eigenschaft muss den Eingabeparameter Namen in der BLL entsprechen, die die ursprünglichen Werte erwarten. Da wir diese Parameter `original_productName`, `original_supplierID`usw. benannt haben, können Sie den `OldValuesParameterFormatString`-Eigenschafts Wert als `original_{0}`belassen. Wenn jedoch die Eingabeparameter der BLL-Methoden über Namen wie `old_productName`, `old_supplierID`usw. verfügen, müssen Sie die Eigenschaft `OldValuesParameterFormatString` auf `old_{0}`aktualisieren.

Es muss eine letzte Eigenschafts Einstellung vorgenommen werden, damit die ObjectDataSource die ursprünglichen Werte ordnungsgemäß an die BLL-Methoden übergibt. ObjectDataSource weist eine [conflicterkennungs-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) auf, die [einem von zwei Werten](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx)zugewiesen werden kann:

- `OverwriteChanges`: der Standardwert. sendet die ursprünglichen Werte nicht an die ursprünglichen Eingabeparameter der BLL-Methoden.
- `CompareAllValues`: sendet die ursprünglichen Werte an die BLL-Methoden. Wählen Sie diese Option, wenn Sie optimistische Parallelität verwenden

Nehmen Sie sich einen Moment Zeit, um die Eigenschaft `ConflictDetection` auf `CompareAllValues`festzulegen.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Konfigurieren der Eigenschaften und Felder der GridView

Nachdem Sie die Eigenschaften von ObjectDataSource ordnungsgemäß konfiguriert haben, müssen Sie das Einrichten von GridView berücksichtigen. Da in der GridView das Bearbeiten und löschen unterstützt werden soll, klicken Sie zunächst auf das Smarttags von GridView aktivieren und löschen aktivieren. Dadurch wird ein CommandField hinzugefügt, dessen `ShowEditButton` und `ShowDeleteButton` beide auf "`true`" festgelegt sind.

Wenn das GridView-Objekt an den `ProductsOptimisticConcurrencyDataSource` ObjectDataSource gebunden ist, enthält es ein Feld für jedes der Datenfelder des Produkts. Obwohl ein solches GridView-Element bearbeitet werden kann, ist die Benutzer Darstellung alles aber akzeptabel. Die Felder `CategoryID` und `SupplierID` boundfields werden als Textfelder gerencht, sodass der Benutzer die entsprechende Kategorie und den Lieferanten als ID-Nummern eingeben muss. Es gibt keine Formatierung für die numerischen Felder und keine Validierungs Steuerelemente, um sicherzustellen, dass der Name des Produkts angegeben wurde und dass der Einheitspreis, die Einheiten in Aktien, die Einheiten auf der Bestellung und die Neuanordnung von ebenenwerten sowohl ordnungsgemäße numerische Werte als auch größer als oder gleich sind. auf NULL.

Wie im Artikel *Hinzufügen von Validierungs Steuerelementen zu den Bearbeitungs-und einfügeschnittstellen* und dem *Anpassen der Tutorials zur Daten Änderungs Schnittstelle* erläutert, kann die Benutzeroberfläche angepasst werden, indem die boundfields durch templatefields ersetzt werden. Ich habe diese GridView und die Bearbeitungs Schnittstelle wie folgt geändert:

- Die `ProductID`, `SupplierName`und `CategoryName` boundfields wurden entfernt.
- Das `ProductName` BoundField wurde in ein TemplateField konvertiert und ein "Requirements dfieldvalidation"-Steuerelement hinzugefügt.
- Die `CategoryID` und `SupplierID` boundfields wurden in templatefields konvertiert, und die Bearbeitungs Schnittstelle wurde so angepasst, dass DropDownLists anstelle von Textfeldern verwendet werden. In diesen templatefields-`ItemTemplates`werden die Felder `CategoryName` und `SupplierName` Daten angezeigt.
- Die `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`und `ReorderLevel` boundfields wurden in templatefields konvertiert und CompareValidator-Steuerelemente hinzugefügt.

Da wir bereits untersucht haben, wie diese Aufgaben in vorherigen Tutorials ausgeführt werden, führe ich einfach die abschließende deklarative Syntax hier ein und lasse die Implementierung als Übung aus.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Wir sind sehr nah an einem voll Funktions bereiten Beispiel. Allerdings gibt es einige Feinheiten, die zu Problemen führen und uns Probleme verursachen. Außerdem benötigen wir immer noch eine Schnittstelle, die den Benutzer benachrichtigt, wenn eine Parallelitäts Verletzung aufgetreten ist.

> [!NOTE]
> Damit ein datenweb-Steuerelement die ursprünglichen Werte ordnungsgemäß an die ObjectDataSource übergibt (die dann an die BLL übergeben werden), ist es wichtig, dass die `EnableViewState`-Eigenschaft der GridView auf `true` (Standard) festgelegt ist. Wenn Sie den Ansichts Zustand deaktivieren, gehen die ursprünglichen Werte beim Postback verloren.

## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Übergeben der korrekten ursprünglichen Werte an die ObjectDataSource

Es gibt einige Probleme mit der Art und Weise, wie die GridView konfiguriert wurde. Wenn die `ConflictDetection`-Eigenschaft von ObjectDataSource auf `CompareAllValues` festgelegt ist (wie es bei uns der Fall ist), versucht ObjectDataSource, die ursprünglichen Werte der GridView in die entsprechenden `Parameter` Instanzen zu kopieren, wenn die `Update()`-oder `Delete()`-Methoden von ObjectDataSource von der GridView (oder DetailsView oder FormView) aufgerufen werden. Eine grafische Darstellung dieses Prozesses finden Sie in Abbildung 2.

Insbesondere werden den ursprünglichen Werten der GridView die Werte in den bidirektionalen Datenbindung-Anweisungen zugewiesen, wenn die Daten an die GridView gebunden werden. Daher ist es von entscheidender Bedeutung, dass die erforderlichen ursprünglichen Werte alle über die bidirektionale Datenbindung aufgezeichnet werden und dass Sie in einem konvertierbaren Format bereitgestellt werden.

Um zu sehen, warum dies wichtig ist, nehmen Sie sich einen Moment Zeit, um unsere Seite in einem Browser zu besuchen. Erwartungsgemäß listet die GridView die einzelnen Produkte mit der Schaltfläche Bearbeiten und löschen in der Spalte ganz links auf.

[![die Produkte in einer GridView aufgeführt sind](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Abbildung 14**: die Produkte werden in einer GridView aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image40.png))

Wenn Sie auf die Schaltfläche "Löschen" für ein beliebiges Produkt klicken, wird eine `FormatException` ausgelöst.

[![Versuch, ein Produkt zu löschen, führt eine FormatException aus.](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Abbildung 15**: Es wird versucht, Produktergebnisse in einem `FormatException` zu löschen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image43.png))

Der `FormatException` wird ausgelöst, wenn ObjectDataSource versucht, den ursprünglichen `UnitPrice` Wert zu lesen. Da der `ItemTemplate` den `UnitPrice` als Währung (`<%# Bind("UnitPrice", "{0:C}") %>`) formatiert hat, enthält er ein Währungssymbol, z. b. $19,95. Der `FormatException` tritt auf, wenn die ObjectDataSource versucht, diese Zeichenfolge in eine `decimal`zu konvertieren. Um dieses Problem zu umgehen, haben wir eine Reihe von Optionen:

- Entfernen Sie die Währungs Formatierung aus der `ItemTemplate`. Das heißt, statt `<%# Bind("UnitPrice", "{0:C}") %>`zu verwenden, verwenden Sie einfach `<%# Bind("UnitPrice") %>`. Der Nachteil hierbei ist, dass der Preis nicht mehr formatiert ist.
- Zeigen Sie die im `ItemTemplate`als Währung formatierte `UnitPrice` an, aber verwenden Sie hierfür das `Eval` Schlüsselwort. Denken Sie daran, dass `Eval` eine unidirektionale Datenbindung ausführt. Wir müssen weiterhin den `UnitPrice` Wert für die ursprünglichen Werte angeben, sodass wir weiterhin eine bidirektionale Datenbindung-Anweisung in der `ItemTemplate`benötigen. Dies kann jedoch in einem Label-websteuer Element platziert werden, dessen `Visible`-Eigenschaft auf `false`festgelegt ist. Wir könnten das folgende Markup in ItemTemplate verwenden:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Entfernen Sie die Währungs Formatierung mithilfe `<%# Bind("UnitPrice") %>`aus der `ItemTemplate`. Greifen Sie im `RowDataBound` Ereignishandler von GridView Programm gesteuert auf das Beschriftungs-websteuer Element zu, in dem der `UnitPrice` Wert angezeigt wird, und legen Sie dessen `Text`-Eigenschaft auf die formatierte Version fest.
- Belassen Sie die `UnitPrice` als Währung formatiert. Ersetzen Sie im `RowDeleting` Ereignishandler von GridView den vorhandenen ursprünglichen `UnitPrice` Wert ($19,95) durch einen tatsächlichen Dezimalwert, indem Sie `Decimal.Parse`verwenden. Wir haben gesehen, wie Sie im `RowUpdating`-Ereignishandler im Tutorial [*Behandeln von BLL-und Dal-Ebene in einem ASP.NET-Seiten*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Lernprogramm etwas Ähnliches erreichen.

Für mein Beispiel habe ich mich für den zweiten Ansatz entschieden, indem ich ein ausgeblendetes Bezeichnungs-websteuer Element hinzu Füge, dessen `Text` Eigenschaft bidirektionale Daten an den nicht formatierten `UnitPrice` Wert gebunden ist

Nachdem Sie dieses Problem gelöst haben, klicken Sie erneut auf die Schaltfläche Löschen für ein beliebiges Produkt. Dieses Mal erhalten Sie eine `InvalidOperationException`, wenn die ObjectDataSource versucht, die `UpdateProduct`-Methode der BLL aufzurufen.

[![die ObjectDataSource keine Methode mit den Eingabe Parametern finden kann, die Sie senden möchte.](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Abbildung 16**: ObjectDataSource kann keine Methode mit den Eingabe Parametern finden, die Sie senden möchten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image46.png))

Wenn Sie die Meldung der Ausnahme betrachten, ist klar, dass ObjectDataSource eine BLL-`DeleteProduct` Methode aufrufen möchte, die `original_CategoryName` und `original_SupplierName` Eingabeparameter enthält. Dies liegt daran, dass die `ItemTemplate` s für die `CategoryID` und `SupplierID` templatefields derzeit bidirektionale BIND-Anweisungen mit den Datenfeldern `CategoryName` und `SupplierName` enthalten. Stattdessen müssen wir `Bind`-Anweisungen in die Datenfelder `CategoryID` und `SupplierID` einschließen. Ersetzen Sie hierzu die vorhandenen BIND-Anweisungen durch `Eval`-Anweisungen, und fügen Sie dann verborgene Label-Steuerelemente hinzu, deren `Text` Eigenschaften an die `CategoryID` und `SupplierID` Datenfelder mithilfe der bidirektionalen Datenbindung gebunden sind, wie unten dargestellt:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Mit diesen Änderungen können wir nun Produktinformationen erfolgreich löschen und bearbeiten. In Schritt 5 wird erläutert, wie Sie überprüfen, ob Parallelitäts Verletzungen erkannt werden. Nehmen Sie aber vorerst einige Minuten Zeit, um zu versuchen, einige Datensätze zu aktualisieren und zu löschen, um sicherzustellen, dass das Aktualisieren und Löschen für einen einzelnen Benutzer erwartungsgemäß funktioniert.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Schritt 5: Testen der Unterstützung für die vollständige Parallelität

Um zu überprüfen, ob Parallelitäts Verletzungen erkannt werden (statt zu Daten, die Blind überschrieben werden), müssen wir auf dieser Seite zwei Browserfenster öffnen. Klicken Sie in beiden Browser Instanzen auf die Schaltfläche Bearbeiten für Chai. Ändern Sie dann in nur einem der Browser den Namen in "Chai Tea", und klicken Sie auf aktualisieren. Das Update sollte erfolgreich sein und die GridView in den Zustand der vorabbearbeitung zurücksetzen, wobei "Chai Tea" als neuer Produktname angezeigt wird.

In der anderen Browserfenster Instanz zeigt das Textfeld "Product Name" jedoch immer noch "Chai" an. Aktualisieren Sie in diesem zweiten Browserfenster die `UnitPrice` auf `25.00`. Ohne Unterstützung der vollständigen Parallelität würde der Produktname durch Klicken auf Update in der zweiten Browser Instanz wieder in "Chai" geändert werden. Dadurch werden die von der ersten Browser Instanz vorgenommenen Änderungen überschrieben. Wenn jedoch die vollständige Parallelität eingesetzt wird, führt das Klicken auf die Schaltfläche Aktualisieren in der zweiten Browser Instanz zu einer dbdinsecurrency- [Ausnahme](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).

[![wenn eine Parallelitäts Verletzung festgestellt wird, wird eine DB-Parallelitäts Ausnahme ausgelöst.](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Abbildung 17**: Wenn eine Parallelitäts Verletzung erkannt wird, wird ein `DBConcurrencyException` ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image49.png))

Der `DBConcurrencyException` wird nur ausgelöst, wenn das Batch Aktualisierungs Muster der dal verwendet wird. Das direkte DB-Muster gibt keine Ausnahme aus, sondern gibt lediglich an, dass keine Zeilen betroffen sind. Um dies zu veranschaulichen, geben Sie die GridView der Browser Instanzen in den Zustand der vorabbearbeitung zurück. Klicken Sie anschließend in der ersten Browser Instanz auf die Schaltfläche Bearbeiten, und ändern Sie den Produktnamen von "Chai Tea" zurück in "Chai". Klicken Sie dann auf aktualisieren. Klicken Sie im zweiten Browserfenster auf die Schaltfläche Löschen für Chai.

Wenn Sie auf Löschen klicken, wird die Seite zurückgesendet, die GridView ruft die `Delete()`-Methode von ObjectDataSource auf, und die ObjectDataSource Ruft die `DeleteProduct`-Methode der `ProductsOptimisticConcurrencyBLL` Klasse auf und übergibt die ursprünglichen Werte. Der ursprüngliche `ProductName` Wert für die zweite Browser Instanz ist "Chai Tea", die nicht mit dem aktuellen `ProductName` Wert in der Datenbank in Einklang steht. Daher wirkt sich die für die Datenbank ausgegebene `DELETE`-Anweisung auf null Zeilen aus, da in der Datenbank kein Datensatz vorhanden ist, den die `WHERE`-Klausel erfüllt. Die `DeleteProduct`-Methode gibt `false` zurück, und die Daten von ObjectDataSource werden wieder an die GridView-Sicht zurückgegeben.

Aus Sicht des Endbenutzers hat der Bildschirm durch Klicken auf die Schaltfläche "Löschen" für "Chai Tea" im zweiten Browserfenster blinkt, und bei der Rückkehr ist das Produkt weiterhin vorhanden, obwohl es nun als "Chai" (die vom ersten Browser vorgenommene Produkt Namensänderung) aufgeführt ist. Instanz). Wenn der Benutzer erneut auf die Schaltfläche "Löschen" klickt, wird der Löschvorgang erfolgreich ausgeführt, da der ursprüngliche `ProductName` Wert ("Chai") der GridView nun mit dem Wert in der Datenbank übereinstimmt.

In beiden Fällen ist die Benutzer Darstellung weit von ideal. Bei Verwendung des Batch Aktualisierungs Musters möchten wir dem Benutzer deutlich nicht die Details des `DBConcurrencyException` Ausnahme Fehler zeigen. Das Verhalten bei der Verwendung des direkten Daten Bank Musters ist etwas verwirrend, da der Benutzer Befehl fehlgeschlagen ist, aber es gab keine genaue Angabe, warum dies der Fall ist.

Um diese beiden Probleme zu beheben, können wir Bezeichnungs-websteuer Elemente auf der Seite erstellen, die Aufschluss darüber geben, warum ein Update oder ein Löschvorgang fehlgeschlagen ist. Für das Batch Aktualisierungs Muster können wir bestimmen, ob eine `DBConcurrencyException` Ausnahme im Ereignishandler der GridView-Methode aufgetreten ist. dabei wird die Warnungs Bezeichnung nach Bedarf angezeigt. Für die direkte DB-Methode können wir den Rückgabewert der BLL-Methode überprüfen (was `true` ist, wenn eine Zeile betroffen war `false` andernfalls) und eine Informations Meldung nach Bedarf anzeigen.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Schritt 6: Hinzufügen von Informationsmeldungen und Anzeigen der Meldungen bei einer Parallelitäts Verletzung

Wenn eine Parallelitäts Verletzung auftritt, hängt das aufgezeigte Verhalten davon ab, ob das Batch Update oder das DB Direct-Muster der dal verwendet wurde. In unserem Tutorial werden beide Muster verwendet, wobei das Batch Aktualisierungs Muster zum Aktualisieren verwendet wird, und das direkte DB-Muster zum Löschen. Zum Einstieg fügen wir der Seite zwei Bezeichnungs-websteuer Elemente hinzu, die erläutern, dass beim Versuch, Daten zu löschen oder zu aktualisieren, eine Parallelitäts Verletzung aufgetreten ist. Legen Sie die `Visible`-und `EnableViewState` Eigenschaften des Label-Steuer Elements auf `false`fest. Dies führt dazu, dass Sie auf jedem Seitenbesuch ausgeblendet werden, mit Ausnahme der einzelnen Seitenbesuche, bei denen Ihre `Visible`-Eigenschaft Programm gesteuert auf `true`festgelegt ist.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Zusätzlich zum Festlegen der Eigenschaften für `Visible`, `EnabledViewState`und `Text` habe ich auch die `CssClass`-Eigenschaft auf `Warning`festgelegt, was bewirkt, dass die Bezeichnung in einer großen, roten, kursiv, fett formatierten Schriftart angezeigt wird. Diese CSS-`Warning` Klasse wurde definiert und zu "Styles. CSS" hinzugefügt, um *die Ereignisse zu untersuchen, die mit dem Einfügen, aktualisieren und Löschen des Tutorials verknüpft sind* .

Nachdem diese Bezeichnungen hinzugefügt wurden, sollte der Designer in Visual Studio in etwa wie in Abbildung 18 aussehen.

[![zwei Label-Steuerelemente wurden der Seite hinzugefügt.](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Abbildung 18**: zwei Label-Steuerelemente wurden der Seite hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image52.png))

Wenn diese Beschriftungs-websteuer Elemente vorhanden sind, können Sie untersuchen, wie Sie ermitteln, wann eine Parallelitäts Verletzung aufgetreten ist. zu diesem Zeitpunkt können Sie die `Visible`-Eigenschaft der entsprechenden Bezeichnung auf `true`festlegen und die Informations Meldung anzeigen.

## <a name="handling-concurrency-violations-when-updating"></a>Behandeln von Parallelitäts Verletzungen beim Aktualisieren

Sehen wir uns zunächst an, wie neben läufigkeits Verletzungen bei Verwendung des Batch Aktualisierungs Musters behandelt werden. Da solche Verstöße mit dem Batch Aktualisierungs Muster bewirken, dass eine `DBConcurrencyException` Ausnahme ausgelöst wird, müssen wir der ASP.NET-Seite Code hinzufügen, um zu bestimmen, ob während des Aktualisierungs Vorgangs eine `DBConcurrencyException` Ausnahme aufgetreten ist. Wenn dies der Fall ist, wird dem Benutzer eine Meldung angezeigt, die anzeigt, dass die Änderungen nicht gespeichert wurden, weil ein anderer Benutzer die gleichen Daten zwischen dem Startzeitpunkt der Daten Satzänderung und dem Klicken auf die Schaltfläche Aktualisieren geändert hat.

Wie in der Behandlung von *Ausnahmen auf BLL-und Dal-Ebene in einem ASP.NET-Seiten* Lernprogramm erläutert, können solche Ausnahmen in den Ereignis Handlern auf der Post-Ebene des Data Web-Steuer Elements erkannt und unterdrückt werden. Daher müssen wir einen Ereignishandler für das `RowUpdated` Ereignis der GridView erstellen, das überprüft, ob eine `DBConcurrencyException` Ausnahme ausgelöst wurde. Diesem Ereignishandler wird ein Verweis auf jede Ausnahme, die während des Aktualisierungs Vorgangs ausgelöst wurde, weitergegeben, wie im folgenden Code dargestellt:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Bei einer `DBConcurrencyException` Ausnahme zeigt dieser Ereignishandler das Steuerelement `UpdateConflictMessage` Bezeichnung an und gibt an, dass die Ausnahme behandelt wurde. Wenn dieser Code vorhanden ist und eine Parallelitäts Verletzung auftritt, wenn ein Datensatz aktualisiert wird, gehen die Änderungen des Benutzers verloren, da Sie gleichzeitig die Änderungen anderer Benutzer überschrieben hätten. Insbesondere wird die GridView an den Zustand der vorabbearbeitung zurückgegeben und an die aktuellen Datenbankdaten gebunden. Dadurch wird die GridView-Zeile mit den Änderungen des anderen Benutzers aktualisiert, die zuvor nicht sichtbar waren. Außerdem erläutert das Steuerelement `UpdateConflictMessage` Bezeichnung dem Benutzer, was gerade passiert ist. Diese Ereignis Sequenz wird in Abbildung 19 ausführlich erläutert.

[![die Updates eines Benutzers bei einer Parallelitäts Verletzung verloren gehen.](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Abbildung 19**: die Updates eines Benutzers gehen bei einer Parallelitäts Verletzung verloren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image55.png))

> [!NOTE]
> Anstatt das GridView-Objekt in den Zustand vor der Bearbeitung zurückzukehren, könnten wir die GridView-Eigenschaft im Bearbeitungs Zustand belassen, indem wir die `KeepInEditMode`-Eigenschaft des übergebenen `GridViewUpdatedEventArgs` Objekts auf "true" festlegen. Wenn Sie diesen Ansatz verwenden, müssen Sie jedoch sicherstellen, dass die Daten erneut an die GridView gebunden werden (indem Sie die `DataBind()`-Methode aufrufen), damit die Werte des anderen Benutzers in die Bearbeitungs Schnittstelle geladen werden. Der Code, der in diesem Tutorial zum Herunterladen verfügbar ist, enthält die folgenden zwei Codezeilen im `RowUpdated` Ereignis Handlers auskommentiert. Entfernen Sie einfach die Auskommentierung dieser Codezeilen, damit die GridView nach einer Parallelitäts Verletzung im Bearbeitungsmodus bleibt.

## <a name="responding-to-concurrency-violations-when-deleting"></a>Reagieren auf Parallelitäts Verletzungen beim Löschen

Beim direkten Daten Bank Muster wird bei einer Parallelitäts Verletzung keine Ausnahme ausgelöst. Stattdessen wirkt sich die Daten Bank Anweisung einfach nicht auf Datensätze aus, da die WHERE-Klausel keinem Datensatz entspricht. Alle in der BLL erstellten Daten Änderungs Methoden wurden so entworfen, dass Sie einen booleschen Wert zurückgeben, der angibt, ob Sie genau einen Datensatz beeinflusst haben. Um zu ermitteln, ob beim Löschen eines Datensatzes eine Parallelitäts Verletzung aufgetreten ist, können wir daher den Rückgabewert der `DeleteProduct`-Methode der BLL überprüfen.

Der Rückgabewert für eine BLL-Methode kann in den Ereignis Handlern der ObjectDataSource-Methode durch die `ReturnValue`-Eigenschaft des `ObjectDataSourceStatusEventArgs`-Objekts, das an den Ereignishandler übergeben wird, untersucht werden. Da wir den Rückgabewert der `DeleteProduct`-Methode ermitteln möchten, müssen wir einen Ereignishandler für das `Deleted`-Ereignis von ObjectDataSource erstellen. Die `ReturnValue`-Eigenschaft ist vom Typ `object` und kann `null` werden, wenn eine Ausnahme ausgelöst wurde und die Methode unterbrochen wurde, bevor ein Wert zurückgegeben werden konnte. Daher sollten Sie zunächst sicherstellen, dass die `ReturnValue`-Eigenschaft nicht `null` und ein boolescher Wert ist. Wenn diese Überprüfung verstrichen ist, wird das Steuerelement `DeleteConflictMessage` Bezeichnung angezeigt, wenn die `ReturnValue` `false`ist. Dies kann mithilfe des folgenden Codes erreicht werden:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

Bei einer Parallelitäts Verletzung wird die Löschanforderung des Benutzers abgebrochen. Die GridView wird aktualisiert und zeigt die Änderungen, die für diesen Datensatz aufgetreten sind, zwischen dem Zeitpunkt, zu dem der Benutzer die Seite geladen hat, und dem Klicken auf die Schaltfläche Löschen. Wenn eine solche Verletzung auftritt, wird der `DeleteConflictMessage` Bezeichnung angezeigt, und es wird erläutert, was gerade passiert ist (siehe Abbildung 20).

[![das Löschen eines Benutzers bei einer Parallelitäts Verletzung abgebrochen wird](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Abbildung 20**: der Löschvorgang eines Benutzers wird bei einer Parallelitäts Verletzung abgebrochen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-vb/_static/image58.png))

## <a name="summary"></a>Summary

Möglichkeiten für Parallelitäts Verletzungen sind in jeder Anwendung vorhanden, die mehreren, gleichzeitigen Benutzern das Aktualisieren oder Löschen von Daten ermöglicht. Wenn solche Verstöße nicht berücksichtigt werden, wenn zwei Benutzer gleichzeitig dieselben Daten aktualisieren, die sich im letzten Schreibvorgang befinden, ändert sich das Überschreiben der Änderungen des anderen Benutzers. Alternativ können Entwickler entweder die vollständige oder die pessimistische Parallelitäts Steuerung implementieren. Die Steuerung der vollständigen Parallelität geht davon aus, dass neben läufigkeits Verletzungen selten auftreten und ein Update-oder DELETE-Befehl, der eine Parallelitäts Verletzung darstellen würde, nicht zulässt. Die pessimistische Parallelitäts Steuerung geht davon aus, dass Parallelitäts Verletzungen häufig auftreten und dass der Update-oder DELETE-Befehl eines Benutzers nicht akzeptiert wird. Bei der Steuerung der pessimistischen Parallelität umfasst das Aktualisieren eines Datensatzes das Sperren. Dadurch wird verhindert, dass andere Benutzer den Datensatz ändern oder löschen, während er gesperrt ist.

Das typisierte DataSet in .net bietet Funktionen für die Unterstützung der Steuerung für optimistische Parallelität. Insbesondere enthalten die `UPDATE`-und `DELETE` Anweisungen, die für die Datenbank ausgegeben werden, alle Spalten der Tabelle. Dadurch wird sichergestellt, dass das Update oder das Löschen nur dann erfolgt, wenn die aktuellen Daten des Datensatzes mit den ursprünglichen Daten übereinstimmen, die der Benutzer beim Durchführen der Aktualisierung oder Löschung besaß. Nachdem die DAL so konfiguriert wurde, dass optimistische Parallelität unterstützt wird, müssen die BLL-Methoden aktualisiert werden. Darüber hinaus muss die ASP.NET Seite, die den BLL aufruft, so konfiguriert werden, dass die ObjectDataSource die ursprünglichen Werte aus dem datenweb-Steuerelement abruft und Sie an die BLL übergibt.

Wie in diesem Tutorial gezeigt, umfasst das Implementieren der Steuerung der vollständigen Parallelität in einer ASP.NET-Webanwendung das Aktualisieren von Dal und BLL und das Hinzufügen von Unterstützung auf der ASP.NET-Seite. Unabhängig davon, ob diese hinzugefügte Arbeit eine Weise Investition ihrer Zeit und ihres Aufwands ist, hängt von Ihrer Anwendung ab. Wenn Sie selten gleichzeitige Benutzer haben, die Daten aktualisieren, oder wenn sich die zu aktualisierenden Daten voneinander unterscheiden, ist die Parallelitäts Steuerung kein wichtiges Problem. Wenn Sie jedoch regelmäßig mehrere Benutzer auf Ihrer Website mit denselben Daten arbeiten, kann die Parallelitäts Steuerung dabei helfen, die Updates oder Löschvorgänge eines Benutzers zu verhindern, dass die Änderungen eines anderen Benutzers überschrieben werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](customizing-the-data-modification-interface-vb.md)
> [Weiter](adding-client-side-confirmation-when-deleting-vb.md)
