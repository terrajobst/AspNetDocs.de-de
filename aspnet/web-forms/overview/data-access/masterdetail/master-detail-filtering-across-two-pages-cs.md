---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Filtern von Master-/detailfiltern aufC#zwei Seiten () | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial implementieren wir dieses Muster mithilfe von GridView, um die Lieferanten in der Datenbank aufzulisten. Jede Lieferanten Zeile in der GridView enthält eine "vie...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: ccb3bfa5f215ba6e65b8a10b40041d5c2896c7e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78424875"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtern von Master-/Detailberichten über zwei Seiten (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) oder [PDF herunterladen](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> In diesem Tutorial implementieren wir dieses Muster mithilfe von GridView, um die Lieferanten in der Datenbank aufzulisten. Jede Lieferanten Zeile in der GridView enthält den Link Produkte anzeigen, bei dem der Benutzer auf eine separate Seite zeigt, auf der diese Produkte für den ausgewählten Lieferanten aufgelistet werden.

## <a name="introduction"></a>Einführung

In den beiden vorherigen Tutorials wurde erläutert, wie [Sie Master-/Detailberichte auf einer einzelnen Webseite anzeigen, indem Sie Dropdown Listen verwenden](master-detail-filtering-with-a-dropdownlist-cs.md) , um [die "Master"-Datensätze anzuzeigen, und ein GridView-oder DetailsView-Steuer](master-detail-filtering-with-two-dropdownlists-cs.md) Element, um die Details anzuzeigen. Ein weiteres häufiges Muster für Master-/Detailberichte ist, dass die Master Datensätze auf einer Webseite und die Details auf einer anderen Webseite angezeigt werden. Eine Forums Website, wie z. b. [die ASP.net-Foren](https://forums.asp.net/), ist ein gutes Beispiel für dieses Muster in der Praxis. Die ASP.net-Foren bestehen aus einer Vielzahl von Foren für die ersten Schritte, Web Forms, Steuerelemente für die Datenpräsentation usw. Jedes Forum besteht aus vielen Threads, und jeder Thread besteht aus einer Reihe von Beiträgen. Auf der ASP.NET Forums-Startseite sind die Foren aufgeführt. Wenn Sie auf ein Forum klicken, werden Sie zu einer `ShowForum.aspx` Seite angezeigt, auf der die Threads für das Forum aufgeführt sind. Ebenso gelangen Sie durch Klicken auf einen Thread `ShowPost.aspx`, in dem die Beiträge für den Thread angezeigt werden, auf den geklickt wurde.

In diesem Tutorial implementieren wir dieses Muster mithilfe von GridView, um die Lieferanten in der Datenbank aufzulisten. Jede Lieferanten Zeile in der GridView enthält den Link Produkte anzeigen, bei dem der Benutzer auf eine separate Seite zeigt, auf der diese Produkte für den ausgewählten Lieferanten aufgelistet werden.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Schritt 1: Hinzufügen von`SupplierListMaster.aspx`und`ProductsForSupplierDetails.aspx`Seiten zum`Filtering`Ordner

Wenn Sie das Seitenlayout im dritten Tutorial definieren, haben wir eine Reihe von "Starter"-Seiten in den Ordnern `BasicReporting`, `Filtering`und `CustomFormatting` hinzugefügt. Wir haben jedoch zu diesem Zeitpunkt keine Starter Seite für dieses Tutorial hinzugefügt. nehmen Sie sich also einen Moment Zeit, um dem Ordner "`Filtering`" zwei neue Seiten hinzuzufügen: `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx`. in `SupplierListMaster.aspx` werden die "Master"-Datensätze (die Lieferanten) aufgelistet, während `ProductsForSupplierDetails.aspx` die Produkte für den ausgewählten Lieferanten anzeigt.

Wenn Sie diese beiden neuen Seiten erstellen, müssen Sie Sie der `Site.master` Master Seite zuordnen.

![Fügen Sie die Seiten "supplierlistmaster. aspx" und "productforsupplierdetails. aspx" dem Filter Ordner hinzu.](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen der `SupplierListMaster.aspx`-und `ProductsForSupplierDetails.aspx` Seiten zum Ordner "`Filtering`"

Wenn Sie dem Projekt neue Seiten hinzufügen, achten Sie auch darauf, dass Sie die Site Übersichts Datei `Web.sitemap`entsprechend aktualisieren. Fügen Sie für dieses Tutorial der Site Zuordnung einfach die `SupplierListMaster.aspx` Seite mit dem folgenden XML-Inhalt als untergeordnetes Element des Filters "Filtering Reports `<siteMapNode>`" hinzu:

[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Sie können den Aktualisierungsprozess der Site Übersichts Datei beim Hinzufügen neuer ASP.NET-Seiten mithilfe [des Kosten](http://odetocode.com/Blogs/scott/)losen Visual Studio [Site Map-Makros](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)von Visual Studio automatisieren.

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Schritt 2: Anzeigen der Lieferantenliste in`SupplierListMaster.aspx`

Wenn die `SupplierListMaster.aspx`-und `ProductsForSupplierDetails.aspx` Seiten erstellt wurden, ist der nächste Schritt das Erstellen der GridView von Lieferanten in `SupplierListMaster.aspx`. Fügen Sie der Seite eine GridView hinzu, und binden Sie Sie an eine neue ObjectDataSource. Diese ObjectDataSource sollte die `GetSuppliers()`-Methode der `SuppliersBLL` Klasse verwenden, um alle Lieferanten zurückzugeben.

[![wählen Sie die suppliersbll-Klasse aus.](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Abbildung 2**: Auswählen der `SuppliersBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image4.png))

[![konfigurieren Sie ObjectDataSource für die Verwendung der getsuppliers ()-Methode.](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren von ObjectDataSource für die Verwendung der `GetSuppliers()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image7.png))

Wir müssen in jeder GridView-Zeile einen Link mit dem Namen "View Products" einschließen, der den Benutzer zum `ProductsForSupplierDetails.aspx` übergibt, indem er den `SupplierID` Wert der ausgewählten Zeile über "QueryString" übergibt. Wenn der Benutzer beispielsweise auf den Link Produkte anzeigen für den Händler "Tokyo Traders" (der den `SupplierID` Wert 4 hat) klickt, sollte er an `ProductsForSupplierDetails.aspx?SupplierID=4`gesendet werden.

Um dies zu erreichen, fügen Sie der GridView ein [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) hinzu, das jeder GridView-Zeile einen Hyperlink hinzufügt. Klicken Sie zunächst im Smarttags von GridView auf den Link Spalten bearbeiten. Wählen Sie als nächstes in der Liste oben links das HyperLinkField aus, und klicken Sie auf Hinzufügen, um das HyperLinkField in der Feldliste der GridView einzuschließen.

[![der GridView ein HyperLinkField hinzufügen](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Abbildung 4**: Hinzufügen eines HyperLinkField zur GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image10.png))

Das HyperLinkField kann so konfiguriert werden, dass die gleichen Text-oder URL-Werte in den einzelnen GridView-Zeilen verwendet werden, oder diese Werte können auf den Datenwerten basieren, die an die einzelnen Zeilen gebunden sind. Um einen statischen Wert für alle Zeilen anzugeben, verwenden Sie die Eigenschaften `Text` oder `NavigateUrl` des HyperLinkField. Da der Linktext für alle Zeilen identisch sein soll, legen Sie die `Text`-Eigenschaft des HyperLinkField auf "Produkte anzeigen" fest.

[![die Text-Eigenschaft von HyperLinkField auf "Produkte anzeigen" festlegen.](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Abbildung 5**: Festlegen der `Text`-Eigenschaft des HyperLinkField zum Anzeigen von Produkten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image13.png))

Um die Text-oder URL-Werte basierend auf den zugrunde liegenden Daten, die an die GridView-Zeile gebunden sind, festzulegen, geben Sie die Datenfelder an, aus denen die Text-oder URL-Werte in den Eigenschaften `DataTextField` oder `DataNavigateUrlFields` abgerufen werden sollen. `DataTextField` kann nur auf ein einzelnes Datenfeld festgelegt werden. `DataNavigateUrlFields`kann jedoch auf eine durch Trennzeichen getrennte Liste mit Datenfeldern festgelegt werden. Wir müssen häufig den Text oder die URL auf eine Kombination aus dem Daten Feldwert der aktuellen Zeile und einem statischen Markup basieren. In diesem Lernprogramm soll beispielsweise die URL der Links für den HyperLinkField `ProductsForSupplierDetails.aspx?SupplierID=supplierID`werden, wobei *`supplierID`* den `SupplierID` Wert jedes GridView-Werts ist. Beachten Sie, dass wir hier sowohl statische als auch datengesteuerte Werte benötigen: der `ProductsForSupplierDetails.aspx?SupplierID=` Teil der URL des Links ist statisch, während der *`supplierID`* Teil Daten gesteuert ist, da sein Wert den eigenen `SupplierID` Wert jeder Zeile ist.

Um eine Kombination aus statischen und datengesteuerten Werten anzugeben, verwenden Sie die Eigenschaften `DataTextFormatString` und `DataNavigateUrlFormatString`. Geben Sie in diese Eigenschaften das statische Markup nach Bedarf ein, und verwenden Sie dann den Marker-`{0}`, in dem der Wert des Felds angezeigt werden soll, das in den Eigenschaften `DataTextField` oder `DataNavigateUrlFields` angegeben wird. Wenn für die `DataNavigateUrlFields`-Eigenschaft mehrere Felder angegeben sind, verwenden Sie `{0}`, wo der erste Feldwert eingefügt werden soll, `{1}` für den zweiten Feldwert usw.

Wenn Sie dies auf unser Tutorial anwenden, müssen wir die `DataNavigateUrlFields`-Eigenschaft auf `SupplierID`festlegen, da dies das Datenfeld ist, dessen Wert auf Zeilen Basis angepasst werden muss, und die `DataNavigateUrlFormatString`-Eigenschaft für `ProductsForSupplierDetails.aspx?SupplierID={0}`.

[![das HyperLinkField so konfigurieren, dass es die richtige Link-URL basierend auf der SupplierID enthält.](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Abbildung 6**: Konfigurieren des HyperLinkField so, dass die richtige Link-URL auf Grundlage des `SupplierID` enthalten ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image16.png))

Nachdem Sie das HyperLinkField hinzugefügt haben, können Sie die Felder der GridView anpassen und neu anordnen. Das folgende Markup zeigt die GridView, nachdem ich einige kleinere Anpassungen auf Feldebene vorgenommen habe.

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Nehmen Sie sich einen Moment Zeit, um die `SupplierListMaster.aspx` Seite über einen Browser anzuzeigen. Wie in Abbildung 7 gezeigt, listet die Seite aktuell alle Lieferanten auf, einschließlich des Links "Produkte anzeigen". Wenn Sie auf den Link Produkte anzeigen klicken, gelangen Sie `ProductsForSupplierDetails.aspx`, indem Sie die `SupplierID` des Lieferanten in der Abfrage Zeichenfolge übergeben.

[![jede Lieferanten Zeile einen Link Produkte anzeigen enthält](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Abbildung 7**: jede Lieferanten Zeile enthält den Link Produkte anzeigen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Schritt 3: Auflisten der Produkte des Lieferanten in`ProductsForSupplierDetails.aspx`

An diesem Punkt sendet die `SupplierListMaster.aspx` Seite Benutzer an `ProductsForSupplierDetails.aspx`und übergibt die `SupplierID` des ausgewählten Lieferanten in der Abfrage Zeichenfolge. Der letzte Schritt des Tutorials besteht darin, die Produkte in einer GridView in `ProductsForSupplierDetails.aspx` anzuzeigen, deren `SupplierID` dem `SupplierID` entspricht, das durch die Abfrage Zeichenfolge übermittelt wurde. Um dies zu erreichen, fügen Sie der `ProductsForSupplierDetails.aspx` Seite eine GridView hinzu, indem Sie ein neues ObjectDataSource-Steuerelement mit dem Namen `ProductsBySupplierDataSource` verwenden, das die `GetProductsBySupplierID(supplierID)`-Methode aus der `ProductsBLL`-Klasse aufruft.

[![eine neue ObjectDataSource mit dem Namen productbysupplierdatasource hinzufügen.](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Abbildung 8**: Hinzufügen einer neuen ObjectDataSource mit dem Namen "`ProductsBySupplierDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image22.png))

[![wählen Sie die productbll-Klasse aus.](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Abbildung 9**: Auswählen der `ProductsBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image25.png))

[![von ObjectDataSource die Methode getproductbysupplierid (SupplierID) aufrufen](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Abbildung 10**: lassen Sie die `GetProductsBySupplierID(supplierID)` Methode von ObjectDataSource aufrufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image28.png))

Im letzten Schritt des Assistenten zum Konfigurieren von Datenquellen werden Sie aufgefordert, die Quelle des *`supplierID`* Parameters der `GetProductsBySupplierID(supplierID)` Methode anzugeben. Wenn Sie den Wert QueryString verwenden möchten, legen Sie die Parameter Quelle auf QueryString fest, und geben Sie den Namen des QueryString-Werts ein, der im Textfeld QueryStringField (`SupplierID`) verwendet werden soll.

[![den SupplierID-Parameter Wert mit dem SupplierID-QueryString-Wert Auffüllen](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Abbildung 11**: Auffüllen des *`supplierID`* Parameter Werts aus dem `SupplierID` QueryString-Wert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image31.png))

Das ist schon alles! In Abbildung 12 wird die `ProductsForSupplierDetails.aspx` Seite angezeigt, wenn Sie durch Klicken auf den Link "Tokyo Traders" von `SupplierListMaster.aspx`klicken.

[![werden die von Tokio Traders gelieferten Produkte angezeigt.](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Abbildung 12**: die von Tokyo Traders gelieferten Produkte werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Anzeigen von Lieferanteninformationen in`ProductsForSupplierDetails.aspx`

Wie in Abbildung 12 gezeigt, werden auf der Seite `ProductsForSupplierDetails.aspx` einfach die Produkte aufgelistet, die von der in der Abfrage Zeichenfolge angegebenen `SupplierID` bereitgestellt werden. Eine Person, die direkt an diese Seite gesendet wird, würde jedoch nicht wissen, dass Abbildung 12 die Produkte von Tokio Traders anzeigt. Um dieses Problem zu beheben, können wir Lieferanteninformationen auch auf dieser Seite anzeigen.

Beginnen Sie mit dem Hinzufügen einer FormView oberhalb der GridView-Produkte. Erstellen Sie ein neues ObjectDataSource-Steuerelement mit dem Namen `SuppliersDataSource`, das die `GetSupplierBySupplierID(supplierID)`-Methode der `SuppliersBLL` Klasse aufruft.

[![wählen Sie die suppliersbll-Klasse aus.](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Abbildung 13**: Auswählen der `SuppliersBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image37.png))

[![von ObjectDataSource die Methode getsupplierbysupplierid (SupplierID) aufrufen](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Abbildung 14**: lassen Sie die `GetSupplierBySupplierID(supplierID)` Methode von ObjectDataSource aufrufen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image40.png))

Legen Sie wie bei den `ProductsBySupplierDataSource`dem *`supplierID`* Parameter den Wert des `SupplierID` QueryString-Werts zugewiesen.

[![den SupplierID-Parameter Wert mit dem SupplierID-QueryString-Wert Auffüllen](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Abbildung 15**: Auffüllen des *`supplierID`* Parameter Werts aus dem `SupplierID` QueryString-Wert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image43.png))

Beim Binden von FormView an die ObjectDataSource in der Designansicht erstellt Visual Studio automatisch die `ItemTemplate`, `InsertItemTemplate`und `EditItemTemplate` von FormView mit Bezeichnungen und TextBox-websteuer Elementen für jedes der Datenfelder, die von ObjectDataSource zurückgegeben werden. Da wir nur Lieferanteninformationen anzeigen möchten, können Sie die `InsertItemTemplate` und `EditItemTemplate`entfernen. Bearbeiten Sie als nächstes die ItemTemplate, sodass Sie den Firmennamen des Lieferanten in einem `<h3>`-Element und die Adresse, die Stadt, das Land und die Telefonnummer unterhalb des Firmennamens anzeigt. Alternativ können Sie die `DataSourceID` von FormView manuell festlegen und das `ItemTemplate` Markup erstellen, wie im Abschnitt "[Anzeigen von Daten mit dem ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)"-Tutorial.

Nachdem diese Änderungen vorgenommen wurden, sollte das deklarative Markup von FormView in etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Abbildung 16 zeigt einen Screenshot der Seite "`ProductsForSupplierDetails.aspx`", nachdem die oben aufgeführten Lieferanteninformationen eingeschlossen wurden.

[![die Liste der Produkte eine Zusammenfassung des Lieferanten enthält.](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Abbildung 16**: die Produktliste enthält eine Übersicht über den Lieferanten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Anwenden der letzten Berührungen für die`ProductsForSupplierDetails.aspx`-Benutzeroberfläche

Um die Benutzer Leistung für diesen Bericht zu verbessern, gibt es einige Ergänzungen, die wir auf der Seite "`ProductsForSupplierDetails.aspx`" vornehmen sollten. Zurzeit kann ein Benutzer von der `ProductsForSupplierDetails.aspx` Seite zurück zur Liste der Lieferanten wechseln, indem er auf die Schaltfläche "zurück" des Browsers klickt. Fügen Sie der `ProductsForSupplierDetails.aspx` Seite ein Hyperlink-Steuerelement hinzu, das mit `SupplierListMaster.aspx`verknüpft ist, und bietet dem Benutzer eine weitere Möglichkeit, zur Masterliste zurückzukehren.

[![ein Hyperlink-Steuerelement hinzufügen, um den Benutzer zu supplierlistmaster. aspx zurückzukehren.](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Abbildung 17**: Hinzufügen eines Hyperlink-Steuer Elements, um den Benutzer wieder `SupplierListMaster.aspx` zu machen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image49.png))

Wenn der Benutzer auf den Link Produkte anzeigen für einen Lieferanten klickt, der keine Produkte enthält, gibt der `ProductsBySupplierDataSource` ObjectDataSource in `ProductsForSupplierDetails.aspx` keine Ergebnisse zurück. Die an ObjectDataSource gebundene GridView-Struktur führt kein Markup aus, das im Browser des Benutzers zu einem leeren Bereich auf der Seite führt. Um dem Benutzer deutlicher mitzuteilen, dass dem ausgewählten Lieferanten keine Produkte zugeordnet sind, können Sie die `EmptyDataText`-Eigenschaft der GridView auf die Meldung festlegen, die angezeigt werden soll, wenn eine solche Situation auftritt. Ich habe diese Eigenschaft auf "Es sind keine Produkte von diesem Lieferanten bereitgestellt" festgelegt.

Standardmäßig wird von allen Lieferanten in der Northwind-Datenbank mindestens ein Produkt bereitgestellt. In diesem Tutorial habe ich jedoch die `Products` Tabelle manuell so geändert, dass der Lieferant escargots nouveaux nicht mehr mit Produkten verknüpft ist. Abbildung 18 zeigt die Detailseite für escargots nouveaux, nachdem diese Änderung vorgenommen wurde.

[![Benutzern wird mitgeteilt, dass der Lieferant keine Produkte bereitstellt.](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Abbildung 18**: Benutzer werden darüber informiert, dass der Lieferant keine Produkte bereitstellt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-filtering-across-two-pages-cs/_static/image52.png))

## <a name="summary"></a>Zusammenfassung

Während Master/Detail-Berichte sowohl die Master-als auch die Detaildaten Sätze auf einer einzelnen Seite anzeigen können, werden Sie in vielen Websites auf zwei Webseiten voneinander getrennt. In diesem Tutorial haben wir uns mit der Implementierung eines solchen Master/Detail-Berichts beschäftigt, indem die Lieferanten in einer GridView auf der Webseite "Master" und die zugehörigen Produkte auf der Seite "Details" aufgeführt werden. Jede Lieferanten Zeile in der Master Webseite enthielt einen Link zur Detailseite, die an den `SupplierID` Wert der Zeile weitergegeben wurde. Solche Zeilen spezifischen Links können problemlos mithilfe des HyperLinkField von GridView hinzugefügt werden.

Auf der Detailseite, auf der die Produkte für den angegebenen Lieferanten abgerufen werden, wurde der `GetProductsBySupplierID(supplierID)` Methode der `ProductsBLL` Klasse aufgerufen. Der *`supplierID`* Parameterwert wurde deklarativ mithilfe von "QueryString" als Parameter Quelle angegeben. Wir haben auch erläutert, wie die Lieferanten Details auf der Detailseite mithilfe von FormView angezeigt werden.

Das [nächste Tutorial](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) ist das letzte in Master/Detail-Berichten. Wir sehen uns an, wie Sie eine Liste von Produkten in einer GridView anzeigen, in der jede Zeile über eine SELECT-Schaltfläche verfügt. Wenn Sie auf die Schaltfläche auswählen klicken, werden die Details dieses Produkts in einem DetailsView-Steuerelement auf derselben Seite angezeigt.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Prüfer für dieses Tutorial war Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Weiter](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
