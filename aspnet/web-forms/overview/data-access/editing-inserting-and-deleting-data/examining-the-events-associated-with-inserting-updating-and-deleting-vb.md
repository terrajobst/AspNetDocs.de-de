---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Untersuchen der Ereignisse im Zusammenhang mit einfügen, aktualisieren und löschen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial untersuchen wir die Verwendung der Ereignisse, die vor, während und nach einem INSERT-, Update-oder DELETE-Vorgang eines ASP.NET Data Web-Steuer Elements auftreten. W...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 35edb3cefc6fe23bb56e667c02d10dc7798f730d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78492975"
---
# <a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Überprüfen von Ereignissen im Zusammenhang mit Vorgängen zum Einfügen, Aktualisieren und Löschen (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) oder [PDF herunterladen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> In diesem Tutorial untersuchen wir die Verwendung der Ereignisse, die vor, während und nach einem INSERT-, Update-oder DELETE-Vorgang eines ASP.NET Data Web-Steuer Elements auftreten. Außerdem wird erläutert, wie die Bearbeitungs Schnittstelle so angepasst wird, dass nur eine Teilmenge der Produktfelder aktualisiert wird.

## <a name="introduction"></a>Einführung

Bei Verwendung der integrierten Funktionen zum Einfügen, bearbeiten oder Löschen der Steuerelemente GridView, DetailsView oder FormView wird eine Reihe von Schritten ausgeführt, wenn der Endbenutzer den Vorgang zum Hinzufügen eines neuen Datensatzes oder zum Aktualisieren oder Löschen eines vorhandenen Datensatzes abschließt. Wie bereits im [vorherigen Tutorial](an-overview-of-inserting-updating-and-deleting-data-vb.md)erläutert, wird die Schaltfläche Bearbeiten durch die Schaltflächen aktualisieren und Abbrechen ersetzt, wenn eine Zeile in der GridView bearbeitet wird und die Schaltfläche "boundfields" in Textfelder umgewandelt wird. Nachdem der Endbenutzer die Daten aktualisiert und auf Aktualisieren geklickt hat, werden die folgenden Schritte für das Postback ausgeführt:

1. Die GridView füllt die `UpdateParameters` ihrer ObjectDataSource mit den eindeutigen identifizierenden Feldern (n) des bearbeiteten Datensatzes (über die `DataKeyNames`-Eigenschaft) zusammen mit den Werten, die vom Benutzer eingegeben wurden.
2. Die GridView ruft die `Update()`-Methode der ObjectDataSource auf, die wiederum die entsprechende Methode im zugrunde liegenden Objekt aufruft (`ProductsDAL.UpdateProduct`, in unserem vorherigen Tutorial).
3. Die zugrunde liegenden Daten, die nun die aktualisierten Änderungen enthalten, werden an die GridView zurückgebunden.

Während dieser Abfolge von Schritten wird eine Reihe von Ereignissen ausgelöst, die es uns ermöglichen, Ereignishandler zu erstellen, um bei Bedarf benutzerdefinierte Logik hinzuzufügen. Beispielsweise wird vor Schritt 1 das `RowUpdating` Ereignis von GridView ausgelöst. An diesem Punkt kann die Aktualisierungs Anforderung abgebrochen werden, wenn ein Validierungs Fehler vorliegt. Wenn die `Update()`-Methode aufgerufen wird, wird das `Updating`-Ereignis von ObjectDataSource ausgelöst und bietet die Möglichkeit, die Werte eines beliebigen `UpdateParameters`hinzuzufügen oder anzupassen. Nachdem die Ausführung der-Methode des zugrunde liegenden Objekts von ObjectDataSource abgeschlossen wurde, wird das `Updated`-Ereignis von ObjectDataSource ausgelöst. Ein Ereignishandler für das `Updated` Ereignis kann die Details zum Aktualisierungs Vorgang überprüfen, z. b. wie viele Zeilen betroffen sind und ob eine Ausnahme aufgetreten ist. Schließlich wird das `RowUpdated` Ereignis der GridView nach Schritt 2 ausgelöst. ein Ereignishandler für dieses Ereignis kann zusätzliche Informationen über den soeben ausgeführten Aktualisierungs Vorgang überprüfen.

Abbildung 1 zeigt eine Reihe von Ereignissen und Schritten beim Aktualisieren von GridView. Das Ereignis Muster in Abbildung 1 ist nicht eindeutig für die Aktualisierung mit einer GridView. Durch das Einfügen, aktualisieren oder Löschen von Daten aus GridView, DetailsView oder FormView wird die gleiche Abfolge von Ereignissen vor und nach der Ebene für das datenweb-und das ObjectDataSource-Steuerelement verursacht.

[![einer Reihe von vor-und nach Ereignissen, die beim Aktualisieren von Daten in einer GridView ausgelöst werden](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Abbildung 1**: eine Reihe von vor-und Post-Ereignissen wird beim Aktualisieren von Daten in einer GridView ausgelöst ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))

