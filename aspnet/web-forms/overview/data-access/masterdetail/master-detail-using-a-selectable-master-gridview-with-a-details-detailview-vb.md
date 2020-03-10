---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Master/Detail mithilfe eines auswählbaren Master-GridView-Detailansicht (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird eine GridView-Ansicht angezeigt, deren Zeilen den Namen und den Preis jedes Produkts zusammen mit der Schaltfläche auswählen enthalten. Klicken auf die Schaltfläche "auswählen" für eine partitioncu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: a953c00acc4c37fd563321477b6b21689d6e686c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78503487"
---
# <a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Master-/Detailbericht unter Verwendung eines auswählbaren GridView-Mastersteuerelements mit einem DetailView-Detailsteuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) oder [PDF herunterladen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> In diesem Tutorial wird eine GridView-Ansicht angezeigt, deren Zeilen den Namen und den Preis jedes Produkts zusammen mit der Schaltfläche auswählen enthalten. Wenn Sie auf die Schaltfläche auswählen für ein bestimmtes Produkt klicken, werden die vollständigen Details in einem DetailsView-Steuerelement auf derselben Seite angezeigt.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](master-detail-filtering-across-two-pages-vb.md) haben wir erläutert, wie Sie einen Master-/Detailbericht mit zwei Webseiten erstellen: einer "Master"-Webseite, auf der die Liste der Lieferanten angezeigt wird. und eine "Detail"-Webseite, die die vom ausgewählten Lieferanten bereitgestellten Produkte aufführte. Dieses Format für zwei Seiten Berichte kann auf eine Seite komprimiert werden. In diesem Tutorial wird eine GridView-Ansicht angezeigt, deren Zeilen den Namen und den Preis jedes Produkts zusammen mit der Schaltfläche auswählen enthalten. Wenn Sie auf die Schaltfläche auswählen für ein bestimmtes Produkt klicken, werden die vollständigen Details in einem DetailsView-Steuerelement auf derselben Seite angezeigt.

[Wenn Sie ![klicken auf die Schaltfläche auswählen, werden die Produkt Details angezeigt](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Abbildung 1**: Klicken auf die Schaltfläche "auswählen" zeigt die Produkt Details[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))

## <a name="step-1-creating-a-selectable-gridview"></a>Schritt 1: Erstellen einer auswählbaren GridView

Beachten Sie, dass im zweiseitigen Master/Detail-Bericht, dass jeder Master Datensatz einen Hyperlink enthielt, der den Benutzer auf die Detailseite gesendet hat, indem er den `SupplierID` Wert der angeklickten Zeile in der Abfrage Zeichenfolge übergibt. Ein solcher Hyperlink wurde jeder GridView-Zeile mithilfe eines HyperLinkField hinzugefügt. Für den Bericht mit einer einzelnen Seite Master/Details wird eine Schaltfläche für jede GridView-Zeile benötigt, bei der die Details angezeigt werden. Das GridView-Steuerelement kann so konfiguriert werden, dass es für jede Zeile, die ein Postback auslöst, eine SELECT-Schaltfläche einschließt und diese Zeile als [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)der GridView markiert.

Fügen Sie zunächst der Seite `DetailsBySelecting.aspx` im Ordner `Filtering` ein GridView-Steuerelement hinzu, und legen Sie dessen Eigenschaft `ID` auf `ProductsGrid`fest. Fügen Sie als nächstes eine neue ObjectDataSource mit dem Namen `AllProductsDataSource` hinzu, die die `GetProducts()`-Methode der `ProductsBLL` Klasse aufruft.

[![erstellen Sie eine ObjectDataSource mit dem Namen allproductdatasource.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Abbildung 2**: Erstellen einer ObjectDataSource mit dem Namen "`AllProductsDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))

[![verwenden der productbll-Klasse](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Abbildung 3**: Verwenden der `ProductsBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))

[![Konfigurieren von ObjectDataSource zum Aufrufen der GetProducts ()-Methode](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource zum Aufrufen der `GetProducts()` Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))

Bearbeiten Sie die Felder der GridView, wobei alle außer den `ProductName`-und `UnitPrice` boundfields-Einstellungen entfernt werden. Sie können diese boundfields auch nach Bedarf anpassen, wie z. b. das Formatieren des `UnitPrice` BoundField als Währung und das Ändern der `HeaderText` Eigenschaften von boundfields. Diese Schritte können grafisch durchgeführt werden, indem Sie auf den Link Spalten bearbeiten im Smarttag der GridView klicken oder die deklarative Syntax manuell konfigurieren.

[![alle außer den "ProductName"-und "UnitPrice boundfields" entfernen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Abbildung 5**: Entfernen aller außer der `ProductName` und `UnitPrice` boundfields ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))

Das endgültige Markup für die GridView lautet wie folgt:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Als nächstes müssen wir die GridView als wählbar markieren, sodass jeder Zeile eine SELECT-Schaltfläche hinzugefügt wird. Um dies zu erreichen, aktivieren Sie einfach das Kontrollkästchen Auswahl aktivieren im Smarttags von GridView.

[![die Zeilen der GridView auswählbar machen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Abbildung 6**: machen Sie die Zeilen der GridView auswählbar ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))

Wenn Sie die Option Auswahl aktivieren aktivieren, wird der `ProductsGrid` GridView ein CommandField hinzugefügt, dessen `ShowSelectButton`-Eigenschaft auf true festgelegt ist. Dies ergibt eine SELECT-Schaltfläche für jede Zeile der GridView, wie in Abbildung 6 veranschaulicht. Standardmäßig werden die SELECT-Schaltflächen als LinkButtons gerendert, aber Sie können stattdessen Schaltflächen oder imagebuttons über die `ButtonType`-Eigenschaft des CommandField verwenden.

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Wenn auf die Select-Schaltfläche einer GridView-Zeile geklickt wird, folgt ein Postback, und die `SelectedRow`-Eigenschaft der GridView wird aktualisiert. Zusätzlich zur `SelectedRow`-Eigenschaft stellt die GridView die Eigenschaften [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)und [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) bereit. Die `SelectedIndex`-Eigenschaft gibt den Index der ausgewählten Zeile zurück, während die Eigenschaften `SelectedValue` und `SelectedDataKey` Werte basierend auf der [DataKeyNames-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)der GridView zurückgeben.

Die `DataKeyNames`-Eigenschaft wird verwendet, um jeder Zeile einen oder mehrere Daten Feldwerte zuzuordnen, und wird häufig verwendet, um eindeutig identifizierende Informationen aus den zugrunde liegenden Daten mit jeder GridView-Zeile zuzuordnen. Die `SelectedValue`-Eigenschaft gibt den Wert des ersten `DataKeyNames` Datenfelds für die ausgewählte Zeile zurück, in der die `SelectedDataKey`-Eigenschaft das `DataKey` Objekt der ausgewählten Zeile zurückgibt, das alle Werte für die angegebenen Datenschlüssel Felder für diese Zeile enthält.

Die `DataKeyNames`-Eigenschaft wird automatisch auf die eindeutig identifizierenden Datenfelder festgelegt, wenn Sie eine Datenquelle an ein GridView-, DetailsView-oder FormView-Objekt über den Designer binden. Diese Eigenschaft wurde in den vorherigen Tutorials automatisch für uns festgelegt, aber die Beispiele hätten funktioniert, ohne dass die `DataKeyNames`-Eigenschaft angegeben wurde. Für die auswählbare GridView in diesem Tutorial sowie für zukünftige Tutorials, in denen das Einfügen, aktualisieren und löschen untersucht wird, muss die `DataKeyNames`-Eigenschaft jedoch ordnungsgemäß festgelegt werden. Nehmen Sie sich einen Moment Zeit, um sicherzustellen, dass die `DataKeyNames` Eigenschaft ihrer GridView auf `ProductID`festgelegt ist.

Sehen wir uns den Fortschritt bis zum Browser an. Beachten Sie, dass in der GridView der Name und der Preis für alle Produkte zusammen mit dem Link Button Select aufgeführt sind. Durch Klicken auf die Schaltfläche auswählen wird ein Postback verursacht. In Schritt 2 sehen Sie, wie eine DetailsView auf dieses Postback reagieren kann, indem Sie die Details für das ausgewählte Produktanzeigen.

[![jede Produkt Zeile eine LinkButton-Auswahl enthält.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Abbildung 7**: jede Produkt Zeile enthält einen LinkButton auswählen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))

## <a name="highlighting-the-selected-row"></a>Markieren der ausgewählten Zeile

Die `ProductsGrid` GridView verfügt über eine `SelectedRowStyle`-Eigenschaft, die verwendet werden kann, um den visuellen Stil für die ausgewählte Zeile festzulegen. Wird ordnungsgemäß verwendet. Dies kann die Benutzer Leistung verbessern, indem die aktuell ausgewählte Zeile der GridView deutlicher angezeigt wird. In diesem Tutorial soll die ausgewählte Zeile mit einem gelben Hintergrund hervorgehoben werden.

Wie bei unseren vorherigen Tutorials soll es uns ermöglichen, die ästhetisch-bezogenen Einstellungen als CSS-Klassen festzuhalten. Erstellen Sie daher eine neue CSS-Klasse in `Styles.css` mit dem Namen `SelectedRowStyle`.

[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Wenn Sie diese CSS-Klasse auf die `SelectedRowStyle`-Eigenschaft *aller* GridViews in der tutorialreihe anwenden möchten, bearbeiten Sie die `GridView.skin` Skin im `DataWebControls` Design, um die `SelectedRowStyle` Einstellungen wie unten dargestellt einzuschließen:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Mit dieser Addition wird die ausgewählte GridView-Zeile nun mit einer gelben Hintergrundfarbe hervorgehoben.

[![die Darstellung der ausgewählten Zeile mithilfe der SelectedRowStyle-Eigenschaft der GridView anpassen.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Abbildung 8**: Anpassen der Darstellung der ausgewählten Zeile mithilfe der `SelectedRowStyle`-Eigenschaft der GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))

## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Schritt 2: Anzeigen der Details des ausgewählten Produkts in einer DetailsView

Wenn die `ProductsGrid` GridView vollständig ist, muss nur noch eine DetailsView hinzugefügt werden, in der Informationen über das ausgewählte Produkt angezeigt werden. Fügen Sie oberhalb der GridView ein DetailsView-Steuerelement hinzu, und erstellen Sie eine neue ObjectDataSource mit dem Namen `ProductDetailsDataSource`. Da diese DetailsView bestimmte Informationen über das ausgewählte Produktanzeigen soll, müssen Sie die `ProductDetailsDataSource` so konfigurieren, dass die `GetProductByProductID(productID)`-Methode der `ProductsBLL` Klasse verwendet wird.

[![rufen Sie die getproductbyproductid (ProductID)-Methode der productbll-Klasse auf.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Abbildung 9**: Aufrufen der `GetProductByProductID(productID)` Methode der `ProductsBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))

Der Wert des *`productID`* Parameters, der aus der `SelectedValue`-Eigenschaft des GridView-Steuer Elements abgerufen wird. Wie bereits erläutert, gibt die `SelectedValue`-Eigenschaft von GridView den ersten Datenschlüssel Wert für die ausgewählte Zeile zurück. Daher ist es zwingend erforderlich, dass die `DataKeyNames`-Eigenschaft der GridView auf `ProductID`festgelegt ist, sodass der `ProductID` Wert der ausgewählten Zeile von `SelectedValue`zurückgegeben wird.

[![den ProductID-Parameter auf die SelectedValue-Eigenschaft der GridView festlegen.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Abbildung 10**: Festlegen des *`productID`* -Parameters auf die `SelectedValue`-Eigenschaft der GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))

Nachdem der `productDetailsDataSource` ObjectDataSource ordnungsgemäß konfiguriert und an die DetailsView gebunden wurde, ist dieses Tutorial abgeschlossen! Beim ersten Besuch der Seite wird keine Zeile ausgewählt, sodass die `SelectedValue`-Eigenschaft von GridView `Nothing`zurückgibt. Da keine Produkte mit einem `NULL` `ProductID`-Wert vorhanden sind, werden von der `GetProductByProductID(productID)`-Methode keine Datensätze zurückgegeben, was bedeutet, dass die DetailsView nicht angezeigt wird (siehe Abbildung 11). Wenn Sie auf die Schaltfläche Auswählen einer GridView-Zeile klicken, folgt ein Postback, und die DetailsView wird aktualisiert. Wenn die `SelectedValue`-Eigenschaft von GridView die `ProductID` der ausgewählten Zeile zurückgibt, gibt die `GetProductByProductID(productID)`-Methode eine `ProductsDataTable` mit Informationen zu diesem Produkt zurück, und die DetailsView zeigt diese Details an (siehe Abbildung 12).

[![beim ersten Besuch wird nur die GridView angezeigt.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Abbildung 11**: beim ersten Besuch wird nur die GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))

[![, wenn Sie eine Zeile auswählen, werden die Details des Produkts angezeigt.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Abbildung 12**: Wenn Sie eine Zeile auswählen, werden die Details des Produkts angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png)).

## <a name="summary"></a>Zusammenfassung

In diesem und den vorherigen drei Tutorials haben wir eine Reihe von Techniken zum Anzeigen von Master-/Detailberichten kennengelernt. In diesem Tutorial haben wir die Verwendung eines auswählbaren GridView-Tools für die Master Datensätze und eine DetailsView untersucht, um Details zum ausgewählten Master Daten Satz auf derselben Seite anzuzeigen. In den vorherigen Tutorials haben wir uns mit dem Anzeigen von Master-/Detailberichten mithilfe von Dropdown Listen und dem Anzeigen von Master Datensätzen auf einer Webseite und detaillierten Datensätzen auf einer anderen Seite beschäftigt.

In diesem Tutorial wird die Untersuchung von Master-/Detailberichten abgeschlossen. Beginnend mit dem nächsten Tutorial beginnen wir mit der Untersuchung der angepassten Formatierung mit GridView, DetailsView und FormView. Wir sehen uns an, wie Sie die Darstellung dieser Steuerelemente basierend auf den Daten, die an Sie gebunden sind, anpassen, wie Sie Daten in der Fußzeile der GridView zusammenfassen und wie Sie Vorlagen verwenden, um ein höheres Maß an Kontrolle über das Layout zu erhalten.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Prüfer für dieses Tutorial war Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Previous](master-detail-filtering-across-two-pages-vb.md)
