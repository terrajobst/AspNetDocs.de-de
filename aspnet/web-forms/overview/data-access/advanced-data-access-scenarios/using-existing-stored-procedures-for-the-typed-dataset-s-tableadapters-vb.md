---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Verwenden vorhandener gespeicherter Prozeduren für die TableAdapters des typisierten Datasets (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Im vorherigen Tutorial haben Sie erfahren, wie Sie den TableAdapter-Assistenten verwenden, um neue gespeicherte Prozeduren zu generieren. In diesem Tutorial erfahren Sie, wie der gleiche TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: e35c3d6a98516a07f6119e6cb9dbeb99bc28fe33
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74613504"
---
# <a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Verwenden von vorhandenen gespeicherten Prozeduren für die TableAdapter-Steuerelemente des typisierten DataSet (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) oder [PDF herunterladen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Im vorherigen Tutorial haben Sie erfahren, wie Sie den TableAdapter-Assistenten verwenden, um neue gespeicherte Prozeduren zu generieren. In diesem Tutorial erfahren Sie, wie der TableAdapter-Assistent mit vorhandenen gespeicherten Prozeduren arbeiten kann. Außerdem wird beschrieben, wie Sie der Datenbank manuell neue gespeicherte Prozeduren hinzufügen.

## <a name="introduction"></a>Einführung

Im [vorherigen Tutorial](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) haben wir gesehen, wie die TableAdapters für typisierte Datasets für die Verwendung gespeicherter Prozeduren für den Zugriff auf Daten anstelle von Ad-hoc-SQL-Anweisungen konfiguriert werden können. Insbesondere wurde untersucht, wie der TableAdapter-Assistent diese gespeicherten Prozeduren automatisch erstellt. Beim Portieren einer Legacy Anwendung auf ASP.NET 2,0 oder beim Aufbau einer ASP.NET 2,0-Website um ein vorhandenes Datenmodell besteht die Wahrscheinlichkeit, dass die Datenbank bereits die gespeicherten Prozeduren enthält, die wir benötigen. Alternativ empfiehlt es sich, die gespeicherten Prozeduren mit einem anderen Tool als dem TableAdapter-Assistenten zu erstellen, der automatisch die gespeicherten Prozeduren generiert.

In diesem Tutorial wird erläutert, wie Sie den TableAdapter für die Verwendung vorhandener gespeicherter Prozeduren konfigurieren. Da die Northwind-Datenbank nur über einen kleinen Satz integrierter gespeicherter Prozeduren verfügt, werden auch die erforderlichen Schritte zum manuellen Hinzufügen neuer gespeicherter Prozeduren zur Datenbank über die Visual Studio-Umgebung erläutert. Legen Sie Los!

> [!NOTE]
> Im Tutorial [Wrapping von Daten Bank Änderungen innerhalb einer Transaktion](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) haben wir dem TableAdapter Methoden hinzugefügt, um Transaktionen (`BeginTransaction`, `CommitTransaction`usw.) zu unterstützen. Alternativ können Transaktionen vollständig in einer gespeicherten Prozedur verwaltet werden, für die keine Änderungen am Code der Datenzugriffs Schicht erforderlich sind. In diesem Tutorial untersuchen wir die T-SQL-Befehle, die zum Ausführen von Anweisungen für gespeicherte Prozeduren innerhalb des Gültigkeits Bereichs einer Transaktion verwendet werden.

## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Schritt 1: Hinzufügen gespeicherter Prozeduren zur Datenbank Northwind

Visual Studio erleichtert das Hinzufügen neuer gespeicherter Prozeduren zu einer Datenbank. Fügen Sie der Northwind-Datenbank eine neue gespeicherte Prozedur hinzu, die alle Spalten aus der `Products` Tabelle für diejenigen mit einem bestimmten `CategoryID` Wert zurückgibt. Erweitern Sie im Fenster Server-Explorer die Datenbank Northwind, sodass Ihre Ordner Daten Bank Diagramme, Tabellen, Sichten usw. angezeigt werden. Wie bereits im vorherigen Tutorial gezeigt, enthält der Ordner gespeicherte Prozeduren die vorhandenen gespeicherten Prozeduren der Datenbank. Um eine neue gespeicherte Prozedur hinzuzufügen, klicken Sie einfach mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie im Kontextmenü die Option neue gespeicherte Prozedur hinzufügen aus.

[![mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und fügen Sie eine neue gespeicherte Prozedur](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Abbildung 1**: Klicken Sie[mit](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und fügen Sie eine neue gespeicherte Prozedur hinzu.

Wie in Abbildung 1 gezeigt, wird durch Auswählen der Option neue gespeicherte Prozedur hinzufügen ein Skript Fenster in Visual Studio mit der Gliederung des SQL-Skripts geöffnet, das zum Erstellen der gespeicherten Prozedur erforderlich ist. Es ist unsere Aufgabe, dieses Skript zu erstellen und auszuführen. zu diesem Zeitpunkt wird die gespeicherte Prozedur der Datenbank hinzugefügt.

Geben Sie das folgende Skript ein:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Bei der Ausführung dieses Skripts wird der Northwind-Datenbank mit dem Namen `Products_SelectByCategoryID`eine neue gespeicherte Prozedur hinzugefügt. Diese gespeicherte Prozedur akzeptiert einen einzelnen Eingabeparameter (`@CategoryID`vom Typ `int`) und gibt alle Felder für diese Produkte mit einem übereinstimmenden `CategoryID` Wert zurück.

Um dieses `CREATE PROCEDURE` Skript auszuführen und die gespeicherte Prozedur der Datenbank hinzuzufügen, klicken Sie auf das Symbol Speichern auf der Symbolleiste, oder drücken Sie STRG + S. Anschließend wird der Ordner für gespeicherte Prozeduren aktualisiert und zeigt die neu erstellte gespeicherte Prozedur. Außerdem ändert das Skript im-Fenster die Feinheiten von `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` in `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` fügt der Datenbank eine neue gespeicherte Prozedur hinzu, während `ALTER PROCEDURE` eine vorhandene gespeicherte Prozedur aktualisiert. Da der Start des Skripts in `ALTER PROCEDURE`geändert wurde, wird die gespeicherte Prozedur durch Ändern der Eingabeparameter oder SQL-Anweisungen für gespeicherte Prozeduren und durch Klicken auf das Symbol "Speichern" mit diesen Änderungen aktualisiert.

Abbildung 2 zeigt Visual Studio, nachdem die gespeicherte Prozedur `Products_SelectByCategoryID` gespeichert wurde.

[![die gespeicherte Prozedur Products_SelectByCategoryID der Datenbank hinzugefügt wurde.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Abbildung 2**: die gespeicherte Prozedur `Products_SelectByCategoryID` wurde der Datenbank hinzugefügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))

## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Schritt 2: Konfigurieren des TableAdapters für die Verwendung einer vorhandenen gespeicherten Prozedur

Nachdem die gespeicherte Prozedur `Products_SelectByCategoryID` der Datenbank hinzugefügt wurde, können wir die Datenzugriffs Ebene so konfigurieren, dass diese gespeicherte Prozedur verwendet wird, wenn eine ihrer Methoden aufgerufen wird. Insbesondere fügen wir der `ProductsTableAdapter` im `NorthwindWithSprocs` typisierten DataSet, das die soeben erstellte `Products_SelectByCategoryID` gespeicherte Prozedur aufruft, eine `GetProductsByCategoryID(<_i22_>categoryID)<!--_i22_-->`-Methode hinzu.

Öffnen Sie zunächst das `NorthwindWithSprocs` DataSet. Klicken Sie mit der rechten Maustaste auf den `ProductsTableAdapter`, und wählen Sie Abfrage hinzufügen aus, um den Assistenten für die Konfiguration von TableAdapter Im [vorherigen Tutorial](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) haben wir entschieden, dass der TableAdapter eine neue gespeicherte Prozedur für uns erstellt. In diesem Tutorial möchten wir jedoch die neue TableAdapter-Methode an die vorhandene gespeicherte Prozedur `Products_SelectByCategoryID` übertragen. Wählen Sie daher die Option vorhandene gespeicherte Prozedur verwenden im ersten Schritt des Assistenten aus, und klicken Sie dann auf Weiter.

[Wählen Sie die Option vorhandene gespeicherte Prozedur verwenden ![aus.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Abbildung 3**: Auswählen der Option zum Verwenden vorhandener gespeicherter Prozeduren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))

Der folgende Bildschirm enthält eine Dropdown Liste mit den gespeicherten Prozeduren der Datenbank. Wenn Sie eine gespeicherte Prozedur auswählen, werden die Eingabeparameter auf der linken Seite und die zurückgegebenen Datenfelder (sofern vorhanden) auf der rechten Seite aufgelistet. Wählen Sie in der Liste die gespeicherte Prozedur `Products_SelectByCategoryID` aus, und klicken Sie auf Weiter.

[![die gespeicherte Prozedur Products_SelectByCategoryID auswählen.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Abbildung 4**: Auswählen der gespeicherten Prozedur `Products_SelectByCategoryID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))

Im nächsten Bildschirm werden wir gefragt, welche Art von Daten von der gespeicherten Prozedur zurückgegeben wird, und unsere Antwort hier bestimmt den Typ, der von der TableAdapter s-Methode zurückgegeben wird. Wenn wir z. b. angeben, dass Tabellendaten zurückgegeben werden, gibt die Methode eine `ProductsDataTable` Instanz zurück, die mit den von der gespeicherten Prozedur zurückgegebenen Datensätzen aufgefüllt wurde. Wenn wir dagegen angeben, dass diese gespeicherte Prozedur einen einzelnen Wert zurückgibt, gibt der TableAdapter einen `Object` zurück, dem der Wert in der ersten Spalte des ersten Datensatzes zugewiesen wird, der von der gespeicherten Prozedur zurückgegeben wird.

Da die gespeicherte Prozedur `Products_SelectByCategoryID` alle Produkte zurückgibt, die zu einer bestimmten Kategorie gehören, wählen Sie die erste Antwort-tabellarische Daten aus, und klicken Sie auf Weiter.

[![anzeigen, dass die gespeicherte Prozedur Tabellendaten zurückgibt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Abbildung 5**: angeben, dass die gespeicherte Prozedur Tabellendaten zurückgibt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))

Sie müssen nur noch angeben, welche Methoden Muster verwendet werden sollen, gefolgt von den Namen für diese Methoden. Lassen Sie die Option Datentabelle ausfüllen, und geben Sie eine Datentabelle zurück, die aktiviert ist, aber benennen Sie die Methoden in `FillByCategoryID` und `GetProductsByCategoryID`um. Klicken Sie dann auf Weiter, um eine Zusammenfassung der Aufgaben zu überprüfen, die der Assistent ausführt. Wenn alles korrekt aussieht, klicken Sie auf Fertigstellen.

[![den Namen der Methoden fillbycategoryid und getproductbycategoryid.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Abbildung 6**: Benennen der Methoden `FillByCategoryID` und `GetProductsByCategoryID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

> [!NOTE]
> Die soeben erstellten TableAdapter-Methoden `FillByCategoryID` und `GetProductsByCategoryID`erwarten einen Eingabeparameter vom Typ `Integer`. Dieser Eingabeparameter Wert wird über seinen `@CategoryID`-Parameter an die gespeicherte Prozedur übergeben. Wenn Sie die Parameter der `Products_SelectByCategory` gespeicherten Prozedur ändern, müssen Sie auch die Parameter für diese TableAdapter-Methoden aktualisieren. Wie bereits im [vorherigen Tutorial](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)erläutert, kann dies auf eine von zwei Arten erfolgen: durch manuelles Hinzufügen oder Entfernen von Parametern aus der Parameter Auflistung oder durch erneutes Ausführen des TableAdapter-Assistenten.

## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Schritt 3: Hinzufügen einer`GetProductsByCategoryID(categoryID)`Methode zum BLL

Wenn die `GetProductsByCategoryID` dal-Methode fertiggestellt ist, besteht der nächste Schritt in der Bereitstellung des Zugriffs auf diese Methode in der Geschäftslogik Schicht. Öffnen Sie die Datei `ProductsBLLWithSprocs`-Klasse, und fügen Sie die folgende Methode hinzu:

[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Diese BLL-Methode gibt einfach die `ProductsDataTable` zurück, die von der `ProductsTableAdapter` s `GetProductsByCategoryID`-Methode zurückgegeben wird. Das `DataObjectMethodAttribute`-Attribut stellt Metadaten bereit, die vom Assistenten zum Konfigurieren von Datenquellen für ObjectDataSource s verwendet werden. Diese Methode wird insbesondere in der Dropdown Liste Registerkarte auswählen angezeigt.

## <a name="step-4-displaying-products-by-category"></a>Schritt 4: Anzeigen von Produkten nach Kategorie

Wenn Sie die neu hinzugefügte gespeicherte Prozedur `Products_SelectByCategoryID` und die entsprechenden dal-und BLL-Methoden testen möchten, erstellen Sie eine ASP.NET-Seite, die eine Dropdown List und eine GridView enthält. In der Dropdown Liste werden alle Kategorien in der Datenbank aufgelistet, während in der GridView die Produkte angezeigt werden, die zur ausgewählten Kategorie gehören.

> [!NOTE]
> Wir haben in vorherigen Tutorials mithilfe von Dropdown Listen Master-/Detail-Schnittstellen erstellt. Ausführliche Informationen zum Implementieren eines solchen Master/Detail-Berichts finden Sie im Tutorial [Master/Detail-Filterung mit einem Dropdown List](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) -Tutorial.

Öffnen Sie die Seite `ExistingSprocs.aspx` im Ordner `AdvancedDAL`, und ziehen Sie eine Dropdown List aus der Toolbox auf den Designer. Legen Sie die Eigenschaft DropDownList s `ID` auf `Categories` und deren `AutoPostBack`-Eigenschaft auf `True`fest. Binden Sie als nächstes die Dropdown Liste aus dem Smarttag an eine neue ObjectDataSource mit dem Namen `CategoriesDataSource`. Konfigurieren Sie die ObjectDataSource so, dass die Daten aus der `CategoriesBLL` Klasse `GetCategories` Methode abgerufen werden. Legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest.

[![Abrufen von Daten aus der Methode "GetCategories" der kategoriesbll-Klasse](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Abbildung 7**: Abrufen von Daten aus der `CategoriesBLL`-Klasse `GetCategories`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))

[![die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) festgelegt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Abbildung 8**: Festlegen der Dropdown Listen auf den Registerkarten "Aktualisieren", "Einfügen" und "Löschen" auf "(keine)" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, konfigurieren Sie die Dropdown Liste, um das `CategoryName` Datenfeld anzuzeigen und das `CategoryID` Feld als `Value` für jede `ListItem`zu verwenden.

An diesem Punkt sollten das deklarative Markup DropDownList und ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Ziehen Sie als nächstes eine GridView-Ansicht auf den Designer, und platzieren Sie Sie unterhalb der Dropdown Liste. Legen Sie die `ID` GridView s auf `ProductsByCategory` fest, und binden Sie das Smarttag an eine neue ObjectDataSource mit dem Namen `ProductsByCategoryDataSource`. Konfigurieren Sie die `ProductsByCategoryDataSource` ObjectDataSource so, dass Sie die `ProductsBLLWithSprocs`-Klasse verwendet, damit Sie Ihre Daten mithilfe der `GetProductsByCategoryID(categoryID)`-Methode abrufen kann. Da diese GridView nur zum Anzeigen von Daten verwendet wird, legen Sie die Dropdown Listen auf den Registerkarten aktualisieren, einfügen und löschen auf (keine) fest, und klicken Sie auf Weiter.

[![konfigurieren Sie ObjectDataSource für die Verwendung der productbllwithsprocs-Klasse.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Abbildung 9**: Konfigurieren von ObjectDataSource für die Verwendung der `ProductsBLLWithSprocs`-Klasse ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))

[![Abrufen von Daten von der getproduczbycategoryid (CategoryID)-Methode](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Abbildung 10**: Abrufen von Daten aus der `GetProductsByCategoryID(categoryID)`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))

Die auf der Registerkarte auswählen ausgewählte Methode erwartet einen Parameter, daher werden Sie im letzten Schritt des Assistenten aufgefordert, den Parameter s Quelle einzugeben. Legen Sie in der Dropdown Liste Parameter Quelle die Option Control fest, und wählen Sie in der Dropdown Liste ControlID das `Categories` Steuerelement aus. Klicken Sie auf Fertig stellen, um den Assistenten abzuschließen.

[![die Dropdown List Kategorien als Quelle für den CategoryID-Parameter verwenden.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Abbildung 11**: Verwenden der Dropdown List-`Categories` als Quelle des `categoryID`-Parameters ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

Nachdem Sie den ObjectDataSource-Assistenten abgeschlossen haben, fügt Visual Studio boundfields und ein CheckBoxField für jedes der Product Data-Felder hinzu. Sie können diese Felder beliebig anpassen.

Besuchen Sie die Seite über einen Browser. Beim Besuch der Seite wird die Kategorie Getränke und die entsprechenden Produkte im Raster angezeigt. Wenn Sie die Dropdown Liste in eine Alternative Kategorie ändern, wie in Abbildung 12 gezeigt, wird ein Postback ausgelöst und das Raster mit den Produkten der neu ausgewählten Kategorie erneut geladen.

[![werden die Produkte in der Kategorie "Produkte" angezeigt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Abbildung 12**: die Produkte in der Kategorie "Produkte" werden angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))

## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Schritt 5: Umlagern von s-Anweisungen einer gespeicherten Prozedur innerhalb des Gültigkeits Bereichs einer Transaktion

Im Tutorial zum [Umpacken von Daten Bank Änderungen in einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) wurden Techniken zum Ausführen einer Reihe von Daten Bank Änderungs Anweisungen im Rahmen einer Transaktion erläutert. Beachten Sie, dass die Änderungen, die mit dem Rahmen einer Transaktion ausgeführt werden, entweder alle erfolgreich verlaufen oder alle fehlschlagen, was Atomizität garantiert. Zu den Verfahren für die Verwendung von Transaktionen gehören:

- Mithilfe der Klassen im `System.Transactions`-Namespace
- Wenn die Datenzugriffs Ebene verwendet wird, verwenden Sie ADO.NET-Klassen wie `SqlTransaction`und
- Direktes Hinzufügen der T-SQL-Transaktions Befehle in der gespeicherten Prozedur

Im Tutorial zum *Umwickeln von Daten Bank Änderungen in einer Transaktion* wurden die ADO.NET-Klassen in der dal verwendet. Im restlichen Teil dieses Tutorials wird untersucht, wie eine Transaktion mithilfe von T-SQL-Befehlen aus einer gespeicherten Prozedur verwaltet wird.

Die drei wichtigsten SQL-Befehle zum manuellen Starten, Ausführen eines Commits und Rollbacks für eine Transaktion sind `BEGIN TRANSACTION`, `COMMIT TRANSACTION`bzw. `ROLLBACK TRANSACTION`. Wie beim ADO.net-Ansatz müssen wir bei der Verwendung von Transaktionen innerhalb einer gespeicherten Prozedur das folgende Muster anwenden:

1. Geben Sie den Start einer Transaktion an.
2. Führen Sie die SQL-Anweisungen aus, die die Transaktion umfassen.
3. Wenn in einer der Anweisungen aus Schritt 2 ein Fehler auftritt, führen Sie ein Rollback für die Transaktion aus.
4. Wenn alle Anweisungen aus Schritt 2 ohne Fehler abgeschlossen werden, führen Sie einen Commit für die Transaktion aus.

Dieses Muster kann in der T-SQL-Syntax mithilfe der folgenden Vorlage implementiert werden:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Die Vorlage beginnt mit der Definition eines `TRY...CATCH` Blocks, einem neuen Konstrukt in SQL Server 2005. Wie bei `Try...Catch` Blöcken in Visual Basic führt der SQL-`TRY...CATCH` Block die Anweisungen im `TRY` Block aus. Wenn eine Anweisung einen Fehler auslöst, wird die Steuerung sofort an den `CATCH`-Block übertragen.

Wenn keine Fehler auftreten, die die SQL-Anweisungen ausführen, mit denen die Transaktion zusammengeführt wird, führt die `COMMIT TRANSACTION` Anweisung einen Commit für die Änderungen aus und schließt die Transaktion Wenn jedoch eine der-Anweisungen zu einem Fehler führt, gibt die `ROLLBACK TRANSACTION` im `CATCH`-Block die Datenbank vor dem Start der Transaktion in ihren Zustand zurück. Die gespeicherte Prozedur löst auch einen Fehler mit dem [Befehl RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)aus, der bewirkt, dass eine `SqlException` in der Anwendung ausgelöst wird.

> [!NOTE]
> Da der `TRY...CATCH`-Block neu in SQL Server 2005 ist, funktioniert die obige Vorlage nicht, wenn Sie ältere Versionen von Microsoft SQL Server verwenden. Wenn Sie nicht SQL Server 2005 verwenden, lesen Sie [Verwalten von Transaktionen in SQL Server gespeicherten Prozeduren](http://www.4guysfromrolla.com/webtech/080305-1.shtml) für eine Vorlage, die mit anderen Versionen von SQL Server funktioniert.

Betrachten wir ein konkretes Beispiel. Eine FOREIGN KEY-Einschränkung besteht zwischen den Tabellen "`Categories`" und "`Products`", was bedeutet, dass jedes `CategoryID` Feld in der `Products` Tabelle einem `CategoryID` Wert in der `Categories` Tabelle zugeordnet werden muss. Jede Aktion, die gegen diese Einschränkung verstoßen würde, z. b. der Versuch, eine Kategorie zu löschen, die über zugeordnete Produkte verfügt, führt zu einer Verletzung der Fremdschlüssel Einschränkung. Um dies zu überprüfen, besuchen Sie das Beispiel aktualisieren und Löschen vorhandener Binärdaten im Abschnitt Arbeiten mit Binärdaten (`~/BinaryData/UpdatingAndDeleting.aspx`). Auf dieser Seite werden die einzelnen Kategorien im System zusammen mit den Schaltflächen Bearbeiten und löschen aufgelistet (siehe Abbildung 13). Wenn Sie jedoch versuchen, eine Kategorie zu löschen, die über zugeordnete Produkte verfügt, z. b. Getränke, schlägt der Löschvorgang aufgrund einer Verletzung der Fremdschlüssel Einschränkung fehl (siehe Abbildung 14).

[![jede Kategorie in einer GridView mit den Schaltflächen "Bearbeiten" und "Löschen" angezeigt wird](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Abbildung 13**: jede Kategorie wird in einer GridView mit den Schaltflächen "Bearbeiten" und "Löschen" angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))

[![Sie keine Kategorie löschen können, die über vorhandene Produkte verfügt.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Abbildung 14**: Sie können keine Kategorie löschen, die über vorhandene Produkte verfügt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))

Stellen Sie sich jedoch vor, dass Kategorien gelöscht werden sollen, unabhängig davon, ob Sie über zugeordnete Produkte verfügen. Wenn eine Kategorie mit Produkten gelöscht werden soll, stellen Sie sich vor, dass wir auch die vorhandenen Produkte löschen möchten (eine andere Möglichkeit wäre, die Produkte einfach `CategoryID` Werte auf `NULL`) festzulegen. Diese Funktion kann durch die kaskadierenden Regeln der FOREIGN KEY-Einschränkung implementiert werden. Alternativ könnten Sie eine gespeicherte Prozedur erstellen, die einen `@CategoryID` Eingabeparameter akzeptiert und beim Aufrufen explizit alle zugeordneten Produkte und dann die angegebene Kategorie löscht.

Der erste Versuch einer solchen gespeicherten Prozedur könnte wie folgt aussehen:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Die zugeordneten Produkte und die Kategorie werden zwar definitiv gelöscht, dies erfolgt jedoch nicht unter dem Rahmen einer Transaktion. Angenommen, es gibt eine andere FOREIGN KEY-Einschränkung für `Categories`, die das Löschen eines bestimmten `@CategoryID` Werts untersagt. Das Problem besteht darin, dass in einem solchen Fall alle Produkte gelöscht werden, bevor Sie versuchen, die Kategorie zu löschen. Das Ergebnis ist, dass diese gespeicherte Prozedur für eine solche Kategorie alle Produkte entfernen würde, während die Kategorie verbleibt, da Sie noch über verwandte Datensätze in einer anderen Tabelle verfügt.

Wenn die gespeicherte Prozedur jedoch in den Gültigkeitsbereich einer Transaktion umschließt, wird für die `Products` Tabelle bei einem fehlerhaften Löschvorgang auf `Categories`ein Rollback ausgeführt. Das folgende Skript für gespeicherte Prozeduren verwendet eine Transaktion, um die Atomizität zwischen den beiden `DELETE`-Anweisungen sicherzustellen:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Nehmen Sie sich einen Moment Zeit, um die gespeicherte Prozedur `Categories_Delete` der Northwind-Datenbank hinzuzufügen. Weitere Informationen zum Hinzufügen gespeicherter Prozeduren zu einer Datenbank finden Sie in Schritt 1.

## <a name="step-6-updating-thecategoriestableadapter"></a>Schritt 6: Aktualisieren der`CategoriesTableAdapter`

Obwohl die gespeicherte Prozedur `Categories_Delete` der-Datenbank hinzugefügt wurde, ist die DAL zurzeit so konfiguriert, dass Sie Ad-hoc-SQL-Anweisungen verwendet, um den Löschvorgang auszuführen. Wir müssen den `CategoriesTableAdapter` aktualisieren und ihn anweisen, stattdessen die gespeicherte Prozedur `Categories_Delete` zu verwenden.

> [!NOTE]
> An früherer Stelle in diesem Tutorial haben wir mit dem `NorthwindWithSprocs` DataSet gearbeitet. Dieses Dataset verfügt jedoch nur über eine einzige Entität, `ProductsDataTable`, und wir müssen mit Kategorien arbeiten. Daher wird für den Rest dieses Tutorials die Datenzugriffs Schicht i m, die auf das `Northwind` DataSet verweist, das erste, das wir im Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) erstellt haben.

Öffnen Sie das Northwind-DataSet, wählen Sie das `CategoriesTableAdapter`aus, und wechseln Sie zum Eigenschaftenfenster. In der Eigenschaftenfenster werden die `InsertCommand`, `UpdateCommand`, `DeleteCommand`und `SelectCommand`, die vom TableAdapter verwendet werden, sowie der Name und die Verbindungsinformationen aufgelistet. Erweitern Sie die `DeleteCommand`-Eigenschaft, um deren Details anzuzeigen. Wie in Abbildung 15 dargestellt, wird die Eigenschaft `DeleteCommand` s `CommandType` auf Text festgelegt, der Sie anweist, den Text in der `CommandText`-Eigenschaft als Ad-hoc-SQL-Abfrage zu senden.

![Wählen Sie im Designer den categoriestableadapter aus, um seine Eigenschaften im Eigenschaften Fenster anzuzeigen.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Abbildung 15**: Auswählen des `CategoriesTableAdapter` im Designer, um seine Eigenschaften im Eigenschaften Fenster anzuzeigen

Um diese Einstellungen zu ändern, wählen Sie den Text (DeleteCommand) im Eigenschaftenfenster aus, und wählen Sie (neu) aus der Dropdown Liste aus. Dadurch werden die Einstellungen für die Eigenschaften "`CommandText`", "`CommandType`" und "`Parameters`" gelöscht. Legen Sie als nächstes die `CommandType`-Eigenschaft auf `StoredProcedure` fest, und geben Sie dann den Namen der gespeicherten Prozedur für die `CommandText` ein (`dbo.Categories_Delete`). Wenn Sie sicherstellen, dass die Eigenschaften in dieser Reihenfolge eingegeben werden, geben Sie zuerst die `CommandType` und dann die `CommandText`-Visual Studio automatisch die Parameter Auflistung ein. Wenn Sie diese Eigenschaften nicht in dieser Reihenfolge eingeben, müssen Sie die Parameter manuell über den Parameter Sammlungs-Editor hinzufügen. In beiden Fällen ist es ratsam, auf die Ellipsen in der Parameters-Eigenschaft zu klicken, um den Parameter Auflistungs-Editor anzuzeigen, um zu überprüfen, ob die korrekten Änderungen an den Parametereinstellungen vorgenommen wurden (siehe Abbildung 16). Wenn im Dialogfeld keine Parameter angezeigt werden, fügen Sie den `@CategoryID`-Parameter manuell hinzu (Sie müssen den `@RETURN_VALUE`-Parameter nicht hinzufügen).

![Stellen Sie sicher, dass die Parametereinstellungen richtig sind.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Abbildung 16**: sicherstellen, dass die Parametereinstellungen richtig sind

Nachdem die DAL aktualisiert wurde, werden durch das Löschen einer Kategorie automatisch alle zugehörigen Produkte gelöscht. Dies geschieht unter dem Rahmen einer Transaktion. Um dies zu überprüfen, kehren Sie zur Seite aktualisieren und Löschen vorhandener Binärdaten zurück, und klicken Sie auf die Schaltfläche Löschen für eine der Kategorien. Mit einem einzigen Mausklick werden die Kategorie und alle zugehörigen Produkte gelöscht.

> [!NOTE]
> Vor dem Testen der gespeicherten Prozedur `Categories_Delete`, mit der eine Reihe von Produkten zusammen mit der ausgewählten Kategorie gelöscht werden, kann es ratsam sein, eine Sicherungskopie der Datenbank zu erstellen. Wenn Sie die `NORTHWND.MDF`-Datenbank in `App_Data`verwenden, schließen Sie einfach Visual Studio, und kopieren Sie die MDF-und LDF-Dateien in `App_Data` in einen anderen Ordner. Nachdem Sie die Funktionalität getestet haben, können Sie die Datenbank wiederherstellen, indem Sie Visual Studio schließen und die aktuellen MDF-und LDF-Dateien in `App_Data` durch die Sicherungskopien ersetzen.

## <a name="summary"></a>Summary

Während der Assistent für TableAdapter s automatisch gespeicherte Prozeduren für uns generiert, gibt es einige Zeiten, in denen diese gespeicherten Prozeduren möglicherweise bereits erstellt wurden oder stattdessen manuell oder mit anderen Tools erstellt werden sollen. Um derartige Szenarios zu ermöglichen, kann der TableAdapter auch so konfiguriert werden, dass er auf eine vorhandene gespeicherte Prozedur verweist. In diesem Tutorial haben wir uns mit dem manuellen Hinzufügen gespeicherter Prozeduren zu einer Datenbank über die Visual Studio-Umgebung und der Übertragung der TableAdapter s-Methoden an diese gespeicherten Prozeduren beschäftigt. Wir haben auch die T-SQL-Befehle und das Skript Muster untersucht, die zum Starten, zum Commit und zum Rollback von Transaktionen innerhalb einer gespeicherten Prozedur verwendet werden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Hilton geisenow, S ren Jacob Lauritsen und Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Weiter](updating-the-tableadapter-to-use-joins-vb.md)
