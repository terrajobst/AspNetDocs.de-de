---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Anzeigen von Binärdaten in den datenweb Steuerelementen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial betrachten wir die Optionen zum Darstellen von Binärdaten auf einer Webseite, einschließlich der Anzeige einer Bilddatei und der Bereitstellung eines "Download"-Links f...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 27c901af092aa990f557750dc5d2c42ba2644c02
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640913"
---
# <a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Anzeigen von Binärdaten in den Datenwebsteuerelementen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) oder [PDF herunterladen](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> In diesem Tutorial betrachten wir die Optionen zum Darstellen von Binärdaten auf einer Webseite, einschließlich der Anzeige einer Bilddatei und der Bereitstellung eines "Download"-Links für eine PDF-Datei.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial haben wir die beiden Verfahren zum Zuordnen von Binärdaten zu einem zugrunde liegenden Datenmodell einer Anwendung untersucht und das FileUpload-Steuerelement verwendet, um Dateien aus einem Browser in das Dateisystem des Webserver s hochzuladen. Wir haben noch erfahren, wie die hochgeladenen Binärdaten dem Datenmodell zugeordnet werden. Das heißt, nachdem eine Datei hochgeladen und im Dateisystem gespeichert wurde, muss ein Pfad zur Datei im entsprechenden Datenbankdaten Satz gespeichert werden. Wenn die Daten direkt in der Datenbank gespeichert werden, müssen die hochgeladenen Binärdaten nicht im Dateisystem gespeichert werden, sondern müssen in die Datenbank eingefügt werden.

Bevor wir uns mit der Zuordnung der Daten zum Datenmodell befassen, sehen wir uns jedoch zunächst an, wie die Binärdaten für den Endbenutzer bereitgestellt werden. Das darstellen von Textdaten ist einfach genug, aber wie sollen Binärdaten angezeigt werden? Dies hängt natürlich von der Art der Binärdaten ab. Bei Bildern möchten wir wahrscheinlich das Bild anzeigen. für PDF-Dateien, Microsoft Word-Dokumente, ZIP-Dateien und andere Typen von Binärdaten ist die Bereitstellung eines Download Links wahrscheinlich besser geeignet.

In diesem Tutorial wird erläutert, wie die Binärdaten zusammen mit den zugehörigen Textdaten mithilfe von datenweb-Steuerelementen wie GridView und DetailsView präsentiert werden. Im nächsten Tutorial wird das Verknüpfen einer hochgeladenen Datei mit der Datenbank berücksichtigt.

## <a name="step-1-providingbrochurepathvalues"></a>Schritt 1: Bereitstellen von`BrochurePath`Werten

Die Spalte `Picture` in der `Categories` Tabelle enthält bereits Binärdaten für die verschiedenen kategorieimages. Insbesondere die Spalte `Picture` für jeden Datensatz enthält den binären Inhalt eines Grad-Bitmap-Bilds mit niedriger Qualität. Jedes Kategoriebild ist 172 Pixel breit und 120 Pixel hoch und verbraucht ungefähr 11 KB. Darüber hinaus enthält der binäre Inhalt in der `Picture`-Spalte einen 78-Byte- [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) -Header, der vor dem Anzeigen des Bilds entfernt werden muss. Diese Header Informationen sind vorhanden, da die Northwind-Datenbank ihre Stämme in Microsoft Access hat. In Access werden Binärdaten mithilfe des OLE-Objekt Datentyps gespeichert, der in diesem Header Tacks speichert. Vorerst sehen Sie, wie Sie die Header aus diesen Bildern mit niedriger Qualität entfernen, um das Bild anzuzeigen. In einem zukünftigen Tutorial erstellen wir eine Schnittstelle zum Aktualisieren der Kategorie s `Picture` Spalte und ersetzen diese Bitmap-Bilder, die OLE-Header mit äquivalenten jpg-Bildern ohne unnötige OLE-Header verwenden.

Im vorherigen Tutorial haben Sie erfahren, wie Sie das FileUpload-Steuerelement verwenden. Aus diesem Grund können Sie dem Webserver-Dateisystem-Dateisystem-Konsolen Dateien hinzufügen. Dadurch wird jedoch die `BrochurePath` Spalte in der `Categories` Tabelle nicht aktualisiert. Im nächsten Tutorial erfahren Sie, wie Sie dies erreichen, aber vorerst müssen wir die Werte für diese Spalte manuell angeben.

