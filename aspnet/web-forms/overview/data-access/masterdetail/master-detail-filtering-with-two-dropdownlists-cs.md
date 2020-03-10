---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Filtern von Master-/detailfiltern mit zwei Dropdown Listen (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird die Master/Detail-Beziehung erweitert, um eine dritte Ebene hinzuzufügen. dabei werden zwei Dropdown List-Steuerelemente verwendet, um das gewünschte übergeordnete und übergeordnete übergeordnete Element zu wählen.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: e24b7f91d34fbce1676f7f28ebb7d23903157f7f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78424287"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Filtern von Master-/Detailberichten mit zwei DropDownList-Steuerelementen (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) oder [PDF herunterladen](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> In diesem Tutorial wird die Master/Detail-Beziehung erweitert, um eine dritte Ebene hinzuzufügen. dabei werden zwei Dropdown List-Steuerelemente verwendet, um die gewünschten übergeordneten und übergeordneten Datensätze

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](master-detail-filtering-with-a-dropdownlist-cs.md) wurde erläutert, wie ein einfacher Master-/Detailbericht mit einer einzelnen Dropdown List, die mit den Kategorien gefüllt ist, und einer GridView angezeigt wird, die die Produkte anzeigt, die zur ausgewählten Kategorie gehören. Dieses Berichts Muster funktioniert gut, wenn Sie Datensätze mit einer 1: n-Beziehung anzeigen und problemlos für Szenarien mit mehreren 1: n-Beziehungen erweitert werden können. Beispielsweise verfügt ein Bestell Eintrags System über Tabellen, die Kunden, Bestellungen und Bestell Positionen entsprechen. Ein bestimmter Kunde kann über mehrere Bestellungen mit jeder Bestellung verfügen, die aus mehreren Elementen besteht. Diese Daten können dem Benutzer mit zwei Dropdown Listen und einer GridView angezeigt werden. Der erste DropDownList-Eintrag enthält ein Listenelement für jeden Kunden in der Datenbank, wobei der Inhalt des zweiten Inhalts die Bestellungen des ausgewählten Kunden ist. Eine GridView listet die Zeilen Elemente aus der ausgewählten Reihenfolge auf.

Während die Northwind-Datenbank die kanonischen Kunden-/Order-/Order-detailsinformationen in den Tabellen `Customers`, `Orders`und `Order Details` enthält, werden diese Tabellen nicht in unserer Architektur aufgezeichnet. Trotzdem können wir die Verwendung von zwei abhängigen Dropdown Listen veranschaulichen. In der ersten Dropdown list-Liste werden die Kategorien und der zweite die Produkte aufgelistet, die zur ausgewählten Kategorie gehören. In einer DetailsView werden dann die Details des ausgewählten Produkts aufgelistet.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Schritt 1: Erstellen und Auffüllen der Dropdown List-Kategorien

Unser erstes Ziel besteht darin, die Dropdown Liste hinzuzufügen, die die Kategorien auflistet. Diese Schritte wurden im vorherigen Tutorial ausführlich untersucht, werden jedoch aus Gründen der Vollständigkeit hier zusammengefasst.

Öffnen Sie die Seite `MasterDetailsDetails.aspx` im Ordner `Filtering`, fügen Sie der Seite eine Dropdown Liste hinzu, legen Sie die Eigenschaft `ID` auf `Categories`fest, und klicken Sie dann im Smarttags auf den Link Datenquelle konfigurieren. Wählen Sie im Assistenten zum Konfigurieren von Datenquellen die Option zum Hinzufügen einer neuen Datenquelle aus.

[![eine neue Datenquelle für die Dropdown Liste hinzufügen.](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Datenquelle für Dropdown List ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))

Die neue Datenquelle sollte natürlich eine ObjectDataSource sein. Benennen Sie diesen neuen ObjectDataSource-`CategoriesDataSource`, und lassen Sie ihn die `GetCategories()`-Methode des `CategoriesBLL`-Objekts aufrufen.

