---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Batch Aktualisierung (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie mehrere Datenbankdaten Sätze in einem einzelnen Vorgang aktualisieren. In der Benutzeroberflächen Ebene erstellen wir eine GridView, in der jede Zeile bearbeitet werden kann. In den Daten...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: baaaf37c47cc57d90ea579a5c20949bf8cfc7a3c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583618"
---
# <a name="batch-updating-c"></a>Aktualisieren in Batches (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) oder [PDF herunterladen](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Erfahren Sie, wie Sie mehrere Datenbankdaten Sätze in einem einzelnen Vorgang aktualisieren. In der Benutzeroberflächen Ebene erstellen wir eine GridView, in der jede Zeile bearbeitet werden kann. In der Datenzugriffs Ebene werden mehrere Aktualisierungs Vorgänge in eine Transaktion eingeschlossen, um sicherzustellen, dass alle Updates erfolgreich sind oder für alle Updates ein Rollback ausgeführt wird.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](wrapping-database-modifications-within-a-transaction-cs.md) wurde erläutert, wie die Datenzugriffs Ebene erweitert wird, um Unterstützung für Datenbanktransaktionen hinzuzufügen. Datenbanktransaktionen garantieren, dass eine Reihe von Daten Änderungs Anweisungen als ein atomarer Vorgang behandelt werden, um sicherzustellen, dass alle Änderungen fehlschlagen oder alle erfolgreich ausgeführt werden. Mit dieser auf Low-Level-dal-Funktionalität haben wir nun die Aufmerksamkeit auf das Erstellen von Schnittstellen für die Batch Datenänderung vorbereitet.

In diesem Tutorial erstellen wir eine GridView, in der jede Zeile bearbeitet werden kann (siehe Abbildung 1). Da jede Zeile in der Bearbeitungs Schnittstelle gerendert wird, ist es nicht erforderlich, dass eine Spalte mit den Schaltflächen Bearbeiten, aktualisieren und Abbrechen vorhanden ist. Stattdessen werden auf der Seite zwei Schaltflächen zum Aktualisieren von Produkten angezeigt. Wenn Sie darauf klicken, werden die GridView-Zeilen aufgezählt, und die Datenbank wird aktualisiert.

[![jede Zeile in der GridView-Tabelle bearbeitet werden kann.](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Abbildung 1**: jede Zeile in der GridView-Tabelle kann bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image2.png))

Legen Sie Los!

> [!NOTE]
> Im Tutorial zum [Durchführen von Batch Updates](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) haben wir eine Batch Bearbeitungs Schnittstelle mit dem DataList-Steuerelement erstellt. Dieses Tutorial unterscheidet sich von dem vorherigen in, das eine GridView verwendet, und das Batch Update wird innerhalb des Umfangs einer Transaktion ausgeführt. Nachdem Sie dieses Tutorial durchgearbeitet haben, empfehlen wir Ihnen, zum vorherigen Tutorial zurückzukehren und es zu aktualisieren, um die im vorherigen Tutorial hinzugefügte Daten Bank Transaktions Funktionalität zu verwenden.

## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Untersuchen der Schritte, mit denen alle GridView-Zeilen bearbeitet werden

Wie in der Übersicht über das [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) beschrieben, bietet die GridView integrierte Unterstützung für die Bearbeitung der zugrunde liegenden Daten auf Zeilen Basis. Intern wird in der GridView festgelegt, welche Zeile durch die [`EditIndex`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx)bearbeitet werden kann. Wenn die GridView an die Datenquelle gebunden wird, wird jede Zeile überprüft, um festzustellen, ob der Index der Zeile dem Wert `EditIndex`entspricht. Wenn dies der Fall ist, werden die Zeilen Felder mithilfe Ihrer Bearbeitungs Schnittstellen gerendert. Bei boundfields ist die Bearbeitungs Schnittstelle ein Textfeld, dessen `Text`-Eigenschaft dem Wert des Daten Felds zugewiesen ist, das durch die Eigenschaft "BoundField s `DataField`" angegeben wird. Bei templatefields wird der `EditItemTemplate` anstelle des `ItemTemplate`verwendet.

Beachten Sie, dass der Bearbeitungs Workflow gestartet wird, wenn ein Benutzer auf die Schaltfläche "Bearbeiten" der Zeile klickt Dadurch wird ein Postback ausgelöst, die GridView s `EditIndex`-Eigenschaft auf den angeklickten Zeilen Index festgelegt, und die Daten werden an das Raster gebunden. Beim Klicken auf die Schaltfläche "Abbrechen" einer Zeile wird beim Postback der `EditIndex` auf den Wert `-1` festgelegt, bevor die Daten erneut an das Raster gebunden werden. Da in den GridView s-Zeilen die Indizierung bei Null beginnt, hat das Festlegen von `EditIndex` auf `-1` die Auswirkung, die GridView im schreibgeschützten Modus anzuzeigen.

Die `EditIndex`-Eigenschaft eignet sich gut für die Zeilen übergreifende Bearbeitung, ist aber nicht für die Batch Bearbeitung vorgesehen. Damit die gesamte GridView bearbeitbar ist, muss jede Zeile mit Ihrer Bearbeitungs Schnittstelle dargestellt werden. Dies lässt sich am einfachsten erreichen, indem Sie erstellen, wo jedes bearbeitbare Feld als TemplateField implementiert wird und seine Bearbeitungs Schnittstelle in der `ItemTemplate`definiert ist.

In den nächsten Schritten erstellen wir eine vollständig bearbeitbare GridView. In Schritt 1 erstellen wir zunächst die GridView und deren ObjectDataSource und konvertieren deren boundfields und CheckBoxField in templatefields. In den Schritten 2 und 3 verschieben wir die Bearbeitungs Schnittstellen aus den templatefields-`EditItemTemplate` s in Ihre `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Schritt 1: Anzeigen von Produktinformationen

Bevor wir uns Gedanken über das Erstellen einer GridView machen, in der die Zeilen bearbeitet werden können, lassen Sie zunächst die Produktinformationen anzeigen. Öffnen Sie die Seite `BatchUpdate.aspx` im Ordner `BatchData`, und ziehen Sie eine GridView aus der Toolbox auf den Designer. Legen Sie die `ID` GridView s auf `ProductsGrid` fest, und wählen Sie aus dem smarttagtag aus, dass es an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`gebunden werden soll. Konfigurieren Sie ObjectDataSource so, dass die Daten aus der `ProductsBLL` Klasse `GetProducts`-Methode abgerufen werden.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbll-Klasse.](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Abbildung 2**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image4.png))

