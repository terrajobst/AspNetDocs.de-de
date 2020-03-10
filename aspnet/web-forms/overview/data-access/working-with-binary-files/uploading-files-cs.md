---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Hochladen von DateienC#() | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Sie es Benutzern ermöglichen, Binärdateien (z. b. Word-oder PDF-Dokumente) auf Ihre Website hochzuladen, wo Sie möglicherweise im Dateisystem des Servers gespeichert werden...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e3e32a829de386a681504c8d5d61dd258b8b2e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441693"
---
# <a name="uploading-files-c"></a>Hochladen von Dateien (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) oder [PDF herunterladen](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Erfahren Sie, wie Sie es Benutzern ermöglichen können, Binärdateien (z. b. Word-oder PDF-Dokumente) auf Ihre Website hochzuladen, wo Sie entweder im Dateisystem des Servers oder in der Datenbank gespeichert werden können.

## <a name="introduction"></a>Einführung

Alle Lernprogramme, die wir bisher geprüft haben, haben ausschließlich mit Textdaten gearbeitet. Viele Anwendungen verfügen jedoch über Datenmodelle, mit denen sowohl Text-als auch Binärdaten erfasst werden. Eine Website für die Online Stellung kann es Benutzern ermöglichen, ein Bild hochzuladen, das Ihrem Profil zugeordnet werden soll. Eine Recruiting-Website ermöglicht Benutzern möglicherweise, ihre Fortsetzung als Microsoft Word-oder PDF-Dokument hochzuladen.

Das Arbeiten mit binären Daten führt zu einer neuen Reihe von Herausforderungen. Wir müssen entscheiden, wie die Binärdaten in der Anwendung gespeichert werden. Die-Schnittstelle, die zum Einfügen neuer Datensätze verwendet wird, muss aktualisiert werden, damit der Benutzer eine Datei von Ihrem Computer hochladen kann. es müssen zusätzliche Schritte ausgeführt werden, um eine Möglichkeit zum Herunterladen der zugeordneten Binärdaten eines Datensatzes anzuzeigen oder bereitzustellen. In diesem Tutorial und den nächsten drei Untersuchungen wird erläutert, wie diese Herausforderungen bewältigt werden können. Am Ende dieser Tutorials haben wir eine voll funktionsfähige Anwendung erstellt, die jeder Kategorie eine Bild-und PDF-Grafik zuordnet. In diesem speziellen Tutorial beschäftigen wir uns mit verschiedenen Verfahren zum Speichern von Binärdaten und erfahren, wie Sie es Benutzern ermöglichen können, eine Datei von Ihrem Computer hochzuladen und auf dem Dateisystem des Webserver Servers zu speichern.

