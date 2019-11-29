---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Verwenden von templatefields im DetailsView-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die gleichen templatefields-Funktionen, die in der GridView verfügbar sind, stehen auch im DetailsView-Steuerelement zur Verfügung. In diesem Tutorial wird ein Produkt angezeigt...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e96f954c27ae1c8ccc18a9c40fe7e541b487c1cc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624942"
---
# <a name="using-templatefields-in-the-detailsview-control-vb"></a>Verwenden von TemplateFields im DetailsView-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) oder [PDF herunterladen](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Die gleichen templatefields-Funktionen, die in der GridView verfügbar sind, stehen auch im DetailsView-Steuerelement zur Verfügung. In diesem Tutorial zeigen wir ein Produkt gleichzeitig mit einer DetailsView, die templatefields enthält.

## <a name="introduction"></a>Einführung

Das TemplateField-Steuerelement bietet einen höheren Grad an Flexibilität beim Rendern von Daten als die Steuerelemente BoundField, CheckBoxField, HyperLinkField und andere Datenfeld-Steuerelemente. Im [vorherigen Tutorial](using-templatefields-in-the-gridview-control-vb.md) haben wir uns mit der Verwendung von TemplateField in einem GridView-Element beschäftigt:

- Zeigen Sie mehrere Daten Feldwerte in einer Spalte an. Insbesondere wurden die Felder "`FirstName`" und "`LastName`" zu einer GridView-Spalte zusammengefasst.
- Verwenden Sie ein alternatives websteuer Element, um einen Daten Feldwert auszudrücken. Wir haben gesehen, wie Sie den `HiredDate` Wert mit einem Kalender Steuerelement anzeigen.
- Anzeigen von Statusinformationen auf Grundlage der zugrunde liegenden Daten. Obwohl die `Employees` Tabelle keine Spalte enthält, die die Anzahl der Tage zurückgibt, in denen ein Mitarbeiter für den Auftrag tätig war, konnten diese Informationen im GridView-Beispiel im vorherigen Tutorial mit der Verwendung einer TemplateField-und einer Formatierungs Methode angezeigt werden.

Die gleichen templatefields-Funktionen, die in der GridView verfügbar sind, stehen auch im DetailsView-Steuerelement zur Verfügung. In diesem Tutorial zeigen wir ein Produkt gleichzeitig mit einer DetailsView an, die zwei templatefields enthält. Im ersten TemplateField werden die Datenfelder `UnitPrice`, `UnitsInStock`und `UnitsOnOrder` in einer DetailsView-Zeile kombiniert. Das zweite TemplateField-Element zeigt den Wert des `Discontinued` Felds an, verwendet jedoch eine Formatierungs Methode, um "yes" anzuzeigen, wenn `Discontinued` `True`ist, andernfalls "No".

[![zwei templatefields-Vorlagen zum Anpassen der Anzeige verwendet.](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Abbildung 1**: zwei templatefields-Zeichen werden verwendet, um die Anzeige anzupassen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))

Fangen wir an!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Schritt 1: Binden der Daten an die DetailsView

Wie bereits im vorherigen Tutorial erläutert, ist es beim Arbeiten mit templatefields häufig am einfachsten, das DetailsView-Steuerelement zu erstellen, das nur boundfields enthält, und dann neue templatefields-Elemente hinzuzufügen oder die vorhandenen boundfields-Elemente in templatefields nach Bedarf zu konvertieren. . Starten Sie daher dieses Tutorial, indem Sie der Seite über den Designer eine DetailsView hinzufügen und diese an eine ObjectDataSource binden, die die Liste der Produkte zurückgibt. Mit diesen Schritten wird eine DetailsView mit boundfields für jedes der nicht booleschen wertefelder des Produkts und ein CheckBoxField für das einboolesche Wertfeld (eingestellt) erstellt.

