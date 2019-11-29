---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Aufbauen eines benutzerdefinierten datenbankgesteuerten Site Übersichts AnbietersC#() | Microsoft-Dokumentation
author: rick-anderson
description: Der Standard-Site Übersichts Anbieter in ASP.NET 2,0 ruft seine Daten aus einer statischen XML-Datei ab. Der XML-basierte Anbieter eignet sich für viele kleine und mittlere siz...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: a3e27b37703b12c9796e8516f0d805aef1fdf8d8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637261"
---
# <a name="building-a-custom-database-driven-site-map-provider-c"></a>Erstellen eines benutzerdefinierten Anbieters für datenbankgesteuerte Siteübersichten (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) oder [PDF herunterladen](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Der Standard-Site Übersichts Anbieter in ASP.NET 2,0 ruft seine Daten aus einer statischen XML-Datei ab. Obwohl der XML-basierte Anbieter für viele kleine und mittelgroße Websites geeignet ist, benötigen größere Webanwendungen eine dynamischere Site Übersicht. In diesem Tutorial erstellen wir einen benutzerdefinierten Site Übersichts Anbieter, der seine Daten aus der Geschäftslogik Schicht abruft, die wiederum Daten aus der Datenbank abruft.

## <a name="introduction"></a>Einführung

Das ASP.NET 2,0 s Site Map-Feature ermöglicht es einem Seiten Entwickler, eine Website Zuordnung für Webanwendungen in einem persistenten Medium (z. b. in einer XML-Datei) zu definieren. Nachdem die Site Übersichts Daten definiert wurden, können Sie Programm gesteuert über die [`SiteMap`-Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx) im [`System.Web`-Namespace](https://msdn.microsoft.com/library/system.web.aspx) oder über eine Vielzahl von navigationweb Steuerelementen wie SiteMapPath-, Menü-und TreeView-Steuerelemente zugreifen. Das Site Übersichts System verwendet das [Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , sodass verschiedene Implementierungen der Site Map-Serialisierung erstellt und an eine Webanwendung angeschlossen werden können. Der standardmäßige Site Übersichts Anbieter, der mit ASP.NET 2,0 ausgeliefert wird, speichert die Site Übersichts Struktur in einer XML-Datei. Im Tutorial zu [Master Seiten und zur Website Navigation](../introduction/master-pages-and-site-navigation-cs.md) haben wir eine Datei namens `Web.sitemap` erstellt, die diese Struktur enthielt und deren XML-Code mit jedem neuen Tutorial-Abschnitt aktualisiert wurde.

Der standardmäßige XML-basierte Site Übersichts Anbieter funktioniert gut, wenn die Struktur der Site Map-Struktur Recht statisch ist, z. b. für diese Tutorials. In vielen Szenarien ist jedoch eine dynamischere Site Map erforderlich. Sehen Sie sich die in Abbildung 1 gezeigte Site Übersicht an, wobei jede Kategorie und jedes Produkt als Abschnitte in der Struktur der Website angezeigt wird. Wenn diese Site Übersicht angezeigt wird, können Sie durch das Aufrufen der Webseite, die dem Stamm Knoten entspricht, alle Kategorien auflisten, während beim Besuch einer bestimmten Kategorie auf der Webseite die Kategorie s Produkte aufgeführt werden und eine bestimmte Produkt-e-Seite angezeigt wird.

[![Sie die Kategorien und Produkte in der Struktur der Site Map-Struktur](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Abbildung 1**: die in der Struktur der Site Map-Struktur eingebundenen Kategorien und Produkte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))

Diese Kategorie-und produktbasierte Struktur kann in der `Web.sitemap`-Datei hart codiert werden, die Datei muss jedoch jedes Mal aktualisiert werden, wenn eine Kategorie oder ein Produkt hinzugefügt, entfernt oder umbenannt wurde. Folglich wird die Wartung der Site Übersicht erheblich vereinfacht, wenn die Struktur aus der Datenbank oder im Idealfall aus der Geschäftslogik Schicht der Anwendungsarchitektur abgerufen wurde. Auf diese Weise wird die Site Übersicht beim Hinzufügen, umbenennen oder Löschen von Produkten und Kategorien automatisch aktualisiert, um diese Änderungen widerzuspiegeln.

Da die ASP.NET 2,0 s-Site Map-Serialisierung über das Anbieter Modell erstellt wurde, können wir unseren eigenen benutzerdefinierten Site Übersichts Anbieter erstellen, der seine Daten aus einem alternativen Datenspeicher, z. b. der Datenbank oder der Architektur, abgreift. In diesem Tutorial erstellen wir einen benutzerdefinierten Anbieter, der seine Daten aus der BLL abruft. Legen Sie Los!

> [!NOTE]
> Der in diesem Tutorial erstellte benutzerdefinierte Site Übersichts Anbieter ist eng mit der Architektur und dem Datenmodell der Anwendung verknüpft. Jeff Prosise s [speichert Site Maps in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) und [der SQL Site Map Provider, den Sie](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) auf Artikel gewartet haben, untersuchen einen generalisierten Ansatz zum Speichern von Site Übersichts Daten in SQL Server.

## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Schritt 1: Erstellen der benutzerdefinierten Website Übersichts Anbieter-Webseiten

Bevor wir mit dem Erstellen eines benutzerdefinierten Site Übersichts Anbieters beginnen, können Sie zunächst die ASP.NET Seiten hinzufügen, die für dieses Tutorial erforderlich sind. Fügen Sie zunächst einen neuen Ordner mit dem Namen `SiteMapProvider`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Fügen Sie dem Ordner `App_Code` auch einen `CustomProviders` Unterordner hinzu.

![Fügen Sie die ASP.NET-Seiten für die Website Übersichts Lernprogramme hinzu.](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Abbildung 2**: Hinzufügen der ASP.NET-Seiten für die Site Map-Anbieter bezogenen Tutorials

Da nur ein Lernprogramm für diesen Abschnitt vorhanden ist, müssen wir nicht `Default.aspx`, die Lernprogramme des Abschnitts aufzulisten. Stattdessen werden `Default.aspx` die Kategorien in einem GridView-Steuerelement anzeigen. Dies wird in Schritt 2 behandelt.

Aktualisieren Sie dann `Web.sitemap`, um einen Verweis auf die `Default.aspx` Seite einzuschließen. Fügen Sie insbesondere nach dem Caching-`<siteMapNode>`folgendes Markup hinzu:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt ein Element für das einzige Tutorial für Site Übersichts Anbieter.

![Die Site Übersicht enthält jetzt einen Eintrag für das Tutorial Site Übersichts Anbieter.](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Abbildung 3**: die Site Übersicht enthält jetzt einen Eintrag für das Tutorial zum Site Map-Anbieter.

Dieses Tutorial konzentriert sich hauptsächlich auf das Erstellen eines benutzerdefinierten Site Übersichts Anbieters und das Konfigurieren einer Webanwendung für die Verwendung dieses Anbieters. Insbesondere erstellen wir einen Anbieter, der eine Site Übersicht zurückgibt, die einen Stamm Knoten zusammen mit einem Knoten für jede Kategorie und jedes Produkt enthält, wie in Abbildung 1 dargestellt. Im Allgemeinen kann jeder Knoten in der Site Übersicht eine URL angeben. Für unsere Site Map wird die URL des Stamm Knotens `~/SiteMapProvider/Default.aspx`, in der alle Kategorien in der Datenbank aufgeführt werden. Jeder Kategorieknoten in der Site Übersicht verfügt über eine URL, die auf `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`verweist, in der alle Produkte in der angegebenen *CategoryID*aufgelistet werden. Schließlich verweist jeder Product Site Map-Knoten auf `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, in dem die Details des jeweiligen Produkts angezeigt werden.

Zunächst müssen die Seiten `Default.aspx`, `ProductsByCategory.aspx`und `ProductDetails.aspx` erstellt werden. Diese Seiten werden in den Schritten 2, 3 bzw. 4 abgeschlossen. Da es sich bei diesem Tutorial um Site Übersichts Anbieter handelt und in den letzten Tutorials das Erstellen dieser Arten von mehrseitigen Master-/Detailberichten erläutert wurde, werden die Schritte 2 bis 4 beschrieben. Wenn Sie ein Aktualisierungs Programm zum Erstellen von Master-/Detailberichten benötigen, das sich über mehrere Seiten erstreckt, lesen Sie das Tutorial zum [Filtern von Master/Details über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-cs.md) .

## <a name="step-2-displaying-a-list-of-categories"></a>Schritt 2: Anzeigen einer Liste von Kategorien

Öffnen Sie die Seite `Default.aspx` im Ordner `SiteMapProvider`, und ziehen Sie eine GridView aus der Toolbox auf den Designer, und legen Sie deren `ID` auf `Categories`fest. Binden Sie das GridView s-Smarttag an eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`, und konfigurieren Sie Sie so, dass Sie Ihre Daten mithilfe der `CategoriesBLL` Class s `GetCategories`-Methode abruft. Da in dieser GridView nur die Kategorien angezeigt werden und keine Daten Änderungs Funktionen bereitgestellt werden, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest.

[![die ObjectDataSource so konfigurieren, dass Kategorien mithilfe der GetCategories-Methode zurückgegeben werden.](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource zum Zurückgeben von Kategorien mit der `GetCategories`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Abbildung 5**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen wird von Visual Studio ein BoundField-Element für `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`und `BrochurePath`hinzugefügt. Bearbeiten Sie die GridView so, dass Sie nur die `CategoryName` und `Description` boundfields enthält, und aktualisieren Sie die Eigenschaft `CategoryName` BoundField s `HeaderText` auf Category.

Fügen Sie als nächstes ein HyperLinkField hinzu, und positionieren Sie es so, dass es das Feld ganz links ist. Legen Sie die `DataNavigateUrlFields`-Eigenschaft auf `CategoryID` und die `DataNavigateUrlFormatString`-Eigenschaft auf `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`fest. Legen Sie die `Text`-Eigenschaft auf Produkte anzeigen fest.

![Hinzufügen eines HyperLinkField zu den Kategorien GridView](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Abbildung 6**: Hinzufügen eines HyperLinkField zum `Categories` GridView

Nachdem Sie die ObjectDataSource erstellt und die GridView s-Felder angepasst haben, werden die beiden Steuerelemente deklaratives Markup wie folgt aussehen:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

Abbildung 7 zeigt `Default.aspx`, wenn Sie in einem Browser angezeigt werden. Wenn Sie auf den Link Produkte anzeigen klicken, gelangen Sie zu `ProductsByCategory.aspx?CategoryID=categoryID`, den wir in Schritt 3 erstellen werden.

[![jede Kategorie zusammen mit dem Link "Produkte anzeigen" aufgeführt ist.](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Abbildung 7**: jede Kategorie wird zusammen mit dem Link "Produkte anzeigen" aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))

## <a name="step-3-listing-the-selected-category-s-products"></a>Schritt 3: Auflisten der Produkte der ausgewählten Kategorie

Öffnen Sie die Seite "`ProductsByCategory.aspx`", und fügen Sie eine GridView-`ProductsByCategory`hinzu. Binden Sie das GridView-Objekt aus dem Smarttag an eine neue ObjectDataSource mit dem Namen `ProductsByCategoryDataSource`. Konfigurieren Sie die ObjectDataSource so, dass Sie die `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode verwendet, und legen Sie in den Registerkarten Update, INSERT und DELETE die Dropdown Liste auf (None) fest.

[![die Methode "getproductbycategoryid" (CategoryID) der productbll-Klasse verwenden](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Abbildung 8**: Verwenden der `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))

Im letzten Schritt des Assistenten zum Konfigurieren von Datenquellen werden Sie aufgefordert, eine Parameter Quelle für *CategoryID*einzugeben. Da diese Informationen über das QueryString-Feld `CategoryID`übergeben werden, wählen Sie in der Dropdown Liste die Option QueryString aus, und geben Sie im Textfeld QueryStringField den Wert CategoryID ein, wie in Abbildung 9 dargestellt. Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen.

[![das Feld CategoryID QueryString für den CategoryID-Parameter verwenden.](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Abbildung 9**: Verwenden des `CategoryID` QueryString-Felds für den *CategoryID* -Parameter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))

Nachdem Sie den Assistenten abgeschlossen haben, fügt Visual Studio die entsprechenden boundfields und ein CheckBoxField der GridView für die Product Data-Felder hinzu. Entfernen Sie alle außer die `ProductName`, `UnitPrice`und `SupplierName` boundfields. Passen Sie diese drei boundfields-`HeaderText` Eigenschaften an, um Product, Price und Supplier zu lesen. Formatieren Sie das `UnitPrice` BoundField als Währung.

Fügen Sie als nächstes ein HyperLinkField hinzu, und verschieben Sie es an die linke Position. Legen Sie die `Text`-Eigenschaft auf Details anzeigen, die `DataNavigateUrlFields`-Eigenschaft auf `ProductID`und deren `DataNavigateUrlFormatString`-Eigenschaft auf `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.

![Fügen Sie ein HyperLinkField für Sicht Details hinzu, das auf ProductDetails. aspx zeigt.](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Abbildung 10**: Hinzufügen eines HyperLinkField für Sicht Details, das auf `ProductDetails.aspx`

Nachdem Sie diese Anpassungen vorgenommen haben, sollten das deklarative Markup der GridView-und ObjectDataSource-Tabelle den folgenden ähneln:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Wechseln Sie zurück zum Anzeigen `Default.aspx` über einen Browser, und klicken Sie auf den Link Produkte anzeigen für Getränke. Auf diese Weise können Sie `ProductsByCategory.aspx?CategoryID=1`, die Namen, Preise und Lieferanten der Produkte in der Northwind-Datenbank anzeigen, die zur Kategorie Getränke gehören (siehe Abbildung 11). Sie können diese Seite weiter verbessern, um einen Link zum Zurückgeben der Benutzer zur kategorieauflistungs Seite (`Default.aspx`) und zum DetailsView-oder FormView-Steuerelement hinzufügen, das den Namen und die Beschreibung der ausgewählten Kategorie anzeigt.

[![werden die Namen, Preise und Lieferanten der Getränke angezeigt.](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Abbildung 11**: die Namen, Preise und Lieferanten der Getränke werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))

## <a name="step-4-showing-a-product-s-details"></a>Schritt 4: Anzeigen der Details eines Produkts

Auf der letzten Seite, `ProductDetails.aspx`, werden die Details der ausgewählten Produkte angezeigt. Öffnen Sie `ProductDetails.aspx`, und ziehen Sie eine DetailsView aus der Toolbox auf den Designer. Legen Sie die Eigenschaft DetailsView s `ID` auf `ProductInfo` fest, und löschen Sie deren `Height` und `Width` Eigenschaftswerte. Binden Sie im smarttagtag die DetailsView an eine neue ObjectDataSource mit dem Namen `ProductDataSource`, und konfigurieren Sie die ObjectDataSource, um Ihre Daten aus der `ProductsBLL` Class s `GetProductByProductID(productID)`-Methode abzurufen. Legen Sie wie bei den vorherigen Webseiten, die in den Schritten 2 und 3 erstellt wurden, die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest.

[![Konfigurieren von ObjectDataSource für die Verwendung der getproductbyproductid (ProductID)-Methode](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Abbildung 12**: Konfigurieren von ObjectDataSource für die Verwendung der `GetProductByProductID(productID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))

Im letzten Schritt des Assistenten zum Konfigurieren von Datenquellen werden Sie aufgefordert, die Quelle des *ProductID-* Parameters einzugeben. Da diese Daten das QueryString-Feld `ProductID`durchlaufen, legen Sie die Dropdown Liste auf QueryString und das Textfeld QueryStringField auf ProductID fest. Klicken Sie abschließend auf die Schaltfläche Fertigstellen, um den Assistenten abzuschließen.

[![den ProductID-Parameter so konfigurieren, dass der Wert aus dem Feld "ProductID QueryString" abgerufen wird.](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Abbildung 13**: Konfigurieren des *ProductID-* Parameters zum Abrufen seines Werts aus dem Feld "`ProductID` QueryString" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen erstellt Visual Studio entsprechende boundfields und ein CheckBoxField in der DetailsView für die Product Data-Felder. Entfernen Sie die `ProductID`, `SupplierID`und `CategoryID` boundfields, und konfigurieren Sie die restlichen Felder wie angezeigt. Nach einigen ästhetischen Konfigurationen hat mein deklaratives Markup "DetailsView" und "ObjectDataSource s" wie folgt ausgesehen:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Kehren Sie zum Testen dieser Seite zu `Default.aspx` zurück, und klicken Sie auf Produkte für die Kategorie Getränke anzeigen. Klicken Sie in der Liste der Getränkeprodukte auf den Link Details anzeigen für Chai-Tea. Dadurch gelangen Sie zu `ProductDetails.aspx?ProductID=1`, in der die Details von Chai Tea angezeigt werden (siehe Abbildung 14).

[![der Hersteller, die Kategorie, den Preis und weitere Informationen Chai-Tea werden angezeigt.](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Abbildung 14**: der Hersteller, die Kategorie, der Preis und weitere Informationen von Chai Tea s werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))

## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Schritt 5: Grundlegendes zur inneren Funktionsweise eines Site Übersichts Anbieters

Die Site Map wird im Arbeitsspeicher des Webservers als Auflistung von `SiteMapNode` Instanzen dargestellt, die eine Hierarchie bilden. Es muss genau ein Stamm vorhanden sein, alle nicht Stamm Knoten müssen über einen übergeordneten Knoten verfügen, und alle Knoten verfügen möglicherweise über eine beliebige Anzahl von untergeordneten Knoten. Jedes `SiteMapNode` Objekt stellt einen Abschnitt in der Struktur der Website dar. in diesen Abschnitten wird häufig eine entsprechende Webseite angezeigt. Folglich hat die [`SiteMapNode`-Klasse](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) Eigenschaften wie `Title`, `Url`und `Description`, die Informationen für den Abschnitt bereitstellen, der von der `SiteMapNode` repräsentiert wird. Außerdem gibt es eine `Key` Eigenschaft, die jede `SiteMapNode` in der Hierarchie eindeutig identifiziert, sowie Eigenschaften, mit denen diese Hierarchie `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`usw. festgelegt wird.

Abbildung 15 zeigt die allgemeine Site Übersichts Struktur aus Abbildung 1, aber mit den Implementierungsdetails, die detaillierter dargestellt werden.

[![jeder SiteMapNode Eigenschaften wie "Title", "URL", "Key" usw.](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Abbildung 15**: jede `SiteMapNode` hat Eigenschaften wie `Title`, `Url`, `Key`usw. ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))

Der Zugriff auf die Site Map ist über die [`SiteMap`-Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx) im [`System.Web`-Namespace](https://msdn.microsoft.com/library/system.web.aspx)möglich. Diese Class s `RootNode`-Eigenschaft gibt die Stamm `SiteMapNode` Instanz der Site Map s zurück. `CurrentNode` gibt das `SiteMapNode` zurück, dessen `Url`-Eigenschaft mit der URL der aktuell angeforderten Seite übereinstimmt. Diese Klasse wird intern von ASP.NET 2,0 s-Navigations-websteuer Elementen verwendet.

Wenn auf die Eigenschaften der `SiteMap`-Klasse zugegriffen wird, muss die Site Map-Struktur von einem persistenten Medium in den Arbeitsspeicher serialisiert werden. Die Serialisierungslogik der Site Map ist jedoch nicht in der `SiteMap`-Klasse hart codiert. Stattdessen bestimmt die `SiteMap`-Klasse zur Laufzeit, welcher Site Übersichts *Anbieter* für die Serialisierung verwendet werden soll. Standardmäßig wird die [`XmlSiteMapProvider`-Klasse](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) verwendet, die die Struktur der Site Map-Struktur aus einer ordnungsgemäß formatierten XML-Datei liest. Allerdings können wir mit etwas Arbeit einen eigenen benutzerdefinierten Site Übersichts Anbieter erstellen.

Alle Site Übersichts Anbieter müssen von der`SiteMapProvider`- [Klasse](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx)abgeleitet werden, die die grundlegenden Methoden und Eigenschaften enthält, die für Site Übersichts Anbieter erforderlich sind, aber viele der Implementierungsdetails ausgelassen werden. Eine zweite Klasse, [`StaticSiteMapProvider`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), erweitert die `SiteMapProvider`-Klasse und enthält eine robustere Implementierung der erforderlichen Funktionalität. Intern speichert der `StaticSiteMapProvider` die `SiteMapNode` Instanzen der Site Zuordnung in einem `Hashtable` und stellt Methoden wie `AddNode(child, parent)`, `RemoveNode(siteMapNode),` und `Clear()` bereit, die `SiteMapNode` s dem internen `Hashtable`hinzufügen bzw. daraus entfernen. `XmlSiteMapProvider` wird von `StaticSiteMapProvider` abgeleitet.

Beim Erstellen eines benutzerdefinierten Site Übersichts Anbieters, der `StaticSiteMapProvider`erweitert, gibt es zwei abstrakte Methoden, die überschrieben werden müssen: [`BuildSiteMap`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) und [`GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, wie der Name schon sagt, ist dafür verantwortlich, die Site Übersichts Struktur aus dem permanenten Speicher zu laden und im Arbeitsspeicher zu erstellen. `GetRootNodeCore` gibt den Stamm Knoten in der Site Übersicht zurück.

Bevor eine Webanwendung einen Site Übersichts Anbieter verwenden kann, muss Sie in der Konfiguration der Anwendung registriert werden. Standardmäßig wird die `XmlSiteMapProvider`-Klasse mit dem Namen `AspNetXmlSiteMapProvider`registriert. Fügen Sie zum Registrieren zusätzlicher Site Übersichts Anbieter `Web.config`das folgende Markup hinzu:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

Der *Name* -Wert weist dem Anbieter einen lesbaren Namen zu, während *Type* den voll qualifizierten Typnamen des Site Übersichts Anbieters angibt. Wir untersuchen konkrete Werte für den *Namen* und die *Typwerte* in Schritt 7, nachdem wir unseren benutzerdefinierten Site Übersichts Anbieter erstellt haben.

Die Site Map-Anbieter Klasse wird beim ersten Zugriff von der `SiteMap`-Klasse instanziiert und bleibt für die Lebensdauer der Webanwendung im Arbeitsspeicher. Da nur eine Instanz des Site Übersichts Anbieters vorhanden ist, die von mehreren parallelen Besucher der Website aufgerufen werden kann, ist es zwingend erforderlich, dass die Methoden des Anbieters *Thread sicher*sind.

Aus Gründen der Leistung und Skalierbarkeit ist es wichtig, dass wir die in-Memory-Site Übersichts Struktur Zwischenspeichern und diese zwischengespeicherte Struktur zurückgeben, anstatt Sie jedes Mal neu zu erstellen, wenn die `BuildSiteMap`-Methode aufgerufen wird. `BuildSiteMap` kann je nach den auf der Seite verwendeten Navigations Steuerelementen und der Tiefe der Site Übersichts Struktur mehrmals pro Seiten Anforderung pro Benutzer aufgerufen werden. Wenn wir die Site Map-`BuildSiteMap` Struktur in jedem Fall nicht zwischenspeichern, müssen wir jedes Mal, wenn Sie aufgerufen wird, die Produkt-und Kategorieinformationen aus der Architektur erneut abrufen (was zu einer Abfrage der Datenbank führen würde). Wie in den vorherigen Tutorials zum Zwischenspeichern erläutert, können zwischengespeicherte Daten veraltet werden. Um dies zu bekämpfen, können wir entweder Zeit-oder SQL-Cache-Abhängigkeits basierte Abläufe verwenden.

> [!NOTE]
> Ein Site Übersichts Anbieter kann optional die [`Initialize` Methode](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx)überschreiben. `Initialize` wird aufgerufen, wenn der Site Übersichts Anbieter zum ersten Mal instanziiert wird und alle benutzerdefinierten Attribute, die dem Anbieter zugewiesen sind, in `Web.config` im `<add>`-Element wie `<add name="name" type="type" customAttribute="value" />`zugewiesen werden. Es ist hilfreich, wenn Sie es einem Seiten Entwickler gestatten möchten, verschiedene Einstellungen für das Site Map-Anbieter anzugeben, ohne den Anbieter-e-Code ändern zu müssen. Wenn wir z. b. die Kategorie-und Produktdaten direkt aus der Datenbank lesen und nicht über die Architektur, möchten wir wahrscheinlich, dass der Seiten Entwickler die Daten bankverbindungs Zeichenfolge über `Web.config` anstatt mithilfe eines hart codierten Werts im e-Code des Anbieters angeben soll. Der benutzerdefinierte Site Übersichts Anbieter, den wir in Schritt 6 erstellen, setzt diese `Initialize` Methode nicht außer Kraft. Ein Beispiel für die Verwendung der `Initialize`-Methode finden Sie unter [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [Speichern von Site Maps in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) Artikel.

## <a name="step-6-creating-the-custom-site-map-provider"></a>Schritt 6: Erstellen des benutzerdefinierten Site Übersichts Anbieters

Zum Erstellen eines benutzerdefinierten Site Übersichts Anbieters, der die Site Übersicht aus den Kategorien und Produkten in der Northwind-Datenbank erstellt, muss eine Klasse erstellt werden, die `StaticSiteMapProvider`erweitert. In Schritt 1 haben Sie aufgefordert, im Ordner "`App_Code`" einen `CustomProviders` Ordner hinzuzufügen. Fügen Sie diesem Ordner `NorthwindSiteMapProvider`eine neue Klasse hinzu. Fügen Sie der `NorthwindSiteMapProvider`-Klasse folgenden Code hinzu:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Beginnen Sie mit der Untersuchung dieser Class s `BuildSiteMap`-Methode, die mit einer [`lock`-Anweisung](https://msdn.microsoft.com/library/c5kehkcz.aspx)beginnt. Mit der `lock`-Anweisung können jeweils nur jeweils ein Thread eingegeben werden. auf diese Weise wird der Zugriff auf den Code serialisiert, und es wird verhindert, dass zwei gleichzeitige Threads aufeinander umtraten.

Die `SiteMapNode` Variable auf Klassenebene wird zum Zwischenspeichern der Site Übersichts Struktur verwendet `root`. Wenn die Site Übersicht zum ersten Mal erstellt wird, oder zum ersten Mal, nachdem die zugrunde liegenden Daten geändert wurden, werden `root` `null`, und die Struktur der Site Struktur wird erstellt. Der Stamm Knoten der Site Übersicht wird `root` während des Erstellungs Prozesses zugewiesen, sodass `root` nicht `null`werden, wenn diese Methode das nächste Mal aufgerufen wird. Folglich wird `root` nicht `null` die Struktur der Site Map an den Aufrufer zurückgegeben, ohne Sie neu erstellen zu müssen.

Wenn root `null`ist, wird die Site Übersichts Struktur aus den Produkt-und Kategorieinformationen erstellt. Die Site Map wird erstellt, indem die `SiteMapNode` Instanzen erstellt und dann die Hierarchie durch Aufrufe der `AddNode`-Methode der `StaticSiteMapProvider`-Klasse gebildet wird. `AddNode` führt die interne Buchführung durch, wobei die verschiedenen `SiteMapNode` Instanzen in einem `Hashtable`gespeichert werden. Bevor wir mit dem Erstellen der Hierarchie beginnen, rufen wir zunächst die `Clear`-Methode auf, die die Elemente aus dem internen `Hashtable`löscht. Als nächstes werden die `ProductsBLL`-Klasse `GetProducts`-Methode und die resultierenden `ProductsDataTable` in lokalen Variablen gespeichert.

Die Erstellung der Site Map-s beginnt mit dem Erstellen des Stamm Knotens und dem zuweisen zu `root`. Der Überladung des [`SiteMapNode` s-Konstruktors](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) , der hier und in dieser `BuildSiteMap` verwendet wird, werden die folgenden Informationen übergeben:

- Ein Verweis auf den Site Übersichts Anbieter (`this`).
- Der `SiteMapNode` s `Key`. Dieser erforderliche Wert muss für jede `SiteMapNode`eindeutig sein.
- Der `SiteMapNode` s `Url`. `Url` ist optional, aber wenn bereitgestellt, muss jeder `SiteMapNode` s `Url` Wert eindeutig sein.
- Der `SiteMapNode` s `Title`, der erforderlich ist.

Der `AddNode(root)`-Methodenaufrufe fügt der Site Map die `SiteMapNode` `root` als Stamm hinzu. Im nächsten Schritt wird jede `ProductRow` in der `ProductsDataTable` aufgelistet. Wenn bereits eine `SiteMapNode` für die aktuelle Product s-Kategorie vorhanden ist, wird darauf verwiesen. Andernfalls wird eine neue `SiteMapNode` für die Kategorie erstellt und mithilfe des `AddNode(categoryNode, root)` Methoden Aufrufes als untergeordnetes Element der `SiteMapNode``root` hinzugefügt. Nachdem die passende Kategorie `SiteMapNode` Knoten gefunden oder erstellt wurde, wird ein `SiteMapNode` für das aktuelle Produkt erstellt und über `AddNode(productNode, categoryNode)`als untergeordnetes Element der Kategorie `SiteMapNode` hinzugefügt. Beachten Sie, dass die Kategorie `SiteMapNode` s `Url`-Eigenschafts Wert `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` ist, während die Product `SiteMapNode` s `Url`-Eigenschaft `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`zugewiesen wird.

> [!NOTE]
> Produkte, die über einen Daten Bank `NULL` Wert für Ihre `CategoryID` verfügen, werden unter einer Kategorie gruppiert `SiteMapNode` deren `Title`-Eigenschaft auf None festgelegt ist und deren `Url`-Eigenschaft auf eine leere Zeichenfolge festgelegt ist. Ich habe mich entschieden, `Url` auf eine leere Zeichenfolge festzulegen, da die `ProductBLL` Class s `GetProductsByCategory(categoryID)`-Methode derzeit nicht die Möglichkeit hat, nur diese Produkte mit einem `NULL` `CategoryID` Wert zurückzugeben. Außerdem wollte ich veranschaulichen, wie die Navigations Steuerelemente einen `SiteMapNode` Rendering, der einen Wert für die `Url`-Eigenschaft nicht enthält. Ich empfehle Ihnen, dieses Tutorial so zu erweitern, dass die Eigenschaft keine `SiteMapNode` s `Url` auf `ProductsByCategory.aspx`verweist, aber nur die Produkte mit `NULL` `CategoryID` Werte anzeigt.

Nach dem Erstellen der Site Übersicht wird dem Daten Cache ein beliebiges Objekt mit einer SQL-Cache Abhängigkeit vom `Categories` und `Products` Tabellen über ein `AggregateCacheDependency` Objekt hinzugefügt. Wir haben die Verwendung von SQL-Cache Abhängigkeiten im vorherigen Tutorial unter *Verwendung von SQL-Cache Abhängigkeiten*untersucht. Der benutzerdefinierte Site Übersichts Anbieter verwendet jedoch eine Überladung der Data Cache s-`Insert` Methode, die wir noch untersuchen können. Diese Überladung akzeptiert als letzten Eingabeparameter einen Delegaten, der aufgerufen wird, wenn das Objekt aus dem Cache entfernt wird. Insbesondere übergeben wir einen neuen [`CacheItemRemovedCallback`](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) Delegaten, der auf die `OnSiteMapChanged` Methode verweist, die weiter unten in der `NorthwindSiteMapProvider`-Klasse definiert ist.

> [!NOTE]
> Die Speicher interne Darstellung der Site Map wird durch die Variable auf Klassenebene `root`zwischengespeichert. Da es nur eine Instanz der benutzerdefinierten Site Übersichts Anbieter-Klasse gibt und diese Instanz von allen Threads in der Webanwendung gemeinsam genutzt wird, dient diese Klassen Variable als Cache. Die `BuildSiteMap`-Methode verwendet auch den Daten Cache, aber nur als Mittel zum Empfangen von Benachrichtigungen, wenn sich die zugrunde liegenden Datenbankdaten in der `Categories`-oder `Products` Tabellen ändern. Beachten Sie, dass der Wert, der im Daten Cache abgelegt wird, nur das aktuelle Datum und die aktuelle Uhrzeit ist. Die tatsächlichen Site Übersichts Daten werden *nicht* in den Daten Cache eingefügt.

Die `BuildSiteMap`-Methode wird durch Zurückgeben des Stamm Knotens der Site Übersicht abgeschlossen.

Die übrigen Methoden sind recht unkompliziert. `GetRootNodeCore` ist für die Rückgabe des Stamm Knotens verantwortlich. Da `BuildSiteMap` den Stamm zurückgibt, gibt `GetRootNodeCore` einfach `BuildSiteMap` s-Rückgabewert zurück. Die `OnSiteMapChanged`-Methode legt `root` auf `null` zurück, wenn das Cache Element entfernt wird. Wenn "root" auf "`null`" zurückgesetzt wird, wird die Site Map-Struktur beim nächsten Aufrufen von `BuildSiteMap` neu erstellt. Schließlich gibt die `CachedDate`-Eigenschaft den im Daten Cache gespeicherten Datums-und Uhrzeitwert zurück, wenn ein solcher Wert vorhanden ist. Diese Eigenschaft kann von einem Seiten Entwickler verwendet werden, um zu bestimmen, wann die Site Übersichts Daten zuletzt zwischengespeichert wurden.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Schritt 7: Registrieren des`NorthwindSiteMapProvider`

Damit die Webanwendung den in Schritt 6 erstellten `NorthwindSiteMapProvider` Site Übersichts Anbieter verwenden kann, müssen wir Sie im `<siteMap>` Abschnitt von `Web.config`registrieren. Fügen Sie in `Web.config`insbesondere das folgende Markup innerhalb des `<system.web>`-Elements hinzu:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Dieses Markup bewirkt zwei Dinge: Erstens gibt es an, dass die integrierte `AspNetXmlSiteMapProvider` der Standard-Site Übersichts Anbieter ist. Zweitens registriert Sie den benutzerdefinierten Site Übersichts Anbieter, der in Schritt 6 erstellt wurde, mit dem menschenfreundlichen Namen "Northwind".

> [!NOTE]
> Bei Site Übersichts Anbietern, die sich im Ordner "Application s `App_Code`" befinden, ist der Wert des `type` Attributs einfach der Klassenname. Alternativ kann der benutzerdefinierte Site Übersichts Anbieter in einem separaten Klassen Bibliotheksprojekt erstellt worden sein, in dem die kompilierte Assembly in das `/Bin` Verzeichnis der Webanwendung eingefügt wurde. In diesem Fall wäre der Wert des `type`-Attributs " *Namespace*". *ClassName*, *AssemblyName* .

Nehmen Sie sich nach dem Aktualisieren `Web.config`einen Moment Zeit, um eine beliebige Seite aus den Tutorials in einem Browser anzuzeigen. Beachten Sie, dass die Navigationsschnittstelle auf der linken Seite weiterhin die Abschnitte und Tutorials anzeigt, die in `Web.sitemap`definiert sind. Dies liegt daran, dass wir `AspNetXmlSiteMapProvider` als Standardanbieter belassen haben. Um ein Navigations Benutzeroberflächen Element zu erstellen, das die `NorthwindSiteMapProvider`verwendet, müssen wir explizit angeben, dass der Northwind Site Map-Anbieter verwendet werden soll. Dies wird in Schritt 8 erläutert.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Schritt 8: Anzeigen von Site Übersichts Informationen mithilfe des benutzerdefinierten Site Übersichts Anbieters

Nachdem Sie den benutzerdefinierten Site Übersichts Anbieter erstellt und in `Web.config`registriert haben, können Sie die Navigations Steuerelemente zu den `Default.aspx`-, `ProductsByCategory.aspx`-und `ProductDetails.aspx` Seiten im `SiteMapProvider`-Ordner hinzufügen. Öffnen Sie zunächst die Seite `Default.aspx`, und ziehen Sie eine `SiteMapPath` aus der Toolbox auf den Designer. Das SiteMapPath-Steuerelement befindet sich im Navigationsbereich der Toolbox.

[!["default. aspx" SiteMapPath hinzufügen](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Abbildung 16**: Hinzufügen eines SiteMapPath zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))

Das SiteMapPath-Steuerelement zeigt einen Breadcrumb-Wert an, der die Position der aktuellen Seite innerhalb der Site Übersicht angibt. Wir haben im Lernprogramm zu *Masterseiten und zur Website Navigation* am Anfang der Master Seite einen SiteMapPath hinzugefügt.

Nehmen Sie sich einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Der in Abbildung 16 hinzugefügte SiteMapPath verwendet den standardmäßigen Site Übersichts Anbieter und zieht seine Daten aus `Web.sitemap`. Der Breadcrumb-Code zeigt daher Start &gt; Anpassung der Site Map an, genau wie die Breadcrumb in der oberen rechten Ecke.

[![Breadcrumb den standardmäßigen Site Übersichts Anbieter verwendet.](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Abbildung 17**: Breadcrumb verwendet den Standard-Site Übersichts Anbieter ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))

Um SiteMapPath in Abbildung 16 hinzuzufügen, verwenden Sie den benutzerdefinierten Site Übersichts Anbieter, den Sie in Schritt 6 erstellt haben, legen Sie die [`SiteMapProvider`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) auf Northwind fest, den Namen, den Sie der `NorthwindSiteMapProvider` in `Web.config`zugewiesen haben. Leider verwendet der Designer weiterhin den standardmäßigen Site Übersichts Anbieter, aber wenn Sie die Seite über einen Browser besuchen, nachdem Sie diese Eigenschaft geändert haben, sehen Sie, dass der Breadcrumb jetzt den benutzerdefinierten Site Übersichts Anbieter verwendet.

[![Breadcrumb verwendet jetzt den benutzerdefinierten Site Übersichts Anbieter "northwindsitemapprovider".](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Abbildung 18**: Breadcrumb verwendet jetzt den benutzerdefinierten Site Übersichts Anbieter `NorthwindSiteMapProvider` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))

Das SiteMapPath-Steuerelement zeigt eine Funktions lichere Benutzeroberfläche auf den `ProductsByCategory.aspx`-und `ProductDetails.aspx` Seiten an. Fügen Sie diesen Seiten eine SiteMapPath-Eigenschaft hinzu, indem Sie die `SiteMapProvider`-Eigenschaft in Northwind festlegen. Klicken Sie in `Default.aspx` auf den Link Produkte anzeigen, und klicken Sie dann auf den Link Details anzeigen für Chai-Tea. Wie in Abbildung 19 gezeigt, enthält der Breadcrumb den aktuellen Site Übersichts Abschnitt (Chai Tea) und seine Vorgänger: Getränke und alle Kategorien.

[![Breadcrumb verwendet jetzt den benutzerdefinierten Site Übersichts Anbieter "northwindsitemapprovider".](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Abbildung 19**: Breadcrumb verwendet jetzt den benutzerdefinierten Site Übersichts Anbieter `NorthwindSiteMapProvider` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))

Neben SiteMapPath können auch andere Elemente der Navigations Benutzeroberfläche verwendet werden, z. b. das Menü und die TreeView-Steuerelemente. Die Seiten `Default.aspx`, `ProductsByCategory.aspx`und `ProductDetails.aspx` im Download für dieses Lernprogramm, z. b. alle Menü Steuerelemente enthalten (siehe Abbildung 20). Eine ausführlichere Betrachtung der Navigations Steuerelemente und des Site Übersichts Systems in ASP.NET 2,0 finden Sie unter unter [Suchen von ASP.NET 2,0 s-Website Navigations Features](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) und im Abschnitt [using Site Navigation Controls](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) des [ASP.NET 2,0-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/) .

[![das Menü Steuerelement listet die einzelnen Kategorien und Produkte auf.](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Abbildung 20**: das Menü Steuerelement listet die einzelnen Kategorien und Produkte auf ([Klicken Sie, um das Bild in voller Größe anzuzeigen](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))

Wie bereits weiter oben in diesem Tutorial erwähnt, kann auf die Site Map-Strukturprogramm gesteuert über die `SiteMap`-Klasse zugegriffen werden. Der folgende Code gibt die Stamm `SiteMapNode` des Standard Anbieters zurück:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Da die `AspNetXmlSiteMapProvider` der Standardanbieter für die Anwendung ist, würde der obige Code den in `Web.sitemap`definierten Stamm Knoten zurückgeben. Wenn Sie auf einen anderen Site Map-Anbieter als den Standard Verweis verweisen möchten, verwenden Sie die `SiteMap`-Klasse [`Providers`-Eigenschaft](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) wie folgt:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Dabei ist *Name* der Name des benutzerdefinierten Site Übersichts Anbieters (Northwind für die Webanwendung).

Um auf einen für einen Site Übersichts anbieterspezifischen Member zuzugreifen, verwenden Sie `SiteMap.Providers["name"]`, um die Anbieter Instanz abzurufen, und wandeln Sie Sie dann in den entsprechenden Typ um. Verwenden Sie z. b. den folgenden Code, um die `NorthwindSiteMapProvider` s `CachedDate`-Eigenschaft auf einer ASP.NET-Seite anzuzeigen:

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Stellen Sie sicher, dass Sie das SQL-Cache-Abhängigkeits Feature testen. Nachdem Sie die Seiten `Default.aspx`, `ProductsByCategory.aspx`und `ProductDetails.aspx` besucht haben, navigieren Sie zu einem der Tutorials im Abschnitt bearbeiten, einfügen und löschen, und bearbeiten Sie den Namen einer Kategorie oder eines Produkts. Kehren Sie dann zu einer der Seiten im `SiteMapProvider`-Ordner zurück. Wenn Sie davon ausgehen, dass genügend Zeit für den Abruf Mechanismus vergangen ist, um die Änderung an der zugrunde liegenden Datenbank zu notieren, sollte die Site Zuordnung aktualisiert werden, um den neuen Produkt-oder Kategorienamen anzuzeigen.

## <a name="summary"></a>Summary

ASP.NET 2,0 s Site Map-Features beinhalten eine `SiteMap` Klasse, eine Reihe integrierter websteuer Elemente für die Navigation und einen Standard-Site Übersichts Anbieter, der erwartet, dass die Site Übersichts Informationen in einer XML-Datei gespeichert werden. Um Site Übersichts Informationen aus einer anderen Quelle (z. b. aus einer Datenbank, der Anwendungsarchitektur oder einem Remoteweb Dienst) zu verwenden, müssen wir einen benutzerdefinierten Site Übersichts Anbieter erstellen. Dies umfasst das Erstellen einer Klasse, die direkt oder indirekt von der `SiteMapProvider` Klasse abgeleitet wird.

In diesem Tutorial wurde erläutert, wie Sie einen benutzerdefinierten Site Übersichts Anbieter erstellen, der die Site Übersicht auf der Grundlage der Produkt-und Kategorieinformationen auf Grundlage der Anwendungsarchitektur erstellt hat. Der Anbieter hat die `StaticSiteMapProvider` Klasse erweitert und eine `BuildSiteMap` Methode erstellt, mit der die Daten abgerufen, die Site Übersichts Hierarchie erstellt und die resultierende Struktur in einer Variablen auf Klassenebene zwischengespeichert wurden. Wir haben eine SQL-Cache Abhängigkeit mit einer Rückruffunktion verwendet, um die zwischengespeicherte Struktur ungültig zu machen, wenn die zugrunde liegenden `Categories` oder `Products` Daten geändert werden.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Speichern von Site Maps in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) und [dem SQL Site Map-Anbieter, auf den Sie gewartet](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) haben
- [Ein Blick auf das ASP.NET 2,0 s-Anbieter Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Untersuchen von ASP.NET 2,0 s-Website Navigations Features](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Dave Gardner, Zack Jones, Teresa Murphy und Bernadette Leigh. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](building-a-custom-database-driven-site-map-provider-vb.md)
