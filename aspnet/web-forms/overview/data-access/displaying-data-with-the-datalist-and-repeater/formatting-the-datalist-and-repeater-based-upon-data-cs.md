---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formatieren von DataList und Repeater auf der GrundlageC#von Daten () | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden Beispiele für das Formatieren der Darstellung der DataList-und Repeater-Steuerelemente mithilfe von Formatierungsfunktionen mit...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 68de3450ed97fc7bd0efb27e089d9db8e3e85fb0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633245"
---
# <a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formatieren des DataList- und Wiederholungssteuerelements auf Datenbasis (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) oder [PDF herunterladen](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> In diesem Tutorial werden Beispiele für das Formatieren der Darstellung der DataList-und Repeater-Steuerelemente erläutert, entweder mithilfe von Formatierungsfunktionen in Vorlagen oder durch Behandeln des Daten gebundenen Ereignisses.

## <a name="introduction"></a>Einführung

Wie wir im vorherigen Tutorial gesehen haben, bietet der DataList eine Reihe von Stil bezogenen Eigenschaften, die sich auf seine Darstellung auswirken. Insbesondere wurde erläutert, wie den Eigenschaften `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`und `SelectedItemStyle` von DataList Standard-CSS-Klassen zugewiesen werden. Zusätzlich zu diesen vier Eigenschaften enthält der DataList eine Reihe von anderen Stil bezogenen Eigenschaften, z. b. `Font`, `ForeColor`, `BackColor`und `BorderWidth`, um nur einige zu nennen. Das Repeater-Steuerelement enthält keine Stil bezogenen Eigenschaften. Alle diese Stileinstellungen müssen direkt innerhalb des Markups in den Wiederholungs Vorlagen erstellt werden.

Häufig hängt jedoch von den Daten ab, wie die Daten formatiert werden sollen. Wenn Sie z. b. Produkte auflisten, möchten wir möglicherweise die Produktinformationen in einer hellgrauen Schriftfarbe anzeigen, wenn Sie nicht mehr unterstützt wird, oder Sie möchten den `UnitsInStock` Wert hervorheben, wenn der Wert 0 (null) ist. Wie in den vorherigen Tutorials gezeigt, bieten GridView, DetailsView und FormView zwei verschiedene Möglichkeiten, ihre Darstellung basierend auf Ihren Daten zu formatieren:

