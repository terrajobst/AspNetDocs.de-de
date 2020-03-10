---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Anzeigen von Daten mit dem ObjectDataSource-Objekt (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Tutorial prüft das ObjectDataSource-Steuerelement mithilfe dieses Steuer Elements. Sie können Daten, die aus der im vorherigen Tutorial erstellten BLL abgerufen wurden, ohne HAVI binden...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 754188352cbfb08e610027f5b7890a32bd88ae26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483051"
---
# <a name="displaying-data-with-the-objectdatasource-vb"></a>Anzeigen von Daten mit dem ObjectDataSource-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) oder [PDF herunterladen](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Dieses Tutorial prüft das ObjectDataSource-Steuerelement mithilfe dieses Steuer Elements. Sie können Daten, die aus der im vorherigen Tutorial erstellten BLL abgerufen wurden, binden, ohne eine Codezeile schreiben zu müssen.

## <a name="introduction"></a>Einführung

Nachdem wir die Anwendungsarchitektur und das Layout der Website Seite abgeschlossen haben, können wir mit der Durchführung einer Vielzahl allgemeiner Daten-und Bericht Erstellungs Aufgaben beginnen. In den vorherigen Tutorials haben wir gesehen, wie Daten Programm gesteuert von der dal und der BLL an ein datenweb-Steuerelement auf einer ASP.NET Seite gebunden werden. Diese Syntax, die die `DataSource`-Eigenschaft des Data Web-Steuer Elements den anzuzeigenden Daten zuweist und dann die `DataBind()`-Methode des Steuer Elements aufzurufen, war das in ASP.NET 1. x-Anwendungen verwendete Muster und kann weiterhin in ihren 2,0-Anwendungen verwendet werden. Die neuen Datenquellen-Steuerelemente von ASP.NET 2.0 bieten jedoch eine deklarative Möglichkeit zum Arbeiten mit Daten. Mithilfe dieser Steuerelemente können Sie Daten binden, die aus der im vorherigen Tutorial erstellten BLL abgerufen wurden, ohne eine Codezeile schreiben zu müssen.

ASP.NET 2,0 umfasst fünf integrierte Datenquellen-Steuerelemente: [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)und [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) , obwohl Sie ggf. ihre eigenen [benutzerdefinierten Datenquellen](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp)-Steuerelemente erstellen können. Da wir eine Architektur für unsere tutorialanwendung entwickelt haben, verwenden wir "ObjectDataSource" für unsere BLL-Klassen.

![ASP.NET 2,0 enthält fünf integrierte Datenquellen-Steuerelemente.](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Abbildung 1**: ASP.NET 2,0 enthält fünf integrierte Datenquellen-Steuerelemente

ObjectDataSource dient als Proxy zum Arbeiten mit einem anderen Objekt. Zum Konfigurieren von ObjectDataSource geben wir dieses zugrunde liegende Objekt und die Zuordnung der Methoden zu den Methoden `Select`, `Insert`, `Update`und `Delete` von ObjectDataSource an. Nachdem dieses zugrunde liegende Objekt angegeben und seine Methoden der ObjectDataSource zugeordnet wurden, können wir die ObjectDataSource an ein datenweb Steuerelement binden. ASP.net wird mit vielen datenweb-Steuerelementen ausgeliefert, einschließlich GridView, DetailsView, RadioButton List und Dropdown List. Während des Lebenszyklus der Seite benötigt das datenweb Steuerelement möglicherweise Zugriff auf die Daten, an die es gebunden ist. Dies wird durch Aufrufen der `Select`-Methode von ObjectDataSource erreicht. Wenn das datenweb-Steuerelement das Einfügen, aktualisieren oder löschen unterstützt, können Aufrufe an die Methoden `Insert`, `Update`oder `Delete` von ObjectDataSource vorgenommen werden. Diese Aufrufe werden dann von ObjectDataSource an die entsprechenden Methoden des zugrunde liegenden Objekts weitergeleitet, wie im folgenden Diagramm veranschaulicht.

[![ObjectDataSource dient als Proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Abbildung 2**: ObjectDataSource dient als Proxy ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image4.png))

Obwohl die ObjectDataSource zum Aufrufen von Methoden zum Einfügen, aktualisieren oder Löschen von Daten verwendet werden kann, konzentrieren wir uns einfach auf die Rückgabe von Daten. in zukünftigen Tutorials wird die Verwendung von ObjectDataSource-und Data Web-Steuerelementen erläutert, mit denen Daten geändert werden.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Schritt 1: Hinzufügen und Konfigurieren des ObjectDataSource-Steuer Elements

Öffnen Sie zunächst die Seite `SimpleDisplay.aspx` im Ordner `BasicReporting`, wechseln Sie zu Designansicht, und ziehen Sie dann ein ObjectDataSource-Steuerelement aus dem Werkzeugkasten auf die Entwurfs Oberfläche der Seite. Die ObjectDataSource wird auf der Entwurfs Oberfläche als graues Feld angezeigt, da Sie kein Markup erzeugt. Es greift einfach auf Daten zu, indem eine Methode aus einem angegebenen Objekt aufgerufen wird. Die von ObjectDataSource zurückgegebenen Daten können von einem datenweb-Steuerelement, z. b. GridView, DetailsView, FormView usw., angezeigt werden.

> [!NOTE]
> Alternativ dazu können Sie zuerst der Seite das datenweb-Steuerelement hinzufügen und dann in der Dropdown Liste in der Dropdown Liste die Option &lt;neue Datenquelle&gt; auswählen.

Um das zugrunde liegende Objekt von ObjectDataSource anzugeben und die Methoden dieses Objekts der ObjectDataSource zuzuordnen, klicken Sie auf den Link Datenquelle konfigurieren aus dem Smarttag von ObjectDataSource.

[![klicken Sie auf den Link Datenquelle konfigurieren des Smarttags.](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Abbildung 3**: Klicken Sie auf den Link "Datenquelle konfigurieren" aus dem Smarttag ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image7.png)).

Dadurch wird der Assistent zum Konfigurieren von Datenquellen angezeigt. Zuerst müssen wir das Objekt angeben, mit dem die ObjectDataSource arbeiten soll. Wenn das Kontrollkästchen "nur Daten Komponenten anzeigen" aktiviert ist, werden in der Dropdown Liste auf diesem Bildschirm nur die Objekte aufgelistet, die mit dem `DataObject`-Attribut versehen wurden. Zurzeit enthält unsere Liste die TableAdapters in das typisierte DataSet und die BLL-Klassen, die wir im vorherigen Tutorial erstellt haben. Wenn Sie vergessen haben, das `DataObject`-Attribut zu den Geschäftslogik Schicht-Klassen hinzuzufügen, werden diese nicht in dieser Liste angezeigt. Deaktivieren Sie in diesem Fall das Kontrollkästchen "nur Daten Komponenten anzeigen", um alle Objekte anzuzeigen, die die BLL-Klassen enthalten sollen (zusammen mit den anderen Klassen im typisierten DataSet DataTables, DataRows usw.).

Wählen Sie in diesem ersten Bildschirm die `ProductsBLL` Klasse aus der Dropdown Liste aus, und klicken Sie auf Weiter.

[![das Objekt angeben, das mit dem ObjectDataSource-Steuerelement verwendet werden soll.](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Abbildung 4**: Angeben des Objekts, das mit dem ObjectDataSource-Steuerelement verwendet werden soll ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image10.png))

Auf dem nächsten Bildschirm des Assistenten werden Sie aufgefordert, die Methode auszuwählen, die von ObjectDataSource aufgerufen werden soll. In der Dropdown Liste sind die Methoden aufgeführt, die Daten in dem vom vorherigen Bildschirm ausgewählten Objekt zurückgeben. Hier sehen Sie `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`und `GetProductsBySupplierID`. Wählen Sie die `GetProducts` Methode aus der Dropdown Liste aus, und klicken Sie auf Fertigstellen (wenn Sie die `DataObjectMethodAttribute` den Methoden des `ProductBLL`hinzugefügt haben, wie im vorherigen Tutorial gezeigt, wird diese Option standardmäßig ausgewählt).

[![auswählen der Methode zum Zurückgeben von Daten auf der Registerkarte "auswählen"](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Abbildung 5**: Auswählen der Methode zum Zurückgeben von Daten auf der Registerkarte "auswählen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image13.png))

## <a name="configure-the-objectdatasource-manually"></a>Manuelles Konfigurieren von ObjectDataSource

Der Assistent zum Konfigurieren von Datenquellen von ObjectDataSource bietet eine schnelle Möglichkeit, das verwendete Objekt anzugeben und anzugeben, welche Methoden des Objekts aufgerufen werden. Sie können jedoch die ObjectDataSource über ihre Eigenschaften konfigurieren, entweder über die Eigenschaftenfenster oder direkt im deklarativen Markup. Legen Sie einfach die `TypeName`-Eigenschaft auf den Typ des zugrunde liegenden Objekts fest, das verwendet werden soll, und die `SelectMethod` auf die Methode, die beim Abrufen von Daten aufgerufen werden soll.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Auch wenn Sie den Assistenten zum Konfigurieren von Datenquellen bevorzugen, kann es vorkommen, dass Sie die ObjectDataSource manuell konfigurieren müssen, da der Assistent nur von Entwicklern erstellte Klassen auflistet. Wenn Sie ObjectDataSource an eine Klasse in der .NET Framework binden möchten, z. b. an die [Mitgliedschafts Klasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx), um auf Benutzerkontoinformationen zuzugreifen, oder auf die [Verzeichnis Klasse](https://msdn.microsoft.com/library/system.io.directory.aspx) , um mit Dateisystem Informationen zu arbeiten, müssen Sie die Eigenschaften von ObjectDataSource manuell festlegen.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Schritt 2: Hinzufügen eines datenweb-Steuer Elements und binden dieses Steuer Elements an die ObjectDataSource

Nachdem die ObjectDataSource der Seite hinzugefügt und konfiguriert wurde, können Sie der Seite datenweb Steuerelemente hinzufügen, um die von der `Select`-Methode von ObjectDataSource zurückgegebenen Daten anzuzeigen. Jedes datenweb Steuerelement kann an eine ObjectDataSource gebunden werden. sehen wir uns nun an, wie die Daten von ObjectDataSource in einem GridView-, DetailsView-und FormView-Objekt angezeigt werden.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Binden einer GridView an die ObjectDataSource

Fügen Sie der Entwurfs Oberfläche `SimpleDisplay.aspx`ein GridView-Steuerelement aus der Toolbox hinzu. Wählen Sie in der GridView-Smarttag das ObjectDataSource-Steuerelement aus, das Sie in Schritt 1 hinzugefügt haben. Dadurch wird automatisch ein BoundField in der GridView für jede Eigenschaft erstellt, die von den Daten aus der `Select`-Methode von ObjectDataSource zurückgegeben wird (die Eigenschaften, die durch die Datentabelle Products definiert werden).

[![eine GridView der Seite hinzugefügt und an die ObjectDataSource gebunden.](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Abbildung 6**: eine GridView wurde der Seite hinzugefügt und an ObjectDataSource gebunden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image16.png))

Sie können dann die boundfields von GridView anpassen, neu anordnen oder entfernen, indem Sie auf die Option Spalten bearbeiten im Smarttags klicken.

[![Verwalten der boundfields-Einstellungen der GridView über das Dialog Feld Spalten bearbeiten](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Abbildung 7**: Verwalten der boundfields-Einstellungen der GridView über das Dialog Feld "Spalten bearbeiten" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image19.png))

Nehmen Sie sich einen Moment Zeit, um die boundfields-Felder der GridView zu ändern, indem Sie die `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`und `ReorderLevel` boundfields entfernen. Wählen Sie einfach das Feld BoundField in der Liste unten links aus, und klicken Sie auf die Schaltfläche Löschen (das rote X), um Sie zu entfernen. Ordnen Sie dann die boundfields neu an, sodass die `CategoryName` und `SupplierName` boundfields dem `UnitPrice` BoundField vorangestellt sind, indem Sie diese boundfields auswählen und auf den Pfeil nach oben klicken. Legen Sie die `HeaderText` Eigenschaften der verbleibenden boundfields auf `Products`, `Category`, `Supplier`bzw. `Price`fest. Legen Sie als nächstes die `Price` BoundField als Währung formatiert, indem Sie die `HtmlEncode`-Eigenschaft von BoundField auf false und deren `DataFormatString`-Eigenschaft auf `{0:c}`festlegen. Richten Sie schließlich die `Price` rechtsbündig und das Kontrollkästchen `Discontinued` im Center über die Eigenschaft `ItemStyle`/`HorizontalAlign` aus.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

[![die boundfields von GridView angepasst wurden.](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Abbildung 8**: die boundfields der GridView wurden angepasst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image22.png))

## <a name="using-themes-for-a-consistent-look"></a>Verwenden von Designs für ein konsistentes Aussehen

Diese Lernprogramme sind bestrebt, alle Stileinstellungen auf Steuerungsebene zu entfernen, anstatt nach Möglichkeit Cascading Stylesheets zu verwenden, die in einer externen Datei definiert sind. Die `Styles.css` Datei enthält `DataWebControlStyle`-, `HeaderStyle`-, `RowStyle`-und `AlternatingRowStyle` CSS-Klassen, die verwendet werden sollten, um die Darstellung der in diesen Tutorials verwendeten datenweb Steuerelemente festzulegen. Um dies zu erreichen, können Sie die `CssClass`-Eigenschaft der GridView auf `DataWebControlStyle`festlegen, und die `AlternatingRowStyle` Eigenschaften der Eigenschaften `HeaderStyle`, `RowStyle`und `CssClass` Eigenschaften entsprechend.

Wenn wir diese `CssClass` Eigenschaften im websteuer Element festlegen, müssten wir daran denken, diese Eigenschaftswerte für jedes datenweb Steuerelement, das den Tutorials hinzugefügt wurde, explizit festzulegen. Ein besser verwaltbarer Ansatz besteht darin, die CSS-bezogenen Eigenschaften für das GridView-, DetailsView-und FormView-Steuerelement mithilfe eines Designs zu definieren. Ein Design ist eine Auflistung von Eigenschafts Einstellungen, Bildern und CSS-Klassen auf Steuerungsebene, die auf Seiten auf einer Website angewendet werden können, um ein gängiges Aussehen und Gefühl zu erzwingen.

Das Design enthält keine Bilder oder CSS-Dateien (wir belassen das Stylesheet `Styles.css` unverändert, das im Stamm Ordner der Webanwendung definiert ist), enthalten aber zwei Skins. Bei einer Skin handelt es sich um eine Datei, die die Standardeigenschaften für ein websteuer Element definiert. Insbesondere verfügen wir über eine Skin-Datei für das GridView-Steuerelement und das DetailsView-Steuerelement, das die Standard `CssClass`bezogenen Eigenschaften angibt.

Fügen Sie zunächst dem Projekt eine neue Skin-Datei mit dem Namen `GridView.skin` hinzu, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen klicken und neues Element hinzufügen auswählen.

[![eine Skin-Datei mit dem Namen GridView. Skin hinzufügen.](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Abbildung 9**: Hinzufügen einer Skin-Datei mit dem Namen `GridView.skin` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image25.png))

Skin-Dateien müssen in einem Design abgelegt werden, das sich im Ordner "`App_Themes`" befindet. Da wir noch nicht über einen solchen Ordner verfügen, kann Visual Studio beim Hinzufügen unserer ersten Skin eine für uns erstellen. Klicken Sie auf Ja, um den `App_Theme` Ordner zu erstellen, und platzieren Sie die neue `GridView.skin` Datei dort.

[![Visual Studio den Ordner "App_Theme" erstellen](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Abbildung 10**: Visual Studio können den `App_Theme` Ordner erstellen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image28.png))

Dadurch wird ein neues Design im Ordner "`App_Themes`" mit dem Namen GridView mit der Skin-Datei `GridView.skin`erstellt.

![Das GridView-Design wurde dem App_Theme-Ordner hinzugefügt.](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Abbildung 11**: das GridView-Design wurde dem `App_Theme`-Ordner hinzugefügt.

Benennen Sie das GridView-Design in datawebcontrols um (Klicken Sie im Ordner `App_Theme` mit der rechten Maustaste auf den GridView-Ordner, und wählen Sie Umbenennen aus). Geben Sie als nächstes das folgende Markup in die `GridView.skin` Datei ein:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Definiert die Standardeigenschaften für die `CssClass`bezogenen Eigenschaften für beliebige GridView auf jeder Seite, die das datawebcontrols-Design verwendet. Fügen wir eine weitere Skin für die DetailsView hinzu, ein Daten-websteuer Element, das wir in Kürze verwenden werden. Fügen Sie dem datawebcontrols-Design eine neue Skin mit dem Namen `DetailsView.skin` hinzu, und fügen Sie das folgende Markup hinzu:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Wenn unser Design definiert ist, besteht der letzte Schritt darin, das Design auf unsere ASP.NET-Seite anzuwenden. Ein Design kann auf seitenweise oder für alle Seiten auf einer Website angewendet werden. Wir verwenden dieses Design für alle Seiten auf der Website. Fügen Sie zu diesem Zweck das folgende Markup zu `Web.config``<system.web>`-Abschnitt hinzu:

[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Das ist schon alles! Die `styleSheetTheme` Einstellung gibt an, dass die Eigenschaften, die im Design angegeben werden, die auf der Steuerelement Ebene angegebenen Eigenschaften *nicht* überschreiben sollen. Verwenden Sie das `theme`-Attribut anstelle von `styleSheetTheme`, um festzulegen, dass die Design Einstellungen die Steuerelement Einstellungen überschreiben sollen. Leider werden Design Einstellungen nicht in der Visual Studio-Designansicht angezeigt. Weitere Informationen zu Designs und Skins finden Sie unter [ASP.net Themes and Skins Overview](https://msdn.microsoft.com/library/ykzx33wh.aspx) und [Server Side Styles using](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) Designs. Weitere Informationen zum Konfigurieren einer Seite für die Verwendung eines Designs finden Sie unter Gewusst [wie: Anwenden von ASP.net](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) Designs.

[![GridView zeigt den Produktnamen, die Kategorie, den Lieferanten, den Preis und die nicht mehr unterstützten Informationen an.](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Abbildung 12**: die GridView zeigt den Produktnamen, die Kategorie, den Lieferanten, den Preis und die nicht mehr unterstützten Informationen[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image32.png))

## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Anzeigen eines Datensatzes zu einem Zeitpunkt in der DetailsView

Die GridView zeigt eine Zeile für jeden Datensatz an, der vom Datenquellen-Steuerelement zurückgegeben wird, an das Sie gebunden ist. Manchmal kann es jedoch vorkommen, dass ein einziger Datensatz oder nur ein Datensatz gleichzeitig angezeigt werden soll. Das [DetailsView-Steuer](https://msdn.microsoft.com/library/s3w1w7t4.aspx) Element bietet diese Funktionalität, das Rendering als HTML-`<table>` mit zwei Spalten und eine Zeile für jede Spalte oder Eigenschaft, die an das Steuerelement gebunden ist. Sie können sich die DetailsView als GridView vorstellen, wobei ein einzelner Datensatz 90 Grad gedreht hat.

Beginnen Sie mit dem Hinzufügen eines DetailsView-Steuer Elements *oberhalb* der GridView in `SimpleDisplay.aspx`. Binden Sie Sie dann an dasselbe ObjectDataSource-Steuerelement wie die GridView. Wie bei GridView wird der DetailsView für jede Eigenschaft im Objekt, das von der `Select`-Methode von ObjectDataSource zurückgegeben wird, ein BoundField-Objekt hinzugefügt. Der einzige Unterschied besteht darin, dass die boundfields der DetailsView horizontal und nicht vertikal angeordnet werden.

[![der Seite eine DetailsView hinzufügen und an die ObjectDataSource binden](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Abbildung 13**: Hinzufügen einer DetailsView zur Seite und Binden an die ObjectDataSource ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image35.png))

Wie bei GridView können die boundfields der DetailsView angepasst werden, um eine besser angepasste Anzeige der von ObjectDataSource zurückgegebenen Daten bereitzustellen. In Abbildung 14 wird die DetailsView angezeigt, nachdem die Eigenschaften von boundfields und `CssClass` so konfiguriert wurden, dass Sie dem GridView-Beispiel ähneln.

[![die DetailsView einen einzelnen Datensatz anzeigt](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Abbildung 14**: die DetailsView zeigt einen einzelnen Datensatz ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image38.png))

Beachten Sie, dass die DetailsView nur den ersten Datensatz anzeigt, der von der zugehörigen Datenquelle zurückgegeben wird. Um dem Benutzer zu ermöglichen, alle Datensätze nacheinander schrittweise zu durchlaufen, müssen wir das Paging für die DetailsView aktivieren. Kehren Sie zu Visual Studio zurück, und aktivieren Sie das Kontrollkästchen Paging aktivieren im Smarttag der DetailsView.

[![Paging im DetailsView-Steuerelement aktivieren](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Abbildung 15**: Aktivieren von Paging im DetailsView-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image41.png))

[![bei aktiviertem Paging kann der Benutzer mit der DetailsView beliebige Produkte anzeigen.](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Abbildung 16**: bei aktiviertem Paging kann der Benutzer mit der DetailsView beliebige Produkte anzeigen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image44.png))

In zukünftigen Tutorials erfahren Sie mehr über das Paging.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Ein flexibleres Layout für die Anzeige eines Datensatzes gleichzeitig

Die DetailsView ist ziemlich starr in der Anzeige jedes Datensatzes, der von ObjectDataSource zurückgegeben wurde. Möglicherweise möchten wir eine flexiblere Ansicht der Daten. Anstatt z. b. den Namen, die Kategorie, den Lieferanten, den Preis und die nicht mehr unterstützten Informationen in einer separaten Zeile anzuzeigen, sollten Sie den Produktnamen und den Preis in einer `<h4>` Überschrift anzeigen, wobei die Kategorie-und Lieferanteninformationen unter dem Namen und dem Preis in einer kleineren Schriftgröße angezeigt werden. Die Eigenschaften Namen ("Product", "Category" usw.) können neben den Werten nicht angezeigt werden.

Das [FormView-Steuer](https://msdn.microsoft.com/library/fyf1dk77.aspx) Element bietet diese Anpassungs Ebene. Anstatt Felder (wie GridView und DetailsView) zu verwenden, verwendet FormView Vorlagen, die eine Mischung aus websteuer Elementen, statischem HTML-und [Datenbindung-Syntax](http://www.15seconds.com/issue/040630.htm)ermöglichen. Wenn Sie mit dem Repeater-Steuerelement von ASP.NET 1. x vertraut sind, können Sie sich die FormView als Wiederholungs Modul vorstellen, um einen einzelnen Datensatz anzuzeigen.

Fügen Sie der Entwurfs Oberfläche der `SimpleDisplay.aspx` Seite ein FormView-Steuerelement hinzu. Anfänglich wird FormView als grauer Block angezeigt, der uns mitteilt, dass die `ItemTemplate`des Steuer Elements zumindest bereitgestellt werden muss.

[![FormView muss ein ItemTemplate-Element enthalten.](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Abbildung 17**: die FormView muss einen `ItemTemplate` enthalten ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image47.png))

Sie können die FormView direkt an ein Datenquellen Steuerelement über das Smarttag von FormView binden, das automatisch eine Standard `ItemTemplate` erstellt (zusammen mit einem `EditItemTemplate` und `InsertItemTemplate`, wenn die `InsertMethod`-und `UpdateMethod` Eigenschaften des ObjectDataSource-Steuer Elements festgelegt sind). In diesem Beispiel binden wir die Daten an die Form View und geben Ihre `ItemTemplate` manuell an. Legen Sie zunächst die `DataSourceID`-Eigenschaft von FormView auf die `ID` des ObjectDataSource-Steuer Elements fest, `ObjectDataSource1`. Erstellen Sie als nächstes die `ItemTemplate`, sodass Sie den Namen und den Preis des Produkts in einem `<h4>`-Element und die untergeordneten Kategorien und shippernamen in einer kleineren Schriftgröße anzeigt.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]

[![das erste Produkt (Chai) in einem benutzerdefinierten Format angezeigt wird.](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Abbildung 18**: das erste Produkt (Chai) wird in einem benutzerdefinierten Format angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-data-with-the-objectdatasource-vb/_static/image50.png))

Der `<%# Eval(propertyName) %>` ist die Datenbindung-Syntax. Die `Eval`-Methode gibt den Wert der angegebenen Eigenschaft für das aktuelle Objekt zurück, das an das FormView-Steuerelement gebunden wird. Weitere Informationen zu den vor-und Nachteile von DataBinding finden Sie im Artikel über die [vereinfachte und erweiterte Daten Bindungs Syntax von Alex Homer in ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm) .

Wie die DetailsView zeigt FormView nur den ersten Datensatz an, der von ObjectDataSource zurückgegeben wurde. Sie können Paging in der FormView aktivieren, damit Besucher die Produkte nacheinander schrittweise durchlaufen können.

## <a name="summary"></a>Zusammenfassung

Der Zugriff auf und die Anzeige von Daten aus einer Geschäftslogik Ebene kann ohne Schreiben einer Codezeile durch das ObjectDataSource-Steuerelement von ASP.NET 2.0 erfolgen. ObjectDataSource Ruft eine angegebene Methode einer Klasse auf und gibt die Ergebnisse zurück. Diese Ergebnisse können in einem datenweb-Steuerelement angezeigt werden, das an ObjectDataSource gebunden ist. In diesem Tutorial haben wir uns mit dem Binden der GridView-, DetailsView-und FormView-Steuerelemente an die ObjectDataSource beschäftigt.

Bisher haben wir nur gesehen, wie Sie mit ObjectDataSource eine Parameter lose Methode aufrufen, aber was ist, wenn wir eine Methode aufrufen möchten, die Eingabeparameter erwartet, wie z. b. die `GetProductsByCategoryID(categoryID)`der `ProductBLL` Klasse? Um eine Methode aufzurufen, die einen oder mehrere Parameter erwartet, muss die ObjectDataSource konfiguriert werden, um die Werte für diese Parameter anzugeben. Im [nächsten Tutorial](declarative-parameters-vb.md)erfahren Sie, wie Sie dies erreichen.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Erstellen eigener Datenquellen-Steuerelemente](https://msdn.microsoft.com/library/ms364049.aspx)
- [GridView-Beispiele für ASP.NET 2,0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Vereinfachte und erweiterte Daten Bindungs Syntax in ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm)
- [Designs in ASP.NET 2,0](http://www.odetocode.com/Articles/423.aspx)
- [Server seitige Stile mithilfe von Designs](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Gewusst wie: Programm gesteuertes Anwenden von ASP.net-Themen](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Prüfer für dieses Tutorial war Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Weiter](declarative-parameters-vb.md)