Öffnen Sie die Seite `DetailsViewTemplateField.aspx`, und ziehen Sie eine DetailsView aus der Toolbox auf den Designer. Wählen Sie im Smarttag der DetailsView ein neues ObjectDataSource-Steuerelement aus, das die `GetProducts()` Methode der `ProductsBLL` Klasse aufruft.

[![ein neues ObjectDataSource-Steuerelement hinzufügen, das die GetProducts ()-Methode aufruft.](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen eines neuen ObjectDataSource-Steuer Elements, das die `GetProducts()`-Methode aufruft ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))

Entfernen Sie für diesen Bericht die `ProductID`, `SupplierID`, `CategoryID`und `ReorderLevel` boundfields. Ordnen Sie dann die boundfields-Eigenschaft so an, dass die `CategoryName`-und `SupplierName` boundfields unmittelbar nach dem `ProductName` BoundField angezeigt werden. Sie können die `HeaderText` Eigenschaften und Formatierungs Eigenschaften für boundfields beliebig anpassen. Wie bei der GridView können diese Bearbeitungen auf BoundField-Ebene über das Dialogfeld "Felder" durchgeführt werden (Zugriff durch Klicken auf den Link "Felder bearbeiten" im Smarttags der DetailsView) oder über die deklarative Syntax. Löschen Sie abschließend die Eigenschaften Werte `Height` und `Width` der DetailsView, damit das DetailsView-Steuerelement basierend auf den angezeigten Daten erweitert werden kann, und aktivieren Sie das Kontrollkästchen Paging aktivieren im Smarttags.

Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup Ihres DetailsView-Steuer Elements in etwa wie folgt aussehen:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um die Seite in einem Browser anzuzeigen. An diesem Punkt sollten Sie ein einzelnes Produkt mit den Zeilen "Name", "Category", "Supplier", "Price", "Units in Stock", "Units On Order" und "nicht unterstützt" anzeigen.

[![die Produkt Details mithilfe einer Reihe von boundfields angezeigt werden.](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Abbildung 3**: die Details des Produkts werden mithilfe einer Reihe von boundfields angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))

## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Schritt 2: Kombinieren von Preis, Einheiten in Aktien und Einheiten in der richtigen Reihenfolge in einer Zeile

Die DetailsView enthält eine Zeile für die Felder `UnitPrice`, `UnitsInStock`und `UnitsOnOrder`. Wir können diese Datenfelder in einer einzelnen Zeile mit einem TemplateField-Element kombinieren, indem Sie entweder ein neues TemplateField-Element hinzufügen oder eine der vorhandenen `UnitPrice`-, `UnitsInStock`-und `UnitsOnOrder` boundfields-Werte in ein TemplateField-Element konvertieren. Ich ziehe es vor, vorhandene boundfields zu konvertieren, lassen Sie uns jedoch ein neues TemplateField-Element hinzufügen.

Klicken Sie zunächst im Smarttag der DetailsView auf den Link Felder bearbeiten, um das Dialogfeld Felder anzuzeigen. Fügen Sie als nächstes ein neues TemplateField-Element hinzu, und legen Sie dessen `HeaderText`-Eigenschaft auf "Price and Inventory" (Preis und Inventar) fest, und verschieben Sie das neue TemplateField-Element, sodass es oberhalb der `UnitPrice`

[![dem DetailsView-Steuerelement ein neues TemplateField-Steuerelement hinzufügen](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Abbildung 4**: Hinzufügen eines neuen TemplateField zum DetailsView-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))

Da dieses neue TemplateField die Werte enthält, die derzeit in den `UnitPrice`, `UnitsInStock`und `UnitsOnOrder` boundfields angezeigt werden, entfernen Sie Sie.

Die letzte Aufgabe für diesen Schritt besteht darin, das `ItemTemplate` Markup für den Preis-und Inventur-TemplateField zu definieren. Dies kann entweder über die Vorlagen Bearbeitungs Schnittstelle von DetailsView im Designer oder durch Hand über die deklarative Syntax des Steuer Elements erfolgen. Wie auch bei GridView kann auf die Vorlagen Bearbeitungs Schnittstelle der DetailsView zugegriffen werden, indem Sie auf den Link Vorlagen bearbeiten im Smarttags klicken. Von hier aus können Sie die zu bearbeitende Vorlage in der Dropdown Liste auswählen und dann alle websteuer Elemente aus der Toolbox hinzufügen.

