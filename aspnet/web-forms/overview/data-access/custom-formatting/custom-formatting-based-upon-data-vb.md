---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Benutzerdefinierte Formatierung basierend auf Daten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Das Anpassen des Formats von GridView, DetailsView oder FormView basierend auf den Daten, an die es gebunden ist, kann auf verschiedene Weise erreicht werden. In diesem Tutorial werden wir...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 268dc763ef6954903f721a3015daaf10bf9bccb1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78482763"
---
# <a name="custom-formatting-based-upon-data-vb"></a>Benutzerdefinierte Formatierung auf Datenbasis (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) oder [PDF herunterladen](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Das Anpassen des Formats von GridView, DetailsView oder FormView basierend auf den Daten, an die es gebunden ist, kann auf verschiedene Weise erreicht werden. In diesem Lernprogramm erfahren Sie, wie Sie die Daten gebundene Formatierung durch die Verwendung der Ereignishandler Daten gebunden und rowdatbound durchführen.

## <a name="introduction"></a>Einführung

Die Darstellung der Steuerelemente GridView, DetailsView und FormView kann durch eine Vielzahl von Stil bezogenen Eigenschaften angepasst werden. Eigenschaften wie `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`und `Height`legen unter anderem die allgemeine Darstellung des gerenderten Steuer Elements vor. Eigenschaften wie `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`und andere ermöglichen es, dass dieselben Stileinstellungen auf bestimmte Abschnitte angewendet werden. Ebenso können diese Stileinstellungen auf Feldebene angewendet werden.

In vielen Szenarios sind die Formatierungs Anforderungen jedoch vom Wert der angezeigten Daten abhängig. Wenn Sie z. b. die Aufmerksamkeit auf die Produkte von nicht vorrätigen Produkten ziehen möchten, kann ein Bericht mit Produktinformationen die Hintergrundfarbe für die Produkte, deren `UnitsInStock` und `UnitsOnOrder` Felder gleich 0 (null) sind, auf gelb festlegen. Um die teureren Produkte hervorzuheben, möchten wir vielleicht die Preise für diese Produkte anzeigen, die mehr als $75,00 in Fett Schrift enthalten.

Das Anpassen des Formats von GridView, DetailsView oder FormView basierend auf den Daten, an die es gebunden ist, kann auf verschiedene Weise erreicht werden. In diesem Tutorial wird erläutert, wie Sie die Daten gebundene Formatierung durch die Verwendung der Ereignishandler `DataBound` und `RowDataBound` ausführen. Im nächsten Tutorial untersuchen wir eine alternative Methode.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Verwenden des`DataBound`Ereignis Handlers des DetailsView-Steuer Elements

Wenn Daten an eine DetailsView gebunden werden, entweder aus einem Datenquellen-Steuerelement oder durch Programm gesteuertes Zuweisen von Daten zur `DataSource`-Eigenschaft des Steuer Elements und Aufrufen seiner `DataBind()`-Methode, werden die folgenden Schritte ausgeführt:

1. Das `DataBinding` Ereignis des Data Web-Steuer Elements wird ausgelöst.
2. Die Daten werden an das datenweb-Steuerelement gebunden.
3. Das `DataBound` Ereignis des Data Web-Steuer Elements wird ausgelöst.

Benutzerdefinierte Logik kann direkt nach den Schritten 1 und 3 durch einen Ereignishandler eingefügt werden. Wenn Sie einen Ereignishandler für das `DataBound`-Ereignis erstellen, können wir die Daten, die an das datenweb-Steuerelement gebunden wurden, Programm gesteuert bestimmen und die Formatierung nach Bedarf anpassen. Um dies zu veranschaulichen, erstellen wir eine DetailsView, in der allgemeine Informationen zu einem Produkt aufgeführt werden. der `UnitPrice` Wert wird jedoch in einer fett formatierten ***Schriftart*** angezeigt, wenn der Wert $75,00 überschreitet.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Schritt 1: Anzeigen der Produktinformationen in einer DetailsView

Öffnen Sie die Seite `CustomColors.aspx` im Ordner `CustomFormatting`, ziehen Sie ein DetailsView-Steuerelement aus der Toolbox auf den Designer, legen Sie dessen `ID`-Eigenschafts Wert auf `ExpensiveProductsPriceInBoldItalic`fest, und binden Sie ihn an ein neues ObjectDataSource-Steuerelement, das die `ProductsBLL`-Methode der `GetProducts()` Klasse aufruft. Die ausführlichen Schritte für das Ausführen dieser Schritte werden aus Gründen der Kürze ausgelassen, da wir Sie in den vorherigen Tutorials ausführlich untersucht haben.

