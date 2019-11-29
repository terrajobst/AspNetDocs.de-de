---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Anpassen der Bearbeitungs Schnittstelle von DataList (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erstellen wir eine umfassendere Bearbeitungs Schnittstelle für DataList, eine mit Dropdown Listen und ein Kontrollkästchen.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: adee419764cff2f39ee16962080c24b52553aa14
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637600"
---
# <a name="customizing-the-datalists-editing-interface-vb"></a>Anpassen der Oberfläche für die Bearbeitung von DataList (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) oder [PDF herunterladen](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> In diesem Tutorial erstellen wir eine umfassendere Bearbeitungs Schnittstelle für DataList, eine mit Dropdown Listen und ein Kontrollkästchen.

## <a name="introduction"></a>Einführung

Das Markup und die websteuer Elemente in den DataList-`EditItemTemplate` definieren seine bearbeitbare Schnittstelle. In allen bearbeitbaren DataList-Beispielen, die wir bisher geprüft haben, besteht die editierbare Schnittstelle aus TextBox-websteuer Elementen. Im [vorangehenden Tutorial](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) haben wir die Benutzeroberflächen zur Bearbeitungszeit durch Hinzufügen von Validierungs Steuerelementen verbessert.

Der `EditItemTemplate` kann erweitert werden, um andere websteuer Elemente als das Textfeld einzuschließen, z. b. Dropdown Listen, RadioButton Lists, Kalender usw. Wie bei Textfeldern können Sie die folgenden Schritte ausführen, wenn Sie die Bearbeitungs Schnittstelle anpassen, um andere websteuer Elemente einzuschließen:

1. Fügen Sie dem `EditItemTemplate`das websteuer Element hinzu.
2. Verwenden Sie die Datenbindung-Syntax, um der entsprechenden Eigenschaft den entsprechenden Daten Feldwert zuzuweisen.
3. Greifen Sie im `UpdateCommand`-Ereignishandler Programm gesteuert auf den Wert des websteuer Elements zu, und übergeben Sie ihn an die entsprechende BLL-Methode.

In diesem Tutorial erstellen wir eine umfassendere Bearbeitungs Schnittstelle für DataList, eine mit Dropdown Listen und ein Kontrollkästchen. Insbesondere erstellen wir einen DataList, der Produktinformationen auflistet und es gestattet, dass der Name, der Lieferant, die Kategorie und der nicht mehr unterstützte Status aktualisiert werden (siehe Abbildung 1).

[![die Bearbeitungs Schnittstelle ein Textfeld, zwei Dropdown Listen und ein Kontrollkästchen enthält.](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Abbildung 1**: die Bearbeitungs Schnittstelle enthält ein Textfeld, zwei Dropdown Listen und ein Kontrollkästchen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image3.png)).

## <a name="step-1-displaying-product-information"></a>Schritt 1: Anzeigen von Produktinformationen

Bevor wir die editierbare Schnittstelle von DataList erstellen können, müssen wir zuerst die schreibgeschützte Schnittstelle erstellen. Öffnen Sie zunächst die Seite `CustomizedUI.aspx` aus dem Ordner `EditDeleteDataList`, und fügen Sie im Designer der Seite einen DataList hinzu, und legen Sie dessen `ID`-Eigenschaft auf `Products`fest. Erstellen Sie aus dem DataList s Smarttags eine neue ObjectDataSource. Benennen Sie diese neue ObjectDataSource-`ProductsDataSource`, und konfigurieren Sie Sie so, dass Sie Daten aus der `ProductsBLL` Class s `GetProducts`-Methode abruft. Wie bei den vorherigen bearbeitbaren DataList-Lernprogrammen aktualisieren wir die bearbeiteten Produkt-e-Informationen, indem wir direkt zur Geschäftslogik Schicht gehen. Legen Sie entsprechend die Dropdown Listen auf den Registerkarten Update, INSERT und DELETE auf (keine) fest.

[![die Dropdown Listen Update, INSERT und DELETE Registerkarten auf (keine) festgelegt.](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Abbildung 2**: Festlegen der Dropdown Listen für das Aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))

Nach dem Konfigurieren von ObjectDataSource erstellt Visual Studio eine Standard `ItemTemplate` für den DataList, der den Namen und den Wert für jedes der zurückgegebenen Datenfelder auflistet. Ändern Sie die `ItemTemplate` so, dass die Vorlage den Produktnamen in einem `<h4>`-Element zusammen mit dem Kategoriename, dem Lieferanten Namen, dem Preis und dem Status "eingestellt" auflistet. Fügen Sie außerdem eine Bearbeitungs Schaltfläche hinzu, um sicherzustellen, dass die `CommandName`-Eigenschaft auf Bearbeiten festgelegt ist. Das deklarative Markup für meine `ItemTemplate` folgt:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Das obige Markup legt die Produktinformationen mithilfe einer &lt;H4-&gt; Überschrift für den Product s-Namen und eine vier spaltige `<table>` für die restlichen Felder fest. Die `ProductPropertyLabel`-und `ProductPropertyValue` CSS-Klassen, die in `Styles.css`definiert sind, wurden in den vorherigen Tutorials erläutert. In Abbildung 3 wird der Fortschritt angezeigt, wenn ein Browser angezeigt wird.

[![der Name, der Lieferant, die Kategorie, der nicht mehr unterstützte Status und der Preis der einzelnen Produkte angezeigt.](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Abbildung 3**: der Name, der Lieferant, die Kategorie, der nicht mehr unterstützte Status und der Preis der einzelnen Produkte werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))

## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Schritt 2: Hinzufügen der websteuer Elemente zur Bearbeitungs Schnittstelle

Der erste Schritt beim Aufbau der angepassten DataList-Bearbeitungs Schnittstelle besteht darin, dem `EditItemTemplate`die erforderlichen websteuer Elemente hinzuzufügen. Insbesondere benötigen wir eine Dropdown List für die Kategorie, eine für den Lieferanten und ein Kontrollkästchen für den nicht unterstützten Zustand. Da der Preis des Produkts in diesem Beispiel nicht bearbeitbar ist, können wir ihn mit einem Label-websteuer Element weiter anzeigen.

Um die Bearbeitungs Schnittstelle anzupassen, klicken Sie auf den Link Vorlagen bearbeiten des Smarttags DataList s, und wählen Sie die Option `EditItemTemplate` aus der Dropdown Liste aus. Fügen Sie dem `EditItemTemplate` eine DropDownList hinzu, und legen Sie dessen `ID` auf `Categories`fest.

[![eine Dropdown Liste für die Kategorien hinzufügen.](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Abbildung 4**: Hinzufügen einer Dropdown Liste für die Kategorien ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))

Wählen Sie als nächstes im Smarttags DropDownList s die Option Datenquelle auswählen aus, und erstellen Sie eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`. Konfigurieren Sie diese ObjectDataSource für die Verwendung der `CategoriesBLL` Class `GetCategories()`-Methode (siehe Abbildung 5). Anschließend fordert der Assistent zum Konfigurieren der Dropdown List s-Datenquelle die Datenfelder an, die für die einzelnen `ListItem` s `Text` und `Value` Eigenschaften verwendet werden sollen. Legen Sie in der Dropdown Liste das Feld für die `CategoryName` Daten und die `CategoryID` als Wert, wie in Abbildung 6 dargestellt.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "categoriesdatasource".](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource mit dem Namen "`CategoriesDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))