In diesem Tutorial untersuchen wir die Verwendung dieser Ereignisse, um die integrierten Einfüge-, Aktualisierungs-und Löschfunktionen der ASP.net-datenweb Steuerelemente zu erweitern. Außerdem wird erläutert, wie die Bearbeitungs Schnittstelle so angepasst wird, dass nur eine Teilmenge der Produktfelder aktualisiert wird.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Schritt 1: Aktualisieren der`ProductName`-und`UnitPrice`Felder eines Produkts

In den Bearbeitungs Schnittstellen aus dem vorherigen Tutorial mussten *alle* Produktfelder, die nicht schreibgeschützt waren, eingeschlossen werden. Wenn ein Feld aus der GridView entfernt werden soll, z. `QuantityPerUnit`: beim Aktualisieren der Daten würde das datenweb-Steuerelement den `QuantityPerUnit` `UpdateParameters` Wert von ObjectDataSource nicht festlegen. Die ObjectDataSource übergibt dann den Wert `Nothing` an die `UpdateProduct` Business Logic Layer (BLL)-Methode, die die `QuantityPerUnit` Spalte des bearbeiteten Datensatzes in einen `NULL` Wert ändern würde. Wenn ein erforderliches Feld, z. b. `ProductName`, von der Bearbeitungs Schnittstelle entfernt wird, schlägt das Update ebenso mit der Ausnahme "*Spalte ' ProductName ' keine NULL-Werte zulassen '* fehl. Der Grund für dieses Verhalten war, dass die ObjectDataSource so konfiguriert wurde, dass Sie die `UpdateProduct`-Methode der `ProductsBLL` Klasse aufruft, die für jedes der Produktfelder einen Eingabeparameter erwartet hat. Daher enthielt die `UpdateParameters` Auflistung von ObjectDataSource einen Parameter für jeden Eingabeparameter der Methode.

Wenn ein datenweb Steuerelement bereitgestellt werden soll, mit dem der Endbenutzer nur eine Teilmenge von Feldern aktualisieren kann, müssen die fehlenden `UpdateParameters` Werte im `Updating` Ereignishandler von ObjectDataSource Programm gesteuert festgelegt oder eine BLL-Methode erstellt und aufgerufen werden, die nur eine Teilmenge der Felder erwartet. Sehen wir uns den letzteren Ansatz an.

Erstellen wir insbesondere eine Seite, auf der nur die Felder "`ProductName`" und "`UnitPrice`" in einer bearbeitbaren GridView angezeigt werden. Mithilfe der Bearbeitungs Schnittstelle dieser GridView kann der Benutzer nur die beiden angezeigten Felder `ProductName` und `UnitPrice`aktualisieren. Da diese Bearbeitungs Schnittstelle nur eine Teilmenge der Felder eines Produkts bereitstellt, müssen wir entweder eine ObjectDataSource erstellen, die die `UpdateProduct`-Methode der vorhandenen BLL verwendet, und die fehlenden Produkt Feldwerte werden Programm gesteuert in Ihrem `Updating`-Ereignishandler festgelegt, oder wir müssen eine neue BLL-Methode erstellen, die nur die Teilmenge der in der GridView definierten Felder erwartet Verwenden Sie für dieses Tutorial die letztgenannte Option, und erstellen Sie eine Überladung der `UpdateProduct`-Methode, die nur drei Eingabeparameter annimmt: `productName`, `unitPrice`und `productID`:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Wie bei der ursprünglichen `UpdateProduct` Methode prüft diese Überladung zunächst, ob ein Produkt in der Datenbank mit dem angegebenen `ProductID`vorhanden ist. Wenn dies nicht der Fall ist, wird `False`zurückgegeben. Dies gibt an, dass die Anforderung zum Aktualisieren der Produktinformationen nicht Andernfalls werden die `ProductName`-und `UnitPrice` Felder des vorhandenen Produktdaten Satzes entsprechend aktualisiert, und es wird ein Commit für das Update ausgeführt, indem die `Update()`-Methode des TableAdapters aufgerufen und die `ProductsRow`-Instanz übergeben wird.

Mit dieser Ergänzung unserer `ProductsBLL`-Klasse können wir nun die vereinfachte GridView-Schnittstelle erstellen. Öffnen Sie die `DataModificationEvents.aspx` im Ordner `EditInsertDelete`, und fügen Sie der Seite eine GridView hinzu. Erstellen Sie eine neue ObjectDataSource, und konfigurieren Sie Sie so, dass Sie die `ProductsBLL`-Klasse mit ihrer `Select()`-Methoden Zuordnung zu `GetProducts` und deren `Update()`-Methode der `UpdateProduct` Überladung verwendet, die nur die `productName`-, `unitPrice`-und `productID`-Eingabeparameter verwendet. Abbildung 2 zeigt den Assistenten zum Erstellen von Datenquellen, wenn die `Update()`-Methode der ObjectDataSource der neuen `UpdateProduct`-Methoden Überladung der `ProductsBLL` Klasse entspricht.