- **Das `DataBound`-Ereignis** erstellt einen Ereignishandler für das entsprechende `DataBound`-Ereignis, das ausgelöst wird, nachdem die Daten an die einzelnen Elemente gebunden wurden (für das GridView-Objekt war es das `RowDataBound`-Ereignis; für das DataList-und das Repeater ist es das `ItemDataBound` Ereignis). In diesem Ereignishandler können die Daten, die gerade gebunden sind, überprüft und Formatierungs Entscheidungen getroffen werden. Wir haben dieses Verfahren im Tutorial [benutzerdefinierte Formatierung basierend auf Daten](../custom-formatting/custom-formatting-based-upon-data-cs.md) untersucht.
- **Formatieren von Funktionen in Vorlagen** bei Verwendung von templatefields in den DetailsView-oder GridView-Steuerelementen oder einer Vorlage im FormView-Steuerelement können wir eine Formatierungsfunktion zur Code-Behind-Klasse der ASP.net page s, der Geschäftslogik Ebene oder einer beliebigen anderen Klassenbibliothek hinzufügen, auf die von der Webanwendung aus zugegriffen werden kann. Diese Formatierungsfunktion kann eine beliebige Anzahl von Eingabe Parametern akzeptieren, muss jedoch den HTML-Code zum Rendering in der Vorlage zurückgeben. Formatierungsfunktionen wurden zunächst im Tutorial [Verwenden von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Element untersucht.

Beide Formatierungs Techniken sind mit den Steuerelementen DataList und Repeater verfügbar. In diesem Tutorial werden Beispiele für die Verwendung beider Techniken für beide Steuerelemente erläutert.

## <a name="using-theitemdataboundevent-handler"></a>Verwenden des`ItemDataBound`-Ereignis Handlers

Wenn Daten an einen DataList gebunden werden, entweder aus einem Datenquellen-Steuerelement oder durch Programm gesteuertes Zuweisen von Daten an das Steuerelement s `DataSource`-Eigenschaft und Aufrufen der `DataBind()`-Methode, wird das DataList-`DataBinding`-Ereignis ausgelöst, die Datenquelle aufgezählt und jeder Daten Satz an den DataList gebunden. Der DataList erstellt für jeden Datensatz in der Datenquelle ein [`DataListItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) Objekt, das dann an den aktuellen Datensatz gebunden wird. Während dieses Prozesses löst der DataList zwei Ereignisse aus:

- **`ItemCreated`** ausgelöst, nachdem die `DataListItem` erstellt wurde.
- **`ItemDataBound`** ausgelöst, nachdem der aktuelle Datensatz an das-`DataListItem` gebunden wurde.

Die folgenden Schritte beschreiben den Daten Bindungsprozess für das DataList-Steuerelement.

1. Das DataList s- [`DataBinding` Ereignis](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) wird ausgelöst.
2. Die Daten sind an den DataList gebunden.  
  
   Für jeden Datensatz in der Datenquelle 

    1. Erstellen eines `DataListItem` Objekts
    2. [`ItemCreated` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx) auslösen
    3. Binden Sie den Datensatz an den `DataListItem`
    4. [`ItemDataBound` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx) auslösen
    5. Fügen Sie die `DataListItem` der `Items` Sammlung hinzu.

Beim Binden von Daten an das Repeater-Steuerelement wird die gleiche Abfolge von Schritten durchlaufen. Der einzige Unterschied besteht darin, dass der Repeater [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s verwendet, anstatt `DataListItem` Instanzen erstellt werden.

> [!NOTE]
> Der aufgeweckter Reader hat möglicherweise eine geringfügige Anomalieerkennung zwischen der Abfolge von Schritten bemerkt, die durchlaufen werden, wenn DataList und Repeater an Daten gebunden sind, und wenn die GridView an Daten gebunden ist. Am Ende des Daten Bindungs Prozesses löst das GridView-Ereignis das `DataBound`-Ereignis aus. Allerdings hat weder das DataList-Steuerelement noch das Repeater-Steuerelement ein derartiges Ereignis. Dies liegt daran, dass die DataList-und Repeater-Steuerelemente im ASP.NET 1. x-Zeitrahmen erstellt wurden, bevor das Ereignishandlermuster vor und nach der Ebene üblich geworden ist.

Wie bei GridView besteht eine Option für die Formatierung auf der Grundlage der Daten darin, einen Ereignishandler für das `ItemDataBound`-Ereignis zu erstellen. Dieser Ereignishandler prüft die Daten, die gerade an die `DataListItem` oder `RepeaterItem` gebunden waren, und wirkt sich bei Bedarf auf die Formatierung des Steuer Elements aus.

Für das DataList-Steuerelement können Formatierungs Änderungen für das gesamte Element mithilfe der Eigenschaften des `DataListItem` s-Stils implementiert werden, einschließlich der Standard `Font`, `ForeColor`, `BackColor`, `CssClass`usw. Um die Formatierung bestimmter websteuer Elemente in der DataList-Vorlage zu beeinflussen, müssen wir den Stil dieser websteuer Elemente Programm gesteuert aufrufen und ändern. Wir haben gesehen, wie Sie dies in der *benutzerdefinierten Formatierung basierend auf datentutorial* durchführen. Wie das Repeater-Steuerelement hat die `RepeaterItem`-Klasse keine Stil bezogenen Eigenschaften. Daher müssen alle Stil bezogenen Änderungen, die an einem `RepeaterItem` im `ItemDataBound`-Ereignishandler vorgenommen werden, durch Programm gesteuertes zugreifen auf und Aktualisieren von websteuer Elementen in der Vorlage erfolgen.

Da die `ItemDataBound` Formatierungs Technik für DataList und Repeater praktisch identisch ist, konzentriert sich unser Beispiel auf die Verwendung von DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Schritt 1: Anzeigen von Produktinformationen im DataList

Bevor wir uns über die Formatierung Gedanken machen, erstellen Sie zunächst eine Seite, die einen DataList zum Anzeigen von Produktinformationen verwendet. Im [vorherigen Tutorial](displaying-data-with-the-datalist-and-repeater-controls-cs.md) haben wir einen DataList erstellt, dessen `ItemTemplate` die einzelnen Produktnamen, Kategorie, Lieferant, Menge pro Einheit und Preis angezeigt. Wiederholen Sie diese Funktion hier in diesem Tutorial. Um dies zu erreichen, können Sie entweder den DataList und dessen ObjectDataSource neu erstellen, oder Sie können die Steuerelemente auf der Seite, die Sie im vorherigen Tutorial (`Basics.aspx`) erstellt haben, kopieren und auf der Seite für dieses Tutorial (`Formatting.aspx`) einfügen.

Nachdem Sie die DataList-und ObjectDataSource-Funktionalität von `Basics.aspx` in `Formatting.aspx`repliziert haben, nehmen Sie sich einen Moment Zeit, um die DataList-Eigenschaft `ID` von `DataList1` in eine beschreibende `ItemDataBoundFormattingExample`zu ändern. Sehen Sie sich als nächstes den DataList in einem Browser an. Wie in Abbildung 1 gezeigt, besteht der einzige Formatierungs Unterschied zwischen den einzelnen Produkten darin, dass die Hintergrundfarbe wechselt.

[![die Produkte im DataList-Steuerelement aufgelistet sind.](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Abbildung 1**: die Produkte sind im DataList-Steuerelement aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))

Im Rahmen dieses Tutorials können Sie den DataList so formatieren, dass alle Produkte mit einem Preis, der kleiner als $20,00 ist, den Namen und den Einheiten Preis gelb hervorheben.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Schritt 2: Programm gesteuertes bestimmen des Werts der Daten im itemdatdent-Ereignis Handler

Da nur für Produkte mit einem Preis unter $20,00 die benutzerdefinierte Formatierung angewendet wird, müssen wir den Preis jedes Produkts ermitteln können. Beim Binden von Daten an einen DataList listet der DataList die Datensätze in der Datenquelle auf und erstellt für jeden Datensatz eine `DataListItem`-Instanz, die den Datenquellen Daten Satz an die `DataListItem`bindet. Nachdem die Daten des jeweiligen Datensatzes an das aktuelle `DataListItem` Objekt gebunden wurden, wird das DataList s `ItemDataBound`-Ereignis ausgelöst. Wir können einen Ereignishandler für dieses Ereignis erstellen, um die Datenwerte für die aktuelle `DataListItem` zu überprüfen. basierend auf diesen Werten nehmen Sie alle erforderlichen Formatierungs Änderungen vor.

Erstellen Sie ein `ItemDataBound` Ereignis für den DataList, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Obwohl das Konzept und die Semantik hinter dem DataList s `ItemDataBound`-Ereignishandler mit denen des GridView s `RowDataBound`-Ereignis Handlers in der *benutzerdefinierten Formatierung auf der Grundlage von datenlern* Programmen identisch sind, unterscheidet sich die Syntax leicht. Wenn das `ItemDataBound`-Ereignis ausgelöst wird, werden die `DataListItem`, die nur an Daten gebunden sind, über `e.Item` an den entsprechenden Ereignishandler geleitet (anstatt `e.Row`, wie bei dem GridView s `RowDataBound`-Ereignishandler). Der DataList s `ItemDataBound`-Ereignishandler wird für *jede* Zeile ausgelöst, die dem DataList hinzugefügt wird, einschließlich Header Zeilen, footerzeilen und Trennzeichen Zeilen. Die Produktinformationen sind jedoch nur an die Daten Zeilen gebunden. Wenn Sie das `ItemDataBound`-Ereignis verwenden, um die Daten zu überprüfen, die an den DataList gebunden sind, müssen wir daher zunächst sicherstellen, dass wir mit einem Datenelement arbeiten. Dies können Sie erreichen, indem Sie die `DataListItem` s [`ItemType`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)überprüfen, die einen der [folgenden acht Werte](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx)aufweisen kann:

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Sowohl `Item` als auch `AlternatingItem``DataListItem` s werden die Datenelemente der DataList-Datenelemente zusammen. Wenn wir mit einer `Item` oder `AlternatingItem`arbeiten, greifen wir auf die tatsächliche `ProductsRow` Instanz zu, die an den aktuellen `DataListItem`gebunden war. Die `DataListItem` s [`DataItem`-Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) enthält einen Verweis auf das `DataRowView`-Objekt, dessen `Row`-Eigenschaft einen Verweis auf das tatsächliche `ProductsRow` Objekt bereitstellt.

Als nächstes überprüfen wir die `ProductsRow` Instanz s `UnitPrice`-Eigenschaft. Da in der Tabelle "Products" der Tabelle "`UnitPrice`" `NULL` Werte zulässig sind, sollten Sie vor dem Zugriff auf die `UnitPrice` Eigenschaft zunächst überprüfen, ob Sie über einen `NULL` Wert verfügt, indem Sie die `IsUnitPriceNull()`-Methode verwenden. Wenn der `UnitPrice` Wert nicht `NULL`ist, überprüfen wir, ob er kleiner als $20,00 ist. Wenn Sie tatsächlich unter $20,00 ist, müssen wir die benutzerdefinierte Formatierung anwenden.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Schritt 3: Hervorheben des Namens und des Preises des Produkts

Sobald wir wissen, dass der Preis eines Produkts kleiner als $20,00 ist, muss nur noch der Name und der Preis hervorgehoben werden. Um dies zu erreichen, müssen wir zunächst in den `ItemTemplate`, die den Namen und den Preis des Produkts anzeigen, Programm gesteuert auf die Bezeichnungs Steuerelemente verweisen. Als nächstes müssen Sie einen gelben Hintergrund anzeigen. Diese Formatierungsinformationen können angewendet werden, indem die Bezeichnungen `BackColor` Eigenschaften (`LabelID.BackColor = Color.Yellow`) direkt geändert werden. im Idealfall sollten jedoch alle Anzeige bezogenen Aspekte über Cascading Stylesheets ausgedrückt werden. Tatsächlich verfügen wir bereits über ein Stylesheet, das die gewünschte Formatierung bereitstellt, die in `Styles.css` - `AffordablePriceEmphasis`definiert ist, die im Tutorial *benutzerdefinierte Formatierung basierend auf Daten* erstellt und erläutert wurde.

Um die Formatierung anzuwenden, legen Sie einfach die zwei Beschriftungs-websteuer Elemente `CssClass` Eigenschaften auf `AffordablePriceEmphasis`fest, wie im folgenden Code gezeigt:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Nachdem der `ItemDataBound`-Ereignishandler abgeschlossen ist, überprüfen Sie die `Formatting.aspx` Seite in einem Browser. Wie Abbildung 2 zeigt, werden für diese Produkte mit einem Preis unter $20,00 sowohl der Name als auch der Preis hervorgehoben.

[![diese Produkte, die kleiner als $20,00 sind, hervorgehoben](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Abbildung 2**: diese Produkte, die kleiner als $20,00 sind, werden hervorgehoben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))

> [!NOTE]
> Da der DataList als HTML-`<table>`gerendert wird, verfügen seine `DataListItem` Instanzen über Stil bezogene Eigenschaften, die so festgelegt werden können, dass ein bestimmter Stil auf das gesamte Element angewendet wird. Wenn beispielsweise das *gesamte* Element gelb hervorgehoben werden soll, wenn der Preis kleiner als $20,00 ist, könnten wir den Code ersetzen, der auf die Bezeichnungen verwiesen hat, und ihre `CssClass` Eigenschaften mit der folgenden Codezeile festlegen: `e.Item.CssClass = "AffordablePriceEmphasis"` (siehe Abbildung 3).

Die `RepeaterItem` en, die das Repeater-Steuerelement bilden, bieten jedoch keine Eigenschaften auf Stilebene. Daher erfordert das Anwenden der benutzerdefinierten Formatierung auf den Repeater die Anwendung der Stileigenschaften auf die websteuer Elemente innerhalb der Repeater s-Vorlagen, wie in Abbildung 2 dargestellt.

[![das gesamte Produkt Element für Produkte unter $20,00 hervorgehoben ist](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Abbildung 3**: das gesamte Produkt Element wird für Produkte unter $20,00 hervorgehoben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))

## <a name="using-formatting-functions-from-within-the-template"></a>Verwenden von Formatierungsfunktionen innerhalb der Vorlage

Im Tutorial *Verwenden von templatefields im GridView-Steuer* Element wurde erläutert, wie Sie eine Formatierungsfunktion innerhalb eines GridView-TemplateField verwenden können, um eine benutzerdefinierte Formatierung basierend auf den Daten, die an die GridView s-Zeilen gebunden sind, anzuwenden. Eine Formatierungsfunktion ist eine Methode, die aus einer Vorlage aufgerufen werden kann, und gibt den HTML-Code zurück, der an seiner Stelle ausgegeben werden soll. Formatierungsfunktionen können sich in der Code Behind-Klasse der ASP.net page s befinden oder in Klassendateien im `App_Code` Ordner oder in einem separaten Klassen Bibliotheksprojekt zentralisiert werden. Das Verschieben der Formatierungsfunktion aus der Code Behind-Klasse der ASP.net page s ist ideal, wenn Sie die gleiche Formatierungsfunktion auf mehreren ASP.NET-Seiten oder in anderen ASP.NET-Webanwendungen verwenden möchten.

Um Formatierungsfunktionen zu veranschaulichen, lassen Sie die Produktinformationen den Text [nicht eingestellt] neben dem Namen des Produkts enthalten, wenn er nicht mehr unterstützt wird. Lassen Sie außerdem den Preis gelb hervorheben, wenn er kleiner als $20,00 ist (wie im Beispiel für den `ItemDataBound` Ereignishandler). Wenn der Preis $20,00 oder höher ist, lassen Sie den tatsächlichen Preis nicht anzeigen, sondern geben Sie für ein Preisangebot ein. Abbildung 4 zeigt einen Screenshot der Produkt Auflistung, auf die diese Formatierungs Regeln angewendet werden.

[![für teure Produkte wird der Preis durch den Text ersetzt. Rufen Sie ein Preisangebot auf.](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Abbildung 4**: für teure Produkte wird der Preis durch den Text ersetzt. Rufen Sie ein Preisangebot auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png)).

## <a name="step-1-create-the-formatting-functions"></a>Schritt 1: Erstellen der Formatierungsfunktionen

Für dieses Beispiel benötigen wir zwei Formatierungsfunktionen: eine, die den Produktnamen zusammen mit dem Text anzeigt, bei Bedarf, und einen weiteren, der entweder einen hervorgehobenen Preis anzeigt, wenn er kleiner als $20,00 ist, oder den Text, um ein Preisangebot zu erhalten. Erstellen Sie diese Funktionen in der Code Behind-Klasse der ASP.net page s, und benennen Sie Sie `DisplayProductNameAndDiscontinuedStatus` und `DisplayPrice`. Beide Methoden müssen den HTML-Code zurückgeben, der als Zeichenfolge dargestellt werden soll, und beide müssen `Protected` (oder `Public`) markiert werden, damit Sie aus dem deklarativen Syntax Bereich der ASP.net page s aufgerufen werden können. Der Code für diese beiden Methoden folgt:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Beachten Sie, dass die `DisplayProductNameAndDiscontinuedStatus`-Methode die Werte der Datenfelder `productName` und `discontinued` als skalare Werte akzeptiert, während die `DisplayPrice`-Methode eine `ProductsRow` Instanz akzeptiert (anstatt einen `unitPrice` skalaren Wert). Beide Vorgehensweisen funktionieren. Wenn die Formatierungsfunktion jedoch mit Skalarwerten arbeitet, die Datenbank-`NULL` Werte enthalten können (z. b. `UnitPrice`, weder `ProductName` noch `Discontinued` `NULL` Werte zulassen), muss bei der Verarbeitung dieser skalaren Eingaben besonders darauf geachtet werden.

Insbesondere muss der Eingabeparameter vom Typ "`Object`" sein, da der eingehende Wert möglicherweise eine `DBNull` Instanz anstelle des erwarteten Datentyps ist. Außerdem muss überprüft werden, ob der eingehende Wert ein Daten Bank `NULL` Wert ist. Das heißt, wenn die `DisplayPrice`-Methode den Preis als Skalarwert akzeptieren soll, müssen wir den folgenden Code verwenden:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Beachten Sie, dass der `unitPrice` Eingabeparameter vom Typ `Object` ist und dass die bedingte Anweisung geändert wurde, um zu ermitteln, ob `unitPrice` `DBNull` ist. Da der `unitPrice` Eingabeparameter als `Object`übergeben wird, muss er außerdem in einen Dezimalwert umgewandelt werden.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Schritt 2: Aufrufen der Formatierungsfunktion aus dem DataList ItemTemplate-Element

Mit den Formatierungsfunktionen, die der ASP.net page s Code-Behind-Klasse hinzugefügt werden, besteht nur noch das Aufrufen dieser Formatierungsfunktionen aus dem DataList-`ItemTemplate`. Um eine Formatierungsfunktion aus einer Vorlage aufzurufen, platzieren Sie den Funktions Aufrufsatz in der Datenbindung-Syntax:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

In den DataList s `ItemTemplate` das websteuer Element `ProductNameLabel` Bezeichnung den Namen des Produkts zurzeit anzeigt, indem er seine `Text`-Eigenschaft dem Ergebnis `<%# Eval("ProductName") %>`zuweist. Um den Namen und den Text [nicht eingestellt] anzuzeigen, aktualisieren Sie ggf. die deklarative Syntax, sodass Sie stattdessen die `Text`-Eigenschaft dem Wert der `DisplayProductNameAndDiscontinuedStatus`-Methode zuweist. Dabei müssen wir den Namen des Produkts und die nicht mehr unterstützten Werte mithilfe der `Eval("columnName")`-Syntax übergeben. `Eval` gibt einen Wert vom Typ `Object`zurück, aber die `DisplayProductNameAndDiscontinuedStatus`-Methode erwartet Eingabeparameter vom Typ `String` und `Boolean`; Daher müssen wir die von der `Eval`-Methode zurückgegebenen Werte wie folgt in die erwarteten Eingabeparameter Typen umwandeln:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Um den Preis anzuzeigen, können wir einfach die `UnitPriceLabel` Bezeichnung s `Text`-Eigenschaft auf den Wert festlegen, der von der `DisplayPrice`-Methode zurückgegeben wird, ebenso wie bei der Anzeige des Product s-namens und des [nicht mehr unterstützten] Texts. Anstatt jedoch die `UnitPrice` als skalaren Eingabeparameter zu übergeben, übergeben wir stattdessen die gesamte `ProductsRow` Instanz:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Nehmen Sie mit den Aufrufen der Formatierungsfunktionen einen Moment Zeit, um den Fortschritt in einem Browser anzuzeigen. Ihr Bildschirm sollte in etwa wie in Abbildung 5 aussehen, wobei die nicht mehr unterstützten Produkte, einschließlich des Texts [nicht eingestellt], und der Produkte, die mehr als $20,00 haben, mit dem entsprechenden Preis durch den Text für ein Preisangebot ersetzt werden.

[![für teure Produkte wird der Preis durch den Text ersetzt. Rufen Sie ein Preisangebot auf.](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Abbildung 5**: für teure Produkte wird der Preis durch den Text ersetzt. Rufen Sie ein Preisangebot auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png)).

## <a name="summary"></a>Summary

Das Formatieren des Inhalts eines DataList-oder Repeater-Steuer Elements basierend auf den Daten kann mithilfe von zwei Verfahren durchgeführt werden. Die erste Methode ist das Erstellen eines Ereignis Handlers für das `ItemDataBound`-Ereignis, das ausgelöst wird, wenn jeder Datensatz in der Datenquelle an einen neuen `DataListItem` oder `RepeaterItem`gebunden ist. Im `ItemDataBound`-Ereignishandler können die aktuellen Elementdaten untersucht werden, und dann kann die Formatierung auf den Inhalt der Vorlage oder, für `DataListItem` s, auf das gesamte Element selbst angewendet werden.

Alternativ kann die benutzerdefinierte Formatierung durch Formatierungsfunktionen realisiert werden. Eine Formatierungsfunktion ist eine Methode, die aus den Vorlagen DataList oder Repeater s aufgerufen werden kann, die den HTML-Code zurückgibt, der an seiner Stelle ausgegeben wird. Häufig wird der von einer Formatierungsfunktion zurückgegebene HTML-Wert durch die Werte bestimmt, die an das aktuelle Element gebunden werden. Diese Werte können an die Formatierungsfunktion übergeben werden, entweder als skalare Werte oder durch Übergeben des gesamten Objekts, das an das Element gebunden ist (z. b. die `ProductsRow` Instanz).

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Yaakov Ellis, Randy Schmidt und Liz shulok. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Weiter](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
