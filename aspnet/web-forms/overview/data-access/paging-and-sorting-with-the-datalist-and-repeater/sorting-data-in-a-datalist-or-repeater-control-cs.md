---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Sortieren von Daten in einem DataList-oder RepeaterC#-Steuerelement () | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie Sortierungs Unterstützung in DataList und Repeater einschließen und wie Sie einen DataList-oder Repeater-Wert erstellen, dessen Daten...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 66a09c637d33c812b39e0ce85a552bd71665a2e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78502953"
---
# <a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Sortieren von Berichtsdaten in einem DataList- oder Wiederholungssteuerelement (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) oder [PDF herunterladen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> In diesem Tutorial wird erläutert, wie Sie Sortierungs Unterstützung in DataList und Repeater einschließen und wie Sie einen DataList-oder Repeater-Wert erstellen, dessen Daten per Pager und Sortierung sortiert werden können.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](paging-report-data-in-a-datalist-or-repeater-control-cs.md) wurde untersucht, wie einem DataList Paging-Unterstützung hinzugefügt wird. Wir haben eine neue Methode in der `ProductsBLL`-Klasse (`GetProductsAsPagedDataSource`) erstellt, die ein `PagedDataSource`-Objekt zurückgegeben hat. Wenn Sie an einen DataList-oder Repeater gebunden ist, zeigt der DataList oder der Repeater nur die angeforderte Datenseite an. Diese Methode ähnelt der Vorgehensweise, die intern von den Steuerelementen GridView, DetailsView und FormView verwendet wird, um Ihre integrierte Standard-Paging-Funktionalität bereitzustellen.

Zusätzlich zur Unterstützung der Paginierung bietet die GridView auch die Standard Sortier Unterstützung. Weder der DataList noch der Repeater bietet integrierte Sortierfunktionen. Sortierungs Features können jedoch mit etwas Code hinzugefügt werden. In diesem Tutorial wird erläutert, wie Sie Sortierungs Unterstützung in DataList und Repeater einschließen und wie Sie einen DataList-oder Repeater-Wert erstellen, dessen Daten per Pager und Sortierung sortiert werden können.

## <a name="a-review-of-sorting"></a>Eine Überprüfung der Sortierung

Wie wir im Tutorial zum [Paging und Sortieren von Berichtsdaten](../paging-and-sorting/paging-and-sorting-report-data-cs.md) gesehen haben, bietet das GridView-Steuerelement standardmäßig eine Sortier Unterstützung. Jedes GridView-Feld kann über ein zugeordnetes `SortExpression`verfügen, das das Datenfeld angibt, nach dem die Daten sortiert werden. Wenn die GridView s `AllowSorting`-Eigenschaft auf `true`festgelegt ist, wird jedes GridView-Feld mit einem `SortExpression`-Eigenschafts Wert als LinkButton gerendert. Wenn ein Benutzer auf einen bestimmten GridView Field s-Header klickt, wird ein Postback ausgeführt, und die Daten werden entsprechend den angeklickten Feld-s-`SortExpression`sortiert.

Das GridView-Steuerelement verfügt auch über eine `SortExpression`-Eigenschaft, die die `SortExpression` des GridView-Felds speichert, nach dem die Daten sortiert werden. Außerdem gibt eine `SortDirection`-Eigenschaft an, ob die Daten in aufsteigender oder absteigender Reihenfolge sortiert werden sollen (wenn ein Benutzer zweimal nacheinander auf einen bestimmten GridView Field s-Header Link klickt, wird die Sortierreihenfolge geändert).

Wenn das GridView-Steuerelement an das Datenquellen-Steuerelement gebunden ist, übergibt es seine `SortExpression`-und `SortDirection` Eigenschaften an das Datenquellen-Steuerelement. Das Datenquellen-Steuerelement ruft die Daten ab und sortiert Sie dann entsprechend der angegebenen `SortExpression` und `SortDirection` Eigenschaften. Nachdem die Daten sortiert wurden, wird Sie vom Datenquellen-Steuerelement an die GridView-Sicht zurückgegeben.

Um diese Funktionalität mit den DataList-oder Repeater-Steuerelementen zu replizieren, müssen folgende Schritte durch

- Erstellen einer Sortierungs Schnittstelle
- Speichern Sie das Datenfeld, nach dem sortiert werden soll, und ob in aufsteigender oder absteigender Reihenfolge sortiert
- Anweisen von ObjectDataSource zum Sortieren der Daten nach einem bestimmten Datenfeld

Wir behandeln diese drei Aufgaben in den Schritten 3 und 4. Im folgenden wird erläutert, wie Sie Paging-und Sortierungs Unterstützung in einen DataList-oder Repeater einschließen.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Schritt 2: Anzeigen der Produkte in einem Wiederholungs Modul

Bevor wir uns mit der Implementierung von Sortierungs bezogenen Funktionen beschäftigen, sollten Sie zunächst die Produkte in einem Wiederholungs Steuerelement auflisten. Öffnen Sie zunächst die Seite `Sorting.aspx` im Ordner `PagingSortingDataListRepeater`. Fügen Sie der Webseite ein Repeater-Steuerelement hinzu, und legen Sie dessen `ID`-Eigenschaft auf `SortableProducts`fest. Erstellen Sie im smarttagwiederholungs-Tag eine neue ObjectDataSource mit dem Namen `ProductsDataSource`, und konfigurieren Sie Sie so, dass Daten aus der `ProductsBLL` Class s `GetProducts()`-Methode abgerufen werden. Wählen Sie auf den Registerkarten einfügen, aktualisieren und löschen in den Dropdown Listen die Option (keine) aus.

