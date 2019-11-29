---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Hinzufügen einer GridView-Spalte mit KontrollC#Kästchen () | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie einem GridView-Steuerelement eine Spalte mit Kontrollkästchen hinzufügen, um dem Benutzer eine intuitive Möglichkeit zur Auswahl mehrerer Zeilen des G...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: b9d1ed50e8d88202c3286b4cd0e9ebf111dfbe21
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592697"
---
# <a name="adding-a-gridview-column-of-checkboxes-c"></a>Hinzufügen einer GridView-Spalte mit Kontrollkästchen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) oder [PDF herunterladen](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> In diesem Tutorial wird erläutert, wie Sie einem GridView-Steuerelement eine Spalte mit Kontrollkästchen hinzufügen, um dem Benutzer eine intuitive Möglichkeit zur Auswahl mehrerer Zeilen der GridView zu bieten.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde untersucht, wie Sie der GridView eine Spalte mit Options Feldern hinzufügen, um einen bestimmten Datensatz auszuwählen. Eine Spalte mit Options Feldern ist eine geeignete Benutzeroberfläche, wenn der Benutzer höchstens ein Element aus dem Raster auswählen darf. Manchmal möchten wir jedoch, dass der Benutzer eine beliebige Anzahl von Elementen aus dem Raster auswählen kann. Webbasierte e-Mail-Clients zeigen z. b. in der Regel die Liste der Nachrichten mit einer Spalte mit Kontrollkästchen an. Der Benutzer kann eine beliebige Anzahl von Nachrichten auswählen und dann Aktionen ausführen, z. b. das Verschieben der e-Mails in einen anderen Ordner oder das Löschen.

In diesem Tutorial erfahren Sie, wie Sie eine Spalte mit Kontrollkästchen hinzufügen und wie Sie feststellen, welche Kontrollkästchen beim Postback überprüft wurden. Insbesondere erstellen wir ein Beispiel, das die Benutzeroberfläche des webbasierten e-Mail-Clients genau imitiert. Das Beispiel enthält eine auslagerbare GridView, in der die Produkte in der `Products` Datenbanktabelle mit einem Kontrollkästchen in den einzelnen Zeilen aufgelistet werden (siehe Abbildung 1). Wenn Sie auf die Schaltfläche ausgewählte Produkte löschen klicken, werden die ausgewählten Produkte gelöscht.

[![jede Produkt Zeile ein Kontrollkästchen enthält.](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Abbildung 1**: jede Produkt Zeile enthält ein Kontrollkästchen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))

## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Schritt 1: Hinzufügen einer auslagerbare GridView, in der Produktinformationen aufgelistet sind

Bevor wir uns mit dem Hinzufügen einer Spalte mit Kontrollkästchen beschäftigen, sollten Sie sich zunächst mit dem Auflisten der Produkte in einer GridView beschäftigen, die das Paging unterstützt. Öffnen Sie zunächst die Seite `CheckBoxField.aspx` im Ordner `EnhancedGridView`, und ziehen Sie eine GridView-Ansicht aus der Toolbox auf den Designer, und legen Sie deren `ID` auf `Products`fest. Als nächstes binden Sie das GridView-Objekt an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren Sie ObjectDataSource für die Verwendung der `ProductsBLL`-Klasse, und rufen Sie die `GetProducts()`-Methode auf, um die Daten zurückzugeben. Da diese GridView schreibgeschützt ist, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest.

