---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Batch einfügen (C#) | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie mehrere Datenbankdaten Sätze in einem einzelnen Vorgang einfügen. In der Benutzeroberflächen Ebene erweitern wir die GridView, um dem Benutzer die Eingabe mehrerer n...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 5dc4d0b6ac9bf3aa2baa54fe9f5d4149494e47d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78501447"
---
# <a name="batch-inserting-c"></a>Einfügen in Batches (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) oder [PDF herunterladen](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Erfahren Sie, wie Sie mehrere Datenbankdaten Sätze in einem einzelnen Vorgang einfügen. In der Benutzeroberflächen Ebene erweitern wir die GridView, um dem Benutzer die Eingabe mehrerer neuer Datensätze zu ermöglichen. In der Datenzugriffs Ebene werden mehrere Einfügevorgänge innerhalb einer Transaktion eingeschlossen, um sicherzustellen, dass alle Einfügungen erfolgreich sind oder für alle Einfügungen ein Rollback ausgeführt wird.

## <a name="introduction"></a>Einführung

Im Tutorial zum [Batch-Update](batch-updating-cs.md) haben wir uns mit der Anpassung des GridView-Steuer Elements für eine Schnittstelle beschäftigt, bei der mehrere Datensätze bearbeitet werden konnten. Der Benutzer, der die Seite besucht, könnte eine Reihe von Änderungen vornehmen und dann mit einem einzigen Klick auf die Schaltfläche ein Batch Update ausführen. In Situationen, in denen Benutzer häufig viele Datensätze in einem Go aktualisieren, kann eine solche Schnittstelle unzählige Klicks und Tastatur-zu-Maus-Kontextwechsel im Vergleich zu den standardmäßigen Bearbeitungs Features pro Zeile, die zuerst in der Übersicht über das [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) untersucht wurden, speichern.

Dieses Konzept kann auch angewendet werden, wenn Datensätze hinzugefügt werden. Stellen Sie sich vor, dass wir bei Northwind Traders häufig Lieferungen von Lieferanten erhalten, die eine Reihe von Produkten für eine bestimmte Kategorie enthalten. Beispielsweise erhalten wir möglicherweise eine Lieferung von sechs verschiedenen Tee-und kaffeeprodukten von Tokyo Traders. Wenn ein Benutzer die sechs Produkte einzeln über ein DetailsView-Steuerelement eingibt, muss er viele der gleichen Werte immer wieder auswählen: er muss dieselbe Kategorie (Getränke), denselben Lieferanten (Tokyo Traders) und denselben nicht mehr unterstützten Wert auswählen ( False) und die gleichen Einheiten für den Bestellwert (0). Dieser wiederkehrende Dateneintrag ist nicht nur zeitaufwändig, sondern fehleranfällig.

Mit geringem Aufwand können wir eine Schnittstelle für das Einfügen von Batches erstellen, die es dem Benutzer ermöglicht, den Lieferanten und die Kategorie einmalig auszuwählen, eine Reihe von Produktnamen und Preis Einheiten einzugeben und dann auf eine Schaltfläche zu klicken, um die neuen Produkte der Datenbank hinzuzufügen (siehe Abbildung 1). Wenn jedes Produkt hinzugefügt wird, werden die Datenfelder `ProductName` und `UnitPrice` den in den Textfeldern eingegebenen Werten zugewiesen, während deren `CategoryID` und `SupplierID` Werte den Werten aus den Dropdown Listen am oberen Rand des Formulars zugewiesen werden. Die Werte für `Discontinued` und `UnitsOnOrder` werden auf die hart codierten Werte von `false` bzw. 0 festgelegt.

[![der Batch-einfügeschnittstelle](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Abbildung 1**: die Schnittstelle zum Einfügen[von Batches (Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image3.png))

In diesem Tutorial erstellen wir eine Seite, auf der die in Abbildung 1 gezeigte Schnittstelle zum Einfügen von Batches implementiert wird. Wie bei den vorherigen beiden Tutorials werden die Einfügungen innerhalb des Umfangs einer Transaktion umschlossen, um die Atomizität sicherzustellen. Legen Sie Los!

## <a name="step-1-creating-the-display-interface"></a>Schritt 1: Erstellen der Anzeige Schnittstelle

Dieses Tutorial besteht aus einer einzelnen Seite, die in zwei Bereiche unterteilt ist: einem Anzeigebereich und einem Einfügebereich. Die Anzeige Schnittstelle, die wir in diesem Schritt erstellen, zeigt die Produkte in einer GridView an und enthält eine Schaltfläche mit dem Namen Process Product Shipping. Wenn Sie auf diese Schaltfläche klicken, wird die Anzeige Schnittstelle durch die einfügeschnittstelle ersetzt, die in Abbildung 1 dargestellt wird. Die Anzeige Schnittstelle wird zurückgegeben, nachdem auf die Schaltflächen Produkte aus Sendung hinzufügen oder Abbrechen geklickt wurde. Die einfügeschnittstelle wird in Schritt 2 erstellt.

Beim Erstellen einer Seite mit zwei Schnittstellen, von denen jeweils nur eine sichtbar ist, wird jede Schnittstelle in der Regel in einem [Panel-websteuer](http://www.w3schools.com/aspnet/control_panel.asp)Element platziert, das als Container für andere Steuerelemente fungiert. Daher hat unsere Seite zwei Panel-Steuerelemente für jede Schnittstelle.

Öffnen Sie zunächst die Seite `BatchInsert.aspx` im Ordner `BatchData`, und ziehen Sie einen Bereich aus der Toolbox auf den Designer (siehe Abbildung 2). Legen Sie die Eigenschaft Bereich s `ID` auf `DisplayInterface`fest. Wenn das Panel dem Designer hinzugefügt wird, werden die Eigenschaften `Height` und `Width` auf 50px bzw. 125 px festgelegt. Löschen Sie diese Eigenschaftswerte aus der Eigenschaftenfenster.

[![ziehen Sie einen Bereich aus der Toolbox auf den Designer.](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Abbildung 2**: Ziehen eines Panels aus der Toolbox auf den Designer ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image6.png))

Ziehen Sie als nächstes ein Schaltflächen-und GridView-Steuerelement in den Bereich. Legen Sie die Schaltfläche s `ID`-Eigenschaft auf `ProcessShipment` und deren `Text`-Eigenschaft auf Produktlieferung verarbeiten fest. Legen Sie die Eigenschaft GridView s `ID` auf `ProductsGrid` fest, und binden Sie das Smarttag an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren Sie ObjectDataSource so, dass die Daten aus der `ProductsBLL` Klasse `GetProducts` Methode abgerufen werden. Da diese GridView nur zum Anzeigen von Daten verwendet wird, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest. Klicken Sie zum Abschließen des Assistenten zum Konfigurieren von Datenquellen auf Fertigstellen.

[![Anzeigen der Daten, die von der Methode "GetProducts" der productbll-Klasse zurückgegeben werden](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Abbildung 3**: Anzeigen der Daten, die von der `ProductsBLL` Klasse `GetProducts`-Methode zurückgegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image9.png))

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Abbildung 4**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image12.png))

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, fügt Visual Studio boundfields und ein CheckBoxField für die Product Data-Felder hinzu. Entfernen Sie alle Felder außer den Feldern `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`und `Discontinued`. Sie können jederzeit beliebige ästhetische Anpassungen vornehmen. Ich habe mich entschieden, das `UnitPrice`-Feld als Währungswert zu formatieren, die Felder neu angeordnet zu haben und einige Felder `HeaderText` Werte umzubenennen. Konfigurieren Sie außerdem die GridView so, dass Paging und Sortier Unterstützung unterstützt werden, indem Sie die Kontrollkästchen Paging aktivieren und Sortierung aktivieren in der GridView s Smarttags aktivieren.

Nach dem Hinzufügen der Steuerelemente Panel, Button, GridView und ObjectDataSource und Anpassen der GridView s-Felder sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Beachten Sie, dass das Markup für die Schaltfläche und die GridView innerhalb der öffnenden und schließenden `<asp:Panel>` Tags angezeigt wird. Da sich diese Steuerelemente im `DisplayInterface` Panel befinden, können Sie diese ausblenden, indem Sie einfach die Eigenschaft `Visible` des Bereichs s auf `false`festlegen. In Schritt 3 wird das programmgesteuerte Ändern der Panel s-`Visible` Eigenschaft als Reaktion auf einen Schaltflächen Klick erläutert, um eine Schnittstelle anzuzeigen, während die andere ausgeblendet wird.

Nehmen Sie sich einen Moment Zeit, um den Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 5 gezeigt, sollte eine Schaltfläche "Produktversand verarbeiten" oberhalb einer GridView angezeigt werden, in der die Produkte jeweils zehn aufgelistet sind.

[![GridView listet die Produkte auf und bietet Sortier-und Pagingfunktionen.](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Abbildung 5**: die GridView listet die Produkte auf und bietet Sortier-und Pagingfunktionen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image15.png))

## <a name="step-2-creating-the-inserting-interface"></a>Schritt 2: Erstellen der einfügeschnittstelle

Nachdem die Anzeige Schnittstelle abgeschlossen ist, können Sie die einfügeschnittstelle neu erstellen. In diesem Tutorial erstellen Sie eine einfügeschnittstelle, die zur Eingabe eines einzelnen Lieferanten-und Kategoriewerts auffordert und dem Benutzer dann ermöglicht, bis zu fünf Produktnamen und Einheiten preiswerte einzugeben. Mit dieser Schnittstelle kann der Benutzer ein bis fünf neue Produkte hinzufügen, die alle dieselbe Kategorie und denselben Lieferanten gemeinsam nutzen, aber eindeutige Produktnamen und Preise aufweisen.

Ziehen Sie zunächst ein Panel aus der Toolbox auf den Designer, und platzieren Sie es unterhalb des vorhandenen `DisplayInterface` Panels. Legen Sie die `ID`-Eigenschaft des neu hinzugefügten Panels auf `InsertingInterface` fest, und legen Sie dessen `Visible`-Eigenschaft auf `false`fest. Wir fügen Code hinzu, mit dem die `InsertingInterface` Panel s `Visible` Eigenschaft auf `true` in Schritt 3 festgelegt wird. Löschen Sie außerdem den Bereich s `Height` und `Width` Eigenschaftswerte.

Als nächstes muss die einfügeschnittstelle erstellt werden, die in Abbildung 1 dargestellt wurde. Diese Schnittstelle kann über eine Vielzahl von HTML-Techniken erstellt werden, aber wir verwenden eine ziemlich unkomplizierte Tabelle: eine Tabelle mit vier Spalten und sieben Zeilen.

> [!NOTE]
> Wenn Sie Markup für HTML-`<table>` Elemente eingeben, bevorzugen Sie die Verwendung der Quell Ansicht. Obwohl Visual Studio über Tools zum Hinzufügen von `<table>` Elementen über den Designer verfügt, scheint der Designer alle nicht angefragten `style` Einstellungen in das Markup einzufügen. Nachdem Sie das `<table>` Markup erstellt haben, kehren Sie in der Regel zum Designer zurück, um die websteuer Elemente hinzuzufügen und ihre Eigenschaften festzulegen. Beim Erstellen von Tabellen mit vordefinierten Spalten und Zeilen verwende ich anstelle des websteuer Elements "Table" anstelle des [websteuer](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) Elements "Table" die Verwendung von statischem HTML, da auf alle websteuer Elemente, `FindControl("controlID")` die in einem Table-websteuer Element platziert sind, Ich verwende jedoch Tabellen-websteuer Elemente für dynamisch formatierte Tabellen (solche, deren Zeilen oder Spalten auf einer Datenbank oder einem benutzerdefinierten Kriterium basieren), da das Table-websteuer Element Programm gesteuert erstellt werden kann.

Geben Sie das folgende Markup innerhalb der `<asp:Panel>` Tags des `InsertingInterface` Panels ein:

[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Dieses `<table>` Markup enthält noch keine websteuer Elemente. Diese werden vorübergehend hinzugefügt. Beachten Sie, dass jedes `<tr>`-Element eine bestimmte CSS-Klassen Einstellung enthält: `BatchInsertHeaderRow` für die Kopfzeile, in der die Dropdown Listen "Lieferant" und "Category" verwendet werden. `BatchInsertFooterRow` für die Footerzeile, in der die Schaltflächen Produkte aus Sendung hinzufügen und Abbrechen angezeigt werden. und abwechselnde `BatchInsertRow` und `BatchInsertAlternatingRow` Werte für die Zeilen, die die Textfeld-Steuerelemente Product und Unit Price enthalten. In der `Styles.css` Datei wurden entsprechende CSS-Klassen erstellt, um der einfügeschnittstelle eine Darstellung ähnlich der GridView-und DetailsView-Steuerelemente zuzuweisen, die wir in diesen Tutorials verwendet haben. Diese CSS-Klassen sind unten dargestellt.

[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Wenn Sie dieses Markup eingegeben haben, kehren Sie zum Designansicht zurück. Diese `<table>` sollte im Designer als vier spaltige Tabelle mit sieben Zeilen angezeigt werden, wie in Abbildung 6 veranschaulicht.

[![die einfügende Schnittstelle aus einer Tabelle mit vier Spalten und sieben Zeilen besteht.](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Abbildung 6**: die einfügende Schnittstelle besteht aus einer Tabelle mit vier Spalten und sieben Zeilen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image18.png))

Nun können Sie die websteuer Elemente zur einfügeschnittstelle hinzufügen. Ziehen Sie zwei Dropdown Listen aus der Toolbox in die entsprechenden Zellen in der Tabelle 1 für den Lieferanten und eine für die Kategorie.

Legen Sie für die Eigenschaft "Lieferant DropDownList s `ID`" den Wert `Suppliers` fest, und binden Sie ihn an eine neue ObjectDataSource namens `SuppliersDataSource`. Konfigurieren Sie die neue ObjectDataSource, um Ihre Daten aus der `SuppliersBLL` Klasse `GetSuppliers` Methode abzurufen, und legen Sie die Dropdown Liste Update Registerkarte auf (keine) fest. Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

[![konfigurieren Sie die ObjectDataSource für die Verwendung der Methode getsuppliers der suppliersbll-Klasse.](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Abbildung 7**: Konfigurieren von ObjectDataSource für die Verwendung der `SuppliersBLL` Class s `GetSuppliers`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image21.png))

Verwenden Sie die Dropdown Liste `Suppliers`, um das `CompanyName` Datenfeld anzuzeigen, und verwenden Sie das `SupplierID` Datenfeld als `ListItem` s-Werte.

[![das Unternehmensname-Datenfeld anzeigen und SupplierID als Wert verwenden.](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Abbildung 8**: Anzeigen des `CompanyName` Datenfelds und verwenden `SupplierID` als Wert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image24.png))

Benennen Sie den zweiten DropDownList-`Categories`, und binden Sie ihn an eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`. Konfigurieren Sie die `CategoriesDataSource` ObjectDataSource für die Verwendung der `CategoriesBLL` Class s `GetCategories`-Methode. Legen Sie die Dropdown Listen auf den Registerkarten aktualisieren und löschen auf (keine) fest, und klicken Sie auf Fertigstellen, um den Assistenten abzuschließen. Zum Schluss soll in der Dropdown List das `CategoryName` Datenfeld angezeigt und die `CategoryID` als Wert verwendet werden.

Nachdem diese beiden Dropdown Listen hinzugefügt und an ordnungsgemäß konfigurierte objectdatasources gebunden wurden, sollte der Bildschirm in etwa wie in Abbildung 9 aussehen.

[![die Kopfzeile enthält jetzt die Dropdown Listen "Suppliers" und "categories".](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Abbildung 9**: die Kopfzeile enthält jetzt die Dropdown Listen `Suppliers` und `Categories` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image27.png))

Wir müssen nun die Textfelder erstellen, um den Namen und den Preis für jedes neue Produkt zu erfassen. Ziehen Sie ein TextBox-Steuerelement aus der Toolbox auf den Designer für jeden der fünf Product Name-und Price-Zeilen. Legen Sie die `ID` Eigenschaften der Textfelder auf `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`usw. fest.`UnitPrice3`

Fügen Sie ein CompareValidator nach jedem der Einheitspreis-Textfelder hinzu, und legen Sie die `ControlToValidate`-Eigenschaft auf die entsprechende `ID`fest. Legen Sie außerdem die Eigenschaft `Operator` auf `GreaterThanEqual`, `ValueToCompare` auf 0 und `Type` auf `Currency`fest. Diese Einstellungen weisen das CompareValidator an, sicherzustellen, dass der Preis, falls eingegeben, ein gültiger Währungswert ist, der größer oder gleich 0 (null) ist. Legen Sie die `Text`-Eigenschaft auf \*fest, und `ErrorMessage` auf den Preis muss größer oder gleich 0 (null) sein. Außerdem sollten Sie keine Währungssymbole weglassen.

> [!NOTE]
> Die einfügende Schnittstelle enthält keine Requirements dfieldvalidator-Steuerelemente, auch wenn das `ProductName`-Feld in der `Products` Datenbanktabelle keine `NULL` Werte zulässt. Der Grund hierfür ist, dass der Benutzer bis zu fünf Produkte eingeben soll. Wenn der Benutzer z. b. den Produktnamen und den Preis für die ersten drei Zeilen angeben und die letzten beiden Zeilen leer gelassen hat, werden dem System lediglich drei neue Produkte hinzugefügt. Da `ProductName` erforderlich ist, müssen wir Programm gesteuert überprüfen, um sicherzustellen, dass ein entsprechender Produkt namens Wert angegeben wird, wenn ein Stückpreis eingegeben wird. Wir behandeln diese Prüfung in Schritt 4.

Beim Validieren der Benutzereingaben meldet das CompareValidator ungültige Daten, wenn der Wert ein Währungssymbol enthält. Fügen Sie vor jedem der UnitPrice-Textfelder einen Wert von $ vor, der als visueller Hinweis dienen soll, der den Benutzer anweist, das Währungssymbol bei der Eingabe des Preises auszulassen.

Fügen Sie abschließend im `InsertingInterface` Panel ein ValidationSummary-Steuerelement hinzu, und legen Sie dessen `ShowMessageBox`-Eigenschaft `true` und dessen `ShowSummary`-Eigenschaft auf `false`fest. Mit diesen Einstellungen wird ein Sternchen neben den problematischen TextBox-Steuerelementen angezeigt, wenn der Benutzer einen ungültigen Einheiten Preis Wert eingibt. in der Datei "ValidationSummary" wird eine Client seitige MessageBox angezeigt, in der die zuvor angegebene Fehlermeldung angezeigt wird.

An diesem Punkt sollte der Bildschirm in etwa wie in Abbildung 10 aussehen.

[![die einfügende Schnittstelle nun Textfelder für die Namen und Preise der Produkte enthält.](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Abbildung 10**: die einfügende Schnittstelle enthält jetzt Textfelder für die Namen und Preise der Produkte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image30.png))

Als nächstes müssen wir die Schaltflächen Produkte aus Versand und Abbrechen der Footerzeile hinzufügen. Ziehen Sie zwei Schaltflächen-Steuerelemente aus der Toolbox in die Fußzeile der einfügeschnittstelle, und legen Sie die Schaltflächen `ID` Eigenschaften auf `AddProducts` und `CancelButton` und `Text` Eigenschaften zum Hinzufügen von Produkten aus der Lieferung bzw. dem Abbruch fest. Legen Sie außerdem die `CancelButton` Control s `CausesValidation`-Eigenschaft auf `false`fest.

Zum Schluss muss ein Label-websteuer Element hinzugefügt werden, das Statusmeldungen für die beiden Schnittstellen anzeigt. Wenn ein Benutzer beispielsweise erfolgreich eine neue Lieferung von Produkten hinzufügt, möchten wir zur Anzeige Schnittstelle zurückkehren und eine Bestätigungsmeldung anzeigen. Wenn der Benutzer jedoch einen Preis für ein neues Produkt bereitstellt, aber den Produktnamen verlässt, muss eine Warnmeldung angezeigt werden, da das Feld "`ProductName`" erforderlich ist. Da diese Meldung für beide Schnittstellen angezeigt werden muss, platzieren Sie Sie am oberen Rand der Seite außerhalb der Panels.

Ziehen Sie ein Label-websteuer Element aus der Toolbox an den oberen Rand der Seite im Designer. Legen Sie die `ID`-Eigenschaft auf `StatusLabel`fest, löschen Sie die `Text`-Eigenschaft, und legen Sie die `Visible`-und `EnableViewState`-Eigenschaften auf `false`fest. Wie in den vorherigen Tutorials gezeigt, können wir die `EnableViewState`-Eigenschaft auf `false` festlegen, um die Eigenschaften Werte der Bezeichnung Programm gesteuert zu ändern und automatisch auf die Standardwerte des nachfolgenden Postbacks zurückzukehren. Dies vereinfacht den Code, um eine Statusmeldung als Reaktion auf eine Benutzeraktion anzuzeigen, die beim nachfolgenden Postback nicht mehr angezeigt wird. Legen Sie abschließend die `StatusLabel` Control s `CssClass` Eigenschaft auf Warning fest. Dies ist der Name einer CSS-Klasse, die in `Styles.css` definiert ist, in der Text in einer großen, kursiv, Fetten und roten Schriftart angezeigt wird.

Abbildung 11 zeigt den Visual Studio-Designer, nachdem die Bezeichnung hinzugefügt und konfiguriert wurde.

[![das Steuerelement "Status Bezeichnung" oberhalb der beiden Panel-Steuerelemente platzieren.](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Abbildung 11**: Platzieren des `StatusLabel`-Steuer Elements oberhalb der beiden Panel-Steuerelemente ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image33.png))

## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Schritt 3: Wechseln zwischen den Anzeige-und einfügeschnittstellen

An dieser Stelle haben wir das Markup für unsere Anzeige-und einfügeschnittstellen abgeschlossen, aber wir haben noch immer zwei Aufgaben verbleiben:

- Wechseln zwischen den Anzeige-und einfügeschnittstellen
- Hinzufügen der Produkte in der Lieferung zur Datenbank

Derzeit ist die Anzeige Schnittstelle sichtbar, aber die einfügende Schnittstelle ist ausgeblendet. Dies liegt daran, dass die `DisplayInterface` Bereich s `Visible` Eigenschaft auf `true` (Standardwert) festgelegt ist, während die `InsertingInterface` Panel s `Visible`-Eigenschaft auf `false`festgelegt ist. Um zwischen den beiden Schnittstellen zu wechseln, müssen Sie einfach die einzelnen Steuerelemente `Visible`-Eigenschafts Wert umschalten.

Wir möchten von der Anzeige Schnittstelle zur einfügeschnittstelle wechseln, wenn auf die Schaltfläche "Produktversand verarbeiten" geklickt wird. Erstellen Sie daher einen Ereignishandler für diese Schaltfläche s `Click` Ereignis, das den folgenden Code enthält:

[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Mit diesem Code wird der `DisplayInterface` Panel ausgeblendet und das `InsertingInterface` Panel angezeigt.

Erstellen Sie anschließend in der einfügeschnittstelle Ereignishandler für die Schaltflächen "Produkte aus Sendung und Abbrechen". Wenn auf eine dieser Schaltflächen geklickt wird, müssen wir auf die Anzeige Schnittstelle zurückgreifen. Erstellen Sie `Click` Ereignishandler für beide Schaltflächen-Steuerelemente, sodass Sie `ReturnToDisplayInterface`, eine Methode, die Sie vorübergehend hinzufügen werden, aufgerufen werden. Zusätzlich zum Ausblenden des `InsertingInterface` Bereichs und der Anzeige des `DisplayInterface` Panel muss die `ReturnToDisplayInterface`-Methode die websteuer Elemente in den Zustand der vorabbearbeitung zurücksetzen. Dies umfasst das Festlegen der DropDownLists-`SelectedIndex` Eigenschaften auf 0 und das Löschen der `Text` Eigenschaften der Textfeld-Steuerelemente.

> [!NOTE]
> Berücksichtigen Sie, was passieren kann, wenn die Steuerelemente vor dem zurückkehren zur Anzeige Schnittstelle nicht in ihren Zustand vor der Bearbeitung zurückgegeben wurden. Wenn ein Benutzer auf die Schaltfläche Produktversand verarbeiten klickt, geben Sie die Produkte der Lieferung ein, und klicken Sie dann auf Produkte aus Lieferung hinzufügen. Dadurch werden die Produkte hinzugefügt, und der Benutzer wird an die Anzeige Schnittstelle zurückgegeben. An diesem Punkt kann der Benutzer eine weitere Lieferung hinzufügen. Wenn Sie auf die Schaltfläche "Produktversand verarbeiten" klicken, werden Sie zur einfügeschnittstelle zurückkehren, aber die DropDownList-Auswahl und die TextBox-Werte werden weiterhin mit ihren vorherigen Werten aufgefüllt.

[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Beide `Click` Ereignishandler greifen einfach auf die `ReturnToDisplayInterface`-Methode zu, obwohl wir zum Hinzufügen von Produkten aus der Lieferung `Click` Ereignishandler in Schritt 4 zurückkehren und Code zum Speichern der Produkte hinzufügen. `ReturnToDisplayInterface` beginnt mit dem Zurückgeben der `Suppliers` und `Categories` Dropdown Listen zu ihren ersten Optionen. Die beiden Konstanten `firstControlID` und `lastControlID` die Start-und End Control Index-Werte, die in Benennen der Product Name-und Unit Price-Textfelder in der einfügschnittstelle verwendet werden, und werden in den Begrenzungen der `for`-Schleife verwendet, mit der die `Text` Eigenschaften der TextBox-Steuerelemente auf eine leere Zeichenfolge zurückgesetzt werden. Zum Schluss werden die Panel-`Visible` Eigenschaften zurückgesetzt, sodass die einfügende Schnittstelle ausgeblendet und die Anzeige Schnittstelle angezeigt wird.

Nehmen Sie sich einen Moment Zeit, um diese Seite in einem Browser zu testen. Beim ersten Besuch der Seite sollte die Anzeige Schnittstelle wie in Abbildung 5 gezeigt angezeigt werden. Klicken Sie auf die Schaltfläche Produktlieferung verarbeiten. Die Seite wird Postback angezeigt, und Sie sollten jetzt die einfügeschnittstelle sehen, wie in Abbildung 12 dargestellt. Wenn Sie auf die Schaltflächen Produkte aus Sendung hinzufügen oder Abbrechen klicken, werden Sie zur Anzeige Schnittstelle zurückgegeben.

> [!NOTE]
> Wenn Sie die einfügeschnittstelle anzeigen, nehmen Sie sich einen Moment Zeit, um die comparevalidators für die Einzelpreis-Textfelder zu testen. Wenn Sie auf die Schaltfläche Produkte aus Lieferung hinzufügen klicken, wird eine Client seitige MessageBox-Warnung angezeigt, die ungültige Währungswerte oder Preise mit einem Wert kleiner als 0 (null) aufweist.

[![die Schaltfläche "Einfügen" angezeigt wird](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Abbildung 12**: die einfügeschnittstelle wird angezeigt[,](batch-inserting-cs/_static/image36.png)nachdem auf die Schaltfläche "Produktversand verarbeiten" geklickt wurde

## <a name="step-4-adding-the-products"></a>Schritt 4: Hinzufügen der Produkte

Für dieses Lernprogramm müssen Sie lediglich die Produkte in der-Datenbank in der Schaltfläche Produkte aus Lieferung hinzufügen `Click` Ereignis Handlers speichern. Dies kann erreicht werden, indem ein `ProductsDataTable` erstellt und für jeden bereitgestellten Produktnamen eine `ProductsRow` Instanz hinzugefügt wird. Nachdem diese `ProductsRow` s hinzugefügt wurden, wird die `ProductsBLL` Class s `UpdateWithTransaction`-Methode aufgerufen, die die `ProductsDataTable`übergibt. Erinnern Sie sich daran, dass die `UpdateWithTransaction`-Methode, die im Tutorial zum [Umpacken von Daten Bank Änderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md) erstellt wurde, die `ProductsDataTable` an die `ProductsTableAdapter` s `UpdateWithTransaction`-Methode übergibt. Von dort aus wird eine ADO.NET-Transaktion gestartet, und der TableAdapter gibt eine `INSERT`-Anweisung für jede hinzugefügte `ProductsRow` in der Datentabelle aus. Wenn alle Produkte ohne Fehler hinzugefügt werden, wird für die Transaktion ein Commit ausgeführt; andernfalls wird ein Rollback ausgeführt.

Der Code für die Schaltfläche Produkte aus Lieferung hinzufügen `Click` Ereignishandler muss ebenfalls eine gewisse Fehlerüberprüfung durchführen. Da keine "Requirements dfieldvalidators" in der einfügeschnittstelle verwendet werden, könnte ein Benutzer einen Preis für ein Produkt eingeben, während der Name ausgelassen wird. Da der Name des Produkts erforderlich ist, müssen wir bei einer solchen Bedingung den Benutzer benachrichtigen und die Einfügungen nicht fortsetzen. Der gesamte `Click` Ereignishandlercode folgt:

[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Der Ereignishandler beginnt mit dem sicherstellen, dass die `Page.IsValid`-Eigenschaft den Wert `true`zurückgibt. Wenn `false`zurückgegeben wird, bedeutet dies, dass mindestens ein comparevalidators ungültige Daten meldet. in einem solchen Fall möchten wir nicht versuchen, die eingegebenen Produkte einzufügen, oder es wird eine Ausnahme ausgelöst, wenn versucht wird, den Wert des vom Benutzer eingegebenen Einheiten Preises der `ProductsRow` s `UnitPrice`-Eigenschaft zuzuweisen.

Als nächstes wird eine neue `ProductsDataTable` Instanz erstellt (`products`). Eine `for`-Schleife wird verwendet, um die Textfelder Product Name und Unit Price zu durchlaufen, und die `Text` Eigenschaften werden in die lokalen Variablen `productName` und `unitPrice`eingelesen. Wenn der Benutzer einen Wert für den Einheitspreis eingegeben hat, aber nicht für den entsprechenden Produktnamen, zeigt der `StatusLabel` die Meldung an, wenn Sie einen unitpreis angeben, müssen Sie auch den Namen des Produkts einschließen, und der Ereignishandler wird beendet.

Wenn ein Produktname angegeben wurde, wird mithilfe der `NewProductsRow`-Methode `ProductsDataTable` s eine neue `ProductsRow` Instanz erstellt. Diese neue `ProductsRow` Instanz s `ProductName` Eigenschaft wird auf das Textfeld aktueller Produktname festgelegt, während die Eigenschaften `SupplierID` und `CategoryID` den `SelectedValue` Eigenschaften der Dropdown Listen im Header der Einfügungs Schnittstelle zugewiesen werden. Wenn der Benutzer einen Wert für den Produkt-e-Preis eingegeben hat, wird er der `ProductsRow` Instanz s `UnitPrice` Eigenschaft zugewiesen. Andernfalls wird die-Eigenschaft nicht zugewiesen, was zu einem `NULL` Wert für `UnitPrice` in der Datenbank führt. Zum Schluss werden die `Discontinued`-und `UnitsOnOrder` Eigenschaften den hart codierten Werten `false` bzw. 0 (null) zugewiesen.

Nachdem die Eigenschaften der `ProductsRow` Instanz zugewiesen wurden, wird Sie der `ProductsDataTable`hinzugefügt.

Nach Abschluss der `for` Schleife überprüfen wir, ob Produkte hinzugefügt wurden. Der Benutzer hat möglicherweise auf die Produkte aus Lieferung hinzufügen geklickt, bevor er Produktnamen oder Preise eingegeben hat. Wenn mindestens ein Produkt im `ProductsDataTable`vorhanden ist, wird die `ProductsBLL` Class s `UpdateWithTransaction`-Methode aufgerufen. Als nächstes werden die Daten an die `ProductsGrid` GridView zurückgesetzt, sodass die neu hinzugefügten Produkte in der Anzeige Schnittstelle angezeigt werden. Der `StatusLabel` wird aktualisiert, um eine Bestätigungsmeldung anzuzeigen, und die `ReturnToDisplayInterface` wird aufgerufen, wobei die einfügeschnittstelle ausgeblendet und die Anzeige Schnittstelle angezeigt wird.

Wenn keine Produkte eingegeben wurden, wird die einfügende Schnittstelle angezeigt, aber es wurden keine Produkte hinzugefügt. Geben Sie die Produktnamen und die Preis Einheiten in die Textfelder ein.

In Abbildung s 13, 14 und 15 werden die Einfüge-und Anzeige Schnittstellen in Aktion angezeigt. In Abbildung 13 hat der Benutzer einen Einzelpreis Wert ohne einen entsprechenden Produktnamen eingegeben. In Abbildung 14 wird die Anzeige Schnittstelle angezeigt, nachdem drei neue Produkte erfolgreich hinzugefügt wurden, während Abbildung 15 zwei der neu hinzugefügten Produkte in der GridView anzeigt (die dritte ist auf der vorherigen Seite).

[![ein Produkt Name erforderlich ist, wenn ein Einzelpreis eingegeben wird](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Abbildung 13**: ein Produkt Name ist erforderlich, wenn ein Einheitspreis eingegeben wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image39.png))

[![drei neue Veggies für den Lieferanten Mayumi s hinzugefügt.](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Abbildung 14**: dem Lieferanten Mayumi s wurden drei neue Veggies hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image42.png))

[![die neuen Produkte finden Sie auf der letzten Seite der GridView.](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Abbildung 15**: die neuen Produkte finden Sie auf der letzten Seite der GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](batch-inserting-cs/_static/image45.png))

> [!NOTE]
> Die in diesem Tutorial verwendete Batch-Einfügungs Logik umschließt die Einfügungen innerhalb des Umfangs der Transaktion. Um dies zu überprüfen, führen Sie absichtlich einen Fehler auf Datenbankebene ein. Anstatt z. b. die neue `ProductsRow` Instanz s `CategoryID` Eigenschaft dem ausgewählten Wert in der `Categories` DropDownList zuzuweisen, weisen Sie ihn einem Wert wie `i * 5`zu. Hier ist `i` der Schleifen Indexer und hat Werte zwischen 1 und 5. Beim Hinzufügen von zwei oder mehr Produkten in Batch INSERT hat das erste Produkt daher einen gültigen `CategoryID` Wert (5), nachfolgende Produkte verfügen jedoch über `CategoryID` Werte, die nicht mit `CategoryID` Werten in der `Categories` Tabelle identisch sind. Dies hat zur Folge, dass während der ersten `INSERT` erfolgreich ist, dass nachfolgende mit einer Verletzung der Fremdschlüssel Einschränkung fehlschlagen. Da die Batch Einfügung atomarisch ist, wird für den ersten `INSERT` ein Rollback ausgeführt, und die Datenbank wird vor Beginn des Batch Einfügungs Prozesses in den Zustand zurückversetzt.

## <a name="summary"></a>Zusammenfassung

In diesem und den beiden vorherigen Tutorials haben wir Schnittstellen erstellt, die das Aktualisieren, löschen und Einfügen von Daten Batches ermöglichen, die alle die Transaktionsunterstützung verwendet haben, die wir der Datenzugriffs Ebene im Tutorial Wrapping von Daten [Bank Änderungen innerhalb einer Transaktion](wrapping-database-modifications-within-a-transaction-cs.md) hinzugefügt haben. In bestimmten Szenarien verbessern diese Benutzeroberflächen für die Batch Verarbeitung die Effizienz von Endbenutzern erheblich, indem die Anzahl der Klicks, Postbacks und Tastatur-zu-Maus-Kontextwechsel gesenkt werden, während gleichzeitig die Integrität der zugrunde liegenden Daten gewahrt bleibt.

In diesem Tutorial wird die Arbeit mit Batch Daten behandelt. Der nächste Satz von Tutorials untersucht eine Reihe von erweiterten Szenarien für die Datenzugriffs Schicht, einschließlich der Verwendung von gespeicherten Prozeduren in den TableAdapter-Methoden, dem Konfigurieren der Einstellungen für die Verbindungs-und Befehls Ebene in der Dal, dem Verschlüsseln von Verbindungs Zeichenfolgen und mehr.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Hilton giesdenow und S ren Jacob Lauritsen. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](batch-deleting-cs.md)
> [Weiter](wrapping-database-modifications-within-a-transaction-vb.md)
