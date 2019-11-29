---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Sortieren von benutzerdefinierten auslagerenen Daten (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Im vorherigen Tutorial haben Sie erfahren, wie Sie benutzerdefiniertes Paging implementieren, wenn Sie Daten auf einer Webseite darstellen. In diesem Tutorial erfahren Sie, wie Sie das obige erweitern...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e55ed9b92814753e95bdfdf26c2f051df6f2630d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642392"
---
# <a name="sorting-custom-paged-data-c"></a>Sortieren von benutzerdefinierten ausgelagerten Daten (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) oder [PDF herunterladen](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Im vorherigen Tutorial haben Sie erfahren, wie Sie benutzerdefiniertes Paging implementieren, wenn Sie Daten auf einer Webseite darstellen. In diesem Tutorial erfahren Sie, wie Sie das vorherige Beispiel erweitern, um die Unterstützung für das Sortieren von benutzerdefiniertem Paging zu bieten

## <a name="introduction"></a>Einführung

Im Vergleich zum Standard-Paging kann durch benutzerdefiniertes Paging die Leistung beim Paging durch Daten um verschiedene Größenordnungen verbessert werden. Dadurch wird das benutzerdefinierte Paging beim Paging durch große Datenmengen zur Deserialisierung der Auslagerungs Auswahl. Das Implementieren von benutzerdefiniertem Paging ist besser als die Implementierung der standardpaginierung, insbesondere beim Hinzufügen von Sortierungen zur Mischung. In diesem Tutorial erweitern wir das Beispiel aus dem vorherigen Beispiel, um Unterstützung für das Sortieren *und* das benutzerdefinierte Paging zu bieten.

> [!NOTE]
> Da dieses Lernprogramm auf dem vorhergehenden erstellt wird, müssen Sie zunächst die deklarative Syntax in das `<asp:Content>`-Element von der vorherigen Tutorial s-Webseite (`EfficientPaging.aspx`) kopieren und zwischen dem `<asp:Content>`-Element auf der `SortParameter.aspx` Seite einfügen. Eine ausführlichere Erläuterung zur Replikation der Funktionalität einer ASP.NET-Seite finden Sie in Schritt 1 des Tutorials [Hinzufügen von Validierungs Steuerelementen zum Bearbeitungs-und einfügeschnittstellen-](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) Tutorial.

## <a name="step-1-reexamining-the-custom-paging-technique"></a>Schritt 1: Untersuchen des benutzerdefinierten pagingverfahrens

Damit das benutzerdefinierte Paging ordnungsgemäß funktioniert, müssen wir einige Techniken implementieren, die eine bestimmte Teilmenge von Datensätzen mit den Parametern "Start Row Index" und "Maximum rows" effizient erfassen können. Es gibt eine Reihe von Techniken, die dazu verwendet werden können, dieses Ziel zu erreichen. Im vorherigen Tutorial haben wir uns mit dem Erreichen dieser Funktion mit der neuen `ROW_NUMBER()` Rang Folge Funktion von Microsoft SQL Server 2005 befasst. Kurz gesagt, weist die `ROW_NUMBER()` Rang Folge Funktion jeder Zeile, die von einer Abfrage zurückgegeben wird, die nach einer angegebenen Sortierreihenfolge sortiert wird, eine Zeilennummer zu. Die entsprechende Teilmenge der Datensätze wird dann abgerufen, indem ein bestimmter Abschnitt der nummerierten Ergebnisse zurückgegeben wird. Die folgende Abfrage veranschaulicht, wie diese Methode verwendet wird, um die Produkte 11 bis 20 zurückzugeben, wenn die Ergebnisse alphabetisch nach dem `ProductName`geordnet werden:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Dieses Verfahren eignet sich gut für das Paging mit einer bestimmten Sortierreihenfolge (in diesem Fall`ProductName` alphabetisch sortiert). die Abfrage muss jedoch geändert werden, um die Ergebnisse nach einem anderen Sortier Ausdruck sortieren zu können. Im Idealfall könnte die obige Abfrage so umgeschrieben werden, dass Sie einen Parameter in der `OVER`-Klausel wie folgt verwendet:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Leider sind parametrisierte `ORDER BY` Klauseln nicht zulässig. Stattdessen müssen Sie eine gespeicherte Prozedur erstellen, die einen `@sortExpression` Eingabeparameter akzeptiert, aber eine der folgenden Problem Umgehungen verwendet:

- Schreiben Sie hart codierte Abfragen für jeden Sortier Ausdruck, der verwendet werden kann. Verwenden Sie dann `IF/ELSE` t-SQL-Anweisungen, um zu bestimmen, welche Abfrage ausgeführt werden soll.
- Verwenden Sie eine `CASE`-Anweisung, um dynamische `ORDER BY` Ausdrücke basierend auf dem `@sortExpressio` n-Eingabeparameter bereitzustellen. Weitere Informationen finden Sie im Abschnitt Verwenden von zum dynamischen Sortieren von Abfrage Ergebnissen in [der Leistungsfähigkeit von SQL `CASE`-Anweisungen](http://www.4guysfromrolla.com/webtech/102704-1.shtml) .
- Erstellen Sie die entsprechende Abfrage als Zeichenfolge in der gespeicherten Prozedur, und verwenden Sie dann [die gespeicherte System Prozedur `sp_executesql`](https://msdn.microsoft.com/library/ms188001.aspx) , um die dynamische Abfrage auszuführen.

Jede dieser Problem Umgehungen hat einige Nachteile. Die erste Option ist nicht so verwaltbar wie die anderen beiden, da Sie erfordert, dass Sie für jeden möglichen Sortier Ausdruck eine Abfrage erstellen. Wenn Sie sich später entscheiden, der GridView neue sortierbare Felder hinzuzufügen, müssen Sie die gespeicherte Prozedur ebenfalls aktualisieren. Der zweite Ansatz bietet einige Feinheiten, die beim Sortieren von Daten Bank Spalten, die keine Zeichenfolge sind, zu Leistungsproblemen führen und auch die gleichen wart barkeits Probleme wie die erste haben. Und die dritte Auswahl, die dynamisches SQL verwendet, stellt das Risiko für einen SQL-Injection-Angriff vor, wenn ein Angreifer die gespeicherte Prozedur ausführen und die Eingabeparameter Werte Ihrer Wahl übergeben kann.

Obwohl keiner dieser Ansätze perfekt ist, denke ich, dass die dritte Option die beste der drei Optionen ist. Durch die Verwendung von dynamischem SQL bietet das Unternehmen eine höhere Flexibilität. Außerdem kann ein SQL-einschleusungs Angriff nur ausgenutzt werden, wenn ein Angreifer die gespeicherte Prozedur ausführen kann, die die Eingabeparameter seiner Wahl übergibt. Da in der dal parametrisierte Abfragen verwendet werden, schützt ADO.net die Parameter, die über die Architektur an die Datenbank gesendet werden. das bedeutet, dass das Sicherheitsrisiko des SQL-Injection-Angriffs nur vorhanden ist, wenn der Angreifer die gespeicherte Prozedur direkt ausführen kann.

Um diese Funktionalität zu implementieren, erstellen Sie eine neue gespeicherte Prozedur in der Northwind-Datenbank mit dem Namen `GetProductsPagedAndSorted`. Diese gespeicherte Prozedur sollte drei Eingabeparameter akzeptieren: `@sortExpression`, einen Eingabeparameter vom Typ `nvarchar(100`), der angibt, wie die Ergebnisse sortiert werden sollen und direkt nach dem `ORDER BY` Text in der `OVER`-Klausel eingefügt werden. und `@startRowIndex` und `@maximumRows`die gleichen beiden ganzzahligen Eingabeparameter aus der gespeicherten Prozedur `GetProductsPaged`, die im vorherigen Tutorial untersucht wurde. Erstellen Sie die gespeicherte Prozedur `GetProductsPagedAndSorted` mithilfe des folgenden Skripts:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Die gespeicherte Prozedur beginnt mit dem sicherstellen, dass ein Wert für den `@sortExpression`-Parameter angegeben wurde. Wenn Sie nicht vorhanden ist, werden die Ergebnisse nach `ProductID`sortiert. Im nächsten Schritt wird die dynamische SQL-Abfrage erstellt. Beachten Sie, dass die dynamische SQL-Abfrage hier geringfügig von den vorherigen Abfragen abweicht, die zum Abrufen aller Zeilen aus der Products-Tabelle verwendet wurden. In den vorherigen Beispielen haben wir die Namen der Kategorie s und Lieferanten-e mithilfe einer Unterabfrage abgerufen. Diese Entscheidung wurde im Lernprogramm zum [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-cs.md) zurückgegeben und wurde anstelle der Verwendung von `JOIN` s vorgenommen, da der TableAdapter nicht automatisch die zugehörigen INSERT-, Update-und Delete-Methoden für solche Abfragen erstellen kann. Die gespeicherte Prozedur `GetProductsPagedAndSorted` muss jedoch `JOIN` s verwenden, damit die Ergebnisse nach Kategorie-oder Lieferanten Namen sortiert werden.

Diese dynamische Abfrage wird erstellt, indem die statischen Abfrage Teile und die Parameter `@sortExpression`, `@startRowIndex`und `@maximumRows` verkettet werden. Da `@startRowIndex` und `@maximumRows` ganzzahlige Parameter sind, müssen Sie in nvarchars konvertiert werden, damit Sie ordnungsgemäß verkettet werden können. Nachdem diese dynamische SQL-Abfrage erstellt wurde, wird Sie über `sp_executesql`ausgeführt.

Nehmen Sie sich einen Moment Zeit, um diese gespeicherte Prozedur mit unterschiedlichen Werten für die Parameter `@sortExpression`, `@startRowIndex`und `@maximumRows` zu testen. Klicken Sie im Server-Explorer mit der rechten Maustaste auf den Namen der gespeicherten Prozedur, und wählen Sie ausführen aus. Dadurch wird das Dialogfeld gespeicherte Prozedur ausführen angezeigt, in das Sie die Eingabeparameter eingeben können (siehe Abbildung 1). Um die Ergebnisse nach Kategorien Amen zu sortieren, verwenden Sie CategoryName für den `@sortExpression` Parameterwert. um nach dem Firmennamen des Lieferanten zu sortieren, verwenden Sie CompanyName. Nachdem Sie die Parameterwerte angegeben haben, klicken Sie auf OK. Die Ergebnisse werden im Fenster Ausgabe angezeigt. Abbildung 2 zeigt die Ergebnisse bei der Rückgabe von Produkten in der Rangfolge 11 bis 20, wenn die Sortierung nach dem `UnitPrice` in absteigender Reihenfolge

![Verwenden Sie unterschiedliche Werte für die drei Eingabeparameter der gespeicherten Prozedur.](sorting-custom-paged-data-cs/_static/image1.png)

**Abbildung 1**: ausprobieren von unterschiedlichen Werten für die drei Eingabeparameter der gespeicherten Prozedur

[![werden die Ergebnisse der gespeicherten Prozedur in der Ausgabefenster angezeigt.](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Abbildung 2**: die Ergebnisse der gespeicherten Prozeduren werden in der Ausgabefenster angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-custom-paged-data-cs/_static/image4.png))

> [!NOTE]
> Wenn die Ergebnisse mit der angegebenen `ORDER BY` Spalte in der `OVER`-Klausel sortiert werden, müssen die Ergebnisse von SQL Server sortiert werden. Dies ist ein schneller Vorgang, wenn ein gruppierter Index für die Spalten vorhanden ist, nach denen die Ergebnisse geordnet werden, oder wenn es einen Abdeck enden Index gibt, der andernfalls kostengünstiger sein kann. Um die Leistung für ausreichend große Abfragen zu verbessern, sollten Sie einen nicht gruppierten Index für die Spalte hinzufügen, nach der die Ergebnisse geordnet sind. Weitere Informationen finden Sie unter [Rang Folge Funktionen und Leistung in SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Schritt 2: Erweitern der Datenzugriffs-und Geschäftslogik Ebenen

Wenn die gespeicherte Prozedur `GetProductsPagedAndSorted` erstellt wurde, besteht der nächste Schritt darin, eine Möglichkeit zum Ausführen dieser gespeicherten Prozedur über unsere Anwendungsarchitektur bereitzustellen. Dies umfasst das Hinzufügen einer entsprechenden Methode zu dal und BLL. Beginnen Sie, indem Sie der dal eine Methode hinzufügen. Öffnen Sie das `Northwind.xsd` typisierte DataSet, klicken Sie mit der rechten Maustaste auf die `ProductsTableAdapter`, und wählen Sie im Kontextmenü die Option Abfrage hinzufügen aus. Wie bereits im vorherigen Tutorial beschrieben, möchten wir diese neue dal-Methode so konfigurieren, dass Sie eine vorhandene gespeicherte Prozedur `GetProductsPagedAndSorted`verwendet, in diesem Fall. Beginnen Sie mit der Angabe, dass die neue TableAdapter-Methode eine vorhandene gespeicherte Prozedur verwenden soll.

![Wählen Sie eine vorhandene gespeicherte Prozedur aus.](sorting-custom-paged-data-cs/_static/image5.png)

**Abbildung 3**: Auswählen der Verwendung einer vorhandenen gespeicherten Prozedur

Um die zu verwendende gespeicherte Prozedur anzugeben, wählen Sie die gespeicherte Prozedur `GetProductsPagedAndSorted` aus der Dropdown Liste auf dem nächsten Bildschirm aus.

![Verwenden Sie die gespeicherte Prozedur getproductspagedandsortierte.](sorting-custom-paged-data-cs/_static/image6.png)

**Abbildung 4**: Verwenden der gespeicherten Prozedur getproductspagedandsortierte

Diese gespeicherte Prozedur gibt eine Reihe von Datensätzen als Ergebnisse zurück, sodass auf dem nächsten Bildschirm angezeigt wird, dass Tabellendaten zurückgegeben werden.

![Gibt an, dass die gespeicherte Prozedur Tabellendaten zurückgibt.](sorting-custom-paged-data-cs/_static/image7.png)

**Abbildung 5**: angeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt

Erstellen Sie schließlich Dal-Methoden, die sowohl das Fill a datdatababel-Steuer Muster als auch ein Datentabelle zurückgeben, indem Sie die Methoden `FillPagedAndSorted` und `GetProductsPagedAndSorted`benennen.

![Methodennamen auswählen](sorting-custom-paged-data-cs/_static/image8.png)

**Abbildung 6**: Auswählen der Methodennamen

Nun, da wir die DAL erweitert haben, sind wir bereit, auf den BLL zu schalten. Öffnen Sie die Datei `ProductsBLL`-Klasse, und fügen Sie eine neue Methode hinzu, `GetProductsPagedAndSorted`. Diese Methode muss drei Eingabeparameter `sortExpression`, `startRowIndex`und `maximumRows` akzeptieren und sollte einfach die Methode `GetProductsPagedAndSorted` der dal-Methode wie folgt aufzurufen:

[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Schritt 3: Konfigurieren von ObjectDataSource für die Übergabe in den SortExpression-Parameter

Wenn Sie die DAL und BLL so erweitert haben, dass Sie Methoden enthalten, die die gespeicherte Prozedur `GetProductsPagedAndSorted` verwenden, müssen Sie nur noch die ObjectDataSource auf der `SortParameter.aspx` Seite so konfigurieren, dass die neue BLL-Methode verwendet wird, und Sie können den `SortExpression`-Parameter basierend auf der Spalte übergeben, die der Benutzer zum Sortieren der Ergebnisse angefordert hat.

Ändern Sie zunächst die `SelectMethod` ObjectDataSource s von `GetProductsPaged` in `GetProductsPagedAndSorted`. Dies kann über den Assistenten zum Konfigurieren von Datenquellen, über das Eigenschaftenfenster oder direkt über die deklarative Syntax durchgeführt werden. Als nächstes müssen wir einen Wert für die ObjectDataSource s`SortParameterName`- [Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)angeben. Wenn diese Eigenschaft festgelegt ist, versucht ObjectDataSource, die GridView s `SortExpression`-Eigenschaft an die `SelectMethod`zu übergeben. Insbesondere sucht ObjectDataSource nach einem Eingabeparameter, dessen Name gleich dem Wert der `SortParameterName`-Eigenschaft ist. Da die BLL s-`GetProductsPagedAndSorted` Methode den Eingabeparameter des Sortier Ausdrucks namens `sortExpression`aufweist, legen Sie die Eigenschaft ObjectDataSource s `SortExpression` auf SortExpression fest.

Nachdem Sie diese beiden Änderungen vorgenommen haben, sollte die deklarative Syntax von ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Stellen Sie wie im vorherigen Tutorial sicher, dass ObjectDataSource die Eingabeparameter SortExpression, StartRowIndex oder MaximumRows *nicht* in der SelectParameters-Auflistung enthält.

Aktivieren Sie zum Aktivieren der Sortierung in der GridView einfach das Kontrollkästchen Sortierung aktivieren im GridView s-Smarttag, das die GridView s `AllowSorting`-Eigenschaft auf `true` festlegt und bewirkt, dass der Header Text für jede Spalte als LinkButton gerendert wird. Wenn der Endbenutzer auf eine der Header Link Schaltflächen klickt, wird ein Postback ausgeführt, und die folgenden Schritte werden erneut ausgeführt:

1. Die GridView aktualisiert die [`SortExpression`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) auf den Wert des `SortExpression` des Felds, auf das geklickt wurde.
2. Die ObjectDataSource Ruft die `GetProductsPagedAndSorted`-Methode der BLL s auf und übergibt die GridView s `SortExpression`-Eigenschaft als Wert für die `sortExpression` Eingabeparameter der Methode (zusammen mit den entsprechenden `startRowIndex`-und `maximumRows` Eingabeparameter Werten).
3. Die BLL Ruft die DAL s-`GetProductsPagedAndSorted` Methode auf.
4. Die DAL führt die gespeicherte Prozedur `GetProductsPagedAndSorted` aus und übergibt dabei den `@sortExpression` Parameter (zusammen mit den `@startRowIndex` und `@maximumRows` Eingabeparameter Werten).
5. Die gespeicherte Prozedur gibt die entsprechende Teilmenge der Daten an die BLL zurück, die Sie an die ObjectDataSource zurückgibt. Diese Daten werden dann an die GridView gebunden, in HTML gerendert und an den Endbenutzer gesendet.

Abbildung 7 zeigt die erste Seite der Ergebnisse, wenn Sie nach dem `UnitPrice` in aufsteigender Reihenfolge sortiert werden.

[![werden die Ergebnisse nach UnitPrice sortiert.](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Abbildung 7**: die Ergebnisse werden nach UnitPrice sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-custom-paged-data-cs/_static/image11.png))

Obwohl die aktuelle Implementierung die Ergebnisse nach Produktname, Kategoriename, Menge pro Einheit und Einheitspreis ordnungsgemäß sortieren kann, führt der Versuch, die Ergebnisse nach dem Lieferanten Namen zu sortieren, zu einer Lauf Zeit Ausnahme (siehe Abbildung 8).

![Der Versuch, die Ergebnisse nach dem Lieferanten zu sortieren, führt zu der folgenden Lauf Zeit Ausnahme.](sorting-custom-paged-data-cs/_static/image12.png)

**Abbildung 8**: der Versuch, die Ergebnisse nach dem Lieferanten zu sortieren, führt zu der folgenden Lauf Zeit Ausnahme.

Diese Ausnahme tritt auf, weil die `SortExpression` von GridView s `SupplierName` BoundField auf `SupplierName`festgelegt ist. Allerdings wird der Name des Lieferanten in der `Suppliers` Tabelle tatsächlich aufgerufen, `CompanyName` wir als Alias für diesen Spaltennamen `SupplierName`. Die von der `ROW_NUMBER()`-Funktion verwendete `OVER`-Klausel kann den Alias jedoch nicht verwenden und muss den tatsächlichen Spaltennamen verwenden. Ändern Sie daher die `SupplierName` BoundField s-`SortExpression` von suppliername in CompanyName (siehe Abbildung 9). Wie in Abbildung 10 gezeigt, können die Ergebnisse nach dieser Änderung nach dem Lieferanten sortiert werden.

!["Suppliername BoundField s SortExpression" in "CompanyName" ändern](sorting-custom-paged-data-cs/_static/image13.png)

**Abbildung 9**: Ändern des suppfield s "suppliername BoundField s SortExpression" in "CompanyName"

[![die Ergebnisse können jetzt nach Lieferant sortiert werden.](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Abbildung 10**: die Ergebnisse können jetzt nach Lieferant sortiert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-custom-paged-data-cs/_static/image16.png))

## <a name="summary"></a>Summary

Die benutzerdefinierte Paginierungs Implementierung, die wir im vorherigen Tutorial überprüft haben, erforderte, dass die Reihenfolge, nach der die Ergebnisse sortiert werden sollen, zur Entwurfszeit angegeben wird. Kurz gesagt, dies bedeutete, dass die Implementierung der benutzerdefinierten Paginierung, die wir implementiert haben, nicht gleichzeitig Sortierfunktionen bereitstellen konnte. In diesem Tutorial haben wir diese Einschränkung überwunden, indem wir die gespeicherte Prozedur von der ersten erweitern, um einen `@sortExpression` Eingabeparameter einzuschließen, mit dem die Ergebnisse sortiert werden könnten.

Nachdem Sie diese gespeicherte Prozedur erstellt und neue Methoden in der dal und der BLL erstellt haben, konnten wir eine GridView implementieren, die sowohl Sortier-als auch benutzerdefinierte Paginierung bot, indem wir die ObjectDataSource so konfigurieren, dass Sie die Eigenschaft "GridView s Current `SortExpression`" an die BLL-`SelectMethod`übergibt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Carlos Santos. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Weiter](creating-a-customized-sorting-user-interface-cs.md)