[![wählen Sie die kategoriesbll-Klasse aus.](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Abbildung 2**: Wählen Sie die `CategoriesBLL` Klasse aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))

[![konfigurieren Sie ObjectDataSource für die Verwendung der GetCategories ()-Methode.](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Abbildung 3**: Konfigurieren von ObjectDataSource für die Verwendung der `GetCategories()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))

Nach dem Konfigurieren von ObjectDataSource müssen Sie immer noch angeben, welches Datenquellen Feld in der `Categories` Dropdown List angezeigt werden soll und welches als Wert für das Listenelement konfiguriert werden soll. Legen Sie das `CategoryName`-Feld als Anzeige fest, und `CategoryID` als Wert für jedes Listenelement.

[![Dropdown List das Feld "CategoryName" anzeigen und "CategoryID" als Wert verwenden.](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Abbildung 4**: "DropDownList" zeigt das `CategoryName` Feld an und verwendet `CategoryID` als Wert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))

An diesem Punkt verfügen wir über ein Dropdown List-Steuerelement (`Categories`), das mit den Datensätzen aus der `Categories` Tabelle aufgefüllt ist. Wenn der Benutzer eine neue Kategorie aus der Dropdown Liste auswählt, soll ein Postback erfolgen, um die Produkt-Dropdown Liste zu aktualisieren, die in Schritt 2 erstellt wird. Aktivieren Sie deshalb die Option AutoPostBack aktivieren im Smarttags `categories` DropDownList.

[Automatische ![für Dropdown List "Kategorien" aktivieren](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Abbildung 5**: Aktivieren von "AutoPostBack" für die Dropdown Liste "`Categories`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))

## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Schritt 2: Anzeigen der Produkte der ausgewählten Kategorie in einer zweiten Dropdown Liste

Wenn die Dropdown Liste `Categories` abgeschlossen ist, besteht der nächste Schritt darin, eine Dropdown Liste der Produkte anzuzeigen, die zur ausgewählten Kategorie gehören. Fügen Sie zu diesem Zweck der Seite mit dem Namen `ProductsByCategory`eine weitere Dropdown Liste hinzu. Erstellen Sie wie in der Dropdown Liste `Categories` eine neue ObjectDataSource für die `ProductsByCategory` DropDownList mit dem Namen `ProductsByCategoryDataSource`.

[![eine neue Datenquelle für die Dropdown Liste producungbycategory hinzufügen.](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Abbildung 6**: Hinzufügen einer neuen Datenquelle für das `ProductsByCategory` Dropdown List ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))

[![erstellen Sie eine neue ObjectDataSource namens productbycategorydatasource.](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Abbildung 7**: Erstellen einer neuen ObjectDataSource mit dem Namen "`ProductsByCategoryDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))

Da in der Dropdown List-`ProductsByCategory` nur die Produkte angezeigt werden müssen, die der ausgewählten Kategorie angehören, muss ObjectDataSource die `GetProductsByCategoryID(categoryID)`-Methode aus dem `ProductsBLL`-Objekt aufrufen.

[![wählen Sie die productbll-Klasse aus.](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Abbildung 8**: Auswählen der Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))

