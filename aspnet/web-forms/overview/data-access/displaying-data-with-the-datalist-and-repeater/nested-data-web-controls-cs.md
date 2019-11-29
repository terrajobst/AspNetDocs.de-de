---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Websteuer Elemente für die DatenC#von Netz Netzen () | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie einen Repeater verwenden, der in einem anderen Repeater geschachtelt ist. In den Beispielen wird veranschaulicht, wie Sie den inneren Repeater auffüllen, d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8ef15bebb2c29976274b0cca1d6ace434ccc55ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640295"
---
# <a name="nested-data-web-controls-c"></a>Geschachtelte Datenwebsteuerelemente (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) oder [PDF herunterladen](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> In diesem Tutorial wird erläutert, wie Sie einen Repeater verwenden, der in einem anderen Repeater geschachtelt ist. In den Beispielen wird veranschaulicht, wie der innere Repeater sowohl deklarativ als auch Programm gesteuert aufgefüllt wird.

## <a name="introduction"></a>Einführung

Zusätzlich zur statischen HTML-und Datenbindung-Syntax können Vorlagen auch websteuer Elemente und Benutzer Steuerelemente enthalten. Die Eigenschaften dieser websteuer Elemente können über deklarative, Datenbindung-Syntax zugewiesen werden, oder Sie können Programm gesteuert in den entsprechenden serverseitigen Ereignis Handlern aufgerufen werden.

