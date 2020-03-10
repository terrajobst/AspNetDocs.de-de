---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: Filtern von Master-/detailfiltern auf zwei Seiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, wie Sie einen Master-/Detailbericht auf zwei Seiten aufteilen. Auf der Seite "Master" verwenden wir ein Repeater-Steuerelement, um eine Liste von "...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 037e5f47efff88bfcbec57b11efa4fec04f9542d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78491475"
---
# <a name="masterdetail-filtering-across-two-pages-vb"></a>Filtern von Master-/Detailberichten über zwei Seiten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) oder [PDF herunterladen](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> In diesem Tutorial wird erläutert, wie Sie einen Master-/Detailbericht auf zwei Seiten aufteilen. Auf der Seite "Master" verwenden wir ein Repeater-Steuerelement, um eine Liste von Kategorien zu erstellen, die den Benutzer beim Klicken auf die Seite "Details" übernehmen, auf der ein zweispaltige DataList die Produkte anzeigt, die zur ausgewählten Kategorie gehören.

## <a name="introduction"></a>Einführung

Im Tutorial [Master/Detail-Filterung über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-vb.md) haben wir dieses Muster mithilfe einer GridView untersucht, um alle Lieferanten im System anzuzeigen. Diese GridView enthielt ein HyperLinkField, das als Link zu einer zweiten Seite gerendert wurde und die `SupplierID` in der Abfrage Zeichenfolge übergibt. Die zweite Seite hat eine GridView zum Auflisten der Produkte verwendet, die vom ausgewählten Lieferanten bereitgestellt werden.

Solche Master-/Detailberichte mit zwei Seiten können auch mit DataList-und Repeater-Steuerelementen durchgeführt werden. Der einzige Unterschied besteht darin, dass weder der DataList noch der Repeater Unterstützung für das HyperLinkField-Steuerelement bereitstellt. Stattdessen müssen Sie ein Hyperlink-websteuer Element oder ein Anker-HTML-Element (`<a>`) innerhalb der `ItemTemplate`des Steuer Elements hinzufügen. Die `NavigateUrl`-Eigenschaft des Links oder das `href` Attribut des Ankers können dann mithilfe von deklarativen oder programmgesteuerten Ansätzen angepasst werden.

In diesem Tutorial untersuchen wir ein Beispiel, das die Kategorien in einer Auflistungs Liste auf einer Seite mithilfe eines Wiederholungs Steuer Elements auflistet. Jedes Listenelement enthält den Namen und die Beschreibung der Kategorie, wobei der Kategoriename als Link zu einer zweiten Seite angezeigt wird. Wenn Sie auf diesen Link klicken, wird der Benutzer auf die zweite Seite angezeigt, auf der ein DataList die Produkte anzeigt, die der ausgewählten Kategorie angehören.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Schritt 1: Anzeigen der Kategorien in einer Auflistungs Liste

Der erste Schritt beim Erstellen eines Master-/Detailberichts besteht darin, die Master-Datensätze anzuzeigen. Daher besteht unsere erste Aufgabe darin, die Kategorien auf der Seite "Master" anzuzeigen. Öffnen Sie die Seite `CategoryListMaster.aspx` im Ordner `DataListRepeaterFiltering`, fügen Sie ein Repeater-Steuerelement hinzu, und wählen Sie aus dem Smarttag ein neues ObjectDataSource-Element hinzufügen aus. Konfigurieren Sie die neue ObjectDataSource so, dass Sie von der `GetCategories` Methode der `CategoriesBLL` Klasse auf Ihre Daten zugreift (siehe Abbildung 1).