[![die Update ()-Methode von ObjectDataSource der neuen UpdateProduct-Überladung zuzuordnen.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Abbildung 2**: Ordnen Sie die `Update()` Methode der ObjectDataSource der neuen `UpdateProduct` Überladung zu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png)).

Da unser Beispiel anfänglich nur die Möglichkeit zum Bearbeiten von Daten benötigt, aber keine Datensätze einfügen oder löschen, nehmen Sie sich einen Moment Zeit, um explizit anzugeben, dass die `Insert()`-und `Delete()` Methoden von ObjectDataSource keinen der Methoden der `ProductsBLL` Klasse zugeordnet werden sollen, indem Sie auf die Registerkarten einfügen und löschen klicken und (keine) aus der Dropdown Liste auswählen.

[![wählen Sie in der Dropdown Liste für die Registerkarten einfügen und löschen die Option (keine) aus.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Abbildung 3**: Wählen Sie in der Dropdown Liste für die Registerkarten einfügen und löschen die Option (keine) aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png)).

Aktivieren Sie nach Abschluss dieses Assistenten das Kontrollkästchen Bearbeitung aktivieren im Smarttag der GridView.

Mit dem Abschluss des Assistenten zum Erstellen von Datenquellen und der Bindung an die GridView hat Visual Studio die deklarative Syntax für beide Steuerelemente erstellt. Wechseln Sie zur Quell Ansicht, um das deklarative Markup von ObjectDataSource zu überprüfen. Dies wird unten dargestellt:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Da keine Zuordnungen für die Methoden `Insert()` und `Delete()` von ObjectDataSource vorhanden sind, gibt es keine `InsertParameters` oder `DeleteParameters` Abschnitte. Da die `Update()`-Methode der `UpdateProduct`-Methoden Überladung zugeordnet ist, die nur drei Eingabeparameter akzeptiert, enthält der `UpdateParameters` Abschnitt nur drei `Parameter` Instanzen.

Beachten Sie, dass die `OldValuesParameterFormatString`-Eigenschaft von ObjectDataSource auf `original_{0}`festgelegt ist. Diese Eigenschaft wird von Visual Studio automatisch festgelegt, wenn der Assistent zum Konfigurieren von Datenquellen verwendet wird. Da unsere BLL-Methoden jedoch nicht erwarten, dass der ursprüngliche `ProductID` Wert übergeben wird, entfernen Sie diese Eigenschaften Zuordnung vollständig aus der deklarativen Syntax von ObjectDataSource.

> [!NOTE]
> Wenn Sie einfach den `OldValuesParameterFormatString`-Eigenschafts Wert aus der Eigenschaftenfenster im Designansicht löschen, ist die Eigenschaft in der deklarativen Syntax weiterhin vorhanden, wird jedoch auf eine leere Zeichenfolge festgelegt. Entfernen Sie die-Eigenschaft vollständig aus der deklarativen Syntax, oder legen Sie aus dem Eigenschaftenfenster den Wert auf den Standardwert `{0}`fest.

Obwohl ObjectDataSource nur `UpdateParameters` für den Namen, den Preis und die ID des Produkts hat, hat Visual Studio in der GridView für jedes der Felder des Produkts ein BoundField-oder CheckBoxField-Element hinzugefügt.

[![der GridView ein BoundField-oder CheckBoxField-Feld für jedes der Produktfelder enthält.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Abbildung 4**: die GridView enthält ein BoundField-oder CheckBoxField-Feld für jedes der Produktfelder ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))

Wenn der Endbenutzer ein Produkt bearbeitet und auf die Aktualisierungs Schaltfläche klickt, listet die GridView die Felder auf, die nicht schreibgeschützt waren. Anschließend wird der Wert des entsprechenden Parameters in der `UpdateParameters` Auflistung von ObjectDataSource auf den Wert festgelegt, der vom Benutzer eingegeben wurde. Wenn kein entsprechender Parameter vorhanden ist, fügt der GridView der Auflistung eine hinzu. Wenn unsere GridView boundfields und checkboxfields für alle Felder des Produkts enthält, wird von ObjectDataSource schließlich die `UpdateProduct` Überladung aufgerufen, die alle diese Parameter annimmt, obwohl das deklarative Markup von ObjectDataSource nur drei Eingabeparameter angibt (siehe Abbildung 5). Wenn in der GridView eine Kombination von nicht schreibgeschützten Produkt Feldern vorhanden ist, die nicht den Eingabe Parametern für eine `UpdateProduct` Überladung entspricht, wird eine Ausnahme ausgelöst, wenn versucht wird, zu aktualisieren.

[![der GridView Parameter zur UpdateParameters-Sammlung von ObjectDataSource hinzufügt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Abbildung 5**: die GridView fügt der `UpdateParameters` Auflistung von ObjectDataSource Parameter hinzu ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))

