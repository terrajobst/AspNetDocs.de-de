---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Hinzufügen und reagieren auf Schaltflächen zu einem GridView-(VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Schaltflächen, sowohl einer Vorlage als auch den Feldern eines GridView-Steuer Elements oder DetailsView-Steuer Elements hinzufügen. Wir werden uns vor allem
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8727d8faead02340d223c75845bf29f63d1a0834
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78442275"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Hinzufügen von Schaltflächen zu einem GridView-Steuerelement und Zuweisen von Funktionen für diese Schaltflächen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) oder [PDF herunterladen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Schaltflächen, sowohl einer Vorlage als auch den Feldern eines GridView-Steuer Elements oder DetailsView-Steuer Elements hinzufügen. Insbesondere erstellen wir eine Schnittstelle mit einer FormView, mit der der Benutzer die Seite durch die Lieferanten durchlaufen kann.

## <a name="introduction"></a>Einführung

Obwohl viele Berichterstattungs Szenarien schreibgeschützten Zugriff auf die Berichtsdaten beinhalten, ist es für Berichte nicht ungewöhnlich, dass Sie auf der Grundlage der angezeigten Daten Aktionen ausführen können. In der Regel umfasste dies das Hinzufügen eines Schaltflächen-, LinkButton-oder ImageButton-websteuer Elements mit jedem Datensatz, der im Bericht angezeigt wird. Wenn Sie darauf klicken, wird ein Postback ausgelöst und ein serverseitiger Code aufgerufen. Das häufigste Beispiel ist das Bearbeiten und Löschen der Daten auf Datensatz-Basis. Wie wir bereits mit der [Übersicht über das Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Lernprogrammen begonnen haben, ist das Bearbeiten und löschen so üblich, dass die Steuerelemente GridView, DetailsView und FormView diese Funktionalität unterstützen können, ohne dass eine einzige Codezeile geschrieben werden muss.

Zusätzlich zu den Schaltflächen Bearbeiten und löschen können auch das GridView-Steuerelement, das DetailsView-Steuerelement und das FormView-Steuerelement Schaltflächen, LinkButtons oder imagebuttons enthalten, die beim Klicken auf eine benutzerdefinierte serverseitige Logik durchgeführt werden. In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Schaltflächen, sowohl einer Vorlage als auch den Feldern eines GridView-Steuer Elements oder DetailsView-Steuer Elements hinzufügen. Insbesondere erstellen wir eine Schnittstelle mit einer FormView, mit der der Benutzer die Seite durch die Lieferanten durchlaufen kann. Für einen bestimmten Lieferanten zeigt die FormView Informationen über den Lieferanten zusammen mit einem Schaltflächen-websteuer Element an, das, wenn darauf geklickt wird, alle zugehörigen Produkte als nicht mehr unterstützt markiert. Außerdem listet eine GridView die Produkte auf, die vom ausgewählten Lieferanten bereitgestellt werden, wobei jede Zeile die Schaltflächen Preis Preis und Preisnachlass Preis enthält, die, wenn Sie darauf geklickt haben, das Produkt s `UnitPrice` um 10% erhöhen oder verringern (siehe Abbildung 1).

[![der FormView und GridView Schaltflächen enthalten, die benutzerdefinierte Aktionen ausführen.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Abbildung 1**: die Form View und die GridView enthalten Schaltflächen, die benutzerdefinierte Aktionen ausführen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Schaltflächen-tutorialwebseiten

Bevor wir uns ansehen, wie Sie benutzerdefinierte Schaltflächen hinzufügen, nehmen Sie sich zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen. Fügen Sie zunächst einen neuen Ordner mit dem Namen `CustomButtons`hinzu. Fügen Sie dann die folgenden beiden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `CustomButtons.aspx`

![Hinzufügen der ASP.NET-Seiten für benutzerdefinierte Schaltflächen bezogene Tutorials](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen der ASP.NET-Seiten für benutzerdefinierte Schaltflächen bezogene Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `CustomButtons` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Abbildung 3**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))

Fügen Sie abschließend die Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem `<siteMapNode>`Paging und Sortieren das folgende Markup hinzu:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials zum Bearbeiten, einfügen und löschen.

![Die Site Übersicht enthält jetzt den Eintrag für das Tutorial benutzerdefinierte Schaltflächen.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Abbildung 4**: die Site Übersicht enthält jetzt den Eintrag für das Tutorial zu benutzerdefinierten Schaltflächen.

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Schritt 2: Hinzufügen einer FormView, die die Lieferanten auflistet

Beginnen Sie mit diesem Tutorial, indem Sie die FormView hinzufügen, die die Lieferanten auflistet. Wie in der Einführung erläutert, ermöglicht diese FormView dem Benutzer das Durchlaufen der Lieferanten und zeigt die Produkte an, die vom Lieferanten in einer GridView bereitgestellt werden. Außerdem enthält diese FormView eine Schaltfläche, mit der alle Lieferanten Produkte als nicht mehr unterstützt gekennzeichnet werden, wenn darauf geklickt wird. Bevor wir uns mit dem Hinzufügen der benutzerdefinierten Schaltfläche zu "FormView" befassen, erstellen Sie zuerst die FormView, sodass die Lieferanteninformationen angezeigt werden.

Öffnen Sie zunächst die Seite `CustomButtons.aspx` im Ordner `CustomButtons`. Fügen Sie der Seite eine FormView hinzu, indem Sie Sie aus der Toolbox auf den Designer ziehen und deren `ID`-Eigenschaft auf `Suppliers`festlegen. Wählen Sie aus dem FormView s-Smarttag aus, dass Sie eine neue ObjectDataSource mit dem Namen `SuppliersDataSource`erstellen möchten.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "suppliersdatasource".](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource mit dem Namen "`SuppliersDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))

Konfigurieren Sie diese neue ObjectDataSource so, dass Sie von der `SuppliersBLL` Klasse `GetSuppliers()`-Methode abgefragt wird (siehe Abbildung 6). Da diese FormView keine Schnittstelle zum Aktualisieren der Lieferanteninformationen bereitstellt, wählen Sie in der Dropdown Liste auf der Registerkarte aktualisieren die Option (keine) aus.

[![die Datenquelle so konfigurieren, dass die suppliersbll Class s getsuppliers ()-Methode verwendet wird.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Abbildung 6**: Konfigurieren der Datenquelle für die Verwendung der `GetSuppliers()` Methode der `SuppliersBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))

Nach dem Konfigurieren von ObjectDataSource generiert Visual Studio eine `InsertItemTemplate`, `EditItemTemplate`und `ItemTemplate` für die Form View. Entfernen Sie die `InsertItemTemplate`, und `EditItemTemplate` und ändern Sie die `ItemTemplate`, sodass nur der Firmenname und die Telefonnummer des Lieferanten angezeigt werden. Aktivieren Sie schließlich die Paging-Unterstützung für die FormView, indem Sie das Kontrollkästchen Paging aktivieren des Smarttags aktivieren (oder indem Sie die `AllowPaging`-Eigenschaft auf `True`festlegen). Nachdem diese Änderungen vorgenommen wurden, sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Abbildung 7 zeigt die Seite custombuttons. aspx, wenn Sie in einem Browser angezeigt wird.

[![FormView listet die Felder "CompanyName" und "Phone" des aktuell ausgewählten Lieferanten auf.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Abbildung 7**: FormView listet die Felder "`CompanyName`" und "`Phone`" des aktuell ausgewählten Lieferanten auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Schritt 3: Hinzufügen einer GridView, in der die Produkte des ausgewählten Lieferanten aufgeführt sind

Bevor wir der FormView s-Vorlage die Schaltfläche Alle Produkte einstellen hinzufügen, können Sie zuerst eine GridView unter der Form Ansicht hinzufügen, in der die Produkte des ausgewählten Lieferanten aufgeführt sind. Fügen Sie hierzu der Seite eine GridView hinzu, legen Sie die `ID`-Eigenschaft auf `SuppliersProducts`fest, und fügen Sie eine neue ObjectDataSource mit dem Namen `SuppliersProductsDataSource`hinzu.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "suppliersproduczdatasource".](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Abbildung 8**: Erstellen einer neuen ObjectDataSource mit dem Namen "`SuppliersProductsDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))

Konfigurieren Sie diese ObjectDataSource für die Verwendung der productbll Class s `GetProductsBySupplierID(supplierID)`-Methode (siehe Abbildung 9). Obwohl diese GridView den Preis eines Product s anpassen kann, werden die integrierten Funktionen zum Bearbeiten oder Löschen von GridView nicht verwendet. Daher können wir die Dropdown Liste für die Registerkarten Update, INSERT und DELETE von ObjectDataSource auf (keine) festlegen.

[![konfigurieren Sie die Datenquelle für die Verwendung der Methode getproductbysupplierid (SupplierID) der productbll-Klasse.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Abbildung 9**: Konfigurieren der Datenquelle für die Verwendung der `GetProductsBySupplierID(supplierID)` Methode der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))

Da die `GetProductsBySupplierID(supplierID)`-Methode einen Eingabeparameter akzeptiert, fordert der ObjectDataSource-Assistent die Quelle dieses Parameter Werts an. Um den `SupplierID` Wert aus der Form Ansicht zu übergeben, legen Sie die Dropdown Liste Parameter Quelle auf Control und die ControlID-Dropdown Liste auf `Suppliers` (die ID der in Schritt 2 erstellten Form Ansicht) fest.

[![angeben, dass der SupplierID-Parameter vom Suppliers-FormView-Steuerelement stammen soll.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Abbildung 10**: Geben Sie an, dass der *`supplierID`* Parameter aus dem `Suppliers` FormView-Steuerelement stammen soll ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png)).

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, enthält das GridView-Objekt ein BoundField-oder CheckBoxField-Objekt für jedes der Product s-Datenfelder. Lassen Sie diese nach unten kürzen, um nur die `ProductName` und `UnitPrice` boundfields zusammen mit dem `Discontinued` CheckBoxField anzuzeigen. Außerdem können Sie das `UnitPrice` BoundField so formatieren, dass sein Text als Währung formatiert ist. Das deklarative Markup von GridView und `SuppliersProductsDataSource` ObjectDataSource s sollte in etwa wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

An dieser Stelle zeigt unser Tutorial einen Master-/Detailbericht an, der es dem Benutzer ermöglicht, einen Lieferanten aus der FormView oben auszuwählen und die Produkte anzuzeigen, die von diesem Lieferanten über die GridView im unteren Bereich bereitgestellt werden. Abbildung 11 zeigt einen Screenshot dieser Seite, wenn Sie den Tokyo Traders-Lieferanten aus der FormView auswählen.

[![die ausgewählten Lieferanten Produkte in der GridView angezeigt werden.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Abbildung 11**: die ausgewählten Lieferanten Produkte werden in der GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Schritt 4: Erstellen von Dal-und BLL-Methoden, um alle Produkte für einen Lieferanten einzustellen

Bevor wir der FormView eine Schaltfläche hinzufügen können, die alle Lieferanten Produkte entfernt, wenn darauf geklickt wird, müssen wir der dal und der BLL, die diese Aktion ausführt, zunächst eine Methode hinzufügen. Insbesondere wird diese Methode mit dem Namen `DiscontinueAllProductsForSupplier(supplierID)`benannt. Beim Klicken auf die Schaltfläche FormView s rufen wir diese Methode in der Geschäftslogik Schicht auf und übergeben dabei den ausgewählten Lieferanten s `SupplierID`; die BLL ruft dann die entsprechende Datenzugriffs Schicht-Methode auf, die eine `UPDATE`-Anweisung für die Datenbank ausgibt, die die angegebenen Lieferanten Produkte nicht mehr fortsetzt.

Wie in den vorherigen Tutorials beschrieben, verwenden wir einen Bottom-up-Ansatz, beginnend mit dem Erstellen der dal-Methode, der BLL-Methode und schließlich der Implementierung der Funktionalität auf der ASP.NET-Seite. Öffnen Sie das `Northwind.xsd` typisierte DataSet im Ordner `App_Code/DAL`, und fügen Sie dem `ProductsTableAdapter` eine neue Methode hinzu (Klicken Sie mit der rechten Maustaste auf das `ProductsTableAdapter`, und wählen Sie Abfrage hinzufügen). Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen angezeigt, der uns durch den Prozess des Hinzufügens der neuen Methode führt. Beginnen Sie mit der Angabe, dass unsere dal-Methode eine Ad-hoc-SQL-Anweisung verwendet.

[![Erstellen der dal-Methode mithilfe einer Ad-hoc-SQL-Anweisung](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Abbildung 12**: Erstellen der dal-Methode mithilfe einer Ad-hoc-SQL-Anweisung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))

Im nächsten Schritt werden wir vom Assistenten aufgefordert, den Typ der zu erstellenden Abfrage zu erstellen. Da die `DiscontinueAllProductsForSupplier(supplierID)`-Methode die `Products` Datenbanktabelle aktualisieren muss, muss das `Discontinued` Feld für alle Produkte, die vom angegebenen *`supplierID`* bereitgestellt werden, auf 1 festgelegt werden. es muss eine Abfrage erstellt werden, mit der Daten aktualisiert werden.

[![wählen Sie den Typ der Update Abfrage aus.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Abbildung 13**: Auswählen des Abfragetyps "Update" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))

Der nächste Assistent zeigt den TableAdapter s-`UPDATE`-Anweisung an, die jedes der Felder aktualisiert, die in der `Products` Datentabelle definiert sind. Ersetzen Sie diesen Abfragetext durch die folgende Anweisung:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Nachdem Sie diese Abfrage eingegeben und auf "weiter" geklickt haben, werden Sie auf der letzten Seite des Assistenten aufgefordert, den Namen der neuen Methode `DiscontinueAllProductsForSupplier`. Beenden Sie den Assistenten durch Klicken auf die Schaltfläche Fertigstellen. Wenn Sie zum DataSet-Designer zurückkehren, sollte in der `ProductsTableAdapter` mit dem Namen `DiscontinueAllProductsForSupplier(@SupplierID)`eine neue Methode angezeigt werden.

[![den Namen der neuen dal-Methode discontinueallproductforsupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Abbildung 14**: Benennen der neuen dal-Methode `DiscontinueAllProductsForSupplier` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))

Wenn die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der Datenzugriffs Ebene erstellt wurde, besteht die nächste Aufgabe darin, die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der Geschäftslogik Ebene zu erstellen. Öffnen Sie hierzu die Datei `ProductsBLL`-Klasse, und fügen Sie Folgendes hinzu:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Diese Methode ruft einfach die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der DAL auf und übergibt den bereitgestellten *`supplierID`* Parameterwert. Wenn Geschäftsregeln vorhanden waren, die nur unter bestimmten Umständen das Unternehmen der Produkte eines Lieferanten waren, sollten diese Regeln hier in der BLL implementiert werden.

> [!NOTE]
> Im Gegensatz zu den `UpdateProduct` Überladungen in der `ProductsBLL`-Klasse enthält die Signatur der `DiscontinueAllProductsForSupplier(supplierID)`-Methode das `DataObjectMethodAttribute`-Attribut (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`) nicht. Dadurch wird die `DiscontinueAllProductsForSupplier(supplierID)`-Methode aus der Dropdown Liste Datenquellen-Assistent für Datenquellen konfigurieren von ObjectDataSource auf der Registerkarte Update ausgeschlossen. Dieses Attribut wurde ausgelassen, da wir die `DiscontinueAllProductsForSupplier(supplierID)` Methode direkt von einem Ereignishandler auf unserer ASP.NET-Seite aufrufen.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Schritt 5: Hinzufügen einer Schaltfläche "alle Produkte" zu "FormView"

Wenn die `DiscontinueAllProductsForSupplier(supplierID)`-Methode in der BLL und Dal abgeschlossen ist, besteht der letzte Schritt zum Hinzufügen der Möglichkeit, alle Produkte für den ausgewählten Lieferanten einzustellen, das Hinzufügen eines Schaltflächen-websteuer Elements zum FormView s-`ItemTemplate`. Fügen Sie mit dem Schaltflächen Text unter der Telefonnummer des Lieferanten eine solche Schaltfläche hinzu, und setzen Sie alle Produkte und einen `ID`-Eigenschafts Wert `DiscontinueAllProductsForSupplier`. Sie können dieses Schaltflächen-websteuer Element über den Designer hinzufügen, indem Sie auf den Link Vorlagen bearbeiten im Smarttag der FormView s (siehe Abbildung 15) oder direkt über die deklarative Syntax klicken.

[![das Schaltflächen-websteuer Element "alle Produkte" für "FormView s ItemTemplate" hinzufügen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Abbildung 15**: Hinzufügen eines websteuer Elements mit der Schaltfläche "alle Produkte" in der Form View s `ItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))

Wenn ein Benutzer auf die Schaltfläche klickt, wird ein Postback und das Formular View s [`ItemCommand`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) ausgelöst. Zum Ausführen von benutzerdefiniertem Code als Antwort auf die Schaltfläche, auf die geklickt wird, können wir einen Ereignishandler für dieses Ereignis erstellen. Beachten Sie jedoch, dass das `ItemCommand`-Ereignis ausgelöst wird, wenn in der FormView auf eine Schaltfläche, ein LinkButton oder *ein* ImageButton-websteuer Element geklickt wird. Dies bedeutet, dass das `ItemCommand` Ereignis ausgelöst wird, wenn der Benutzer in der Form Ansicht von einer Seite zu einem anderen wechselt. Dies ist der Fall, wenn der Benutzer in einer FormView, die das Einfügen, aktualisieren oder löschen unterstützt, auf neu, bearbeiten oder Löschen klickt.

Da der `ItemCommand` unabhängig von der Schaltfläche, auf die geklickt wird, ausgelöst wird, benötigen wir im Ereignishandler eine Methode, um zu bestimmen, ob auf die Schaltfläche Alle Produkte Abbrechen geklickt wurde oder ob es sich um eine andere Schaltfläche handelt Um dies zu erreichen, können wir das Schaltflächen-websteuer Element s `CommandName`-Eigenschaft auf einen identifizierenden Wert festlegen. Wenn auf die Schaltfläche geklickt wird, wird dieser `CommandName` Wert an den `ItemCommand`-Ereignishandler übermittelt, sodass wir bestimmen können, ob auf die Schaltfläche Alle Produkte einstellen geklickt wurde. Legen Sie die Schaltfläche Alle Produkte einstellen `CommandName` Eigenschaft auf discontinueproducts fest.

Verwenden Sie abschließend ein Client seitiges Dialogfeld bestätigen, um sicherzustellen, dass der Benutzer die Produkte des ausgewählten Lieferanten wirklich einstellen möchte. Wie im Lernprogramm zum [Hinzufügen von Client seitiger Bestätigung beim Löschen](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) gesehen, kann dies mit etwas JavaScript erreicht werden. Legen Sie insbesondere die Eigenschaft OnClientClick des Schaltflächen-websteuer Elements auf `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Nachdem Sie diese Änderungen vorgenommen haben, sollte die deklarative Syntax von FormView s wie folgt aussehen:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Erstellen Sie als nächstes einen Ereignishandler für das Ereignis "FormView s `ItemCommand`". In diesem Ereignishandler muss zuerst festgestellt werden, ob auf die Schaltfläche Alle Produkte Abbrechen geklickt wurde. Wenn dies der Fall ist, möchten wir eine Instanz der `ProductsBLL` Klasse erstellen und ihre `DiscontinueAllProductsForSupplier(supplierID)`-Methode aufrufen, indem wir die `SupplierID` der ausgewählten FormView übergeben:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Beachten Sie, dass der Zugriff auf die `SupplierID` des aktuellen ausgewählten Lieferanten in FormView mithilfe der FormView s [`SelectedValue`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)möglich ist. Die `SelectedValue`-Eigenschaft gibt den ersten Datenschlüssel Wert für den Datensatz zurück, der in der FormView angezeigt wird. Die FormView s [`DataKeyNames`-Eigenschaft](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), die die Datenfelder angibt, aus denen die Datenschlüssel Werte abgerufen werden, wurde automatisch auf `SupplierID` von Visual Studio festgelegt, wenn die ObjectDataSource an die FormView zurück in Schritt 2 gebunden wird.

Wenn der `ItemCommand`-Ereignishandler erstellt wurde, nehmen Sie sich einen Moment Zeit, um die Seite zu testen. Navigieren Sie zum Lieferanten von "Cooperativa de Quesos" las Cabras "(es ist der fünfte Lieferant in der FormView für mich). Dieser Lieferant stellt zwei Produkte bereit: Queso Cabrales und Queso mandego La Pastora, die beide *nicht* eingestellt werden.

Stellen Sie sich vor, dass kooperative ATIVA de Quesos "las Cabras" aus dem Unternehmen entfernt wurde und daher die Produkte eingestellt werden. Klicken Sie auf die Schaltfläche Alle Produkte einstellen. Dadurch wird das Dialogfeld Client seitiges bestätigen angezeigt (siehe Abbildung 16).

[![Cooperativa de Quesos las Cabras liefert zwei aktive Produkte](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Abbildung 16**: Kooperativa de Quesos las Cabras stellt zwei aktive Produkte bereit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))

Wenn Sie im Dialogfeld Client seitige Bestätigung auf OK klicken, wird die Übermittlung des Formulars fortgesetzt. Dies führt zu einem Postback, bei dem das FormView s `ItemCommand`-Ereignis ausgelöst wird. Der von uns erstellte Ereignishandler wird dann ausgeführt, wobei die `DiscontinueAllProductsForSupplier(supplierID)`-Methode aufgerufen und sowohl die Queso-Cabrales-als auch die Queso-Klasse "Pastora" nicht mehr fortgesetzt werden.

Wenn Sie den Ansichts Zustand von GridView deaktiviert haben, wird die GridView bei jedem Postback an den zugrunde liegenden Datenspeicher zurückgegeben und wird daher sofort aktualisiert, um anzuzeigen, dass diese beiden Produkte nun eingestellt werden (siehe Abbildung 17). Wenn Sie jedoch den Ansichts Zustand in der GridView nicht deaktiviert haben, müssen Sie die Daten nach dem vornehmen der Änderung manuell an die GridView-Ansicht binden. Um dies zu erreichen, führen Sie einfach nach dem Aufrufen der `DiscontinueAllProductsForSupplier(supplierID)`-Methode einen Aufruf an die GridView s `DataBind()`-Methode aus.

[![nach dem Klicken auf die Schaltfläche Alle Produkte einstellen werden die Produkte der Lieferanten Produkte entsprechend aktualisiert.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Abbildung 17**: nach dem Klicken auf die Schaltfläche Alle Produkte einstellen werden die Produkte der Lieferanten Produkte entsprechend aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Schritt 6: Erstellen einer UpdateProduct-Überladung in der Geschäftslogik Schicht zum Anpassen eines Product s Preises

Wie bei der Schaltfläche Alle Produkte einstellen in FormView müssen Sie zum Hinzufügen von Schaltflächen zum erhöhen und verringern des Preises für ein Produkt in der GridView zunächst die entsprechenden Methoden für die Datenzugriffs Ebene und die Geschäftslogik Schicht hinzufügen. Da wir bereits über eine Methode verfügen, die eine einzelne Produkt Zeile in der dal aktualisiert, können wir eine solche Funktionalität bereitstellen, indem wir eine neue Überladung für die `UpdateProduct`-Methode in der BLL erstellen.

Unsere bisherigen `UpdateProduct` Überladungen haben einige Kombinationen von Produkt Feldern als skalare Eingabewerte übernommen und dann nur die Felder für das angegebene Produkt aktualisiert. Bei dieser Überladung variieren wir geringfügig von diesem Standard und übergeben stattdessen die Product s `ProductID` und den Prozentsatz, um den die `UnitPrice` angepasst werden soll (im Gegensatz zur Übergabe der neuen, angepassten `UnitPrice` selbst). Diese Vorgehensweise vereinfacht den Code, den wir in der Code Behind-Klasse der ASP.NET-Seite schreiben müssen, da wir uns nicht mit der Bestimmung der aktuellen Product s `UnitPrice`beschäftigen müssen.

Die `UpdateProduct` Überladung für dieses Tutorial wird unten dargestellt:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Diese Überladung ruft Informationen über das angegebene Produkt über die DAL s-`GetProductByProductID(productID)`-Methode ab. Anschließend wird überprüft, ob dem Product s `UnitPrice` eine Datenbank `NULL` Wert zugewiesen wird. Wenn dies der Fall ist, bleibt der Preis unverändert. Wenn jedoch ein nicht`NULL` `UnitPrice` Wert vorhanden ist, aktualisiert die-Methode die Produkt-s `UnitPrice` um den angegebenen Prozentsatz (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Schritt 7: Hinzufügen der Schaltflächen "vergrößern" und "verringern" zur GridView

Die GridView (und DetailsView) bestehen aus einer Auflistung von Feldern. Zusätzlich zu boundfields, checkboxfields und templatefields enthält ASP.NET das Button Field, das, wie der Name schon sagt, als Spalte mit einer Schaltfläche, einem LinkButton oder einem ImageButton für jede Zeile gerendert wird. Ähnlich wie bei der FormView führt das Klicken auf eine *beliebige* Schaltfläche in den GridView-Pagingschaltflächen, das Bearbeiten oder Löschen von Schaltflächen, das Sortieren von Schaltflächen usw. zu einem Postback und löst das [`RowCommand` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)GridView s aus.

Das ButtonField-Objekt verfügt über eine `CommandName`-Eigenschaft, die den angegebenen Wert den einzelnen Schaltflächen `CommandName` Eigenschaften zuweist. Wie bei der FormView wird der `CommandName` Wert vom `RowCommand`-Ereignishandler verwendet, um zu bestimmen, auf welche Schaltfläche geklickt wurde.

Fügen Sie der GridView zwei neue Schaltflächen hinzu, eine mit einem Schaltflächen-textpreis + 10% und die andere mit dem Text Price-10%. Wenn Sie diese Schaltflächen hinzufügen möchten, klicken Sie auf den Link Spalten bearbeiten im GridView s-Smarttag, wählen Sie in der Liste in der oberen linken Ecke den Feldtyp ButtonField aus, und klicken Sie auf die Schaltfläche hinzufügen.

![Fügen Sie der GridView zwei buttonfields-Felder hinzu.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Abbildung 18**: Hinzufügen von zwei buttonfields zur GridView

Verschieben Sie die beiden buttonfields, sodass Sie als die ersten beiden GridView-Felder angezeigt werden. Legen Sie als nächstes die `Text` Eigenschaften dieser beiden Button Fields auf Price + 10% und Price-10% fest, und legen Sie die `CommandName`-Eigenschaften auf "Erhöhung Price" bzw. "dekreaseprice" fest. Standardmäßig rendert ein ButtonField seine Spalte mit Schaltflächen als LinkButtons. Dies kann jedoch über die Button Field s- [`ButtonType`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)geändert werden. Lassen Sie zu, dass diese beiden Schaltflächen als reguläre pushschaltflächen gerendert werden. Legen Sie daher die `ButtonType`-Eigenschaft auf `Button`fest. Abbildung 19 zeigt das Dialogfeld "Felder", nachdem diese Änderungen vorgenommen wurden. Im folgenden wird das deklarative Markup von GridView s angezeigt.

![Konfigurieren der Eigenschaften Button Fields Text, CommandName und ButtonType](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Abbildung 19**: Konfigurieren der Eigenschaften Button Fields `Text`, `CommandName`und `ButtonType`

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Wenn diese buttonfields erstellt wurden, ist der letzte Schritt das Erstellen eines Ereignis Handlers für das GridView s `RowCommand`-Ereignis. Wenn Sie auf die Schaltflächen "Preis + 10%" oder "Preis-10%" geklickt haben, muss dieser Ereignishandler die `ProductID` für die Zeile ermitteln, auf deren Schaltfläche geklickt wurde, und dann die `ProductsBLL` Class-`UpdateProduct`-Methode aufrufen und dabei die entsprechende `UnitPrice` Prozentsatz Anpassung zusammen mit der `ProductID`übergeben. Der folgende Code führt diese Aufgaben aus:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Um die `ProductID` für die Zeile zu ermitteln, deren Preis + 10% oder die Schaltfläche "Price-10%" angeklickt wurde, müssen wir die `DataKeys` Auflistung der GridView s. Diese Auflistung enthält die Werte der Felder, die in der `DataKeyNames`-Eigenschaft für jede GridView-Zeile angegeben werden. Da die Eigenschaft "GridView s `DataKeyNames`" von Visual Studio auf "ProductID" festgelegt wurde, wenn die ObjectDataSource an die GridView gebunden wurde, stellt `DataKeys(rowIndex).Value` den `ProductID` für den angegebenen *rowIndex*bereit.

Das ButtonField übergibt automatisch den *rowIndex* der Zeile, deren Schaltfläche durch den `e.CommandArgument`-Parameter geklickt wurde. Zum Ermitteln der `ProductID` für die Zeile, auf die die Schaltfläche Price + 10% oder Price-10% geklickt wurde, wird daher: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`verwendet.

Wenn Sie den Ansichts Zustand von GridView (alle Produkte) deaktiviert haben, wird die GridView bei jedem Postback an den zugrunde liegenden Datenspeicher zurückgegeben und daher sofort aktualisiert, um eine Preisänderung zu berücksichtigen, die durch Klicken auf eine der Schaltflächen. Wenn Sie jedoch den Ansichts Zustand in der GridView nicht deaktiviert haben, müssen Sie die Daten nach dem vornehmen der Änderung manuell an die GridView-Ansicht binden. Um dies zu erreichen, führen Sie einfach nach dem Aufrufen der `UpdateProduct`-Methode einen Aufruf an die GridView s `DataBind()`-Methode aus.

Abbildung 20 zeigt die Seite, wenn Sie die Produkte anzeigen, die von Oma Kelly Homestead bereitgestellt werden. In Abbildung 21 werden die Ergebnisse angezeigt, nachdem auf die Schaltfläche Price + 10% zweimal geklickt wurde, um die boysenberry-Verteilung von Oma und die Schaltfläche Price-10% einmal für Northwoods Cranberry-Sauce anzuzeigen.

[![die GridView-Schaltflächen Preis + 10% und Preis-10% enthält.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Abbildung 20**: GridView enthält die Schaltflächen Price + 10% und Price-10% ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))

[![die Preise für das erste und dritte Produkt über die Schaltflächen Price + 10% und Price-10% aktualisiert.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Abbildung 21**: die Preise für das erste und dritte Produkt wurden über die Schaltflächen Price + 10% und Price-10% aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png)).

> [!NOTE]
> Der GridView (und DetailsView) können auch Schaltflächen, LinkButtons oder imagebuttons hinzugefügt werden, die zu ihren templatefields-Vorlagen hinzugefügt wurden. Wie bei dem BoundField auslösen diese Schaltflächen, wenn darauf geklickt wird, ein Postback, wodurch das GridView s `RowCommand`-Ereignis ausgelöst wird. Beim Hinzufügen von Schaltflächen in einem TemplateField wird die Schaltfläche s `CommandArgument` jedoch nicht automatisch auf den Index der Zeile festgelegt, wie es bei Verwendung von Button Fields der Fall ist. Wenn Sie den Zeilen Index der Schaltfläche ermitteln müssen, auf die innerhalb des `RowCommand` Ereignis Handlers geklickt wurde, müssen Sie die Schaltfläche s `CommandArgument` Eigenschaft in der deklarativen Syntax in TemplateField manuell mithilfe von Code wie dem folgenden festlegen:  
> [https://login.microsoftonline.com/consumers/](`<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`).

## <a name="summary"></a>Zusammenfassung

Die Steuerelemente GridView, DetailsView und FormView können alle Schaltflächen, LinkButtons oder imagebuttons enthalten. Wenn Sie auf diese Schaltflächen klicken, wird ein Postback ausgelöst und das `ItemCommand`-Ereignis in den Steuerelementen FormView und DetailsView sowie das `RowCommand`-Ereignis in der GridView ausgelöst. Diese datenweb Steuerelemente verfügen über integrierte Funktionen zum Verarbeiten allgemeiner Befehls bezogener Aktionen, wie z. b. das Löschen oder Bearbeiten von Datensätzen. Wir können jedoch auch Schaltflächen verwenden, die beim Klicken auf die Ausführung unseres eigenen benutzerdefinierten Codes reagieren.

Um dies zu erreichen, müssen wir einen Ereignishandler für das `ItemCommand`-oder `RowCommand`-Ereignis erstellen. In diesem Ereignishandler überprüfen wir zuerst den Wert eingehender `CommandName`, um zu bestimmen, auf welche Schaltfläche geklickt wurde, und dann die entsprechende benutzerdefinierte Aktion auszuführen. In diesem Tutorial haben Sie erfahren, wie Sie mithilfe von Schaltflächen und buttonfields alle Produkte für einen bestimmten Lieferanten einstellen oder den Preis eines bestimmten Produkts um 10% erhöhen oder verringern.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Previous](adding-and-responding-to-buttons-to-a-gridview-cs.md)
