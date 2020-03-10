---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Ausführen von Batch AktualisierungenC#() | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie einen vollständig bearbeitbaren DataList erstellen können, in dem sich alle Elemente im Bearbeitungsmodus befinden und deren Werte durch Klicken auf die Schaltfläche "Alle aktualisieren" auf der Schaltfläche "Alle aktualisieren" gespeichert werden können.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: cde12a4d24555216adc49dd02818901278932eaa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78480045"
---
# <a name="performing-batch-updates-c"></a>Durchführen von Batchupdates (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) oder [PDF herunterladen](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Erfahren Sie, wie Sie einen vollständig bearbeitbaren DataList erstellen können, in dem sich alle Elemente im Bearbeitungsmodus befinden und deren Werte durch Klicken auf die Schaltfläche "Alle aktualisieren" auf der Seite gespeichert werden können.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) wurde erläutert, wie ein DataList auf Element Ebene erstellt wird. Ebenso wie die standardmäßige bearbeitbare GridView enthielt jedes Element im DataList eine Bearbeitungs Schaltfläche, die beim Klicken auf das Element bearbeitet werden kann. Diese Bearbeitung auf Element Ebene eignet sich zwar gut für Daten, die nur gelegentlich aktualisiert werden, aber für bestimmte Anwendungsszenarien ist es erforderlich, dass der Benutzer viele Datensätze bearbeitet. Wenn ein Benutzer Dutzende von Datensätzen bearbeiten muss und gezwungen ist, auf Bearbeiten zu klicken, die Änderungen vorzunehmen und für jeden einzelnen Benutzer auf aktualisieren zu klicken, kann die Anzahl der Klicks die Produktivität beeinträchtigen. In solchen Fällen ist es eine bessere Option, einen vollständig bearbeitbaren DataList bereitzustellen, einer, in dem sich *alle* Elemente im Bearbeitungsmodus befinden und deren Werte bearbeitet werden können, indem Sie auf die Schaltfläche Alle aktualisieren auf der Seite klicken (siehe Abbildung 1).

[![jedes Element in einem vollständig bearbeitbaren DataList kann geändert werden.](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Abbildung 1**: jedes Element in einem vollständig bearbeitbaren DataList kann geändert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-batch-updates-cs/_static/image3.png))

In diesem Tutorial wird erläutert, wie Sie es Benutzern ermöglichen können, die Adressinformationen von Lieferanten mithilfe eines vollständig bearbeitbaren DataList zu aktualisieren.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Schritt 1: Erstellen der bearbeitbaren Benutzeroberfläche in der DataList s-ItemTemplate

Im vorherigen Tutorial, in dem wir einen standardmäßigen bearbeitbaren DataList auf Element Ebene erstellen, wurden zwei Vorlagen verwendet:

- `ItemTemplate` enthielt die schreibgeschützte Benutzeroberfläche (die Beschriftungs-websteuer Elemente zum Anzeigen der Namen und Preise der einzelnen Produkte).
- `EditItemTemplate` enthielt die Benutzeroberfläche für den Bearbeitungsmodus (die zwei TextBox-websteuer Elemente).

Die DataList s `EditItemIndex`-Eigenschaft legt fest, welche `DataListItem` (sofern vorhanden) mit dem `EditItemTemplate`gerendert wird. Insbesondere die `DataListItem`, deren `ItemIndex` Wert mit der DataList s-`EditItemIndex` Eigenschaft übereinstimmt, werden mithilfe des `EditItemTemplate`gerendert. Dieses Modell funktioniert gut, wenn jeweils nur ein Element bearbeitet werden kann, jedoch beim Erstellen eines vollständig bearbeitbaren DataList.

Für einen vollständig bearbeitbaren DataList sollen *alle* `DataListItem` s mithilfe der bearbeitbaren Schnittstelle dargestellt werden. Die einfachste Möglichkeit, dies zu erreichen, besteht darin, die bearbeitbare Schnittstelle im `ItemTemplate`zu definieren. Zum Ändern der Adressinformationen des Lieferanten enthält die bearbeitbare Schnittstelle den Lieferanten Namen als Text und dann Textfelder für die Werte "Address", "City" und "Country".