In diesem Tutorial finden Sie sieben PDF-Datei Dateien im Ordner "`~/Brochures`", eine für jede Kategorie mit Ausnahme von "Meeresfrüchte". Ich habe absichtlich das Hinzufügen einer Seafood-Broschüre ausgelassen, um zu veranschaulichen, wie Szenarios behandelt werden, bei denen nicht alle Datensätze binäre Daten Um die `Categories` Tabelle mit diesen Werten zu aktualisieren, klicken Sie mit der rechten Maustaste auf den Knoten `Categories` von Server-Explorer, und wählen Sie Tabellendaten anzeigen aus. Geben Sie dann die virtuellen Pfade zu den Prospekt Dateien für jede Kategorie ein, die eine-Broschüre enthält, wie in Abbildung 1 gezeigt. Da es keine Broschüre für die Kategorie "Seafood" gibt, belassen Sie den Wert der `BrochurePath` Spalte `NULL`.

[![die Werte für die Spalte "categories" in der Tabelle "Kategoriepfad" manuell eingeben](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Abbildung 1**: Manuelles Eingeben der Werte für den `Categories` Tabelle s `BrochurePath` Spalte ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))

## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Schritt 2: Bereitstellen eines Download Links für die Broschüren in einer GridView

Mit den `BrochurePath` Werten, die für die Tabelle "`Categories`" bereitgestellt werden, können wir erneut eine GridView erstellen, die jede Kategorie zusammen mit einem Link zum Herunterladen der Kategorie "Category" auflistet. In Schritt 4 erweitern wir diese GridView, um auch das Bild der Kategorie s anzuzeigen.

Ziehen Sie zunächst eine GridView aus der Toolbox auf den Designer der Seite `DisplayOrDownloadData.aspx` im Ordner `BinaryData`. Legen Sie die `ID` GridView s auf `Categories` und durch das Smarttag GridView s fest, das Sie an eine neue Datenquelle binden möchten. Binden Sie Sie insbesondere an eine ObjectDataSource mit dem Namen `CategoriesDataSource`, die Daten mithilfe der `CategoriesBLL` Object s `GetCategories()`-Methode abruft.