Um sicherzustellen, dass ObjectDataSource die `UpdateProduct` Überladung aufruft, die nur den Namen, den Preis und die ID des Produkts annimmt, muss die GridView auf bearbeitbare Felder nur für die `ProductName` und `UnitPrice`eingeschränkt werden. Dies können Sie erreichen, indem Sie die anderen boundfields-und checkboxfields-Felder entfernen, indem Sie die `ReadOnly`-Eigenschaft der anderen Felder auf `True`oder eine Kombination der beiden Eigenschaften festlegen. In diesem Tutorial entfernen wir einfach alle GridView-Felder außer den `ProductName`-und `UnitPrice` boundfields, nach denen das deklarative Markup der GridView aussieht:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Obwohl die `UpdateProduct` Überladung drei Eingabeparameter erwartet, haben wir nur zwei boundfields in unserer GridView. Dies liegt daran, dass der `productID` Eingabeparameter ein Primärschlüssel Wert ist und über den Wert der `DataKeyNames`-Eigenschaft der bearbeiteten Zeile übergeben wird.

Unsere GridView ermöglicht zusammen mit der `UpdateProduct` Überladung einem Benutzer, nur den Namen und den Preis eines Produkts zu bearbeiten, ohne dass die anderen Produktfelder verloren gehen.

[![die Schnittstelle die Bearbeitung des Produkt namens und-Preises ermöglicht.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Abbildung 6**: die Schnittstelle ermöglicht das Bearbeiten nur des Produkt namens und-Preises ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))

