---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Erstellen neuer gespeicherter Prozeduren für die TableAdapters des typisierten Datasets (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir SQL-Anweisungen in unserem Code erstellt und die-Anweisungen an die Datenbank weitergegeben, die ausgeführt werden soll. Ein alternativer Ansatz ist die Verwendung von s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a7cc890038e5bb4eb61c7c3b808154c196ab2423
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74605639"
---
# <a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Erstellen von neuen gespeicherten Prozeduren für die TableAdapter-Steuerelemente des typisierten DataSet (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) oder [PDF herunterladen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> In den vorherigen Tutorials haben wir SQL-Anweisungen in unserem Code erstellt und die-Anweisungen an die Datenbank weitergegeben, die ausgeführt werden soll. Ein alternativer Ansatz ist die Verwendung von gespeicherten Prozeduren, bei denen die SQL-Anweisungen in der Datenbank vordefiniert sind. In diesem Tutorial erfahren Sie, wie Sie mit dem TableAdapter-Assistenten neue gespeicherte Prozeduren für uns generieren.

## <a name="introduction"></a>Einführung

Die Datenzugriffs Schicht (Data Access Layer, DAL) für diese Tutorials verwendet typisierte Datasets. Wie im Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) erläutert, bestehen typisierte Datasets aus stark typisierten DataTables und TableAdapters. Die DataTables stellen die logischen Entitäten im System dar, während die TableAdapters-Schnittstelle mit der zugrunde liegenden Datenbank zum Ausführen des Datenzugriffs funktioniert. Dies umfasst das Auffüllen der DataTables mit Daten, das Ausführen von Abfragen, die skalare Daten zurückgeben, sowie das Einfügen, aktualisieren und Löschen von Datensätzen aus der Datenbank.

Die von den TableAdapters ausgeführten SQL-Befehle können entweder Ad-hoc-SQL-Anweisungen sein, z. b. `SELECT columnList FROM TableName`oder gespeicherte Prozeduren. Die TableAdapters in unserer Architektur verwenden Ad-hoc-SQL-Anweisungen. Viele Entwickler und Datenbankadministratoren bevorzugen jedoch gespeicherte Prozeduren über Ad-hoc-SQL-Anweisungen aus Gründen der Sicherheit, Wartbarkeit und Aktualisierbarkeit. Andere bevorzugen Ad-hoc-SQL-Anweisungen für Ihre Flexibilität. In meiner eigenen Arbeit bevorzuge ich gespeicherte Prozeduren für Ad-hoc-SQL-Anweisungen, entschied mich jedoch für die Verwendung von Ad-hoc-SQL-Anweisungen zur Vereinfachung der früheren Tutorials.

Wenn Sie einen TableAdapter definieren oder neue Methoden hinzufügen, erleichtert der TableAdapter s-Assistent das Erstellen neuer gespeicherter Prozeduren oder das Verwenden vorhandener gespeicherter Prozeduren, wie bei der Verwendung von Ad-hoc-SQL-Anweisungen. In diesem Tutorial wird erläutert, wie der TableAdapter s-Assistent die automatische Generierung gespeicherter Prozeduren durchführen kann. Im nächsten Tutorial wird erläutert, wie die TableAdapter s-Methoden für die Verwendung vorhandener oder manuell erstellter gespeicherter Prozeduren konfiguriert werden.