Nachdem Sie ObjectDataSource an die DetailsView gebunden haben, nehmen Sie sich einen Moment Zeit, um die Feldliste zu ändern. Ich habe mich dafür entschieden, die `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`und `Discontinued` boundfields zu entfernen und die verbleibenden boundfields neu zu formatiert. Außerdem habe ich die Einstellungen für `Width` und `Height` gelöscht. Da in der DetailsView nur ein einziger Datensatz angezeigt wird, müssen wir das Paging aktivieren, damit der Endbenutzer alle Produkte anzeigen kann. Aktivieren Sie hierzu das Kontrollkästchen Paging aktivieren im Smarttags der DetailsView.

[![Abbildung 1: Aktivieren Sie das Kontrollkästchen "Paging aktivieren" im Smarttags der DetailsView.](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Abbildung 1**: Aktivieren Sie das Kontrollkästchen Paging aktivieren im smarttagtag der DetailsView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image3.png)).

Nach diesen Änderungen sieht das DetailsView-Markup wie folgt aus:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um diese Seite in Ihrem Browser zu testen.

[![das DetailsView-Steuerelement ein Produkt gleichzeitig anzeigt.](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Abbildung 2**: das DetailsView-Steuerelement zeigt jeweils ein Produkt an ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image6.png))

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Schritt 2: Programm gesteuertes bestimmen des Werts der Daten im Daten gebundenen Ereignis Handler

Um den Preis in einer fett formatierten Schriftart für die Produkte anzuzeigen, deren `UnitPrice` Wert $75,00 überschreitet, müssen wir zunächst in der Lage sein, den `UnitPrice` Wert Programm gesteuert zu bestimmen. Für die DetailsView kann dies im `DataBound` Ereignishandler erreicht werden. Um den Ereignishandler zu erstellen, klicken Sie im Designer auf die DetailsView, und navigieren Sie dann zum Eigenschaftenfenster. Drücken Sie F4, um Sie aufzurufen, wenn Sie nicht sichtbar ist, oder wechseln Sie zum Menü Ansicht, und wählen Sie die Menüoption Eigenschaften Fenster aus. Klicken Sie im Eigenschaftenfenster auf das Blitz Symbol, um die Ereignisse der DetailsView aufzulisten. Doppelklicken Sie dann entweder auf das `DataBound`-Ereignis, oder geben Sie den Namen des Ereignis Handlers ein, den Sie erstellen möchten.

![Erstellen eines Ereignis Handlers für das Daten gebundene Ereignis](custom-formatting-based-upon-data-vb/_static/image7.png)

**Abbildung 3**: Erstellen eines Ereignis Handlers für das `DataBound`-Ereignis

> [!NOTE]
> Sie können auch einen Ereignishandler aus dem Codeteil der ASP.NET-Seite erstellen. Dort finden Sie zwei Dropdown Listen am oberen Rand der Seite. Wählen Sie in der rechten Dropdown Liste das Objekt aus der linken Dropdown Liste und das Ereignis aus, für das Sie einen Handler erstellen möchten, und Visual Studio erstellt automatisch den entsprechenden Ereignishandler.

Dadurch wird der Ereignishandler automatisch erstellt, und Sie gelangen zum Code Teil, in dem er hinzugefügt wurde. An diesem Punkt wird Folgendes angezeigt:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Auf die Daten, die an die DetailsView gebunden sind, kann über die `DataItem`-Eigenschaft zugegriffen werden. Denken Sie daran, dass wir unsere Steuerelemente an eine stark typisierte Datentabelle binden, die aus einer Auflistung stark typisierter DataRow-Instanzen besteht. Wenn die Datentabelle an die DetailsView gebunden ist, wird die erste DataRow in der Datentabelle der `DataItem`-Eigenschaft der DetailsView zugewiesen. Insbesondere wird der `DataItem`-Eigenschaft ein `DataRowView` Objekt zugewiesen. Wir können die `Row`-Eigenschaft des `DataRowView`verwenden, um Zugriff auf das zugrunde liegende DataRow-Objekt zu erhalten, das tatsächlich eine `ProductsRow` Instanz ist. Wenn wir diese `ProductsRow` Instanz haben, können wir unsere Entscheidung treffen, indem wir einfach die Eigenschaftswerte des Objekts überprüfen.