[![Konfigurieren von ObjectDataSource für die Verwendung der GetCategories-Methode der categoriesbll-Klasse.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Abbildung 1**: Konfigurieren von ObjectDataSource für die Verwendung der `GetCategories`-Methode der `CategoriesBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))

Definieren Sie als nächstes die Vorlagen des Repeater so, dass jeder Kategorienname und jede Beschreibung als Element in einer Auflistungs Liste angezeigt wird. Wir kümmern uns noch nicht darum, dass die einzelnen Kategorien mit der Detailseite verknüpft sind. Das folgende Beispiel zeigt das deklarative Markup für "Repeater" und "ObjectDataSource":

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Wenn dieses Markup abgeschlossen ist, nehmen Sie sich einen Moment Zeit, um den Fortschritt über einen Browser anzuzeigen. Wie in Abbildung 2 gezeigt, wird der Repeater als Auflistungs Liste gerendert, die den Namen und die Beschreibung der einzelnen Kategorien anzeigt.

[![jede Kategorie als Auflistungs Listenelement angezeigt wird.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Abbildung 2**: jede Kategorie wird als Auflistungs Listenelement angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))

## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Schritt 2: Umbenennen des Kategorienamens in einen Link zur Detailseite

Um es Benutzern zu gestatten, die "Details"-Informationen für eine bestimmte Kategorie anzuzeigen, müssen wir jedem Auflistungs Listenelement einen Link hinzufügen, der den Benutzer beim Klicken auf die zweite Seite (`ProductsForCategoryDetails.aspx`) führt. Auf dieser zweiten Seite werden dann die Produkte für die ausgewählte Kategorie mithilfe eines DataList-Steuerelemente angezeigt. Um die Kategorie zu bestimmen, auf die geklickt wurde, müssen wir die `CategoryID` der Kategorie, auf die geklickt wurde, auf die zweite Seite durch einen Mechanismus übergeben. Die einfachste, einfachste Methode zum Übertragen von skalaren Daten von einer Seite auf eine andere ist die Abfrage Zeichenfolge. Dies ist die Option, die in diesem Tutorial verwendet wird. Insbesondere erwartet die `ProductsForCategoryDetails.aspx` Seite, dass der ausgewählte *`categoryID`* Wert durch ein QueryString-Feld mit dem Namen `CategoryID`durchlaufen wird. Wenn Sie z. b. die Produkte der Kategorie Getränke anzeigen möchten, die einen `CategoryID` von 1 hat, besuchen Sie `ProductsForCategoryDetails.aspx?CategoryID=1`.

Um einen Hyperlink für jedes Auflistungs Listenelement im Wiederholungs Modul zu erstellen, müssen Sie der `ItemTemplate`entweder ein Hyperlink-websteuer Element oder ein HTML-Anker Element (`<a>`) hinzufügen. In Szenarien, in denen der Hyperlink für jede Zeile gleich angezeigt wird, genügt beides. Für Repeater verwende ich lieber das Anchor-Element. Um das Anchor-Element zu verwenden, aktualisieren Sie das ItemTemplate-Element des Repeater auf Folgendes:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Beachten Sie, dass der `CategoryID` direkt innerhalb des `href` Attributs des Anker Elements eingefügt werden kann. um dies zu tun, müssen Sie jedoch sicher sein, dass der Wert des `href` Attributs mit Apostrophe (und Anführungszeichen) getrennt wird, da die `Eval`-Methode innerhalb des `href` Attributs die Zeichenfolge (`"CategoryID"`) mit Anführungszeichen begrenzt. Alternativ kann stattdessen ein Hyperlink-websteuer Element verwendet werden:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Beachten Sie, dass der statische Teil der URL-– `ProductsForCategoryDetails.aspx?CategoryID` – an das Ergebnis von `Eval("CategoryID")` direkt innerhalb der Datenbindung-Syntax mithilfe der Zeichen folgen Verkettung angehängt wird.

Ein Vorteil der Verwendung des Hyperlink-Steuer Elements besteht darin, dass es bei Bedarf Programm gesteuert über den `ItemDataBound` Ereignishandler des Wiederholungs Moduls aufgerufen werden kann. Beispielsweise können Sie den Kategorienamen als Text anstelle eines Links für Kategorien ohne zugeordnete Produkte anzeigen. Eine solche Überprüfung kann Programm gesteuert im `ItemDataBound` Ereignishandler ausgeführt werden. für Kategorien ohne zugeordnete Produkte könnte die `NavigateUrl`-Eigenschaft des Links auf eine leere Zeichenfolge festgelegt werden. Dadurch wird ein bestimmtes Kategoriename als nur-Text gerendert (anstatt als Link). Weitere Informationen zum Formatieren von DataList-und Repeater-Inhalten auf der Grundlage Programm gesteuerter Logik mithilfe des `ItemDataBound`-Ereignis Handlers finden Sie im Tutorial [Formatieren des DataList-und Repeater basierend auf den Daten](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) .

Wenn Sie die nachfolgende Vorgehensweise verwenden, können Sie das Anker Element oder das Hyperlink-Steuerelement auf Ihrer Seite verwenden. Unabhängig von der Vorgehensweise sollte beim Anzeigen der Seite über einen Browser jeder Kategoriename als Link zu `ProductsForCategoryDetails.aspx`gerendert werden, wobei der entsprechende `CategoryID` Wert übergeben wird (siehe Abbildung 3).

[![Sie die Kategorienamen jetzt mit productforcategorydetails. aspx verknüpfen.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Abbildung 3**: die Kategorienamen werden jetzt mit `ProductsForCategoryDetails.aspx` verknüpft ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))

## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Schritt 3: Auflisten der Produkte, die der ausgewählten Kategorie angehören

Wenn die `CategoryListMaster.aspx` Seite abgeschlossen ist, können wir uns auf die Implementierung der Seite "Details" ("Details") konzentrieren, `ProductsForCategoryDetails.aspx`. Öffnen Sie diese Seite, ziehen Sie einen DataList aus der Toolbox auf den Designer, und legen Sie dessen `ID`-Eigenschaft auf `ProductsInCategory`fest. Wählen Sie als nächstes im Smarttags des DataList eine neue ObjectDataSource auf der Seite aus, und benennen Sie Sie `ProductsInCategoryDataSource`. Konfigurieren Sie Sie so, dass Sie die `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse aufruft. Legen Sie die Dropdown Listen auf den Registerkarten einfügen, aktualisieren und löschen auf (keine) fest.

[![konfigurieren Sie ObjectDataSource für die Verwendung der getproductbycategoryid (CategoryID)-Methode der productbll-Klasse.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource für die Verwendung der `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))

Da die `GetProductsByCategoryID(categoryID)`-Methode einen Eingabeparameter ( *`categoryID`* ) akzeptiert, bietet der Assistent zum Auswählen von Datenquellen die Möglichkeit, die Quelle des Parameters anzugeben. Legen Sie die Parameter Quelle mithilfe der QueryStringField-`CategoryID`auf QueryString fest.

[![das QueryString-Feld "CategoryID" als Quelle des Parameters verwenden.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Abbildung 5**: Verwenden des Felds "QueryString" `CategoryID` als Quelle des Parameters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))

Wie in den vorherigen Tutorials gezeigt, erstellt Visual Studio nach Abschluss des Assistenten zum Auswählen von Datenquellen automatisch eine `ItemTemplate` für den DataList, der die einzelnen Daten Feldnamen und-Werte auflistet. Ersetzen Sie diese Vorlage durch eine, die nur den Namen, den Lieferanten und den Preis des Produkts auflistet. Legen Sie außerdem die `RepeatColumns`-Eigenschaft des DataList auf 2 fest. Nachdem Sie diese Änderungen vorgenommen haben, sollte Ihr deklaratives Markup für DataList und ObjectDataSource in etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Um diese Seite in Aktion zu sehen, starten Sie auf der Seite `CategoryListMaster.aspx`. Klicken Sie anschließend in der Liste Kategorien aufzählen auf einen Link. Wenn Sie dies tun, werden Sie `ProductsForCategoryDetails.aspx`, indem Sie die `CategoryID` durch die Abfrage Zeichenfolge übergeben. Der `ProductsInCategoryDataSource` ObjectDataSource in `ProductsForCategoryDetails.aspx` erhält dann nur die Produkte für die angegebene Kategorie und zeigt Sie in der DataList an, die zwei Produkte pro Zeile rendert. Abbildung 6 zeigt einen Screenshot der `ProductsForCategoryDetails.aspx`, wenn Sie die Getränke anzeigen.

[![werden die Getränke angezeigt, zwei pro Zeile](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Abbildung 6**: die Getränke werden angezeigt, zwei pro Zeile ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))

## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Schritt 4: Anzeigen von Kategorieinformationen in productforcategorydetails. aspx

Wenn ein Benutzer in `CategoryListMaster.aspx`auf eine Kategorie klickt, wird er zu `ProductsForCategoryDetails.aspx` und die Produkte angezeigt, die der ausgewählten Kategorie angehören. In `ProductsForCategoryDetails.aspx` gibt es jedoch keine visuellen Hinweise, welche Kategorie ausgewählt wurde. Ein Benutzer, der auf Getränke klicken soll, aber versehentlich auf die Bedingungen geklickt hat, kann den Fehler nicht erkennen, wenn er `ProductsForCategoryDetails.aspx`erreicht. Um dieses potenzielle Problem zu beheben, können Sie Informationen über die ausgewählte Kategorie – den Namen und die Beschreibung – am oberen Rand der Seite `ProductsForCategoryDetails.aspx` anzeigen.

Fügen Sie hierzu in `ProductsForCategoryDetails.aspx`über dem Repeater-Steuerelement eine FormView hinzu. Fügen Sie als nächstes der Seite ein neues ObjectDataSource-Objekt aus dem Smarttag von FormView mit dem Namen `CategoryDataSource` hinzu, und konfigurieren Sie es so, dass die `GetCategoryByCategoryID(categoryID)`-Methode der `CategoriesBLL` Klasse verwendet wird.

[![Zugriffs Informationen über die Kategorie über die getcategorybycategoryid (CategoryID)-Methode der kategoriesbll-Klasse.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Abbildung 7**: Zugreifen auf Informationen über die Kategorie über die `GetCategoryByCategoryID(categoryID)`-Methode der `CategoriesBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))

Wie bei der `ProductsInCategoryDataSource` ObjectDataSource, die in Schritt 3 hinzugefügt wurde, werden Sie vom `CategoryDataSource`-Assistenten zum Konfigurieren von Datenquellen aufgefordert, eine Quelle für den Eingabeparameter der `GetCategoryByCategoryID(categoryID)` Methode einzugeben. Verwenden Sie genau die gleichen Einstellungen wie zuvor, und legen Sie die Parameter Quelle auf QueryString und den Wert QueryStringField auf `CategoryID` fest (siehe Abbildung 5).

Nachdem Sie den Assistenten abgeschlossen haben, erstellt Visual Studio automatisch eine `ItemTemplate`, `EditItemTemplate`und `InsertItemTemplate` für die Form View. Da wir eine schreibgeschützte Schnittstelle bereitstellen, können Sie die `EditItemTemplate` und `InsertItemTemplate`entfernen. Sie können auch die `ItemTemplate`von FormView anpassen. Nachdem Sie die überflüssigen Vorlagen entfernt und die ItemTemplate angepasst haben, sollten das deklarative Markup von FormView und ObjectDataSource in etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Abbildung 8 zeigt einen Screenshot, wenn Sie diese Seite über einen Browser anzeigen.

> [!NOTE]
> Zusätzlich zu FormView habe ich auch ein Hyperlink-Steuerelement oberhalb der FormView hinzugefügt, das den Benutzer wieder zur Liste der Kategorien (`CategoryListMaster.aspx`) führt. Sie können diesen Link auch an anderer Stelle platzieren oder ganz weglassen.

[![Kategorieinformationen werden nun oben auf der Seite angezeigt.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Abbildung 8**: Kategorieinformationen werden nun oben auf der Seite angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))

## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Schritt 5: Anzeigen einer Meldung, wenn keine Produkte zur ausgewählten Kategorie gehören

Auf der `CategoryListMaster.aspx` Seite werden alle Kategorien im System aufgelistet, unabhängig davon, ob zugeordnete Produkte vorhanden sind. Wenn ein Benutzer auf eine Kategorie ohne zugeordnete Produkte klickt, wird der DataList in `ProductsForCategoryDetails.aspx` nicht gerendert, weil die zugehörige Datenquelle keine Elemente enthält. Wie wir bereits in früheren Tutorials gesehen haben, bietet die GridView eine `EmptyDataText`-Eigenschaft, mit der eine Text Meldung angegeben werden kann, die angezeigt wird, wenn keine Datensätze in der Datenquelle vorhanden sind. Leider hat weder der DataList noch der Repeater eine solche Eigenschaft.

Um eine Meldung anzuzeigen, die den Benutzer darüber informiert, dass keine übereinstimmenden Produkte für die ausgewählte Kategorie vorhanden sind, müssen wir der Seite ein Label-Steuerelement hinzufügen, dessen `Text`-Eigenschaft der Meldung zugewiesen wird, dass keine übereinstimmenden Produkte vorhanden sind. Anschließend muss die `Visible`-Eigenschaft Programm gesteuert festgelegt werden, je nachdem, ob der DataList Elemente enthält.

Um dies zu erreichen, fügen Sie unter dem DataList eine Bezeichnung hinzu. Legen Sie die `ID`-Eigenschaft auf `NoProductsMessage` und deren `Text`-Eigenschaft auf "Es sind keine Produkte für die ausgewählte Kategorie vorhanden..." fest. Als nächstes müssen wir die `Visible`-Eigenschaft dieser Bezeichnung Programm gesteuert festlegen, je nachdem, ob Daten an den `ProductsInCategory` DataList gebunden wurden. Diese Zuweisung muss erfolgen, nachdem die Daten an den DataList gebunden wurden. Für GridView, DetailsView und FormView könnten wir einen Ereignishandler für das `DataBound`-Ereignis des Steuer Elements erstellen, das nach Abschluss der Datenbindung ausgelöst wird. Es ist jedoch weder der DataList noch der Repeater ein `DataBound` Ereignis verfügbar.

In diesem Beispiel können Sie die `Visible`-Eigenschaft der Bezeichnung im `Page_Load`-Ereignishandler zuweisen, da die Daten vor dem `Load` Ereignis der Seite dem DataList zugewiesen wurden. Dieser Ansatz funktioniert jedoch im Allgemeinen nicht, da die Daten von ObjectDataSource später im Lebenszyklus der Seite an den DataList gebunden werden können. Wenn die angezeigten Daten z. b. auf dem Wert in einem anderen Steuerelement basieren, z. b. Wenn Sie einen Master-/Detailbericht mit einer Dropdown List-Auflistung für die Master-Datensätze anzeigen, werden die Daten bis zur `PreRender` Phase im Lebenszyklus der Seite möglicherweise erst wieder an das datenweb-Steuerelement zurückgebunden.

Eine Lösung, die für alle Fälle funktioniert, besteht darin, die `Visible`-Eigenschaft `False` im `ItemDataBound` (oder `ItemCreated`)-Ereignishandler des DataList zuzuweisen, wenn ein Elementtyp `Item` oder `AlternatingItem`gebunden wird. In diesem Fall wissen wir, dass mindestens ein Datenelement in der Datenquelle vorhanden ist, und kann daher die `NoProductsMessage` Bezeichnung ausblenden. Zusätzlich zu diesem Ereignishandler benötigen wir auch einen Ereignishandler für das `DataBinding`-Ereignis des DataList, bei dem wir die `Visible`-Eigenschaft der Bezeichnung zum `True`initialisieren. Da das `DataBinding`-Ereignis vor den `ItemDataBound` Ereignissen ausgelöst wird, wird die Eigenschaft `Visible` der Bezeichnung zunächst auf `True`festgelegt. Wenn Datenelemente vorhanden sind, wird Sie jedoch auf `False`festgelegt. Der folgende Code implementiert diese Logik:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Alle Kategorien in der Northwind-Datenbank sind einem oder mehreren Produkten zugeordnet. Um dieses Feature zu testen, habe ich die Northwind-Datenbank für dieses Tutorial manuell angepasst und alle Produkte, die der produplizierungskategorie (`CategoryID` = 7) zugeordnet sind, der Kategorie "Seafood" (`CategoryID` = 8) neu zugewiesen. Dies kann über die Server-Explorer erreicht werden, indem Sie neue Abfrage auswählen und folgende `UPDATE` Anweisung verwenden:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Nachdem Sie die Datenbank entsprechend aktualisiert haben, kehren Sie zur Seite `CategoryListMaster.aspx` zurück, und klicken Sie auf den Link für die Erstellung. Da es keine Produkte mehr gibt, die zur Kategorie "Produktion" gehören, sollte das Feld "Es sind keine Produkte für die ausgewählte Kategorie vorhanden..." angezeigt werden. Meldung, wie in Abbildung 9 dargestellt.

[![eine Meldung angezeigt wird, wenn keine Produkte zur ausgewählten Kategorie gehören.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Abbildung 9**: eine Meldung wird angezeigt, wenn keine Produkte zur ausgewählten Kategorie gehören ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))

## <a name="summary"></a>Zusammenfassung

Während Master/Detail-Berichte sowohl die Master-als auch die Detaildaten Sätze auf einer einzelnen Seite anzeigen können, werden Sie in vielen Websites auf zwei Webseiten voneinander getrennt. In diesem Tutorial haben wir uns mit der Implementierung eines solchen Master/Detail-Berichts mit einem Repeater auf der "Master"-Webseite und den zugehörigen Produkten befasst, die auf der Seite "Details" aufgelistet sind. Jedes Listenelement in der Master Webseite enthielt einen Link zur Detailseite, die an den `CategoryID` Wert der Zeile weitergegeben wurde.

Auf der Detailseite, in der die Produkte für den angegebenen Lieferanten abgerufen wurden, wurde die `GetProductsByCategoryID(categoryID)`-Methode der `ProductsBLL` Klasse durchgeführt. Der *`categoryID`* Parameterwert wurde deklarativ mithilfe des `CategoryID` QueryString-Werts als Parameter Quelle angegeben. Wir haben auch erfahren, wie Sie Kategoriedetails auf der Detailseite mithilfe von FormView anzeigen und wie Sie eine Meldung anzeigen, wenn keine Produkte zur ausgewählten Kategorie gehören.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank...

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Führende Prüfer für dieses Tutorial waren Zack Jones und Liz shulok. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [Weiter](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
