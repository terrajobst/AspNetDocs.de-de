---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Abfragen von Daten mit dem SqlDataSource-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir das ObjectDataSource-Steuerelement verwendet, um die Präsentationsschicht vollständig von der Datenzugriffs Ebene zu trennen. Ab diesem Dozenten...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 199ddb46e877c3a0937672d33241a240660684da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606030"
---
# <a name="querying-data-with-the-sqldatasource-control-vb"></a>Abfragen von Daten mit dem SqlDataSource-Steuerelement (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) oder [PDF herunterladen](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> In den vorherigen Tutorials haben wir das ObjectDataSource-Steuerelement verwendet, um die Präsentationsschicht vollständig von der Datenzugriffs Ebene zu trennen. Ab diesem Tutorial erfahren Sie, wie das SqlDataSource-Steuerelement für einfache Anwendungen verwendet werden kann, die keine strikte Trennung der Präsentation und des Datenzugriffs erfordern.

## <a name="introduction"></a>Einführung

Alle Lernprogramme, die wir bisher geprüft haben, haben eine mehrstufige Architektur verwendet, die aus Präsentations-, Geschäftslogik-und Datenzugriffs Schichten besteht. Die Datenzugriffs Schicht (Data Access Layer, DAL) wurde im ersten Tutorial ([Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md)) und der Geschäftslogik Schicht im zweiten Schritt ([Erstellen einer Geschäftslogik Schicht) erstellt](../introduction/creating-a-business-logic-layer-vb.md). Beginnend mit dem Tutorial [Anzeigen von Daten mit dem ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) -Tutorial haben Sie erfahren, wie Sie das neue ObjectDataSource-Steuerelement ASP.NET 2,0 s verwenden, um eine deklarative Schnittstelle mit der Architektur von der Präsentationsschicht zu erhalten.

Obwohl alle Tutorials bisher die Architektur verwendet haben, um mit Daten zu arbeiten, ist es auch möglich, Datenbankdaten direkt von einer ASP.NET-Seite aus zu aktualisieren, einzufügen, zu aktualisieren und zu löschen, wobei die Architektur umgangen wird. Dadurch werden die spezifischen Datenbankabfragen und die Geschäftslogik direkt auf der Webseite platziert. Bei ausreichend großen oder komplexen Anwendungen ist das Entwerfen, implementieren und Verwenden einer mehrstufigen Architektur äußerst wichtig für die erfolgreiche, Aktualisierbarkeit und Verwaltbarkeit der Anwendung. Das Entwickeln einer robusten Architektur ist jedoch möglicherweise unnötig, wenn überaus einfache, einmalige Anwendungen erstellt werden.