Der folgende Code veranschaulicht, wie Sie bestimmen können, ob der `UnitPrice` Wert, der an das DetailsView-Steuerelement gebunden ist, größer als $75,00 ist:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Da `UnitPrice` einen `NULL` Wert in der Datenbank haben kann, stellen wir zuerst sicher, dass wir nicht mit einem `NULL` Wert arbeiten, bevor Sie auf die `UnitPrice`-Eigenschaft des `ProductsRow`zugreifen. Diese Überprüfung ist wichtig, denn wenn wir versuchen, auf die `UnitPrice`-Eigenschaft zuzugreifen, wenn Sie einen `NULL`-Wert aufweist, löst das `ProductsRow`-Objekt eine [StrongTypingException-Ausnahme](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)aus.

## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Schritt 3: Formatieren des UnitPrice-Werts in der DetailsView

An dieser Stelle können wir bestimmen, ob der an die DetailsView gebundene `UnitPrice` Wert einen Wert von $75,00 überschreitet, aber wir haben noch nicht erfahren, wie die Formatierung der DetailsView Programm gesteuert angepasst wird. Um die Formatierung einer gesamten Zeile in der DetailsView zu ändern, greifen Sie Programm gesteuert auf die Zeile mit `DetailsViewID.Rows(index)`; um eine bestimmte Zelle zu ändern, verwenden Sie den Zugriff `DetailsViewID.Rows(index).Cells(index)`. Sobald ein Verweis auf die Zeile oder Zelle vorhanden ist, können wir die Darstellung anpassen, indem Sie die Stil bezogenen Eigenschaften festlegen.

Der programmgesteuerte Zugriff auf eine Zeile erfordert, dass Sie den Index der Zeile kennen, der bei 0 beginnt. Die `UnitPrice` Zeile ist die fünfte Zeile in der DetailsView und gibt ihr einen Index von 4 und ermöglicht den programmgesteuerten Zugriff mithilfe `ExpensiveProductsPriceInBoldItalic.Rows(4)`. An diesem Punkt kann der gesamte Inhalt der Zeile in einer fett formatierten Schriftart angezeigt werden, indem der folgende Code verwendet wird:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Dadurch werden jedoch *sowohl* die Bezeichnung (Preis) als auch der Wert fett und kursiv formatiert. Wenn wir nur den fett formatierten und kursiv formatierten Wert festlegen möchten, müssen wir diese Formatierung auf die zweite Zelle in der Zeile anwenden. Dies kann mit folgendem erreicht werden:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Da unsere Lernprogramme bisher Stylesheets verwendet haben, um eine saubere Trennung zwischen dem gerenderten Markup und den Stil bezogenen Informationen zu gewährleisten, anstatt die spezifischen Stileigenschaften festzulegen, wie oben gezeigt, verwenden wir stattdessen eine CSS-Klasse. Öffnen Sie das `Styles.css` Stylesheet, und fügen Sie eine neue CSS-Klasse mit dem Namen `ExpensivePriceEmphasis` mit der folgenden Definition hinzu:

[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Legen Sie dann im Ereignishandler für `DataBound` die `CssClass`-Eigenschaft der Zelle auf `ExpensivePriceEmphasis`fest. Der folgende Code zeigt den `DataBound` Ereignishandler in seiner Gesamtheit:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Beim Anzeigen von Chai, bei dem weniger als $75,00 anfallen, wird der Preis in einer normalen Schriftart angezeigt (siehe Abbildung 4). Wenn Sie jedoch Mishi Kobe niku mit einem Preis von $97,00 anzeigen, wird der Preis in einer fett formatierten Schriftart angezeigt (siehe Abbildung 5).

[![Preise, die kleiner als $75,00 sind, werden in einer normalen Schriftart angezeigt.](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Abbildung 4**: Preise, die kleiner als $75,00 sind, werden in einer normalen Schriftart angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image10.png))

[die Preise ![teuren Produkte werden in einer fett formatierten Schriftart angezeigt.](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Abbildung 5**: die Preise für teure Produkte werden in fett formatierter Schrift angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image13.png))

## <a name="using-the-formview-controlsdataboundevent-handler"></a>Verwenden des`DataBound`Ereignis Handlers des FormView-Steuer Elements

Die Schritte zum Bestimmen der zugrunde liegenden Daten, die an eine FormView gebunden sind, sind identisch mit denen für eine DetailsView Erstellen eines `DataBound`-Ereignis Handlers, Umwandeln der `DataItem`-Eigenschaft in den entsprechenden Objekttyp, der an das Steuerelement gebunden ist Die Form View und DetailsView unterscheiden sich jedoch in der Aktualisierung der Darstellung Ihrer Benutzeroberfläche.

Die FormView enthält keine boundfields und verfügt daher nicht über die `Rows` Auflistung. Stattdessen besteht eine FormView aus Vorlagen, die eine Mischung aus statischem HTML, websteuer Elementen und Datenbindung-Syntax enthalten können. Das Anpassen des Formats einer Form Ansicht umfasst in der Regel das Anpassen des Stils von einem oder mehreren websteuer Elementen in den Vorlagen von FormView.

Um dies zu veranschaulichen, verwenden wir eine FormView, um Produkte wie im vorherigen Beispiel aufzulisten, aber dieses Mal zeigen wir nur den Produktnamen und die Einheiten im Lager an, wobei die Einheiten in einer roten Schriftart angezeigt werden, wenn diese kleiner oder gleich 10 ist.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Schritt 4: Anzeigen der Produktinformationen in einer FormView

Fügen Sie der `CustomColors.aspx` Seite unterhalb der DetailsView eine FormView hinzu, und legen Sie die Eigenschaft `ID` auf `LowStockedProductsInRed`fest. Binden Sie die FormView an das ObjectDataSource-Steuerelement, das im vorherigen Schritt erstellt wurde. Dadurch wird eine `ItemTemplate`, `EditItemTemplate`und `InsertItemTemplate` für die FormView erstellt. Entfernen Sie die `EditItemTemplate` und `InsertItemTemplate`, und vereinfachen Sie die `ItemTemplate`, um nur die `ProductName`-und `UnitsInStock` Werte einzuschließen, jeweils in ihren eigenen entsprechend benannten Bezeichnung-Steuerelementen. Aktivieren Sie wie bei der DetailsView aus dem vorherigen Beispiel auch das Kontrollkästchen Paging aktivieren im Smarttag von FormView.

Nachdem diese Änderungen vorgenommen wurden, sollte das Markup der FormView in etwa wie folgt aussehen:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Beachten Sie, dass die `ItemTemplate` Folgendes enthält:

- **Statischer HTML** -Text "Product:" und "Units in Stock:" zusammen mit den `<br />`-und `<b>` Elementen.
- **Web steuert** die zwei Label-Steuerelemente, `ProductNameLabel` und `UnitsInStockLabel`.
- **DataBinding-Syntax** die Syntax von `<%# Bind("ProductName") %>` und `<%# Bind("UnitsInStock") %>`, die die Werte aus diesen Feldern den `Text` Eigenschaften der Label-Steuerelemente zuweist.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Schritt 5: Programm gesteuertes bestimmen des Werts der Daten im Daten gebundenen Ereignis Handler

Wenn das Markup von FormView fertiggestellt ist, besteht der nächste Schritt darin, Programm gesteuert zu ermitteln, ob der `UnitsInStock` Wert kleiner oder gleich 10 ist. Dies erfolgt auf die gleiche Weise wie bei der FormView, wie es bei der DetailsView der Fall war. Beginnen Sie mit dem Erstellen eines Ereignis Handlers für das `DataBound`-Ereignis der FormView.

![Erstellen des Daten gebundenen Ereignis Handlers](custom-formatting-based-upon-data-vb/_static/image14.png)

**Abbildung 6**: Erstellen des `DataBound` Ereignis Handlers

Wandeln Sie im Ereignishandler die `DataItem`-Eigenschaft von FormView in eine `ProductsRow` Instanz um, und stellen Sie fest, ob der `UnitsInPrice` Wert so ist, dass wir ihn in einer roten Schriftart anzeigen müssen.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Schritt 6: Formatieren des unitsinstocklabel-Bezeichnung-Steuer Elements in der "ItemTemplate" von FormView

Der letzte Schritt besteht darin, den angezeigten `UnitsInStock` Wert in einer roten Schriftart zu formatieren, wenn der Wert 10 oder kleiner ist. Um dies zu erreichen, müssen wir Programm gesteuert auf das `UnitsInStockLabel`-Steuerelement im `ItemTemplate` zugreifen und seine Stileigenschaften so festlegen, dass der Text rot angezeigt wird. Um auf ein websteuer Element in einer Vorlage zuzugreifen, verwenden Sie die `FindControl("controlID")`-Methode wie folgt:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

In unserem Beispiel möchten wir auf ein Label-Steuerelement zugreifen, dessen `ID` Wert `UnitsInStockLabel`ist. Daher verwenden wir Folgendes:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Sobald ein Programm gesteuerter Verweis auf das websteuer Element vorhanden ist, können wir die Stil bezogenen Eigenschaften nach Bedarf ändern. Wie im obigen Beispiel habe ich eine CSS-Klasse in `Styles.css` namens "`LowUnitsInStockEmphasis`" erstellt. Wenn Sie diesen Stil auf das Bezeichnungs-websteuer Element anwenden möchten, legen Sie dessen `CssClass`-Eigenschaft entsprechend fest

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Die Syntax für das Formatieren einer Vorlage, die den programmgesteuerten Zugriff auf das websteuer Element mithilfe `FindControl("controlID")` und anschließendes Festlegen der Stil bezogenen Eigenschaften verwendet, kann auch verwendet werden, wenn [templatefields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) in den DetailsView-oder GridView-Steuerelementen verwendet wird Wir untersuchen templatefields im nächsten Tutorial.

Abbildung 7 zeigt die Form Ansicht beim Anzeigen eines Produkts, dessen `UnitsInStock` Wert größer als 10 ist, während das Produkt in Abbildung 8 den Wert kleiner als 10 hat.

[![für Produkte mit ausreichend großen vorrätigen Einheiten wird keine benutzerdefinierte Formatierung angewendet.](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Abbildung 7**: für Produkte mit ausreichend großen vorrätigen Einheiten wird keine benutzerdefinierte Formatierung angewendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image17.png))

[![die Einheiten in der aktiennummer für diese Produkte mit Werten von 10 oder weniger rot angezeigt.](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Abbildung 8**: die Einheiten in der aktiennummer werden für diese Produkte rot mit Werten von 10 oder weniger angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image20.png)).

## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatieren mit dem`RowDataBound`Ereignis der GridView

Früher haben wir die Abfolge der Schritte untersucht, die die DetailsView und FormView während der Datenbindung durchläuft. Betrachten wir diese Schritte erneut als Auffrischung.

1. Das `DataBinding` Ereignis des Data Web-Steuer Elements wird ausgelöst.
2. Die Daten werden an das datenweb-Steuerelement gebunden.
3. Das `DataBound` Ereignis des Data Web-Steuer Elements wird ausgelöst.

Diese drei einfachen Schritte sind für die DetailsView und FormView ausreichend, da nur ein einzelner Datensatz angezeigt wird. In der GridView, in der *alle* an Sie gebundenen Datensätze (nicht nur die ersten) angezeigt werden, ist Schritt 2 etwas mehr beteiligt.

In Schritt 2 wird die Datenquelle von GridView aufgelistet, und für jeden Datensatz wird eine `GridViewRow` Instanz erstellt und der aktuelle Datensatz an ihn gebunden. Für jede `GridViewRow`, die der GridView hinzugefügt wird, werden zwei Ereignisse ausgelöst:

- **`RowCreated`** ausgelöst, nachdem die `GridViewRow` erstellt wurde.
- **`RowDataBound`** ausgelöst, nachdem der aktuelle Datensatz an den `GridViewRow`gebunden wurde.

Für GridView wird die Datenbindung in der folgenden Schrittfolge genauer beschrieben:

1. Das `DataBinding` Ereignis der GridView wird ausgelöst.
2. Die Daten werden an die GridView gebunden.   
  
   Für jeden Datensatz in der Datenquelle 

    1. Erstellen eines `GridViewRow` Objekts
    2. `RowCreated` Ereignis auslösen
    3. Binden Sie den Datensatz an den `GridViewRow`
    4. `RowDataBound` Ereignis auslösen
    5. Fügen Sie die `GridViewRow` der `Rows` Sammlung hinzu.
3. Das `DataBound` Ereignis der GridView wird ausgelöst.

Um das Format der einzelnen Datensätze der GridView anzupassen, müssen wir einen Ereignishandler für das `RowDataBound`-Ereignis erstellen. Um dies zu veranschaulichen, fügen wir der Seite "`CustomColors.aspx`" eine GridView hinzu, die den Namen, die Kategorie und den Preis für jedes Produkt auflistet und die Produkte hervorhebt, deren Preis kleiner ist als $10,00 mit einer gelben Hintergrundfarbe.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Schritt 7: Anzeigen von Produktinformationen in einer GridView

Fügen Sie unter der Form Ansicht aus dem vorherigen Beispiel eine GridView hinzu, und legen Sie die `ID`-Eigenschaft auf `HighlightCheapProducts`fest. Da wir bereits über eine ObjectDataSource verfügen, die alle Produkte auf der Seite zurückgibt, binden Sie die GridView an diese. Bearbeiten Sie abschließend die boundfields-Struktur der GridView, um nur die Namen, Kategorien und Preise der Produkte einzubeziehen. Nachdem Sie diese Änderungen vorgenommen haben, sollte das Markup der GridView wie folgt aussehen:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Abbildung 9 zeigt den Fortschritt bis zu diesem Punkt, wenn er über einen Browser angezeigt wird.

[![GridView listet den Namen, die Kategorie und den Preis für jedes Produkt auf.](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Abbildung 9**: die GridView listet den Namen, die Kategorie und den Preis für jedes Produkt auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image23.png))

## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Schritt 8: Programm gesteuertes bestimmen des Werts der Daten im rowdatin-Ereignis Handler

Wenn die `ProductsDataTable` an das GridView-Element gebunden ist, werden die `ProductsRow` Instanzen aufgelistet, und für jedes `ProductsRow` wird eine `GridViewRow` erstellt. Die `DataItem`-Eigenschaft des `GridViewRow`wird der jeweiligen `ProductRow`zugewiesen, nach der der `RowDataBound` Ereignishandler der GridView ausgelöst wird. Um den `UnitPrice` Wert für jedes Produkt zu ermitteln, das an die GridView gebunden ist, müssen wir einen Ereignishandler für das `RowDataBound`-Ereignis der GridView erstellen. In diesem Ereignishandler können wir den `UnitPrice` Wert für die aktuelle `GridViewRow` überprüfen und eine Formatierungs Entscheidung für diese Zeile treffen.

Dieser Ereignishandler kann mit den gleichen Schritten wie mit der Form View und der DetailsView erstellt werden.

![Erstellen eines Ereignis Handlers für das rowdatabo-Ereignis von GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Abbildung 10**: Erstellen eines Ereignis Handlers für das `RowDataBound` Ereignis der GridView

Wenn Sie den Ereignishandler auf diese Weise erstellen, wird der folgende Code automatisch dem Codeteil der ASP.NET-Seite hinzugefügt:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Wenn das `RowDataBound`-Ereignis ausgelöst wird, wird der Ereignishandler als zweiter Parameter an ein Objekt vom Typ `GridViewRowEventArgs`übergeben, das über eine Eigenschaft mit dem Namen `Row`verfügt. Diese Eigenschaft gibt einen Verweis auf den `GridViewRow` zurück, der lediglich Daten gebunden war. Um auf die `ProductsRow` Instanz zuzugreifen, die an den `GridViewRow` gebunden ist, verwenden wir die `DataItem`-Eigenschaft wie folgt:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Beim Arbeiten mit dem `RowDataBound`-Ereignishandler ist es wichtig zu beachten, dass die GridView aus unterschiedlichen Zeilen Typen besteht und dass dieses Ereignis für *alle* Zeilen Typen ausgelöst wird. Der Typ eines `GridViewRow`kann durch seine `RowType`-Eigenschaft bestimmt werden und kann einen der möglichen Werte aufweisen:

- `DataRow` eine Zeile, die vom `DataSource` der GridView an einen Datensatz gebunden ist.
- `EmptyDataRow` die Zeile, die angezeigt wird, wenn die `DataSource` der GridView leer ist.
- `Footer` der Footerzeile. wird angezeigt, wenn die `ShowFooter`-Eigenschaft der GridView auf festgelegt ist `True`
- die Kopfzeile `Header`. wird angezeigt, wenn die ShowHeader-Eigenschaft der GridView auf `True` festgelegt ist (Standard).
- `Pager` für GridView, das Paging implementiert, die Zeile, in der die Paging-Schnittstelle angezeigt wird.
- `Separator` nicht für die GridView verwendet, sondern von den `RowType` Eigenschaften für die DataList-und Repeater-Steuerelemente verwendet, zwei datenweb Steuerelemente, die in zukünftigen Tutorials besprochen werden.

Da die `EmptyDataRow`-, `Header`-, `Footer`-und `Pager` Zeilen keinem `DataSource` Datensatz zugeordnet sind, haben Sie immer den Wert `Nothing` für Ihre `DataItem`-Eigenschaft. Aus diesem Grund müssen wir vor dem Versuch, mit der `DataItem`-Eigenschaft des aktuellen `GridViewRow`zu arbeiten, zunächst sicherstellen, dass wir mit einem `DataRow`arbeiten. Dies kann erreicht werden, indem Sie die `RowType`-Eigenschaft des `GridViewRow`wie folgt überprüfen:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Schritt 9: Markieren der Zeile gelb, wenn der UnitPrice-Wert kleiner als $10,00 ist

Der letzte Schritt besteht darin, die gesamte `GridViewRow` Programm gesteuert hervorzuheben, wenn der `UnitPrice` Wert für diese Zeile kleiner als $10,00 ist. Die Syntax für den Zugriff auf die Zeilen oder Zellen einer GridView-Sicht ist identisch mit der DetailsView-`GridViewID.Rows(index)`, um auf die gesamte Zeile zuzugreifen `GridViewID.Rows(index).Cells(index)`, um auf eine bestimmte Zelle zuzugreifen. Wenn der `RowDataBound`-Ereignishandler jedoch die Daten gebundene `GridViewRow` auslöst, muss der `Rows` Auflistung von GridView noch hinzugefügt werden. Daher können Sie nicht über den `RowDataBound`-Ereignishandler auf die aktuelle `GridViewRow` Instanz mithilfe der Rows-Auflistung zugreifen.

Anstelle `GridViewID.Rows(index)`können wir mit `e.Row`auf die aktuelle `GridViewRow` Instanz im `RowDataBound`-Ereignishandler verweisen. Das heißt, um die aktuelle `GridViewRow` Instanz aus dem `RowDataBound`-Ereignishandler hervorzuheben, verwenden wir Folgendes:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Anstatt die `BackColor`-Eigenschaft des `GridViewRow`direkt festzulegen, wollen wir uns mit der Verwendung von CSS-Klassen anhalten. Ich habe eine CSS-Klasse mit dem Namen `AffordablePriceEmphasis` erstellt, mit der die Hintergrundfarbe auf Gelb festgelegt wird. Der abgeschlossene Ereignishandler für `RowDataBound` folgt:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]

[![die am günstigsten Esten Produkte gelb hervorgehoben](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Abbildung 11**: die günstigsten Produkte sind gelb hervorgehoben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](custom-formatting-based-upon-data-vb/_static/image27.png))

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie GridView, DetailsView und FormView basierend auf den an das Steuerelement gebundenen Daten formatieren. Um dies zu erreichen, haben wir einen Ereignishandler für die `DataBound`-oder `RowDataBound` Ereignisse erstellt, bei denen die zugrunde liegenden Daten bei Bedarf zusammen mit einer Formatierungs Änderung überprüft wurden. Um auf die Daten zuzugreifen, die an eine DetailsView oder FormView gebunden sind, verwenden wir die `DataItem`-Eigenschaft im `DataBound`-Ereignishandler. für eine GridView enthält jede `DataItem`-Eigenschaft der `GridViewRow` Instanz die Daten, die an diese Zeile gebunden sind, die im `RowDataBound`-Ereignishandler verfügbar ist.

Die Syntax für die programmgesteuerte Anpassung der Formatierung des Daten websteuer Elements hängt vom websteuer Element und der Anzeige der zu formatierenden Daten ab. Für DetailsView-und GridView-Steuerelemente kann über einen Ordinalindex auf die Zeilen und Zellen zugegriffen werden. Bei der FormView, in der Vorlagen verwendet werden, wird die `FindControl("controlID")`-Methode häufig zum Suchen eines websteuer Elements in der Vorlage verwendet.

Im nächsten Tutorial wird erläutert, wie Vorlagen mit GridView und DetailsView verwendet werden. Außerdem wird ein weiteres Verfahren zum Anpassen der Formatierung basierend auf den zugrunde liegenden Daten angezeigt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial waren E.R. Gilmore, Dennis Patterson und Dan Jager. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Weiter](using-templatefields-in-the-gridview-control-vb.md)
