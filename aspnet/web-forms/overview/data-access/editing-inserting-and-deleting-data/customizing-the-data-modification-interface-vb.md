---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Anpassen der Daten Änderungs Schnittstelle (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie die Schnittstelle eines bearbeitbaren GridView-Steuer Elements anpassen, indem Sie die Standard Steuerelemente "TextBox" und "CheckBox" durch alternati ersetzen...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec7bdde6b2bffbbda066b0441bbd36b7072197
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78479253"
---
# <a name="customizing-the-data-modification-interface-vb"></a>Anpassen der Oberfläche zum Ändern von Daten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) oder [PDF herunterladen](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> In diesem Tutorial wird erläutert, wie Sie die Schnittstelle eines bearbeitbaren GridView-Steuer Elements anpassen, indem Sie die Standard Steuerelemente "TextBox" und "CheckBox" durch alternative Eingabe-websteuer Elemente ersetzen.

## <a name="introduction"></a>Einführung

Die von den GridView-und DetailsView-Steuerelementen verwendeten boundfields und checkboxfields vereinfachen das Ändern von Daten aufgrund ihrer Fähigkeit zum Rendering Schreib geschützter, bearbeitbarer und eincheckbarer Schnittstellen. Diese Schnittstellen können ohne zusätzliches deklaratives Markup oder Code gerendert werden. Allerdings fehlt in den Schnittstellen von BoundField und CheckBoxField die Anpassbarkeit, die häufig in realen Szenarios benötigt wird. Um die bearbeitbare oder einfügbar-Schnittstelle in einem GridView-oder DetailsView-Element anzupassen, muss stattdessen ein TemplateField verwendet werden.

Im [vorherigen Tutorial](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) wurde erläutert, wie Sie die Schnittstellen für die Datenänderung anpassen, indem Sie Validierungs-websteuer Elemente hinzufügen. In diesem Tutorial erfahren Sie, wie Sie die eigentlichen Daten Sammlungs-websteuer Elemente anpassen und die standardmäßigen Textfeld-und CheckBox-Steuerelemente von BoundField und CheckBoxField durch alternative Eingabe-websteuer Elemente ersetzen. Insbesondere erstellen wir eine bearbeitbare GridView, die das Aktualisieren des Namens, der Kategorie, des Lieferanten und des nicht mehr unterstützten Zustands eines Produkts ermöglicht. Wenn Sie eine bestimmte Zeile bearbeiten, werden die Felder Kategorie und Lieferant als DropDownLists gerenstet, die die verfügbaren Kategorien und Lieferanten enthalten, aus denen Sie auswählen können. Außerdem ersetzen wir das Standard Kontrollkästchen CheckBoxField durch ein RadioButton List-Steuerelement, das zwei Optionen bietet: "aktiv" und "nicht mehr unterstützt".

[![die Bearbeitungs Schnittstelle von GridView Dropdown Listen und Radiobuttons enthält](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Abbildung 1**: die Bearbeitungs Schnittstelle der GridView enthält Dropdown Listen und Radiobuttons ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image3.png))

## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Schritt 1: Erstellen der entsprechenden`UpdateProduct`Überladung

In diesem Tutorial erstellen wir eine bearbeitbare GridView, die das Bearbeiten des Namens, der Kategorie, des Lieferanten und des Status eines Produkts ermöglicht. Daher benötigen wir eine `UpdateProduct` Überladung, die fünf Eingabeparameter akzeptiert. diese vier Produktwerte und die `ProductID`. Wie bei den vorherigen über Ladungen sieht dies wie folgt aus:

1. Abrufen der Produktinformationen für die angegebene `ProductID`aus der Datenbank
2. Aktualisieren Sie die Felder `ProductName`, `CategoryID`, `SupplierID`und `Discontinued`.
3. Senden Sie die Aktualisierungs Anforderung über die `Update()`-Methode des TableAdapter an die DAL.

Aus Gründen der Übersichtlichkeit habe ich bei dieser speziellen Überladung die Geschäftsregel Überprüfung ausgelassen, mit der sichergestellt wird, dass ein Produkt als nicht eingestellt gekennzeichnet ist Wenn Sie möchten, können Sie Sie in hinzufügen, oder Sie können die Logik im Idealfall auf eine separate Methode umgestalten.

Der folgende Code zeigt die neue `UpdateProduct` Überladung in der `ProductsBLL`-Klasse:

[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Schritt 2: Erstellen der bearbeitbaren GridView

Nachdem die `UpdateProduct` Überladung hinzugefügt wurde, können wir unsere bearbeitbare GridView erstellen. Öffnen Sie die Seite `CustomizedUI.aspx` im Ordner `EditInsertDelete`, und fügen Sie dem Designer ein GridView-Steuerelement hinzu. Erstellen Sie als nächstes eine neue ObjectDataSource aus dem Smarttag der GridView. Konfigurieren Sie ObjectDataSource zum Abrufen von Produktinformationen über die `GetProducts()`-Methode der `ProductBLL`-Klasse und zum Aktualisieren von Produktdaten mithilfe der soeben erstellten `UpdateProduct` Überladung. Wählen Sie auf der Registerkarte Einfügen und löschen in den Dropdown Listen die Option (keine) aus.

[![Konfigurieren von ObjectDataSource für die Verwendung der soeben erstellten UpdateProduct-Überladung](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Abbildung 2**: Konfigurieren von ObjectDataSource für die Verwendung der soeben erstellten `UpdateProduct` Überladung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image6.png))

Wie wir in den Tutorials für die Datenänderung gesehen haben, weist die deklarative Syntax für die von Visual Studio erstellte ObjectDataSource die `OldValuesParameterFormatString`-Eigenschaft `original_{0}`zu. Dies funktioniert natürlich nicht mit unserer Geschäftslogik Schicht, da unsere Methoden nicht erwarten, dass der ursprüngliche `ProductID` Wert weitergegeben wird. In den vorherigen Tutorials haben Sie daher einen Moment Zeit, um diese Eigenschaften Zuweisung aus der deklarativen Syntax zu entfernen, oder legen Sie den Wert dieser Eigenschaft auf `{0}`fest.

Nach dieser Änderung sollte das deklarative Markup von ObjectDataSource wie folgt aussehen:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Beachten Sie, dass die `OldValuesParameterFormatString`-Eigenschaft entfernt wurde und dass für jeden der von der `UpdateProduct` Überladung erwarteten Eingabeparameter ein `Parameter` in der `UpdateParameters`-Auflistung vorhanden ist.

Obwohl die ObjectDataSource so konfiguriert ist, dass nur eine Teilmenge der Produktwerte aktualisiert wird, zeigt die GridView aktuell *alle* Produktfelder an. Nehmen Sie sich einen Moment Zeit, um die GridView so zu bearbeiten:

- Sie enthält nur die `ProductName`, `SupplierName``CategoryName` boundfields und das `Discontinued` CheckBoxField.
- Die `CategoryName`-und `SupplierName` Felder, die vor (auf der linken Seite) der `Discontinued` CheckBoxField angezeigt werden.
- Die `CategoryName`-und `SupplierName` boundfields ' `HeaderText`-Eigenschaft ist auf ' Category ' bzw. ' Supplier ' festgelegt.
- Die Bearbeitungs Unterstützung ist aktiviert (aktivieren Sie das Kontrollkästchen "Bearbeiten aktivieren" in der GridView-Smarttag).

Nachdem diese Änderungen vorgenommen wurden, sieht der Designer ähnlich wie in Abbildung 3 aus, und die deklarative Syntax von GridView wird unten dargestellt.

[![entfernen Sie die nicht benötigten Felder aus der GridView.](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Abbildung 3**: Entfernen der nicht benötigten Felder aus der GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image9.png))

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

An diesem Punkt ist das schreibgeschützte Verhalten der GridView fertiggestellt. Beim Anzeigen der Daten wird jedes Produkt als Zeile in der GridView gerendert und zeigt den Namen, die Kategorie, den Lieferanten und den Status des Produkts an.

[![die schreibgeschützte Schnittstelle der GridView ist fertiggestellt.](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Abbildung 4**: die schreibgeschützte Schnittstelle der GridView ist vollständig ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image12.png))

> [!NOTE]
> Wie in [einer Übersicht über das Einfügen, aktualisieren und Löschen von Daten](an-overview-of-inserting-updating-and-deleting-data-cs.md)beschrieben, ist es von entscheidender Bedeutung, dass der GridView s-Ansichts Zustand aktiviert ist (das Standardverhalten). Wenn Sie die Eigenschaft GridView s `EnableViewState` auf `false`festlegen, besteht das Risiko, dass gleichzeitige Benutzerdaten Sätze versehentlich löschen oder bearbeiten. Weitere Informationen finden Sie [unter Warnung: Parallelitäts Problem mit ASP.NET 2,0 GridViews/DetailsView/formviews, die das Bearbeiten und/oder löschen unterstützen und deren Ansichts Zustand deaktiviert ist](http://scottonwriting.net/sowblog/posts/10054.aspx) .

## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Schritt 3: Verwenden einer Dropdown List für die Schnittstellen für die Kategorie-und Lieferanten Bearbeitung

Beachten Sie, dass das `ProductsRow`-Objekt `CategoryID`-, `CategoryName`-, `SupplierID`-und `SupplierName`-Eigenschaften enthält, die die tatsächlichen Fremdschlüssel-ID-Werte in der `Products` Datenbanktabelle und die entsprechenden `Name` Werte in den `Categories`-und `Suppliers`-Tabellen bereitstellen. Die `CategoryID` und `SupplierID` des `ProductRow`können aus gelesen und geschrieben werden, während die Eigenschaften `CategoryName` und `SupplierName` als schreibgeschützt gekennzeichnet sind.

Aufgrund des schreibgeschützten Status der Eigenschaften `CategoryName` und `SupplierName` wurde die `ReadOnly`-Eigenschaft der entsprechenden boundfields auf `True`festgelegt, wodurch verhindert wird, dass diese Werte geändert werden, wenn eine Zeile bearbeitet wird. Die `ReadOnly`-Eigenschaft kann zwar auf `False`festgelegt werden, wobei die `CategoryName` und `SupplierName` boundfields als Textfelder während der Bearbeitung gerendert werden. ein solcher Ansatz führt z. b. zu einer Ausnahme, wenn der Benutzer versucht, das Produkt zu aktualisieren, da keine `UpdateProduct` Überladung vorhanden ist, die `CategoryName` und `SupplierName` Eingaben annimmt. Diese Überladung soll aus zwei Gründen nicht erstellt werden:

- Die `Products` Tabelle enthält keine `SupplierName`-oder `CategoryName` Felder, sondern `SupplierID` und `CategoryID`. Daher möchten wir, dass unsere Methode diese speziellen ID-Werte und nicht die Werte ihrer Nachschlage Tabellen übermittelt.
- Der Benutzer muss den Namen des Lieferanten oder der Kategorie eingeben, da er die verfügbaren Kategorien und Lieferanten und deren korrekte Rechtschreibprüfung kennen muss.

In den Feldern "Lieferant" und "Category" sollten die Namen der Kategorie und der Lieferanten im schreibgeschützten Modus (wie jetzt) sowie eine Dropdown Liste der anwendbaren Optionen angezeigt werden, wenn Sie bearbeitet werden. Mithilfe einer Dropdown Liste kann der Endbenutzer schnell feststellen, welche Kategorien und Lieferanten zur Auswahl stehen, und er kann leichter seine Auswahl treffen.

Um dieses Verhalten bereitzustellen, müssen wir die `SupplierName` und `CategoryName` boundfields in templatefields konvertieren, deren `ItemTemplate` die Werte `SupplierName` und `CategoryName` ausgibt und deren `EditItemTemplate` ein Dropdown List-Steuerelement verwendet, um die verfügbaren Kategorien und Lieferanten aufzulisten.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Hinzufügen der`Categories`und`Suppliers`Dropdown Listen

Beginnen Sie, indem Sie die `SupplierName` und `CategoryName` boundfields in templatefields konvertieren, indem Sie auf den Link Spalten bearbeiten im Smarttag der GridView klicken. Auswählen von BoundField in der Liste unten links und klicken Sie auf den Link "dieses Feld in einen TemplateField konvertieren". Beim Konvertierungs Vorgang wird ein TemplateField-Element mit einem `ItemTemplate` und einem `EditItemTemplate`erstellt, wie in der deklarativen Syntax unten dargestellt:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Da das BoundField-Objekt schreibgeschützt ist, enthalten die `ItemTemplate` und `EditItemTemplate` ein Label-websteuer Element, dessen `Text`-Eigenschaft an das anwendbare Datenfeld gebunden ist (`CategoryName`in der obigen Syntax). Wir müssen die `EditItemTemplate`ändern und das Label-websteuer Element durch ein Dropdown List-Steuerelement ersetzen.

Wie in den vorherigen Tutorials gezeigt, kann die Vorlage über den Designer oder direkt über die deklarative Syntax bearbeitet werden. Wenn Sie die Datei über den Designer bearbeiten möchten, klicken Sie im Smarttag der GridView auf den Link Vorlagen bearbeiten, und wählen Sie die Option zum Arbeiten mit dem `EditItemTemplate`des Kategoriefelds aus. Entfernen Sie das Label-websteuer Element, und ersetzen Sie es durch ein Dropdown List-Steuerelement, indem Sie die Eigenschaft ID der Dropdown Liste auf `Categories`festlegen.

[![entfernen Sie die texbox, und fügen Sie der EditItemTemplate eine DropDownList hinzu.](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Abbildung 5**: Entfernen der texbox und Hinzufügen einer Dropdown List zum `EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image15.png))

Als nächstes müssen Sie die Dropdown Liste mit den verfügbaren Kategorien auffüllen. Klicken Sie in der Dropdown List-Smarttag auf den Link Datenquelle auswählen, und erstellen Sie eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`.

[![erstellen Sie ein neues ObjectDataSource-Steuerelement namens "kategoriesdatasource".](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Abbildung 6**: Erstellen eines neuen ObjectDataSource-Steuer Elements namens `CategoriesDataSource` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image18.png))

Damit diese ObjectDataSource alle Kategorien zurückgibt, binden Sie Sie an die `GetCategories()` Methode der `CategoriesBLL` Klasse.

[![die ObjectDataSource an die GetCategories ()-Methode von categoriesbll binden.](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Abbildung 7**: Binden von ObjectDataSource an die `GetCategories()`-Methode des `CategoriesBLL`([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image21.png))

Konfigurieren Sie abschließend die Einstellungen der Dropdown List, sodass das `CategoryName` Feld in jeder Dropdown List-`ListItem` angezeigt wird, wobei das `CategoryID` Feld als Wert verwendet wird.

[![, dass das Feld "CategoryName" und die CategoryID als Wert verwendet werden.](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Abbildung 8**: das `CategoryName` Feld wird angezeigt, und die `CategoryID` als Wert verwendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image24.png))

Nachdem Sie diese Änderungen vorgenommen haben, enthält das deklarative Markup für die `EditItemTemplate` in der `CategoryName` TemplateField sowohl eine Dropdown List als auch eine ObjectDataSource:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Der Ansichts Zustand der Dropdown List im `EditItemTemplate` muss aktiviert sein. In Kürze fügen wir eine Datenbindung-Syntax zur deklarativen Syntax von DropDownList hinzu, und Datenbindung-Befehle wie `Eval()` und `Bind()` können nur in Steuerelementen angezeigt werden, deren Ansichts Zustand aktiviert ist.

Wiederholen Sie diese Schritte, um dem `EditItemTemplate`des `SupplierName` TemplateField eine DropDownList mit dem Namen `Suppliers` hinzuzufügen. Dies umfasst das Hinzufügen einer Dropdown List zum `EditItemTemplate` und das Erstellen eines weiteren ObjectDataSource. Die ObjectDataSource der `Suppliers` DropDownList sollte jedoch so konfiguriert werden, dass die `GetSuppliers()`-Methode der `SuppliersBLL` Klasse aufgerufen wird. Konfigurieren Sie außerdem die Dropdown Liste `Suppliers`, um das Feld `CompanyName` anzuzeigen, und verwenden Sie das Feld `SupplierID` als Wert für die `ListItem` s.

Nachdem Sie die Dropdown Listen zu den beiden `EditItemTemplate` s hinzugefügt haben, laden Sie die Seite in einem Browser, und klicken Sie auf die Schaltfläche Bearbeiten für das Cajun Seasoning-Produkt von Chef Anton. Wie in Abbildung 9 gezeigt, werden die Kategorie-und Lieferanten Spalten des Produkts als Dropdown Listen mit den verfügbaren Kategorien und Anbietern gerendert, aus denen Sie auswählen können. Beachten Sie jedoch, dass die *ersten* Elemente in beiden Dropdown Listen standardmäßig ausgewählt sind (Getränke für die Kategorie und exotische Flüssigkeiten als Lieferant), auch wenn die Cajun-Saison von Chef Anton eine Condition ist, die von New Orleans Cajun-Köstlichkeiten bereitgestellt wird.

[Standardmäßig ist ![das erste Element in den Dropdown Listen ausgewählt.](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Abbildung 9**: das erste Element in den Dropdown Listen ist standardmäßig ausgewählt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image27.png))

Wenn Sie auf Aktualisieren klicken, werden Sie außerdem feststellen, dass die `CategoryID`-und `SupplierID` Werte des Produkts auf `NULL`festgelegt sind. Beide unerwünschten Verhalten sind darauf zurückzuführen, dass die Dropdown Listen in den `EditItemTemplate` s nicht an Datenfelder aus den zugrunde liegenden Produktdaten gebunden sind.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Binden der Dropdown Listen an die Datenfelder "`CategoryID`" und "`SupplierID`"

Damit die Kategorie-und Lieferanten-Dropdown Listen der bearbeiteten Produkte auf die entsprechenden Werte festgelegt werden und diese Werte nach dem Klicken auf Aktualisieren an die `UpdateProduct` Methode der BLL zurückgesendet werden, müssen wir die `SelectedValue` Eigenschaften der DropDownLists an die `CategoryID` und `SupplierID` Datenfelder mithilfe der bidirektionalen Datenbindung binden. Um dies mit der Dropdown Liste `Categories` zu erreichen, können Sie der deklarativen Syntax `SelectedValue='<%# Bind("CategoryID") %>'` direkt hinzufügen.

Alternativ können Sie die Datenbindung für DropDownList festlegen, indem Sie die Vorlage über den Designer bearbeiten und auf den Link "DataBindings bearbeiten" im Smarttags der Dropdown Liste klicken. Geben Sie als nächstes an, dass die `SelectedValue`-Eigenschaft mithilfe der bidirektionalen Datenbindung an das `CategoryID` Feld gebunden werden soll (siehe Abbildung 10). Wiederholen Sie entweder den deklarativen Prozess oder den Designer Prozess, um das `SupplierID` Datenfeld an die `Suppliers` Dropdown List zu binden.

[![die CategoryID mithilfe der bidirektionalen Datenbindung an die SelectedValue-Eigenschaft von DropDownList binden.](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Abbildung 10**: Binden der `CategoryID` an die `SelectedValue`-Eigenschaft der DropDownList mithilfe der bidirektionalen Datenbindung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image30.png))

Nachdem die Bindungen auf die `SelectedValue` Eigenschaften der beiden Dropdown Listen angewendet wurden, werden die Kategorie-und Lieferanten Spalten der bearbeiteten Produkte standardmäßig auf die Werte des aktuellen Produkts festgelegt. Nachdem Sie auf "Aktualisieren" geklickt haben, werden die `CategoryID`-und `SupplierID` Werte des ausgewählten Dropdown Listen Elements an die `UpdateProduct`-Methode übermittelt. Abbildung 11 zeigt das Tutorial, nachdem die Datenbindung-Anweisungen hinzugefügt wurden. Beachten Sie, dass die ausgewählten Dropdown Listenelemente für die Cajun-saisononing von Chef Anton ordnungsgemäß und New Orleans-Cajun-Informationen sind.

[Standardmäßig sind die aktuellen Kategorie-und Lieferanten Werte des bearbeiteten Produkts ![ausgewählt.](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Abbildung 11**: die aktuellen Kategorie-und Lieferanten Werte der bearbeiteten Produkte sind standardmäßig ausgewählt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image33.png))

## <a name="handlingnullvalues"></a>Behandeln von`NULL`Werten

Die Spalten "`CategoryID`" und "`SupplierID`" in der `Products` Tabelle können `NULL`werden, aber die Dropdown Listen in den `EditItemTemplate` s enthalten kein Listenelement, das einen `NULL` Wert darstellt. Dies hat zwei Konsequenzen:

- Der Benutzer kann die-Schnittstelle nicht verwenden, um die Kategorie oder den Lieferanten eines Produkts von einem nicht`NULL` Wert in einen `NULL` zu ändern.
- Wenn ein Produkt über eine `NULL` `CategoryID` oder `SupplierID`verfügt, wird durch Klicken auf die Schaltfläche "Bearbeiten" eine Ausnahme ausgelöst. Dies liegt daran, dass der von `CategoryID` (oder `SupplierID`) in der `Bind()` Anweisung zurückgegebene `NULL` Wert nicht einem Wert in der Dropdown List zugeordnet ist (DropDownList löst eine Ausnahme aus, wenn die `SelectedValue`-Eigenschaft auf einen Wert festgelegt ist, der *nicht* in der Auflistung von Listenelementen enthalten ist).

Um `NULL` `CategoryID` und `SupplierID`-Werte zu unterstützen, müssen Sie jeder Dropdown Liste einen weiteren `ListItem` hinzufügen, um den `NULL` Wert darzustellen. Im Lernprogramm zum [Filtern von Master-/detailfiltern mit einem Dropdown List](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) -Tutorial wurde gezeigt, wie Sie eine zusätzliche `ListItem` zu einer Daten gebundenen Dropdown List hinzufügen, die dazu beiträgt, die `AppendDataBoundItems`-Eigenschaft der DropDownList auf `True` zu setzen und die zusätzlichen `ListItem`manuell hinzuzufügen. In diesem vorherigen Tutorial haben wir jedoch eine `ListItem` mit einer `Value` von `-1`hinzugefügt. Durch die Daten Bindungs Logik in ASP.net wird jedoch automatisch eine leere Zeichenfolge in einen `NULL` Wert konvertiert und umgekehrt. Daher soll für dieses Tutorial die `Value` der `ListItem`eine leere Zeichenfolge sein.

Legen Sie zunächst fest, dass die Dropdown Listen-Eigenschaft `AppendDataBoundItems` auf `True`festgelegt ist. Fügen Sie als nächstes die `NULL` `ListItem` hinzu, indem Sie der Dropdown Liste das folgende `<asp:ListItem>` Element hinzufügen, damit das deklarative Markup wie folgt aussieht:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Ich habe mich entschieden, "(None)" als Textwert für diesen `ListItem`zu verwenden, aber Sie können ihn auch ändern, wenn Sie möchten.

> [!NOTE]
> Wie im Lernprogramm zum *Filtern von Master/Details mit einem Dropdown List* -Tutorial gezeigt, können `ListItem` s zu einer Dropdown List über den Designer hinzugefügt werden, indem Sie im Eigenschaftenfenster auf die `Items` Eigenschaft von DropDownList klicken (in der der `ListItem` Auflistungs-Editor angezeigt wird). Stellen Sie jedoch sicher, dass Sie die `NULL` `ListItem` für dieses Tutorial über die deklarative Syntax hinzufügen. Wenn Sie den `ListItem` Auflistungs-Editor verwenden, wird die `Value` Einstellung von der generierten deklarativen Syntax nicht vollständig ausgelassen, wenn eine leere Zeichenfolge zugewiesen wird. dabei wird ein deklaratives Markup wie: `<asp:ListItem>(None)</asp:ListItem>`erstellt. Obwohl dies harmlos aussehen kann, bewirkt der fehlende Wert, dass in der Dropdown List der Wert der `Text`-Eigenschaft verwendet wird. Dies bedeutet Folgendes: Wenn diese `NULL` `ListItem` ausgewählt ist, wird der Wert "(None)" versucht, dem `CategoryID`zugewiesen zu werden. Dies führt zu einer Ausnahme. Wenn Sie `Value=""`explizit festlegen, wird `CategoryID` ein `NULL` Wert zugewiesen, wenn der `NULL` `ListItem` ausgewählt wird.

Wiederholen Sie diese Schritte für die Dropdown Liste "Suppliers".

Mit dieser zusätzlichen `ListItem`kann die Bearbeitungs Schnittstelle nun `NULL` Werte den `CategoryID`-und `SupplierID` Feldern eines Produkts zuweisen, wie in Abbildung 12 dargestellt.

[![wählen Sie (keine) aus, um einen NULL-Wert für die Kategorie oder den Lieferanten eines Produkts zuzuweisen.](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Abbildung 12**: Wählen Sie (keine) aus, um einen `NULL` Wert für die Kategorie oder den Lieferanten eines Produkts zuzuweisen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image36.png)).

## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Schritt 4: Verwenden von Radiobuttons für den nicht mehr unterstützten Status

Das `Discontinued` Datenfeld "Products" wird derzeit mithilfe eines CheckBoxField ausgedrückt, das ein deaktiviertes Kontrollkästchen für die schreibgeschützten Zeilen und ein aktiviertes Kontrollkästchen für die Zeile, die bearbeitet wird, rendert. Obwohl diese Benutzeroberfläche häufig geeignet ist, können wir Sie bei Bedarf mithilfe eines TemplateField anpassen. In diesem Tutorial ändern wir das CheckBoxField in ein TemplateField-Steuerelement, das ein RadioButton List-Steuerelement mit zwei Optionen "aktiv" und "nicht eingestellt" verwendet, von denen der Benutzer den `Discontinued` Wert des Produkts angeben kann.

Konvertieren Sie zunächst das `Discontinued` CheckBoxField in ein TemplateField-Element, das ein TemplateField mit einem `ItemTemplate` und `EditItemTemplate`erstellt. Beide Vorlagen enthalten ein Kontrollkästchen, bei dem die `Checked`-Eigenschaft an das `Discontinued` Datenfeld gebunden ist. der einzige Unterschied besteht darin, dass die `Enabled`-Eigenschaft des `ItemTemplate`-Kontrollkästchens auf `False`festgelegt ist.

Ersetzen Sie das Kontrollkästchen sowohl im `ItemTemplate` als auch im `EditItemTemplate` durch ein RadioButton List-Steuerelement, indem Sie die `ID` Eigenschaften von RadioButton lists auf `DiscontinuedChoice`festlegen. Geben Sie als nächstes an, dass die RadioButton lists jeweils zwei Options Felder enthalten sollen, eine mit der Bezeichnung "aktiv" mit dem Wert "false" und eine mit der Bezeichnung "nicht eingestellt" mit dem Wert "true". Um dies zu erreichen, können Sie die `<asp:ListItem>` Elemente direkt über die deklarative Syntax eingeben oder den `ListItem` Auflistungs-Editor des Designers verwenden. In Abbildung 13 wird der `ListItem` Auflistungs-Editor angezeigt, nachdem die beiden Optionsfeld Optionen angegeben wurden.

[!["RadioButton List" aktive und nicht mehr unterstützte Optionen hinzufügen](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Abbildung 13**: Hinzufügen von aktiven und nicht mehr unterstützten Optionen zur RadioButton List ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image39.png))

Da das RadioButton List-Objekt im `ItemTemplate` nicht bearbeitbar sein sollte, legen Sie dessen `Enabled`-Eigenschaft auf `False`fest, und belassen Sie die `Enabled`-Eigenschaft `True` (Standardeinstellung) für die RadioButton List im `EditItemTemplate`. Dadurch werden die Options Felder in der nicht bearbeiteten Zeile schreibgeschützt, aber der Benutzer kann die RadioButton-Werte für die bearbeitete Zeile ändern.

Wir müssen die `SelectedValue` Eigenschaften der RadioButton List-Steuerelemente weiterhin zuweisen, damit das entsprechende Optionsfeld basierend auf dem `Discontinued` Datenfeld des Produkts ausgewählt wird. Wie bei den zuvor in diesem Tutorial untersuchten Dropdown Listen kann diese Datenbindung-Syntax entweder direkt dem deklarativen Markup oder über den Link "DataBindings bearbeiten" in den Smarttags der RadioButton lists hinzugefügt werden.

Nachdem Sie die beiden RadioButton lists hinzugefügt und konfiguriert haben, sollte das deklarative Markup des `Discontinued` TemplateField wie folgt aussehen:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Mit diesen Änderungen wurde die `Discontinued` Spalte aus einer Liste von Kontrollkästchen in eine Liste von Optionsfeld Paaren transformiert (siehe Abbildung 14). Wenn Sie ein Produkt bearbeiten, wird das entsprechende Optionsfeld ausgewählt, und der nicht mehr unterstützte Status kann aktualisiert werden, indem Sie das Optionsfeld andere aktivieren und auf Aktualisieren klicken.

[![die nicht mehr unterstützten Kontrollkästchen durch Optionsfeld Paare ersetzt wurden.](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Abbildung 14**: die nicht mehr unterstützten Kontrollkästchen wurden durch Optionsfeld Paare ersetzt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](customizing-the-data-modification-interface-vb/_static/image42.png))

> [!NOTE]
> Da die `Discontinued` Spalte in der `Products`-Datenbank keine `NULL` Werte aufweisen kann, müssen Sie sich keine Gedanken über die Erfassung `NULL` Informationen in der Schnittstelle machen. Wenn `Discontinued` Spalte jedoch `NULL` Werte enthalten kann, sollten Sie der Liste ein drittes Optionsfeld hinzufügen, dessen `Value` auf eine leere Zeichenfolge (`Value=""`) festgelegt ist, genau wie bei der Kategorie und den Lieferanten Dropdown Listen.

## <a name="summary"></a>Zusammenfassung

Obwohl BoundField und CheckBoxField automatisch schreibgeschützte, Bearbeitungs-und einfügeschnittstellen erzeugen, ist Ihnen die Anpassung nicht möglich. Häufig müssen wir jedoch die Bearbeitungs-oder einfügeschnittstelle anpassen und ggf. Validierungs Steuerelemente hinzufügen (wie im vorherigen Tutorial erläutert) oder indem wir die Benutzeroberfläche für die Datenerfassung angepasst haben (wie in diesem Tutorial erläutert). Das Anpassen der-Schnittstelle mit einem TemplateField-Element kann in den folgenden Schritten summiert werden:

1. Fügen Sie ein TemplateField-Element hinzu, oder konvertieren Sie ein vorhandenes BoundField oder CheckBoxField in ein TemplateField.
2. Erweitern Sie die Schnittstelle nach Bedarf
3. Binden der entsprechenden Datenfelder an die neu hinzugefügten websteuer Elemente mithilfe der bidirektionalen Datenbindung

Zusätzlich zur Verwendung der integrierten ASP.net-websteuer Elemente können Sie auch die Vorlagen eines TemplateField-Steuer Elements mit benutzerdefinierten, kompilierten Server Steuerelementen und Benutzer Steuerelementen anpassen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Weiter](implementing-optimistic-concurrency-vb.md)