[![erstellen Sie eine ObjectDataSource, und konfigurieren Sie Sie für die Verwendung der getproductaspgeddatasource ()-Methode.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Abbildung 1**: Erstellen einer ObjectDataSource und Konfigurieren der Methode für die Verwendung der `GetProductsAsPagedDataSource()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Abbildung 2**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))

Anders als beim DataList erstellt Visual Studio nicht automatisch eine `ItemTemplate` für das Repeater-Steuerelement, nachdem es an eine Datenquelle gebunden wurde. Außerdem müssen wir diese `ItemTemplate` deklarativ hinzufügen, da das Smarttags des Repeater-Steuer Elements nicht über die Option Vorlagen bearbeiten in den DataList s verfügt. Verwenden Sie den gleichen `ItemTemplate` aus dem vorherigen Tutorial, in dem der Name, Lieferant und die Kategorie des Produkts angezeigt werden.

Nachdem Sie die `ItemTemplate`hinzugefügt haben, sollten das deklarative Markup für Repeater und ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

In Abbildung 3 wird diese Seite angezeigt, wenn Sie in einem Browser angezeigt wird.

[![werden die Namen, der Lieferant und die Kategorie der einzelnen Produkte angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Abbildung 3**: jeder Produkt Name, Lieferant und Kategorie wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))

## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Schritt 3: anweisen von ObjectDataSource zum Sortieren der Daten

Um die im Wiederholungs Modul angezeigten Daten zu sortieren, muss die ObjectDataSource des Sortier Ausdrucks, mit dem die Daten sortiert werden sollen, informiert werden. Bevor die ObjectDataSource Ihre Daten abruft, wird zuerst das [`Selecting`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx)ausgelöst, das uns die Möglichkeit bietet, einen Sortierungs Ausdruck anzugeben. Dem `Selecting`-Ereignishandler wird ein Objekt vom Typ " [`ObjectDataSourceSelectingEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx)" übermittelt, das eine Eigenschaft mit dem Namen [`Arguments`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) vom Typ [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)aufweist. Die `DataSourceSelectArguments`-Klasse ist so konzipiert, dass Sie datenbezogene Anforderungen von einem Consumer von Daten an das Datenquellen-Steuerelement übergibt und eine [`SortExpression`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx)enthält.

Wenn Sie Sortier Informationen von der ASP.NET-Seite an ObjectDataSource übergeben möchten, erstellen Sie einen Ereignishandler für das `Selecting`-Ereignis, und verwenden Sie den folgenden Code:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

Dem *SortExpression* -Wert sollte der Name des Daten Felds zugewiesen werden, nach dem die Daten sortiert werden sollen (z. b. ProductName). Es ist keine Eigenschaft für die Sortierrichtung vorhanden. Wenn Sie also die Daten in absteigender Reihenfolge sortieren möchten, fügen Sie die Zeichenfolge "ensc" an den *SortExpression* -Wert an (z. b. ProductName-Debug).

Probieren Sie einige unterschiedliche hart codierte Werte für *SortExpression* aus, und testen Sie die Ergebnisse in einem Browser. Wie in Abbildung 4 gezeigt, werden die Produkte bei Verwendung von ProductName Debug als *SortExpression*nach Ihrem Namen in umgekehrter alphabetischer Reihenfolge sortiert.

[![werden die Produkte nach Ihrem Namen in umgekehrter alphabetischer Reihenfolge sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Abbildung 4**: die Produkte werden nach Ihrem Namen in umgekehrter alphabetischer Reihenfolge sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))

## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Schritt 4: Erstellen der Sortierungs Schnittstelle und Speichern des Sortier Ausdrucks und der Richtung

Durch das Aktivieren der Sortier Unterstützung in der GridView wird jeder Header Text der sortierbaren Felder in einen LinkButton konvertiert, bei dem die Daten nach dem Klicken entsprechend sortiert werden. Eine solche Sortier Schnittstelle eignet sich für die GridView, bei der Ihre Daten in Spalten sauber angeordnet werden. Für die Steuerelemente DataList und Repeater wird jedoch eine andere Sortier Schnittstelle benötigt. Eine allgemeine Sortier Schnittstelle für eine Liste von Daten (im Gegensatz zu einem Datenraster) ist eine Dropdown Liste, die die Felder bereitstellt, nach denen die Daten sortiert werden können. Wir implementieren eine solche Schnittstelle für dieses Tutorial.

Fügen Sie ein Dropdown List-websteuer Element oberhalb der `SortableProducts` Repeater hinzu, und legen Sie dessen `ID`-Eigenschaft auf `SortBy`fest. Klicken Sie im Eigenschaftenfenster auf die Auslassungs Punkte in der `Items`-Eigenschaft, um den ListItem-Auflistungs-Editor anzuzeigen. Fügen Sie `ListItem` s hinzu, um die Daten nach den Feldern `ProductName`, `CategoryName`und `SupplierName` zu sortieren. Fügen Sie außerdem eine `ListItem` hinzu, um die Produkte nach Ihrem Namen in umgekehrter alphabetischer Reihenfolge zu sortieren.

Die `ListItem` `Text` Eigenschaften können auf einen beliebigen Wert (z. b. Name) festgelegt werden, die `Value` Eigenschaften müssen jedoch auf den Namen des Daten Felds (z. b. ProductName) festgelegt werden. Um die Ergebnisse in absteigender Reihenfolge zu sortieren, fügen Sie die Zeichenfolge DESC an den Daten Feldnamen an, z. b. ProductName DESC.

![Fügen Sie für jedes sortierbare Daten Feld ein ListItem-Element hinzu.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Abbildung 5**: Hinzufügen eines `ListItem` für die einzelnen sortierbaren Datenfelder

Fügen Sie schließlich rechts neben der Dropdown Liste ein Schaltflächen-websteuer Element hinzu. Legen Sie den `ID` auf `RefreshRepeater` und dessen `Text` Eigenschaft auf Aktualisieren fest.

Nachdem Sie die `ListItem` s erstellt und die Schaltfläche Aktualisieren hinzugefügt haben, sollten die deklarative Syntax DropDownList und Button s in etwa wie folgt aussehen:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Nachdem die DropDownList-Sortierung fertiggestellt wurde, müssen wir den ObjectDataSource s-`Selecting` Ereignishandler aktualisieren, sodass die ausgewählte `SortBy``ListItem` s-Eigenschaft `Value` anstelle eines hart codierten Sortier Ausdrucks verwendet wird.

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

An diesem Punkt werden die Produkte zunächst nach dem `ProductName` Datenfeld sortiert, da die `SortBy` `ListItem` standardmäßig ausgewählt ist (siehe Abbildung 6). Wenn Sie eine andere Sortieroption auswählen, z. b. Kategorie, und auf Aktualisieren klicken, wird ein Postback ausgelöst und die Daten nach dem Kategorienamen erneut sortiert, wie in Abbildung 7 dargestellt.

[![werden die Produkte anfänglich nach Ihrem Namen sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Abbildung 6**: die Produkte werden anfänglich nach Ihrem Namen sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))

[![die Produkte nach Kategorie sortiert](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Abbildung 7**: die Produkte sind jetzt nach Kategorie sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))

> [!NOTE]
> Wenn Sie auf die Schaltfläche "Aktualisieren" klicken, werden die Daten automatisch neu sortiert, da der Ansichts Zustand der Wiederholungsart deaktiviert wurde, sodass der Repeater bei jedem Postback eine erneute Bindung an seine Datenquelle bewirkt hat. Wenn Sie den Ansichts Zustand der Wiederholungs-s aktiviert haben, hat das Ändern der Dropdown Liste Sortierung keine Auswirkung auf die Sortierreihenfolge. Um dieses Problem zu beheben, erstellen Sie einen Ereignishandler für die Schaltfläche "Aktualisieren" `Click` Ereignis, und binden Sie den Repeater erneut an die zugehörige Datenquelle (durch Aufrufen der Repeater s `DataBind()`-Methode).

## <a name="remembering-the-sort-expression-and-direction"></a>Speichern des Sortier Ausdrucks und der Richtung

Beim Erstellen eines sortierbaren DataList-oder Wiederholungs Moduls auf einer Seite, auf der nicht sortierende Postbacks auftreten können, ist es zwingend erforderlich, dass der Sortier Ausdruck und die Richtung über Postbacks hinweg gespeichert werden. Stellen Sie sich beispielsweise vor, dass wir den Repeater in diesem Tutorial aktualisiert haben, um die Schaltfläche "Löschen" für jedes Produkt einzuschließen Wenn der Benutzer auf die Schaltfläche "Löschen" klickt, führen wir Code aus, um das ausgewählte Produkt zu löschen, und binden dann die Daten erneut an den Wiederholungs-. Wenn die Sortier Details nicht über Postbacks hinweg beibehalten werden, werden die auf dem Bildschirm angezeigten Daten auf die ursprüngliche Sortierreihenfolge zurückgesetzt.

In diesem Tutorial speichert DropDownList den Sortier Ausdruck und die Richtung implizit im Ansichts Zustand für uns. Wenn wir eine andere Sortier Schnittstelle verwendet haben, z. b. LinkButtons, die die verschiedenen Sortieroptionen bereitstellten, die wir für die Sortierreihenfolge in den Postbacks benötigen. Dies kann erreicht werden, indem die Sortierparameter im Ansichts Zustand der Seite gespeichert werden, indem der Sort-Parameter in QueryString oder eine andere Zustands persistenztechnik eingeschlossen wird.

In den zukünftigen Beispielen in diesem Tutorial wird erläutert, wie die Sortierungs Details im Ansichts Zustand der Seite persistent gespeichert werden.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Schritt 5: Hinzufügen von Sortierungs Unterstützung zu einem DataList, der Standard-Paging verwendet

Im [vorherigen Tutorial](paging-report-data-in-a-datalist-or-repeater-control-cs.md) wurde erläutert, wie standardpaginierung mit einem DataList implementiert wird. Lassen Sie das vorherige Beispiel erweitern, um die Möglichkeit zum Sortieren der auslagerenen Daten einzubeziehen. Öffnen Sie zunächst die Seiten `SortingWithDefaultPaging.aspx` und `Paging.aspx` im Ordner `PagingSortingDataListRepeater`. Klicken Sie auf der Seite `Paging.aspx` auf die Schaltfläche Quelle, um das deklarative Markup der Seite anzuzeigen. Kopieren Sie den markierten Text (siehe Abbildung 8), und fügen Sie ihn in das deklarative Markup der `SortingWithDefaultPaging.aspx` zwischen den `<asp:Content>`-Tags ein.

[![Replizieren des deklarativen Markups in den &lt;ASP: Content&gt; Tags von "Paging. aspx" in "sortingwithdefaultpaging. aspx".](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Abbildung 8**: Replizieren des deklarativen Markups in den `<asp:Content>` Tags von `Paging.aspx` auf `SortingWithDefaultPaging.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))

Nach dem Kopieren des deklarativen Markups kopieren Sie die Methoden und Eigenschaften in der Code Behind-Klasse der `Paging.aspx` Page s in die Code Behind-Klasse für `SortingWithDefaultPaging.aspx`. Nehmen Sie als nächstes einen Moment Zeit, um die `SortingWithDefaultPaging.aspx` Seite in einem Browser anzuzeigen. Es sollte die gleiche Funktionalität und Darstellung wie `Paging.aspx`aufweisen.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Erweitern von productbll um eine standardmäßige Paging-und Sortierungs Methode

Im vorherigen Tutorial haben wir eine `GetProductsAsPagedDataSource(pageIndex, pageSize)` Methode in der `ProductsBLL`-Klasse erstellt, die ein `PagedDataSource`-Objekt zurückgegeben hat. Dieses `PagedDataSource` Objekt wurde mit *allen* Produkten aufgefüllt (über die BLL s `GetProducts()`-Methode), aber wenn es an den DataList gebunden ist, wurden nur die Datensätze angezeigt, die den angegebenen Eingabe Parametern *pageIndex* und *PageSize* entsprechen.

An früherer Stelle in diesem Tutorial haben wir die Sortierungs Unterstützung durch Angabe des Sortier Ausdrucks aus dem ObjectDataSource s `Selecting`-Ereignishandler hinzugefügt. Dies funktioniert gut, wenn ObjectDataSource ein Objekt zurückgibt, das sortiert werden kann, wie z. b. die `ProductsDataTable`, die von der `GetProducts()`-Methode zurückgegeben wird. Das `PagedDataSource` Objekt, das von der `GetProductsAsPagedDataSource`-Methode zurückgegeben wird, unterstützt das Sortieren der inneren Datenquelle jedoch nicht. Stattdessen müssen die Ergebnisse, die von der `GetProducts()`-Methode zurückgegeben werden, sortiert werden, *bevor* Sie in der `PagedDataSource`abgelegt werden.

Um dies zu erreichen, erstellen Sie eine neue Methode in der `ProductsBLL`-Klasse, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Um die `ProductsDataTable` zu sortieren, die von der `GetProducts()`-Methode zurückgegeben wird, geben Sie die `Sort`-Eigenschaft ihrer Standard `DataTableView`an:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Die `GetProductsSortedAsPagedDataSource`-Methode unterscheidet sich nur geringfügig von der `GetProductsAsPagedDataSource` Methode, die im vorherigen Tutorial erstellt wurde. Insbesondere akzeptiert `GetProductsSortedAsPagedDataSource` einen zusätzlichen Eingabeparameter `sortExpression` und weist diesen Wert der `Sort`-Eigenschaft der `ProductDataTable` s `DefaultView`zu. Ein paar Codezeilen später wird der Datenquelle des `PagedDataSource` Objekts die `ProductDataTable` s `DefaultView`zugewiesen.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Aufrufen der getproductssortedaspgeddatasource-Methode und angeben des Werts für den SortExpression-Eingabe Parameter

Wenn die `GetProductsSortedAsPagedDataSource`-Methode fertiggestellt ist, besteht der nächste Schritt darin, den Wert für diesen Parameter anzugeben. Die ObjectDataSource in `SortingWithDefaultPaging.aspx` ist zurzeit so konfiguriert, dass Sie die `GetProductsAsPagedDataSource`-Methode aufruft und die beiden Eingabeparameter über die beiden `QueryStringParameters`, die in der `SelectParameters` Auflistung angegeben sind, übergibt. Diese beiden `QueryStringParameters` anzeigen, dass die Quelle für die Parameter " *pageIndex* " und " *PageSize* " der `GetProductsAsPagedDataSource`-Methode aus den QueryString-Feldern `pageIndex` und `pageSize`stammt.

Aktualisieren Sie die ObjectDataSource s-`SelectMethod` Eigenschaft, sodass Sie die neue `GetProductsSortedAsPagedDataSource`-Methode aufruft. Fügen Sie dann eine neue `QueryStringParameter` hinzu, sodass der Zugriff auf den *SortExpression* -Eingabeparameter aus dem QueryString-Feld `sortExpression`erfolgt. Legen Sie die `QueryStringParameter` s `DefaultValue` auf ProductName fest.

Nach diesen Änderungen sollte das deklarative Markup von ObjectDataSource s wie folgt aussehen:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

An diesem Punkt werden die Ergebnisse auf der Seite "`SortingWithDefaultPaging.aspx`" alphabetisch nach dem Produktnamen sortiert (siehe Abbildung 9). Der Grund hierfür ist, dass standardmäßig der Wert "ProductName" als Parameter für die `GetProductsSortedAsPagedDataSource`-Methode " *SortExpression* " übergeben wird.

[![standardmäßig werden die Ergebnisse nach ProductName sortiert.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Abbildung 9**: Standardmäßig werden die Ergebnisse nach `ProductName` sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))

Wenn Sie manuell ein `sortExpression` QueryString-Feld hinzufügen, z. b. `SortingWithDefaultPaging.aspx?sortExpression=CategoryName`, werden die Ergebnisse nach der angegebenen `sortExpression`sortiert. Dieser `sortExpression` Parameter ist jedoch beim Verschieben auf eine andere Datenseite nicht in der Abfrage Zeichenfolge enthalten. Wenn Sie auf die Schaltflächen "Next" oder "Last Page" klicken, werden Sie zum `Paging.aspx`zurückkehren! Außerdem gibt es zurzeit keine Sortierungs Schnittstelle. Die einzige Möglichkeit, wie Benutzer die Sortierreihenfolge der auslagerenen Daten ändern können, besteht darin, die QueryString direkt zu bearbeiten.

## <a name="creating-the-sorting-interface"></a>Erstellen der Sortier Schnittstelle

Zuerst müssen wir die `RedirectUser`-Methode aktualisieren, um den Benutzer an `SortingWithDefaultPaging.aspx` (anstelle von `Paging.aspx`) zu senden und den `sortExpression` Wert in der Abfrage Zeichenfolge einzuschließen. Außerdem sollten Sie eine schreibgeschützte, auf Seitenebene benannte `SortExpression`-Eigenschaft hinzufügen. Diese Eigenschaft, ähnlich wie die Eigenschaften `PageIndex` und `PageSize`, die im vorherigen Tutorial erstellt wurden, gibt den Wert des Felds `sortExpression` QueryString zurück, sofern vorhanden, und andernfalls den Standardwert (ProductName).

Die `RedirectUser`-Methode akzeptiert derzeit nur einen einzelnen Eingabeparameter, der den Index der anzuzeigenden Seite anzeigt. Es kann jedoch vorkommen, dass der Benutzer mit einem anderen Sortier Ausdruck, der in der Abfrage Zeichenfolge angegeben ist, zu einer bestimmten Datenseite umgeleitet werden soll. In Kürze erstellen wir die Sortierungs Schnittstelle für diese Seite, die eine Reihe von Schaltflächen-websteuer Elementen zum Sortieren der Daten nach einer bestimmten Spalte enthält. Wenn auf eine dieser Schaltflächen geklickt wird, möchten wir den Benutzer umleiten, indem er den entsprechenden Sortierungs Ausdruckswert übergibt. Um diese Funktionalität bereitzustellen, erstellen Sie zwei Versionen der `RedirectUser`-Methode. Der erste darf nur den anzuzeigenden Seitenindex akzeptieren, während der zweite den Seitenindex und den Sortier Ausdruck akzeptiert.

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

Im ersten Beispiel in diesem Tutorial haben wir mithilfe einer Dropdown List eine Sortier Schnittstelle erstellt. In diesem Beispiel verwenden Sie drei Schaltflächen-websteuer Elemente, die oberhalb des DataList-Steuer Elements für die Sortierung nach `ProductName`positioniert sind, eines für `CategoryName`und eines für `SupplierName`. Fügen Sie die drei Schaltflächen-websteuer Elemente hinzu, indem Sie Ihre `ID` und `Text` Eigenschaften entsprechend festlegen:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Erstellen Sie als nächstes einen `Click` Ereignishandler für jeden. Die Ereignishandler sollten die `RedirectUser`-Methode abrufen und den Benutzer mithilfe des entsprechenden Sortier Ausdrucks an die erste Seite zurückgeben.

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Beim ersten Besuch der Seite werden die Daten nach dem Produktnamen alphabetisch sortiert (siehe Abbildung 9). Klicken Sie auf die Schaltfläche Weiter, um zur zweiten Seite der Daten zu gelangen, und klicken Sie dann auf die Schaltfläche nach Kategorie sortieren. Dadurch wird die erste Seite der Daten nach Kategorien Amen sortiert zurückgegeben (siehe Abbildung 10). Wenn Sie auf die Schaltfläche nach Lieferant sortieren klicken, werden die Daten nach Lieferant sortiert, beginnend mit der ersten Seite der Daten. Die Sortier Auswahl wird beim Durchlaufen der Daten gespeichert. In Abbildung 11 wird die Seite nach der Sortierung nach Kategorie und dann auf die dreizehnte Seite der Daten gezeigt.

[![die Produkte nach Kategorie sortiert](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Abbildung 10**: die Produkte sind nach Kategorie sortiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))

[![der Sortier Ausdruck beim Paging durch die Daten gespeichert wird.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Abbildung 11**: der Sortier Ausdruck wird beim Paging durch die Daten gespeichert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))

## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Schritt 6: benutzerdefiniertes Paging durch Datensätze in einem Repeater

Das DataList-Beispiel, das in Schritt 5 untersucht wird, unter Verwendung des ineffizienten Standard pagingverfahrens. Beim Paging durch ausreichend große Datenmengen ist es zwingend erforderlich, dass benutzerdefinierte Paginierung verwendet werden. Zurück zum [effizienten Paging durch große Datenmengen](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) und zum Sortieren von Tutorials für [benutzerdefinierte](../paging-and-sorting/sorting-custom-paged-data-cs.md) Auslagerungs Daten untersuchen wir die Unterschiede zwischen der standardmäßigen und benutzerdefinierten Auslagerung und den erstellten Methoden in der BLL für die Verwendung von benutzerdefiniertem Paging und Sortieren von benutzerdefinierten Auslagerungs Daten. Insbesondere in diesen beiden vorherigen Tutorials haben wir die folgenden drei Methoden zur `ProductsBLL`-Klasse hinzugefügt:

- `GetProductsPaged(startRowIndex, maximumRows)` gibt eine bestimmte Teilmenge von Datensätzen zurück, beginnend bei *startRowIndex* und nicht überschreitet *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` gibt eine bestimmte Teilmenge von Datensätzen zurück, sortiert nach dem angegebenen *SortExpression* -Eingabeparameter.
- `TotalNumberOfProducts()` gibt die Gesamtanzahl der Datensätze in der `Products` Datenbanktabelle an.

Diese Methoden können verwendet werden, um Daten mithilfe eines DataList-oder Repeater-Steuer Elements effizient zu sortieren und zu sortieren. Um dies zu veranschaulichen, beginnen Sie mit dem Erstellen eines Repeater-Steuer Elements mit Unterstützung für benutzerdefinierte Paginierung. Anschließend fügen wir Sortierungs Funktionen hinzu.

Öffnen Sie die Seite `SortingWithCustomPaging.aspx` im Ordner `PagingSortingDataListRepeater`, und fügen Sie der Seite einen Wiederholungs Modul hinzu. Legen Sie dessen Eigenschaft `ID` auf `Products`fest. Erstellen Sie im smarttagwiederholungs-Tag eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren Sie Sie so, dass die Daten aus der `ProductsBLL` Klasse `GetProductsPaged` Methode ausgewählt werden.

[![konfigurieren Sie ObjectDataSource für die Verwendung der getproductspaged-Methode der productbll-Klasse.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Abbildung 12**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL` Class s `GetProductsPaged`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))

Legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest, und klicken Sie dann auf die Schaltfläche Weiter. Der Assistent zum Konfigurieren von Datenquellen fordert nun zur Eingabe der Quellen der Eingabeparameter " *startRowIndex* " und " *maximumRows* " der `GetProductsPaged` Methode auf. In Wirklichkeit werden diese Eingabeparameter ignoriert. Stattdessen werden die Werte *startRowIndex* und *maximumRows* durch die `Arguments`-Eigenschaft im ObjectDataSource s `Selecting`-Ereignishandler übergeben. Dies entspricht der Angabe von *SortExpression* in diesem Tutorial s First Demo. Lassen Sie daher die Dropdown Listen für Parameter Quelle im Assistenten auf keine festgelegt.

[![lassen Sie die Parameter Quellen auf None fest.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Abbildung 13**: belassen Sie die Parameter Quellen auf None ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))

> [!NOTE]
> Legen Sie die Eigenschaft ObjectDataSource s `EnablePaging` *nicht* auf `true`fest. Dies bewirkt, dass ObjectDataSource automatisch seine eigenen Parameter *startRowIndex* und *maximumRows* in die Liste der vorhandenen Parameter von `SelectMethod` s einschließt. Die `EnablePaging`-Eigenschaft ist nützlich, wenn benutzerdefinierte auslagerbare Daten an ein GridView-, DetailsView-oder FormView-Steuerelement gebunden werden, da diese Steuerelemente ein bestimmtes Verhalten von ObjectDataSource erwarten, das nur verfügbar ist, wenn `EnablePaging` Eigenschaft `true`ist. Da wir die Paging-Unterstützung für DataList und Repeater manuell hinzufügen müssen, lassen Sie diese Eigenschaft auf `false` (Standardeinstellung) fest, da wir die erforderliche Funktionalität direkt auf unserer ASP.NET-Seite speichern.

Definieren Sie abschließend die Repeater-`ItemTemplate`, sodass der Name, die Kategorie und der Lieferant des Produkts angezeigt werden. Nach diesen Änderungen sollte die deklarative Syntax von Repeater und ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Nehmen Sie sich einen Moment Zeit, um die Seite über einen Browser zu besuchen. Der Grund hierfür ist, dass wir noch die Parameterwerte *startRowIndex* und *maximumRows* angeben müssen. Daher werden die Werte 0 für beide übermittelt. Um diese Werte anzugeben, erstellen Sie einen Ereignishandler für das Ereignis ObjectDataSource s `Selecting`, und legen Sie diese Parameterwerte Programm gesteuert auf hart codierte Werte von 0 bzw. 5 fest:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Durch diese Änderung zeigt die Seite, wenn Sie in einem Browser angezeigt wird, die ersten fünf Produkte.

[![werden die ersten fünf Datensätze angezeigt.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Abbildung 14**: die ersten fünf Datensätze werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))

> [!NOTE]
> Die in Abbildung 14 aufgeführten Produkte werden nach dem Produktnamen sortiert, da die gespeicherte Prozedur `GetProductsPaged`, die die effiziente benutzerdefinierte Pagingabfrage ausführt, die Ergebnisse nach `ProductName`sortiert.

Damit der Benutzer die Seiten schrittweise durchlaufen kann, müssen wir den Start Zeilen Index und die maximalen Zeilen nachverfolgen und diese Werte über Postbacks hinweg speichern. Im standardmäßigen Paging-Beispiel haben wir QueryString-Felder verwendet, um diese Werte beizubehalten. Speichern Sie diese Informationen für diese Demo im Ansichts Zustand der Seite. Erstellen Sie die folgenden beiden Eigenschaften:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Aktualisieren Sie anschließend den Code im Auswahl Ereignishandler, sodass er die Eigenschaften `StartRowIndex` und `MaximumRows` anstelle der hart codierten Werte 0 und 5 verwendet:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

An diesem Punkt zeigt unsere Seite weiterhin nur die ersten fünf Datensätze an. Wenn diese Eigenschaften jedoch vorhanden sind, können wir unsere pagingschnittstelle neu erstellen.

## <a name="adding-the-paging-interface"></a>Hinzufügen der Paging-Schnittstelle

Verwenden Sie die gleiche erste, vorherige, nächste und letzte Paging-Schnittstelle, die im standardmäßigen Paging-Beispiel verwendet wird, einschließlich des Beschriftungs-websteuer Elements, das anzeigt, welche Seite der Daten angezeigt wird und wie viele Seiten insgesamt vorhanden sind. Fügen Sie unter dem Repeater die vier Schaltflächen-websteuer Elemente und die Bezeichnung hinzu.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Erstellen Sie als nächstes `Click` Ereignishandler für die vier Schaltflächen. Wenn auf eine dieser Schaltflächen geklickt wird, müssen wir die `StartRowIndex` aktualisieren und die Daten erneut an den Repeater binden. Der Code für die Schaltflächen "First", "Previous" und "Next" ist einfach genug, aber für die letzte Schaltfläche wie bestimmen wir den Start Zeilen Index für die letzte Seite der Daten? Um diesen Index zu berechnen und zu bestimmen, ob die nächste und die letzte Schaltfläche aktiviert werden sollen, müssen wir wissen, wie viele Datensätze insgesamt ausgelagert werden. Dies können Sie durch Aufrufen der `ProductsBLL` Class s `TotalNumberOfProducts()`-Methode ermitteln. Erstellen Sie eine schreibgeschützte Eigenschaft auf Seitenebene mit dem Namen `TotalRowCount`, die die Ergebnisse der `TotalNumberOfProducts()` Methode zurückgibt:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Mit dieser Eigenschaft können wir nun den Start Zeilen Index der letzten Seite festlegen. Dabei handelt es sich um das ganzzahlige Ergebnis des `TotalRowCount` minus 1 dividiert durch `MaximumRows`, multipliziert mit `MaximumRows`. Wir können nun die `Click` Ereignishandler für die vier Schaltflächen für die Auslagerungs Schnittstelle schreiben:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Zum Schluss müssen die Schaltflächen "erste" und "zurück" in der pagingschnittstelle deaktiviert werden, wenn die erste Seite der Daten angezeigt wird, und die Schaltflächen "weiter" und "letzte" beim Anzeigen der letzten Seite Fügen Sie hierzu den folgenden Code zum ObjectDataSource s `Selecting`-Ereignishandler hinzu:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Nachdem Sie diese `Click` Ereignishandler und den Code hinzugefügt haben, um die Paging-Schnittstellen Elemente basierend auf dem aktuellen Start Zeilen Index zu aktivieren oder zu deaktivieren, testen Sie die Seite in einem Browser. Wie in Abbildung 15 dargestellt, werden beim ersten Besuch der Seite die Schaltflächen "erste" und "zurück" deaktiviert. Wenn Sie auf Weiter klicken, wird die zweite Seite der Daten angezeigt. beim Klicken auf letzte wird die letzte Seite angezeigt (siehe Abbildung 16 und 17). Beim Anzeigen der letzten Seite der Daten sind die Schaltflächen "weiter" und "letzte" deaktiviert.

[![die Schaltflächen "Previous" und "Last" deaktiviert sind, wenn Sie die erste Seite der Produkte anzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Abbildung 15**: die Schaltflächen "Previous" und "Last" sind beim Anzeigen der ersten Produktseite deaktiviert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))

[![die zweite Seite der Produkte angezeigt wird.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Abbildung 16**: die zweite Seite der Produkte wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))

[![klicken auf letzte zeigt die letzte Seite der Daten an](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Abbildung 17**: Klicken auf "letzte" zeigt die letzte Seite mit Daten an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))

## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Schritt 7: einschließen der Sortier Unterstützung mit dem benutzerdefinierten auslagerungssrepeater

Nachdem das benutzerdefinierte Paging implementiert wurde, können wir nun die Sortier Unterstützung einschließen. Die `ProductsBLL` Class s `GetProductsPagedAndSorted`-Methode hat die gleichen *startRowIndex* -und *maximumRows* -Eingabeparameter wie `GetProductsPaged`, erlaubt aber einen zusätzlichen *SortExpression* -Eingabeparameter. Um die `GetProductsPagedAndSorted`-Methode aus `SortingWithCustomPaging.aspx`verwenden zu können, müssen die folgenden Schritte ausgeführt werden:

1. Ändern Sie die Eigenschaft ObjectDataSource s `SelectMethod` von `GetProductsPaged` in `GetProductsPagedAndSorted`.
2. Fügen Sie der ObjectDataSource s `SelectParameters` Auflistung ein *SortExpression* -`Parameter` Objekt hinzu.
3. Erstellen Sie eine private auf Seitenebene `SortExpression` Eigenschaft, die ihren Wert über Postbacks über den Ansichts Zustand der Seite beibehält.
4. Aktualisieren Sie den ObjectDataSource s `Selecting`-Ereignishandler, um den Parameter ObjectDataSource s *SortExpression* dem Wert der `SortExpression` Eigenschaft auf Seitenebene zuzuweisen.
5. Erstellen Sie die Sortierungs Schnittstelle.

Aktualisieren Sie zunächst die ObjectDataSource s `SelectMethod`-Eigenschaft, und fügen Sie einen *SortExpression* -`Parameter`hinzu. Stellen Sie sicher, dass die Eigenschaft *SortExpression* `Parameter` s `Type` auf `String`festgelegt ist. Nachdem Sie die ersten beiden Aufgaben abgeschlossen haben, sollte das deklarative Markup von ObjectDataSource s wie folgt aussehen:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Als nächstes benötigen wir eine `SortExpression` Eigenschaft auf Seitenebene, deren Wert zum Ansichts Zustand serialisiert wird. Wenn kein Sortierungs Ausdruckswert festgelegt wurde, verwenden Sie ProductName als Standardwert:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Bevor die ObjectDataSource die `GetProductsPagedAndSorted`-Methode aufruft, müssen wir den *SortExpression* -`Parameter` auf den Wert der `SortExpression`-Eigenschaft festlegen. Fügen Sie im Ereignishandler `Selecting` die folgende Codezeile hinzu:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Sie müssen nur noch die Sortierungs Schnittstelle implementieren. Wie im letzten Beispiel gezeigt, kann die Sortier Schnittstelle mit drei Schaltflächen-websteuer Elementen implementiert werden, die es dem Benutzer ermöglichen, die Ergebnisse nach Produktname, Kategorie oder Lieferanten zu sortieren.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Erstellen Sie `Click` Ereignishandler für diese drei Schaltflächen-Steuerelemente. Setzen Sie im-Ereignishandler die `StartRowIndex` auf 0 zurück, legen Sie die `SortExpression` auf den entsprechenden Wert fest, und binden Sie die Daten erneut an den Repeater:

[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Das ist schon alles! Es gab zwar eine Reihe von Schritten, um benutzerdefiniertes Paging und Sortierung implementiert zu haben, die Schritte waren jedoch sehr ähnlich wie bei der Standard Auslagerung. In Abbildung 18 werden die Produkte angezeigt, wenn die letzte Seite der Daten angezeigt wird, wenn Sie nach Kategorie sortiert werden.

[![die letzte Seite der Daten sortiert nach Kategorie angezeigt wird.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Abbildung 18**: die letzte Seite mit Daten, sortiert nach Kategorie, wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))

> [!NOTE]
> In den vorherigen Beispielen wurde das Sortieren nach dem Lieferanten von Supplier suppliername als Sortier Ausdruck verwendet. Für die Implementierung der benutzerdefinierten Paginierung müssen wir jedoch "CompanyName" verwenden. Dies liegt daran, dass die gespeicherte Prozedur, die für die Implementierung von benutzerdefiniertem Paging zuständig ist `GetProductsPagedAndSorted` den Sortier Ausdruck an das `ROW_NUMBER()`-Schlüsselwort übergibt, das `ROW_NUMBER()`-Schlüsselwort den tatsächlichen Spaltennamen anstelle eines Alias. Daher müssen wir `CompanyName` (der Name der Spalte in der `Suppliers` Tabelle) anstelle des Alias verwenden, der in der `SELECT` Abfrage (`SupplierName`) für den Sortier Ausdruck verwendet wird.

## <a name="summary"></a>Zusammenfassung

Weder der DataList noch der Repeater bieten integrierte Sortier Unterstützung, aber mit etwas Code und einer benutzerdefinierten Sortier Schnittstelle können solche Funktionen hinzugefügt werden. Beim Implementieren von Sortierungen, aber nicht beim Paging, kann der Sortier Ausdruck über das `DataSourceSelectArguments` Objekt angegeben werden, das an die ObjectDataSource s `Select`-Methode übergeben wird. Diese `DataSourceSelectArguments` Objekt s `SortExpression` Eigenschaft kann im Ereignishandler von ObjectDataSource s `Selecting` zugewiesen werden.

Zum Hinzufügen von Sortierungs Funktionen zu einem DataList-oder Wiederholungs Modul, das bereits Paging-Unterstützung bietet, besteht der einfachste Ansatz darin, die Geschäftslogik Schicht an eine Methode anzupassen, die einen Sortierungs Ausdruck annimmt. Diese Informationen können dann über einen Parameter in der ObjectDataSource s-`SelectParameters`übergeben werden.

In diesem Tutorial wird die Untersuchung von Paging und Sortierungen mit den Steuerelementen DataList und Repeater abgeschlossen. Im nächsten und abschließenden Tutorial wird erläutert, wie Sie Schaltflächen-websteuer Elemente zu den Vorlagen DataList und Repeater hinzufügen, um eine benutzerdefinierte, vom Benutzer initiierte Funktionalität für einzelne Elemente bereitzustellen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war David suru. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Weiter](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