> [!NOTE]
> Wie bereits im vorherigen Tutorial erläutert, ist es von entscheidender Bedeutung, dass der GridView s-Ansichts Zustand aktiviert ist (das Standardverhalten). Wenn Sie die Eigenschaft GridView s `EnableViewState` auf `false`festlegen, besteht das Risiko, dass gleichzeitige Benutzerdaten Sätze versehentlich löschen oder bearbeiten. Weitere Informationen finden Sie [unter Warnung: Parallelitäts Problem mit ASP.NET 2,0 GridViews/DetailsView/formviews, die das Bearbeiten und/oder löschen unterstützen und deren Ansichts Zustand deaktiviert ist](http://scottonwriting.net/sowblog/posts/10054.aspx) .

## <a name="improving-theunitpriceformatting"></a>Verbessern der`UnitPrice`Formatierung

Obwohl das in Abbildung 6 gezeigte GridView-Beispiel funktioniert, ist das `UnitPrice` Feld überhaupt nicht formatiert, was zu einer Preisanzeige führt, die keine Währungssymbole und vier Dezimalstellen enthält. Wenn Sie eine Währungs Formatierung für die nicht bearbeitbaren Zeilen anwenden möchten, legen Sie einfach die `DataFormatString`-Eigenschaft des `UnitPrice` BoundField auf `{0:c}` und deren `HtmlEncode`-Eigenschaft auf `False`fest.

[![die Eigenschaften DataFormatString und HtmlEncode von UnitPrice entsprechend festlegen.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Abbildung 7**: Festlegen der Eigenschaften des `UnitPrice``DataFormatString` und `HtmlEncode` entsprechend ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))

Durch diese Änderung formatieren die nicht bearbeitbaren Zeilen den Preis als Währung. die bearbeitete Zeile zeigt jedoch weiterhin den Wert ohne das Währungssymbol und vier Dezimalstellen an.

[![nicht bearbeitbare Zeilen werden nun als Währungswerte formatiert.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Abbildung 8**: die nicht bearbeitbaren Zeilen sind nun als Währungswerte formatiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))

Die in der `DataFormatString`-Eigenschaft angegebenen Formatierungs Anweisungen können auf die Bearbeitungs Schnittstelle angewendet werden, indem die `ApplyFormatInEditMode`-Eigenschaft von BoundField auf `True` festgelegt wird (der Standardwert ist `False`). Nehmen Sie sich einen Moment Zeit, um diese Eigenschaft auf `True`festzulegen.

[![die ApplyFormatInEditMode-Eigenschaft des UnitPrice BoundField auf "true" festgelegt.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Abbildung 9**: Festlegen der `ApplyFormatInEditMode`-Eigenschaft des `UnitPrice` BoundField auf `True` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))

Mit dieser Änderung wird der Wert des in der bearbeiteten Zeile angezeigten `UnitPrice` auch als Währung formatiert.

[![der UnitPrice-Wert der bearbeiteten Zeile ist nun als Währung formatiert.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Abbildung 10**: der `UnitPrice` Wert der bearbeiteten Zeile ist nun als Währung formatiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))

Beim Aktualisieren eines Produkts mit dem Währungssymbol im Textfeld, z. b. $19,00, wird jedoch ein `FormatException`ausgelöst. Wenn die GridView versucht, die vom Benutzer bereitgestellten Werte der `UpdateParameters` Collection von ObjectDataSource zuzuweisen, kann die `UnitPrice` Zeichenfolge "$19,00" nicht in die `Decimal` konvertiert werden, die für den-Parameter erforderlich ist (siehe Abbildung 11). Um dieses Problem zu beheben, können wir einen Ereignishandler für das `RowUpdating` Ereignis der GridView erstellen und die vom Benutzer bereitgestellten `UnitPrice` als Währungs formatierte `Decimal`analysieren lassen.

Das `RowUpdating` Ereignis von GridView akzeptiert als zweiten Parameter ein Objekt vom Typ " [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx)", das ein `NewValues` Wörterbuch als eine seiner Eigenschaften enthält, die die vom Benutzer bereitgestellten Werte enthält, die der `UpdateParameters` Auflistung von ObjectDataSource zugewiesen werden können. Wir können den vorhandenen `UnitPrice` Wert in der `NewValues` Auflistung mit einem Dezimalwert überschreiben, der im Währungs Format mit den folgenden Codezeilen im `RowUpdating`-Ereignishandler analysiert wurde:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Wenn der Benutzer einen `UnitPrice` Wert (z. b. "$19,00") angegeben hat, wird dieser Wert mit dem Dezimalwert überschrieben, der durch [Decimal.](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)Analyse berechnet wird, wobei der Wert als Währung analysiert wird. Dadurch wird das Dezimaltrennzeichen im Fall von Währungs Symbolen, Kommas, Dezimaltrennzeichen usw. ordnungsgemäß analysiert und die [Enumeration "zahlstile](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) " im [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) -Namespace verwendet.

Abbildung 11 zeigt das Problem, das durch Währungssymbole in der vom Benutzer bereitgestellten `UnitPrice`verursacht wird, sowie die Art und Weise, wie der `RowUpdating` Ereignishandler von GridView zum ordnungsgemäßen analysieren solcher Eingaben verwendet werden kann.

[![der UnitPrice-Wert der bearbeiteten Zeile ist nun als Währung formatiert.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Abbildung 11**: der `UnitPrice` Wert der bearbeiteten Zeile ist nun als Währung formatiert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))

## <a name="step-2-prohibitingnull-unitprices"></a>Schritt 2: verbieten von`NULL UnitPrices`

Obwohl die Datenbank so konfiguriert ist, dass `NULL` Werte in der `UnitPrice` Spalte der `Products` Tabelle zulässig sind, möchten Sie möglicherweise verhindern, dass Benutzer, die diese bestimmte Seite besuchen, eine `NULL` `UnitPrice` Wert angeben. Das heißt, wenn ein Benutzer beim Bearbeiten einer Produkt Zeile einen `UnitPrice` Wert nicht eingeben kann, anstatt die Ergebnisse in der Datenbank zu speichern, möchten wir eine Meldung anzeigen, die den Benutzer darüber informiert, dass auf dieser Seite für alle bearbeiteten Produkte ein Preis angegeben werden muss.

Das `GridViewUpdateEventArgs` Objekt, das an den `RowUpdating`-Ereignishandler der GridView übermittelt wird, enthält eine `Cancel`-Eigenschaft, die bei Festlegung auf `True`den Aktualisierungsprozess beendet. Erweitern Sie den `RowUpdating`-Ereignishandler, um `e.Cancel` auf `True` festzulegen, und zeigen Sie eine Meldung an, in der erläutert wird, warum der `UnitPrice` Wert in der `NewValues` Auflistung den Wert `Nothing`hat.

Fügen Sie zunächst der Seite mit dem Namen `MustProvideUnitPriceMessage`ein Label-websteuer Element hinzu. Dieses Bezeichnung-Steuerelement wird angezeigt, wenn der Benutzer beim Aktualisieren eines Produkts keinen `UnitPrice` Wert angeben kann. Legen Sie die `Text`-Eigenschaft der Bezeichnung auf "Sie müssen einen Preis für das Produkt angeben" fest. Außerdem habe ich eine neue CSS-Klasse in `Styles.css` namens `Warning` mit der folgenden Definition erstellt:

[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Legen Sie abschließend die `CssClass`-Eigenschaft der Bezeichnung auf `Warning`fest. An diesem Punkt sollte der Designer die Warnmeldung in einer rot, Fett, kursiv und sehr großen Schriftgröße oberhalb der GridView anzeigen, wie in Abbildung 12 dargestellt.

[![eine Bezeichnung oberhalb der GridView hinzugefügt wurde.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Abbildung 12**: eine Bezeichnung wurde oberhalb der GridView hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))

Diese Bezeichnung sollte standardmäßig ausgeblendet werden. Legen Sie daher die `Visible`-Eigenschaft auf `False` im `Page_Load`-Ereignishandler fest:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Wenn der Benutzer versucht, ein Produkt zu aktualisieren, ohne den `UnitPrice`anzugeben, möchten wir die Aktualisierung abbrechen und die Bezeichnung "Warning" anzeigen. Vergrößern Sie den `RowUpdating` Ereignishandler der GridView wie folgt:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Wenn ein Benutzer versucht, ein Produkt zu speichern, ohne einen Preis anzugeben, wird das Update abgebrochen, und es wird eine hilfreiche Meldung angezeigt. Obwohl die Datenbank (und die Geschäftslogik) `NULL` `UnitPrice` s zulässt, ist diese spezielle ASP.NET Seite nicht.

[![ein Benutzer darf UnitPrice nicht leer lassen.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Abbildung 13**: ein Benutzer kann `UnitPrice` nicht leer lassen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))

Bisher haben wir gesehen, wie das `RowUpdating` Ereignis der GridView verwendet wird, um die Parameterwerte Programm gesteuert zu ändern, die der `UpdateParameters` Auflistung von ObjectDataSource zugewiesen sind, und wie der Aktualisierungsprozess vollständig abgebrochen wird. Diese Konzepte werden in die Steuerelemente DetailsView und FormView übernommen und gelten auch für das Einfügen und löschen.

Diese Aufgaben können auch auf der ObjectDataSource-Ebene durch Ereignishandler für die `Inserting`-, `Updating`-und `Deleting`-Ereignisse ausgeführt werden. Diese Ereignisse werden ausgelöst, bevor die zugehörige-Methode des zugrunde liegenden Objekts aufgerufen wird, und stellen eine Chance der letzten Chance bereit, die Auflistung der Eingabeparameter zu ändern oder den Vorgang direkt abzubrechen. Den Ereignis Handlern für diese drei Ereignisse wird ein Objekt vom Typ [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) übergeben, das zwei relevante Eigenschaften hat:

- [Abbrechen](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), bei Festlegung auf `True`bricht den ausgeführten Vorgang ab.
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), eine Auflistung von `InsertParameters`, `UpdateParameters`oder `DeleteParameters`, abhängig davon, ob der Ereignishandler für das `Inserting`-, `Updating`-oder `Deleting`-Ereignis gilt.

Um die Arbeit mit den Parameterwerten auf der ObjectDataSource-Ebene zu veranschaulichen, nehmen wir eine DetailsView auf unserer Seite auf, mit der Benutzer ein neues Produkt hinzufügen können. Diese DetailsView wird verwendet, um eine Schnittstelle zum schnellen Hinzufügen eines neuen Produkts zur Datenbank bereitzustellen. Um beim Hinzufügen eines neuen Produkts eine konsistente Benutzeroberfläche beizubehalten, dürfen Benutzer nur Werte für die Felder `ProductName` und `UnitPrice` eingeben. Standardmäßig werden die Werte, die nicht in der einfügeschnittstelle der DetailsView bereitgestellt werden, auf einen `NULL` Daten Bank Wert festgelegt. Wir können jedoch das `Inserting`-Ereignis von ObjectDataSource verwenden, um andere Standardwerte einzufügen, wie wir es in Kürze sehen werden.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Schritt 3: Bereitstellen einer Schnittstelle zum Hinzufügen neuer Produkte

Ziehen Sie eine DetailsView aus der Toolbox auf den Designer oberhalb der GridView, löschen Sie den `Height`-und `Width` Eigenschaften, und binden Sie ihn an die bereits auf der Seite vorhandene ObjectDataSource. Dadurch wird ein BoundField-oder CheckBoxField-Feld für jedes der Felder des Produkts hinzugefügt. Da wir diese DetailsView zum Hinzufügen neuer Produkte verwenden möchten, müssen wir die Option zum Aktivieren der Option zum Aktivieren des Smarttags aktivieren. Diese Option ist jedoch nicht verfügbar, da die `Insert()`-Methode von ObjectDataSource keiner Methode in der `ProductsBLL`-Klasse zugeordnet ist (denken Sie daran, dass Sie diese Zuordnung auf (keine) festgelegt haben, wenn Sie die Datenquelle konfigurieren, siehe Abbildung 3).

Wählen Sie zum Konfigurieren von ObjectDataSource den Link Datenquelle konfigurieren aus dem Smarttag aus, und starten Sie den Assistenten. Auf dem ersten Bildschirm können Sie das zugrunde liegende Objekt ändern, an das ObjectDataSource gebunden ist. Legen Sie den Wert auf `ProductsBLL`fest. Auf dem nächsten Bildschirm werden die Zuordnungen von den Methoden von ObjectDataSource zum zugrunde liegenden-Objekt aufgelistet. Obwohl wir explizit angegeben haben, dass die Methoden `Insert()` und `Delete()` keinen Methoden zugeordnet werden sollen, sehen Sie, dass eine Zuordnung vorhanden ist, wenn Sie auf die Registerkarten einfügen und löschen klicken. Dies liegt daran, dass die Methoden `AddProduct` und `DeleteProduct` des `ProductsBLL`das `DataObjectMethodAttribute`-Attribut verwenden, um anzugeben, dass es sich hierbei um die Standardmethoden für `Insert()` bzw. `Delete()`handelt. Daher wählt der ObjectDataSource-Assistent diese bei jedem Ausführen des Assistenten aus, es sei denn, es ist explizit ein anderer Wert angegeben.

Belassen Sie die `Insert()`-Methode, die auf die `AddProduct`-Methode zeigt, und legen Sie die Dropdown Liste der Registerkarte löschen auf (keine) fest.

[![die Dropdown Liste der Registerkarte Einfügen auf die addProduct-Methode festgelegt.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Abbildung 14**: Festlegen der Dropdown Liste der Registerkarte "Einfügen" auf die `AddProduct` Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))

[![die Dropdown Liste der Registerkarte löschen auf (keine) festgelegt.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Abbildung 15**: Festlegen der Dropdown Liste der Registerkarte "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))

Nachdem Sie diese Änderungen vorgenommen haben, wird die deklarative Syntax von ObjectDataSource so erweitert, dass Sie eine `InsertParameters` Auflistung enthält, wie unten dargestellt:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Wenn Sie den Assistenten erneut ausführen, wird die `OldValuesParameterFormatString`-Eigenschaft zurückgesetzt. Nehmen Sie sich einen Moment Zeit, um diese Eigenschaft zu löschen, indem Sie Sie auf den Standardwert (`{0}`) festlegen oder Sie vollständig aus der deklarativen Syntax entfernen.

Wenn die ObjectDataSource Einfügefunktionen bereitstellt, enthält das Smarttag der DetailsView nun das Kontrollkästchen einfügen aktivieren. Kehren Sie zum Designer zurück, und aktivieren Sie diese Option. Im nächsten Schritt wird in der DetailsView nach unten angezeigt, sodass nur zwei boundfields-`ProductName` und `UnitPrice` sowie das CommandField-Feld verwendet werden. An diesem Punkt sollte die deklarative Syntax der DetailsView wie folgt aussehen:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

In Abbildung 16 wird diese Seite angezeigt, wenn Sie an dieser Stelle über einen Browser angezeigt wird. Wie Sie sehen können, listet die DetailsView den Namen und den Preis des ersten Produkts (Chai) auf. Es ist jedoch eine Benutzeroberfläche, die eine Benutzeroberfläche bereitstellt, mit der der Benutzer schnell ein neues Produkt zur Datenbank hinzufügen kann.

[![die DetailsView momentan im schreibgeschützten Modus gerendert wird.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Abbildung 16**: die DetailsView wird zurzeit im schreibgeschützten Modus gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))

Um die DetailsView im Einfügemodus anzuzeigen, müssen Sie die `DefaultMode`-Eigenschaft auf `Inserting`festlegen. Dadurch wird die DetailsView beim ersten Besuch im Einfügemodus gerendert und dort nach dem Einfügen eines neuen Datensatzes aufbewahrt. Wie in Abbildung 17 dargestellt, bietet eine solche DetailsView eine kurze Oberfläche zum Hinzufügen eines neuen Datensatzes.

[![DetailsView stellt eine Schnittstelle für das schnelle Hinzufügen eines neuen Produkts bereit.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Abbildung 17**: die DetailsView stellt eine Schnittstelle zum schnellen Hinzufügen eines neuen Produkts bereit ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))

Wenn der Benutzer einen Produktnamen und-Preis (z. b. "Acme Water" und 1,99, wie in Abbildung 17) eingibt und auf "Einfügen" klickt, wird ein Postback durchlaufen, und der einfügeworkflow beginnt mit einem neuen Produktdaten Satz, der der Datenbank hinzugefügt wird. Die DetailsView verwaltet die einfügeschnittstelle, und die GridView wird automatisch an die zugehörige Datenquelle zurückgebracht, um das neue Produkt einzuschließen, wie in Abbildung 18 dargestellt.

![Das Produkt](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Abbildung 18**: das Produkt "Acme Water" wurde der Datenbank hinzugefügt.

Obwohl die GridView in Abbildung 18 dies nicht anzeigt, werden die Produktfelder, die in der DetailsView-Schnittstelle fehlen `CategoryID`, `SupplierID`, `QuantityPerUnit`usw. `NULL` Daten Bankwerte zugewiesen. Sie sehen dies, indem Sie die folgenden Schritte ausführen:

1. Wechseln Sie in Visual Studio zum Server-Explorer.
2. Erweitern des Knotens "`NORTHWND.MDF` Datenbank"
3. Klicken Sie mit der rechten Maustaste auf den Knoten Datenbanktabelle `Products`.
4. Wählen Sie Tabellendaten anzeigen aus.

Dadurch werden alle Datensätze in der `Products` Tabelle aufgeführt. Wie in Abbildung 19 gezeigt, haben alle anderen Spalten des neuen Produkts als `ProductID`, `ProductName`und `UnitPrice` `NULL` Werte.

[![den in der DetailsView nicht angegebenen Produkt Feldern werden NULL-Werte zugewiesen.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Abbildung 19**: die Produktfelder, die nicht in der DetailsView angegeben sind, werden `NULL` Werten zugewiesen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))

Möglicherweise möchten Sie einen anderen Standardwert als `NULL` für einen oder mehrere dieser Spaltenwerte angeben, da `NULL` nicht die beste Standardoption ist oder weil die Daten Bank Spalte selbst `NULL` s nicht zulässt. Um dies zu erreichen, können Sie die Werte der Parameter der `InputParameters` Auflistung der DetailsView Programm gesteuert festlegen. Diese Zuweisung kann entweder im Ereignishandler für das `ItemInserting`-Ereignis der DetailsView oder in das `Inserting`-Ereignis von ObjectDataSource erfolgen. Da wir bereits die Verwendung der Ereignisse vor und nach der Ebene auf der Daten-websteuer Element Ebene untersucht haben, können wir uns dieses Mal mit den Ereignissen von ObjectDataSource beschäftigen.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Schritt 4: Zuweisen von Werten zu den Parametern`CategoryID`und`SupplierID`

In diesem Tutorial stellen wir uns vor, dass für die Anwendung beim Hinzufügen eines neuen Produkts über diese Schnittstelle eine `CategoryID` und `SupplierID` Wert von 1 zugewiesen werden soll. Wie bereits erwähnt, verfügt ObjectDataSource über ein paar von Ereignissen vor und nach der Ebene, die während des Daten Änderungsprozesses ausgelöst werden. Wenn die `Insert()`-Methode aufgerufen wird, löst ObjectDataSource zuerst das `Inserting`-Ereignis aus, ruft dann die-Methode auf, der die `Insert()`-Methode zugeordnet wurde, und löst schließlich das `Inserted`-Ereignis aus. Der `Inserting`-Ereignishandler bietet uns eine letzte Gelegenheit, die Eingabeparameter zu optimieren oder den Vorgang ganz einfach abzubrechen.

> [!NOTE]
> In einer realen Anwendung ist es wahrscheinlich, dass der Benutzer die Kategorie und den Lieferanten angeben oder diesen Wert auf der Grundlage bestimmter Kriterien oder Geschäftslogik auswählen soll (anstatt die ID 1 Blind auszuwählen). Das Beispiel veranschaulicht, wie der Wert eines Eingabe Parameters Programm gesteuert aus dem Pre-Level-Ereignis von ObjectDataSource festgelegt wird.

Nehmen Sie sich einen Moment Zeit, um einen Ereignishandler für das `Inserting` Ereignis von ObjectDataSource zu erstellen. Beachten Sie, dass der zweite Eingabeparameter des Ereignis Handlers ein Objekt vom Typ `ObjectDataSourceMethodEventArgs`ist, das über eine-Eigenschaft für den Zugriff auf die Parameter Auflistung (`InputParameters`) und eine-Eigenschaft verfügt, um den Vorgang abzubrechen (`Cancel`).

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

An diesem Punkt enthält die `InputParameters`-Eigenschaft die `InsertParameters` Auflistung von ObjectDataSource mit den Werten, die aus der DetailsView zugewiesen werden. Wenn Sie den Wert eines dieser Parameter ändern möchten, verwenden Sie einfach: `e.InputParameters("paramName") = value`. Wenn Sie die `CategoryID` und `SupplierID` auf die Werte 1 festlegen möchten, passen Sie den Ereignishandler für `Inserting` so an, dass er wie folgt aussieht:

[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Dieses Mal, wenn Sie ein neues Produkt hinzufügen (z. b. acme Soda), werden die `CategoryID`-und `SupplierID` Spalten des neuen Produkts auf 1 festgelegt (siehe Abbildung 20).

[für ![neuen Produkte sind nun die Werte für CategoryID und SupplierID auf 1 festgelegt.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Abbildung 20**: für neue Produkte werden nun ihre `CategoryID` und `SupplierID` Werte auf 1 festgelegt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))

## <a name="summary"></a>Zusammenfassung

Beim Bearbeiten, einfügen und löschen durchlaufen sowohl das datenweb Steuerelement als auch die ObjectDataSource eine Reihe von Ereignissen vor und nach der Ebene. In diesem Tutorial haben wir die Ereignisse vor der Ebene untersucht und gesehen, wie diese verwendet werden, um die Eingabeparameter anzupassen oder den Daten Änderungs Vorgang vollständig sowohl aus dem datenweb-Steuerelement als auch aus den Ereignissen von ObjectDataSource abzubrechen. Im nächsten Tutorial wird das Erstellen und Verwenden von Ereignis Handlern für die Ereignisse nach der Ebene untersucht.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Prüfer für dieses Tutorial waren Jackie Goor und Liz shulok. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [Weiter](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