> [!NOTE]
> Weitere Informationen zu den vor-und Nachteile von gespeicherten Prozeduren und [AD](https://weblogs.asp.net/fbouma/) -hoc-SQL finden Sie im [Blogeintrag von](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) Rob Howard. die [gespeicherten Prozeduren sind schlecht, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) .

## <a name="stored-procedure-basics"></a>Grundlagen der gespeicherten Prozeduren

Funktionen sind ein Konstrukt, das für alle Programmiersprachen gemeinsam ist. Eine Funktion ist eine Auflistung von-Anweisungen, die ausgeführt werden, wenn die-Funktion aufgerufen wird. Funktionen können Eingabeparameter akzeptieren und optional einen Wert zurückgeben. *[Gespeicherte Prozeduren](http://en.wikipedia.org/wiki/Stored_procedure)* sind Datenbankkonstrukte, die viele Ähnlichkeiten mit Funktionen in Programmiersprachen aufweisen. Eine gespeicherte Prozedur besteht aus einem Satz von T-SQL-Anweisungen, die ausgeführt werden, wenn die gespeicherte Prozedur aufgerufen wird. Eine gespeicherte Prozedur akzeptiert möglicherweise null bis viele Eingabeparameter und kann skalare Werte, Ausgabeparameter oder, am häufigsten Resultsets, aus `SELECT` Abfragen zurückgeben.

> [!NOTE]
> Gespeicherte Prozeduren werden oft als Sprocs oder SPS bezeichnet.

Gespeicherte Prozeduren werden mit der [`CREATE PROCEDURE`](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) t-SQL-Anweisung erstellt. Das folgende T-SQL-Skript erstellt z. b. eine gespeicherte Prozedur mit dem Namen `GetProductsByCategoryID`, die einen einzelnen Parameter mit dem Namen `@CategoryID` annimmt und die Felder `ProductID`, `ProductName`, `UnitPrice`und `Discontinued` der Spalten in der `Products` Tabelle zurückgibt, die einen entsprechenden `CategoryID` Wert aufweisen:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Nachdem diese gespeicherte Prozedur erstellt wurde, kann Sie mithilfe der folgenden Syntax aufgerufen werden:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Im nächsten Tutorial erfahren Sie, wie Sie gespeicherte Prozeduren über die Visual Studio-IDE erstellen. In diesem Tutorial soll der TableAdapter-Assistent die gespeicherten Prozeduren für uns jedoch automatisch generieren.

Zusätzlich zur einfachen Rückgabe von Daten werden gespeicherte Prozeduren häufig verwendet, um mehrere Daten Bank Befehle innerhalb des Gültigkeits Bereichs einer einzelnen Transaktion auszuführen. Eine gespeicherte Prozedur mit dem Namen `DeleteCategory`beispielsweise könnte einen `@CategoryID`-Parameter annehmen und zwei `DELETE`-Anweisungen ausführen: zuerst eine zum Löschen der zugehörigen Produkte und ein zweites, das die angegebene Kategorie löscht. Mehrere Anweisungen in einer gespeicherten Prozedur werden *nicht* automatisch in eine Transaktion umschließt. Weitere T-SQL-Befehle müssen ausgegeben werden, um sicherzustellen, dass die gespeicherten Prozeduren mehrere Befehle als atomarer Vorgang behandelt werden. Im nachfolgenden Tutorial wird erläutert, wie Sie Befehle von gespeicherten Prozeduren innerhalb des Umfangs einer Transaktion umschließen.

Bei der Verwendung von gespeicherten Prozeduren in einer Architektur rufen die Methoden der Datenzugriffs Ebene eine bestimmte gespeicherte Prozedur auf, anstatt eine Ad-hoc-SQL-Anweisung auszugeben. Dadurch wird der Speicherort der SQL-Anweisungen, die ausgeführt werden (in der Datenbank), zentralisiert, anstatt Sie in der Architektur der Anwendung zu definieren. Diese Zentralisierung vereinfacht das Auffinden, analysieren und Optimieren der Abfragen und bietet ein viel deutlicheres Bild, wo und wie die Datenbank verwendet wird.

Weitere Informationen zu den Grundlagen gespeicherter Prozeduren finden Sie in den Ressourcen im Abschnitt Weitere Informationen am Ende dieses Tutorials.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Schritt 1: Erstellen der erweiterten Szenarios für die Datenzugriffs Schicht-Webseiten

Bevor wir uns mit der Erstellung einer dal mithilfe gespeicherter Prozeduren auseinandersetzen, nehmen Sie sich zunächst einen Moment Zeit, um die ASP.NET-Seiten in unserem Website Projekt zu erstellen, die wir für dieses und die nächsten verschiedenen Tutorials benötigen. Fügen Sie zunächst einen neuen Ordner mit dem Namen `AdvancedDAL`hinzu. Fügen Sie dann die folgenden ASP.NET-Seiten zu diesem Ordner hinzu, und stellen Sie sicher, dass Sie die einzelnen Seiten der `Site.master` Master Seite zuordnen:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`

![Fügen Sie die ASP.NET-Seiten für Szenarios mit erweiterten Datenzugriffsebenen-Szenarien hinzu.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen der ASP.NET-Seiten für Szenarios mit erweiterten Datenzugriffsebenen-Szenarios

Wie in den anderen Ordnern werden `Default.aspx` im Ordner `AdvancedDAL` die Lernprogramme in diesem Abschnitt auflisten. Denken Sie daran, dass das `SectionLevelTutorialListing.ascx` Benutzer Steuerelement diese Funktionalität bereitstellt. Fügen Sie dieses Benutzer Steuerelement daher `Default.aspx` hinzu, indem Sie es aus dem Projektmappen-Explorer auf die Seite s Designansicht ziehen.

[![das Benutzer Steuerelement "sectionleveltutoriallisting. ascx" zu "default. aspx" hinzufügen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen des `SectionLevelTutorialListing.ascx` Benutzer Steuer Elements zu `Default.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))

Fügen Sie diese Seiten schließlich als Einträge zur `Web.sitemap` Datei hinzu. Fügen Sie insbesondere nach dem `<siteMapNode>`arbeiten mit Batch Daten das folgende Markup hinzu:

[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Nehmen Sie sich nach dem Aktualisieren `Web.sitemap`einen Moment Zeit, um die Tutorials-Website über einen Browser anzuzeigen. Das Menü auf der linken Seite enthält jetzt Elemente für die Tutorials für erweiterte dal-Szenarios.

![Die Site Übersicht enthält jetzt Einträge für die Tutorials zu erweiterten dal-Szenarios.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Abbildung 3**: die Site Übersicht enthält jetzt Einträge für die Tutorials zu erweiterten dal-Szenarios.

## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Schritt 2: Konfigurieren eines TableAdapters zum Erstellen neuer gespeicherter Prozeduren

Zum Veranschaulichen der Erstellung einer Datenzugriffs Ebene, die gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen verwendet, erstellen Sie ein neues typisiertes DataSet im `~/App_Code/DAL` Ordner mit dem Namen `NorthwindWithSprocs.xsd`. Da wir diesen Prozess in vorherigen Tutorials ausführlich durchlaufen haben, werden wir die hier beschriebenen Schritte schnell durchführen. Wenn Sie hängen bleiben oder weitere Schritt-für-Schritt-Anleitungen zum Erstellen und Konfigurieren eines typisierten Datasets benötigen, lesen Sie das Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) .

Fügen Sie dem Projekt ein neues Dataset hinzu, indem Sie mit der rechten Maustaste auf den Ordner `DAL` klicken, neues Element hinzufügen auswählen und die DataSet-Vorlage auswählen, wie in Abbildung 4 dargestellt.

[![dem Projekt ein neues typisiertes DataSet mit dem Namen northwindwithsprocs. xsd hinzufügen.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Abbildung 4**: Hinzufügen eines neuen typisierten Datasets zum Projekt mit dem Namen `NorthwindWithSprocs.xsd` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))

Dadurch wird das neue typisierte DataSet erstellt, der zugehörige Designer geöffnet, ein neuer TableAdapter erstellt und der TableAdapter-Konfigurations-Assistent gestartet. Im ersten Schritt des TableAdapter-Konfigurations-Assistenten werden Sie aufgefordert, die Datenbank auszuwählen, mit der Sie arbeiten möchten. Die Verbindungs Zeichenfolge für die Northwind-Datenbank sollte in der Dropdown Liste aufgeführt werden. Wählen Sie diesen aus, und klicken Sie auf weiter

Auf dem nächsten Bildschirm können Sie auswählen, wie der TableAdapter auf die Datenbank zugreifen soll. In den vorherigen Tutorials haben wir die erste Option "SQL-Anweisungen verwenden" ausgewählt. Wählen Sie für dieses Tutorial die zweite Option aus, erstellen Sie neue gespeicherte Prozeduren, und klicken Sie auf Weiter.

[![den TableAdapter anzuweisen, neue gespeicherte Prozeduren zu erstellen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Abbildung 5**: anweisen des TableAdapters, neue gespeicherte Prozeduren zu erstellen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))

Wie bei der Verwendung von Ad-hoc-SQL-Anweisungen werden wir im folgenden Schritt aufgefordert, die `SELECT`-Anweisung für die Haupt Abfrage TableAdapter s bereitzustellen. Anstatt jedoch die hier eingegebene `SELECT`-Anweisung zu verwenden, um eine Ad-hoc-Abfrage direkt auszuführen, erstellt der TableAdapter s-Assistent eine gespeicherte Prozedur, die diese `SELECT` Abfrage enthält.

Verwenden Sie die folgende `SELECT` Abfrage für diesen TableAdapter:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

[![Sie die SELECT-Abfrage ein.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Abbildung 6**: eingeben der `SELECT` Abfrage ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))

> [!NOTE]
> Die obige Abfrage unterscheidet sich geringfügig von der Haupt Abfrage der `ProductsTableAdapter` im `Northwind` typisierten DataSet. Beachten Sie, dass die `ProductsTableAdapter` im `Northwind` typisierten Dataset zwei korrelierte Unterabfragen enthält, um den Kategorien Amen und den Firmennamen für die einzelnen Produkt-und Lieferanten Kategorien wiederholen zu können. Im nächsten Tutorial zum [Aktualisieren des TableAdapter zum Verwenden von Joins](updating-the-tableadapter-to-use-joins-vb.md) wird das Hinzufügen dieser verknüpften Daten zu diesem TableAdapter untersucht.

Nehmen Sie sich einen Moment Zeit, um auf die Schaltfläche Erweiterte Optionen Hier können Sie angeben, ob der Assistent auch INSERT-, Update-und DELETE-Anweisungen für den TableAdapter generieren soll, ob die optimistische Parallelität verwendet werden soll und ob die Datentabelle nach Einfügungen und Updates aktualisiert werden soll. Die Option INSERT-, Update-und DELETE-Anweisungen generieren ist standardmäßig aktiviert. Lassen Sie die Option aktiviert. Lassen Sie für dieses Tutorial die Option optimistische Parallelität verwenden deaktiviert.

Wenn die gespeicherten Prozeduren automatisch mit dem TableAdapter-Assistenten erstellt werden, wird die Option Datentabelle aktualisieren ignoriert. Unabhängig davon, ob dieses Kontrollkästchen aktiviert ist, rufen die sich ergebenden gespeicherten Prozeduren INSERT und Update den soeben eingefügten oder soeben aktualisierten Datensatz ab, wie in Schritt 3 zu sehen.

![Lassen Sie die Option INSERT-, Update-und DELETE-Anweisungen generieren aktiviert.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Abbildung 7**: belassen der Option INSERT-, Update-und DELETE-Anweisungen generieren aktiviert

> [!NOTE]
> Wenn die Option "vollständige Parallelität verwenden" aktiviert ist, fügt der Assistent der `WHERE`-Klausel weitere Bedingungen hinzu, mit denen verhindert wird, dass Daten aktualisiert werden, wenn andere Felder geändert wurden. Weitere Informationen zur Verwendung der integrierten Funktion für die vollständige Parallelitäts Steuerung von TableAdapter finden Sie im Tutorial zum [Implementieren von optimistischer](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) Parallelität.

Nachdem Sie die `SELECT` Abfrage eingegeben und bestätigt haben, dass die Option INSERT-, Update-und DELETE-Anweisungen generieren aktiviert ist, klicken Sie auf Weiter. Auf dem nächsten Bildschirm, der in Abbildung 8 dargestellt wird, werden die Namen der gespeicherten Prozeduren angezeigt, die der Assistent zum auswählen, einfügen, aktualisieren und Löschen von Daten erstellt. Ändern Sie die Namen dieser gespeicherten Prozeduren in `Products_Select`, `Products_Insert`, `Products_Update`und `Products_Delete`.

[Umbenennen der gespeicherten Prozeduren ![](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Abbildung 8**: Umbenennen der gespeicherten Prozeduren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

Zum Anzeigen des T-SQL-Assistenten, der vom TableAdapter-Assistenten zum Erstellen der vier gespeicherten Prozeduren verwendet wird, klicken Sie auf die Schaltfläche SQL-Skript Im Dialogfeld Vorschau-SQL-Skript können Sie das Skript in einer Datei speichern oder in die Zwischenablage kopieren.

![Vorschau des zum Generieren der gespeicherten Prozeduren verwendeten SQL-Skripts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Abbildung 9**: Vorschau des zum Generieren der gespeicherten Prozeduren verwendeten SQL-Skripts

Nachdem Sie die gespeicherten Prozeduren benannt haben, klicken Sie auf Weiter, um die entsprechenden Methoden für TableAdapter zu benennen Ebenso wie bei der Verwendung von Ad-hoc-SQL-Anweisungen können wir Methoden erstellen, die eine vorhandene Datentabelle ausfüllen oder eine neue zurückgeben. Wir können auch angeben, ob der TableAdapter das DB-Direct-Muster zum Einfügen, aktualisieren und Löschen von Datensätzen enthalten soll. Lassen Sie alle drei Kontrollkästchen aktiviert, benennen Sie jedoch die Methode zum Zurückgeben einer Datentabelle in `GetProducts` um (siehe Abbildung 10).

[![den Namen der Methoden Fill und GetProducts.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Abbildung 10**: Benennen der Methoden `Fill` und `GetProducts` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))

Klicken Sie auf Weiter, um eine Zusammenfassung der Schritte anzuzeigen, die der Assistent ausführt. Beenden Sie den Assistenten durch Klicken auf die Schaltfläche Fertigstellen. Sobald der Assistent abgeschlossen ist, werden Sie an den DataSet-Designer zurückgegeben, der nun die `ProductsDataTable`enthalten sollte.

[![der DataSet-Designer zeigt das neu hinzugefügte productdatabel-Element an.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Abbildung 11**: der DataSet s-Designer zeigt das neu hinzugefügte `ProductsDataTable` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))

## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Schritt 3: Untersuchen der neu erstellten gespeicherten Prozeduren

Mit dem TableAdapter-Assistenten, der in Schritt 2 verwendet wurde, wurden automatisch die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren und Löschen von Daten erstellt. Diese gespeicherten Prozeduren können über Visual Studio angezeigt oder geändert werden, indem Sie auf den Server-Explorer klicken und einen Drilldown in den Ordner gespeicherte Prozeduren der Datenbank ausführen. Wie in Abbildung 12 gezeigt, enthält die Northwind-Datenbank vier neue gespeicherte Prozeduren: `Products_Delete`, `Products_Insert`, `Products_Select`und `Products_Update`.

![Die vier gespeicherten Prozeduren, die in Schritt 2 erstellt wurden, befinden sich im Ordner gespeicherte Prozeduren der Datenbank.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Abbildung 12**: die vier gespeicherten Prozeduren, die in Schritt 2 erstellt wurden, befinden sich im Ordner gespeicherte Prozeduren der Datenbank.

> [!NOTE]
> Wenn das Server-Explorer nicht angezeigt wird, klicken Sie auf das Menü Ansicht, und wählen Sie die Option Server-Explorer. Wenn die produktbezogenen gespeicherten Prozeduren aus Schritt 2 nicht angezeigt werden, klicken Sie mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie aktualisieren aus.

Um eine gespeicherte Prozedur anzuzeigen oder zu ändern, doppelklicken Sie in der Server-Explorer auf den Namen, oder klicken Sie alternativ mit der rechten Maustaste auf die gespeicherte Prozedur, und wählen Sie öffnen aus. Abbildung 13 zeigt die gespeicherte Prozedur `Products_Delete`, wenn Sie geöffnet wird.

[![gespeicherte Prozeduren können innerhalb von Visual Studio geöffnet und geändert werden.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Abbildung 13**: gespeicherte Prozeduren können innerhalb von Visual Studio geöffnet und geändert werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))

Der Inhalt der gespeicherten Prozeduren `Products_Delete` und `Products_Select` ist recht unkompliziert. Die gespeicherten Prozeduren `Products_Insert` und `Products_Update` haben dagegen eine genauere Betrachtung, da beide eine `SELECT`-Anweisung nach Ihren `INSERT`-und `UPDATE`-Anweisungen ausführen. Der folgende SQL-Code bildet z. b. die gespeicherte Prozedur `Products_Insert`:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Die gespeicherte Prozedur akzeptiert die `Products` Spalten, die von der im TableAdapter-Assistenten angegebenen `SELECT` Abfrage zurückgegeben wurden, als Eingabeparameter, und diese Werte werden in einer `INSERT`-Anweisung verwendet. Nach der `INSERT`-Anweisung wird eine `SELECT` Abfrage verwendet, um die `Products` Spaltenwerte (einschließlich der `ProductID`) des neu hinzugefügten Datensatzes zurückzugeben. Diese Aktualisierungs Funktion ist hilfreich, wenn ein neuer Datensatz mit dem Batch Aktualisierungs Muster hinzugefügt wird, da automatisch die neu hinzugefügten `ProductRow` Instanzen `ProductID` Eigenschaften mit den automatisch inkrementierten Werten aktualisiert werden, die von der Datenbank zugewiesen werden.

Der folgende Code veranschaulicht diese Funktion. Sie enthält einen `ProductsTableAdapter` und `ProductsDataTable`, der für das `NorthwindWithSprocs` typisierte DataSet erstellt wurde. Ein neues Produkt wird der Datenbank hinzugefügt, indem eine `ProductsRow` Instanz erstellt wird, die Werte bereitgestellt werden und die TableAdapter s `Update`-Methode aufgerufen wird. dabei wird die `ProductsDataTable`übergeben. Intern listet die TableAdapter s-`Update`-Methode die `ProductsRow` Instanzen in der übergeordneten Datentabelle auf (in diesem Beispiel gibt es nur einen, den wir soeben hinzugefügt haben) und führt den entsprechenden INSERT-, Update-oder DELETE-Befehl aus. In diesem Fall wird die gespeicherte Prozedur `Products_Insert` ausgeführt, wodurch der `Products` Tabelle ein neuer Datensatz hinzugefügt und die Details des neu hinzugefügten Datensatzes zurückgegeben werden. Der Wert der `ProductsRow` Instanz s `ProductID` wird dann aktualisiert. Nachdem die `Update`-Methode abgeschlossen wurde, können Sie über die Eigenschaft `ProductsRow` s `ProductID` auf den neu hinzugefügten Datensatz s `ProductID` Wert zugreifen.

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

Die gespeicherte Prozedur `Products_Update` enthält auf ähnliche Weise eine `SELECT`-Anweisung nach der `UPDATE`-Anweisung.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Beachten Sie, dass diese gespeicherte Prozedur zwei Eingabeparameter für `ProductID`enthält: `@Original_ProductID` und `@ProductID`. Diese Funktion ermöglicht Szenarien, in denen der Primärschlüssel geändert werden kann. Beispielsweise kann in einer Mitarbeiter Datenbank jeder Mitarbeiterdaten Satz die Sozialversicherungsnummer des Mitarbeiters als Primärschlüssel verwenden. Um eine vorhandene Sozialversicherungsnummer eines Mitarbeiters zu ändern, müssen sowohl die neue Sozialversicherungsnummer als auch die ursprüngliche Sozialversicherungsnummer angegeben werden. Diese Funktionalität wird für die `Products` Tabelle nicht benötigt, da die `ProductID` Spalte eine `IDENTITY` Spalte ist und nie geändert werden sollte. Tatsächlich ist die `UPDATE`-Anweisung in der gespeicherten Prozedur `Products_Update` nicht die `ProductID` Spalte in der zugehörigen Spaltenliste enthalten. Obwohl `@Original_ProductID` in der `UPDATE` Anweisung s `WHERE`-Klausel verwendet wird, ist Sie für die `Products` Tabelle überflüssig und kann durch den `@ProductID` Parameter ersetzt werden. Wenn Sie die Parameter einer gespeicherten Prozedur ändern, ist es wichtig, dass die TableAdapter-Methoden, die diese gespeicherte Prozedur verwenden, ebenfalls aktualisiert werden.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Schritt 4: Ändern der Parameter einer gespeicherten Prozedur und Aktualisieren des TableAdapter

Da der `@Original_ProductID`-Parameter überflüssig ist, entfernen Sie ihn aus der `Products_Update` gespeicherten Prozedur. Öffnen Sie die gespeicherte Prozedur `Products_Update`, löschen Sie den Parameter `@Original_ProductID`, und ändern Sie in der `WHERE`-Klausel der `UPDATE`-Anweisung den von `@Original_ProductID` verwendeten Parameternamen in `@ProductID`. Nachdem Sie diese Änderungen vorgenommen haben, sollte T-SQL in der gespeicherten Prozedur wie folgt aussehen:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Um diese Änderungen in der Datenbank zu speichern, klicken Sie auf der Symbolleiste auf das Symbol speichern, oder drücken Sie STRG + S. An diesem Punkt erwartet die gespeicherte Prozedur `Products_Update` keinen `@Original_ProductID` Input-Parameter, aber der TableAdapter ist so konfiguriert, dass er einen solchen Parameter übergibt. Sie können die Parameter anzeigen, die der TableAdapter an die gespeicherte Prozedur `Products_Update` sendet, indem Sie den TableAdapter im DataSet-Designer auswählen, zum Eigenschaftenfenster navigieren und auf die Ellipsen in der `Parameters` Auflistung `UpdateCommand` s klicken. Dadurch wird das Dialogfeld Parameter Sammlungs-Editor angezeigt, das in Abbildung 14 angezeigt wird.

![Der Parameter Sammlungs-Editor listet die Parameter auf, die an die gespeicherte Prozedur Products_Update verwendet werden.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Abbildung 14**: der Parameter Sammlungs-Editor listet die Parameter auf, die an die gespeicherte Prozedur `Products_Update` verwendet werden.

Sie können diesen Parameter hier entfernen, indem Sie einfach den `@Original_ProductID`-Parameter aus der Liste der Elemente auswählen und auf die Schaltfläche Entfernen klicken.

Alternativ können Sie die für alle Methoden verwendeten Parameter aktualisieren, indem Sie im Designer mit der rechten Maustaste auf den TableAdapter klicken und konfigurieren auswählen. Dadurch wird der TableAdapter-Konfigurations-Assistent angezeigt, in dem die für das auswählen, einfügen, aktualisieren und löschen verwendeten gespeicherten Prozeduren zusammen mit den Parametern aufgelistet werden, die von den gespeicherten Prozeduren erwartet werden. Wenn Sie auf die Dropdown Liste aktualisieren klicken, sehen Sie, dass die `Products_Update` gespeicherten Prozeduren die Eingabeparameter erwartet haben, die jetzt nicht mehr `@Original_ProductID` enthalten (siehe Abbildung 15). Klicken Sie einfach auf "Fertigstellen", um die vom TableAdapter verwendete Parameter Sammlung automatisch zu aktualisieren.

[![Alternativ können Sie den Konfigurations-Assistenten für TableAdapter s verwenden, um die Methoden Parameter Sammlungen zu aktualisieren.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Abbildung 15**: Alternativ können Sie den Konfigurations-Assistenten für TableAdapter s verwenden, um die Methoden Parameter Auflistungen zu aktualisieren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png)).

## <a name="step-5-adding-additional-tableadapter-methods"></a>Schritt 5: Hinzufügen zusätzlicher TableAdapter-Methoden

Wie in Schritt 2 dargestellt, ist es bei der Erstellung eines neuen TableAdapters einfach, die entsprechenden gespeicherten Prozeduren automatisch zu generieren. Dasselbe gilt, wenn einem TableAdapter zusätzliche Methoden hinzugefügt werden. Um dies zu veranschaulichen, fügen Sie dem in Schritt 2 erstellten `ProductsTableAdapter` eine `GetProductByProductID(productID)`-Methode hinzu. Diese Methode nimmt einen `ProductID` Wert als Eingabe an und gibt Details zu dem angegebenen Produkt zurück.

Klicken Sie zunächst mit der rechten Maustaste auf den TableAdapter, und wählen Sie im Kontextmenü Abfrage hinzufügen aus.

![Neue Abfrage zum TableAdapter hinzufügen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Abbildung 16**: Hinzufügen einer neuen Abfrage zum TableAdapter

Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen gestartet, der zuerst auffordert, wie der TableAdapter auf die Datenbank zugreifen soll. Um eine neue gespeicherte Prozedur zu erstellen, wählen Sie die Option neue gespeicherte Prozedur erstellen aus, und klicken Sie auf Weiter.

[Wählen Sie ![die Option neue gespeicherte Prozedur erstellen.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Abbildung 17**: Auswählen der Option zum Erstellen einer neuen gespeicherten Prozedur ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))

Im nächsten Bildschirm werden wir aufgefordert, den Typ der auszuführenden Abfrage zu identifizieren, ob ein Satz von Zeilen oder ein einzelner Skalarwert zurückgegeben wird oder ob eine `UPDATE`-, `INSERT`-oder `DELETE`-Anweisung ausgeführt werden soll. Da die `GetProductByProductID(productID)`-Methode eine Zeile zurückgibt, lassen Sie die Option SELECT, die die Zeile zurückgibt ausgewählt und dann auf Weiter.

[![wählen Sie die Option Select What Returns Row aus.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Abbildung 18**: Auswählen der Option auswählen, welche Zeile zurückgibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))

Im nächsten Bildschirm wird die Haupt Abfrage TableAdapter s angezeigt, die nur den Namen der gespeicherten Prozedur (`dbo.Products_Select`) auflistet. Ersetzen Sie den Namen der gespeicherten Prozedur durch die folgende `SELECT`-Anweisung, die alle Produktfelder für ein bestimmtes Produkt zurückgibt:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]

[![den Namen der gespeicherten Prozedur durch eine SELECT-Abfrage ersetzen.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Abbildung 19**: Ersetzen des Namens einer gespeicherten Prozedur durch eine `SELECT` Abfrage ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))

Auf dem nachfolgenden Bildschirm werden Sie aufgefordert, die gespeicherte Prozedur zu benennen, die erstellt wird. Geben Sie den Namen `Products_SelectByProductID`, und klicken Sie auf Weiter.

[![der neuen gespeicherten Prozedur den Namen Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Abbildung 20**: Benennen der neuen gespeicherten Prozedur `Products_SelectByProductID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))

Der letzte Schritt des Assistenten ermöglicht es uns, die generierten Methodennamen zu ändern und anzugeben, ob das Muster Fill a databel verwendet werden soll, ob ein Datentabelle oder beides zurückgegeben werden soll. Lassen Sie für diese Methode beide Optionen aktiviert, benennen Sie jedoch die Methoden in `FillByProductID` und `GetProductByProductID`um. Klicken Sie auf Weiter, um eine Zusammenfassung der vom Assistenten ausgeführten Schritte anzuzeigen, und klicken Sie dann auf Fertigstellen, um den Assistenten abzuschließen.

[![die TableAdapter s-Methoden in fillbyproductid und getproductbyproductid umbenennen.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Abbildung 21**: Umbenennen der TableAdapter s-Methoden in `FillByProductID` und `GetProductByProductID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))

Nachdem Sie den Assistenten abgeschlossen haben, ist für den TableAdapter eine neue Methode verfügbar, `GetProductByProductID(productID)`, die die soeben erstellte `Products_SelectByProductID` gespeicherte Prozedur ausführt, wenn Sie aufgerufen wird. Nehmen Sie sich einen Moment Zeit, um diese neue gespeicherte Prozedur aus der Server-Explorer anzuzeigen, indem Sie einen Drilldown in den Ordner gespeicherte Prozeduren ausführen und `Products_SelectByProductID` öffnen (wenn Sie ihn nicht sehen, klicken Sie mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren und wählen

Beachten Sie, dass die gespeicherte Prozedur `SelectByProductID` `@ProductID` als Eingabeparameter annimmt und die `SELECT`-Anweisung ausführt, die wir im Assistenten eingegeben haben.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Schritt 6: Erstellen einer Klasse für die Geschäftslogik Schicht

In der tutorialreihe haben wir festgelegt, dass Sie eine geschichtete Architektur erhalten, in der die Präsentationsschicht alle Aufrufe an die Geschäftslogik Schicht (Business Logic Layer, BLL) gerichtet hat. Um diese Entwurfs Entscheidung einzuhalten, müssen wir zuerst eine BLL-Klasse für das neue typisierte DataSet erstellen, bevor wir auf die Produktdaten von der Präsentationsebene aus zugreifen können.

Erstellen Sie eine neue Klassendatei mit dem Namen `ProductsBLLWithSprocs.vb` im Ordner `~/App_Code/BLL`, und fügen Sie Ihr den folgenden Code hinzu:

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Diese Klasse imitiert die Semantik der `ProductsBLL`-Klasse aus früheren Tutorials, verwendet jedoch die Objekte `ProductsTableAdapter` und `ProductsDataTable` aus dem `NorthwindWithSprocs` DataSet. Anstatt z. b. eine `Imports NorthwindTableAdapters`-Anweisung am Anfang der Klassendatei zu haben, wie `ProductsBLL`, verwendet die `ProductsBLLWithSprocs`-Klasse `Imports NorthwindWithSprocsTableAdapters`. Ebenso werden die `ProductsDataTable`-und `ProductsRow` Objekte, die in dieser Klasse verwendet werden, dem `NorthwindWithSprocs` Namespace vorangestellt. Die `ProductsBLLWithSprocs`-Klasse bietet zwei Datenzugriffs Methoden `GetProducts` und `GetProductByProductID`sowie Methoden zum Hinzufügen, aktualisieren und Löschen einer einzelnen Produkt Instanz.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Schritt 7: Arbeiten mit dem`NorthwindWithSprocs`DataSet von der Präsentationsschicht

Nun haben wir eine dal erstellt, die gespeicherte Prozeduren verwendet, um auf die zugrunde liegenden Datenbankdaten zuzugreifen und diese zu ändern. Wir haben auch eine rudimentäre BLL mit Methoden erstellt, mit denen alle Produkte oder ein bestimmtes Produkt zusammen mit Methoden zum Hinzufügen, aktualisieren und Löschen von Produkten abgerufen werden können. Um dieses Lernprogramm zu beenden, können Sie eine ASP.NET-Seite erstellen, die die BLL s-`ProductsBLLWithSprocs` Klasse zum Anzeigen, aktualisieren und Löschen von Datensätzen verwendet.

Öffnen Sie die Seite `NewSprocs.aspx` im Ordner `AdvancedDAL`, und ziehen Sie eine GridView-Ansicht aus der Toolbox auf den Designer, und benennen Sie Sie `Products`. Wählen Sie aus dem GridView s-Smarttag die Bindung an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren Sie ObjectDataSource so, dass die `ProductsBLLWithSprocs`-Klasse verwendet wird, wie in Abbildung 22 dargestellt.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbllwithsprocs-Klasse.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Abbildung 22**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLLWithSprocs`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))

Die Dropdown Liste auf der Registerkarte auswählen verfügt über zwei Optionen: `GetProducts` und `GetProductByProductID`. Da wir alle Produkte in der GridView anzeigen möchten, wählen Sie die `GetProducts`-Methode aus. Die Dropdown Listen in den Registerkarten Update, INSERT und DELETE verfügen jeweils nur über eine einzige Methode. Stellen Sie sicher, dass für jede dieser Dropdown Listen die entsprechende Methode ausgewählt ist, und klicken Sie dann auf Fertigstellen.

Nachdem der ObjectDataSource-Assistent abgeschlossen wurde, fügt Visual Studio boundfields und ein CheckBoxField zur GridView für die Product Data-Felder hinzu. Aktivieren Sie die integrierten Funktionen zum Bearbeiten und Löschen von GridView-Funktionen, indem Sie die Optionen Aktivieren der Bearbeitung aktivieren und löschen aktivieren im Smarttags aktivieren.

[![die Seite eine GridView mit aktivierter Unterstützung für die Bearbeitung und Löschung enthält.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Abbildung 23**: die Seite enthält eine GridView mit aktivierter Unterstützung für die Bearbeitung und Löschung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))

Wie bereits in den vorherigen Tutorials erläutert, legt Visual Studio nach Abschluss des Assistenten für ObjectDataSource s die `OldValuesParameterFormatString`-Eigenschaft auf ursprüngliches\_{0}fest. Dies muss auf den Standardwert von {0} zurückgesetzt werden, damit die Daten Änderungs Funktionen mit den Parametern, die von den Methoden in unserer BLL erwartet werden, ordnungsgemäß funktionieren. Stellen Sie daher sicher, dass Sie die `OldValuesParameterFormatString`-Eigenschaft auf {0} festlegen, oder entfernen Sie die-Eigenschaft vollständig aus der deklarativen Syntax.

Nach dem Abschließen des Assistenten zum Konfigurieren von Datenquellen, beim Aktivieren der Unterstützung für das Bearbeiten und löschen in der GridView und beim Zurückgeben der Eigenschaft ObjectDataSource s `OldValuesParameterFormatString` auf den Standardwert, sollte das deklarative Markup der Seite in etwa wie folgt aussehen:

[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

An diesem Punkt könnten wir die GridView bereinigen, indem wir die Bearbeitungs Schnittstelle so anpassen, dass Sie überprüft werden kann, dass die `CategoryID`-und `SupplierID` Spalten als Dropdown Listen usw. angezeigt werden. Wir könnten der Schaltfläche "Löschen" auch eine Client seitige Bestätigung hinzufügen, und ich empfehle Ihnen, sich die Zeit zu nehmen, diese Verbesserungen zu implementieren. Da diese Themen bereits in vorherigen Tutorials behandelt wurden, werden Sie hier nicht näher behandelt.

Unabhängig davon, ob Sie die GridView verbessern, testen Sie die Core-Features der Seite in einem Browser. Wie in Abbildung 24 gezeigt, werden auf der Seite die Produkte in einer GridView aufgelistet, die Bearbeitungs-und Löschfunktionen pro Zeile bereitstellt.

[![können die Produkte in der GridView angezeigt, bearbeitet und gelöscht werden.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Abbildung 24**: die Produkte können in der GridView angezeigt, bearbeitet und gelöscht werden ([Klicken Sie, um das Bild in voller Größe anzuzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))

## <a name="summary"></a>Summary

Die TableAdapters in einem typisierten DataSet können mithilfe von Ad-hoc-SQL-Anweisungen oder gespeicherten Prozeduren auf Daten aus der Datenbank zugreifen. Beim Arbeiten mit gespeicherten Prozeduren können entweder vorhandene gespeicherte Prozeduren verwendet werden, oder der TableAdapter-Assistent kann angewiesen werden, neue gespeicherte Prozeduren auf der Grundlage einer `SELECT` Abfrage zu erstellen. In diesem Tutorial haben wir untersucht, wie die gespeicherten Prozeduren automatisch für uns erstellt werden.

Obwohl das automatische Generieren gespeicherter Prozeduren zu einer Zeitersparnis beiträgt, gibt es bestimmte Fälle, in denen die vom Assistenten erstellte gespeicherte Prozedur nicht an dem, was wir selbst erstellt hätten, ausgerichtet ist. Ein Beispiel hierfür ist die gespeicherte Prozedur `Products_Update`, die sowohl `@Original_ProductID` als auch `@ProductID` Eingabeparameter erwartet, obwohl der `@Original_ProductID` Parameter überflüssig war.

In vielen Szenarios wurden die gespeicherten Prozeduren möglicherweise bereits erstellt, oder wir möchten Sie ggf. manuell erstellen, um eine präzisere Steuerung der Befehle gespeicherter Prozeduren zu erhalten. In beiden Fällen möchten wir den TableAdapter anweisen, vorhandene gespeicherte Prozeduren für seine Methoden zu verwenden. Im nächsten Tutorial wird erläutert, wie Sie dies erreichen.

Fröhliche Programmierung!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den in diesem Tutorial behandelten Themen finden Sie in den folgenden Ressourcen:

- [Erstellen und Verwalten von gespeicherten Prozeduren](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Abrufen von skalaren Daten aus einer gespeicherten Prozedur](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Grundlagen der SQL Server gespeicherten Prozedur](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Gespeicherte Prozeduren: eine Übersicht](http://www.sqlteam.com/item.asp?ItemID=563)
- [Schreiben einer gespeicherten Prozedur](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Hilton geisenow. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Weiter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
