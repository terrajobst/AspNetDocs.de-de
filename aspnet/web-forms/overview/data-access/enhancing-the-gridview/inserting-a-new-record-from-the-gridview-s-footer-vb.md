---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Einfügen eines neuen Datensatzes aus dem GridView-Footer (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Das GridView-Steuerelement bietet zwar keine integrierte Unterstützung für das Einfügen eines neuen Datensatzes, aber in diesem Tutorial wird gezeigt, wie Sie die GridView so erweitern können, dass ein...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ef370a90bc843f5c2da80bb43c8ef8de216b51
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631733"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Einfügen eines neuen Datensatzes in den GridView-Fuß (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) oder [PDF herunterladen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Das GridView-Steuerelement bietet zwar keine integrierte Unterstützung für das Einfügen eines neuen Datensatzes, aber in diesem Tutorial wird gezeigt, wie Sie die GridView so erweitern können, dass Sie eine einfügeschnittstelle einschließt.

## <a name="introduction"></a>Einführung

Wie in der Übersicht über das [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) beschrieben, beinhalten die websteuer Elemente GridView, DetailsView und FormView jeweils integrierte Daten Änderungs Funktionen. Bei Verwendung mit deklarativen Datenquellen-Steuerelementen können diese drei websteuer Elemente schnell und einfach zum Ändern von Daten und in Szenarios konfiguriert werden, ohne dass eine einzige Codezeile geschrieben werden muss. Leider bieten nur die Steuerelemente DetailsView und FormView integrierte Funktionen zum Einfügen, bearbeiten und löschen. Die GridView bietet nur Unterstützung für die Bearbeitung und Löschung. Mit einem kleinen bogenfeckpunkt können wir jedoch die GridView erweitern, um eine einfügende Schnittstelle einzubeziehen.

Beim Hinzufügen von Einfügungs Funktionen zur GridView sind wir dafür verantwortlich, zu entscheiden, wie neue Datensätze hinzugefügt werden, die einfügeschnittstelle erstellt und der Code zum Einfügen des neuen Datensatzes geschrieben wird. In diesem Tutorial erfahren Sie, wie Sie die einfügeschnittstelle zur GridView-Footerzeile hinzufügen (siehe Abbildung 1). Die footerzelle für jede Spalte enthält das entsprechende Benutzeroberflächen Element für die Datensammlung (ein Textfeld für den Produktnamen, eine Dropdown Liste für den Lieferanten usw.). Wir benötigen auch eine Spalte für die Schaltfläche "hinzufügen", die beim Klicken ein Postback verursacht und einen neuen Datensatz in die `Products` Tabelle einfügt, wobei die in der Footerzeile angegebenen Werte verwendet werden.

[![der Footerzeile stellt eine Schnittstelle zum Hinzufügen neuer Produkte bereit.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Abbildung 1**: die Footerzeile stellt eine Schnittstelle zum Hinzufügen neuer Produkte bereit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))

## <a name="step-1-displaying-product-information-in-a-gridview"></a>Schritt 1: Anzeigen von Produktinformationen in einer GridView

Bevor wir uns mit dem Erstellen der einfügeschnittstelle in der Fußzeile von GridView befassen, konzentrieren wir uns zunächst auf das Hinzufügen einer GridView-Ansicht auf der Seite, auf der die Produkte in der Datenbank aufgelistet sind. Öffnen Sie zunächst die Seite `InsertThroughFooter.aspx` im Ordner `EnhancedGridView`, und ziehen Sie eine GridView aus der Toolbox auf den Designer. Legen Sie die Eigenschaft GridView s `ID` auf `Products`fest. Verwenden Sie als nächstes das Smarttag GridView s, um es an eine neue ObjectDataSource namens `ProductsDataSource`zu binden.

[![erstellen Sie eine neue ObjectDataSource namens productdatasource.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource mit dem Namen "`ProductsDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))

Konfigurieren Sie ObjectDataSource so, dass die `ProductsBLL` Class s `GetProducts()`-Methode verwendet wird, um Produktinformationen abzurufen. Konzentrieren Sie sich in diesem Tutorial ausschließlich auf das Hinzufügen von Funktionen, und machen Sie sich keine Gedanken über das Bearbeiten und löschen. Stellen Sie daher sicher, dass die Dropdown Liste auf der Registerkarte Einfügen auf `AddProduct()` festgelegt ist und dass die Dropdown Listen auf den Registerkarten aktualisieren und löschen auf (keine) festgelegt sind.

[![ordnen Sie die addProduct-Methode der ObjectDataSource s Insert ()-Methode zu.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Abbildung 3**: Zuordnen der `AddProduct` Methode zur ObjectDataSource s `Insert()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))

[![die Dropdown Listen Update und DELETE Registerkarten auf (keine) festgelegt.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Abbildung 4**: Festlegen der Dropdown Listen für das Aktualisieren und Löschen von Registerkarten auf (keine) ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))

Nachdem der Assistent zum Konfigurieren der Datenquelle für ObjectDataSource s abgeschlossen ist, fügt Visual Studio der GridView automatisch Felder für jedes der entsprechenden Datenfelder hinzu. Belassen Sie vorerst alle Felder, die von Visual Studio hinzugefügt werden. Später in diesem Tutorial werden einige der Felder entfernt, deren Werte beim Hinzufügen eines neuen Datensatzes nicht angegeben werden müssen.

Da es in der Datenbank fast 80 Produkte gibt, muss ein Benutzer bis zum Ende der Webseite einen Bildlauf nach unten durchführen, um einen neuen Datensatz hinzuzufügen. Aktivieren Sie daher das Paging, um die einfügeschnittstelle besser sichtbar und zugänglich zu machen. Um das Paging zu aktivieren, aktivieren Sie einfach das Kontrollkästchen Paging aktivieren aus dem GridView s-Smarttag.

An diesem Punkt sollten das deklarative Markup von GridView und ObjectDataSource in etwa wie folgt aussehen:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]

[![alle Product Data-Felder in einer auslagerbare GridView angezeigt werden.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Abbildung 5**: alle Product Data-Felder werden in einer auslagerbare GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))

## <a name="step-2-adding-a-footer-row"></a>Schritt 2: Hinzufügen einer Footerzeile

Zusammen mit der Kopfzeile und den Daten Zeilen enthält die GridView eine Footerzeile. Die Kopf-und footerzeilen werden in Abhängigkeit von den Werten der Eigenschaften GridView s [`ShowHeader`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showheader.aspx) und [`ShowFooter`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showfooter.aspx) angezeigt. Um die Fußzeile anzuzeigen, legen Sie einfach die `ShowFooter`-Eigenschaft auf `True`fest. Wie in Abbildung 6 veranschaulicht, wird durch das Festlegen der `ShowFooter`-Eigenschaft auf `True` dem Raster eine Fußzeile hinzugefügt.

[![, um die Fußzeile anzuzeigen, legen Sie ShowFooter auf true fest.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Abbildung 6**: um die Fußzeile anzuzeigen, legen Sie `ShowFooter` auf `True` fest ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png)).

Beachten Sie, dass die Footerzeile eine dunkelrote Hintergrundfarbe aufweist. Der Grund hierfür ist das datawebcontrols-Design, das wir erstellt und auf alle Seiten im Tutorial [Anzeigen von Daten mit dem ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) -Tutorial angewendet haben. Insbesondere die `GridView.skin` Datei konfiguriert die `FooterStyle`-Eigenschaft, die die `FooterStyle` CSS-Klasse verwendet. Die `FooterStyle`-Klasse wird wie folgt in `Styles.css` definiert:

[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Wir haben mit der GridView-Footerzeile in vorherigen Tutorials untersucht. Wenn erforderlich, lesen Sie die [Informationen zur Anzeige von Zusammenfassungs Informationen im Lernprogramm von GridView im Footer-](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Tutorial.

Nachdem Sie die `ShowFooter`-Eigenschaft auf `True`festgelegt haben, nehmen Sie sich einen Moment Zeit, um die Ausgabe in einem Browser anzuzeigen. Die Footerzeile enthält derzeit keine Text-oder websteuer Elemente. In Schritt 3 ändern wir die Fußzeile für die einzelnen GridView-Felder, sodass Sie die entsprechende einfügeschnittstelle enthalten.

[![die leere Footerzeile oberhalb der Paging-Schnittstellen Steuerelemente angezeigt wird.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Abbildung 7**: die leere Footerzeile wird oberhalb der Paging-Schnittstellen Steuerelemente angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))

## <a name="step-3-customizing-the-footer-row"></a>Schritt 3: Anpassen der Footerzeile

Wieder im Tutorial [Verwenden von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Element wurde erläutert, wie die Anzeige einer bestimmten GridView-Spalte mithilfe von templatefields (im Gegensatz zu boundfields oder checkboxfields) erheblich angepasst wird. beim [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) haben wir uns mit der Verwendung von templatefields zum Anpassen der Bearbeitungs Schnittstelle in einer GridView beschäftigt. Denken Sie daran, dass ein TemplateField aus einer Reihe von Vorlagen besteht, die die Mischung aus Markup, websteuer Elementen und Datenbindung-Syntax definieren, die für bestimmte Arten von Zeilen verwendet wird. Der `ItemTemplate`gibt z. b. die Vorlage an, die für schreibgeschützte Zeilen verwendet wird, während der `EditItemTemplate` die Vorlage für die bearbeitbare Zeile definiert.

Zusammen mit dem `ItemTemplate` und `EditItemTemplate`enthält das TemplateField auch eine `FooterTemplate`, die den Inhalt für die Footerzeile angibt. Aus diesem Grund können wir die websteuer Elemente hinzufügen, die für jede field s-`FooterTemplate`einfügeschnittstelle erforderlich sind. Konvertieren Sie zunächst alle Felder in der GridView in templatefields. Klicken Sie hierzu in der GridView-Smarttags auf den Link Spalten bearbeiten, wählen Sie in der unteren linken Ecke jedes Feld aus, und klicken Sie auf das Feld dieses Feld in einen TemplateField-Link konvertieren.

![Jedes Feld in ein TemplateField-Element konvertieren](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Abbildung 8**: konvertieren jedes Felds in ein TemplateField-Element

Wenn Sie auf das Feld dieses Feld in ein TemplateField konvertieren klicken, wird der aktuelle Feldtyp in ein entsprechendes TemplateField-Element umgewandelt. Beispielsweise wird jedes BoundField durch ein TemplateField-Element mit einem `ItemTemplate` ersetzt, das eine Bezeichnung enthält, die das entsprechende Datenfeld anzeigt, und eine `EditItemTemplate`, die das Datenfeld in einem Textfeld anzeigt. Das `ProductName` BoundField wurde in das folgende TemplateField-Markup konvertiert:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Entsprechend wurde das `Discontinued` CheckBoxField in ein TemplateField konvertiert, dessen `ItemTemplate` und `EditItemTemplate` ein CheckBox-websteuer Element enthalten (bei deaktiviertem Kontrollkästchen `ItemTemplate` s). Das schreibgeschützte `ProductID` BoundField wurde in ein TemplateField-Steuerelement mit einem Label-Steuerelement sowohl in der `ItemTemplate` als auch in der `EditItemTemplate`konvertiert. Kurz gesagt: das Konvertieren eines vorhandenen GridView-Felds in ein TemplateField ist eine schnelle und einfache Möglichkeit, auf das anpassbare TemplateField-Element zu wechseln, ohne die vorhandenen Field s-Funktionen zu verlieren.

Da die GridView, mit der wir arbeiten, das Bearbeiten nicht unterstützt, können Sie die `EditItemTemplate` aus jedem TemplateField entfernen, sodass nur die `ItemTemplate`. Danach sollte das deklarative Markup der GridView s wie folgt aussehen:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Nun, da jedes GridView-Feld in ein TemplateField-Element konvertiert wurde, können wir die entsprechende einfügeschnittstelle in die einzelnen Feld-s-`FooterTemplate`eingeben. Einige der Felder haben keine einfügeschnittstelle (z. & #`ProductID`). andere unterscheiden sich in den websteuer Elementen, die zum Erfassen der neuen Produktinformationen verwendet werden.

Um die Bearbeitungs Schnittstelle zu erstellen, wählen Sie den Link Vorlagen bearbeiten aus dem GridView s-Smarttag. Wählen Sie dann in der Dropdown Liste das entsprechende Feld s `FooterTemplate` aus, und ziehen Sie das entsprechende Steuerelement aus der Toolbox auf den Designer.

[![dem Feld "FooterTemplate" die entsprechende einfügeschnittstelle hinzufügen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Abbildung 9**: Hinzufügen der entsprechenden einfügeschnittstelle zu jedem Feld s `FooterTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))

In der folgenden Aufzählung werden die GridView-Felder aufgelistet, die die hinzu zufügende einfügeschnittstelle angeben:

- `ProductID` keine.
- `ProductName` fügen Sie ein Textfeld hinzu, und legen Sie dessen `ID` auf `NewProductName`fest. Fügen Sie auch ein Steuerelement "Requirements dfieldvalidator" hinzu, um sicherzustellen, dass der Benutzer einen Wert für den neuen Produktnamen eingibt.
- `SupplierID` keine.
- `CategoryID` keine.
- `QuantityPerUnit` fügen Sie ein Textfeld hinzu, und legen Sie dessen `ID` auf `NewQuantityPerUnit`fest.
- `UnitPrice` Hinzufügen eines Textfelds mit dem Namen `NewUnitPrice` und einem CompareValidator, das sicherstellt, dass der eingegebene Wert ein Währungswert größer oder gleich 0 (null) ist.
- `UnitsInStock` ein Textfeld verwenden, dessen `ID` auf `NewUnitsInStock`festgelegt ist. Fügen Sie ein CompareValidator ein, das sicherstellt, dass der eingegebene Wert ein ganzzahliger Wert größer oder gleich 0 (null) ist.
- `UnitsOnOrder` ein Textfeld verwenden, dessen `ID` auf `NewUnitsOnOrder`festgelegt ist. Fügen Sie ein CompareValidator ein, das sicherstellt, dass der eingegebene Wert ein ganzzahliger Wert größer oder gleich 0 (null) ist.
- `ReorderLevel` ein Textfeld verwenden, dessen `ID` auf `NewReorderLevel`festgelegt ist. Fügen Sie ein CompareValidator ein, das sicherstellt, dass der eingegebene Wert ein ganzzahliger Wert größer oder gleich 0 (null) ist.
- `Discontinued` fügen Sie ein Kontrollkästchen hinzu, indem Sie dessen `ID` auf `NewDiscontinued`festlegen.
- `CategoryName` eine Dropdown List hinzufügen und deren `ID` auf `NewCategoryID`festlegen. Binden Sie es an eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`, und konfigurieren Sie Sie so, dass Sie die `CategoriesBLL` Class s `GetCategories()`-Methode verwendet. Lassen Sie die Dropdown List s `ListItem` s das `CategoryName` Datenfeld anzeigen, indem Sie das `CategoryID` Datenfeld als Werte verwenden.
- `SupplierName` eine Dropdown List hinzufügen und deren `ID` auf `NewSupplierID`festlegen. Binden Sie es an eine neue ObjectDataSource mit dem Namen `SuppliersDataSource`, und konfigurieren Sie Sie so, dass Sie die `SuppliersBLL` Class s `GetSuppliers()`-Methode verwendet. Lassen Sie die Dropdown List s `ListItem` s das `CompanyName` Datenfeld anzeigen, indem Sie das `SupplierID` Datenfeld als Werte verwenden.

Deaktivieren Sie für jedes Validierungs Steuerelement die `ForeColor`-Eigenschaft, sodass die weiß Vordergrundfarbe der `FooterStyle` CSS-Klasse anstelle der standardmäßigen roten verwendet wird. Verwenden Sie außerdem die `ErrorMessage`-Eigenschaft für eine ausführliche Beschreibung, legen Sie die `Text`-Eigenschaft jedoch auf ein Sternchen fest. Um zu verhindern, dass der Text der Validierungs Steuerung den Text der Einfügungs Schnittstelle in zwei Zeilen umschließt, legen Sie die `FooterStyle` s `Wrap`-Eigenschaft für jede der `FooterTemplate` en, die ein Validierungs Steuerelement verwenden, auf false fest. Fügen Sie abschließend ein ValidationSummary-Steuerelement unterhalb von GridView hinzu, und legen Sie dessen `ShowMessageBox`-Eigenschaft auf `True` und dessen `ShowSummary`-Eigenschaft auf `False`fest.

Beim Hinzufügen eines neuen Produkts müssen wir die `CategoryID` und `SupplierID`bereitstellen. Diese Informationen werden über die Dropdown Listen in den Fußzeile-Zellen der Felder `CategoryName` und `SupplierName` aufgezeichnet. Ich habe mich entschieden, diese Felder im Gegensatz zum `CategoryID` und `SupplierID` templatefields zu verwenden, da der Benutzer in den Daten Zeilen des Datenblatts wahrscheinlich eher die Kategorie-und Lieferanten Namen als die ID-Werte anzeigen möchte. Da die Werte für "`CategoryID`" und "`SupplierID`" nun in der `CategoryName` aufgezeichnet werden, und `SupplierName` Field s-Schnittstellen einfügen, können wir die `CategoryID` und `SupplierID` templatefields aus der GridView entfernen.

Ebenso wird der `ProductID` nicht verwendet, wenn ein neues Produkt hinzugefügt wird, sodass die `ProductID` TemplateField ebenfalls entfernt werden kann. Lassen Sie jedoch das `ProductID` Feld im Raster. Zusätzlich zu den Textfeldern, Dropdown Listen, Kontrollkästchen und Validierungs Steuerelementen, die die einfügeschnittstelle bilden, benötigen wir auch eine Schaltfläche "hinzufügen", die beim Klicken auf die Logik zum Hinzufügen des neuen Produkts zur Datenbank führt. In Schritt 4 fügen wir eine Schaltfläche "hinzufügen" in die einfügeschnittstelle in der `ProductID` TemplateField s-`FooterTemplate`ein.

Sie können die Darstellung der verschiedenen GridView-Felder verbessern. Beispielsweise können Sie die `UnitPrice` Werte als Währung formatieren, die `UnitsInStock`-, `UnitsOnOrder`-und `ReorderLevel` Felder rechtsbündig ausrichten und die `HeaderText` Werte für templatefields aktualisieren.

Nachdem Sie die gekoppelten Schnittstellen in den `FooterTemplate` en eingefügt, die `SupplierID`und die `CategoryID` templatefields entfernt und die Ästhetik des Rasters durch formatieren und Ausrichten von templatefields verbessert haben, sollte Ihr deklaratives Markup der GridView s in etwa wie folgt aussehen:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Wenn die GridView-Footerzeile in einem Browser angezeigt wird, enthält Sie nun die abgeschlossene einfügeschnittstelle (siehe Abbildung 10). An diesem Punkt schließt die einfügende Schnittstelle keine Mittel für den Benutzer ein, um anzugeben, dass Sie die Daten für das neue Produkt eingegeben hat und einen neuen Datensatz in die Datenbank einfügen möchte. Außerdem wird erläutert, wie die in den Fußzeile eingegebenen Daten in einen neuen Datensatz in der `Products` Datenbank übersetzt werden. In Schritt 4 sehen Sie, wie Sie eine Schaltfläche "hinzufügen" in die einfügeschnittstelle einschließen und Code beim Postback ausführen, wenn auf Sie geklickt wird. In Schritt 5 wird gezeigt, wie ein neuer Datensatz mit den Daten aus der Fußzeile eingefügt wird.

[![der GridView-Footer eine Schnittstelle zum Hinzufügen eines neuen Datensatzes bereitstellt.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Abbildung 10**: der GridView-Footer stellt eine Schnittstelle zum Hinzufügen eines neuen Datensatzes bereit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))

## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Schritt 4: einschließen der Schaltfläche "hinzufügen" in die einfügende

Wir müssen eine Schaltfläche "hinzufügen" an eine beliebige Stelle in der einfügeschnittstelle einfügen, da die einfügende Schnittstelle der Fußzeile-Zeile für den Benutzer keine Möglichkeit hat, anzugeben, dass die Eingabe der neuen Produktinformationen abgeschlossen wurde. Diese kann in einer der vorhandenen `FooterTemplate` s platziert werden, oder wir könnten dem Raster für diesen Zweck eine neue Spalte hinzufügen. Legen Sie in diesem Tutorial die Schaltfläche "hinzufügen" in die `ProductID` TemplateField s `FooterTemplate`.

Klicken Sie im Designer auf den Link Vorlagen bearbeiten im GridView s-Smarttag, und wählen Sie dann in der Dropdown Liste die `FooterTemplate` `ProductID` Felder aus. Fügen Sie der Vorlage ein Schaltflächen-websteuer Element (oder einen LinkButton oder ImageButton, falls gewünscht) hinzu, und legen Sie die zugehörige ID auf `AddProduct`, die einzufügende `CommandName` und deren `Text`-Eigenschaft hinzu, wie in Abbildung 11 dargestellt.

[![die Schaltfläche "hinzufügen" in der "ProductID TemplateField s FooterTemplate" platzieren.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Abbildung 11**: Platzieren der Schaltfläche "hinzufügen" in der `ProductID` TemplateField s `FooterTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))

Nachdem Sie die Schaltfläche Hinzufügen eingefügt haben, testen Sie die Seite in einem Browser. Beachten Sie, dass beim Klicken auf die Schaltfläche hinzufügen mit ungültigen Daten in der einfügenden Schnittstelle das Postback kurzgeschlossen wird und das ValidationSummary-Steuerelement die ungültigen Daten anzeigt (siehe Abbildung 12). Wenn Sie die entsprechenden Daten eingegeben haben, bewirkt das Klicken auf die Schaltfläche hinzufügen ein Postback Der Datenbank wird jedoch kein Datensatz hinzugefügt. Wir müssen etwas Code schreiben, um die Einfügung tatsächlich auszuführen.

[![das Postback der Schaltfläche "Schaltfläche hinzufügen" kurzgeschlossen wird, wenn die einfügende Schnittstelle ungültige Daten enthält.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Abbildung 12**: das Postback der Schaltfläche "Schaltfläche hinzufügen" ist kurz, wenn in der einfügeschnittstelle ungültige Daten vorhanden sind ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png)).

> [!NOTE]
> Die Validierungs Steuerelemente in der einfügeschnittstelle wurden keiner Validierungs Gruppe zugewiesen. Dies funktioniert gut, solange die Einfügungs Schnittstelle der einzige Satz von Validierungs Steuerelementen auf der Seite ist. Wenn es jedoch andere Validierungs Steuerelemente auf der Seite gibt (z. b. Validierungs Steuerelemente in der Bearbeitungs Schnittstelle des Raster s), sollten den Validierungs Steuerelementen in der einfügeschnittstelle und der Schaltfläche "Add Button s" `ValidationGroup`-Eigenschaften derselbe Wert zugewiesen werden, um diese Steuerelemente einer bestimmten Validierungs Gruppe zuzuordnen. Weitere Informationen zum Partitionieren der Validierungs Steuerelemente und Schaltflächen auf einer Seite in Validierungs Gruppen finden Sie unter [Zerlegen der Validierungs Steuerelemente in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) .

## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Schritt 5: Einfügen eines neuen Datensatzes in die`Products`Tabelle

Bei Verwendung der integrierten Bearbeitungsfunktionen von GridView verarbeitet das GridView-Steuerelemente automatisch alle notwendigen Arbeiten zum Ausführen des Updates. Wenn auf die Schaltfläche Aktualisieren geklickt wird, werden die von der Bearbeitungs Schnittstelle eingegebenen Werte in die Parameter in der `UpdateParameters` Auflistung von ObjectDataSource kopiert, und das Update wird durch Aufrufen der `Update()` Methode ObjectDataSource s gestartet. Da die GridView nicht so integrierte Funktionen zum Einfügen bereitstellt, müssen wir Code implementieren, der die ObjectDataSource s-`Insert()`-Methode aufruft und die Werte aus der einfügeschnittstelle in die ObjectDataSource s-`InsertParameters` Auflistung kopiert.

Diese Einfügungs Logik sollte ausgeführt werden, nachdem auf die Schaltfläche Hinzufügen geklickt wurde. Wie in den Schaltflächen [Hinzufügen und reagieren auf Schaltflächen in einem GridView-](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) Tutorial erläutert wird, wird das Ereignis GridView s `RowCommand` Ereignis beim Postback ausgelöst. Dieses Ereignis wird ausgelöst, wenn die Schaltfläche "Schaltfläche", "LinkButton" oder "ImageButton" explizit hinzugefügt wurde, wie z. b. die Schaltfläche "hinzufügen" in der Footerzeile oder wenn Sie automatisch von der GridView hinzugefügt wurde (z. b. die Link Schaltflächen oben in jeder Spalte LinkButtons in der pagingschnittstelle, wenn Paging aktivieren ausgewählt ist.

Um dem Benutzer zu antworten, indem er auf die Schaltfläche hinzufügen klickt, muss daher ein Ereignishandler für das GridView s `RowCommand`-Ereignis erstellt werden. Da dieses Ereignis ausgelöst wird, wenn in der GridView auf *eine beliebige* Schaltfläche, eine linktaste oder ein ImageButton geklickt wird, ist es wichtig, dass wir nur mit der Einfügungs Logik fortfahren, wenn die `CommandName`-Eigenschaft, die an den Ereignishandler weitergegeben wird, dem `CommandName` Wert der Schaltfläche hinzufügen (einfügen) Außerdem sollten wir nur fortfahren, wenn die Validierungs Steuerelemente gültige Daten melden. Um dies zu ermöglichen, erstellen Sie einen Ereignishandler für das `RowCommand`-Ereignis mit folgendem Code:

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Vielleicht Fragen Sie sich vielleicht, warum der Ereignishandler das Überprüfen der `Page.IsValid`-Eigenschaft stört. Schließlich wird das Postback nicht unterdrückt, wenn in der einfügeschnittstelle ungültige Daten bereitgestellt werden? Diese Annahme ist korrekt, solange der Benutzer JavaScript nicht deaktiviert hat oder Schritte zur Umgehung der Client seitigen Validierungs Logik durchgeführt hat. Kurz gesagt, Sie sollten sich nie strikt auf die Client seitige Validierung verlassen. eine serverseitige Überprüfung der Gültigkeit sollte vor dem Arbeiten mit den Daten immer durchgeführt werden.

In Schritt 1 haben wir den `ProductsDataSource` ObjectDataSource so erstellt, dass seine `Insert()`-Methode der `AddProduct`-Methode der `ProductsBLL`-Klasse zugeordnet ist. Um den neuen Datensatz in die `Products` Tabelle einzufügen, können wir einfach die ObjectDataSource s-`Insert()` Methode aufrufen:

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Nachdem nun die `Insert()`-Methode aufgerufen wurde, besteht nur noch die Möglichkeit, die Werte aus der einfügeschnittstelle in die Parameter zu kopieren, die an die `AddProduct`-Methode der `ProductsBLL`-Klasse weitergegeben wurden. Wie Sie in der unter [suchung der Ereignisse im Zusammenhang mit dem Einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) Lernprogrammen gesehen haben, können Sie dies über das Ereignis "ObjectDataSource s `Inserting`" erreichen. Im `Inserting`-Ereignis müssen wir Programm gesteuert auf die Steuerelemente aus der `Products` GridView-Footerzeile verweisen und deren Werte der `e.InputParameters`-Auflistung zuweisen. Wenn der Benutzer einen Wert auslässt, z. b. das Textfeld `ReorderLevel` leer lassen, müssen Sie angeben, dass der in die Datenbank eingefügte Wert `NULL`werden soll. Da die `AddProducts`-Methode Werte zulässt-Typen für Datenbankfelder zulässt, die NULL-Werte zulassen, verwenden Sie einfach einen Typ, der NULL-Werte zulässt, und legen Sie den Wert auf `Nothing` fest, wenn Benutzereingaben ausgelassen werden

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Nachdem der `Inserting`-Ereignishandler abgeschlossen ist, können der `Products` Datenbanktabelle neue Datensätze über die GridView-Footerzeile hinzugefügt werden. Versuchen Sie, mehrere neue Produkte hinzuzufügen.

## <a name="enhancing-and-customizing-the-add-operation"></a>Erweitern und Anpassen des Add-Vorgangs

Wenn Sie auf die Schaltfläche Hinzufügen klicken, wird der Datenbanktabelle ein neuer Datensatz hinzugefügt, es wird jedoch kein visuelles Feedback bereitgestellt, dass der Datensatz erfolgreich hinzugefügt wurde. Im Idealfall würde ein Bezeichnungsfeld-websteuer Element oder ein Client seitiges Benachrichtigungs Feld den Benutzer darüber informieren, dass der Einfügevorgang erfolgreich abgeschlossen wurde. Ich lasse dies als Übung für den Reader aus.

Die in diesem Tutorial verwendete GridView wendet keine Sortierreihenfolge auf die aufgelisteten Produkte an und ermöglicht dem Endbenutzer auch nicht das Sortieren der Daten. Folglich werden die Datensätze nach ihrem Primärschlüssel Feld in der Datenbank angeordnet. Da jeder neue Datensatz einen `ProductID` Wert hat, der größer als der letzte ist, wird jedes Mal, wenn ein neues Produkt hinzugefügt wird, an das Ende des Rasters geackt. Daher empfiehlt es sich, den Benutzer automatisch an die letzte Seite der GridView zu senden, nachdem ein neuer Datensatz hinzugefügt wurde. Dies kann erreicht werden, indem nach dem `ProductsDataSource.Insert()` im `RowCommand`-Ereignishandler die folgende Codezeile hinzugefügt wird, um anzugeben, dass der Benutzer nach dem Binden der Daten an die GridView an die letzte Seite gesendet werden muss:

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` ist eine boolesche Variable auf Seitenebene, der anfänglich der Wert `False`zugewiesen wird. Wenn `SendUserToLastPage` in der GridView s `DataBound`-Ereignishandler false ist, wird die `PageIndex`-Eigenschaft aktualisiert, um den Benutzer an die letzte Seite zu senden.

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Der Grund dafür, dass die `PageIndex`-Eigenschaft im `DataBound`-Ereignishandler festgelegt wird (im Gegensatz zum `RowCommand`-Ereignishandler), liegt daran, dass beim Auslösen des `RowCommand` Ereignis Handlers noch der neue Datensatz der `Products` Datenbanktabelle hinzugefügt wird. Daher stellt der letzte Seitenindex (`PageCount - 1`) im `RowCommand`-Ereignishandler den letzten Seitenindex dar, *bevor* das neue Produkt hinzugefügt wurde. Für die Mehrzahl der hinzugefügten Produkte ist der letzte Seitenindex nach dem Hinzufügen des neuen Produkts identisch. Wenn das hinzugefügte Produkt jedoch zu einem neuen letzten Seitenindex führt, wird der `PageIndex` im `RowCommand`-Ereignishandler nicht ordnungsgemäß aktualisiert, dann werden wir auf die zweite Seite (der letzte Seitenindex vor dem Hinzufügen des neuen Produkts) anstatt auf den neuen letzten Seitenindex weitergeleitet. Da der `DataBound`-Ereignishandler ausgelöst wird, nachdem das neue Produkt hinzugefügt wurde und die Daten an das Raster wiedergegeben wurden, indem Sie die `PageIndex`-Eigenschaft festgelegt haben, wissen wir, dass wir den korrekten letzten Seitenindex erhalten.

Schließlich ist die in diesem Tutorial verwendete GridView aufgrund der Anzahl der Felder, die für das Hinzufügen eines neuen Produkts gesammelt werden müssen, ziemlich breit. Aufgrund dieser Breite könnte ein vertikales Layout für DetailsView bevorzugt werden. Die Gesamtbreite der GridView s kann reduziert werden, indem weniger Eingaben gesammelt werden. Vielleicht müssen wir beim Hinzufügen eines neuen Produkts nicht die Felder `UnitsOnOrder`, `UnitsInStock`und `ReorderLevel` erfassen. in diesem Fall könnten diese Felder aus der GridView entfernt werden.

Zum Anpassen der gesammelten Daten können wir einen der beiden folgenden Ansätze verwenden:

- Verwenden Sie weiterhin die `AddProduct`-Methode, die Werte für die Felder "`UnitsOnOrder`", "`UnitsInStock`" und "`ReorderLevel`" erwartet. Stellen Sie im `Inserting`-Ereignishandler hart codierte Standardwerte bereit, die für diese Eingaben verwendet werden sollen, die aus der einfügeschnittstelle entfernt wurden.
- Erstellen Sie eine neue Überladung der `AddProduct`-Methode in der `ProductsBLL`-Klasse, die keine Eingaben für die Felder `UnitsOnOrder`, `UnitsInStock`und `ReorderLevel` akzeptiert. Konfigurieren Sie dann auf der Seite ASP.net die ObjectDataSource für die Verwendung dieser neuen Überladung.

Beide Optionen funktionieren ebenfalls gleichermaßen. In den vorherigen Tutorials haben wir die letztgenannte Option verwendet, um mehrere über Ladungen für die `ProductsBLL` Klasse `UpdateProduct`-Methode zu erstellen.

## <a name="summary"></a>Summary

In der GridView fehlen die integrierten Einfügefunktionen in den DetailsView und FormView, aber mit etwas Aufwand kann eine einfügende Schnittstelle der Fußzeile hinzugefügt werden. Um die Footerzeile in einer GridView anzuzeigen, legen Sie einfach die `ShowFooter`-Eigenschaft auf `True`fest. Der Inhalt der Footerzeile kann für jedes Feld angepasst werden, indem das Feld in ein TemplateField-Element umgerechnet und die einfügeschnittstelle der `FooterTemplate`hinzugefügt wird. Wie in diesem Tutorial gezeigt, können die `FooterTemplate` Schaltflächen, Textfelder, Dropdown Listen, Kontrollkästchen, Datenquellen-Steuerelemente zum Auffüllen von datengesteuerten websteuer Elementen (z. b. Dropdown Listen) und Validierungs Steuerelemente enthalten. Zusammen mit Steuerelementen zum Erfassen der Benutzereingaben ist eine Schaltfläche "hinzufügen", "LinkButton" oder "ImageButton" erforderlich.

Wenn Sie auf die Schaltfläche Hinzufügen klicken, wird die ObjectDataSource s-`Insert()`-Methode aufgerufen, um den einfügeworkflow zu starten. Die ObjectDataSource ruft dann die konfigurierte Insert-Methode auf (die `ProductsBLL` Klasse s `AddProduct` Methode in diesem Tutorial). Wir müssen die Werte aus der GridView s-einfügeschnittstelle in die ObjectDataSource s-`InsertParameters` Auflistung kopieren, bevor die Insert-Methode aufgerufen wird. Dies kann durch Programm gesteuertes verweisen auf die websteuer Elemente der einfügeschnittstelle in den ObjectDataSource s-`Inserting` Ereignis Handlers erreicht werden.

In diesem Tutorial wird das Aussehen der Techniken zum Verbessern der Darstellung von GridView erläutert. In den nächsten Tutorials wird erläutert, wie Sie mit Binärdaten wie Bildern, PDF-Dateien, Word-Dokumenten usw. und den datenweb Steuerelementen arbeiten.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial ist Bernadette Leigh. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](adding-a-gridview-column-of-checkboxes-vb.md)
