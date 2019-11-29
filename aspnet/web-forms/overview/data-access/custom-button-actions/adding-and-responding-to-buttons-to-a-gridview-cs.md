---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: Hinzufügen und reagieren auf Schaltflächen zu einer GridView (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Schaltflächen, sowohl einer Vorlage als auch den Feldern eines GridView-Steuer Elements oder DetailsView-Steuer Elements hinzufügen. Wir werden uns vor allem
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5c87386e4fe2c53b39162071689f2522dcc6c7ac
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74602405"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>Hinzufügen von Schaltflächen zu einem GridView-Steuerelement und Zuweisen von Funktionen für diese Schaltflächen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe) oder [PDF herunterladen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Schaltflächen, sowohl einer Vorlage als auch den Feldern eines GridView-Steuer Elements oder DetailsView-Steuer Elements hinzufügen. Insbesondere erstellen wir eine Schnittstelle mit einer FormView, mit der der Benutzer die Seite durch die Lieferanten durchlaufen kann.

## <a name="introduction"></a>Einführung

Obwohl viele Berichterstattungs Szenarien schreibgeschützten Zugriff auf die Berichtsdaten beinhalten, ist es nicht ungewöhnlich, dass Berichte die Möglichkeit enthalten, Aktionen basierend auf den angezeigten Daten auszuführen. In der Regel umfasste dies das Hinzufügen eines Schaltflächen-, LinkButton-oder ImageButton-websteuer Elements mit jedem Datensatz, der im Bericht angezeigt wird. Wenn Sie darauf klicken, wird ein Postback ausgelöst und ein serverseitiger Code aufgerufen. Das häufigste Beispiel ist das Bearbeiten und Löschen der Daten auf Datensatz-Basis. Wie wir bereits mit der [Übersicht über das Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Lernprogrammen begonnen haben, ist das Bearbeiten und löschen so üblich, dass die Steuerelemente GridView, DetailsView und FormView diese Funktionalität unterstützen können, ohne dass eine einzige Codezeile geschrieben werden muss.

Zusätzlich zu den Schaltflächen Bearbeiten und löschen können auch das GridView-Steuerelement, das DetailsView-Steuerelement und das FormView-Steuerelement Schaltflächen, LinkButtons oder imagebuttons enthalten, die beim Klicken auf eine benutzerdefinierte serverseitige Logik durchgeführt werden. In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Schaltflächen, sowohl einer Vorlage als auch den Feldern eines GridView-Steuer Elements oder DetailsView-Steuer Elements hinzufügen. Insbesondere erstellen wir eine Schnittstelle mit einer FormView, mit der der Benutzer die Seite durch die Lieferanten durchlaufen kann. Für einen bestimmten Lieferanten zeigt die FormView Informationen über den Lieferanten zusammen mit einem Schaltflächen-websteuer Element an, das, wenn darauf geklickt wird, alle zugehörigen Produkte als nicht mehr unterstützt markiert. Außerdem listet eine GridView die Produkte auf, die vom ausgewählten Lieferanten bereitgestellt werden. jede Zeile enthält die Schaltflächen Preis Preis und Preisnachlass Preis, die, wenn Sie darauf geklickt haben, den `UnitPrice` des Produkts um 10% erhöhen oder verringern (siehe Abbildung 1).

[![der FormView und GridView Schaltflächen enthalten, die benutzerdefinierte Aktionen ausführen.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**Abbildung 1**: die Form View und die GridView enthalten Schaltflächen, die benutzerdefinierte Aktionen ausführen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Schaltflächen-tutorialwebseiten

Bevor wir uns mit dem Hinzufügen von benutzerdefinierten Schaltflächen beschäftigen, nehmen wir uns zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen. Fügen Sie zunächst einen neuen Ordner mit dem Namen `CustomButtons`hinzu. Fügen Sie dann die folgenden beiden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `CustomButtons.aspx`

![Hinzufügen der ASP.NET-Seiten für benutzerdefinierte Schaltflächen bezogene Tutorials](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen der ASP.NET-Seiten für benutzerdefinierte Schaltflächen bezogene Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `CustomButtons` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Designansicht der Seite ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**Abbildung 3**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))

Fügen Sie abschließend die Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem `<siteMapNode>`Paging und Sortieren das folgende Markup hinzu:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials zum Bearbeiten, einfügen und löschen.

![Die Site Übersicht enthält jetzt den Eintrag für das Tutorial benutzerdefinierte Schaltflächen.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**Abbildung 4**: die Site Übersicht enthält jetzt den Eintrag für das Tutorial zu benutzerdefinierten Schaltflächen.

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Schritt 2: Hinzufügen einer FormView, die die Lieferanten auflistet

Wir beginnen mit diesem Tutorial, indem wir die FormView hinzufügen, die die Lieferanten auflistet. Wie in der Einführung erläutert, ermöglicht diese FormView dem Benutzer das Durchlaufen der Lieferanten und zeigt die Produkte an, die vom Lieferanten in einer GridView bereitgestellt werden. Außerdem enthält diese FormView eine Schaltfläche, mit der alle Produkte des Lieferanten als nicht mehr unterstützt markiert werden, wenn darauf geklickt wird. Bevor wir uns mit dem Hinzufügen der benutzerdefinierten Schaltfläche zu FormView befassen, erstellen wir zunächst die FormView, sodass die Lieferanteninformationen angezeigt werden.

Öffnen Sie zunächst die Seite `CustomButtons.aspx` im Ordner `CustomButtons`. Fügen Sie der Seite eine FormView hinzu, indem Sie Sie aus der Toolbox auf den Designer ziehen und deren `ID`-Eigenschaft auf `Suppliers`festlegen. Wählen Sie aus dem Smarttag von FormView eine neue ObjectDataSource mit dem Namen `SuppliersDataSource`.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "suppliersdatasource".](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource mit dem Namen "`SuppliersDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))

Konfigurieren Sie diese neue ObjectDataSource so, dass Sie von der `GetSuppliers()`-Methode der `SuppliersBLL` Klasse abgefragt wird (siehe Abbildung 6). Da diese FormView keine Schnittstelle zum Aktualisieren der Lieferanteninformationen bereitstellt, wählen Sie in der Dropdown Liste auf der Registerkarte aktualisieren die Option (keine) aus.

[![die Datenquelle so konfigurieren, dass die suppliersbll Class s getsuppliers ()-Methode verwendet wird.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**Abbildung 6**: Konfigurieren der Datenquelle für die Verwendung der `GetSuppliers()` Methode der `SuppliersBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))

Nach dem Konfigurieren von ObjectDataSource generiert Visual Studio eine `InsertItemTemplate`, `EditItemTemplate`und `ItemTemplate` für die Form View. Entfernen Sie die `InsertItemTemplate`, und `EditItemTemplate` und ändern Sie die `ItemTemplate`, sodass nur der Firmenname und die Telefonnummer des Lieferanten angezeigt werden. Aktivieren Sie schließlich die Paging-Unterstützung für die FormView, indem Sie das Kontrollkästchen Paging aktivieren des Smarttags aktivieren (oder indem Sie die `AllowPaging`-Eigenschaft auf `True`festlegen). Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

Abbildung 7 zeigt die Seite custombuttons. aspx, wenn Sie in einem Browser angezeigt wird.

[![FormView listet die Felder "CompanyName" und "Phone" des aktuell ausgewählten Lieferanten auf.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**Abbildung 7**: FormView listet die Felder "`CompanyName`" und "`Phone`" des aktuell ausgewählten Lieferanten auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>Schritt 3: Hinzufügen einer GridView, in der die Produkte des ausgewählten Lieferanten aufgeführt sind

Bevor wir die Schaltfläche Alle Produkte beenden zur Vorlage von FormView hinzufügen, fügen wir zuerst eine GridView unterhalb von FormView hinzu, in der die Produkte aufgelistet sind, die vom ausgewählten Lieferanten bereitgestellt werden. Fügen Sie hierzu der Seite eine GridView hinzu, legen Sie die `ID`-Eigenschaft auf `SuppliersProducts`fest, und fügen Sie eine neue ObjectDataSource mit dem Namen `SuppliersProductsDataSource`hinzu.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "suppliersproduczdatasource".](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**Abbildung 8**: Erstellen einer neuen ObjectDataSource mit dem Namen "`SuppliersProductsDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))

Konfigurieren Sie diese ObjectDataSource für die Verwendung der `GetProductsBySupplierID(supplierID)` Methode der productbll-Klasse (siehe Abbildung 9). Obwohl diese GridView den Preis eines Produkts anpassen kann, werden die integrierten Funktionen zum Bearbeiten oder löschen in der GridView nicht verwendet. Daher können wir die Dropdown Liste für die Registerkarten aktualisieren, einfügen und Löschen von ObjectDataSource auf (keine) festlegen.

[![konfigurieren Sie die Datenquelle für die Verwendung der Methode getproductbysupplierid (SupplierID) der productbll-Klasse.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**Abbildung 9**: Konfigurieren der Datenquelle für die Verwendung der `GetProductsBySupplierID(supplierID)` Methode der `ProductsBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))

Da die `GetProductsBySupplierID(supplierID)`-Methode einen Eingabeparameter akzeptiert, fordert der ObjectDataSource-Assistent die Quelle dieses Parameter Werts an. Um den `SupplierID` Wert aus der Form Ansicht zu übergeben, legen Sie die Dropdown Liste Parameter Quelle auf Control und die ControlID-Dropdown Liste auf `Suppliers` (die ID der in Schritt 2 erstellten Form Ansicht) fest.

[![angeben, dass der SupplierID-Parameter vom Suppliers-FormView-Steuerelement stammen soll.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**Abbildung 10**: Geben Sie an, dass der *`supplierID`* Parameter aus dem `Suppliers` FormView-Steuerelement stammen soll ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png)).

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, enthält das GridView-Objekt für jedes der Datenfelder des Produkts ein BoundField-oder CheckBoxField-Objekt. Wir kürzen dies, um nur die `ProductName` und `UnitPrice` boundfields zusammen mit dem `Discontinued` CheckBoxField anzuzeigen. Außerdem formatieren wir das `UnitPrice` BoundField so, dass sein Text als Währung formatiert ist. Ihr GridView-und `SuppliersProductsDataSource` ObjectDataSource-deklaratives Markup sollte in etwa wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

An dieser Stelle zeigt unser Tutorial einen Master-/Detailbericht an, der es dem Benutzer ermöglicht, einen Lieferanten aus der FormView oben auszuwählen und die Produkte anzuzeigen, die von diesem Lieferanten über die GridView im unteren Bereich bereitgestellt werden. Abbildung 11 zeigt einen Screenshot dieser Seite, wenn Sie den Tokyo Traders-Lieferanten aus der FormView auswählen.

[![die ausgewählten Lieferanten Produkte in der GridView angezeigt werden.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**Abbildung 11**: die Produkte des ausgewählten Lieferanten werden in der GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Schritt 4: Erstellen von Dal-und BLL-Methoden, um alle Produkte für einen Lieferanten einzustellen

Bevor Sie der FormView eine Schaltfläche hinzufügen können, die die Produkte des Lieferanten entfernt, wenn Sie darauf klicken, werden alle Produkte des Lieferanten entfernt. Wir müssen zunächst der dal und der BLL, die diese Aktion ausführt, eine Methode hinzufügen. Insbesondere wird diese Methode mit dem Namen `DiscontinueAllProductsForSupplier(supplierID)`benannt. Wenn Sie auf die Schaltfläche "FormView" klicken, rufen wir diese Methode in der Geschäftslogik Schicht auf und übergeben dabei den `SupplierID`des ausgewählten Lieferanten. die BLL ruft dann die entsprechende Datenzugriffs Schicht-Methode auf, die eine `UPDATE`-Anweisung an die Datenbank ausgibt, die die Produkte des angegebenen Lieferanten disfährt.

Wie in den vorherigen Tutorials beschrieben, verwenden wir einen Bottom-up-Ansatz, beginnend mit dem Erstellen der dal-Methode, der BLL-Methode und schließlich der Implementierung der Funktionalität auf der ASP.NET-Seite. Öffnen Sie das `Northwind.xsd` typisierte DataSet im Ordner `App_Code/DAL`, und fügen Sie dem `ProductsTableAdapter` eine neue Methode hinzu (Klicken Sie mit der rechten Maustaste auf das `ProductsTableAdapter`, und wählen Sie Abfrage hinzufügen). Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen angezeigt, der uns durch den Prozess des Hinzufügens der neuen Methode führt. Beginnen Sie mit der Angabe, dass unsere dal-Methode eine Ad-hoc-SQL-Anweisung verwendet.

[![Erstellen der dal-Methode mithilfe einer Ad-hoc-SQL-Anweisung](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**Abbildung 12**: Erstellen der dal-Methode mithilfe einer Ad-hoc-SQL-Anweisung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))

Im nächsten Schritt werden wir vom Assistenten aufgefordert, den Typ der zu erstellenden Abfrage zu erstellen. Da die `DiscontinueAllProductsForSupplier(supplierID)`-Methode die `Products` Datenbanktabelle aktualisieren muss, muss das `Discontinued` Feld für alle Produkte, die vom angegebenen *`supplierID`* bereitgestellt werden, auf 1 festgelegt werden. es muss eine Abfrage erstellt werden, mit der Daten aktualisiert werden.

[![wählen Sie den Typ der Update Abfrage aus.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**Abbildung 13**: Auswählen des Abfragetyps "Update" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))

Der nächste Assistent zeigt die vorhandene `UPDATE`-Anweisung des TableAdapter an, die jedes der Felder aktualisiert, die in der `Products` Datentabelle definiert sind. Ersetzen Sie diesen Abfragetext durch die folgende Anweisung:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

Nachdem Sie diese Abfrage eingegeben und auf "weiter" geklickt haben, werden die Namen der neuen Methode im letzten Assistenten angezeigt `DiscontinueAllProductsForSupplier`. Beenden Sie den Assistenten durch Klicken auf die Schaltfläche Fertigstellen. Wenn Sie zum DataSet-Designer zurückkehren, sollte in der `ProductsTableAdapter` mit dem Namen `DiscontinueAllProductsForSupplier(@SupplierID)`eine neue Methode angezeigt werden.

[![den Namen der neuen dal-Methode discontinueallproductforsupplier](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**Abbildung 14**: Benennen der neuen dal-Methode `DiscontinueAllProductsForSupplier` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))

Wenn die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der Datenzugriffs Ebene erstellt wurde, besteht die nächste Aufgabe darin, die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der Geschäftslogik Ebene zu erstellen. Öffnen Sie hierzu die Datei `ProductsBLL`-Klasse, und fügen Sie Folgendes hinzu:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

Diese Methode ruft einfach die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der DAL auf und übergibt den bereitgestellten *`supplierID`* Parameterwert. Wenn Geschäftsregeln vorhanden waren, die nur unter bestimmten Umständen die Produkte eines Lieferanten Unternehmen konnten, sollten diese Regeln hier in der BLL implementiert werden.

> [!NOTE]
> Im Gegensatz zu den `UpdateProduct` Überladungen in der `ProductsBLL`-Klasse enthält die Signatur der `DiscontinueAllProductsForSupplier(supplierID)`-Methode das `DataObjectMethodAttribute`-Attribut (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`) nicht. Dadurch wird die `DiscontinueAllProductsForSupplier(supplierID)`-Methode aus der Dropdown Liste Datenquelle konfigurieren von ObjectDataSource auf der Registerkarte Update ausgeschlossen. Dieses Attribut wurde ausgelassen, da wir die `DiscontinueAllProductsForSupplier(supplierID)` Methode direkt von einem Ereignishandler auf unserer ASP.NET-Seite aufrufen.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Schritt 5: Hinzufügen einer Schaltfläche "alle Produkte" zu "FormView"

Wenn die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der BLL und Dal abgeschlossen ist, besteht der letzte Schritt zum Hinzufügen der Möglichkeit, alle Produkte für den ausgewählten Lieferanten einzustellen, dem `ItemTemplate`der FormView ein Schaltflächen-websteuer Element hinzuzufügen. Fügen wir eine solche Schaltfläche unterhalb der Telefonnummer des Lieferanten mit dem Schaltflächen Text hinzu, brechen Sie alle Produkte ab, und klicken Sie auf den `ID`-Eigenschafts Wert `DiscontinueAllProductsForSupplier`. Sie können dieses Schaltflächen-websteuer Element über den Designer hinzufügen, indem Sie auf den Link Vorlagen bearbeiten im Smarttags von FormView (siehe Abbildung 15) oder direkt über die deklarative Syntax klicken.

[![das Schaltflächen-websteuer Element "alle Produkte" für "FormView s ItemTemplate" hinzufügen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**Abbildung 15**: Hinzufügen eines websteuer Elements vom Typ "alle Produkte unterbrechen" zum `ItemTemplate` von FormView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))

Wenn ein Benutzer auf die Schaltfläche klickt, wird ein Postback und das [`ItemCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) von FormView ausgelöst. Zum Ausführen von benutzerdefiniertem Code als Antwort auf die Schaltfläche, auf die geklickt wird, können wir einen Ereignishandler für dieses Ereignis erstellen. Beachten Sie jedoch, dass das `ItemCommand`-Ereignis ausgelöst wird, wenn in der FormView auf eine Schaltfläche, ein LinkButton oder *ein* ImageButton-websteuer Element geklickt wird. Dies bedeutet, dass das `ItemCommand` Ereignis ausgelöst wird, wenn der Benutzer in der Form Ansicht von einer Seite zu einem anderen wechselt. Dies ist der Fall, wenn der Benutzer in einer FormView, die das Einfügen, aktualisieren oder löschen unterstützt, auf neu, bearbeiten oder Löschen klickt.

Da der `ItemCommand` unabhängig von der Schaltfläche, auf die geklickt wird, ausgelöst wird, benötigen wir im Ereignishandler eine Methode, um zu bestimmen, ob auf die Schaltfläche Alle Produkte Abbrechen geklickt wurde oder ob es sich um eine andere Schaltfläche handelt Um dies zu erreichen, können Sie die `CommandName`-Eigenschaft des Schaltflächen-websteuer Elements auf einen identifizierenden Wert festlegen. Wenn auf die Schaltfläche geklickt wird, wird dieser `CommandName` Wert an den `ItemCommand`-Ereignishandler übermittelt, sodass wir bestimmen können, ob auf die Schaltfläche Alle Produkte einstellen geklickt wurde. Legen Sie die `CommandName`-Eigenschaft der Schaltfläche Alle Produkte auf discontinueproducts fest.

Verwenden Sie abschließend ein Client seitiges Bestätigungs Dialogfeld, um sicherzustellen, dass der Benutzer die Produkte des ausgewählten Lieferanten wirklich einstellen möchte. Wie im Lernprogramm zum [Hinzufügen von Client seitiger Bestätigung beim Löschen](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md) gesehen, kann dies mit etwas JavaScript erreicht werden. Legen Sie insbesondere die OnClientClick-Eigenschaft des Schaltflächen-websteuer Elements auf `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Nachdem Sie diese Änderungen vorgenommen haben, sollte die deklarative Syntax von FormView wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das `ItemCommand`-Ereignis der FormView. In diesem Ereignishandler muss zuerst festgestellt werden, ob auf die Schaltfläche Alle Produkte Abbrechen geklickt wurde. Wenn dies der Fall ist, möchten wir eine Instanz der `ProductsBLL` Klasse erstellen und ihre `DiscontinueAllProductsForSupplier(supplierID)`-Methode aufrufen, indem wir die `SupplierID` der ausgewählten FormView übergeben:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

Beachten Sie, dass der Zugriff auf die `SupplierID` des aktuellen ausgewählten Lieferanten in der FormView mithilfe der [`SelectedValue`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)von FormView möglich ist. Die `SelectedValue`-Eigenschaft gibt den ersten Datenschlüssel Wert für den Datensatz zurück, der in der FormView angezeigt wird. Die [`DataKeyNames`-Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)von FormView, die die Datenfelder angibt, aus denen die Datenschlüssel Werte abgerufen werden, wurde automatisch auf `SupplierID` von Visual Studio festgelegt, wenn die ObjectDataSource an die FormView zurück in Schritt 2 gebunden wird.

Wenn der `ItemCommand`-Ereignishandler erstellt wurde, nehmen Sie sich einen Moment Zeit, um die Seite zu testen. Navigieren Sie zum Lieferanten von "Cooperativa de Quesos" las Cabras "(der fünfte Lieferant in der FormView für mich). Dieser Lieferant stellt zwei Produkte bereit: Queso Cabrales und Queso mandego La Pastora, die beide *nicht* eingestellt werden.

Stellen Sie sich vor, dass kooperative ATIVA de Quesos "las Cabras" aus dem Unternehmen entfernt wurde und daher die Produkte eingestellt werden. Klicken Sie auf die Schaltfläche Alle Produkte einstellen. Dadurch wird das Dialogfeld Client seitiges bestätigen angezeigt (siehe Abbildung 16).

[![Cooperativa de Quesos las Cabras liefert zwei aktive Produkte](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**Abbildung 16**: Kooperativa de Quesos las Cabras stellt zwei aktive Produkte bereit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))

Wenn Sie im Dialogfeld Client seitige Bestätigung auf OK klicken, wird die Übermittlung des Formulars fortgesetzt. Dies führt zu einem Postback, bei dem das `ItemCommand` Ereignis von FormView ausgelöst wird. Der von uns erstellte Ereignishandler wird dann ausgeführt, wobei die `DiscontinueAllProductsForSupplier(supplierID)`-Methode aufgerufen und sowohl die Queso-Cabrales-als auch die Queso-Klasse "Pastora" nicht mehr fortgesetzt werden.

Wenn Sie den Ansichts Zustand der GridView deaktiviert haben, wird die GridView bei jedem Postback an den zugrunde liegenden Datenspeicher zurückgegeben und wird daher sofort aktualisiert, um anzuzeigen, dass diese beiden Produkte nun eingestellt werden (siehe Abbildung 17). Wenn Sie jedoch den Ansichts Zustand in der GridView nicht deaktiviert haben, müssen Sie die Daten nach dem vornehmen der Änderung manuell an die GridView-Ansicht binden. Um dies zu erreichen, rufen Sie einfach direkt nach dem Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)`-Methode die `DataBind()` Methode der GridView auf.

[![nach dem Klicken auf die Schaltfläche Alle Produkte einstellen werden die Produkte der Lieferanten Produkte entsprechend aktualisiert.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**Abbildung 17**: nach dem Klicken auf die Schaltfläche Alle Produkte einstellen werden die Produkte des Lieferanten entsprechend aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>Schritt 6: Erstellen einer UpdateProduct-Überladung in der Geschäftslogik Schicht zum Anpassen des Preises eines Produkts

Wie bei der Schaltfläche Alle Produkte einstellen in FormView müssen Sie zum Hinzufügen von Schaltflächen zum erhöhen und verringern des Preises für ein Produkt in der GridView zunächst die entsprechenden Methoden für die Datenzugriffs Ebene und die Geschäftslogik Schicht hinzufügen. Da wir bereits über eine Methode verfügen, die eine einzelne Produkt Zeile in der dal aktualisiert, können wir eine solche Funktionalität bereitstellen, indem wir eine neue Überladung für die `UpdateProduct`-Methode in der BLL erstellen.

Unsere bisherigen `UpdateProduct` Überladungen haben einige Kombinationen von Produkt Feldern als skalare Eingabewerte übernommen und dann nur die Felder für das angegebene Produkt aktualisiert. Bei dieser Überladung variieren wir geringfügig von diesem Standard und übergeben stattdessen den `ProductID` des Produkts und den Prozentsatz, um den die `UnitPrice` angepasst werden soll (im Gegensatz zur Übergabe der neuen, angepassten `UnitPrice` selbst). Dieser Ansatz vereinfacht den Code, den wir in der Code Behind-Klasse der ASP.NET-Seite schreiben müssen, da wir uns nicht mit der Bestimmung der `UnitPrice`des aktuellen Produkts beschäftigen müssen.

Die `UpdateProduct` Überladung für dieses Tutorial wird unten dargestellt:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

Diese Überladung ruft Informationen über das angegebene Produkt über die `GetProductByProductID(productID)` Methode der DAL ab. Anschließend wird überprüft, ob dem `UnitPrice` des Produkts ein Daten Bank `NULL` Wert zugewiesen wird. Wenn dies der Fall ist, bleibt der Preis unverändert. Wenn jedoch ein nicht`NULL` `UnitPrice` Wert vorhanden ist, aktualisiert die-Methode die `UnitPrice` des Produkts um den angegebenen Prozentsatz (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Schritt 7: Hinzufügen der Schaltflächen "vergrößern" und "verringern" zur GridView

Die GridView (und DetailsView) bestehen aus einer Auflistung von Feldern. Zusätzlich zu boundfields, checkboxfields und templatefields enthält ASP.NET das Button Field, das, wie der Name schon sagt, als Spalte mit einer Schaltfläche, einem LinkButton oder einem ImageButton für jede Zeile gerendert wird. Ähnlich wie bei der FormView führt das Klicken auf eine *beliebige* Schaltfläche in den GridView-Pagingschaltflächen, bearbeiten oder löschen, Schaltflächen usw. zu einem Postback und löst das [`RowCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)der GridView aus.

Das ButtonField-Objekt verfügt über eine `CommandName`-Eigenschaft, die den angegebenen Wert den einzelnen Schaltflächen `CommandName` Eigenschaften zuweist. Wie bei der FormView wird der `CommandName` Wert vom `RowCommand`-Ereignishandler verwendet, um zu bestimmen, auf welche Schaltfläche geklickt wurde.

Fügen wir der GridView zwei neue buttonfields hinzu, eine mit einem Schaltflächen-textpreis + 10% und die andere mit dem Text Price-10%. Wenn Sie diese Schaltflächen hinzufügen möchten, klicken Sie auf den Link Spalten bearbeiten im Smarttagmenü der GridView, wählen Sie in der Liste in der oberen linken Ecke den Feldtyp ButtonField aus, und klicken Sie auf die Schaltfläche hinzufügen.

![Fügen Sie der GridView zwei buttonfields-Felder hinzu.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**Abbildung 18**: Hinzufügen von zwei buttonfields zur GridView

Verschieben Sie die beiden buttonfields, sodass Sie als die ersten beiden GridView-Felder angezeigt werden. Legen Sie als nächstes die `Text` Eigenschaften dieser beiden Button Fields auf Price + 10% und Price-10% fest, und legen Sie die `CommandName`-Eigenschaften auf "Erhöhung Price" bzw. "dekreaseprice" fest. Standardmäßig rendert ein ButtonField seine Spalte mit Schaltflächen als LinkButtons. Dies kann jedoch über die [`ButtonType`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)des ButtonField geändert werden. Lassen Sie diese beiden Schaltflächen als reguläre pushschaltflächen gerendert. Legen Sie daher die `ButtonType`-Eigenschaft auf `Button`fest. Abbildung 19 zeigt das Dialogfeld "Felder", nachdem diese Änderungen vorgenommen wurden. Im folgenden wird das deklarative Markup der GridView angezeigt.

![Konfigurieren der Eigenschaften Button Fields Text, CommandName und ButtonType](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**Abbildung 19**: Konfigurieren der Eigenschaften Button Fields `Text`, `CommandName`und `ButtonType`

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

Wenn diese buttonfields erstellt wurden, ist der letzte Schritt das Erstellen eines Ereignis Handlers für das `RowCommand` Ereignis der GridView. Wenn Sie auf die Schaltflächen "Preis + 10%" oder "Preis-10%" geklickt haben, muss dieser Ereignishandler die `ProductID` für die Zeile ermitteln, auf deren Schaltfläche geklickt wurde, und dann die `UpdateProduct` Methode der `ProductsBLL` Klasse aufrufen und dabei die geeignete `UnitPrice` Prozentsatz Anpassung zusammen mit der `ProductID`übergeben. Der folgende Code führt diese Aufgaben aus:

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

Um die `ProductID` für die Zeile zu ermitteln, deren Preis + 10% oder die Schaltfläche Price-10% geklickt wurde, müssen wir die `DataKeys` Auflistung der GridView-Auflistung überprüfen. Diese Auflistung enthält die Werte der Felder, die in der `DataKeyNames`-Eigenschaft für jede GridView-Zeile angegeben werden. Da die `DataKeyNames`-Eigenschaft der GridView bei der Bindung von ObjectDataSource an die GridView von Visual Studio auf ProductID festgelegt wurde, stellt `DataKeys(rowIndex).Value` den `ProductID` für den angegebenen *rowIndex*bereit.

Das ButtonField übergibt automatisch den *rowIndex* der Zeile, deren Schaltfläche durch den `e.CommandArgument`-Parameter geklickt wurde. Zum Ermitteln der `ProductID` für die Zeile, auf die die Schaltfläche Price + 10% oder Price-10% geklickt wurde, wird daher: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`verwendet.

Wenn Sie den Ansichts Zustand der GridView deaktiviert haben, wird wie bei der Schaltfläche Alle Produkte einstellen die GridView-Ansicht auf den zugrunde liegenden Datenspeicher bei jedem Postback zurückgeführt und daher sofort aktualisiert, um eine Preisänderung zu berücksichtigen, die durch Klicken auf eine der Schaltflächen. Wenn Sie jedoch den Ansichts Zustand in der GridView nicht deaktiviert haben, müssen Sie die Daten nach dem vornehmen der Änderung manuell an die GridView-Ansicht binden. Um dies zu erreichen, rufen Sie einfach direkt nach dem Aufrufen der `UpdateProduct`-Methode die `DataBind()` Methode der GridView auf.

Abbildung 20 zeigt die Seite, wenn Sie die Produkte anzeigen, die von Oma Kelly Homestead bereitgestellt werden. In Abbildung 21 werden die Ergebnisse angezeigt, nachdem auf die Schaltfläche Price + 10% zweimal geklickt wurde, um die boysenberry-Verteilung von Oma und die Schaltfläche Price-10% einmal für Northwoods Cranberry-Sauce anzuzeigen.

[![die GridView-Schaltflächen Preis + 10% und Preis-10% enthält.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**Abbildung 20**: GridView enthält die Schaltflächen Price + 10% und Price-10% ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))

[![die Preise für das erste und dritte Produkt über die Schaltflächen Price + 10% und Price-10% aktualisiert.](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**Abbildung 21**: die Preise für das erste und dritte Produkt wurden über die Schaltflächen Price + 10% und Price-10% aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png)).

> [!NOTE]
> Der GridView (und DetailsView) können auch Schaltflächen, LinkButtons oder imagebuttons hinzugefügt werden, die zu ihren templatefields-Vorlagen hinzugefügt wurden. Wie bei dem BoundField auslösen diese Schaltflächen, wenn darauf geklickt wird, ein Postback, wodurch das `RowCommand` Ereignis der GridView ausgelöst wird. Beim Hinzufügen von Schaltflächen in einem TemplateField wird jedoch die `CommandArgument` der Schaltfläche nicht automatisch auf den Index der Zeile festgelegt, wie es bei Verwendung von Button Fields der Fall ist. Wenn Sie den Zeilen Index der Schaltfläche ermitteln müssen, auf die innerhalb des `RowCommand` Ereignis Handlers geklickt wurde, müssen Sie die `CommandArgument`-Eigenschaft der Schaltfläche in der deklarativen Syntax in TemplateField manuell mit dem folgenden Code festlegen:  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`.

## <a name="summary"></a>Summary

Die Steuerelemente GridView, DetailsView und FormView können alle Schaltflächen, LinkButtons oder imagebuttons enthalten. Wenn Sie auf diese Schaltflächen klicken, wird ein Postback ausgelöst und das `ItemCommand`-Ereignis in den Steuerelementen FormView und DetailsView sowie das `RowCommand`-Ereignis in der GridView ausgelöst. Diese datenweb Steuerelemente verfügen über integrierte Funktionen zum Verarbeiten allgemeiner Befehls bezogener Aktionen, wie z. b. das Löschen oder Bearbeiten von Datensätzen. Wir können jedoch auch Schaltflächen verwenden, die beim Klicken auf die Ausführung unseres eigenen benutzerdefinierten Codes reagieren.

Um dies zu erreichen, müssen wir einen Ereignishandler für das `ItemCommand`-oder `RowCommand`-Ereignis erstellen. In diesem Ereignishandler überprüfen wir zuerst den Wert eingehender `CommandName`, um zu bestimmen, auf welche Schaltfläche geklickt wurde, und dann die entsprechende benutzerdefinierte Aktion auszuführen. In diesem Tutorial haben Sie erfahren, wie Sie mithilfe von Schaltflächen und buttonfields alle Produkte für einen bestimmten Lieferanten einstellen oder den Preis eines bestimmten Produkts um 10% erhöhen oder verringern.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](adding-and-responding-to-buttons-to-a-gridview-vb.md)