ASP.NET 2,0 bietet fünf integrierte Datenquellen-Steuerelemente: [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)und [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource kann verwendet werden, um direkt aus einer relationalen Datenbank auf Daten zuzugreifen und diese zu ändern, einschließlich Microsoft SQL Server, Microsoft Access, Oracle, MySQL und anderen. In diesem Tutorial und den nächsten drei untersuchen wir, wie Sie mit dem SqlDataSource-Steuerelement arbeiten und wie Sie Datenbankdaten Abfragen und Filtern und wie Sie SqlDataSource verwenden, um Daten einzufügen, zu aktualisieren und zu löschen.

![ASP.NET 2,0 enthält fünf integrierte Datenquellen-Steuerelemente.](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Abbildung 1**: ASP.NET 2,0 enthält fünf integrierte Datenquellen-Steuerelemente

## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Vergleichen von "ObjectDataSource" und "SqlDataSource"

Konzeptionell sind sowohl die ObjectDataSource-als auch die SqlDataSource-Steuerelemente einfach Proxys zu Daten. Wie im Tutorial [Anzeigen von Daten mit dem ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) -Tutorial erläutert, verfügt ObjectDataSource über Eigenschaften, die den Objekttyp angeben, der die Daten und die aufzurufenden Methoden zum auswählen, einfügen, aktualisieren und Löschen von Daten aus dem zugrunde liegenden Objekttyp bereitstellt. Nachdem die Eigenschaften von ObjectDataSource s konfiguriert wurden, kann ein datenweb Steuerelement wie z. b. GridView, DetailsView oder DataList an das Steuerelement gebunden werden, wobei die Methoden `Select()`, `Insert()`, `Delete()`und `Update()` von ObjectDataSource zum interagieren mit der zugrunde liegenden Architektur verwendet werden.

SqlDataSource bietet die gleiche Funktionalität, funktioniert jedoch nicht mit einer Objektbibliothek, sondern mit einer relationalen Datenbank. Mit SqlDataSource müssen wir die Daten bankverbindungs Zeichenfolge und die Ad-hoc-SQL-Abfragen oder gespeicherten Prozeduren angeben, die ausgeführt werden sollen, um Daten einzufügen, zu aktualisieren, zu löschen und abzurufen. Die SqlDataSource-Methoden `Select()`, `Insert()`, `Update()`und `Delete()`, wenn Sie aufgerufen werden, stellen eine Verbindung mit der angegebenen Datenbank her und geben die entsprechende SQL-Abfrage aus. Wie das folgende Diagramm veranschaulicht, führen diese Methoden die grunt-Arbeit aus, um eine Verbindung mit einer Datenbank herzustellen, eine Abfrage auszugeben und die Ergebnisse zurückzugeben.

![SqlDataSource dient als Proxy für die Datenbank.](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Abbildung 2**: SqlDataSource dient als Proxy für die Datenbank.

> [!NOTE]
> In diesem Tutorial konzentrieren wir uns auf das Abrufen von Daten aus der Datenbank. Im Tutorial [Einfügen, aktualisieren und Löschen von Daten mit dem SqlDataSource-Steuer](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) Element erfahren Sie, wie Sie SqlDataSource konfigurieren, um das Einfügen, aktualisieren und löschen zu unterstützen.

## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Die SqlDataSource-und AccessDataSource-Steuerelemente

Zusätzlich zum SqlDataSource-Steuerelement enthält ASP.NET 2,0 auch ein AccessDataSource-Steuerelement. Diese zwei verschiedenen Steuerelemente leiten viele Entwickler neu in ASP.NET 2,0, um zu vermuten, dass das AccessDataSource-Steuerelement ausschließlich für den Microsoft-Zugriff mit dem SqlDataSource-Steuerelement entworfen wurde, das ausschließlich für die Microsoft SQL Server verwendet werden soll. Obwohl die AccessDataSource speziell für die Verwendung mit Microsoft Access entwickelt wurde, funktioniert das SqlDataSource-Steuerelement mit *allen* relationalen Datenbanken, auf die über .NET zugegriffen werden kann. Dies umfasst u. a. alle OLE DB-oder ODBC-kompatiblen Datenspeicher, z. b. Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL und PostgreSQL.

Der einzige Unterschied zwischen den Steuerelementen AccessDataSource und SqlDataSource besteht darin, wie die Daten bankverbindungs Informationen angegeben werden. Das AccessDataSource-Steuerelement benötigt lediglich den Dateipfad zur Access-Datenbankdatei. Die SqlDataSource hingegen erfordert eine komplette Verbindungs Zeichenfolge.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Schritt 1: Erstellen der "SqlDataSource"-Webseiten

Bevor wir untersuchen, wie Sie mit dem SqlDataSource-Steuerelement direkt mit Datenbankdaten arbeiten, sollten Sie sich zunächst einen Moment Zeit nehmen, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses Tutorial benötigen, und die nächsten drei. Fügen Sie zunächst einen neuen Ordner mit dem Namen `SqlDataSource`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`

![Fügen Sie die ASP.NET-Seiten für die SqlDataSource-bezogenen Tutorials hinzu.](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Abbildung 3**: Hinzufügen der ASP.NET-Seiten für die auf SqlDataSource bezogenen Tutorials

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `SqlDataSource` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Abbildung 4**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))

Fügen Sie schließlich diese vier Seiten als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie dem DataList-und Repeater-`<siteMapNode>`insbesondere das folgende Markup hinzu, um die benutzerdefinierten Schaltflächen hinzuzufügen:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials zum Bearbeiten, einfügen und löschen.

![Die Site Übersicht enthält jetzt Einträge für die SqlDataSource-Tutorials.](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Abbildung 5**: die Site Übersicht enthält jetzt Einträge für die SqlDataSource-Tutorials.

## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Schritt 2: Hinzufügen und Konfigurieren des SqlDataSource-Steuer Elements

Öffnen Sie zunächst die Seite `Querying.aspx` im Ordner `SqlDataSource`, und wechseln Sie zu Designansicht. Ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox auf den Designer, und legen Sie dessen `ID` auf `ProductsDataSource`fest. Wie bei ObjectDataSource erzeugt SqlDataSource keine gerenderte Ausgabe und wird daher als graues Feld auf der Entwurfs Oberfläche angezeigt. Um SqlDataSource zu konfigurieren, klicken Sie auf den Link Datenquelle konfigurieren des Smarttags SqlDataSource s.

![Klicken Sie im smarttagtag SqlDataSource auf den Link Datenquelle konfigurieren.](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Abbildung 6**: Klicken Sie auf den Link "Datenquelle konfigurieren" aus dem Smarttag SqlDataSource s.

Dadurch wird der Assistent zum Konfigurieren von Datenquellen des SqlDataSource-Steuer Elements angezeigt. Während sich die Schritte des Assistenten von den ObjectDataSource-Steuerelementen unterscheiden, ist das Endergebnis identisch, um die Details zum Abrufen, einfügen, aktualisieren und Löschen von Daten über die Datenquelle bereitzustellen. Für SqlDataSource bedeutet dies, die zugrunde liegende Datenbank anzugeben, die verwendet werden soll, und die Ad-hoc-SQL-Anweisungen oder gespeicherte Prozeduren bereitzustellen

Im ersten Schritt des Assistenten werden wir zur Eingabe der Datenbank aufgefordert. Die Dropdown Liste enthält die Datenbanken, die sich im Ordner "Webanwendungen" `App_Data` befinden, sowie die Datenbanken, die dem Knoten "Datenverbindungen" in der Server-Explorer hinzugefügt wurden. Da wir bereits eine Verbindungs Zeichenfolge für die `NORTHWIND.MDF`-Datenbank im Ordner "`App_Data`" zu unserer `Web.config`-Datei des Projekts hinzugefügt haben, enthält die Dropdown Liste einen Verweis auf die Verbindungs Zeichenfolge, `NORTHWINDConnectionString`. Wählen Sie dieses Element in der Dropdown Liste aus, und klicken Sie auf Weiter.

![Wählen Sie NorthwindConnectionString aus der Dropdown Liste aus.](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Abbildung 7**: Auswählen des `NORTHWINDConnectionString` aus der Dropdown Liste

Nachdem Sie die Datenbank ausgewählt haben, fragt der Assistent nach der Abfrage ab, die Daten zurückgeben soll. Wir können entweder die Spalten einer Tabelle oder Sicht angeben, die zurückgegeben werden sollen, oder Sie können eine benutzerdefinierte SQL-Anweisung oder eine gespeicherte Prozedur angeben. Sie können zwischen dieser Auswahl wechseln, indem Sie die Options Felder benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben und Spalten aus einer Tabelle oder Sicht angeben.

> [!NOTE]
> Verwenden Sie für dieses erste Beispiel die Option Spalten aus einer Tabelle oder Sicht angeben. Wir kehren später in diesem Tutorial zum Assistenten zurück und untersuchen die Option benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben.

Abbildung 8 zeigt den Bildschirm Select-Anweisung konfigurieren, wenn das Optionsfeld Spalten aus einer Tabelle oder Sicht angeben ausgewählt ist. Die Dropdown Liste enthält den Satz von Tabellen und Sichten in der Northwind-Datenbank, wobei die Spalten der ausgewählten Tabelle oder Sicht in der Liste unten angezeigt werden. Geben Sie in diesem Beispiel die Spalten `ProductID`, `ProductName`und `UnitPrice` aus der `Products` Tabelle zurück. Wie in Abbildung 8 gezeigt, zeigt der Assistent die resultierende SQL-Anweisung `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`an, nachdem er diese Auswahl getroffen hat.

![Rückgabe von Daten aus der Products-Tabelle](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Abbildung 8**: Zurückgeben von Daten aus der `Products` Tabelle

Nachdem Sie den Assistenten so konfiguriert haben, dass er die Spalten `ProductID`, `ProductName`und `UnitPrice` aus der `Products` Tabelle zurückgibt, klicken Sie auf die Schaltfläche Weiter. Dieser letzte Bildschirm bietet die Möglichkeit, die Ergebnisse der Abfrage zu untersuchen, die Sie im vorherigen Schritt konfiguriert haben. Durch Klicken auf die Schaltfläche Test Abfrage wird die konfigurierte `SELECT`-Anweisung ausgeführt und die Ergebnisse in einem Raster angezeigt.

![Klicken Sie auf die Schaltfläche Test Abfrage, um die SELECT-Abfrage anzuzeigen](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Abbildung 9**: Klicken auf die Schaltfläche "Test Abfrage" zum Überprüfen der `SELECT` Abfrage

Zum Abschließen des Assistenten klicken Sie auf Fertig stellen.

Wie bei ObjectDataSource weist der SqlDataSource-Assistent nur den Eigenschaften des Steuer Elements Werte zu, nämlich die Eigenschaften [`ConnectionString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) und [`SelectCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) . Nachdem Sie den Assistenten abgeschlossen haben, sollte das deklarative Markup Ihres SqlDataSource-Steuer Elements in etwa wie folgt aussehen:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

Die `ConnectionString`-Eigenschaft stellt Informationen zum Herstellen einer Verbindung mit der-Datenbank bereit. Dieser Eigenschaft kann ein kompletter, hart codierte Verbindungs Zeichenfolgen-Wert zugewiesen werden, oder Sie kann auf eine Verbindungs Zeichenfolge in `Web.config`verweisen. Verwenden Sie die Syntax `<%$ expressionPrefix:expressionValue %>`, um auf einen Verbindungs Zeichen folgen Wert in "Web. config" zu verweisen. In der Regel ist *expressionPrefix* connectionStrings, und *expressionvalue* ist der Name der Verbindungs Zeichenfolge im [Abschnitt `Web.config``<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx). Die Syntax kann jedoch verwendet werden, um auf `<appSettings>` Elemente oder Inhalte aus Ressourcen Dateien zu verweisen. Weitere Informationen zu dieser Syntax finden Sie unter [Übersicht über ASP.net-Ausdrücke](https://msdn.microsoft.com/library/d5bd1tad.aspx) .

Die `SelectCommand`-Eigenschaft gibt die Ad-hoc-SQL-Anweisung oder die gespeicherte Prozedur an, die zum Zurückgeben der Daten ausgeführt wird.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Schritt 3: Hinzufügen eines datenweb-Steuer Elements und binden dieses Steuer Elements an SqlDataSource

Nachdem die SqlDataSource konfiguriert wurde, kann Sie an ein datenweb Steuerelement gebunden werden, z. b. an GridView oder DetailsView. In diesem Tutorial können Sie die Daten in einer GridView anzeigen. Ziehen Sie aus der Toolbox eine GridView auf die Seite, und binden Sie Sie an den `ProductsDataSource` SqlDataSource, indem Sie die Datenquelle in der Dropdown Liste des Smarttags GridView s auswählen.

[![eine GridView-Ansicht hinzuzufügen und Sie an das SqlDataSource-Steuerelement zu binden.](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Abbildung 10**: Hinzufügen einer GridView und Binden der Ansicht an das SqlDataSource-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))

Nachdem Sie das SqlDataSource-Steuerelement aus der Dropdown Liste im "GridView"-Smarttag ausgewählt haben, fügt Visual Studio der GridView automatisch ein BoundField-oder CheckBoxField-Element für jede Spalte hinzu, die vom Datenquellen-Steuerelement zurückgegeben wird. Da SqlDataSource drei Daten Bank Spalten `ProductID`, `ProductName`und `UnitPrice` zurückgibt, gibt es drei Felder in der GridView.

Nehmen Sie sich einen Moment Zeit, um die drei boundfields (GridView) zu konfigurieren. Ändern Sie die `ProductName` Feld s `HeaderText` Eigenschaft in Product Name und das Feld `UnitPrice` in Price. Formatieren Sie auch das `UnitPrice`-Feld als Währung. Nachdem Sie diese Änderungen vorgenommen haben, sollte das deklarative Markup der GridView s in etwa wie folgt aussehen:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Besuchen Sie diese Seite über einen Browser. Wie in Abbildung 11 dargestellt, listet die GridView die einzelnen Produkte `ProductID`, `ProductName`und `UnitPrice` Werte auf.

[![das GridView-Objekt die Werte "ProductID", "ProductName" und "UnitPrice" des Produkts anzeigt](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Abbildung 11**: in der GridView werden alle Produkte `ProductID`, `ProductName`und `UnitPrice` Werte angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif)).

Beim Besuch der Seite ruft das GridView-Steuerelement die `Select()`-Methode des Datenquellen-Steuer Elements auf. Als wir das ObjectDataSource-Steuerelement verwendet haben, wurde dies als `ProductsBLL` Class s `GetProducts()`-Methode bezeichnet. Mit SqlDataSource stellt die `Select()`-Methode jedoch eine Verbindung mit der angegebenen Datenbank her und gibt die `SelectCommand` aus (in diesem Beispiel`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`). Die SqlDataSource gibt die Ergebnisse zurück, die die GridView dann auflistet, und erstellt eine Zeile in der GridView für jeden zurückgegebenen Datenbankdaten Satz.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Die integrierten datenweb-Steuerelement Features und das SqlDataSource-Steuerelement

Im Allgemeinen sind die Features, die dem datenweb Steuerelement Paging, Sortierung, Bearbeitung, löschen, einfügen usw. unterliegen, für das datenweb-Steuerelement spezifisch und nicht vom verwendeten Datenquellen-Steuerelement abhängig. Das heißt, die GridView kann das integrierte Paging, sortieren, bearbeiten und Löschen verwenden, ob es an eine ObjectDataSource oder eine SqlDataSource gebunden ist. Bestimmte datenwebsteuerungs-Funktionen sind jedoch für das verwendete Datenquellen-Steuerelement oder die Konfiguration des Datenquellen Steuer Elements sensibel.

Beispielsweise wird im Tutorial [effizientes Paging durch große Datenmengen](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) erläutert, wie die Auslagerungs Logik für die datenweb Steuerelemente standardmäßig *alle* Datensätze aus der zugrunde liegenden Datenquelle zurückgibt und dann nur die entsprechende Teilmenge der Datensätze anzeigt, wenn der aktuelle Seitenindex und die Anzahl der Datensätze pro Seite angezeigt werden. Dieses Modell ist beim Paging durch ausreichend große Resultsets sehr ineffizient. Glücklicherweise kann die ObjectDataSource so konfiguriert werden, dass benutzerdefinierte Paginierung unterstützt werden, die nur die genaue Teilmenge der anzuzeigenden Datensätze zurückgibt. Im SqlDataSource-Steuerelement fehlen jedoch die Eigenschaften zum Implementieren von benutzerdefiniertem Paging.

Eine weitere Feinheit beim Paging und Sortieren ergibt sich aus der SqlDataSource. Standardmäßig können die von einer SqlDataSource zurückgegebenen Daten per Pager oder über die GridView-Struktur sortiert werden. Um dies zu veranschaulichen, aktivieren Sie die Option Paging aktivieren und Sortieren in der GridView s-Smarttag in `Querying.aspx`, und überprüfen Sie, ob dies erwartungsgemäß funktioniert.

Das Sortieren und Paging funktioniert, da die SqlDataSource die Datenbankdaten in ein lose typisiertes DataSet abruft. Die Gesamtanzahl der Datensätze, die von der Abfrage zurückgegeben werden, ist ein wichtiger Aspekt bei der Implementierung von Paging. Darüber hinaus können die Ergebnisse des Datasets durch eine DataView sortiert werden. Diese Funktionen werden automatisch von SqlDataSource verwendet, wenn die GridView auslagerbare oder sortierte Daten anfordert.

SqlDataSource kann so konfiguriert werden, dass anstelle eines Datasets ein DataReader-Objekt zurückgegeben wird, indem die [`DataSourceMode`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) von `DataSet` (Standard) in `DataReader`geändert wird. Die Verwendung eines DataReader kann in Situationen bevorzugt werden, in denen die Ergebnisse von SqlDataSource s an vorhandenen Code übergeben werden, der einen DataReader erwartet. Da DataReaders erheblich einfachere Objekte sind als Datasets, bieten Sie außerdem eine bessere Leistung. Wenn Sie diese Änderung vornehmen, kann das datenweb Steuerelement jedoch weder sortieren noch seitenweise, da SqlDataSource nicht ermitteln kann, wie viele Datensätze von der Abfrage zurückgegeben werden, und der DataReader bietet keine Techniken zum Sortieren der zurückgegebenen Daten.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Schritt 4: Verwenden einer benutzerdefinierten SQL-Anweisung oder gespeicherten Prozedur

Beim Konfigurieren des SqlDataSource-Steuer Elements kann die zum Zurückgeben von Daten verwendete Abfrage in einem von zwei Ansätzen als benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur oder als Spalten aus einer vorhandenen Tabelle oder Sicht angegeben werden. In Schritt 2 haben wir die Auswahl von Spalten aus der `Products` Tabelle untersucht. Sehen Sie sich die Verwendung einer benutzerdefinierten SQL-Anweisung an.

Fügen Sie der `Querying.aspx` Seite ein weiteres GridView-Steuerelement hinzu, und wählen Sie aus der Dropdown Liste im Smarttags eine neue Datenquelle aus. Geben Sie als nächstes an, dass die Daten aus einer Datenbank abgerufen werden, sodass ein neues SqlDataSource-Steuerelement erstellt wird. Benennen Sie das Steuerelement `ProductsWithCategoryInfoDataSource`.

![Erstellen Sie ein neues SqlDataSource-Steuerelement mit dem Namen productwithcategoryinfodatasource.](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Abbildung 12**: Erstellen eines neuen SqlDataSource-Steuer Elements namens `ProductsWithCategoryInfoDataSource`

Im nächsten Bildschirm werden Sie aufgefordert, die Datenbank anzugeben. Wählen Sie in Abbildung 7 die `NORTHWINDConnectionString` aus der Dropdown Liste aus, und klicken Sie auf Weiter. Wählen Sie im Bildschirm "SELECT-Anweisung konfigurieren" das Optionsfeld benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben aus, und klicken Sie auf Weiter. Dadurch wird der Bildschirm benutzerdefinierte Anweisungen oder gespeicherte Prozeduren definieren angezeigt, der Registerkarten mit den Bezeichnungen SELECT, Update, INSERT und DELETE enthält. Auf jeder Registerkarte können Sie eine benutzerdefinierte SQL-Anweisung in das Textfeld eingeben oder eine gespeicherte Prozedur aus der Dropdown Liste auswählen. In diesem Tutorial erfahren Sie, wie Sie eine benutzerdefinierte SQL-Anweisung eingeben. Das nächste Tutorial enthält ein Beispiel, in dem eine gespeicherte Prozedur verwendet wird.

![Benutzerdefinierte SQL-Anweisung eingeben oder gespeicherte Prozedur auswählen](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Abbildung 13**: eingeben einer benutzerdefinierten SQL-Anweisung oder Auswählen einer gespeicherten Prozedur

Die benutzerdefinierte SQL-Anweisung kann per Hand in das Textfeld eingegeben werden oder durch Klicken auf die Schaltfläche "Abfrage-Generator" grafisch erstellt werden. Verwenden Sie die folgende Abfrage aus dem Abfrage-Generator oder dem Textfeld, um die Felder `ProductID` und `ProductName` aus der `Products` Tabelle zurückzugeben. verwenden Sie dazu ein `JOIN`, um die Product s `CategoryName` aus der `Categories` Tabelle abzurufen:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]

![Sie können die Abfrage mithilfe der Abfrage-Generator](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Abbildung 14**: Sie können die Abfrage mithilfe der Abfrage-Generator

Nachdem Sie die Abfrage angegeben haben, klicken Sie auf Weiter, um mit dem Bildschirm Test Abfrage fortzufahren. Klicken Sie auf Fertigstellen, um den SqlDataSource-Assistenten abzuschließen.

Nachdem Sie den Assistenten abgeschlossen haben, werden der GridView drei boundfields hinzugefügt, in denen die von der Abfrage zurückgegebenen Spalten `ProductID`, `ProductName`und `CategoryName` angezeigt werden und das folgende deklaratives Markup ergibt:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]

[![der GridView die einzelnen Produkt-ID, den Namen und den zugehörigen Kategorienamen anzeigt](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Abbildung 15**: die GridView zeigt die ID des Produkts, den Namen und den zugehörigen Kategorienamen[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))

## <a name="summary"></a>Summary

In diesem Tutorial wurde erläutert, wie Daten mithilfe des SqlDataSource-Steuer Elements abgefragt und angezeigt werden. Wie bei ObjectDataSource fungiert auch SqlDataSource als Proxy, der einen deklarativen Ansatz für den Zugriff auf Daten bereitstellt. Die Eigenschaften geben die Datenbank an, mit der eine Verbindung hergestellt werden soll, und die auszuführende SQL `SELECT` Abfrage. Sie können über die Eigenschaftenfenster oder mithilfe des Assistenten zum Konfigurieren von Datenquellen angegeben werden.

In den `SELECT` Abfrage Beispielen, die wir in diesem Tutorial überprüft haben, wurden alle Datensätze aus der angegebenen Abfrage zurückgegeben. Das SqlDataSource-Steuerelement kann jedoch eine `WHERE`-Klausel mit Parametern enthalten, deren Werte Programm gesteuert zugewiesen werden oder automatisch aus einer angegebenen Quelle abgerufen werden. Im nächsten Tutorial wird erläutert, wie parametrisierte Abfragen erstellt und verwendet werden.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Zugreifen auf relationale Datenbankdaten](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Übersicht über das SqlDataSource-Steuerelement](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [ASP.net Quick Start-Tutorials: das SqlDataSource-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Das Web. config-`<connectionStrings>` Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Daten bankverbindungs-Zeichen folgen Verweis](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Susan-Konstante, Bernadette Leigh und David suru. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [Weiter](using-parameterized-queries-with-the-sqldatasource-vb.md)