Öffnen Sie zunächst die Seite `BatchUpdate.aspx`, fügen Sie ein DataList-Steuerelement hinzu, und legen Sie dessen `ID`-Eigenschaft auf `Suppliers`fest. Wählen Sie aus dem DataList s-Smarttag ein neues ObjectDataSource-Steuerelement mit dem Namen `SuppliersDataSource`.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "suppliersdatasource".](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource mit dem Namen "`SuppliersDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-batch-updates-cs/_static/image6.png))

Konfigurieren Sie ObjectDataSource zum Abrufen von Daten mithilfe der `SuppliersBLL` Class s `GetSuppliers()`-Methode (siehe Abbildung 3). Wie im vorherigen Tutorial, anstatt die Lieferanteninformationen über die ObjectDataSource zu aktualisieren, arbeiten wir direkt mit der Geschäftslogik Schicht. Legen Sie daher auf der Registerkarte aktualisieren die Dropdown Liste auf (None) fest (siehe Abbildung 4).

[![Abrufen von Lieferanteninformationen mithilfe der getsuppliers ()-Methode](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Abbildung 3**: Abrufen von Lieferanteninformationen mithilfe der `GetSuppliers()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-batch-updates-cs/_static/image9.png))

[![die Dropdown Liste auf der Registerkarte "Aktualisieren" auf (keine) festlegen](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Abbildung 4**: Festlegen der Dropdown Liste auf (None) auf der Registerkarte "Aktualisieren" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-batch-updates-cs/_static/image12.png))

Nachdem Sie den Assistenten abgeschlossen haben, generiert Visual Studio automatisch die DataList-`ItemTemplate`, um die einzelnen Datenfelder anzuzeigen, die von der Datenquelle in einem Label-websteuer Element zurückgegeben werden. Wir müssen diese Vorlage so ändern, dass Sie stattdessen die Bearbeitungs Schnittstelle bereitstellt. Die `ItemTemplate` kann über den Designer angepasst werden, indem Sie die Option Vorlagen bearbeiten aus dem DataList s Smarttags oder direkt über die deklarative Syntax verwenden.

Nehmen Sie sich einen Moment Zeit, um eine Bearbeitungs Schnittstelle zu erstellen, die den Namen des Lieferanten als Text anzeigt. er enthält jedoch Textfelder für die Werte von Supplier s Address, City und Country. Nachdem Sie diese Änderungen vorgenommen haben, sollte die deklarative Syntax Ihrer Seite in etwa wie folgt aussehen:

[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Wie beim vorherigen Tutorial muss für den DataList in diesem Tutorial der Ansichts Zustand aktiviert sein.

In der `ItemTemplate` I m, die zwei neue CSS-Klassen verwendet, `SupplierPropertyLabel` und `SupplierPropertyValue`, die der `Styles.css`-Klasse hinzugefügt wurden und für die Verwendung der gleichen Stileinstellungen wie die `ProductPropertyLabel`-und `ProductPropertyValue`-CSS-Klassen konfiguriert wurden.

[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Nachdem Sie diese Änderungen vorgenommen haben, besuchen Sie diese Seite über einen Browser. Wie in Abbildung 5 gezeigt, zeigt jedes DataList-Element den Lieferanten Namen als Text an und verwendet Textfelder, um die Adresse, die Stadt und das Land anzuzeigen.

[![jeder Lieferant in der DataList kann bearbeitet werden.](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Abbildung 5**: jeder Lieferant in der DataList-Tabelle kann bearbeitet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-batch-updates-cs/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Schritt 2: Hinzufügen der Schaltfläche "Alle aktualisieren"

Obwohl für jeden Lieferanten in Abbildung 5 die Felder Adresse, Ort und Land in einem Textfeld angezeigt werden, ist derzeit keine Aktualisierungs Schaltfläche verfügbar. Anstatt eine Schaltfläche "Aktualisieren" pro Element zu haben, wird mit vollständig bearbeitbaren datalisten in der Regel eine einzelne Schaltfläche "Alle aktualisieren" auf der Seite angezeigt, die *alle* Datensätze in der DataList aktualisiert, wenn darauf geklickt wird. Fügen Sie in diesem Tutorial zwei Update alle-Schaltflächen hinzu: eins oben auf der Seite und eins unten (auch wenn auf eine der beiden Schaltflächen geklickt wird).

Fügen Sie zunächst ein Schaltflächen-websteuer Element oberhalb des DataList hinzu, und legen Sie dessen `ID`-Eigenschaft auf `UpdateAll1`fest. Fügen Sie anschließend unter dem DataList das zweite Schaltflächen-websteuer Element hinzu, und legen Sie dessen `ID` auf `UpdateAll2`fest. Legen Sie die `Text` Eigenschaften für die beiden Schaltflächen so fest, dass alle aktualisiert werden. Erstellen Sie abschließend Ereignishandler für beide Schaltflächen `Click` Ereignisse. Anstatt die Aktualisierungs Logik in den einzelnen Ereignis Handlern zu duplizieren, lassen Sie diese Logik mit einer dritten Methode umgestalten, `UpdateAllSupplierAddresses`, damit die Ereignishandler einfach diese dritte Methode aufrufen.

[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Abbildung 6 zeigt die Seite, nachdem die Schaltflächen alle aktualisieren hinzugefügt wurden.

[![beiden Schaltflächen alle aktualisieren wurden der Seite hinzugefügt.](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Abbildung 6**: zwei Schaltflächen "Alle aktualisieren" wurden der Seite hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](performing-batch-updates-cs/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Schritt 3: Aktualisieren aller Adressinformationen des Lieferanten

Wenn alle DataList s-Elemente die Bearbeitungs Schnittstelle anzeigen und durch Hinzufügen der Schaltflächen alle aktualisieren, wird nur noch der Code zum Ausführen des Batch Updates geschrieben. Insbesondere müssen wir die DataList s-Elemente durchlaufen und die `SuppliersBLL` Class s `UpdateSupplierAddress`-Methode für jeden einzelnen aufzurufen.

Die Auflistung von `DataListItem` Instanzen, auf die der DataList durch die DataList- [`Items` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)zugegriffen werden kann. Mit einem Verweis auf einen `DataListItem`können wir die entsprechende `SupplierID` aus der `DataKeys` Collection erfassen und Programm gesteuert auf die TextBox-websteuer Elemente innerhalb der `ItemTemplate` verweisen, wie im folgenden Code veranschaulicht:

[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Wenn der Benutzer auf eine der Schaltflächen alle Aktualisieren klickt, durchläuft die `UpdateAllSupplierAddresses`-Methode jede `DataListItem` im `Suppliers` DataList und ruft die `SuppliersBLL` Class s `UpdateSupplierAddress`-Methode auf und übergibt dabei die entsprechenden Werte. Ein nicht eingegebener Wert für "Address", "City" oder "Country Pass" ist ein Wert von "`Nothing`", "`UpdateSupplierAddress`" (anstelle einer leeren Zeichenfolge), was zu einer Datenbank `NULL` für die zugrunde liegenden Felder "Datensatz" führt.

> [!NOTE]
> Als Ergänzung können Sie der Seite ein websteuer Element für die Status Bezeichnung hinzufügen, das nach der Ausführung des Batch Updates eine Bestätigungsmeldung bereitstellt.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>Nur die geänderten Adressen werden aktualisiert

Der für dieses Tutorial verwendete Batch Update Algorithmus Ruft die `UpdateSupplierAddress`-Methode für *jeden* Lieferanten in der DataList auf, unabhängig davon, ob Ihre Adressinformationen geändert wurden. Diese Blind Updates sind zwar in der Regel kein Leistungsproblem, aber Sie können zu überflüssigen Datensätzen führen, wenn Sie Änderungen an der Datenbanktabelle erneut überwachen. Wenn Sie z. b. Trigger verwenden, um alle `UPDATE` s in der `Suppliers` Tabelle in einer Überwachungs Tabelle aufzuzeichnen, wird jedes Mal, wenn ein Benutzer auf die Schaltfläche Alle aktualisieren klickt, ein neuer Überwachungsdaten Satz für jeden Lieferanten im System erstellt, unabhängig davon, ob der Benutzer Änderungen vorgenommen hat.

Die ADO.net datdatababel-Klasse und die DataAdapter-Klasse sind für die Unterstützung von Batch Aktualisierungen konzipiert, wenn nur geänderte, gelöschte und neue Datensätze zu Datenbankkommunikation führen. Jede Zeile in der Datentabelle verfügt über eine [`RowState`-Eigenschaft](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) , die angibt, ob die Zeile der Datentabelle hinzugefügt, aus ihr gelöscht, geändert oder unverändert bleibt. Wenn eine Datentabelle anfänglich aufgefüllt wird, werden alle Zeilen als unverändert markiert. Wenn Sie den Wert einer beliebigen Zeilen Spalte ändern, wird die Zeile als geändert markiert.

In der `SuppliersBLL`-Klasse aktualisieren wir die Adressinformationen des angegebenen Lieferanten, indem wir zuerst den einzelnen Lieferantendaten Satz in einem `SuppliersDataTable` lesen und dann die Werte für `Address`, `City`und `Country` Spalten mithilfe des folgenden Codes festlegen:

[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Mit diesem Code werden die Werte für die Übergabe Adresse, den Ort und den Wert des `SuppliersRow` in der `SuppliersDataTable`, unabhängig davon, ob sich die Werte geändert haben, nicht geändert. Diese Änderungen führen dazu, dass die `SuppliersRow` s-Eigenschaft `RowState` als geändert markiert wird. Wenn die `Update`-Methode der Datenzugriffs Ebene aufgerufen wird, wird erkannt, dass die `SupplierRow` geändert wurde und daher einen `UPDATE`-Befehl an die Datenbank sendet.

Stellen Sie sich jedoch vor, dass wir dieser Methode Code hinzugefügt haben, um nur die übergebenen Adressen, Städte und Länder Werte zuzuweisen, wenn Sie sich von den `SuppliersRow` vorhandenen Werten unterscheiden. Wenn die Adresse, die Stadt und das Land mit den vorhandenen Daten identisch sind, werden keine Änderungen vorgenommen, und die `SupplierRow` s `RowState` wird als unverändert markiert. Das Ergebnis ist, dass beim Aufrufen der dal s-`Update`-Methode kein Daten Bank Aufruf erfolgt, da die `SuppliersRow` nicht geändert wurde.

Um diese Änderung zu implementieren, ersetzen Sie die-Anweisungen, die die übergebenen Adressen, den Ort und die Länder Werte blind durch den folgenden Code zuweisen:

[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Mit diesem hinzugefügten Code sendet die DAL s-`Update`-Methode eine `UPDATE`-Anweisung nur für die Datensätze, deren Adress bezogene Werte geändert wurden.

Alternativ können wir verfolgen, ob es Unterschiede zwischen den übergebenen Adressfeldern und den Datenbankdaten gibt. wenn keine vorhanden sind, umgehen Sie einfach den aufrufungs-`Update` Methode. Diese Vorgehensweise funktioniert gut, wenn Sie die direkte DB-Methode wieder verwenden, da die direkte DB-Methode eine `SuppliersRow`-Instanz weitergegeben hat, deren `RowState` geprüft werden kann, um zu bestimmen, ob tatsächlich ein Daten Bank Aufrufe erforderlich ist.

> [!NOTE]
> Jedes Mal, wenn die `UpdateSupplierAddress`-Methode aufgerufen wird, wird ein Aufruf an die Datenbank ausgeführt, um Informationen über den aktualisierten Datensatz abzurufen. Wenn dann Änderungen an den Daten vorgenommen werden, wird ein weiterer Daten Banksatz aufgerufen, um die Tabellenzeile zu aktualisieren. Dieser Workflow kann durch Erstellen einer `UpdateSupplierAddress`-Methoden Überladung optimiert werden, die eine `EmployeesDataTable` Instanz akzeptiert, die *alle* Änderungen von der `BatchUpdate.aspx` Seite aufweist. Anschließend könnte es einen Daten Bankvorgang zum Abrufen aller Datensätze aus der `Suppliers` Tabelle erstellen. Die beiden Resultsets können dann aufgezählt werden, und nur die Datensätze, in denen Änderungen aufgetreten sind, können aktualisiert werden.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie einen vollständig bearbeitbaren DataList erstellen, der es einem Benutzer ermöglicht, die Adressinformationen für mehrere Lieferanten schnell zu ändern. Wir haben begonnen, indem wir die Bearbeitungs Schnittstelle für das TextBox-websteuer Element für die Werte "Supplier s Address", "City" und "Country" im DataList-`ItemTemplate` Als nächstes haben wir alle Schaltflächen Aktualisieren oberhalb und unterhalb des DataList-Steuerelemente hinzugefügt. Nachdem ein Benutzer seine Änderungen vorgenommen und auf eine der Schaltflächen "Alle aktualisieren" geklickt hat, werden die `DataListItem` s aufgelistet, und es wird ein aufzurufende `SuppliersBLL` Klassen-`UpdateSupplierAddress` Methode ausgelöst.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Prüfer für dieses Tutorial waren Zack Jones und Ken pespisa. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [Weiter](handling-bll-and-dal-level-exceptions-cs.md)
