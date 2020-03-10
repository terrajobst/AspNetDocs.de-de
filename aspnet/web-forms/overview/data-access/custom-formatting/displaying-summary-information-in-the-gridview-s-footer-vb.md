---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Anzeigen von Zusammenfassungs Informationen im GridView-Footer (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Zusammenfassungs Informationen werden häufig im unteren Bereich des Berichts in einer Zusammenfassungs Zeile angezeigt. Das GridView-Steuerelement kann eine Footerzeile enthalten, in deren Zellen wir PR haben können...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: c208b4a756f5700be46eec924d8cf8f49b9d2507
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78482079"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Anzeigen von Zusammenfassungsinformationen im GridView-Fuß (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) oder [PDF herunterladen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> Zusammenfassungs Informationen werden häufig im unteren Bereich des Berichts in einer Zusammenfassungs Zeile angezeigt. Das GridView-Steuerelement kann eine Footerzeile enthalten, in deren Zellen wir Programm gesteuert Aggregatdaten einfügen können. In diesem Tutorial wird erläutert, wie Aggregatdaten in dieser Footerzeile angezeigt werden.

## <a name="introduction"></a>Einführung

Zusätzlich zum Anzeigen der Preise der Produkte, Einheiten in Aktien, Einheiten in der Bestellung und Neuanordnung der Ebenen kann ein Benutzer auch an Aggregat Informationen interessiert sein, z. b. den Durchschnittspreis, die Gesamtzahl der Einheiten im Lager usw. Diese Zusammenfassungs Informationen werden häufig in einer Zusammenfassungs Zeile unten im Bericht angezeigt. Das GridView-Steuerelement kann eine Footerzeile enthalten, in deren Zellen wir Programm gesteuert Aggregatdaten einfügen können.

Diese Aufgabe stellt drei Herausforderungen dar:

1. Konfigurieren der GridView zum Anzeigen der Footerzeile
2. Bestimmen der Zusammenfassungs Daten; Das heißt, wie berechnen wir den Durchschnittspreis oder die Summe der Einheiten im Lager?
3. Einfügen der Zusammenfassungs Daten in die entsprechenden Zellen der Footerzeile

In diesem Tutorial erfahren Sie, wie Sie diese Herausforderungen meistern können. Insbesondere erstellen wir eine Seite, die die Kategorien in einer Dropdown Liste mit den in einer GridView angezeigten Produkten der ausgewählten Kategorie auflistet. Die GridView enthält eine Footerzeile, in der der Durchschnittspreis und die Gesamtzahl der Einheiten in der Bestellung und die Reihenfolge der Produkte in dieser Kategorie angezeigt werden.

[![Zusammenfassungs Informationen werden in der Fußzeile der GridView angezeigt.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Abbildung 1**: zusammenfassende Informationen werden in der Fußzeile der GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))

In diesem Tutorial mit der Kategorie "Produkte Master/Detail Interface" werden die Konzepte erläutert, die in der vorherigen [Master/Detail-Filterung mit einem DropDownList-](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Tutorial behandelt wurden. Wenn Sie noch nicht mit dem vorherigen Tutorial gearbeitet haben, sollten Sie dies tun, bevor Sie mit diesem Tutorial fortfahren.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Schritt 1: Hinzufügen der Kategorien DropDownList und Products GridView

Bevor wir uns mit dem Hinzufügen von Zusammenfassungs Informationen zum Footer von GridView befassen, erstellen wir zunächst einfach den Master-/Detailbericht. Nachdem wir diesen ersten Schritt abgeschlossen haben, sehen wir uns an, wie Zusammenfassungs Daten eingeschlossen werden.

Öffnen Sie zunächst die Seite `SummaryDataInFooter.aspx` im Ordner `CustomFormatting`. Fügen Sie ein Dropdown List-Steuerelement hinzu, und legen Sie dessen `ID` auf `Categories` Klicken Sie anschließend auf den Link Datenquelle auswählen aus dem Smarttag DropDownList, und wählen Sie das Hinzufügen einer neuen ObjectDataSource namens `CategoriesDataSource` aus, die die `GetCategories()`-Methode der `CategoriesBLL` Klasse aufruft.

[![eine neue ObjectDataSource mit dem Namen "categoriesdatasource" hinzufügen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen ObjectDataSource mit dem Namen "`CategoriesDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))

[![, dass ObjectDataSource die GetCategories ()-Methode der kategoriesbll-Klasse aufruft.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Abbildung 3**: lassen Sie die `GetCategories()` Methode der `CategoriesBLL` Klasse von ObjectDataSource aufrufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))

Nach dem Konfigurieren von ObjectDataSource kehrt der Assistent zum Konfigurationsassistenten für Datenquellen von DropDownList zurück, von dem aus angegeben werden muss, welcher Daten Feldwert angezeigt werden soll und welcher dem Wert der `ListItem` s der DropDownList entsprechen soll. Lassen Sie das `CategoryName` Feld angezeigt, und verwenden Sie die `CategoryID` als Wert.

[![die Felder "CategoryName" und "CategoryID" als Text und Wert für die "ListItems"-Werte verwenden.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Abbildung 4**: Verwenden der Felder "`CategoryName`" und "`CategoryID`" als `Text` und `Value` für die `ListItem` e ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))

An dieser Stelle haben wir eine Dropdown List (`Categories`), in der die Kategorien im System aufgelistet sind. Wir müssen nun eine GridView-Ansicht hinzufügen, die die Produkte auflistet, die zur ausgewählten Kategorie gehören. Nehmen Sie sich jedoch einen Moment Zeit, um das Kontrollkästchen AutoPostBack aktivieren im Smarttag DropDownList zu aktivieren. Wie im Tutorial *Master/Detail Filtering with a DropDownList* erläutert, legen Sie die `AutoPostBack`-Eigenschaft von DropDownList auf fest, `True` die Seite jedes Mal zurückgesendet wird, wenn der Dropdown List-Wert geändert wird. Dadurch wird die GridView aktualisiert und zeigt die Produkte für die neu ausgewählte Kategorie an. Wenn die `AutoPostBack`-Eigenschaft auf `False` (Standardeinstellung) festgelegt ist, führt das Ändern der Kategorie nicht zu einem Postback und aktualisiert daher die aufgelisteten Produkte nicht.

[![aktivieren Sie das Kontrollkästchen AutoPostBack aktivieren im Smarttag DropDownList.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Abbildung 5**: Aktivieren Sie das Kontrollkästchen AutoPostBack aktivieren im Smarttag DropDownList ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png)).

Fügen Sie der Seite ein GridView-Steuerelement hinzu, um die Produkte für die ausgewählte Kategorie anzuzeigen. Legen Sie den `ID` der GridView auf `ProductsInCategory`, und binden Sie ihn an eine neue ObjectDataSource mit dem Namen `ProductsInCategoryDataSource`.

[![eine neue ObjectDataSource mit dem Namen productsincategorydatasource hinzufügen.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen einer neuen ObjectDataSource mit dem Namen "`ProductsInCategoryDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))

Konfigurieren Sie die ObjectDataSource so, dass Sie die `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse aufruft.

[![von ObjectDataSource die getproductbycategoryid (CategoryID)-Methode aufrufen.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Abbildung 7**: lassen Sie die `GetProductsByCategoryID(categoryID)` Methode von ObjectDataSource aufrufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))

Da die `GetProductsByCategoryID(categoryID)`-Methode einen Eingabeparameter annimmt, können Sie im letzten Schritt des Assistenten die Quelle des Parameter Werts angeben. Um diese Produkte aus der ausgewählten Kategorie anzuzeigen, lassen Sie den Parameter aus der Dropdown Liste `Categories`.

[![den Wert für den CategoryID-Parameter aus den Dropdown Listen "ausgewählte Kategorien"](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Abbildung 8: ermitteln**des *`categoryID`* Parameter Werts aus der Dropdown Liste Ausgewählte Kategorien ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))

Nachdem Sie den Assistenten abgeschlossen haben, verfügt die GridView über ein BoundField für jede der Produkteigenschaften. Wir bereinigen diese boundfields, damit nur die `ProductName`, `UnitPrice`, `UnitsInStock`und `UnitsOnOrder` boundfields angezeigt werden. Sie können den verbleibenden boundfields-Einstellungen auf Feldebene hinzufügen (z. b. das `UnitPrice` als Währung formatieren). Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der GridView in etwa wie folgt aussehen:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

Zu diesem Zeitpunkt verfügen wir über einen voll funktionsfähigen Master/Detail-Bericht, in dem der Name, der Einheitspreis, die Einheiten im Lager und die Einheiten für die Produkte, die zur ausgewählten Kategorie gehören, angezeigt werden.

[![den Wert für den CategoryID-Parameter aus den Dropdown Listen "ausgewählte Kategorien"](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Abbildung 9**: Anzeigen des *`categoryID`* Parameter Werts aus den Dropdown Listen für ausgewählte Kategorien ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Schritt 2: Anzeigen einer Fußzeile in der GridView

Das GridView-Steuerelement kann eine Kopf-und eine Footerzeile anzeigen. Diese Zeilen werden abhängig von den Werten der Eigenschaften `ShowHeader` und `ShowFooter` angezeigt, wobei `ShowHeader` standardmäßig `True` und `ShowFooter` `False`. Wenn Sie eine Fußzeile in die GridView einschließen möchten, legen Sie einfach die `ShowFooter`-Eigenschaft auf `True`fest.

[![legen Sie die ShowFooter-Eigenschaft der GridView auf "true" fest.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Abbildung 10**: Festlegen der `ShowFooter`-Eigenschaft der GridView auf `True` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))

Die Footerzeile enthält eine Zelle für jedes der Felder, die in der GridView definiert sind. Diese Zellen sind jedoch standardmäßig leer. Nehmen Sie sich einen Moment Zeit, um den Fortschritt in einem Browser anzuzeigen. Wenn die `ShowFooter`-Eigenschaft jetzt auf `True`festgelegt ist, enthält die GridView eine leere Footerzeile.

[![die GridView nun eine Footerzeile enthält.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Abbildung 11**: die GridView enthält jetzt eine Footerzeile ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))

Die Footerzeile in Abbildung 11 steht nicht aus, da Sie einen weißen Hintergrund hat. Erstellen Sie eine `FooterStyle` CSS-Klasse in `Styles.css`, die einen dunkelroten Hintergrund angibt, und konfigurieren Sie dann die `GridView.skin` Skin-Datei im `DataWebControls` Design, um diese CSS-Klasse der `FooterStyle`-Eigenschaft der GridView zuzuweisen.`CssClass` Wenn Sie für Skins und Designs einen Pinsel einrichten müssen, lesen Sie das Tutorial [Anzeigen von Daten mit dem ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) -Tutorial.

Fügen Sie zunächst die folgende CSS-Klasse zu `Styles.css`hinzu:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

Die `FooterStyle` CSS-Klasse ähnelt der-Klasse für die `HeaderStyle`-Klasse. die Hintergrundfarbe des `HeaderStyle`ist jedoch dunkler, und der Text wird in Fett Schrift angezeigt. Außerdem wird der Text in der Fußzeile rechtsbündig ausgerichtet, während der Text des Headers zentriert ist.

Um diese CSS-Klasse jedem GridView-Footer zuzuordnen, öffnen Sie die Datei `GridView.skin` im `DataWebControls` Design, und legen Sie die `CssClass`-Eigenschaft des `FooterStyle`fest. Nach dieser Addition sollte das Markup der Datei wie folgt aussehen:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Wie der Screenshot unten zeigt, macht diese Änderung den Fußzeile deutlicher.

[![die Footerzeile der GridView jetzt eine rot-Hintergrundfarbe](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Abbildung 12**: die Footerzeile der GridView weist jetzt eine farbige Hintergrundfarbe auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Schritt 3: Berechnen der Zusammenfassungs Daten

Wenn die Fußzeile der GridView angezeigt wird, besteht die nächste Herausforderung darin, wie die Zusammenfassungs Daten berechnet werden. Diese Aggregat Informationen können auf zwei Arten berechnet werden:

1. Über eine SQL-Abfrage könnten wir eine zusätzliche Abfrage an die Datenbank ausgeben, um die Zusammenfassungs Daten für eine bestimmte Kategorie zu berechnen. SQL enthält eine Reihe von Aggregatfunktionen zusammen mit einer `GROUP BY`-Klausel, um die Daten anzugeben, über die die Daten zusammengefasst werden sollen. Die folgende SQL-Abfrage würde die benötigten Informationen zurückbringen:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Natürlich sollten Sie diese Abfrage nicht direkt über die `SummaryDataInFooter.aspx` Seite ausgeben, sondern indem Sie eine Methode in der `ProductsTableAdapter` und der `ProductsBLL`erstellen.
2. Berechnen Sie diese Informationen, während Sie der GridView hinzugefügt werden, wie unter [benutzerdefinierte Formatierung basierend auf datentutorial](custom-formatting-based-upon-data-cs.md) erläutert wird. der `RowDataBound` Ereignishandler von GridView wird für jede Zeile, die der GridView hinzugefügt wurde, nach der Datenbindung einmal ausgelöst. Durch das Erstellen eines Ereignis Handlers für dieses Ereignis können wir eine laufende Summe der zu aggregierenden Werte beibehalten. Nachdem die letzte Daten Zeile an die GridView gebunden wurde, verfügen wir über die Summen und die erforderlichen Informationen, um den Durchschnitt zu berechnen.

Ich verwende in der Regel den zweiten Ansatz, da eine Fahrt zur Datenbank und der Aufwand zum Implementieren der Zusammenfassungs Funktionalität in der Datenzugriffs Schicht und in der Geschäftslogik Schicht erforderlich ist, aber beide Vorgehensweisen sind ausreichend. Verwenden Sie für dieses Tutorial die zweite Option, und verfolgen Sie die laufende Summe mithilfe des `RowDataBound`-Ereignis Handlers.

Erstellen Sie einen `RowDataBound` Ereignishandler für das GridView-Ereignis, indem Sie im Designer das GridView-Symbol auswählen, auf das Blitz Symbol des Eigenschaftenfenster klicken und auf das `RowDataBound`-Ereignis doppelklicken. Alternativ können Sie das GridView-Ereignis und das zugehörige rowdatabosereignis aus den Dropdown Listen am Anfang der ASP.NET-Code Behind-Klassendatei auswählen. Dadurch wird ein neuer Ereignishandler mit dem Namen `ProductsInCategory_RowDataBound` in der Code Behind-Klasse der `SummaryDataInFooter.aspx` Seite erstellt.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Um eine laufende Summe beizubehalten, müssen wir Variablen außerhalb des Gültigkeits Bereichs des Ereignis Handlers definieren. Erstellen Sie die folgenden vier Variablen auf Seitenebene:

- `_totalUnitPrice`vom Typ `Decimal`
- `_totalNonNullUnitPriceCount`vom Typ `Integer`
- `_totalUnitsInStock`vom Typ `Integer`
- `_totalUnitsOnOrder`vom Typ `Integer`

Schreiben Sie als nächstes den Code, um diese drei Variablen für jede Daten Zeile zu erhöhen, die im `RowDataBound`-Ereignishandler gefunden wurde.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

Der `RowDataBound` Ereignishandler stellt sicher, dass wir mit einer DataRow arbeiten. Nachdem dieser festgelegt wurde, wird die `Northwind.ProductsRow`-Instanz, die nur an das `GridViewRow`-Objekt in `e.Row` gebunden war, in der Variablen `product`gespeichert. Als nächstes wird das Ausführen von Gesamt Variablen durch die entsprechenden Werte des aktuellen Produkts erhöht (vorausgesetzt, dass Sie keinen Daten Bank `NULL` Wert enthalten). Wir verfolgen sowohl den laufenden `UnitPrice` insgesamt als auch die Anzahl der nicht`NULL` `UnitPrice` Datensätze, da der Durchschnittspreis der Quotienten dieser beiden Zahlen ist.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Schritt 4: Anzeigen der Zusammenfassungs Daten in der Fußzeile

Wenn die Zusammenfassungs Daten summiert sind, besteht der letzte Schritt darin, Sie in der Fußzeile der GridView anzuzeigen. Diese Aufgabe kann auch Programm gesteuert über den `RowDataBound`-Ereignishandler ausgeführt werden. Beachten Sie, dass der `RowDataBound` Ereignishandler für *jede* Zeile ausgelöst wird, die an die GridView gebunden ist, einschließlich der Footerzeile. Daher können wir den Ereignishandler erweitern, um die Daten in der Footerzeile mithilfe des folgenden Codes anzuzeigen:

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Da die Footerzeile der GridView hinzugefügt wird, nachdem alle Daten Zeilen hinzugefügt wurden, können wir sicher sein, dass Sie bis zum Anzeigen der Zusammenfassungs Daten in der Fußzeile sicher sind, dass die laufenden gesamten Berechnungen abgeschlossen sind. Der letzte Schritt besteht darin, diese Werte in den Zellen des Footers festzulegen.

Zum Anzeigen von Text in einer bestimmten footerzelle verwenden Sie `e.Row.Cells(index).Text = value`, bei dem die `Cells` Indizierung bei 0 beginnt. Der folgende Code berechnet den Durchschnittspreis (der Gesamtpreis dividiert durch die Anzahl der Produkte) und zeigt ihn zusammen mit der Gesamtanzahl der Einheiten im Lager und den Einheiten in den entsprechenden Fußzeilen Zellen der GridView an.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Abbildung 13 zeigt den Bericht, nachdem dieser Code hinzugefügt wurde. Beachten Sie, dass der `ToString("c")` bewirkt, dass die Informationen zur durchschnittlichen Preis Zusammenfassung wie eine Währung formatiert werden.

[![die Footerzeile der GridView jetzt eine rot-Hintergrundfarbe](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Abbildung 13**: die Footerzeile der GridView weist jetzt eine farbige Hintergrundfarbe auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))

## <a name="summary"></a>Zusammenfassung

Das Anzeigen von Zusammenfassungs Daten ist eine gängige Berichts Anforderung, und das GridView-Steuerelement erleichtert das einschließen derartiger Informationen in der Footerzeile. Die Footerzeile wird angezeigt, wenn die `ShowFooter`-Eigenschaft der GridView auf `True` festgelegt ist und der Text in den Zellen Programm gesteuert über den `RowDataBound`-Ereignishandler festgelegt werden kann. Das Berechnen der Zusammenfassungs Daten kann entweder durch erneutes Abfragen der Datenbank oder durch Verwendung von Code in der Code Behind-Klasse der ASP.NET-Seite erfolgen, um die Zusammenfassungs Daten Programm gesteuert zu berechnen.

In diesem Tutorial wird die Prüfung der benutzerdefinierten Formatierung mit den Steuerelementen GridView, DetailsView und FormView abgeschlossen. Im nächsten Tutorial wird das Einfügen, aktualisieren und Löschen von Daten mit denselben Steuerelementen untersucht.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Previous](using-the-formview-s-templates-vb.md)
