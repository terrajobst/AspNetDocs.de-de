---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Filtern von Master-/detailfiltern mit Dropdown List (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial erfahren Sie, wie Sie Master-/Detailberichte auf einer einzelnen Webseite anzeigen, indem Sie Dropdown Listen verwenden, um die Master-Datensätze und einen DataList anzuzeigen...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 537f8e76bc0cbfa759a014b63ae5f68b5d3ca64d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78491085"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtern von Master-/Detailberichten mit einem DropDownList-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) oder [PDF herunterladen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> In diesem Tutorial erfahren Sie, wie Sie Master-/Detailberichte auf einer einzelnen Webseite anzeigen, indem Sie Dropdown Listen verwenden, um die "Master"-Datensätze anzuzeigen, und einen DataList zum Anzeigen der "Details".

## <a name="introduction"></a>Einführung

Der Master/Detail-Bericht, den wir zuerst mithilfe eines GridView-Filters in der vorherigen [Master-/detailfilterung mit einem Dropdown List-](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) Tutorial erstellt haben, beginnt mit dem Anzeigen einiger "Master"-Datensätze. Der Benutzer kann dann einen Drilldown in einen der Master Datensätze ausführen und so die Details des Master Datensatzes anzeigen. Master/Detail-Berichte sind eine ideale Wahl zum Visualisieren von 1: n-Beziehungen und zum Anzeigen detaillierter Informationen von besonders "breiten" Tabellen (mit vielen Spalten). Wir haben untersucht, wie Master-/Detailberichte mithilfe der GridView-und DetailsView-Steuerelemente in vorherigen Tutorials implementiert werden. In diesem Lernprogramm und den nächsten zwei untersuchen wir diese Konzepte erneut, aber konzentrieren wir uns stattdessen auf die Verwendung von DataList-und Repeater-Steuerelementen.

In diesem Tutorial betrachten wir die Verwendung einer Dropdown List, die die "Master"-Datensätze enthält, wobei die "Details"-Datensätze in einem DataList angezeigt werden.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Schritt 1: Hinzufügen der Webseiten mit dem Master/Detail-Tutorial

Bevor wir mit diesem Tutorial beginnen, nehmen wir uns einen Moment Zeit, um die Ordner-und ASP.NET Seiten hinzuzufügen, die für dieses Tutorial erforderlich sind, und die nächsten beiden Arbeiten mit Master/Detail-Berichten, die die DataList-und Repeater-Steuerelemente verwenden. Erstellen Sie zunächst einen neuen Ordner in dem Projekt mit dem Namen `DataListRepeaterFiltering`. Fügen Sie anschließend die folgenden fünf ASP.NET-Seiten zu diesem Ordner hinzu, wobei alle für die Verwendung der Master Seite `Site.master`konfiguriert sind:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`

![Erstellen Sie einen Ordner "datalistrepaterfiltering", und fügen Sie das Tutorial ASP.net Pages hinzu.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Abbildung 1**: Erstellen eines `DataListRepeaterFiltering` Ordners und Hinzufügen des Tutorials ASP.NET Seiten

Öffnen Sie als nächstes die Seite `Default.aspx`, und ziehen Sie das Steuerelement `SectionLevelTutorialListing.ascx` Benutzer aus dem Ordner `UserControls` auf die Entwurfs Oberfläche. Dieses Benutzer Steuerelement, das wir im Lernprogramm zu [Master Seiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-vb.md) erstellt haben, listet die Site Map auf und zeigt die Tutorials aus dem aktuellen Abschnitt in einer Aufzählung an.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))

Damit in der Auflistungs Liste die Lernprogramme für Master/Details angezeigt werden, die wir erstellen, müssen wir Sie der Site Übersicht hinzufügen. Öffnen Sie die Datei `Web.sitemap`, und fügen Sie das folgende Markup nach dem "Anzeigen von Daten mit dem DataList-und Repeater"-Site Map-Knoten Markup hinzu:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]

![Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Abbildung 3**: Aktualisieren der Site Übersicht, um die neuen ASP.NET-Seiten einzuschließen

## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Schritt 2: Anzeigen der Kategorien in einer Dropdown Liste

Der Master/Detail-Bericht listet die Kategorien in einer Dropdown Liste auf, wobei die Produkte des ausgewählten Listen Elements weiter unten auf der Seite in einem DataList angezeigt werden. Die erste Aufgabe vor uns besteht darin, die Kategorien in einer Dropdown Liste anzuzeigen. Öffnen Sie zunächst die Seite `FilterByDropDownList.aspx` im Ordner `DataListRepeaterFiltering`, und ziehen Sie eine Dropdown List aus der Toolbox auf den Designer der Seite. Legen Sie als nächstes die `ID`-Eigenschaft der Dropdown List auf `Categories`fest. Klicken Sie in der Dropdown List-Smarttag auf den Link Datenquelle auswählen, und erstellen Sie eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`.

[![eine neue ObjectDataSource mit dem Namen "categoriesdatasource" hinzufügen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Abbildung 4**: Hinzufügen einer neuen ObjectDataSource mit dem Namen "`CategoriesDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))

Konfigurieren Sie die neue ObjectDataSource so, dass Sie die `GetCategories()`-Methode der `CategoriesBLL` Klasse aufruft. Nach dem Konfigurieren von ObjectDataSource müssen Sie immer noch angeben, welches Datenquellen Feld in der Dropdown Liste angezeigt werden soll und welches als Wert für jedes Listenelement zugeordnet werden soll. Legen Sie das `CategoryName`-Feld als Anzeige und als Wert für jedes Listenelement `CategoryID`.

[![Dropdown List das Feld "CategoryName" anzeigen und "CategoryID" als Wert verwenden.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Abbildung 5**: "DropDownList" zeigt das `CategoryName` Feld an und verwendet `CategoryID` als Wert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))

An diesem Punkt verfügen wir über ein Dropdown List-Steuerelement, das mit den Datensätzen aus der `Categories` Tabelle gefüllt wird (alle in ungefähr sechs Sekunden). In Abbildung 6 wird der Fortschritt angezeigt, der in einem Browser angezeigt wird.

[![einer Dropdown Liste werden die aktuellen Kategorien aufgelistet.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Abbildung 6**: eine Dropdown Liste listet die aktuellen Kategorien auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))

## <a name="step-2-adding-the-products-datalist"></a>Schritt 2: Hinzufügen des Products-DataList

Im letzten Schritt des Master/Detail-Berichts werden die Produkte aufgelistet, die der ausgewählten Kategorie zugeordnet sind. Um dies zu erreichen, fügen Sie der Seite einen DataList hinzu, und erstellen Sie eine neue ObjectDataSource mit dem Namen `ProductsByCategoryDataSource`. Lassen Sie das `ProductsByCategoryDataSource`-Steuerelement die Daten aus der `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse abrufen. Da dieser Master-/Detailbericht schreibgeschützt ist, wählen Sie auf den Registerkarten einfügen, aktualisieren und löschen die Option (keine) aus.

[![wählen Sie die getproduczbycategoryid (CategoryID)-Methode aus.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Abbildung 7**: Auswählen der `GetProductsByCategoryID(categoryID)` Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))

Nachdem Sie auf "weiter" geklickt haben, fordert der ObjectDataSource-Assistent Sie zur Angabe der Quelle des Werts für den *`categoryID`* Parameter der `GetProductsByCategoryID(categoryID)` Methode auf. Um den Wert des ausgewählten `categories` DropDownList-Elements zu verwenden, legen Sie die Parameter Quelle auf Control und ControlID auf `Categories`fest.

[![den CategoryID-Parameter auf den Wert der Dropdown List-Kategorien festlegen.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Abbildung 8**: Festlegen des *`categoryID`* -Parameters auf den Wert des `Categories` Dropdown List ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen wird von Visual Studio automatisch ein `ItemTemplate` für den DataList generiert, der den Namen und den Wert der einzelnen Datenfelder anzeigt. Wir erweitern den DataList, um stattdessen eine `ItemTemplate` zu verwenden, die nur den Namen, die Kategorie, den Lieferanten, die Menge pro Einheit und den Preis des Produkts anzeigt, sowie eine `SeparatorTemplate`, die ein `<hr>` Element zwischen jedem Element einfügt. Ich verwende die `ItemTemplate` aus einem Beispiel im Tutorial "Anzeigen von [Daten mit dem DataList-und Repeater-](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) Steuerelement", aber Sie können auch das beliebige Vorlagen Markup verwenden, das Sie visuell ansprechend finden.

Nachdem Sie diese Änderungen vorgenommen haben, sollten Ihr DataList-und Ihr ObjectDataSource-Markup in etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Nehmen Sie sich einen Moment Zeit, um den Fortschritt in einem Browser zu überprüfen. Beim ersten Besuch der Seite werden die Produkte, die der ausgewählten Kategorie (Getränke) angehören, angezeigt (wie in Abbildung 9 dargestellt). durch das Ändern der Dropdown Liste werden die Daten jedoch nicht aktualisiert. Dies liegt daran, dass ein Postback erfolgen muss, damit der DataList aktualisiert werden kann. Um dies zu erreichen, können Sie entweder die `AutoPostBack`-Eigenschaft der DropDownList auf `true` festlegen oder der Seite ein Schaltflächen-websteuer Element hinzufügen. Für dieses Lernprogramm habe ich die `AutoPostBack`-Eigenschaft der Dropdown List auf `true`festgelegt.

Die Abbildungen 9 und 10 veranschaulichen den Master-/Detailbericht in Aktion.

[beim ersten Besuch der Seite werden die Getränkeprodukte angezeigt. ![](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Abbildung 9**: beim ersten Besuch der Seite werden die Getränkeprodukte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))

[![auswählen eines neuen Produkts (erzeugt) automatisch ein Postback, Aktualisieren des DataList-Elements](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Abbildung 10**: das Auswählen eines neuen Produkts (erzeugt) löst automatisch ein Postback, aktualisiert den DataList ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))

## <a name="adding-a----choose-a-category----list-item"></a>Hinzufügen eines "--Choose a category--" Listen Elements

Beim ersten Besuch der Seite "`FilterByDropDownList.aspx`" ist das erste Listenelement (Getränke) der Kategorie DropDownList standardmäßig ausgewählt, das die Getränkeprodukte in der DataList anzeigt. Im Lernprogramm zum *Filtern von Master-/detailfiltern mit einem Dropdown List* -Tutorial haben wir die Option "--wählen Sie eine Kategorie" in der Dropdown Liste hinzugefügt, die standardmäßig ausgewählt ist, und *alle* Produkte in der Datenbank angezeigt, wenn Sie ausgewählt wurden. Ein solcher Ansatz war bei der Auflistung der Produkte in einem GridView-Verfahren zu handhaben, da jede Produkt Zeile einen kleinen Teil der Bildschirmgröße aufbrauchte. Mit dem DataList-DataList verbraucht die Informationen aller Produkte jedoch einen viel größeren Teil des Bildschirms. Fügen Sie die Option "--wählen Sie eine Kategorie" hinzu, und lassen Sie die Option standardmäßig ausgewählt. Wenn Sie jedoch nicht alle Produkte anzeigen, wenn Sie ausgewählt ist, konfigurieren Sie Sie so, dass keine Produkte angezeigt werden.

Um der Dropdown Liste ein neues Listenelement hinzuzufügen, klicken Sie auf die Eigenschaftenfenster, und klicken Sie dann auf die Auslassungs Punkte in der `Items`-Eigenschaft. Fügen Sie ein neues Listenelement mit dem `Text` "--wählen Sie eine Kategorie--" und die `Value` `0`hinzu.

![Hinzufügen eines](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Abbildung 11**: Hinzufügen eines Listen Elements "--wählen Sie eine Kategorie"

Alternativ können Sie das Listenelement hinzufügen, indem Sie der Dropdown Liste das folgende Markup hinzufügen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Außerdem müssen wir die `AppendDataBoundItems` des DropDownList-Steuer Elements auf `true` festlegen, denn wenn die Kategorie auf "`false` (Standard)" festgelegt ist, werden alle manuell hinzugefügten Listenelemente überschrieben, wenn die Kategorien an die Dropdown Liste aus der ObjectDataSource gebunden werden.

![Legen Sie die AppendDataBoundItems-Eigenschaft auf "true" fest.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Abbildung 12**: Festlegen der `AppendDataBoundItems`-Eigenschaft auf "true"

Der Grund, warum wir den Wert `0` für das Listenelement "--wählen Sie eine Kategorie" gewählt haben, liegt darin, dass im System keine Kategorien mit dem Wert "`0`" vorhanden sind. Daher werden keine Produktdaten Sätze zurückgegeben, wenn das Listenelement "--wählen Sie eine Kategorie" ausgewählt ist. Um dies zu bestätigen, nehmen Sie sich einen Moment Zeit, um die Seite über einen Browser zu besuchen. Wie in Abbildung 13 gezeigt, wird beim anfänglichen Anzeigen der Seite das Listenelement "--wählen Sie eine Kategorie" ausgewählt, und es werden keine Produkte angezeigt.

[![, wenn die](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Abbildung 13**: Wenn das Listenelement "--Kategorie auswählen" ausgewählt ist, werden keine Produkte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))

Wenn Sie *alle* Produkte anzeigen möchten, wenn die Option "--wählen Sie eine Kategorie" ausgewählt ist, verwenden Sie stattdessen den Wert `-1`. Der Leser, der sich in der *Master/Detail-Filterung mit einem Dropdown List* -Tutorial befand, erinnert, dass wir die `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse aktualisiert haben, damit bei der Übergabe eines *`categoryID`* Werts `-1` alle Produktdaten Sätze zurückgegeben werden.

## <a name="summary"></a>Zusammenfassung

Wenn hierarchisch verwandte Daten angezeigt werden, ist es häufig hilfreich, die Daten mithilfe von Master-/Detailberichten darzustellen, von denen der Benutzer mit der Verwendung der Daten vom oberen Rand der Hierarchie beginnen und Details anzeigen kann. In diesem Tutorial wurde das Entwickeln eines einfachen Master/Detail-Berichts untersucht, der die Produkte einer ausgewählten Kategorie anzeigt. Dies wurde erreicht, indem eine Dropdown Liste für die Liste der Kategorien und ein DataList für die Produkte verwendet wurde, die zur ausgewählten Kategorie gehören.

Im nächsten Tutorial untersuchen wir die Trennung der Master-und Detaildaten Sätze auf zwei Seiten. Auf der ersten Seite wird eine Liste der "Master"-Datensätze mit einem Link zum Anzeigen der Details angezeigt. Wenn Sie auf den Link klicken, wird der Benutzer auf die zweite Seite angezeigt, auf der die Details für den ausgewählten Master Daten Satz angezeigt werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Reviewer für dieses Tutorial war Randy Schmidt. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [Weiter](master-detail-filtering-acess-two-pages-datalist-vb.md)