Durch Einbetten von Steuerelementen in einer Vorlage können die Darstellung und die Benutzer Darstellung angepasst und verbessert werden. Beispielsweise wurde im Tutorial [Verwenden von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Element erläutert, wie Sie die GridView s-Anzeige anpassen, indem Sie ein Kalender Steuerelement in einem TemplateField-Steuerelement hinzufügen, um das Einstellungs Datum eines Mitarbeiters anzuzeigen. in den Tutorials zum [Hinzufügen von Validierungs Steuerelementen zu den Bearbeitungs-und einfügeschnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) und zum [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) haben wir gesehen, wie Sie die Bearbeitungs-und einfügeschnittstellen durch Hinzufügen von Validierungs Steuerelementen, Textfeldern, Dropdown Listen und anderen websteuer Elementen anpassen

Vorlagen können auch andere datenweb Steuerelemente enthalten. Das heißt, dass wir einen DataList haben können, der einen anderen DataList (oder Repeater, GridView oder DetailsView usw.) in seinen Vorlagen enthält. Die Herausforderung bei einer solchen Schnittstelle besteht darin, die entsprechenden Daten an das innere datenweb Steuerelement zu binden. Es gibt verschiedene Ansätze, die von deklarativen Optionen unter Verwendung von ObjectDataSource bis hin zu programmatischen Optionen reichen.

In diesem Tutorial wird erläutert, wie Sie einen Repeater verwenden, der in einem anderen Repeater geschachtelt ist. Der äußere Repeater enthält ein Element für jede Kategorie in der Datenbank und zeigt den Namen und die Beschreibung der Kategorie an. Jedes innere Wiederholungs Modul für Kategorieelemente zeigt Informationen für jedes Produkt an, das zu dieser Kategorie gehört (siehe Abbildung 1). Unsere Beispiele veranschaulichen, wie der innere Repeater sowohl deklarativ als auch Programm gesteuert aufgefüllt wird.

[![jede Kategorie zusammen mit ihren Produkten aufgeführt.](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Abbildung 1**: jede Kategorie wird zusammen mit ihren Produkten aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-data-web-controls-cs/_static/image3.png))

## <a name="step-1-creating-the-category-listing"></a>Schritt 1: Erstellen der Kategorienauflistung

Beim Erstellen einer Seite, die geschachtelte datenweb Steuerelemente verwendet, ist es hilfreich, zuerst das äußerste datenweb Steuerelement zu entwerfen, zu erstellen und zu testen, ohne sich um das innere geschachtelte Steuerelement Gedanken machen zu müssen. Beginnen Sie daher mit den Schritten, die zum Hinzufügen eines Wiederholungs Moduls zur Seite erforderlich sind, das den Namen und die Beschreibung für jede Kategorie auflistet.

Öffnen Sie zunächst die Seite `NestedControls.aspx` im Ordner `DataListRepeaterBasics`, und fügen Sie der Seite ein Repeater-Steuerelement hinzu. Legen Sie dessen `ID`-Eigenschaft auf `CategoryList`fest. Erstellen Sie im smarttagwiederholungs-Tag eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`.

[![den Namen der neuen ObjectDataSource kategoriesdatasource.](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Abbildung 2**: Benennen der neuen ObjectDataSource-`CategoriesDataSource` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-data-web-controls-cs/_static/image6.png))

Konfigurieren Sie die ObjectDataSource so, dass die Daten aus der `CategoriesBLL` Klasse `GetCategories` Methode abgerufen werden.

[![konfigurieren Sie die ObjectDataSource für die Verwendung der GetCategories-Methode der categoriesbll-Klasse.](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Abbildung 3**: Konfigurieren von ObjectDataSource für die Verwendung der `CategoriesBLL` Class s `GetCategories`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-data-web-controls-cs/_static/image9.png))

Um den Inhalt der Repeater s-Vorlage anzugeben, müssen wir zur Quell Ansicht wechseln und die deklarative Syntax manuell eingeben. Fügen Sie einen `ItemTemplate` hinzu, der den Name der Kategorie in einem `<h4>`-Element und die Beschreibung der Kategorie s in einem Absatz Element (`<p>`) anzeigt. Außerdem trennen Sie jede Kategorie durch eine horizontale Regel (`<hr>`). Nachdem Sie diese Änderungen vorgenommen haben, sollte die Seite deklarative Syntax für das Repeater und das ObjectDataSource-Objekt enthalten, das in etwa wie folgt aussieht:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Abbildung 4 zeigt den Fortschritt, wenn Sie in einem Browser angezeigt werden.

[![werden die Namen und Beschreibungen der Kategorien durch eine horizontale Regel getrennt aufgeführt.](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Abbildung 4**: der Name und die Beschreibung der Kategorien sind durch eine horizontale Regel getrennt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-data-web-controls-cs/_static/image12.png))

## <a name="step-2-adding-the-nested-product-repeater"></a>Schritt 2: Hinzufügen des aufgefügten Produkt Wiederholungs Moduls

Nachdem die Kategorienauflistung fertiggestellt wurde, besteht die nächste Aufgabe darin, der `CategoryList` s `ItemTemplate` einen Repeater hinzuzufügen, der Informationen zu den Produkten anzeigt, die zur entsprechenden Kategorie gehören. Es gibt eine Reihe von Möglichkeiten, die Daten für diesen inneren Wiederholungs Modul abzurufen. zwei davon werden wir kurz untersuchen. Erstellen Sie vorerst einfach die Produkt Wiederholung innerhalb der `CategoryList` Repeater s `ItemTemplate`. Mit dem Produkt Repeater werden alle Produkte in einer Auflistungs Liste angezeigt, einschließlich der Produktnamen und des Preises.

Um diesen Wiederholungs Modul zu erstellen, müssen Sie die deklarative Syntax und die Vorlagen für innere Wiederholungs-e manuell in die `CategoryList` s `ItemTemplate`eingeben. Fügen Sie das folgende Markup innerhalb der `CategoryList` Repeater s `ItemTemplate`hinzu:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Schritt 3: Binden der kategoriespezifischen Produkte an productbycategorylist Repeater

Wenn Sie die Seite zu diesem Zeitpunkt über einen Browser aufrufen, sieht Ihr Bildschirm wie in Abbildung 4 aus, da wir noch keine Daten an den Repeater binden müssen. Es gibt mehrere Möglichkeiten, die entsprechenden Produktdaten Sätze zu erfassen und an den Repeater zu binden, was effizienter als andere ist. Die Hauptaufgabe besteht hier darin, die entsprechenden Produkte für die angegebene Kategorie zurückzukehren.

Auf die Daten, die an das innere Wiederholungs Steuerelement gebunden werden sollen, kann entweder deklarativ, über eine ObjectDataSource im `CategoryList` Repeater s `ItemTemplate`oder Programm gesteuert über die Code Behind-Seite der ASP.net page s zugegriffen werden. Auf ähnliche Weise können diese Daten entweder deklarativ über die inneren Repeater s `DataSourceID`-Eigenschaft oder durch deklarative Datenbindung-Syntax oder Programm gesteuert durch Verweisen auf das innere Wiederholungs Modul im `CategoryList` Repeater-`ItemDataBound`-Ereignishandler gebunden werden, die `DataSource`-Eigenschaft Programm gesteuert festlegen und die `DataBind()`-Methode aufrufen. Untersuchen Sie die einzelnen Ansätze.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Deklaratives zugreifen auf die Daten mit einem ObjectDataSource-Steuerelement und dem`ItemDataBound`-Ereignis Handler

Da wir die ObjectDataSource ausgiebig in dieser tutorialreihe verwendet haben, ist die natürlichste Wahl für den Datenzugriff für dieses Beispiel die Verwendung von ObjectDataSource. Die `ProductsBLL`-Klasse verfügt über eine `GetProductsByCategoryID(categoryID)`-Methode, die Informationen zu den Produkten zurückgibt, die zum angegebenen *`categoryID`* gehören. Daher können wir eine ObjectDataSource zum `CategoryList` Repeater s-`ItemTemplate` hinzufügen und diese so konfigurieren, dass Sie über diese Klassen-Methode auf Ihre Daten zugreift.

Leider lässt der Repeater nicht zu, dass seine Vorlagen über die Designansicht bearbeitet werden, sodass wir die deklarative Syntax für dieses ObjectDataSource-Steuerelement per Hand hinzufügen müssen. Die folgende Syntax zeigt die `CategoryList` Repeater s `ItemTemplate` nach dem Hinzufügen dieser neuen ObjectDataSource (`ProductsByCategoryDataSource`):

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Wenn Sie den ObjectDataSource-Ansatz verwenden, müssen wir die `ProductsByCategoryList` Repeater s `DataSourceID`-Eigenschaft auf die `ID` von ObjectDataSource (`ProductsByCategoryDataSource`) festlegen. Beachten Sie auch, dass unsere ObjectDataSource über ein `<asp:Parameter>`-Element verfügt, das den *`categoryID`* Wert angibt, der an die `GetProductsByCategoryID(categoryID)`-Methode übergeben wird. Aber wie geben wir diesen Wert an? Im Idealfall kann die `DefaultValue`-Eigenschaft des `<asp:Parameter>`-Elements einfach mithilfe der Datenbindung-Syntax wie folgt festgelegt werden:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Leider ist die Datenbindung-Syntax nur in Steuerelementen gültig, die über ein `DataBinding` Ereignis verfügen. Die `Parameter`-Klasse verfügt nicht über ein derartiges Ereignis, daher ist die obige Syntax unzulässig und führt zu einem Laufzeitfehler.

Um diesen Wert festzulegen, müssen wir einen Ereignishandler für das `CategoryList` Repeater s `ItemDataBound`-Ereignis erstellen. Beachten Sie, dass das `ItemDataBound`-Ereignis einmal für jedes an den Repeater gebundene Element ausgelöst wird. Daher können Sie jedes Mal, wenn dieses Ereignis für die äußere Wiederholung ausgelöst wird, den aktuellen `CategoryID` Wert dem `ProductsByCategoryDataSource` ObjectDataSource s-`CategoryID`-Parameter zuweisen.

Erstellen Sie mit folgendem Code einen Ereignishandler für das `CategoryList` Repeater s `ItemDataBound`-Ereignis:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Mit diesem Ereignishandler wird zunächst sichergestellt, dass wir mit einem Datenelement, nicht mit dem Header-, Fußzeilen-oder Trennzeichen Element, erneut arbeiten. Als Nächstes verweisen wir auf die tatsächliche `CategoriesRow`-Instanz, die gerade an die aktuelle `RepeaterItem`gebunden wurde. Schließlich verweisen wir auf die ObjectDataSource im `ItemTemplate` und weisen den Wert des `CategoryID`-Parameters dem `CategoryID` der aktuellen `RepeaterItem`zu.

Mit diesem Ereignishandler wird der `ProductsByCategoryList` Repeater in jeder `RepeaterItem` an diese Produkte in der Kategorie `RepeaterItem` s gebunden. Abbildung 5 zeigt einen Screenshot der resultierenden Ausgabe.

[![der äußere Repeater jede Kategorie auflistet. der innere Abschnitt listet die Produkte für diese Kategorie auf.](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Abbildung 5**: der äußere Repeater listet jede Kategorie auf. die innere Liste listet die Produkte für diese Kategorie auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](nested-data-web-controls-cs/_static/image15.png)).

## <a name="accessing-the-products-by-category-data-programmatically"></a>Programm gesteuertes zugreifen auf die Produkte nach Kategoriedaten

Anstatt eine ObjectDataSource zum Abrufen der Produkte für die aktuelle Kategorie zu verwenden, könnten wir eine Methode in der Code Behind-Klasse der ASP.net page s (oder im Ordner "`App_Code`" oder in einem separaten Klassen Bibliotheksprojekt) erstellen, die den passenden Satz von Produkten zurückgibt, wenn Sie in einem `CategoryID`übergeben wird. Stellen Sie sich vor, dass wir eine solche Methode in der Code Behind-Klasse der ASP.net page s haben und dass Sie den Namen `GetProductsInCategory(categoryID)`hat. Mit dieser Methode könnten wir die Produkte für die aktuelle Kategorie mit der folgenden deklarativen Syntax an die innere Wiederholung binden:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

Die Repeater s `DataSource`-Eigenschaft verwendet die Datenbindung-Syntax, um anzugeben, dass die Daten aus der `GetProductsInCategory(categoryID)`-Methode stammen. Da `Eval("CategoryID")` einen Wert vom Typ `Object`zurückgibt, wandeln wir das Objekt in eine `Integer` um, bevor es an die `GetProductsInCategory(categoryID)`-Methode übergeben wird. Beachten Sie, dass die `CategoryID`, auf die über die Datenbindung-Syntax zugegriffen wird, die `CategoryID` im *äußeren* Repeater (`CategoryList`) ist, die an die Datensätze in der `Categories` Tabelle gebunden ist. Aus diesem Grundwissen wir, dass `CategoryID` kein Daten Bank `NULL` Wert sein kann. aus diesem Grund können wir die `Eval`-Methode Blind umwandeln, ohne zu überprüfen, ob wir uns mit einer `DBNull`befassen.

Bei diesem Ansatz müssen wir die `GetProductsInCategory(categoryID)`-Methode erstellen und den entsprechenden Satz von Produkten abrufen lassen, wenn der bereitgestellte *`categoryID`* ist. Hierfür können Sie einfach die `ProductsDataTable` zurückgeben, die von der `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode zurückgegeben wird. Erstellen Sie die `GetProductsInCategory(categoryID)`-Methode in der Code Behind-Klasse für unsere `NestedControls.aspx` Seite. Verwenden Sie hierzu den folgenden Code:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Diese Methode erstellt einfach eine Instanz der `ProductsBLL`-Methode und gibt die Ergebnisse der `GetProductsByCategoryID(categoryID)`-Methode zurück. Beachten Sie, dass die Methode als `Public` oder `Protected`markiert werden muss. Wenn die Methode als `Private`gekennzeichnet ist, kann nicht vom deklarativen Markup der ASP.NET-Seite aufgerufen werden.

Nachdem Sie diese Änderungen vorgenommen haben, um diese neue Technik zu verwenden, nehmen Sie sich einen Moment Zeit, um die Seite über einen Browser anzuzeigen. Die Ausgabe sollte mit der Ausgabe identisch sein, wenn der ObjectDataSource-und `ItemDataBound`-ereignishandleransatz verwendet wird (siehe Abbildung 5, um einen Screenshot anzuzeigen).

> [!NOTE]
> Es mag so aussehen, als ob helpdeskrechnungen die `GetProductsInCategory(categoryID)` Methode in der Code Behind-Klasse der ASP.net page s erstellt. Schließlich erstellt diese Methode einfach eine Instanz der `ProductsBLL`-Klasse und gibt die Ergebnisse ihrer `GetProductsByCategoryID(categoryID)`-Methode zurück. Warum wird diese Methode nicht einfach direkt aus der Datenbindung-Syntax im Inneren Repeater aufgerufen, wie: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Obwohl diese Syntax nicht mit der aktuellen Implementierung der `ProductsBLL`-Klasse funktioniert (da die `GetProductsByCategoryID(categoryID)`-Methode eine Instanzmethode ist), können Sie `ProductsBLL` ändern, um eine statische `GetProductsByCategoryID(categoryID)`-Methode einzuschließen, oder die Klasse muss eine statische `Instance()` Methode enthalten, um eine neue Instanz der `ProductsBLL`-Klasse zurückzugeben.

Obwohl solche Änderungen die `GetProductsInCategory(categoryID)` Methode in der Code Behind-Klasse der ASP.NET-Seite nicht benötigen, bietet die Code Behind-Klassenmethode mehr Flexibilität bei der Arbeit mit den abgerufenen Daten, wie wir in Kürze sehen werden.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Abrufen aller Produktinformationen auf einmal

Die zwei von uns erprüften Verfahren untersuchen diese Produkte für die aktuelle Kategorie, indem Sie die `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode aufrufen (der erste Ansatz hat dies durch eine ObjectDataSource erzielt, die zweite durch die `GetProductsInCategory(categoryID)`-Methode in der Code Behind-Klasse). Jedes Mal, wenn diese Methode aufgerufen wird, ruft die Geschäftslogik Schicht die Datenzugriffs Ebene ab, die die Datenbank mit einer SQL-Anweisung abfragt, die Zeilen aus der `Products` Tabelle zurückgibt, deren `CategoryID` Feld mit dem angegebenen Eingabeparameter übereinstimmt.

Bei *n* Kategorien im System sind bei diesem Ansatz *n* + 1 Aufrufe der Datenbank eine Datenbankabfrage, um alle Kategorien zu erhalten, und dann *n* Aufrufe, um die für jede Kategorie spezifischen Produkte zu erhalten. Wir können jedoch alle benötigten Daten in nur zwei Daten Bank aufrufen abrufen, indem wir einen Aufruf zum Abrufen aller Kategorien und einen anderen abrufen, um alle Produkte zu erhalten. Wenn alle Produkte vorhanden sind, können wir diese Produkte so filtern, dass nur die Produkte, die mit der aktuellen `CategoryID` übereinstimmen, an die innere Wiederholung der Kategorie gebunden sind.

Um diese Funktionalität bereitzustellen, müssen wir nur geringfügige Änderungen an der `GetProductsInCategory(categoryID)`-Methode in der Code Behind-Klasse der ASP.net page s vornehmen. Anstatt die Ergebnisse der `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode zurückzugeben, können wir stattdessen zuerst auf *alle* Produkte zugreifen (sofern noch nicht darauf zugegriffen wurde) und dann nur die gefilterte Ansicht der Produkte zurückgeben, die auf dem weiter gegebenen `CategoryID`basiert.

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Beachten Sie das Hinzufügen der Variablen auf Seitenebene, `allProducts`. Diese enthält Informationen zu allen Produkten und wird beim ersten Aufruf der `GetProductsInCategory(categoryID)`-Methode aufgefüllt. Nachdem sichergestellt wurde, dass das `allProducts` Objekt erstellt und aufgefüllt wurde, filtert die-Methode die Ergebnisse der Datentabelle, sodass nur die Zeilen, deren `CategoryID` mit dem angegebenen `CategoryID` übereinstimmen, zugänglich sind. Diese Vorgehensweise verringert die Häufigkeit, mit der der Zugriff auf die Datenbank von *N* + 1 auf zwei reduziert wird.

Durch diese Erweiterung werden keine Änderungen am gerenderten Markup der Seite eingeführt, und es werden weniger Datensätze als bei der anderen Methode zurückgegeben. Die Anzahl der Aufrufe der Datenbank wird einfach reduziert.

> [!NOTE]
> Dies kann intuitiv daran liegen, dass durch das Reduzieren der Anzahl von Daten Bank Zugriffen die Leistung ordnungsgemäß verbessert wird. Dies ist jedoch möglicherweise nicht der Fall. Wenn Sie über eine große Anzahl von Produkten verfügen, deren `CategoryID` z. b. `NULL`ist, gibt der-Befehl der `GetProducts`-Methode eine Reihe von Produkten zurück, die nie angezeigt werden. Außerdem kann das Zurückgeben aller Produkte verschwenderisch sein, wenn Sie nur eine Teilmenge der Kategorien anzeigt. Dies kann der Fall sein, wenn Sie Paging implementiert haben.

Wie immer, wenn es um die Analyse der Leistung von zwei Techniken geht, besteht das einzige Surefire-Measure darin, gesteuerte Tests auszuführen, die auf gängige Fallszenarien Ihrer Anwendung zugeschnitten sind.

## <a name="summary"></a>Summary

In diesem Tutorial haben Sie erfahren, wie Sie ein datenweb Steuerelement in einem anderen schachteln können. dabei wird erläutert, wie ein äußerer Repeater ein Element für jede Kategorie mit einem inneren Repeater anzeigt, in dem die Produkte für jede Kategorie in einer Auflistungs Liste aufgelistet werden. Die größte Herausforderung beim Aufbau einer geschachtelten Benutzeroberfläche besteht darin, auf die richtigen Daten zuzugreifen und diese an das innere datenweb Steuerelement zu binden. Es stehen eine Vielzahl von Techniken zur Verfügung, von denen zwei in diesem Tutorial untersucht wurden. Der erste Ansatz, der untersucht wurde, verwendete ObjectDataSource im Outer Data Web Control s-`ItemTemplate`, das über seine `DataSourceID`-Eigenschaft an das innere Daten-websteuer Element gebunden war. Das zweite Verfahren hat über eine Methode in der Code Behind-Klasse der ASP.NET-Seite auf die Daten zugegriffen. Diese Methode kann dann über die Datenbindung-Syntax an die `DataSource` Eigenschaft des inneren Daten-websteuer Elements gebunden werden.

Obwohl die geschachtelte Benutzeroberfläche, die in diesem Tutorial überprüft wurde, einen Repeater verwendet hat, der in einem Repeater geschachtelt ist, können diese Techniken auf die anderen datenweb Steuerelemente erweitert werden. Sie können einen Repeater innerhalb einer GridView oder einer GridView innerhalb eines DataList Schachteln usw.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Zack Jones und Liz shulok. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [Weiter](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