[![erstellen Sie eine neue ObjectDataSource mit dem Namen "categoriesdatasource".](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Abbildung 2**: Erstellen einer neuen ObjectDataSource mit dem Namen "`CategoriesDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))

[![Konfigurieren von ObjectDataSource für die Verwendung der Klasse "kategoriesbll"](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Abbildung 3**: Konfigurieren von ObjectDataSource für die Verwendung der `CategoriesBLL`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))

[![die Liste der Kategorien mithilfe der GetCategories ()-Methode abzurufen.](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Abbildung 4**: Abrufen der Liste der Kategorien mit der `GetCategories()`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))

Nachdem Sie den Assistenten zum Konfigurieren von Datenquellen abgeschlossen haben, fügt Visual Studio der `Categories` GridView automatisch ein BoundField für die `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`und `BrochurePath` `DataColumn` s hinzu. Entfernen Sie den `NumberOfProducts` BoundField, da die Abfrage der `GetCategories()` Methode diese Informationen nicht abruft. Entfernen Sie außerdem das `CategoryID` BoundField, und benennen Sie die `CategoryName` und `BrochurePath` boundfields `HeaderText` Eigenschaften in Category bzw. Nachdem Sie diese Änderungen vorgenommen haben, sollten das deklarative Markup der GridView-und ObjectDataSource-Objekte wie folgt aussehen:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Diese Seite über einen Browser anzeigen (siehe Abbildung 5). Jede der acht Kategorien ist aufgeführt. In den sieben Kategorien mit `BrochurePath` Werten wird der `BrochurePath` Wert im jeweiligen BoundField-Wert angezeigt. Bei Meeresfrüchten mit einem `NULL` Wert für die `BrochurePath`wird eine leere Zelle angezeigt.

[![werden die einzelnen Kategorien "Name", "Beschreibung" und "brochlinienpfad" aufgeführt.](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Abbildung 5**: die Kategorien "Name", "Beschreibung" und "`BrochurePath`" werden aufgelistet ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))

Anstatt den Text der `BrochurePath` Spalte anzuzeigen, möchten wir einen Link zur Broschüre erstellen. Um dies zu erreichen, entfernen Sie das `BrochurePath` BoundField, und ersetzen Sie es durch ein HyperLinkField. Legen Sie die neue Eigenschaft "HyperLinkField s `HeaderText`" auf "Prospekt", die `Text`-Eigenschaft auf "View" und deren `DataNavigateUrlFields` Eigenschaft auf "`BrochurePath`" fest.

![HyperLinkField für "brochurepath" hinzufügen](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Abbildung 6**: Hinzufügen eines HyperLinkField für `BrochurePath`

Dadurch wird eine Spalte mit Links zur GridView hinzugefügt, wie in Abbildung 7 gezeigt. Wenn Sie auf einen Link zum Anzeigen der Broschüre klicken, wird die PDF-Datei entweder direkt im Browser angezeigt, oder der Benutzer wird aufgefordert, die Datei herunterzuladen, je nachdem, ob ein PDF-Reader installiert ist, und die Browsereinstellungen.

[![eine Kategorie s-Broschüre angezeigt werden, indem Sie auf den Link "anzeigen" klicken](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Abbildung 7**: eine Kategorie-s-Broschüre kann angezeigt werden, indem Sie auf den Link "anzeigen"[klicken (Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))

[![die PDF-Datei der Kategorie s wird angezeigt.](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Abbildung 8**: die PDF-Datei der Kategorie s wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))

## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ausblenden des Texts der Ansichts Broschüre für Kategorien ohne eine Broschüre

Wie in Abbildung 7 dargestellt, zeigt der `BrochurePath` HyperLinkField seinen `Text`-Eigenschafts Wert ("Display") für alle Datensätze an, unabhängig davon, ob für `BrochurePath`ein nicht`NULL` Wert vorhanden ist. Wenn `BrochurePath` `NULL`ist, wird der Link natürlich nur als Text angezeigt, wie es bei der Kategorie "Seafood" der Fall ist (siehe Abbildung 7). Anstatt die Text Ansichts Broschüre anzuzeigen, ist es möglicherweise schön, dass diese Kategorien ohne einen `BrochurePath` Wert einen alternativen Text anzeigen, z. b. keine verfügbare Broschüre.

Um dieses Verhalten bereitzustellen, müssen wir ein TemplateField verwenden, dessen Inhalt durch einen Aufrufen einer Seiten Methode generiert wird, die die entsprechende Ausgabe basierend auf dem `BrochurePath` Wert ausgibt. Wir untersuchten dieses Formatierungs Verfahren zunächst im Tutorial [Verwenden von templatefields im GridView-Steuer](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) Element.

Aktivieren Sie das HyperLinkField in ein TemplateField-Element, indem Sie das `BrochurePath` HyperLinkField auswählen und dann im Dialogfeld Spalten bearbeiten auf das Feld dieses Feld in einen TemplateField-Link konvertieren klicken.

![Konvertieren des HyperLinkField in ein TemplateField-Element](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Abbildung 9**: Konvertieren des HyperLinkField in ein TemplateField-Element

Dadurch wird ein TemplateField mit einem `ItemTemplate` erstellt, das ein Hyperlink-websteuer Element enthält, dessen `NavigateUrl`-Eigenschaft an den `BrochurePath` Wert gebunden ist. Ersetzen Sie dieses Markup durch einen-Befehl für die-Methode `GenerateBrochureLink`, und übergeben Sie dabei den Wert `BrochurePath`:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Erstellen Sie als nächstes eine `Protected` Methode in der Code Behind-Klasse der ASP.net page s mit dem Namen `GenerateBrochureLink`, die eine `String` zurückgibt und eine `Object` als Eingabeparameter akzeptiert.

[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Diese Methode bestimmt, ob es sich bei dem `Object` Wert für die Übergabe um eine Datenbank handelt `NULL` und gibt, wenn dies der Fall ist, eine Meldung zurück, die angibt, dass in der Kategorie keine Andernfalls, wenn ein `BrochurePath` Wert vorhanden ist, wird er in einem Hyperlink angezeigt. Beachten Sie Folgendes: Wenn der `BrochurePath` Wert vorhanden ist, wird er an die [`ResolveUrl(url)`-Methode](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx)übermittelt. Diese Methode löst die über gegebene *URL*auf und ersetzt dabei das `~` Zeichen durch den entsprechenden virtuellen Pfad. Wenn die Anwendung z. b. auf `/Tutorial55`liegt, wird `ResolveUrl("~/Brochures/Meats.pdf")` `/Tutorial55/Brochures/Meat.pdf`zurückgeben.

Abbildung 10 zeigt die Seite, nachdem diese Änderungen angewendet wurden. Beachten Sie, dass im Feld "`BrochurePath`" der Kategorie "" nun der Text keine-Broschüre verfügbar anzeigt.

[![der Text für diese Kategorien ohne eine Broschüre wird keine verfügbare Broschüre angezeigt](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Abbildung 10**: der Text keine verfügbare Broschüre wird für diese Kategorien ohne eine-Broschüre angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))

## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Schritt 3: Hinzufügen einer Webseite zum Anzeigen eines kategoriebilds

Wenn ein Benutzer eine ASP.NET Seite besucht, empfängt er das HTML-Format der ASP.NET-Seite. Der empfangene HTML-Code ist nur Text und enthält keine Binärdaten. Alle zusätzlichen Binärdaten, z. b. Bilder, Audiodateien, Macromedia Flash Anwendungen, eingebettete Windows Media Player-Videos usw., sind als separate Ressourcen auf dem Webserver vorhanden. Der HTML-Code enthält Verweise auf diese Dateien, enthält jedoch nicht den eigentlichen Inhalt der Dateien.

Beispielsweise wird in HTML das `<img>`-Element verwendet, um auf ein Bild zu verweisen, wobei das `src` Attribut wie folgt auf die Bilddatei zeigt:

[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Wenn ein Browser diese HTML-Datei empfängt, sendet er eine weitere Anforderung an den Webserver, den binären Inhalt der Bilddatei abzurufen, der dann im Browser angezeigt wird. Dasselbe Konzept gilt für Binärdaten. In Schritt 2 wurde die Broschüre nicht als Teil des HTML-Markups der Seite an den Browser gesendet. Stattdessen gab der gerenderte HTML-Code Hyperlinks aus, die dazu geführt haben, dass der Browser das PDF-Dokument direkt angefordert hat.

Um Benutzern das Herunterladen von Binärdaten in der Datenbank anzuzeigen oder zuzulassen, muss eine separate Webseite erstellt werden, die die Daten zurückgibt. Für unsere Anwendung gibt es nur ein binäres Daten Feld, das direkt in der Datenbank in der Kategorie s angezeigt wird. Daher benötigen wir eine Seite, die beim Aufrufen die Bilddaten für eine bestimmte Kategorie zurückgibt.

Fügen Sie eine neue ASP.NET-Seite zum `BinaryData` Ordner mit dem Namen `DisplayCategoryPicture.aspx`hinzu. Lassen Sie dabei das Kontrollkästchen Master Seite auswählen deaktiviert. Diese Seite erwartet einen `CategoryID` Wert in der Abfrage Zeichenfolge und gibt die Binärdaten dieser Kategorie `Picture` Spalte zurück. Da auf dieser Seite binäre Daten zurückgegeben werden und nichts anderes vorhanden ist, wird im HTML-Abschnitt kein Markup benötigt. Klicken Sie daher in der unteren linken Ecke auf die Registerkarte Quelle, und entfernen Sie das gesamte Markup der Seite mit Ausnahme der `<%@ Page %>` Direktive. Das heißt, dass `DisplayCategoryPicture.aspx` s deklaratives Markup aus einer einzelnen Zeile bestehen sollte:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Wenn das `MasterPageFile`-Attribut in der `<%@ Page %>`-Direktive angezeigt wird, entfernen Sie es.

Fügen Sie in der Code Behind-Klasse der Seite dem `Page_Load`-Ereignishandler den folgenden Code hinzu:

[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Dieser Code beginnt mit dem Lesen des `CategoryID` QueryString-Werts in eine Variable mit dem Namen `categoryID`. Als nächstes werden die Bilddaten über einen aufzurufenden `CategoriesBLL` Klasse `GetCategoryWithBinaryDataByCategoryID(categoryID)`-Methode abgerufen. Diese Daten werden mit der `Response.BinaryWrite(data)`-Methode an den Client zurückgegeben, aber bevor dieser aufgerufen wird, muss der OLE-Header der `Picture` Column-Wert s entfernt werden. Dies wird erreicht, indem ein `Byte` Array namens `strippedImageData` erstellt wird, das genau 78 Zeichen enthält, die kleiner sind als die in der `Picture` Spalte. Die [`Array.Copy`-Methode](https://msdn.microsoft.com/library/z50k9bft.aspx) wird verwendet, um die Daten aus `category.Picture` beginnend an der Position 78 auf `strippedImageData`zu kopieren.

Die `Response.ContentType`-Eigenschaft gibt den [MIME-Typ](http://en.wikipedia.org/wiki/MIME) des Inhalts an, der zurückgegeben wird, damit der Browser weiß, wie er dargestellt wird. Da es sich bei der `Categories` Tabelle `Picture` Spalte um ein Bitmap-Bild handelt, wird hier der Bitmap-MIME-Typ verwendet (Bild/BMP). Wenn Sie den MIME-Typ weglassen, wird das Bild in den meisten Browsern weiterhin ordnungsgemäß angezeigt, da Sie den Typ basierend auf dem Inhalt der Binärdaten der Bilddatei-Binärdaten ableiten können. Es ist jedoch ratsam, den MIME-Typ nach Möglichkeit einzubeziehen. Eine komplette Liste der [MIME-Medientypen](http://www.iana.org/assignments/media-types/)finden Sie auf der [Website Internet Assigned Numbers Authority](http://www.iana.org/) .

Wenn diese Seite erstellt wurde, kann ein bestimmtes Bild der Kategorie angezeigt werden, indem Sie `DisplayCategoryPicture.aspx?CategoryID=categoryID`besuchen. Abbildung 11 zeigt die Bildkategorie "Getränke", die in `DisplayCategoryPicture.aspx?CategoryID=1`angezeigt werden kann.

[![das Bild der Kategorie "Getränke" angezeigt wird.](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Abbildung 11**: Bild der Kategorie "Getränke" wird angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))

Wenn beim Besuch `DisplayCategoryPicture.aspx?CategoryID=categoryID`eine Ausnahme ausgelöst wird, die liest, dass ein Objekt vom Typ "System. DBNull" nicht in den Typ "System. Byte []" umgewandelt werden kann, gibt es zwei Dinge, die dies verursachen können. Zuerst lässt die `Categories` Tabelle s `Picture` Spalte `NULL` Werte zu. Auf der `DisplayCategoryPicture.aspx` Seite wird jedoch davon ausgegangen, dass ein nicht`NULL` Wert vorhanden ist. Auf die `Picture`-Eigenschaft des `CategoriesDataTable` kann nicht direkt zugegriffen werden, wenn Sie über einen `NULL` Wert verfügt. Wenn Sie `NULL` Werte für die Spalte `Picture` zulassen möchten, sollten Sie die folgende Bedingung einschließen:

[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Der obige Code geht davon aus, dass es eine Bilddatei mit dem Namen `NoPictureAvailable.gif` im `Images` Ordner gibt, den Sie für diese Kategorien ohne Bild anzeigen möchten.

Diese Ausnahme kann auch dadurch verursacht werden, dass die `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT`-Anweisung in der Spaltenliste der Haupt Abfrage wieder hergestellt wurde. Dies kann vorkommen, wenn Sie Ad-hoc-SQL-Anweisungen verwenden und den Assistenten für die Haupt Abfrage TableAdapter s erneut ausführen. Stellen Sie sicher, dass `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT`-Anweisung weiterhin die `Picture` Spalte enthält.

> [!NOTE]
> Jedes Mal, wenn die `DisplayCategoryPicture.aspx` besucht wird, wird auf die Datenbank zugegriffen, und die Bilddaten der angegebenen Kategorie werden zurückgegeben. Wenn das Bild der Kategorie nicht geändert wurde, da der Benutzer es zuletzt angezeigt hat, ist dies der Aufwand. Glücklicherweise ermöglicht http das *bedingtes*abrufen. Bei einem bedingten Get sendet der Client, der die HTTP-Anforderung sendet, eine [`If-Modified-Since` http-Kopfzeile](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) , die das Datum und die Uhrzeit bereitstellt, zu der der Client diese Ressource zuletzt vom Webserver abgerufen hat. Wenn sich der Inhalt seit dem angegebenen Datum nicht geändert hat, antwortet der Webserver möglicherweise mit dem [Statuscode "nicht geändert" (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) und sendet den angeforderten Inhalt der Ressource zurück. Kurz gesagt, diese Vorgehensweise weist den Webserver an, den Inhalt für eine Ressource zurücksenden zu müssen, wenn Sie seit dem letzten Zugriff auf den Client nicht geändert wurde.

Um dieses Verhalten zu implementieren, erfordert jedoch, dass Sie der `Categories` Tabelle eine `PictureLastModified` Spalte hinzufügen, die erfasst werden soll, wenn die `Picture` Spalte zuletzt aktualisiert wurde, sowie Code zum Überprüfen des `If-Modified-Since` Headers. Weitere Informationen zum `If-Modified-Since`-Header und zum bedingten Get-Workflow finden Sie unter [http Conditional Get for RSS Hacker](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) und genauere [Betrachtung der Durchführung von HTTP-Anforderungen auf einer ASP.NET-Seite](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Schritt 4: Anzeigen der Kategoriebilder in einer GridView

Nachdem wir nun über eine Webseite verfügen, auf der ein bestimmtes Bild der Kategorie angezeigt wird, können wir Sie mit dem [Image-websteuer](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) Element oder einem HTML-`<img>` Element anzeigen, das auf `DisplayCategoryPicture.aspx?CategoryID=categoryID`verweist. Bilder, deren URL durch Datenbankdaten bestimmt wird, können in GridView oder DetailsView mithilfe von ImageField angezeigt werden. Das ImageField enthält `DataImageUrlField`-und `DataImageUrlFormatString` Eigenschaften, die wie die Eigenschaften `DataNavigateUrlFields` und `DataNavigateUrlFormatString` von HyperLinkField funktionieren.

Erweitern Sie die `Categories` GridView in `DisplayOrDownloadData.aspx`, indem Sie ein ImageField hinzufügen, um die einzelnen Kategorien anzuzeigen. Fügen Sie einfach das ImageField hinzu, und legen Sie dessen Eigenschaften für `DataImageUrlField` und `DataImageUrlFormatString` auf `CategoryID` bzw. `DisplayCategoryPicture.aspx?CategoryID={0}`fest. Dadurch wird eine GridView-Spalte erstellt, die ein `<img>` Element rendert, dessen `src` Attribut auf `DisplayCategoryPicture.aspx?CategoryID={0}`verweist, wobei {0} durch den `CategoryID` Wert der GridView-Zeile ersetzt wird.

![Hinzufügen eines ImageField zur GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Abbildung 12**: Hinzufügen eines ImageField zur GridView

Nach dem Hinzufügen von ImageField sollte die deklarative Syntax von GridView s wie folgt aussehen:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Nehmen Sie sich einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Beachten Sie, dass jeder Datensatz nun ein Bild für die Kategorie enthält.

[![der Bildkategorie s für jede Zeile angezeigt wird.](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Abbildung 13**: das Bild der Kategorie wird für jede Zeile angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))

## <a name="summary"></a>Summary

In diesem Tutorial wurde das präsentieren von Binärdaten untersucht. Wie die Daten dargestellt werden, hängt von der Art der Daten ab. Bei den PDF-Datei Dateien haben wir dem Benutzer einen Ansichts Prospekt Link angeboten, der den Benutzer beim Klicken direkt in die PDF-Datei aufgenommen hat. Für das Bild der Kategorie wurde zuerst eine Seite zum Abrufen und Zurückgeben der Binärdaten aus der Datenbank erstellt. Anschließend wird diese Seite verwendet, um jedes Bild der Kategorie in einer GridView anzuzeigen.

Nachdem wir uns nun mit dem Anzeigen von Binärdaten beschäftigt haben, können wir die Ausführung von Einfüge-, Aktualisierungs-und Lösch Vorgängen für die Datenbank mit den Binärdaten untersuchen. Im nächsten Tutorial wird erläutert, wie eine hochgeladene Datei mit dem entsprechenden Daten Bank Datensatz verknüpft wird. In diesem Tutorial erfahren Sie, wie Sie vorhandene Binärdaten aktualisieren und die Binärdaten löschen, wenn der zugehörige Datensatz entfernt wird.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Teresa Murphy und Dave Gardner. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](uploading-files-vb.md)
> [Weiter](including-a-file-upload-option-when-adding-a-new-record-vb.md)
