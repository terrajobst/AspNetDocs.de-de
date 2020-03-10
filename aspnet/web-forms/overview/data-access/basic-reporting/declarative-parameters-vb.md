---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Deklarative Parameter (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird veranschaulicht, wie Sie einen Parameter, der auf einen hart codierten Wert festgelegt ist, verwenden, um die Daten auszuwählen, die in einem DetailsView-Steuerelement angezeigt werden.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: cdc42752fc78d18366af037a81fe4ebe5a1646ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483129"
---
# <a name="declarative-parameters-vb"></a>Deklarative Parameter (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) oder [PDF herunterladen](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> In diesem Tutorial wird veranschaulicht, wie Sie einen Parameter, der auf einen hart codierten Wert festgelegt ist, verwenden, um die Daten auszuwählen, die in einem DetailsView-Steuerelement angezeigt werden.

## <a name="introduction"></a>Einführung

Im [letzten Tutorial](displaying-data-with-the-objectdatasource-vb.md) haben wir uns mit der Anzeige von Daten mit den Steuerelementen GridView, DetailsView und FormView befasst, die an ein ObjectDataSource-Steuerelement gebunden sind, das die `GetProducts()`-Methode aus der `ProductsBLL`-Klasse aufgerufen hat. Die `GetProducts()`-Methode gibt eine stark typisierte Datentabelle zurück, die mit allen Datensätzen aus der `Products` Tabelle der Northwind-Datenbank aufgefüllt wurde. Die `ProductsBLL`-Klasse enthält zusätzliche Methoden zum Zurückgeben von nur Teilmengen der Products-`GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`und `GetProductsBySupplierID(supplierID)`. Diese drei Methoden erwarten einen Eingabeparameter, der angibt, wie die zurückgegebenen Produktinformationen gefiltert werden.

Die ObjectDataSource kann verwendet werden, um Methoden aufzurufen, die Eingabeparameter erwarten. hierfür müssen wir jedoch angeben, wo die Werte für diese Parameter stammen. Die Parameterwerte können hart codiert sein oder aus einer Vielzahl dynamischer Quellen stammen, wie z. b. QueryString-Werte, Sitzungsvariablen, den Eigenschafts Wert eines websteuer Elements auf der Seite oder andere.

In diesem Tutorial wird veranschaulicht, wie Sie einen Parameter verwenden, der auf einen hart codierten Wert festgelegt ist. Insbesondere wird das Hinzufügen einer DetailsView zu der Seite untersucht, die Informationen zu einem bestimmten Produkt anzeigt, d. h. die Gumbo-Mischung von Chef Anton, die eine `ProductID` von 5 hat. Als Nächstes erfahren Sie, wie der Parameterwert auf Grundlage eines websteuer Elements festgelegt wird. Insbesondere verwenden wir ein Textfeld, um den Benutzer in ein Land einzugeben. Anschließend können Sie auf eine Schaltfläche klicken, um die Liste der Lieferanten in diesem Land anzuzeigen.

## <a name="using-a-hard-coded-parameter-value"></a>Verwenden eines hart codierten Parameter Werts

Beginnen Sie im ersten Beispiel mit dem Hinzufügen eines DetailsView-Steuer Elements zur Seite `DeclarativeParams.aspx` im `BasicReporting`-Ordner. Wählen Sie aus der Dropdown Liste aus dem Smarttag der DetailsView &lt;neuen Datenquellen&gt; aus, und wählen Sie die Option ObjectDataSource hinzufügen aus.

[![der Seite eine ObjectDataSource hinzufügen](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen von ObjectDataSource zur Seite ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image3.png))

Dadurch wird automatisch der Assistent zum Auswählen von Datenquellen des ObjectDataSource-Steuer Elements gestartet. Wählen Sie auf dem ersten Bildschirm des Assistenten die `ProductsBLL` Klasse aus.

[![wählen Sie die productbll-Klasse aus.](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Abbildung 2**: Auswählen der `ProductsBLL` Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image6.png))

Da wir Informationen zu einem bestimmten Produktanzeigen möchten, möchten wir die `GetProductByProductID(productID)`-Methode verwenden.

[![wählen Sie die Methode getproductbyproductid (ProductID) aus.](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Abbildung 3**: Auswählen der `GetProductByProductID(productID)` Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image9.png))

Da die ausgewählte Methode einen Parameter enthält, gibt es einen weiteren Bildschirm für den Assistenten, bei dem wir aufgefordert werden, den Wert zu definieren, der für den Parameter verwendet werden soll. Die Liste auf der linken Seite zeigt alle Parameter für die ausgewählte Methode an. Für `GetProductByProductID(productID)` gibt es nur eine `productID`. Auf der rechten Seite können Sie den Wert für den ausgewählten Parameter angeben. Die Dropdown Liste Parameter Quelle listet die verschiedenen möglichen Quellen für den Parameterwert auf. Da wir für den `productID` Parameter einen hart codierten Wert von "5" angeben möchten, belassen Sie die Parameter Quelle "None", und geben Sie 5 in das Textfeld "DefaultValue" ein.

[![ein hart codierter Parameter Wert von 5 für den ProductID-Parameter verwendet wird.](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Abbildung 4**: der hart codierte Parameterwert 5 wird für den `productID` Parameter verwendet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image12.png))

Nach Abschluss des Assistenten zum Konfigurieren von Datenquellen enthält das deklarative Markup des ObjectDataSource-Steuer Elements ein `Parameter` Objekt in der `SelectParameters` Auflistung für jeden der Eingabeparameter, die von der in der `SelectMethod`-Eigenschaft definierten Methode erwartet werden. Da die Methode, die wir in diesem Beispiel verwenden, nur einen Eingabeparameter erwartet (`parameterID`), gibt es hier nur einen Eintrag. Die `SelectParameters`-Auflistung kann jede Klasse enthalten, die von der `Parameter`-Klasse im `System.Web.UI.WebControls`-Namespace abgeleitet ist. Für hart codierte Parameterwerte wird die Basis `Parameter` Klasse verwendet, aber für die anderen Parameter Quell Optionen wird eine abgeleitete `Parameter` Klasse verwendet; Sie können bei Bedarf auch eigene [benutzerdefinierte Parametertypen](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)erstellen.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Wenn Sie auf Ihrem Computer daran arbeiten, kann das deklarative Markup, das Sie zu diesem Zeitpunkt sehen, Werte für die Eigenschaften `InsertMethod`, `UpdateMethod`und `DeleteMethod` sowie `DeleteParameters`enthalten. Der Assistent zum Auswählen von Datenquellen von ObjectDataSource gibt automatisch die Methoden aus dem `ProductBLL` an, die zum Einfügen, aktualisieren und Löschen verwendet werden sollen. Wenn Sie diese also nicht explizit löschen, werden Sie im obigen Markup enthalten sein.

Wenn Sie diese Seite aufrufen, ruft das datenweb-Steuerelement die `Select`-Methode von ObjectDataSource auf, die die `GetProductByProductID(productID)`-Methode der `ProductsBLL` Klasse aufruft, indem der hart codierte Wert 5 für den `productID` Eingabeparameter verwendet wird. Die Methode gibt ein stark typisiertes `ProductDataTable` Objekt zurück, das eine einzelne Zeile mit Informationen über die Gumbo-Mischung von Chef Anton (das Produkt mit `ProductID` 5) enthält.

[die ![Informationen zur Gumbo-Mischung von Chef Anton werden angezeigt.](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Abbildung 5**: Informationen zur Gumbo-Mischung von Chef Anton werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image15.png))

## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Festlegen des Parameter Werts auf den Eigenschafts Wert eines websteuer Elements

Die Parameterwerte der ObjectDataSource können auch basierend auf dem Wert eines websteuer Elements auf der Seite festgelegt werden. Um dies zu veranschaulichen, haben wir eine GridView, die alle Lieferanten auflistet, die sich in einem Land befinden, das vom Benutzer angegeben wird. Um dies zu erreichen, fügen Sie der Seite, in die der Benutzer einen Ländernamen eingeben kann, ein Textfeld hinzu. Legen Sie die `ID`-Eigenschaft dieses Textfeld-Steuer Elements auf `CountryName`fest. Fügen Sie auch ein Schaltflächen-websteuer Element hinzu.

[![der Seite mit der ID CountryName ein Textfeld hinzufügen](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen eines Textfelds zur Seite mit `ID` `CountryName` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image18.png))

Fügen Sie als nächstes der Seite ein GridView-Objekt hinzu, und wählen Sie aus dem Smarttag eine neue ObjectDataSource hinzufügen aus. Da wir Lieferanteninformationen anzeigen möchten, wählen Sie die `SuppliersBLL`-Klasse auf dem ersten Bildschirm des Assistenten aus. Wählen Sie auf dem zweiten Bildschirm die `GetSuppliersByCountry(country)`-Methode aus.

[![wählen Sie die getsuppliersbycountry (Country)-Methode aus.](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Abbildung 7**: Auswählen der `GetSuppliersByCountry(country)` Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image21.png))

Da die `GetSuppliersByCountry(country)`-Methode über einen Eingabeparameter verfügt, enthält der Assistent erneut einen letzten Bildschirm zum Auswählen des Parameter Werts. Legen Sie dieses Mal die Parameter Quelle auf Control fest. Dadurch wird die ControlID-Dropdown Liste mit den Namen der Steuerelemente auf der Seite aufgefüllt. Wählen Sie das `CountryName` Steuerelement aus der Liste aus. Beim ersten Besuch der Seite ist das Textfeld `CountryName` leer, sodass keine Ergebnisse zurückgegeben werden und nichts angezeigt wird. Wenn Sie einige Ergebnisse standardmäßig anzeigen möchten, legen Sie das Textfeld DefaultValue entsprechend fest.

[![legen Sie den Parameter Wert auf den CountryName-Steuerelement Wert fest.](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Abbildung 8**: Festlegen des Parameter Werts auf den Wert des `CountryName` Steuer Elements ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image24.png))

Das deklarative Markup von ObjectDataSource unterscheidet sich geringfügig vom ersten Beispiel mithilfe eines [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) anstelle des Standard-`Parameter` Objekts. Ein `ControlParameter` verfügt über zusätzliche Eigenschaften, um die `ID` des websteuer Elements und den Eigenschafts Wert anzugeben, der für den Parameter (`PropertyName`) verwendet werden soll. Der Assistent zum Konfigurieren von Datenquellen war intelligent genug, um zu bestimmen, dass für ein Textfeld wahrscheinlich die `Text`-Eigenschaft für den Parameterwert verwendet werden soll. Wenn Sie jedoch einen anderen Eigenschafts Wert aus dem websteuer Element verwenden möchten, können Sie den `PropertyName` Wert hier ändern oder indem Sie im Assistenten auf den Link Erweiterte Eigenschaften anzeigen klicken.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Wenn Sie die Seite zum ersten Mal aufrufen, ist das `CountryName` Textfeld leer. Die `Select`-Methode von ObjectDataSource wird weiterhin von der GridView aufgerufen, aber der Wert `Nothing` wird an die `GetSuppliersByCountry(country)`-Methode übergeben. Der TableAdapter konvertiert die `Nothing` in einen Daten Bank `NULL` Wert (`DBNull.Value`), aber die von der `GetSuppliersByCountry(country)`-Methode verwendete Abfrage wird so geschrieben, dass Sie keine Werte zurückgibt, wenn ein `NULL` Wert für den `@CategoryID` Parameter angegeben wird. Kurz gesagt, es werden keine Lieferanten zurückgegeben.

Nachdem der Besucher in ein Land gelangt ist und auf die Schaltfläche Suppliers anzeigen klickt, um ein Postback auszulösen, wird die `Select`-Methode von ObjectDataSource angefordert, und der `Text` Wert des TextBox-Steuer Elements wird als `country`-Parameter übergeben.

[![diese Lieferanten aus Kanada angezeigt werden](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Abbildung 9**: die Lieferanten aus Kanada werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image27.png))

## <a name="showing-all-suppliers-by-default"></a>Standardmäßige Anzeige aller Lieferanten

Anstatt den Anbietern anzuzeigen, wenn Sie die Seite zum ersten Mal anzeigen, möchten wir ggf. *alle* Lieferanten zuerst anzeigen, sodass der Benutzer die Liste durch Eingeben eines Länder namens in das Textfeld durchlaufen kann. Wenn das Textfeld leer ist, wird die `GetSuppliersByCountry(country)`-Methode der `SuppliersBLL` Klasse `Nothing` für den *`country`* Eingabeparameter übergeben. Dieser `Nothing` Wert wird dann in die `GetSupplierByCountry(country)`-Methode der dal weitergegeben, wo er in eine Datenbank `NULL` Wert für den `@Country`-Parameter in der folgenden Abfrage übersetzt wird:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

Der Ausdruck `Country = NULL` gibt immer false zurück, auch für Datensätze, deren `Country` Spalte einen `NULL` Wert aufweist. aus diesem Grund werden keine Datensätze zurückgegeben.

Um *alle* Lieferanten zurückzugeben, wenn das Textfeld Country leer ist, können wir die `GetSuppliersByCountry(country)`-Methode in der BLL erweitern, um die `GetSuppliers()`-Methode aufzurufen, wenn der Country-Parameter `Nothing` ist, und andernfalls die `GetSuppliersByCountry(country)` Methode der dal aufzurufen. Dies hat den Effekt, dass alle Lieferanten zurückgegeben werden, wenn kein Land angegeben ist, und die entsprechende Teilmenge der Lieferanten, wenn der Country-Parameter eingeschlossen wird.

Ändern Sie die `GetSuppliersByCountry(country)`-Methode in der `SuppliersBLL`-Klasse wie folgt:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Mit dieser Änderung werden auf der `DeclarativeParams.aspx` Seite alle Lieferanten angezeigt, wenn Sie zum ersten Mal besucht werden (oder wenn das `CountryName` Textfeld leer ist).

[![alle Lieferanten jetzt standardmäßig angezeigt werden](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Abbildung 10**: alle Lieferanten werden jetzt standardmäßig angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](declarative-parameters-vb/_static/image30.png))

## <a name="summary"></a>Zusammenfassung

Um Methoden mit Eingabe Parametern verwenden zu können, müssen die Werte für die Parameter in der `SelectParameters` Auflistung von ObjectDataSource angegeben werden. Verschiedene Typen von Parametern ermöglichen, dass der Parameterwert aus unterschiedlichen Quellen abgerufen wird. Der Standard Parametertyp verwendet einen hart codierten Wert, aber ebenso einfach (und ohne eine Codezeile) können Parameterwerte aus der QueryString, den Sitzungsvariablen, Cookies und sogar vom Benutzer eingegebenen Werten aus den websteuer Elementen auf der Seite abgerufen werden.

In den Beispielen in diesem Tutorial wurde gezeigt, wie deklarative Parameterwerte verwendet werden. Es kann jedoch vorkommen, dass eine Parameter Quelle verwendet werden muss, die nicht verfügbar ist, z. b. das aktuelle Datum und die Uhrzeit, oder wenn unsere Website die Mitgliedschaft verwendet hat, die Benutzer-ID des Besuchers. In solchen Szenarien können die Parameterwerte Programm gesteuert festgelegt werden, bevor ObjectDataSource die-Methode des zugrunde liegenden Objekts aufruft. Im [nächsten Tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)erfahren Sie, wie Sie dies erreichen.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Der Lead Prüfer für dieses Tutorial war Hilton giesreviewer. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-data-with-the-objectdatasource-vb.md)
> [Weiter](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
