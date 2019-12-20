---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Filtern von Master-/detailfiltern mit Dropdown List (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie die Master Datensätze in einem Dropdown List-Steuerelement und die Details des ausgewählten Listen Elements in einer GridView angezeigt werden.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 62cd296a3f36e1779666a6b5db15b0ce2488d0e4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74639913"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtern von Master-/Detailberichten mit einem DropDownList-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) oder [PDF herunterladen](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> In diesem Tutorial erfahren Sie, wie die Master Datensätze in einem Dropdown List-Steuerelement und die Details des ausgewählten Listen Elements in einer GridView angezeigt werden.

## <a name="introduction"></a>Einführung

Ein allgemeiner Berichtstyp ist der *Master/Detail-Bericht*, in dem der Bericht beginnt, indem er einen Satz von "Master"-Datensätzen anzeigt. Der Benutzer kann dann einen Drilldown in einen der Master Datensätze ausführen und so die Details des Master Datensatzes anzeigen. Master/Detail-Berichte sind eine ideale Wahl für die Visualisierung von 1: n-Beziehungen, z. b. einen Bericht, der alle Kategorien anzeigt und einem Benutzer dann ermöglicht, eine bestimmte Kategorie auszuwählen und die zugehörigen Produkte anzuzeigen. Außerdem sind Master/Detail-Berichte nützlich, um ausführliche Informationen von besonders "breiten" Tabellen (mit vielen Spalten) anzuzeigen. Beispielsweise kann die Master Ebene eines Master/Detail-Berichts nur den Produktnamen und den Einzelpreis der Produkte in der Datenbank anzeigen, und ein Drilldown in ein bestimmtes Produkt würde die zusätzlichen Produktfelder (Kategorie, Lieferant, Menge pro Einheit usw.) anzeigen.

Es gibt viele Möglichkeiten, wie ein Master-/Detailbericht implementiert werden kann. In diesem und den nächsten drei Tutorials betrachten wir eine Reihe von Master-/Detailberichten. In diesem Tutorial erfahren Sie, wie die Master Datensätze in einem [Dropdown List-Steuer](https://msdn.microsoft.com/library/dtx91y0z.aspx) Element und die Details des ausgewählten Listen Elements in einer GridView angezeigt werden. Insbesondere der Master/Detail-Bericht dieses Lernprogramms listet die Kategorie-und Produktinformationen auf.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Schritt 1: Anzeigen der Kategorien in einer Dropdown Liste

Der Master/Detail-Bericht listet die Kategorien in einer Dropdown Liste auf, wobei die Produkte des ausgewählten Listen Elements weiter unten auf der Seite in einer GridView angezeigt werden. Die erste Aufgabe vor uns besteht darin, die Kategorien in einer Dropdown Liste anzuzeigen. Öffnen Sie die Seite `FilterByDropDownList.aspx` im Ordner `Filtering`, ziehen Sie die Dropdown Liste aus der Toolbox in den Designer der Seite, und legen Sie die Eigenschaft `ID` auf `Categories`fest. Klicken Sie anschließend im Smarttags DropDownList auf den Link Datenquelle auswählen. Dadurch wird der Assistent zum Konfigurieren von Datenquellen angezeigt.

[![die Datenquelle der DropDownList anzugeben.](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Abbildung 1**: Geben Sie die Datenquelle der DropDownList[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png)).

Wählen Sie das Hinzufügen einer neuen ObjectDataSource namens `CategoriesDataSource` aus, die die `GetCategories()`-Methode der `CategoriesBLL` Klasse aufruft.

[![eine neue ObjectDataSource mit dem Namen "categoriesdatasource" hinzufügen](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie eine neue ObjectDataSource mit dem Namen `CategoriesDataSource` hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png)).

[![wählen Sie die kategoriesbll-Klasse aus.](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie die `CategoriesBLL` Klasse aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png)).

[![konfigurieren Sie ObjectDataSource für die Verwendung der GetCategories ()-Methode.](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource für die Verwendung der `GetCategories()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))

Nach dem Konfigurieren von ObjectDataSource müssen Sie immer noch angeben, welches Datenquellen Feld in DropDownList angezeigt werden soll und welches als Wert für das Listenelement zugeordnet werden soll. Legen Sie das `CategoryName`-Feld als Anzeige und als Wert für jedes Listenelement `CategoryID`.

[![Dropdown List das Feld "CategoryName" anzeigen und "CategoryID" als Wert verwenden.](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Abbildung 5**: Legen Sie in der Dropdown Liste das Feld `CategoryName` angezeigt, und verwenden Sie `CategoryID` als Wert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png)).

An diesem Punkt verfügen wir über ein Dropdown List-Steuerelement, das mit den Datensätzen aus der `Categories` Tabelle gefüllt wird (alle in ungefähr sechs Sekunden). In Abbildung 6 wird der Fortschritt angezeigt, der in einem Browser angezeigt wird.

[![einer Dropdown Liste werden die aktuellen Kategorien aufgelistet.](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Abbildung 6**: Eine Dropdown Liste listet die aktuellen Kategorien auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))

## <a name="step-2-adding-the-products-gridview"></a>Schritt 2: Hinzufügen der Produkte GridView

Im letzten Schritt des Master/Detail-Berichts werden die Produkte aufgelistet, die der ausgewählten Kategorie zugeordnet sind. Fügen Sie hierzu der Seite eine GridView hinzu, und erstellen Sie eine neue ObjectDataSource mit dem Namen `productsDataSource`. Lassen Sie das `productsDataSource` Steuerelement die Daten aus der `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse.

[![wählen Sie die getproduczbycategoryid (CategoryID)-Methode aus.](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Abbildung 7**: Wählen Sie die `GetProductsByCategoryID(categoryID)` Methode aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png)).

Nachdem Sie diese Methode ausgewählt haben, werden Sie vom Assistenten für ObjectDataSource aufgefordert, den Wert für den *`categoryID`* Parameter der Methode einzugeben. Um den Wert des ausgewählten `categories` DropDownList-Elements zu verwenden, legen Sie die Parameter Quelle auf Control und ControlID auf `Categories`fest.

[![den CategoryID-Parameter auf den Wert der Dropdown List-Kategorien festlegen.](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Abbildung 8**: Legen Sie den *`categoryID`* -Parameter auf den Wert des `Categories` Dropdown List fest ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png)).

Nehmen Sie sich einen Moment Zeit, um den Fortschritt in einem Browser zu überprüfen. Wenn Sie die Seite zum ersten Mal aufrufen, gehören diese Produkte zur ausgewählten Kategorie (Getränke), die angezeigt wird (wie in Abbildung 9 dargestellt). durch das Ändern der Dropdown Liste werden die Daten jedoch nicht aktualisiert. Dies liegt daran, dass ein Postback erfolgen muss, damit die GridView aktualisiert wird. Um dies zu erreichen, stehen zwei Optionen zur Verfügung (es ist nicht erforderlich, Code zu schreiben):

- **Legen Sie die**[Auto-Back-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**der Kategorien DropDownListauf true fest.** (Sie können dies erreichen, indem Sie die Option "AutoPostBack aktivieren" im Smarttags der Dropdown Liste aktivieren.) Dadurch wird immer dann ein Postback auslöst, wenn das ausgewählte Element von DropDownList vom Benutzer geändert wird. Wenn der Benutzer daher eine neue Kategorie aus der Dropdown Liste auswählt, wird ein Postback ausgeführt, und die GridView wird mit den Produkten für die neu ausgewählte Kategorie aktualisiert. (Dies ist der Ansatz, den ich in diesem Tutorial verwendet habe.)
- **Fügen Sie ein Schaltflächen-websteuer Element neben der Dropdown list hinzu.** Legen Sie die `Text`-Eigenschaft auf Aktualisieren oder ähnliches fest. Bei diesem Ansatz muss der Benutzer eine neue Kategorie auswählen und dann auf die Schaltfläche klicken. Wenn Sie auf die Schaltfläche klicken, wird ein Postback ausgelöst, und die GridView wird aktualisiert, um die Produkte der ausgewählten Kategorie aufzulisten.

Die Abbildungen 9 und 10 veranschaulichen den Master-/Detailbericht in Aktion.

[beim ersten Besuch der Seite werden die Getränkeprodukte angezeigt. ![](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Abbildung 9**: Beim ersten Besuch der Seite werden die Getränkeprodukte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png)).

[![auswählen eines neuen Produkts (erzeugt) automatisch ein Postback, Aktualisieren der GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Abbildung 10**: Wenn Sie ein neues Produkt (erzeugt) auswählen, wird automatisch ein Postback ausgelöst, das GridView-Bild wird aktualisiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png)

## <a name="adding-a----choose-a-category----list-item"></a>Hinzufügen eines "--Choose a category--" Listen Elements

Beim ersten Besuch der Seite "`FilterByDropDownList.aspx`" ist das erste Listenelement (Getränke) der Kategorien DropDownList standardmäßig ausgewählt, und die Getränkeprodukte werden in der GridView angezeigt. Anstatt die Produkte der ersten Kategorie anzuzeigen, empfiehlt es sich, stattdessen ein DropDownList-Element auszuwählen, das etwa wie folgt aussieht: "--wählen Sie eine Kategorie--".

Um der Dropdown Liste ein neues Listenelement hinzuzufügen, klicken Sie auf die Eigenschaftenfenster, und klicken Sie dann auf die Auslassungs Punkte in der `Items`-Eigenschaft. Fügen Sie ein neues Listenelement mit dem `Text` "--wählen Sie eine Kategorie--" und die `Value` `-1`hinzu.

[![hinzufügen: Wählen Sie eine Kategorie aus, und wählen Sie ein Listenelement aus.](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Abbildung 11**: Hinzufügen: Wählen Sie eine Kategorie aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png)).

Alternativ können Sie das Listenelement hinzufügen, indem Sie der Dropdown Liste das folgende Markup hinzufügen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Außerdem müssen wir die `AppendDataBoundItems` des DropDownList-Steuer Elements auf "true" festlegen, da wenn die Kategorien an die Dropdown List von der ObjectDataSource gebunden werden, werden alle manuell hinzugefügten Listenelemente überschrieben, wenn `AppendDataBoundItems` nicht "true" ist.

![Legen Sie die AppendDataBoundItems-Eigenschaft auf "true" fest.](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Abbildung 12**: Legen Sie die `AppendDataBoundItems`-Eigenschaft auf true fest.

Nachdem diese Änderungen vorgenommen wurden, wird beim ersten Besuch der Seite die Option "--wählen Sie eine Kategorie aus" ausgewählt, und es werden keine Produkte angezeigt.

[![auf dem ersten Seiten Ladevorgang werden keine Produkte angezeigt.](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Abbildung 13**: Auf dem ersten Seiten Ladevorgang werden keine Produkte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png)).

Der Grund dafür, dass keine Produkte angezeigt werden, wenn das Listenelement "--wählen Sie eine Kategorie" ausgewählt ist, ist der Wert `-1`, und es sind keine Produkte in der Datenbank mit einer `CategoryID` von `-1`vorhanden. Wenn dies das gewünschte Verhalten ist, sind Sie an dieser Stelle fertig! Wenn Sie jedoch *alle* Kategorien anzeigen möchten, wenn das Listenelement "--Kategorie auswählen" ausgewählt ist, kehren Sie zur `ProductsBLL`-Klasse zurück, und passen Sie die `GetProductsByCategoryID(categoryID)`-Methode so an, dass Sie die `GetProducts()`-Methode aufruft, wenn der übergebenen *`categoryID`* -Parameter kleiner als 0 (null) ist:

[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

Das hier verwendete Verfahren ähnelt dem Ansatz, mit dem wir alle Zulieferer im [deklarativen](../basic-reporting/declarative-parameters-cs.md) parametertutorial angezeigt haben. in diesem Beispiel verwenden wir jedoch den Wert `-1`, um anzugeben, dass im Gegensatz zu `Nothing`alle Datensätze abgerufen werden sollen. Dies liegt daran, dass der *`categoryID`* -Parameter der `GetProductsByCategoryID(categoryID)`-Methode als ganzzahliger Wert erwartet, während wir im Tutorial für deklarative Parameter einen Zeichen folgen-Eingabeparameter übergeben haben.

In Abbildung 14 wird ein Screenshot der `FilterByDropDownList.aspx` angezeigt, wenn die Option "--wählen Sie eine Kategorie" ausgewählt ist. Hier werden alle Produkte standardmäßig angezeigt, und der Benutzer kann die Anzeige eingrenzen, indem er eine bestimmte Kategorie auswählt.

[![alle Produkte sind jetzt standardmäßig aufgeführt.](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Abbildung 14**: Alle Produkte sind jetzt standardmäßig aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png)).

## <a name="summary"></a>Zusammenfassung

Wenn hierarchisch verwandte Daten angezeigt werden, ist es häufig hilfreich, die Daten mithilfe von Master-/Detailberichten darzustellen, von denen der Benutzer mit der Verwendung der Daten vom oberen Rand der Hierarchie beginnen und Details anzeigen kann. In diesem Tutorial wurde das Entwickeln eines einfachen Master/Detail-Berichts untersucht, der die Produkte einer ausgewählten Kategorie anzeigt. Dies wurde erreicht, indem eine Dropdown Liste für die Liste der Kategorien und eine GridView für die Produkte verwendet wurde, die zur ausgewählten Kategorie gehören.

Im [nächsten Tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) übernehmen wir die DropDownList-Schnittstelle mit zwei Dropdown Listen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Weiter](master-detail-filtering-with-two-dropdownlists-vb.md)