> [!NOTE]
> Binärdaten, die Teil eines Anwendungsdaten Modells sind, werden mitunter als [BLOB](http://en.wikipedia.org/wiki/Binary_large_object)bezeichnet, ein Akronym für das binäre große Objekt. In diesen Tutorials habe ich mich für die Verwendung der Terminologie-Binärdaten entschieden, obwohl der Begriff "BLOB" gleichbedeutend ist.

## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Schritt 1: Erstellen der Webseiten zum Arbeiten mit Binärdaten

Bevor wir beginnen, die Herausforderungen im Zusammenhang mit dem Hinzufügen von Unterstützung für Binärdaten zu untersuchen, nehmen Sie sich einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen, und die nächsten drei Fügen Sie zunächst einen neuen Ordner mit dem Namen `BinaryData`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`

![Fügen Sie die ASP.NET-Seiten für die Binärdaten bezogenen Lernprogramme hinzu.](uploading-files-cs/_static/image1.gif)

**Abbildung 1**: Hinzufügen der ASP.NET-Seiten für die Binärdaten bezogenen Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `BinaryData` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image2.png))

Fügen Sie diese Seiten schließlich als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach der Erweiterung des GridView-`<siteMapNode>`das folgende Markup hinzu:

[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials arbeiten mit binären Daten.

![Die Site Übersicht enthält jetzt Einträge für die Tutorials arbeiten mit binären Daten.](uploading-files-cs/_static/image3.gif)

**Abbildung 3**: die Site Übersicht enthält jetzt Einträge für die Tutorials "arbeiten mit Binärdaten".

## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Schritt 2: Bestimmen des Speicher Orts der Binärdaten

Binärdaten, die dem Datenmodell der Anwendung zugeordnet sind, können an einer von zwei Stellen gespeichert werden: auf dem Dateisystem des Webserver s mit einem Verweis auf die in der Datenbank gespeicherte Datei. oder direkt in der Datenbank selbst (siehe Abbildung 4). Jeder Ansatz verfügt über einen eigenen Satz von vor-und Nachteile und verdient eine ausführlichere Erläuterung.

[![Binärdaten können im Datei System oder direkt in der Datenbank gespeichert werden.](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Abbildung 4**: Binärdaten können im Datei System oder direkt in der Datenbank gespeichert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image4.png))

Stellen Sie sich vor, dass die Northwind-Datenbank erweitert werden soll, um jedem Produkt ein Bild zuzuordnen. Eine Möglichkeit besteht darin, diese Bilddateien im Dateisystem des Webserver s zu speichern und den Pfad in der `Products` Tabelle aufzuzeichnen. Bei diesem Ansatz fügen wir eine `ImagePath` Spalte zur `Products`-Tabelle des Typs `varchar(200)`hinzu, vielleicht. Wenn ein Benutzer ein Bild für Chai hochgeladen hat, wird das Bild möglicherweise auf dem Dateisystem des Webserver s auf `~/Images/Tea.jpg`gespeichert, wobei `~` den physischen Pfad der Anwendung darstellt. Das heißt, wenn sich die Website auf den physischen Pfad `C:\Websites\Northwind\`befindet, entsprechen `~/Images/Tea.jpg` `C:\Websites\Northwind\Images\Tea.jpg`. Nachdem Sie die Bilddatei hochgeladen haben, aktualisieren wir den Chai-Datensatz in der `Products`-Tabelle, damit die `ImagePath` Spalte auf den Pfad des neuen Images verwiesen wird. Wir könnten `~/Images/Tea.jpg` oder nur `Tea.jpg` verwenden, wenn wir beschlossen haben, dass alle Produkt Images in den Ordner der Anwendung `Images` eingefügt werden.

Die Hauptvorteile der Speicherung der Binärdaten im Dateisystem sind:

- **Einfache Implementierung** , wie wir bald sehen werden, umfasst das Speichern und Abrufen von Binärdaten, die direkt in der Datenbank gespeichert werden, etwas mehr Code als bei der Arbeit mit Daten über das Dateisystem. Damit ein Benutzer Binärdaten anzeigen oder herunterladen kann, muss er außerdem mit einer URL zu diesen Daten angezeigt werden. Wenn sich die Daten auf dem Dateisystem des Webserver Servers befinden, ist die URL unkompliziert. Wenn die Daten in der Datenbank gespeichert werden, muss jedoch eine Webseite erstellt werden, mit der die Daten aus der Datenbank abgerufen und zurückgegeben werden.
- **Größerer Zugriff auf die Binärdaten** , auf die die Binärdaten möglicherweise von anderen Diensten oder Anwendungen zugegriffen werden kann, von denen die Daten nicht aus der Datenbank abgerufen werden können. Beispielsweise können die Images, die mit den einzelnen Produkten verknüpft sind, auch für Benutzer über [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)verfügbar sein. in diesem Fall möchten wir die Binärdaten im Dateisystem speichern.
- **Leistung** wenn die Binärdaten im Dateisystem gespeichert werden, sind die Anforderungen und die Netzwerk Überlastung zwischen dem Datenbankserver und dem Webserver geringer, als wenn die Binärdaten direkt in der Datenbank gespeichert werden.

Der Hauptnachteil beim Speichern von Binärdaten im Dateisystem besteht darin, dass die Daten von der Datenbank entkoppelt werden. Wenn ein Datensatz aus der `Products` Tabelle gelöscht wird, wird die zugehörige Datei im Dateisystem des Webserver s nicht automatisch gelöscht. Wir müssen zusätzlichen Code schreiben, um die Datei zu löschen, oder das Dateisystem wird mit nicht verwendeten, verwaisten Dateien überlastet. Außerdem muss beim Sichern der Datenbank sichergestellt werden, dass auch Sicherungen der zugeordneten Binärdaten im Dateisystem vorgenommen werden. Das Verschieben der Datenbank auf einen anderen Standort oder Server bringt ähnliche Herausforderungen mit sich.

Alternativ können binäre Daten direkt in einer Microsoft SQL Server 2005-Datenbank gespeichert werden, indem eine Spalte vom Typ "`varbinary`" erstellt wird. Wie bei anderen Datentypen variabler Länge können Sie eine maximale Länge der Binärdaten angeben, die in dieser Spalte gespeichert werden können. Verwenden Sie z. b. zum Reservieren von höchstens 5.000 Bytes `varbinary(5000)`; `varbinary(MAX)` ermöglicht die maximale Speichergröße von ca. 2 GB.

Der Hauptvorteil der direkten Speicherung von Binärdaten in der Datenbank ist die enge Kopplung zwischen den Binärdaten und dem Datenbankdaten Satz. Dadurch werden Datenbankverwaltungsaufgaben, wie z. b. Sicherungen oder das Verschieben der Datenbank auf einen anderen Standort oder Server, erheblich vereinfacht. Beim Löschen eines Datensatzes werden auch die entsprechenden Binärdaten automatisch gelöscht. Die Speicherung der Binärdaten in der Datenbank hat ebenfalls größere Vorteile. Eine ausführlichere Erläuterung finden [Sie unter Speichern von Binärdateien direkt in der Datenbank mit ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) .

> [!NOTE]
> In Microsoft SQL Server 2000 und früheren Versionen enthielt der `varbinary` Datentyp einen maximalen Grenzwert von 8.000 Bytes. Zum Speichern von bis zu 2 GB Binärdaten muss stattdessen der [`image` Datentyp](https://msdn.microsoft.com/library/ms187993.aspx) verwendet werden. Durch das Hinzufügen von `MAX` in SQL Server 2005 ist der `image` Datentyp jedoch veraltet. Es wird weiterhin aus Gründen der Abwärtskompatibilität unterstützt, aber Microsoft hat angekündigt, dass der `image`-Datentyp in einer zukünftigen Version von SQL Server entfernt wird.

Wenn Sie mit einem älteren Datenmodell arbeiten, sehen Sie möglicherweise den `image`-Datentyp. Die `Categories` Tabelle der Northwind-Datenbank verfügt über eine `Picture` Spalte, die zum Speichern der Binärdaten einer Bilddatei für die Kategorie verwendet werden kann. Da die Northwind-Datenbank Ihre Stamm Elemente in Microsoft Access und früheren Versionen von SQL Server hat, ist diese Spalte vom Typ `image`.

In diesem Tutorial und den nächsten drei Methoden werden beide Ansätze verwendet. Die `Categories` Tabelle verfügt bereits über eine `Picture` Spalte zum Speichern des binären Inhalts eines Bilds für die Kategorie. Wir fügen eine zusätzliche Spalte (`BrochurePath`) hinzu, um einen Pfad zu einer PDF-Datei im Webserver-Dateisystem zu speichern, die verwendet werden kann, um eine drucksqualität, eine polierte Übersicht über die Kategorie bereitzustellen.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Schritt 3: Hinzufügen der`BrochurePath`Spalte zur Tabelle "`Categories`"

Die Kategorietabelle enthält derzeit nur vier Spalten: `CategoryID`, `CategoryName`, `Description`und `Picture`. Zusätzlich zu diesen Feldern müssen wir eine neue hinzufügen, die auf die-Kategorie der Kategorie zeigt (sofern vorhanden). Wechseln Sie zum Hinzufügen dieser Spalte zum Server-Explorer, führen Sie einen Drilldown in die Tabellen aus, klicken Sie mit der rechten Maustaste auf die `Categories` Tabelle, und wählen Sie Tabellen Definition öffnen (siehe Abbildung 5). Wenn die Server-Explorer nicht angezeigt wird, wählen Sie Sie aus, indem Sie im Menü Ansicht die Option Server-Explorer auswählen oder STRG + ALT + S drücken.

Fügen Sie der `Categories` Tabelle, die `BrochurePath` benannt ist, eine neue `varchar(200)` Spalte hinzu, die `NULL` s zulässt, und klicken Sie auf das Symbol "Speichern" (oder drücken Sie STRG + s).

[![der Tabelle "categories" eine "brochurepath"-Spalte hinzufügen](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Abbildung 5**: Hinzufügen einer `BrochurePath` Spalte zur `Categories` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image6.png))

## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Schritt 4: Aktualisieren der Architektur zur Verwendung der Spalten "`Picture`" und "`BrochurePath`"

Für den `CategoriesDataTable` in der Datenzugriffs Schicht (Data Access Layer, DAL) sind derzeit vier `DataColumn` s definiert: `CategoryID`, `CategoryName`, `Description`und `NumberOfProducts`. Wenn diese Datentabelle ursprünglich im Tutorial [Erstellen einer Datenzugriffs Schicht](../introduction/creating-a-data-access-layer-cs.md) entworfen wurde, hatten die `CategoriesDataTable` nur die ersten drei Spalten. die Spalte `NumberOfProducts` wurde im [Master/Detail mithilfe einer Auflistungs Liste mit Master Datensätzen mit einem DataList-Detail](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) -Tutorial hinzugefügt.

Wie unter *Erstellen einer Datenzugriffs Ebene*erläutert, bilden die DataTables im typisierten DataSet die Geschäftsobjekte. Die TableAdapters sind dafür verantwortlich, mit der Datenbank zu kommunizieren und die Geschäftsobjekte mit den Abfrage Ergebnissen aufzufüllen. Die `CategoriesDataTable` wird mit dem `CategoriesTableAdapter`aufgefüllt, das drei Methoden zum Abrufen von Daten enthält:

- `GetCategories()` führt die Haupt Abfrage des TableAdapter s aus und gibt die Felder `CategoryID`, `CategoryName`und `Description` aller Datensätze in der `Categories` Tabelle zurück. Die Haupt Abfrage wird von den automatisch generierten Methoden `Insert` und `Update` verwendet.
- `GetCategoryByCategoryID(categoryID)` gibt die Felder `CategoryID`, `CategoryName`und `Description` der Kategorie zurück, deren `CategoryID` *CategoryID*ist.
- `GetCategoriesAndNumberOfProducts()`: gibt die Felder `CategoryID`, `CategoryName`und `Description` für alle Datensätze in der `Categories` Tabelle zurück. Verwendet auch eine Unterabfrage, um die Anzahl der jeder Kategorie zugeordneten Produkte zurückzugeben.

Beachten Sie, dass keine dieser Abfragen die `Categories` Tabelle s `Picture` oder `BrochurePath` Spalten zurückgibt. und die `CategoriesDataTable` `DataColumn` s für diese Felder bereitstellen. Um mit den Bild-und `BrochurePath` Eigenschaften arbeiten zu können, müssen Sie Sie zunächst der `CategoriesDataTable` hinzufügen und dann die `CategoriesTableAdapter`-Klasse aktualisieren, um diese Spalten zurückzugeben.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Hinzufügen der`Picture`und`BrochurePath``DataColumn` s

Beginnen Sie, indem Sie diese beiden Spalten zum `CategoriesDataTable`hinzufügen. Klicken Sie mit der rechten Maustaste auf den Header `CategoriesDataTable` s, wählen Sie im Kontextmenü hinzufügen aus, und wählen Sie dann die Option Column aus. Dadurch wird eine neue `DataColumn` in der Datentabelle mit dem Namen "`Column1`" erstellt. Benennen Sie diese Spalte in `Picture`um. Legen Sie im Eigenschaftenfenster die Eigenschaft `DataColumn` s `DataType` auf `System.Byte[]` fest (Dies ist keine Option in der Dropdown Liste; Sie müssen Sie in eingeben).

[![erstellen Sie eine datacolenn mit dem Namen "Image", deren Datentyp "System. Byte []" ist.](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Abbildung 6**: Erstellen eines `DataColumn` namens `Picture` dessen `DataType` `System.Byte[]` ist ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image8.png))

Fügen Sie der Datentabelle eine weitere `DataColumn` hinzu, und benennen Sie Sie `BrochurePath` mit dem Standard `DataType` Wert (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Zurückgeben der Werte für`Picture`und`BrochurePath`aus dem TableAdapter

Nachdem diese beiden `DataColumn` s der `CategoriesDataTable`hinzugefügt wurden, können Sie die `CategoriesTableAdapter`aktualisieren. Wir könnten beide Spaltenwerte in der Haupt-TableAdapter-Abfrage zurückgegeben haben, aber dies würde die Binärdaten jedes Mal wiederholen, wenn die `GetCategories()`-Methode aufgerufen wurde. Aktualisieren Sie stattdessen die Haupt-TableAdapter-Abfrage so, dass `BrochurePath` zurückgegeben wird, und erstellen Sie eine zusätzliche Datenabruf Methode, die eine bestimmte Kategorie s `Picture` Spalte zurückgibt.

Klicken Sie zum Aktualisieren der TableAdapter-Haupt Abfrage mit der rechten Maustaste auf den Header `CategoriesTableAdapter` s, und wählen Sie im Kontextmenü die Option konfigurieren aus. Dadurch wird der Assistent für die Tabellen Adapter Konfiguration geöffnet, den wir in einer Reihe früherer Tutorials kennengelernt haben. Aktualisieren Sie die Abfrage, um die `BrochurePath` wiederholen, und klicken Sie auf Fertigstellen.

[![die Spaltenliste in der SELECT-Anweisung aktualisieren, damit auch "brochurepath" zurückgegeben wird.](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Abbildung 7**: Aktualisieren der Spaltenliste in der `SELECT`-Anweisung, um auch `BrochurePath` zurückzugeben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image10.png))

Wenn Sie Ad-hoc-SQL-Anweisungen für den TableAdapter verwenden, wird durch Aktualisieren der Spaltenliste in der Haupt Abfrage die Spaltenliste für alle `SELECT` Abfrage Methoden im TableAdapter aktualisiert. Dies bedeutet, dass die `GetCategoryByCategoryID(categoryID)`-Methode aktualisiert wurde, um die `BrochurePath` Spalte zurückzugeben. Dies ist möglicherweise beabsichtigt. Es wurde jedoch auch die Spaltenliste in der `GetCategoriesAndNumberOfProducts()`-Methode aktualisiert, wobei die Unterabfrage entfernt wird, die die Anzahl der Produkte für jede Kategorie zurückgibt. Daher müssen wir diese Methode `SELECT` Abfrage aktualisieren. Klicken Sie mit der rechten Maustaste auf die `GetCategoriesAndNumberOfProducts()` Methode, wählen Sie konfigurieren aus, und setzen Sie die `SELECT` Abfrage auf ihren ursprünglichen Wert zurück:

[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Erstellen Sie als nächstes eine neue TableAdapter-Methode, die eine bestimmte Kategorie s `Picture` Spaltenwert zurückgibt. Klicken Sie mit der rechten Maustaste auf den Header `CategoriesTableAdapter` s, und wählen Sie die Option Abfrage hinzufügen aus, um den Assistenten zum Konfigurieren von TableAdapter abzufragen Im ersten Schritt dieses Assistenten werden Sie gefragt, ob Sie Daten mit einer Ad-hoc-SQL-Anweisung, einer neuen gespeicherten Prozedur oder einer vorhandenen Abfrage Abfragen möchten. Wählen Sie SQL-Anweisungen verwenden, und klicken Sie auf weiter Da wir eine Zeile zurückgeben, wählen Sie im zweiten Schritt die Option Select What Returns Rows aus.

[![wählen Sie die Option SQL-Anweisungen verwenden aus.](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Abbildung 8**: Auswählen der Option "SQL-Anweisungen verwenden" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image12.png))

[![, da die Abfrage einen Datensatz aus der Categories-Tabelle zurückgibt, wählen Sie auswählen, wodurch Zeilen zurückgegeben werden.](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Abbildung 9**: da die Abfrage einen Datensatz aus der Categories-Tabelle zurückgibt, wählen Sie auswählen, wodurch die Zeilen zurückgegeben werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image14.png)).

Geben Sie im dritten Schritt die folgende SQL-Abfrage ein, und klicken Sie auf Weiter:

[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

Der letzte Schritt besteht darin, den Namen für die neue Methode auszuwählen. Verwenden Sie `FillCategoryWithBinaryDataByCategoryID` und `GetCategoryWithBinaryDataByCategoryID` für die Datentabelle ausfüllen und Datentabelle zurückgeben. Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

[![wählen Sie die Namen für die TableAdapter s-Methoden aus.](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Abbildung 10**: Wählen Sie die Namen für die TableAdapter s-Methoden aus ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image16.png)).

> [!NOTE]
> Nachdem Sie den Assistenten zum Konfigurieren der Tabellen Adapter Abfrage abgeschlossen haben, wird möglicherweise ein Dialogfeld angezeigt, das Sie darüber informiert, dass der neue Befehls Text Daten zurückgibt, die sich vom Schema der Haupt Abfrage unterscheiden. Kurz gesagt: der Assistent gibt an, dass die Haupt Abfrage `GetCategories()` des TableAdapter s ein anderes Schema zurückgibt als das, das wir soeben erstellt haben. Aber dies ist das, was wir wollen, damit Sie diese Meldung ignorieren können.

Beachten Sie auch, dass bei Verwendung von Ad-hoc-SQL-Anweisungen und der Verwendung des Assistenten zum Ändern der Haupt Abfrage des TableAdapter s zu einem späteren Zeitpunkt geändert wird, dass die Spaltenliste der `GetCategoryWithBinaryDataByCategoryID`-Methode s `SELECT` Anweisung so geändert wird, dass nur die Spalten aus der Haupt Abfrage eingeschlossen werden (d. h., die `Picture` Spalte wird aus der Abfrage entfernt). Sie müssen die Spaltenliste manuell aktualisieren, um die `Picture` Spalte zurückzugeben, ähnlich wie bei der `GetCategoriesAndNumberOfProducts()`-Methode weiter oben in diesem Schritt.

Nach dem Hinzufügen der beiden `DataColumn` s zum `CategoriesDataTable` und der `GetCategoryWithBinaryDataByCategoryID`-Methode zum `CategoriesTableAdapter`sollten diese Klassen im typisierten DataSet-Designer wie der Screenshot in Abbildung 11 aussehen.

![Der DataSet-Designer enthält die neuen Spalten und Methoden.](uploading-files-cs/_static/image11.gif)

**Abbildung 11**: der DataSet-Designer enthält die neuen Spalten und Methoden.

## <a name="updating-the-business-logic-layer-bll"></a>Aktualisieren der Geschäftslogik Schicht (Business Logic Layer, BLL)

Wenn die DAL aktualisiert wurde, besteht nur noch die Erweiterung der Geschäftslogik Schicht (BLL), um eine Methode für die neue `CategoriesTableAdapter` Methode einzuschließen. Fügen Sie der `CategoriesBLL`-Klasse die folgende Methode hinzu:

[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Schritt 5: Hochladen einer Datei vom Client auf den Webserver

Beim Sammeln von Binärdaten werden diese Daten oft von einem Endbenutzer bereitgestellt. Um diese Informationen zu erfassen, muss der Benutzer in der Lage sein, eine Datei von Ihrem Computer auf den Webserver hochzuladen. Die hochgeladenen Daten müssen dann in das Datenmodell integriert werden. Dies kann bedeuten, dass die Datei im Dateisystem des Webserver gespeichert wird und ein Pfad zur Datei in der Datenbank hinzugefügt oder der binäre Inhalt direkt in die Datenbank geschrieben wird. In diesem Schritt wird erläutert, wie Sie es Benutzern ermöglichen, Dateien von Ihrem Computer auf den Server hochzuladen. Im nächsten Tutorial wird die Integration der hochgeladenen Datei in das Datenmodell berücksichtigt.

ASP.NET 2,0 s neues [FileUpload-websteuer](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) Element bietet einen Mechanismus, mit dem Benutzer eine Datei von Ihrem Computer an den Webserver senden können. Das FileUpload-Steuerelement wird als `<input>` Element gerendert, dessen `type`-Attribut auf File festgelegt ist. die Browser werden als Textfeld mit einer Schaltfläche zum Durchsuchen angezeigt. Wenn Sie auf die Schaltfläche Durchsuchen klicken, wird ein Dialogfeld geöffnet, in dem der Benutzer eine Datei auswählen kann. Wenn das Formular zurückgesendet wird, werden die Inhalte der ausgewählten Dateien zusammen mit dem Postback gesendet. Auf der Serverseite sind Informationen über die hochgeladene Datei über die Eigenschaften des FileUpload-Steuer Elements zugänglich.

Um das Hochladen von Dateien zu veranschaulichen, öffnen Sie die Seite `FileUpload.aspx` im Ordner `BinaryData`, ziehen Sie ein FileUpload-Steuerelement aus der Toolbox auf den Designer, und legen Sie die Eigenschaft des Steuer Elements `ID` auf `UploadTest`fest. Fügen Sie als nächstes ein Schaltflächen-websteuer Element hinzu, indem Sie dessen `ID` und `Text` Eigenschaften `UploadButton` und die ausgewählte Datei hochladen. Platzieren Sie abschließend ein Label-websteuer Element unterhalb der Schaltfläche, löschen Sie die `Text`-Eigenschaft, und legen Sie die `ID`-Eigenschaft auf `UploadDetails`fest.

[![der Seite "ASP.net" ein FileUpload-Steuerelement hinzufügen](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Abbildung 12**: Hinzufügen eines FileUpload-Steuer Elements zur Seite "ASP.net" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image18.png))

In Abbildung 13 wird diese Seite angezeigt, wenn Sie in einem Browser angezeigt wird. Wenn Sie auf die Schaltfläche Durchsuchen klicken, wird ein Dialogfeld für die Dateiauswahl angezeigt, in dem der Benutzer eine Datei auf dem Computer auswählen kann. Nachdem eine Datei ausgewählt wurde, wird durch Klicken auf die Schaltfläche ausgewählte Datei hochladen ein Postback ausgelöst, bei dem der Binär Inhalt der ausgewählten Datei an den Webserver gesendet wird.

[![kann der Benutzer eine Datei auswählen, die von seinem Computer auf den Server hochgeladen werden soll.](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Abbildung 13**: der Benutzer kann eine Datei auswählen, die von seinem Computer auf den Server hochgeladen werden soll ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image20.png))

Beim Postback kann die hochgeladene Datei im Dateisystem gespeichert werden, oder die zugehörigen Binärdaten können direkt über einen Stream bearbeitet werden. Erstellen Sie in diesem Beispiel einen `~/Brochures` Ordner, und speichern Sie die hochgeladene Datei dort. Fügen Sie zunächst den `Brochures` Ordner der Website als Unterordner des Stamm Verzeichnisses hinzu. Erstellen Sie als nächstes einen Ereignishandler für das `UploadButton` s `Click`-Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Das FileUpload-Steuerelement bietet eine Vielzahl von Eigenschaften zum Arbeiten mit den hochgeladenen Daten. Die [`HasFile`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) gibt beispielsweise an, ob eine Datei vom Benutzer hochgeladen wurde, während die [`FileBytes`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) den Zugriff auf die hochgeladenen Binärdaten als Bytearray ermöglicht. Der `Click` Ereignishandler stellt sicher, dass eine Datei hochgeladen wurde. Wenn eine Datei hochgeladen wurde, wird in der Bezeichnung der Name der hochgeladenen Datei, die Größe in Bytes und deren Inhaltstyp angezeigt.

> [!NOTE]
> Um sicherzustellen, dass der Benutzer eine Datei hochlädt, können Sie die `HasFile`-Eigenschaft überprüfen und eine Warnung anzeigen, wenn Sie `false`, oder Sie können stattdessen das Steuerelement "Requirements [dfieldvalidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) " verwenden.

Das FileUpload s-`SaveAs(filePath)` speichert die hochgeladene Datei im angegebenen *FilePath*. " *FilePath* " muss ein *physischer Pfad* (`C:\Websites\Brochures\SomeFile.pdf`) anstelle eines *virtuellen* *Pfads* (`/Brochures/SomeFile.pdf`) sein. Die [`Server.MapPath(virtPath)`-Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) nimmt einen virtuellen Pfad an und gibt den zugehörigen physischen Pfad zurück. Hier ist der virtuelle Pfad `~/Brochures/fileName`, wobei *filename* der Name der hochgeladenen Datei ist. Weitere Informationen zu virtuellen und physischen Pfaden und zum Verwenden von `Server.MapPath`finden Sie unter Verwenden von " [Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) ".

Nachdem Sie den `Click`-Ereignishandler abgeschlossen haben, nehmen Sie sich einen Moment Zeit, um die Seite in einem Browser zu testen. Klicken Sie auf die Schaltfläche Durchsuchen, wählen Sie eine Datei auf der Festplatte aus, und klicken Sie auf die Schaltfläche ausgewählte Datei hochladen Das Postback sendet den Inhalt der ausgewählten Datei an den Webserver, der Informationen über die Datei anzeigt, bevor Sie im Ordner "`~/Brochures`" gespeichert wird. Kehren Sie nach dem Hochladen der Datei zu Visual Studio zurück, und klicken Sie im Projektmappen-Explorer auf die Schaltfläche Aktualisieren. Die Datei, die Sie soeben hochgeladen haben, sollte in den Ordner ~/brochures angezeigt werden.

[![die Datei "evolutional. jpg" auf den Webserver hochgeladen wurde.](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Abbildung 14**: der Datei `EvolutionValley.jpg` wurde auf den Webserver hochgeladen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](uploading-files-cs/_static/image22.png))

!["Evolutionvalley. jpg" wurde im Ordner "~/Brochures" gespeichert.](uploading-files-cs/_static/image15.gif)

**Abbildung 15**: `EvolutionValley.jpg` im Ordner "`~/Brochures`" gespeichert

## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Feinheiten beim Speichern von hochgeladenen Dateien im Datei System

Es gibt mehrere Feinheiten, die beim Speichern des Hochladens von Dateien in das Dateisystem des Webserver s behandelt werden müssen. Erstens gibt es das Problem der Sicherheit. Um eine Datei im Dateisystem zu speichern, muss der Sicherheitskontext, unter dem die ASP.NET Seite ausgeführt wird, über Schreibberechtigungen verfügen. Der ASP.NET Development-Webserver wird im Kontext Ihres aktuellen Benutzerkontos ausgeführt. Wenn Sie Microsoft s Internetinformationsdienste (IIS) als Webserver verwenden, hängt der Sicherheitskontext von der IIS-Version und der zugehörigen Konfiguration ab.

Eine weitere Herausforderung, Dateien im Dateisystem zu speichern, dreht sich um das Benennen der Dateien. Zurzeit speichert unsere Seite alle hochgeladenen Dateien im `~/Brochures` Verzeichnis mit dem gleichen Namen wie die Datei auf dem Client Computer. Wenn Benutzer a eine Broschüre mit dem Namen `Brochure.pdf`hochlädt, wird die Datei als `~/Brochure/Brochure.pdf`gespeichert. Was passiert jedoch, wenn Benutzer a später eine andere Datei mit dem gleichen Dateinamen (`Brochure.pdf`) hochladen? Mit dem Code, den wir jetzt haben, wird der Benutzer eine s-Datei mit Upload von Benutzer B s überschrieben.

Es gibt eine Reihe von Techniken zum Auflösen von Dateinamen Konflikten. Eine Möglichkeit besteht darin, das Hochladen einer Datei zu verbieten, wenn bereits eine Datei mit demselben Namen vorhanden ist. Wenn Benutzer b versucht, eine Datei mit dem Namen "`Brochure.pdf`" hochzuladen, speichert das System die Datei nicht und zeigt stattdessen eine Meldung an, in der Benutzer b zum Umbenennen der Datei und wiederholen des Vorgangs angezeigt wird. Ein anderer Ansatz besteht darin, die Datei mit einem eindeutigen Dateinamen zu speichern, bei dem es sich um einen [Globally Unique Identifier (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) oder den Wert aus den entsprechenden Primärschlüssel Spalten des Datensatzes handelt (vorausgesetzt, der Upload ist einer bestimmten Zeile im Datenmodell zugeordnet). Im nächsten Tutorial werden diese Optionen ausführlicher erläutert.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Herausforderungen bei sehr großen Mengen an Binärdaten

In diesen Tutorials wird davon ausgegangen, dass die Größe der aufgezeichneten Binärdaten sehr gering ist. Das Arbeiten mit sehr großen Binärdaten Dateien mit mehreren Megabyte oder mehr führt zu neuen Herausforderungen, die den Rahmen dieser Tutorials sprengen. Beispielsweise lehnt ASP.NET standardmäßig Uploads von mehr als 4 MB ab, obwohl dies über das [`<httpRuntime>`-Element](https://msdn.microsoft.com/library/e1f13641.aspx) in `Web.config`konfiguriert werden kann. IIS erzwingt auch eigene Größenbeschränkungen für Datei Uploads. Weitere Informationen finden Sie unter [IIS-Uploaddatei](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) . Außerdem kann die Zeit, die zum Hochladen großer Dateien benötigt wird, den Standardwert von 110 Sekunden überschreiten ASP.net wartet auf eine Anforderung. Es gibt auch Arbeitsspeicher-und Leistungsprobleme, die bei der Arbeit mit großen Dateien auftreten.

Das FileUpload-Steuerelement ist für große Datei Uploads nicht praktikabel. Wenn der Inhalt der Datei an den Server gesendet wird, muss der Endbenutzer geduldig warten, bis er bestätigt, dass sein Upload fortschreitet. Dies stellt bei kleineren Dateien, die innerhalb weniger Sekunden hochgeladen werden können, kein Problem dar. es kann jedoch ein Problem sein, wenn größere Dateien verarbeitet werden müssen, die möglicherweise einige Minuten in Anspruch nehmen. Es gibt eine Vielzahl von Steuerelementen für das Hochladen von Dateien von Drittanbietern, die für die Verarbeitung großer Uploads besser geeignet sind. viele dieser Anbieter bieten Status Indikatoren und ActiveX-Uploadmanager, die eine viel höhere Benutzer Darstellung bieten.

Wenn Ihre Anwendung große Dateien verarbeiten muss, müssen Sie die Herausforderungen sorgfältig untersuchen und geeignete Lösungen für Ihre speziellen Anforderungen finden.

## <a name="summary"></a>Zusammenfassung

Das Erstellen einer Anwendung, die Binärdaten erfassen muss, führt zu einer Reihe von Herausforderungen. In diesem Tutorial haben wir die ersten beiden untersucht: entscheiden, wo die Binärdaten gespeichert werden sollen, und ermöglichen einem Benutzer, Binär Inhalte über eine Webseite hochzuladen. In den nächsten drei Tutorials erfahren Sie, wie die hochgeladenen Daten einem Datensatz in der Datenbank zugeordnet werden, und wie die Binärdaten zusammen mit den Text Datenfeldern angezeigt werden.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weitere nützliche Informationen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Verwenden von Datentypen mit umfangreichen Werten](https://msdn.microsoft.com/library/ms178158.aspx)
- [Schnellstarts für FileUpload-Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Das ASP.NET 2,0 FileUpload-Server Steuerelement](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Die dunkle Seite der Datei Uploads.](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Teresa Murphy und Bernadette Leigh. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Weiter](displaying-binary-data-in-the-data-web-controls-cs.md)