Fügen Sie für dieses Tutorial zunächst ein Label-Steuerelement zum `ItemTemplate`der Preis-und Bestands Ansicht von TemplateField hinzu. Klicken Sie anschließend im Smarttags des Beschriftungs websteuer Elements auf den Link DataBindings bearbeiten, und binden Sie die `Text`-Eigenschaft an das `UnitPrice` Feld.

[![die Text-Eigenschaft der Bezeichnung an das UnitPrice-Datenfeld binden.](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Abbildung 5**: Binden der `Text`-Eigenschaft der Bezeichnung an das `UnitPrice` Datenfeld ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))

## <a name="formatting-the-price-as-a-currency"></a>Formatieren des Preises als Währung

Mit dieser Ergänzung wird in der Bezeichnung "Web Control Price" und "Inventory TemplateField" nun nur der Preis für das ausgewählte Produkt angezeigt. Abbildung 6 zeigt einen Screenshot dieses Fortschritts, der in einem Browser angezeigt wird.

[![der Preis-und Inventar-TemplateField den Preis](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Abbildung 6**: der Preis und das Inventar-TemplateField zeigt den Preis[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))

Beachten Sie, dass der Preis des Produkts nicht als Währung formatiert ist. Bei einem BoundField ist die Formatierung möglich, indem die `HtmlEncode`-Eigenschaft auf `False` und die `DataFormatString`-Eigenschaft auf `{0:formatSpecifier}`festgelegt wird. Für ein TemplateField-Element müssen jedoch alle Formatierungs Anweisungen in der Datenbindung-Syntax oder durch die Verwendung einer Formatierungs Methode angegeben werden, die an einer beliebigen Stelle im Code der Anwendung definiert ist (z. b. in der Code Behind-Klasse der ASP.NET-Seite).

Wenn Sie die Formatierung für die im Label-websteuer Element verwendete Datenbindung-Syntax angeben möchten, kehren Sie zum Dialogfeld DataBindings zurück, indem Sie auf den Link DataBindings bearbeiten des Smarttags der Bezeichnung klicken. Sie können die Formatierungs Anweisungen direkt in der Dropdown Liste Format eingeben oder eine der definierten Format Zeichenfolgen auswählen. Wie bei der `DataFormatString` Eigenschaft von BoundField wird die Formatierung mithilfe `{0:formatSpecifier}`angegeben.

Verwenden Sie für das Feld `UnitPrice` die Währungs Formatierung, die Sie entweder durch Auswählen des entsprechenden Dropdown Listen Werts oder durch Eingabe von `{0:C}` per Hand angegeben haben.

[der Preis wird ![als Währung formatiert.](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Abbildung 7**: Formatieren des Preises als Währung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))

Deklarativ ist die Formatierungs Spezifikation als zweiter Parameter in den `Bind`-oder `Eval`-Methoden angegeben. Die soeben durch den Designer vorgenommenen Einstellungen führen zu folgendem Datenbindung-Ausdruck im deklarativen Markup:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Hinzufügen der restlichen Datenfelder zum TemplateField

An dieser Stelle haben wir das Feld `UnitPrice` Daten im Feld Price and Inventory TemplateField angezeigt und formatiert, müssen jedoch weiterhin die Felder `UnitsInStock` und `UnitsOnOrder` anzeigen. Wir zeigen diese in einer Zeile unter dem Preis und in Klammern an. Über die Vorlagen Bearbeitungs Schnittstelle im Designer kann dieses Markup hinzugefügt werden, indem Sie den Cursor in der Vorlage positionieren und einfach den Text eingeben, der angezeigt werden soll. Alternativ kann dieses Markup direkt in die deklarative Syntax eingegeben werden.

Fügen Sie das statische Markup, Beschriftungs-websteuer Elemente und die Datenbindung-Syntax hinzu, sodass der Preis und die Inventur im TemplateField-Steuerelement wie folgt angezeigt werden:

*UnitPrice*  
(**In Stock/on-Order:** *UnitsInStock* / *UnitsOnOrder*)

Nachdem Sie diese Aufgabe durchgeführt haben, sollte das deklarative Markup der DetailsView in etwa wie folgt aussehen:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Mit diesen Änderungen haben wir den Preis und die Inventur Informationen in einer einzelnen DetailsView-Zeile konsolidiert.

[![der Preis und die Inventur Informationen werden in einer einzelnen Zeile angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Abbildung 8**: die Preis-und Inventur Informationen werden in einer einzelnen Zeile angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))

## <a name="step-3-customizing-the-discontinued-field-information"></a>Schritt 3: Anpassen der nicht mehr unterstützten Feldinformationen

Die `Discontinued` Spalte der `Products` Tabelle ist ein Bitwert, der angibt, ob das Produkt eingestellt wurde. Wenn eine DetailsView (oder GridView) an ein Datenquellen Steuerelement gebunden wird, werden die booleschen wertefelder wie `Discontinued`als checkboxfields implementiert, wohingegen nicht boolesche Wert Felder, wie `ProductID`, `ProductName`usw., als boundfields implementiert werden. Das CheckBoxField wird als deaktiviertes Kontrollkästchen gerendert, das überprüft wird, wenn der Wert des Daten Felds true und andernfalls deaktiviert ist.

Anstatt das CheckBoxField anzuzeigen, können Sie stattdessen Text anzeigen, der angibt, ob das Produkt nicht mehr unterstützt wird. Um dies zu erreichen, können wir das CheckBoxField aus der DetailsView entfernen und dann ein BoundField-Objekt hinzufügen, dessen `DataField`-Eigenschaft auf `Discontinued`festgelegt wurde. Nehmen Sie sich einen Moment Zeit, um dies zu tun. Nach dieser Änderung zeigt die DetailsView den Text "true" für nicht mehr unterstützte Produkte und "false" für Produkte an, die noch aktiv sind.

[![die Zeichen folgen "true" und "false" verwendet, um den nicht unterstützten Zustand](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Abbildung 9**: die Zeichen folgen "true" und "false" werden zum Anzeigen des nicht unterstützten Zustands verwendet ([Klicken Sie, um das Bild in voller Größe](using-templatefields-in-the-detailsview-control-vb/_static/image27.png)anzuzeigen

Stellen Sie sich vor, dass die Zeichen folgen "true" oder "false" nicht verwendet werden sollen, sondern "yes" und "No". Diese Anpassung kann mit der Unterstützung einer TemplateField-Methode und einer Formatierungs Methode ausgeführt werden. Eine Formatierungs Methode kann eine beliebige Anzahl von Eingabe Parametern annehmen, muss jedoch den HTML-Code (als Zeichenfolge) zurückgeben, um Sie in die Vorlage einzufügen.

Fügen Sie der Code Behind-Klasse der `DetailsViewTemplateField.aspx` Seite eine Formatierungs Methode mit dem Namen `DisplayDiscontinuedAsYESorNO` hinzu, die ein `Northwind.ProductsRow`-Objekt als Eingabeparameter akzeptiert und eine Zeichenfolge zurückgibt. Wie bereits im vorherigen Tutorial erläutert, *muss* diese Methode als `Protected` oder `Public` gekennzeichnet werden, damit Sie über die Vorlage zugänglich ist.

[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Diese Methode überprüft den Eingabeparameter (`discontinued`) und gibt "yes" zurück, wenn er `True`ist, andernfalls "No".

> [!NOTE]
> In der Formatierungs Methode, die im vorherigen Tutorial untersucht wurde, wurde ein Datenfeld übergeben, das möglicherweise `NULL` s enthält. daher musste überprüft werden, ob der Wert des `HiredDate`-Eigenschafts Werts für den Mitarbeiter über eine Datenbank `NULL` Wert verfügte, bevor auf die `HiredDate`-Eigenschaft des `EmployeesRow`zugegriffen wurde. Eine solche Überprüfung ist hier nicht erforderlich, da der `Discontinued` Spalte niemals Daten Bank `NULL` Werte zugewiesen werden können. Außerdem kann die Methode einen booleschen Eingabeparameter akzeptieren, anstatt eine `ProductsRow` Instanz oder einen Parameter vom Typ "`Object`" zu akzeptieren.

Wenn diese Formatierungs Methode vollständig ist, muss nur noch aus der `ItemTemplate`von TemplateField aufgerufen werden. Um das TemplateField zu erstellen, entfernen Sie entweder das `Discontinued` BoundField, und fügen Sie ein neues TemplateField-Element hinzu, oder konvertieren Sie das `Discontinued` BoundField in ein TemplateField. Bearbeiten Sie dann in der deklarativen Markup Ansicht das TemplateField, sodass es nur ein ItemTemplate-Element enthält, das die `DisplayDiscontinuedAsYESorNO`-Methode aufruft, und übergeben Sie den Wert der `Discontinued`-Eigenschaft der aktuellen `ProductRow` Instanz. Auf diese kann über die `Eval`-Methode zugegriffen werden. Insbesondere das Markup von TemplateField sollte wie folgt aussehen:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Dies bewirkt, dass die `DisplayDiscontinuedAsYESorNO`-Methode aufgerufen wird, wenn die DetailsView gerendert wird, wobei der `Discontinued` Wert der `ProductRow` Instanz übergeben wird. Da die `Eval`-Methode einen Wert vom Typ `Object`zurückgibt, die `DisplayDiscontinuedAsYESorNO`-Methode jedoch einen Eingabeparameter vom Typ `Boolean`erwartet, wird der Rückgabewert der `Eval` Methoden in `Boolean`umgewandelt. Die `DisplayDiscontinuedAsYESorNO`-Methode gibt dann abhängig vom empfangenen Wert "yes" oder "No" zurück. Der zurückgegebene Wert ist das, was in dieser DetailsView-Zeile angezeigt wird (siehe Abbildung 10).

[![ja oder keine Werte werden jetzt in der nicht mehr unterstützten Zeile angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Abbildung 10**: Ja oder keine Werte werden jetzt in der nicht mehr unterstützten Zeile angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))

## <a name="summary"></a>Summary

Das TemplateField im DetailsView-Steuerelement ermöglicht ein höheres Maß an Flexibilität beim Anzeigen von Daten, als für die anderen Feld Steuerelemente verfügbar sind, und eignet sich ideal für Situationen, in denen Folgendes gilt:

- In einer GridView-Spalte müssen mehrere Datenfelder angezeigt werden.
- Die Daten werden am besten mithilfe eines websteuer Elements anstelle von nur-Text ausgedrückt.
- Die Ausgabe hängt von den zugrunde liegenden Daten ab, z. b. durch das Anzeigen von Metadaten oder das Neuformatieren der Daten.

Während templatefields einen höheren Grad an Flexibilität beim Rendern der zugrunde liegenden Daten der DetailsView ermöglicht, spürt die DetailsView-Ausgabe nach wie vor ein Bit-boxy, da jedes Feld als Zeile in einer HTML-`<table>`gerendert wird.

Das FormView-Steuerelement bietet ein höheres Maß an Flexibilität beim Konfigurieren der gerenderten Ausgabe. Die FormView enthält keine Felder, sondern nur eine Reihe von Vorlagen (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`usw.). In unserem nächsten Tutorial wird erläutert, wie Sie die FormView verwenden, um eine noch bessere Kontrolle über das gerenderte Layout zu erhalten.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Dan Jagers. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-templatefields-in-the-gridview-control-vb.md)
> [Weiter](using-the-formview-s-templates-vb.md)