[![Konfigurieren von ObjectDataSource für die Verwendung der getproductbycategoryid (CategoryID)-Methode](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Abbildung 9**: Konfigurieren von ObjectDataSource für die Verwendung der `GetProductsByCategoryID(categoryID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))

Im letzten Schritt des Assistenten müssen wir den Wert des *`categoryID`* -Parameters angeben. Weisen Sie diesen Parameter dem ausgewählten Element aus der Dropdown Liste `Categories` zu.

[![rufen Sie den Wert für den CategoryID-Parameter aus den Kategorien DropDownList ab.](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Abbildung 10**: Abrufen des *`categoryID`* Parameter Werts aus der `Categories` Dropdown List ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))

Wenn die ObjectDataSource konfiguriert ist, müssen Sie nur noch angeben, welche Datenquellen Felder für die Anzeige und den Wert der Elemente der Dropdown List verwendet werden. Zeigen Sie das `ProductName` Feld an, und verwenden Sie das Feld `ProductID` als Wert.

[![die Datenquellen Felder angeben, die für die ListItems-Text-und Wert Eigenschaften der DropDownList verwendet werden.](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Abbildung 11**: Angeben der Datenquellen Felder, die für die Eigenschaften `Text` und `Value` von Dropdown`ListItem` List verwendet werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))

Wenn die Seite ObjectDataSource und `ProductsByCategory` Dropdown List konfiguriert ist, werden auf der Seite zwei Dropdown Listen angezeigt: im ersten werden alle Kategorien aufgeführt, während im zweiten Fall die Produkte aufgelistet werden, die zur ausgewählten Kategorie gehören. Wenn der Benutzer eine neue Kategorie aus der ersten Dropdown Liste auswählt, wird ein Postback durchführt, und die zweite Dropdown List wird erneut mit den Produkten angezeigt, die der neu ausgewählten Kategorie angehören. In den Abbildungen 12 und 13 wird `MasterDetailsDetails.aspx` in Aktion angezeigt, wenn Sie in einem Browser angezeigt wird.

[beim ersten Besuch der Seite wird die Kategorie Getränke ausgewählt. ![](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Abbildung 12**: beim ersten Besuch der Seite wird die Kategorie Getränke ausgewählt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))

[Wenn Sie ![eine andere Kategorie auswählen, werden die Produkte der neuen Kategorie angezeigt.](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Abbildung 13**: Auswählen einer anderen Kategorie zeigt die Produkte der neuen Kategorie[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))

Derzeit führt das `productsByCategory` Dropdown List, wenn es geändert wird, *nicht* zu einem Postback. Wir möchten jedoch, dass ein Postback stattfindet, sobald wir eine DetailsView hinzufügen, um die Details des ausgewählten Produkts anzuzeigen (Schritt 3). Aktivieren Sie daher das Kontrollkästchen AutoPostBack aktivieren im Smarttags `productsByCategory` DropDownList.

[![aktivieren Sie die Funktion "AutoPostBack" für die Dropdown Liste "produczbycategory".](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Abbildung 14**: Aktivieren der Funktion "AutoPostBack" für die Dropdown Liste "`productsByCategory`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))

## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Schritt 3: Verwenden von DetailsView zum Anzeigen von Details für das ausgewählte Produkt

Der letzte Schritt besteht darin, die Details für das ausgewählte Produkt in einer DetailsView anzuzeigen. Fügen Sie zu diesem Zweck der Seite eine DetailsView hinzu, legen Sie die `ID`-Eigenschaft auf `ProductDetails`fest, und erstellen Sie eine neue ObjectDataSource. Konfigurieren Sie diese ObjectDataSource, um Ihre Daten aus der `GetProductByProductID(productID)` Methode der `ProductsBLL` Klasse abzurufen, indem Sie den ausgewählten Wert des `ProductsByCategory` DropDownList für den Wert des *`productID`* -Parameters verwenden.

[![wählen Sie die productbll-Klasse aus.](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Abbildung 15**: Auswählen der Verwendung der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))

[![Konfigurieren von ObjectDataSource für die Verwendung der getproductbyproductid (ProductID)-Methode](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Abbildung 16**: Konfigurieren von ObjectDataSource für die Verwendung der `GetProductByProductID(productID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))

[![den Wert des ProductID-Parameters aus der Dropdown Liste productbycategory abrufen](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Abbildung 17**: Abrufen des *`productID`* -Parameter Werts aus der `ProductsByCategory` Dropdown List ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))

Sie können die verfügbaren Felder in der DetailsView anzeigen. Ich habe mich entschieden, die Felder `ProductID`, `SupplierID`und `CategoryID` zu entfernen und die übrigen Felder neu angeordnet und formatiert Außerdem habe ich die Eigenschaften "`Height`" und "`Width`" der DetailsView gelöscht, sodass die DetailsView auf die Breite erweitert werden kann, um die Daten am besten anzuzeigen, anstatt Sie auf eine angegebene Größe beschränkt zu haben. Das vollständige Markup wird unten angezeigt:

[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Nehmen Sie sich einen Moment Zeit, um die `MasterDetailsDetails.aspx` Seite in einem Browser auszuprobieren. Auf den ersten Blick sieht es so aus, als ob alles wie gewünscht funktioniert, aber es gibt ein Problem mit einem geringfügigen Problem. Wenn Sie eine neue Kategorie auswählen, wird der `ProductsByCategory` Dropdown List aktualisiert, um diese Produkte für die ausgewählte Kategorie einzuschließen. die `ProductDetails` DetailsView zeigt jedoch weiterhin die vorherigen Produktinformationen an. Die DetailsView wird aktualisiert, wenn ein anderes Produkt für die ausgewählte Kategorie ausgewählt wird. Wenn Sie außerdem gründlich genug testen, werden Sie feststellen, dass bei der kontinuierlichen Auswahl neuer Kategorien (z. b. auswählen von Getränken aus den `Categories` Dropdown List, dann durch Bedingungen, anschließend für die durch Haltung) jede andere Kategorieauswahl bewirkt, dass die `ProductDetails` DetailsView aktualisiert wird.

Um dieses Problem zu konkretisieren, betrachten wir ein bestimmtes Beispiel. Beim ersten Besuch der Seite wird die Kategorie Getränke ausgewählt, und die zugehörigen Produkte werden in die Dropdown Liste `ProductsByCategory` geladen. Chai ist das ausgewählte Produkt, und seine Details werden in der `ProductDetails` DetailsView angezeigt, wie in Abbildung 18 dargestellt.

[![die Details des ausgewählten Produkts in einer DetailsView angezeigt.](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Abbildung 18**: die Details des ausgewählten Produkts werden in einer DetailsView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))

Wenn Sie die Kategorieauswahl von Getränke in Bedingungen ändern, erfolgt ein Postback, und die `ProductsByCategory` Dropdown List wird entsprechend aktualisiert. in der DetailsView werden jedoch weiterhin Details für Chai angezeigt.

[![die Details des zuvor ausgewählten Produkts weiterhin angezeigt werden.](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Abbildung 19**: die Details des zuvor ausgewählten Produkts werden weiterhin angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))

Wenn Sie ein neues Produkt aus der Liste auswählen, wird die DetailsView erwartungsgemäß aktualisiert. Wenn Sie nach der Produkt Änderung eine neue Kategorie auswählen, wird die DetailsView erneut nicht aktualisiert. Wenn Sie jedoch ein neues Produkt auswählen, das Sie für eine neue Kategorie ausgewählt haben, wird die DetailsView aktualisiert. Was ist in der Welt passiert?

Das Problem ist ein Zeit Steuerungs Problem im Lebenszyklus der Seite. Wenn eine Seite angefordert wird, durchläuft Sie eine Reihe von Schritten als Rendering. In einem dieser Schritte überprüfen die ObjectDataSource-Steuerelemente, ob sich Ihre `SelectParameters` Werte geändert haben. Wenn dies der Fall ist, weiß das an ObjectDataSource gebundene datenweb-Steuerelement, dass es die Anzeige aktualisieren muss. Wenn z. b. eine neue Kategorie ausgewählt ist, erkennt der `ProductsByCategoryDataSource` ObjectDataSource, dass die Parameterwerte geändert wurden, und die `ProductsByCategory` DropDownList leitet sich an sich selbst zurück, wobei die Produkte für die ausgewählte Kategorie erhalten werden.

Das Problem, das in dieser Situation auftritt, besteht darin, dass der Punkt im Lebenszyklus der Seite, den die objectdatasources auf geänderte Parameter überprüft, *vor* der erneuten Bindung der zugeordneten datenweb Steuerelemente auftritt. Wenn Sie eine neue Kategorie auswählen, erkennt der `ProductsByCategoryDataSource` ObjectDataSource eine Änderung des Parameter Werts. Die von der `ProductDetails` DetailsView verwendete ObjectDataSource hat jedoch keine Änderungen an solchen Änderungen, da die `ProductsByCategory` Dropdown List noch nicht wieder hergestellt werden muss. Zu einem späteren Zeitpunkt im Lebenszyklus wird der `ProductsByCategory` DropDownList an seine ObjectDataSource gebunden, wobei die Produkte für die neu ausgewählte Kategorie gepackt werden. Während sich der Wert der `ProductsByCategory` DropDownList geändert hat, hat die ObjectDataSource der `ProductDetails` DetailsView bereits den Parameterwert überprüft. aus diesem Grund werden in der DetailsView die vorherigen Ergebnisse angezeigt. Diese Interaktion ist in Abbildung 20 dargestellt.

[![sich der productbycategory-DropDownList-Wert geändert, nachdem die ObjectDataSource der ProductDetails DetailsView auf Änderungen überprüft hat.](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Abbildung 20**: der `ProductsByCategory` DropDownList-Wert ändert sich, nachdem die ObjectDataSource der `ProductDetails` DetailsView auf Änderungen überprüft hat ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png)).

Um dieses Problem zu beheben, müssen wir den `ProductDetails` DetailsView explizit neu binden, nachdem die `ProductsByCategory` Dropdown List gebunden wurde. Dies erreichen Sie, indem Sie die `DataBind()`-Methode der `ProductDetails` DetailsView aufrufen, wenn das `DataBound` Ereignis der `ProductsByCategory` DropDownList ausgelöst wird. Fügen Sie der Code Behind-Klasse der `MasterDetailsDetails.aspx` Seite den folgenden Ereignishandlercode hinzu (Weitere Informationen zum Hinzufügen eines Ereignis Handlers finden Sie im Abschnitt "Programm gesteuertes[Festlegen der Parameter Werte von ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"):

[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Nachdem dieser explizite aufrufsbefehl der `DataBind()`-Methode der `ProductDetails` DetailsView hinzugefügt wurde, funktioniert das Tutorial erwartungsgemäß. In Abbildung 21 wird hervorgehoben, wie sich dadurch das vorherige Problem behoben hat.

[![ProductDetails DetailsView wird explizit aktualisiert, wenn das Daten gebundene Ereignis "productbycategory DropDownList" ausgelöst wird.](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Abbildung 21**: die `ProductDetails` DetailsView wird explizit aktualisiert, wenn das `DataBound` Ereignis der `ProductsByCategory` DropDownList ausgelöst wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))

## <a name="summary"></a>Zusammenfassung

Die Dropdown List dient als ideales Benutzeroberflächen Element für Master-/Detailberichte, bei denen eine 1: n-Beziehung zwischen den Master-und Detaildaten Sätzen vorliegt. Im vorherigen Tutorial wurde erläutert, wie Sie eine einzelne Dropdown List verwenden, um die Produkte zu filtern, die in der ausgewählten Kategorie angezeigt werden. In diesem Tutorial haben wir die GridView der Produkte durch eine Dropdown List ersetzt und eine DetailsView verwendet, um die Details des ausgewählten Produkts anzuzeigen. Die in diesem Tutorial beschriebenen Konzepte können problemlos auf Datenmodelle erweitert werden, die mehrere 1: n-Beziehungen betreffen, wie z. b. Kunden, Bestellungen und Bestell Elemente. Im Allgemeinen können Sie in den 1: n-Beziehungen immer eine Dropdown List für jede der "One"-Entitäten hinzufügen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Prüfer für dieses Tutorial war Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Weiter](master-detail-filtering-across-two-pages-cs.md)