[![erstellen Sie eine neue ObjectDataSource namens productdatasource.](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource mit dem Namen "`ProductsDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))

[![Konfigurieren von ObjectDataSource zum Abrufen von Daten mithilfe der GetProducts ()-Methode](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren von ObjectDataSource zum Abrufen von Daten mit der `GetProducts()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Abbildung 4**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen erstellt Visual Studio automatisch BoundColumns und eine CheckBoxColumn für die produktbezogenen Datenfelder. Entfernen Sie wie im vorherigen Tutorial alle außer den `ProductName`, `CategoryName`und `UnitPrice` boundfields, und ändern Sie die `HeaderText` Eigenschaften in Product, Category und Price. Konfigurieren Sie das `UnitPrice` BoundField so, dass sein Wert als Währung formatiert ist. Konfigurieren Sie außerdem die GridView, um das Paging zu unterstützen, indem Sie das Kontrollkästchen Paging aktivieren des Smarttags aktivieren.

Fügen Sie außerdem die Benutzeroberfläche zum Löschen der ausgewählten Produkte hinzu. Fügen Sie unterhalb der GridView ein Schaltflächen-websteuer Element hinzu, und legen Sie dessen `ID` auf `DeleteSelectedProducts` und seine `Text`-Eigenschaft fest, um die ausgewählten Produkte Anstatt Produkte aus der Datenbank zu löschen, wird in diesem Beispiel nur eine Meldung angezeigt, in der die Produkte angegeben werden, die gelöscht wurden. Fügen Sie ein Label-websteuer Element unterhalb der Schaltfläche hinzu, um dies zu ermöglichen. Legen Sie die ID auf `DeleteResults`fest, löschen Sie die `Text`-Eigenschaft, und legen Sie deren Eigenschaften `Visible` und `EnableViewState` auf `false`fest.

Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup "GridView", "ObjectDataSource", "Button" und "Label" in etwa wie folgt aussehen:

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um die Seite in einem Browser anzuzeigen (siehe Abbildung 5). An diesem Punkt sollten der Name, die Kategorie und der Preis der ersten zehn Produkte angezeigt werden.

[![Name, Kategorie und Preis der ersten zehn Produkte werden aufgelistet.](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Abbildung 5**: Name, Kategorie und Preis der ersten zehn Produkte werden aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))

## <a name="step-2-adding-a-column-of-checkboxes"></a>Schritt 2: Hinzufügen einer Spalte mit Kontrollkästchen

Da ASP.NET 2,0 ein CheckBoxField enthält, könnte man vermuten, dass es zum Hinzufügen einer Spalte mit Kontrollkästchen zu einer GridView verwendet werden kann. Leider ist dies nicht der Fall, da das CheckBoxField für die Arbeit mit einem booleschen Datenfeld konzipiert ist. Das heißt, um das CheckBoxField zu verwenden, muss das zugrunde liegende Datenfeld angegeben werden, dessen Wert dahingehend überprüft wird, ob das gerenderte Kontrollkästchen aktiviert ist. Wir können das CheckBoxField nicht verwenden, um nur eine Spalte mit nicht überprüften Kontrollkästchen einzuschließen.

Stattdessen müssen wir ein TemplateField-Steuerelement hinzufügen und seinem `ItemTemplate`ein CheckBox-websteuer Element hinzufügen. Fügen Sie der `Products` GridView ein TemplateField-Element hinzu, und legen Sie es als erstes Feld (weit links) ab. Klicken Sie im GridView s-Smarttag auf den Link Vorlagen bearbeiten, und ziehen Sie dann ein CheckBox-websteuer Element aus der Toolbox in die `ItemTemplate`. Legen Sie diese Checkbox s `ID`-Eigenschaft auf `ProductSelector`fest.

[![ein CheckBox-websteuer Element namens productselector zu TemplateField s ItemTemplate hinzufügen.](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Abbildung 6**: Hinzufügen eines CheckBox-websteuer Elements mit dem Namen `ProductSelector` zum `ItemTemplate` TemplateField s ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))

Wenn das websteuer Element TemplateField und CheckBox hinzugefügt wurde, enthält jede Zeile nun ein Kontrollkästchen. In Abbildung 7 wird diese Seite angezeigt, wenn Sie über einen Browser angezeigt wird, nachdem das TemplateField-Element und das CheckBox-Element hinzugefügt wurden.

[![jede Produkt Zeile nun ein Kontrollkästchen enthält.](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Abbildung 7**: jede Produkt Zeile enthält jetzt ein Kontrollkästchen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))

## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Schritt 3: bestimmen, welche Kontrollkästchen beim Postback aktiviert wurden

An dieser Stelle haben wir eine Spalte mit Kontrollkästchen, aber keine Möglichkeit, zu bestimmen, welche Kontrollkästchen beim Postback überprüft wurden. Wenn Sie auf die Schaltfläche ausgewählte Produkte löschen klicken, müssen Sie jedoch wissen, welche Kontrollkästchen aktiviert wurden, um diese Produkte zu löschen.

Die GridView s [`Rows`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) ermöglicht den Zugriff auf die Daten Zeilen in der GridView. Wir können diese Zeilen durchlaufen, Programm gesteuert auf das CheckBox-Steuerelement zugreifen und dann die `Checked`-Eigenschaft überprüfen, um zu bestimmen, ob das Kontrollkästchen aktiviert wurde.

Erstellen Sie einen Ereignishandler für das `Click` Ereignis für das `DeleteSelectedProducts` Button-websteuer Element, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

Die `Rows`-Eigenschaft gibt eine Auflistung von `GridViewRow`-Instanzen zurück, die die Daten Zeilen der GridView-Datenbank zusammenstellen. In der `foreach`-Schleife wird diese Auflistung aufgelistet. Für jedes `GridViewRow` Objekt wird das Kontrollkästchen Zeile s mithilfe `row.FindControl("controlID")`Programm gesteuert aufgerufen. Wenn das Kontrollkästchen aktiviert ist, wird die Zeile s, der `ProductID` Wert entspricht, aus der `DataKeys` Auflistung abgerufen. In dieser Übung zeigen wir einfach eine informative Meldung in der `DeleteResults` Bezeichnung an, obwohl wir in einer funktionierenden Anwendung stattdessen die `ProductsBLL` Class s `DeleteProduct(productID)`-Methode aufrufen.

Durch das Hinzufügen dieses Ereignis Handlers werden nun die `ProductID` s der ausgewählten Produkte angezeigt, wenn Sie auf die Schaltfläche ausgewählte Produkte löschen klicken.

[Wenn auf die Schaltfläche ausgewählte Produkte Löschen geklickt wird, werden die Produkte ProductIDs der ausgewählten Produkte aufgeführt. ![](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Abbildung 8**: Wenn auf die Schaltfläche ausgewählte Produkte Löschen geklickt wird, werden die ausgewählten Produkte `ProductID` s aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png)).

## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Schritt 4: Hinzufügen der Schaltflächen alle überprüfen und alle deaktivieren

Wenn ein Benutzer alle Produkte auf der aktuellen Seite löschen möchte, muss er jedes der zehn Kontrollkästchen aktivieren. Wir können diesen Prozess beschleunigen, indem wir die Schaltfläche Alle überprüfen hinzufügen. Wenn Sie darauf klicken, werden alle Kontrollkästchen im Raster ausgewählt. Die Schaltfläche Alle deaktivieren wäre gleichermaßen hilfreich.

Fügen Sie der Seite zwei Schaltflächen-websteuer Elemente hinzu, und platzieren Sie Sie über der GridView. Legen Sie die erste `ID` auf `CheckAll` und deren `Text`-Eigenschaft auf alle überprüfen. Legen Sie die zweite s-`ID` auf `UncheckAll` und deren `Text`-Eigenschaft auf alle deaktivieren fest.

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Erstellen Sie als nächstes eine Methode in der Code Behind-Klasse mit dem Namen `ToggleCheckState(checkState)`, die beim Aufrufen die `Products` GridView s `Rows`-Auflistung auflistet und jede Checkbox s `Checked`-Eigenschaft auf den Wert des übergebenen *CheckState* -Parameters festlegt.

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Erstellen Sie als nächstes `Click` Ereignishandler für die Schaltflächen `CheckAll` und `UncheckAll`. Nennen Sie in `CheckAll` s-Ereignishandler einfach `ToggleCheckState(true)`; nennen Sie in `UncheckAll``ToggleCheckState(false)`.

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Mit diesem Code wird durch Klicken auf die Schaltfläche Alle überprüfen ein Postback ausgelöst und alle Kontrollkästchen in der GridView überprüft. Wenn Sie auf alle deaktivieren klicken, werden alle Kontrollkästchen deaktiviert. Abbildung 9 zeigt den Bildschirm, nachdem die Schaltfläche Alle überprüfen aktiviert wurde.

[![klicken auf die Schaltfläche Alle überprüfen wählt alle Kontrollkästchen aus.](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Abbildung 9**: durch Klicken auf die Schaltfläche Alle überprüfen werden alle Kontrollkästchen ausgewählt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))

> [!NOTE]
> Wenn Sie eine Spalte mit Kontrollkästchen anzeigen, wird ein Ansatz zum auswählen oder Aufheben der Auswahl aller Kontrollkästchen durch ein Kontrollkästchen in der Kopfzeile angezeigt. Außerdem ist für die gesamte Implementierung überprüfen alle/alle deaktivieren ein Postback erforderlich. Die Kontrollkästchen können allerdings vollständig über das Client seitige Skript aktiviert oder deaktiviert werden. Dadurch wird eine benutzerfreundliche Darstellung des Benutzer Erlebnisses bereitgestellt. Aktivieren Sie das Kontrollkästchen Alle [Kontrollkästchen in einer GridView mithilfe eines Client seitigen Skripts und eines Kontrollkästchens überprüfen](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx), um die Verwendung einer Kopfzeile für alle Überprüfungen und das Deaktivieren der Daten im Detail zu untersuchen.

## <a name="summary"></a>Summary

In Fällen, in denen Sie es Benutzern ermöglichen müssen, eine beliebige Anzahl von Zeilen aus einer GridView auszuwählen, bevor Sie fortfahren, ist das Hinzufügen einer Spalte mit Kontrollkästchen eine Option. Wie in diesem Tutorial gezeigt, umfasst das Hinzufügen einer Spalte mit Kontrollkästchen in der GridView das Hinzufügen eines TemplateField mit einem CheckBox-websteuer Element. Durch die Verwendung eines websteuer Elements (im Gegensatz zum direkten Einfügen von Markup in die Vorlage, wie im vorherigen Tutorial), wird ASP.NET automatisch daran erinnert, welche Kontrollkästchen waren und nicht über das Postback überprüft wurden. Wir können auch Programm gesteuert auf die Kontrollkästchen im Code zugreifen, um zu bestimmen, ob ein bestimmtes Kontrollkästchen aktiviert ist, oder um den aktivierten Zustand zu ändern.

In diesem Tutorial und dem letzten Artikel wurde das Hinzufügen einer Zeilenselektor-Spalte zur GridView behandelt. Im nächsten Tutorial untersuchen wir, wie wir mit etwas Arbeit der GridView Funktionen zum Einfügen hinzufügen können.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Weiter](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