[![die Felder "DropDownList s Display" und "Value" konfigurieren.](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Abbildung 6**: Konfigurieren der Felder "DropDownList s Display" und "Value" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))

Wiederholen Sie diese Schritte, um eine Dropdown List für die Lieferanten zu erstellen. Legen Sie den `ID` für diese Dropdown Liste auf `Suppliers` fest, und benennen Sie die ObjectDataSource-`SuppliersDataSource`.

Nachdem Sie die beiden Dropdown Listen hinzugefügt haben, fügen Sie ein Kontrollkästchen für den Status "eingestellt" und ein Textfeld für den Namen des Produkts hinzu. Legen Sie die `ID` s für das Kontrollkästchen und Textfeld auf `Discontinued` bzw. `ProductName`fest. Fügen Sie ein "Requirements dfieldvalidator" hinzu, um sicherzustellen, dass der Benutzer einen Wert für den Namen des Produkts bereitstellt.

Fügen Sie abschließend die Schaltflächen aktualisieren und Abbrechen hinzu. Beachten Sie, dass es für diese beiden Schaltflächen zwingend erforderlich ist, dass die `CommandName` Eigenschaften auf aktualisieren bzw. Abbrechen festgelegt sind.

Sie können die Bearbeitungs Schnittstelle beliebig gestalten. Ich habe mich für die Verwendung desselben vier spaltigen `<table>` Layouts von der schreibgeschützten Oberfläche entschieden, wie die folgende deklarative Syntax und der folgende Screenshot veranschaulicht:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]