[![Abrufen der Produktdaten mithilfe der GetProducts-Methode](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Abbildung 3**: Abrufen der Produktdaten mit der `GetProducts`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image6.png))

Wie bei GridView sind die Modifizierungs Features von ObjectDataSource für zeilenweise so konzipiert, dass Sie auf Zeilen Basis funktionieren. Um einen Satz von Datensätzen zu aktualisieren, müssen wir ein Bit Code in der Code Behind-Klasse der ASP.net page s schreiben, die die Daten stapelt und an die BLL übergibt. Legen Sie daher die Dropdown Listen in den Registerkarten Update, INSERT und DELETE von ObjectDataSource auf (keine) fest. Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen.

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Abbildung 4**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image8.png))

Nach Abschluss des Assistenten zum Konfigurieren von Datenquellen sollte das deklarative Markup von ObjectDataSource s wie folgt aussehen:

[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Wenn Sie den Assistenten zum Konfigurieren von Datenquellen abschließen, erstellt Visual Studio außerdem boundfields und ein CheckBoxField für die Product Data-Felder in der GridView. In diesem Tutorial dürfen die Benutzer nur den Namen, die Kategorie, den Preis und den Status des Produkts anzeigen und bearbeiten. Entfernen Sie alle Felder außer den Feldern `ProductName`, `CategoryName`, `UnitPrice`und `Discontinued`, und benennen Sie die `HeaderText` Eigenschaften der ersten drei Felder in Product, Category bzw. Price um. Aktivieren Sie abschließend die Kontrollkästchen Paging aktivieren und Sortierung aktivieren im Smarttag GridView s.

An diesem Punkt enthält die GridView drei boundfields (`ProductName`, `CategoryName`und `UnitPrice`) und ein CheckBoxField (`Discontinued`). Diese vier Felder müssen in templatefields konvertiert und dann die Bearbeitungs Schnittstelle von der `EditItemTemplate` TemplateField s in den `ItemTemplate`verschoben werden.

> [!NOTE]
> Wir haben das Erstellen und Anpassen von templatefields im Tutorial [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) untersucht. Wir führen Sie durch die Schritte zum Konvertieren von boundfields und CheckBoxField in templatefields und zum Definieren Ihrer Bearbeitungs Schnittstellen in ihren `ItemTemplate` en. Wenn Sie jedoch hängen bleiben oder ein Aktualisierungs Programm benötigen, können Sie sich nicht mehr auf dieses vorherige Tutorial zurückgreifen.

Klicken Sie in der GridView s-Smarttag auf den Link Spalten bearbeiten, um das Dialogfeld Felder zu öffnen. Wählen Sie als nächstes jedes Feld aus, und klicken Sie auf das Feld dieses Feld in einen TemplateField-Link konvertieren.

![Vorhandene boundfields und CheckBoxField in TemplateField konvertieren](batch-updating-cs/_static/image5.gif)

**Abbildung 5**: Konvertieren der vorhandenen boundfields und CheckBoxField in TemplateField

Da jedes Feld nun ein TemplateField ist, können Sie die Bearbeitungs Schnittstelle aus den `EditItemTemplate` s in die `ItemTemplate` s verschieben.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Schritt 2: Erstellen der Schnittstellen für`ProductName`,`UnitPrice`und`Discontinued`Bearbeitung

Das Erstellen der `ProductName`, `UnitPrice`und `Discontinued` Bearbeitungs Schnittstellen ist das Thema dieses Schritts und ist recht unkompliziert, da jede Schnittstelle bereits in der `EditItemTemplate`TemplateField s definiert ist. Das Erstellen der `CategoryName` Bearbeitungs Schnittstelle ist etwas komplizierter, da wir eine Dropdown Liste der entsprechenden Kategorien erstellen müssen. Diese `CategoryName` Bearbeitungs Schnittstelle wird in Schritt 3 behandelt.

Beginnen Sie mit der `ProductName` TemplateField. Klicken Sie in der GridView s-Smarttag auf den Link Vorlagen bearbeiten, und führen Sie einen Drilldown zum `ProductName` TemplateField s `EditItemTemplate`aus. Wählen Sie das Textfeld aus, kopieren Sie es in die Zwischenablage, und fügen Sie es dann in das `ProductName` TemplateField s-`ItemTemplate`ein. Ändern Sie die Eigenschaft TextBox s `ID` in `ProductName`.

Fügen Sie als nächstes dem `ItemTemplate` ein "Requirements dfieldvalidator" hinzu, um sicherzustellen, dass der Benutzer für jeden Produktnamen einen Wert bereitstellt. Legen Sie die `ControlToValidate`-Eigenschaft auf ProductName fest. die Eigenschaft `ErrorMessage`, auf die Sie den Namen des Produkts angeben müssen. und die `Text`-Eigenschaft, die \*werden soll. Nachdem Sie diese Ergänzungen an der `ItemTemplate`vorgenommen haben, sollte der Bildschirm in etwa wie in Abbildung 6 aussehen.

[![das Element ProductName TemplateField nun ein Textfeld und ein "Requirements dfieldvalidator" enthält.](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Abbildung 6**: die `ProductName` TemplateField enthält jetzt ein Textfeld und ein "Requirements dfieldvalidator" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image10.png)).

Kopieren Sie für die `UnitPrice` Bearbeitungs Schnittstelle zunächst das Textfeld aus der `EditItemTemplate` in die `ItemTemplate`. Platzieren Sie als nächstes ein $ vor dem Textfeld, und legen Sie dessen `ID`-Eigenschaft auf UnitPrice und dessen `Columns`-Eigenschaft auf 8 fest.

Fügen Sie auch ein CompareValidator zum `UnitPrice` s `ItemTemplate` hinzu, um sicherzustellen, dass der vom Benutzer eingegebene Wert ein gültiger Währungswert ist, der größer oder gleich $0,00 ist. Legen Sie die Eigenschaft Prüfungs s `ControlToValidate` auf UnitPrice fest, deren `ErrorMessage`-Eigenschaft auf Sie müssen einen gültigen Währungswert eingeben. Lassen Sie alle Währungssymbole aus., dessen `Text` Eigenschaft \*, die `Type` Eigenschaft für `Currency`, die `Operator`-Eigenschaft `GreaterThanEqual`und deren `ValueToCompare`-Eigenschaft auf 0 (null).

[![einen CompareValidator hinzufügen, um sicherzustellen, dass der eingegebene Preis ein nicht negativer Währungswert ist.](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Abbildung 7**: Hinzufügen eines CompareValidator, um sicherzustellen, dass der eingegebene Preis ein nicht negativer Währungswert ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image12.png))

Für das `Discontinued` TemplateField können Sie das Kontrollkästchen verwenden, das bereits in der `ItemTemplate`definiert ist. Legen Sie den `ID` einfach auf "nicht eingestellt" und dessen `Enabled`-Eigenschaft auf `true`fest.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Schritt 3: Erstellen der`CategoryName`Bearbeitungs Schnittstelle

Die Bearbeitungs Schnittstelle in der `CategoryName` TemplateField s `EditItemTemplate` enthält ein Textfeld, in dem der Wert des `CategoryName` Datenfelds angezeigt wird. Wir müssen dies durch eine Dropdown List ersetzen, die die möglichen Kategorien auflistet.

> [!NOTE]
> Das Tutorial zum [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) enthält eine ausführlichere und vollständigste Erläuterung zum Anpassen einer Vorlage, um eine Dropdown List im Gegensatz zu einem Textfeld einzuschließen. Obwohl die hier beschriebenen Schritte durchgeführt wurden, werden Sie auf die gleiche Weise dargestellt. Eine ausführlichere Betrachtung der Erstellung und Konfiguration der Dropdown List-Kategorien finden Sie im Tutorial [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

Ziehen Sie eine Dropdown List aus der Toolbox auf die `CategoryName` TemplateField s `ItemTemplate`, und legen Sie deren `ID` auf `Categories`fest. An diesem Punkt definieren wir in der Regel die DropDownLists s-Datenquelle über das Smarttags und erstellen eine neue ObjectDataSource. Dadurch wird jedoch die ObjectDataSource innerhalb der `ItemTemplate`hinzugefügt, was dazu führt, dass für jede Zeile der GridView-Instanz eine ObjectDataSource-Instanz erstellt wird. Erstellen Sie stattdessen die ObjectDataSource außerhalb der GridView s templatefields. Beenden Sie die Vorlagen Bearbeitung, und ziehen Sie ObjectDataSource aus der Toolbox auf den Designer unterhalb der `ProductsDataSource` ObjectDataSource. Benennen Sie den neuen ObjectDataSource-`CategoriesDataSource`, und konfigurieren Sie ihn so, dass er die `CategoriesBLL` Class s `GetCategories`-Methode verwendet.

[![Konfigurieren von ObjectDataSource für die Verwendung der Klasse "kategoriesbll"](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Abbildung 8**: Konfigurieren von ObjectDataSource für die Verwendung der `CategoriesBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image14.png))

[![die Kategoriedaten mithilfe der GetCategories-Methode abrufen.](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Abbildung 9**: Abrufen der Kategorieinformationen mithilfe der `GetCategories`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image16.png))

Da diese ObjectDataSource lediglich zum Abrufen von Daten verwendet wird, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren und löschen auf (keine) fest. Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen.

[![die Dropdown Listen auf den Registerkarten aktualisieren und löschen auf (keine) festgelegt.](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Abbildung 10**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image18.png))

Nachdem Sie den Assistenten abgeschlossen haben, sollte das deklarative Markup der `CategoriesDataSource` wie folgt aussehen:

[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Nachdem Sie die `CategoriesDataSource` erstellt und konfiguriert haben, kehren Sie zum `CategoryName` TemplateField s `ItemTemplate` zurück, und klicken Sie im Smarttag DropDownList s auf den Link Datenquelle auswählen. Wählen Sie im Assistenten zum Konfigurieren von Datenquellen in der ersten Dropdown Liste die Option `CategoriesDataSource` aus, und wählen Sie aus, dass `CategoryName` für die Anzeige und `CategoryID` als Wert verwendet werden soll.

[!["Dropdown List" an "categoriesdatasource" binden](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Abbildung 11**: Binden der Dropdown List an den `CategoriesDataSource` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image20.png))

An diesem Punkt listet der `Categories` Dropdown List alle Kategorien auf, aber es wird noch nicht automatisch die entsprechende Kategorie für das an die GridView-Zeile gebundene Produkt ausgewählt. Um dies zu erreichen, müssen wir die `Categories` DropDownList s-`SelectedValue` auf den Product s `CategoryID` Wert festlegen. Klicken Sie im Smarttag DropDownList s auf den Link DataBindings bearbeiten, und ordnen Sie die `SelectedValue`-Eigenschaft dem `CategoryID` Datenfeld zu, wie in Abbildung 12 dargestellt.

![Binden Sie den Wert des Product s CategoryID an die DropDownList s SelectedValue-Eigenschaft.](batch-updating-cs/_static/image12.gif)

**Abbildung 12**: Binden des Product s `CategoryID` Werts an die DropDownList s `SelectedValue`-Eigenschaft

Ein letztes Problem bleibt bestehen: Wenn für das Produkt kein `CategoryID` Wert angegeben wird, führt die Datenbindung-Anweisung auf `SelectedValue` zu einer Ausnahme. Dies liegt daran, dass DropDownList nur Elemente für die Kategorien enthält und keine Option für die Produkte bietet, die einen `NULL` Daten Bank Wert für `CategoryID`haben. Um dieses Problem zu beheben, legen Sie die Eigenschaft DropDownList s `AppendDataBoundItems` auf `true` fest, und fügen Sie der Dropdown Liste ein neues Element hinzu, wobei die Eigenschaft `Value` aus der deklarativen Syntax weggelassen wird. Stellen Sie also sicher, dass die deklarative Syntax `Categories` DropDownList s wie folgt aussieht:

[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Beachten Sie, dass die `<asp:ListItem Value="">`--Select-Eigenschaft ein-`Value` Attribut explizit auf eine leere Zeichenfolge festgelegt ist. Im Tutorial zum [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) finden Sie eine ausführlichere Erläuterung dazu, warum dieses zusätzliche DropDownList-Element benötigt wird, um den `NULL` Fall zu behandeln, und warum die Zuweisung der `Value` Eigenschaft zu einer leeren Zeichenfolge erforderlich ist.

> [!NOTE]
> Es gibt ein potenzielles Problem mit der Leistung und Skalierbarkeit, das erwähnt werden sollte. Da jede Zeile über eine Dropdown List verfügt, die die `CategoriesDataSource` als Datenquelle verwendet, wird die `CategoriesBLL` Class s `GetCategories`-Methode *n* -Mal pro Seitenbesuch aufgerufen, wobei *n* die Anzahl der Zeilen in der GridView ist. Diese *n* Aufrufe von `GetCategories` führen zu *n* Abfragen an die Datenbank. Diese Auswirkung auf die Datenbank kann verringert werden, indem die zurückgegebenen Kategorien entweder in einem Anforderungs bezogenen Cache oder über die zwischen Speicherungs Schicht mithilfe einer SQL-zwischen Speicherungs Abhängigkeit oder einem sehr kurzen zeitbasierten Ablauf zwischengespeichert werden. Weitere Informationen zur Cache Option pro Anforderung finden Sie unter [`HttpContext.Items` eines Cache Speichers pro Anforderung](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).

## <a name="step-4-completing-the-editing-interface"></a>Schritt 4: Abschließen der Bearbeitungs Schnittstelle

Wir haben eine Reihe von Änderungen an den GridView s-Vorlagen vorgenommen, ohne den Fortschritt anzuzeigen. Nehmen Sie sich einen Moment Zeit, um den Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 13 gezeigt, wird jede Zeile mit der `ItemTemplate`gerendert, die die Bearbeitungs Schnittstelle für Zellen enthält.

[![jeder GridView-Zeile kann bearbeitet werden.](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Abbildung 13**: die einzelnen GridView-Zeilen können bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image22.png))

An dieser Stelle sind einige geringfügige Formatierungs Probleme zu berücksichtigen. Beachten Sie zunächst, dass der `UnitPrice` Wert vier Dezimalstellen enthält. Um dieses Problem zu beheben, kehren Sie zur `UnitPrice` TemplateField s `ItemTemplate` zurück, und klicken Sie im TextBox-smarttagtag auf den Link DataBindings bearbeiten. Geben Sie als nächstes an, dass die `Text`-Eigenschaft als Zahl formatiert werden soll.

![Formatieren der Text-Eigenschaft als Zahl](batch-updating-cs/_static/image14.gif)

**Abbildung 14**: Formatieren der `Text`-Eigenschaft als Zahl

Im zweiten Bereich können Sie das Kontrollkästchen in der `Discontinued` Spalte zentrieren (anstatt Sie linksbündig auszurichten). Klicken Sie in der GridView s-Smarttag auf Spalten bearbeiten, und wählen Sie die `Discontinued` TemplateField aus der Liste der Felder in der unteren linken Ecke aus. Führen Sie einen Drilldown in `ItemStyle` aus, und legen Sie die `HorizontalAlign`-Eigenschaft wie in Abbildung 15 dargestellt auf Center

![Kontrollkästchen für das unterstützte Center](batch-updating-cs/_static/image15.gif)

**Abbildung 15**: Kontrollkästchen "`Discontinued` zentrieren"

Fügen Sie anschließend der Seite ein ValidationSummary-Steuerelement hinzu, und legen Sie dessen `ShowMessageBox`-Eigenschaft auf `true` und dessen `ShowSummary`-Eigenschaft auf `false`fest. Fügen Sie auch die Schaltflächen-websteuer Elemente hinzu, die die Änderungen des Benutzers aktualisieren, wenn Sie darauf klicken. Fügen Sie insbesondere zwei Schaltflächen-websteuer Elemente hinzu, eine oberhalb der GridView und eine darunter, und legen Sie beide Steuerelemente `Text` Eigenschaften zum Aktualisieren von Produkten fest.

Da die GridView s-Bearbeitungs Schnittstelle in den templatefields-`ItemTemplate` s definiert ist, sind die `EditItemTemplate` s überflüssig und können gelöscht werden.

Nachdem Sie die oben genannten Formatierungs Änderungen vorgenommen, die Schaltflächen-Steuerelemente hinzugefügt und die nicht benötigten `EditItemTemplate` s entfernt haben, sollte die deklarative Syntax Ihrer Seite s wie folgt aussehen:

[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Abbildung 16 zeigt diese Seite, wenn Sie über einen Browser angezeigt werden, nachdem die Schaltflächen-websteuer Elemente hinzugefügt und die Formatierungs Änderungen vorgenommen wurden.

[![die Seite jetzt zwei Schaltflächen zum Aktualisieren von Produkten enthält.](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Abbildung 16**: die Seite enthält jetzt zwei Schaltflächen zum Aktualisieren[von Produkten (Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-updating-cs/_static/image24.png))

## <a name="step-5-updating-the-products"></a>Schritt 5: Aktualisieren der Produkte

Wenn ein Benutzer auf diese Seite wechselt, führt er die Änderungen aus und klickt dann auf eine der beiden Schaltflächen zum Aktualisieren von Produkten. An dieser Stelle müssen wir die vom Benutzer eingegebenen Werte für jede Zeile in einer `ProductsDataTable`-Instanz speichern und diese dann an eine BLL-Methode übergeben, die diese `ProductsDataTable` Instanz an die DAL-`UpdateWithTransaction`-Methode übergibt. Mit der `UpdateWithTransaction`-Methode, die wir im [vorherigen Tutorial](wrapping-database-modifications-within-a-transaction-cs.md)erstellt haben, wird sichergestellt, dass der Batch von Änderungen als atomarer Vorgang aktualisiert wird.

Erstellen Sie eine Methode mit dem Namen `BatchUpdate` in `BatchUpdate.aspx.cs`, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Diese Methode beginnt, indem alle Produkte mithilfe eines Aufrufens der `GetProducts`-Methode der BLL s in einer `ProductsDataTable` zurückerhalten werden. Anschließend wird die `ProductGrid` GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx)Auflistung aufgelistet. Die `Rows`-Auflistung enthält eine [`GridViewRow` Instanz](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) für jede in der GridView angezeigte Zeile. Da höchstens zehn Zeilen pro Seite angezeigt werden, enthält die GridView s-`Rows` Sammlung höchstens zehn Elemente.

Für jede Zeile wird die `ProductID` aus der `DataKeys` Auflistung und die entsprechende `ProductsRow` aus der `ProductsDataTable`ausgewählt. Auf die vier TemplateField-Eingabe Steuerelemente wird Programm gesteuert verwiesen, und ihre Werte werden den Eigenschaften der `ProductsRow`-Instanz zugewiesen. Nachdem alle GridView-Zeilen Werte verwendet wurden, um die `ProductsDataTable`zu aktualisieren, werden Sie an die BLL s-`UpdateWithTransaction`-Methode übermittelt, die wie im vorherigen Tutorial bereits in der `UpdateWithTransaction`-Methode von Dal aufgerufen wurde.

Der für dieses Tutorial verwendete Batch Aktualisierungs Algorithmus aktualisiert jede Zeile in der `ProductsDataTable`, die einer Zeile in der GridView entspricht, unabhängig davon, ob die Produkt-e-Informationen geändert wurden. Diese Blind Updates sind zwar in der Regel kein Leistungsproblem, aber Sie können zu überflüssigen Datensätzen führen, wenn Sie Änderungen an der Datenbanktabelle erneut überwachen. Wieder im Tutorial zum [Durchführen von Batch Updates](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) haben wir eine Batch Aktualisierungs Schnittstelle mit dem DataList untersucht und Code hinzugefügt, mit dem nur die Datensätze aktualisiert werden, die tatsächlich vom Benutzer geändert wurden. Wenn gewünscht, können Sie die Verfahren aus der [Ausführung von Batch Updates](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) verwenden, um den Code in diesem Tutorial zu aktualisieren.

> [!NOTE]
> Wenn die Datenquelle über das Smarttag an die GridView gebunden wird, weist Visual Studio automatisch die-Primärschlüssel Werte der Datenquelle der GridView s-`DataKeyNames`-Eigenschaft zu. Wenn Sie ObjectDataSource nicht wie in Schritt 1 beschrieben an das GridView-Smarttag gebunden haben, müssen Sie die Eigenschaft GridView s `DataKeyNames` manuell auf ProductID festlegen, um auf den `ProductID` Wert für jede Zeile über die `DataKeys` Auflistung zuzugreifen.

Der in `BatchUpdate` verwendete Code ähnelt dem, der in den `UpdateProduct` Methoden der BLL s verwendet wird. der Hauptunterschied besteht darin, dass in den `UpdateProduct`-Methoden nur eine einzige `ProductRow` Instanz aus der Architektur abgerufen wird. Der Code, der die Eigenschaften des `ProductRow` zuweist, ist zwischen den `UpdateProducts` Methoden und dem Code innerhalb der `foreach`-Schleife in `BatchUpdate`identisch, wie das Gesamt Muster.

Um dieses Tutorial abzuschließen, muss die `BatchUpdate`-Methode aufgerufen werden, wenn auf eine der Schaltflächen zum Aktualisieren von Produkten geklickt wird. Erstellen Sie Ereignishandler für die `Click`-Ereignisse dieser beiden Schaltflächen-Steuerelemente, und fügen Sie den folgenden Code in den Ereignis Handlern ein:

[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Zuerst wird ein-`BatchUpdate`aufgerufen. Im nächsten Schritt wird das `ClientScript property` zum Einfügen von JavaScript verwendet, das eine MessageBox anzeigt, die liest, dass die Produkte aktualisiert wurden.

Nehmen Sie sich einen Moment Zeit, um diesen Code zu testen. Besuchen Sie `BatchUpdate.aspx` über einen Browser, bearbeiten Sie eine Reihe von Zeilen, und klicken Sie auf eine der Schaltflächen zum Aktualisieren von Produkten. Wenn keine Eingabevalidierungsfehler vorliegen, sollte eine MessageBox angezeigt werden, die liest, dass die Produkte aktualisiert wurden. Um die Atomizität des Updates zu überprüfen, sollten Sie eine zufällige `CHECK` Einschränkung hinzufügen, wie z. b. eine, die `UnitPrice` Werte von 1234,56 nicht zulässt. Bearbeiten Sie anschließend in `BatchUpdate.aspx`eine Reihe von Datensätzen, und stellen Sie sicher, dass Sie einen der Product s `UnitPrice` Wert auf den unzulässigen Wert festlegen (1234,56). Dies sollte zu einem Fehler führen, wenn Sie während dieses Batch Vorgangs auf Produkte aktualisieren klicken, die auf die ursprünglichen Werte zurückgesetzt werden.

## <a name="an-alternativebatchupdatemethod"></a>Eine Alternative`BatchUpdate`-Methode

Die `BatchUpdate` Methode, die wir gerade überprüft haben, ruft *alle* Produkte aus der BLL s-`GetProducts` Methode ab und aktualisiert dann nur die Datensätze, die in der GridView angezeigt werden. Diese Vorgehensweise ist ideal, wenn die GridView kein Paging verwendet, aber wenn dies der Fall ist, gibt es möglicherweise Hunderte, Tausende oder Zehntausende von Produkten, aber nur zehn Zeilen in der GridView. In einem solchen Fall ist es weniger als ideal, alle Produkte aus der Datenbank zu ändern, um 10 davon zu ändern.

Für diese Art von Situationen sollten Sie stattdessen die folgende `BatchUpdateAlternate`-Methode verwenden:

[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` beginnt mit dem Erstellen einer neuen leeren `ProductsDataTable` mit dem Namen `products`. Anschließend werden die `Rows` Auflistung von GridView s durchlaufen, und für jede Zeile werden die jeweiligen Produktinformationen mithilfe der BLL s `GetProductByProductID(productID)`-Methode abgerufen. Die Eigenschaften der abgerufenen `ProductsRow` Instanz werden auf die gleiche Weise wie `BatchUpdate`aktualisiert, aber nach dem Aktualisieren der Zeile wird Sie über die Datentabelle [`ImportRow(DataRow)`-Methode](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx)in die `products``ProductsDataTable` importiert.

Nachdem die `foreach` Schleife abgeschlossen ist, enthält `products` eine `ProductsRow` Instanz für jede Zeile in der GridView. Da jede der `ProductsRow` Instanzen der `products` hinzugefügt wurde (anstatt Sie zu aktualisieren), wird die `ProductsTableAdapter` versuchen, jeden der Datensätze in die Datenbank einzufügen, wenn Sie Sie blind an die `UpdateWithTransaction`-Methode übergeben. Stattdessen müssen Sie angeben, dass alle Zeilen geändert (nicht hinzugefügt) werden.

Dies kann durch Hinzufügen einer neuen Methode zur BLL namens `UpdateProductsWithTransaction`erreicht werden. `UpdateProductsWithTransaction`, wie unten dargestellt, legt die `RowState` der einzelnen `ProductsRow` Instanzen in der `ProductsDataTable` auf `Modified` fest und übergibt dann den `ProductsDataTable` an die DAL s `UpdateWithTransaction`-Methode.

[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Summary

Die GridView bietet integrierte Zeilen übergreifende Bearbeitungsfunktionen, bietet jedoch keine Unterstützung für das Erstellen von vollständig bearbeitbaren Schnittstellen. Wie in diesem Tutorial gezeigt, sind solche Schnittstellen möglich, erfordern jedoch etwas Arbeit. Zum Erstellen einer GridView-Tabelle, in der jede Zeile bearbeitet werden kann, müssen die GridView s-Felder in templatefields konvertiert und die Bearbeitungs Schnittstelle innerhalb der `ItemTemplate` s definiert werden. Darüber hinaus müssen der Seite, getrennt von der GridView, die websteuer Elemente für alle Typen aktualisieren hinzugefügt werden. Diese Schaltflächen `Click` Ereignishandler müssen die GridView s `Rows` Auflistung auflisten, die Änderungen in einem `ProductsDataTable`speichern und die aktualisierten Informationen an die entsprechende BLL-Methode übergeben.

Im nächsten Tutorial wird erläutert, wie eine Schnittstelle für das Löschen von Batches erstellt wird. Vor allem enthält jede GridView-Zeile ein Kontrollkästchen und anstelle der Schaltflächen alle Typen aktualisieren werden die Schaltflächen ausgewählte Zeilen löschen angezeigt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Teresa Murphy und David suru. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](wrapping-database-modifications-within-a-transaction-cs.md)
> [Weiter](batch-deleting-cs.md)