[![die Bearbeitungs Schnittstelle wie die schreibgeschützte Schnittstelle angelegt ist.](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Abbildung 7**: die Bearbeitungs Schnittstelle wird wie die schreibgeschützte Schnittstelle angeordnet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))

## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Schritt 3: Erstellen der Ereignishandler EditCommand und CancelCommand

Derzeit gibt es keine Datenbindung-Syntax in der `EditItemTemplate` (mit Ausnahme des `UnitPriceLabel`, das aus der `ItemTemplate`kopiert wurde). Die Datenbindung-Syntax wird vorübergehend hinzugefügt, aber zuerst werden die Ereignishandler für die DataList-`EditCommand` und `CancelCommand` Ereignisse erstellt. Beachten Sie, dass die Aufgabe des `EditCommand` Ereignis Handlers darin besteht, die Bearbeitungs Schnittstelle für das DataList-Element zu rendernden, auf dessen Bearbeitungs Schaltfläche geklickt wurde, während der `CancelCommand` s-Auftrag den DataList in den Zustand der vorabbearbeitung zurückgibt.

Erstellen Sie diese beiden Ereignishandler, und verwenden Sie den folgenden Code:

[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Wenn diese beiden Ereignishandler vorhanden sind, wird durch Klicken auf die Schaltfläche Bearbeiten die Bearbeitungs Schnittstelle angezeigt. Wenn Sie auf die Schaltfläche Abbrechen klicken, wird das bearbeitete Element in den schreibgeschützten Modus zurückgegeben. Abbildung 8 zeigt den DataList nach dem Klicken auf die Schaltfläche "Bearbeiten" für Chef Anton s Gumbo Mix. Da wir eine beliebige Datenbindung-Syntax zur Bearbeitungs Schnittstelle hinzugefügt haben, ist das Textfeld `ProductName` leer, das Kontrollkästchen `Discontinued` deaktiviert und die ersten Elemente, die aus den Dropdown Listen `Categories` und `Suppliers` ausgewählt werden.

[![klicken auf die Schaltfläche "Bearbeiten" zeigt die Bearbeitungs Schnittstelle an](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Abbildung 8**: Klicken auf die Schaltfläche "Bearbeiten" zeigt die Bearbeitungs Schnittstelle[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))

## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Schritt 4: Hinzufügen der DataBinding-Syntax zur Bearbeitungs Schnittstelle

Damit die Bearbeitungs Schnittstelle die aktuellen Product s-Werte anzeigt, müssen wir die Daten Feldwerte mithilfe der Datenbindung-Syntax den entsprechenden websteuer Element Werten zuweisen. Die Datenbindung-Syntax kann über den Designer angewendet werden, indem Sie auf den Bildschirm Vorlagen bearbeiten klicken und den Link DataBindings bearbeiten aus den Smarttags für websteuer Elemente auswählen. Alternativ kann die Datenbindung-Syntax direkt dem deklarativen Markup hinzugefügt werden.

Weisen Sie den Wert `ProductName` Data Field der Eigenschaft `ProductName` TextBox s `Text`, die `CategoryID`-und `SupplierID` Daten Feldwerte den `Categories` Eigenschaften `Suppliers` und `SelectedValue` Dropdown Listen zu, und legen Sie den Wert `Discontinued` Daten Feldwert der `Discontinued`-Eigenschaft `Checked` CheckBox s. Nachdem Sie diese Änderungen vorgenommen haben, entweder über den Designer oder direkt über das deklarative Markup, besuchen Sie die Seite über einen Browser, und klicken Sie auf die Schaltfläche Bearbeiten für Chef Anton s Gumbo Mix. Wie Abbildung 9 zeigt, hat die Datenbindung-Syntax die aktuellen Werte in das Textfeld, DropDownLists und CheckBox eingefügt.

[![klicken auf die Schaltfläche "Bearbeiten" zeigt die Bearbeitungs Schnittstelle an](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Abbildung 9**: Klicken auf die Schaltfläche "Bearbeiten" zeigt die Bearbeitungs Schnittstelle[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))

## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Schritt 5: Speichern der Benutzer-e-Änderungen im UpdateCommand-Ereignis Handler

Wenn der Benutzer ein Produkt bearbeitet und auf die Schaltfläche Aktualisieren klickt, erfolgt ein Postback, und das DataList s-`UpdateCommand` Ereignis wird ausgelöst. Im-Ereignishandler müssen die Werte aus den websteuer Elementen in der `EditItemTemplate` und der Schnittstelle mit der BLL gelesen werden, um das Produkt in der Datenbank zu aktualisieren. Wie bereits in den vorherigen Tutorials gezeigt, ist der `ProductID` des aktualisierten Produkts über die `DataKeys` Sammlung erreichbar. Der Zugriff auf die Benutzer eingegebenen Felder erfolgt durch Programm gesteuertes verweisen auf die websteuer Elemente mithilfe von `FindControl("controlID")`, wie im folgenden Code gezeigt:

[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Der Code beginnt mit der `Page.IsValid`-Eigenschaft, um sicherzustellen, dass alle Validierungs Steuerelemente auf der Seite gültig sind. Wenn `Page.IsValid` `True`ist, wird der bearbeitete Product s `ProductID` Wert aus der `DataKeys` Auflistung gelesen, und auf die Dateneingabe-websteuer Elemente im `EditItemTemplate` wird Programm gesteuert verwiesen. Als nächstes werden die Werte dieser websteuer Elemente in Variablen eingelesen, die dann an die entsprechende `UpdateProduct` Überladung weitergegeben werden. Nach dem Aktualisieren der Daten wird der DataList in den Zustand der vorabbearbeitung zurückversetzt.

> [!NOTE]
> Ich habe die Ausnahme Behandlungs Logik ausgelassen, die im Tutorial [Behandlung von BLL-und Dal-Ebene-Ausnahmen](handling-bll-and-dal-level-exceptions-vb.md) hinzugefügt wurde, um den Code und dieses Beispiel im Fokus zu halten. Als Übung fügen Sie diese Funktion nach Abschluss dieses Tutorials hinzu.

## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Schritt 6: Behandeln von NULL-CategoryID-und SupplierID-Werten

Die Northwind-Datenbank ermöglicht das `NULL` von Werten für die `Products` Tabellen `CategoryID` und `SupplierID` Spalten. Die Bearbeitungs Schnittstelle unternimmt jedoch derzeit keine `NULL` Werte. Wenn Sie versuchen, ein Produkt zu bearbeiten, das einen `NULL` Wert für die `CategoryID`-oder `SupplierID` Spalten hat, erhalten Sie eine `ArgumentOutOfRangeException` mit einer Fehlermeldung ähnlich der folgenden: *"categories" weist einen "SelectedValue" auf, der ungültig ist, weil er nicht in der Liste der Elemente vorhanden ist.* Außerdem gibt es derzeit keine Möglichkeit, die Kategorie oder den Lieferanten Wert eines Produkts von einem Wert, der nicht`NULL` ist, in einen `NULL` zu ändern.

Um `NULL` Werte für die Dropdown Listen Kategorie und Lieferanten zu unterstützen, müssen wir ein zusätzliches `ListItem`hinzufügen. Ich habe mich für die Verwendung von (keine) als `Text` Wert für diese `ListItem`entschieden, aber Sie können ihn in etwas anderes ändern (z. b. eine leere Zeichenfolge). Vergessen Sie schließlich nicht, die DropDownLists-`AppendDataBoundItems` auf `True`festzulegen. Wenn Sie dies vergessen, überschreiben die an die Dropdown Liste gebundenen Kategorien und Lieferanten die statisch hinzugefügten `ListItem`.

Nachdem Sie diese Änderungen vorgenommen haben, sollte das DropDownLists-Markup in der DataList s-`EditItemTemplate` in etwa wie folgt aussehen:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statische `ListItem` s können einer Dropdown List über den Designer oder direkt über die deklarative Syntax hinzugefügt werden. Stellen Sie beim Hinzufügen eines Dropdown List-Elements zur Darstellung eines Daten Bank `NULL` Werts sicher, dass Sie die `ListItem` über die deklarative Syntax hinzufügen. Wenn Sie den `ListItem` Auflistungs-Editor im-Designer verwenden, wird die `Value` Einstellung von der generierten deklarativen Syntax nicht vollständig ausgelassen, wenn eine leere Zeichenfolge zugewiesen wird, wobei deklaratives Markup wie folgt erstellt wird: `<asp:ListItem>(None)</asp:ListItem>`. Obwohl dies harmlos aussehen kann, bewirkt das fehlende `Value`, dass die Dropdown List den `Text`-Eigenschafts Wert verwendet. Dies bedeutet Folgendes: Wenn diese `NULL` `ListItem` ausgewählt ist, wird der Wert (None) dem Feld "Product Data" (`CategoryID` oder `SupplierID`in diesem Tutorial) zugewiesen, was zu einer Ausnahme führt. Wenn Sie `Value=""`explizit festlegen, wird dem Feld "Product Data" ein `NULL` Wert zugewiesen, wenn der `NULL` `ListItem` ausgewählt wird.

Nehmen Sie sich einen Moment Zeit, um den Fortschritt über einen Browser anzuzeigen. Beachten Sie beim Bearbeiten eines Produkts, dass die Dropdown Listen `Categories` und `Suppliers` beide eine (None)-Option am Anfang der Dropdown List haben.

[![die Dropdown Listen Kategorien und Lieferanten enthalten die Option (None).](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Abbildung 10**: die Dropdown Listen `Categories` und `Suppliers` enthalten die Option (keine) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))

Zum Speichern der Option (None) als Daten Bank `NULL` Wert müssen wir zum `UpdateCommand`-Ereignishandler zurückkehren. Ändern Sie die `categoryIDValue`-und `supplierIDValue` Variablen in Werte, die auf NULL festgelegt werden können, und weisen Sie Ihnen einen anderen Wert als `Nothing` zu, wenn die DropDownList s-`SelectedValue` keine leere Zeichenfolge ist:

[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Mit dieser Änderung wird der Wert `Nothing` an die `UpdateProduct` BLL-Methode übermittelt, wenn der Benutzer die Option (None) aus einer der Dropdown Listen ausgewählt hat, die einem `NULL` Daten Bank Wert entspricht.

## <a name="summary"></a>Summary

In diesem Tutorial wurde erläutert, wie eine komplexere DataList-Bearbeitungs Schnittstelle erstellt wird, die drei verschiedene websteuer Elemente für Eingaben, zwei Dropdown Listen und ein Kontrollkästchen mit Validierungs Steuerelementen enthielt. Wenn Sie die Bearbeitungs Schnittstelle entwickeln, sind die Schritte unabhängig von den verwendeten websteuer Elementen identisch: beginnen Sie mit dem Hinzufügen der websteuer Elemente zum DataList-`EditItemTemplate`; Verwenden Sie die Datenbindung-Syntax, um die entsprechenden Daten Feldwerte den entsprechenden websteuer Element Eigenschaften zuzuweisen. und in der `UpdateCommand` Ereignishandler Programm gesteuert auf die websteuer Elemente und die entsprechenden Eigenschaften zugreifen, wobei ihre Werte an die BLL übergeben werden.

Wenn Sie eine Bearbeitungs Schnittstelle erstellen, unabhängig davon, ob es sich um nur Textfelder oder eine Auflistung verschiedener websteuer Elemente handelt, achten Sie darauf, die Daten Bank `NULL` Werte ordnungsgemäß zu verarbeiten. Wenn Sie `NULL` s in Rechnung stellen, ist es zwingend erforderlich, dass Sie einen vorhandenen `NULL` Wert nicht nur ordnungsgemäß in der Bearbeitungs Schnittstelle anzeigen, sondern auch ein Mittel zum Markieren eines Werts als `NULL`anbieten. Für Dropdown Listen in datalisten bedeutet das in der Regel, dass ein statisches `ListItem` hinzugefügt wird, dessen `Value`-Eigenschaft explizit auf eine leere Zeichenfolge (`Value=""`) festgelegt ist, und dem `UpdateCommand` Ereignishandler ein Bit Code hinzufügt, um zu bestimmen, ob der `NULL``ListItem` ausgewählt wurde.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Dennis Patterson, David suru und Randy Schmidt. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
