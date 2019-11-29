---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Einfügen, aktualisieren und Löschen von Daten mit SqlDataSource (C#) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir gelernt, wie das ObjectDataSource-Steuerelement das Einfügen, aktualisieren und Löschen von Daten zugelassen hat. Das SqlDataSource-Steuerelement unterstützt t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 495e0e81a67e6926e1c4fa92e29ebbda747cd418
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610560"
---
# <a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Einfügen, Aktualisieren und Löschen von Daten mit dem SqlDataSource-Steuerelement (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) oder [PDF herunterladen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> In den vorherigen Tutorials haben wir gelernt, wie das ObjectDataSource-Steuerelement das Einfügen, aktualisieren und Löschen von Daten zugelassen hat. Das SqlDataSource-Steuerelement unterstützt die gleichen Vorgänge, aber der Ansatz ist anders, und in diesem Tutorial wird gezeigt, wie Sie SqlDataSource zum Einfügen, aktualisieren und Löschen von Daten konfigurieren.

## <a name="introduction"></a>Einführung

Wie in [einer Übersicht über das Einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)erläutert, bietet das GridView-Steuerelement integrierte Aktualisierungs-und Löschfunktionen, während das DetailsView-Steuerelement und das FormView-Steuerelement das Einfügen von Unterstützung sowie das Bearbeiten und Löschen von Funktionen beinhalten. Diese Daten Änderungs Funktionen können direkt in ein Datenquellen-Steuerelement eingebunden werden, ohne dass eine Codezeile geschrieben werden muss. [Eine Übersicht über das Einfügen, aktualisieren und Löschen von](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) untersuchten mithilfe von ObjectDataSource, um das Einfügen, aktualisieren und Löschen mit den Steuerelementen GridView, DetailsView und FormView zu vereinfachen. Alternativ kann SqlDataSource anstelle von ObjectDataSource verwendet werden.

Um das Einfügen, aktualisieren und Löschen mit ObjectDataSource zu unterstützen, mussten wir die Methoden für die Objektebene angeben, die aufgerufen werden sollen, um die INSERT-, Update-oder DELETE-Aktion auszuführen. Mit SqlDataSource müssen wir `INSERT`, `UPDATE`und `DELETE` SQL-Anweisungen (oder gespeicherte Prozeduren) bereitstellen, die ausgeführt werden sollen. Wie in diesem Tutorial zu sehen ist, können diese Anweisungen manuell erstellt werden oder können automatisch vom Assistenten zum Konfigurieren von Datenquellen von SqlDataSource s generiert werden.

> [!NOTE]
> Da wir bereits die Funktionen zum Einfügen, bearbeiten und Löschen der Steuerelemente GridView, DetailsView und FormView besprochen haben, konzentriert sich dieses Tutorial auf die Konfiguration des SqlDataSource-Steuer Elements, um diese Vorgänge zu unterstützen. Wenn Sie die Implementierung dieser Features in GridView, DetailsView und FormView durchführen müssen, kehren Sie zu den Tutorials zum Bearbeiten, einfügen und Löschen von Daten zurück, beginnend mit [einer Übersicht über das Einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).

## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Schritt 1: Angeben von`INSERT`-,`UPDATE`-und`DELETE`-Anweisungen

Wie in den letzten beiden Tutorials gezeigt, müssen zum Abrufen von Daten aus einem SqlDataSource-Steuerelement zwei Eigenschaften festgelegt werden:

1. `ConnectionString`, das angibt, an welche Datenbank die Abfrage gesendet werden soll, und
2. `SelectCommand`, das die Ad-hoc-SQL-Anweisung oder den Namen der gespeicherten Prozedur angibt, die zum Zurückgeben der Ergebnisse ausgeführt werden soll.

Für `SelectCommand` Werte mit Parametern werden die Parameterwerte über die SqlDataSource s-`SelectParameters` Auflistung angegeben und können hart codierte Werte, allgemeine Parameter Quell Werte (queryString-Felder, Sitzungsvariablen, websteuer Element Werte usw.) enthalten, oder Sie können Programm gesteuert zugewiesen werden. Wenn das SqlDataSource-Steuerelement s `Select()` Methode entweder Programm gesteuert oder automatisch von einem datenweb-Steuerelement aufgerufen wird, wird eine Verbindung mit der Datenbank hergestellt, die Parameterwerte werden der Abfrage zugewiesen, und der Befehl wird aus der Datenbank abgefragt. Die Ergebnisse werden dann entweder als DataSet oder DataReader zurückgegeben, abhängig vom Wert der `DataSourceMode`-Eigenschaft des Steuer Elements.

Zusammen mit der Auswahl von Daten kann das SqlDataSource-Steuerelement verwendet werden, um Daten einzufügen, zu aktualisieren und zu löschen, indem `INSERT`-, `UPDATE`-und `DELETE` SQL-Anweisungen auf die gleiche Weise bereitgestellt werden. Weisen Sie einfach die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` den `INSERT`-, `UPDATE`-und `DELETE` SQL-Anweisungen zu, die ausgeführt werden sollen. Wenn die-Anweisungen über Parameter verfügen (wie Sie am meisten immer), schließen Sie Sie in die `InsertParameters`-, `UpdateParameters`-und `DeleteParameters`-Auflistungen ein.

Nachdem ein `InsertCommand`-, `UpdateCommand`-oder `DeleteCommand` Wert angegeben wurde, wird die Option Einfügen aktivieren, Bearbeiten aktivieren oder löschen aktivieren im entsprechenden datwebcontrol s-Smarttag verfügbar. Um dies zu veranschaulichen, betrachten Sie ein Beispiel von der `Querying.aspx` Seite, die im Tutorial [Abfragen von Daten mit dem SqlDataSource-Steuer](querying-data-with-the-sqldatasource-control-cs.md) Element erstellt wurde, und erweitern Sie es, um Löschfunktionen einzuschließen.

Öffnen Sie zunächst die Seiten `InsertUpdateDelete.aspx` und `Querying.aspx` aus dem Ordner `SqlDataSource`. Wählen Sie im Designer auf der Seite `Querying.aspx` die SqlDataSource-und GridView-Elemente aus dem ersten Beispiel aus (die `ProductsDataSource`-und `GridView1`-Steuerelemente). Nachdem Sie die beiden Steuerelemente ausgewählt haben, wechseln Sie zum Menü Bearbeiten, und wählen Sie kopieren aus (oder drücken Sie einfach STRG + C). Navigieren Sie als nächstes zum Designer von `InsertUpdateDelete.aspx`, und fügen Sie die Steuerelemente ein. Nachdem Sie die beiden Steuerelemente in `InsertUpdateDelete.aspx`verschoben haben, testen Sie die Seite in einem Browser. Es sollten die Werte der Spalten `ProductID`, `ProductName`und `UnitPrice` für alle Datensätze in der `Products` Datenbanktabelle angezeigt werden.

[![alle Produkte aufgeführt werden, geordnet nach ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Abbildung 1**: alle Produkte werden aufgelistet, geordnet nach `ProductID` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))

## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Hinzufügen der Eigenschaften "SqlDataSource s`DeleteCommand`" und "`DeleteParameters`"

An dieser Stelle haben wir eine SqlDataSource, die einfach alle Datensätze aus der `Products` Tabelle und eine GridView zurückgibt, die diese Daten rendert. Unser Ziel ist es, dieses Beispiel zu erweitern, um dem Benutzer zu ermöglichen, Produkte über die GridView zu löschen. Um dies zu erreichen, müssen Sie Werte für das SqlDataSource-Steuerelement s `DeleteCommand` und `DeleteParameters` Eigenschaften angeben und dann die GridView so konfigurieren, dass das Löschen unterstützt wird.

Die Eigenschaften "`DeleteCommand`" und "`DeleteParameters`" können auf verschiedene Arten angegeben werden:

- Durch die deklarative Syntax
- Aus der Eigenschaftenfenster im Designer
- Über den Bildschirm benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben im Assistenten zum Konfigurieren von Datenquellen
- Über die Schaltfläche Erweitert im Bildschirm Spalten aus einer Tabelle der Sicht angeben im Assistenten zum Konfigurieren von Datenquellen, der automatisch die `DELETE` SQL-Anweisung und die Parameter Auflistung generiert, die in den Eigenschaften `DeleteCommand` und `DeleteParameters` verwendet werden.

Wir untersuchen, wie die `DELETE`-Anweisung in Schritt 2 automatisch erstellt wird. Verwenden Sie vorerst die Eigenschaftenfenster im Designer, obwohl der Assistent zum Konfigurieren von Datenquellen oder die deklarative Syntax ebenfalls funktionieren würde.

Klicken Sie im Designer in `InsertUpdateDelete.aspx`auf die `ProductsDataSource` SqlDataSource, und rufen Sie dann die Eigenschaftenfenster auf (Klicken Sie im Menü Ansicht auf Eigenschaftenfenster, oder drücken Sie einfach F4). Wählen Sie die DeleteQuery-Eigenschaft aus, die eine Reihe von Ellipsen aufführt.

![Wählen Sie die DeleteQuery-Eigenschaft im Eigenschaften Fenster aus.](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Abbildung 2**: Auswählen der DeleteQuery-Eigenschaft im Eigenschaften Fenster

> [!NOTE]
> SqlDataSource hat keine DeleteQuery-Eigenschaft. DeleteQuery ist vielmehr eine Kombination aus den Eigenschaften `DeleteCommand` und `DeleteParameters` und wird nur im Eigenschaftenfenster aufgelistet, wenn das Fenster über den Designer angezeigt wird. Wenn Sie sich die Eigenschaftenfenster in der Quell Ansicht ansehen, finden Sie stattdessen die Eigenschaft `DeleteCommand`.

Klicken Sie in der DeleteQuery-Eigenschaft auf die Auslassungs Punkte, um das Dialogfeld Befehls-und Parameter-Editor anzuzeigen (siehe Abbildung 3). In diesem Dialogfeld können Sie die `DELETE` SQL-Anweisung angeben und die Parameter angeben. Geben Sie die folgende Abfrage in das Textfeld `DELETE`-Befehl ein (entweder manuell oder mithilfe des Abfrage-Generator, wenn Sie es bevorzugen):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Klicken Sie anschließend auf die Schaltfläche Parameter aktualisieren, um der Liste der unten aufgeführten Parameter den `@ProductID`-Parameter hinzuzufügen.

[![die DeleteQuery-Eigenschaft im Eigenschaften Fenster auswählen.](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Abbildung 3**: Auswählen der DeleteQuery-Eigenschaft im Eigenschaften Fenster ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))

Geben Sie *keinen* Wert für diesen Parameter an (belassen Sie die Parameter Quelle bei None). Nachdem wir die Unterstützung für das Löschen der GridView hinzugefügt haben, wird dieser Parameterwert von GridView automatisch bereitgestellt. dabei wird der Wert der `DataKeys` Auflistung für die Zeile verwendet, auf deren Lösch Schaltfläche geklickt wurde.

> [!NOTE]
> Der Parameter Name, der in der `DELETE` Abfrage verwendet wird, *muss* mit dem Namen des `DataKeyNames` Werts in GridView, DetailsView oder FormView identisch sein. Das heißt, der-Parameter in der `DELETE`-Anweisung wird gezielt `@ProductID` benannt (anstelle von `@ID`), da der Name der Primärschlüssel Spalte in der Products-Tabelle (und somit der DataKeyNames-Wert in der GridView) `ProductID`ist.

Wenn der Parameter Name und der `DataKeyNames` Wert nicht mit t Stimmen, kann die GridView den Parameter nicht automatisch dem Wert aus der `DataKeys` Auflistung zuweisen.

Nachdem Sie die Lösch bezogenen Informationen in das Dialogfeld Befehls-und Parameter-Editor eingegeben haben, klicken Sie auf OK, und wechseln Sie zur Quell Ansicht, um das resultierende deklarative Markup zu untersuchen:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Beachten Sie das Hinzufügen der `DeleteCommand`-Eigenschaft sowie des `<DeleteParameters>` Abschnitts und des Parameter Objekts mit dem Namen `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Konfigurieren der GridView zum Löschen

Wenn die `DeleteCommand`-Eigenschaft hinzugefügt wurde, enthält das GridView s-Smarttag jetzt die Option zum Aktivieren der Löschung. Aktivieren Sie dieses Kontrollkästchen. Wie in [einer Übersicht über das Einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)erläutert, fügt die GridView ein CommandField-Objekt hinzu, dessen `ShowDeleteButton`-Eigenschaft auf `true`festgelegt ist. Wie in Abbildung 4 gezeigt, wird die Schaltfläche "Löschen" angezeigt, wenn die Seite in einem Browser geöffnet wird. Testen Sie diese Seite, indem Sie einige Produkte löschen.

[![jede GridView-Zeile nun eine Schaltfläche zum Löschen enthält.](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Abbildung 4**: jede GridView-Zeile enthält jetzt eine Lösch Schaltfläche ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))

Wenn Sie auf eine Lösch Schaltfläche klicken, wird ein Postback durchgeführt. die GridView weist den `ProductID`-Parameter den Wert des `DataKeys` Sammlungs Werts für die Zeile zu, auf deren Lösch Schaltfläche geklickt wurde, und ruft die `Delete()`-Methode SqlDataSource auf. Das SqlDataSource-Steuerelement stellt dann eine Verbindung mit der Datenbank her und führt die `DELETE`-Anweisung aus. Die GridView bindet dann an die SqlDataSource zurück und zeigt die aktuelle Produktgruppe an (die nicht mehr den soeben gelöschten Datensatz enthält).

> [!NOTE]
> Da in der GridView-Auflistung die SqlDataSource-Parameter mit der `DataKeys`-Auflistung aufgefüllt werden, ist es wichtig, dass die GridView s `DataKeyNames`-Eigenschaft auf die Spalte (n) festgelegt wird, die den Primärschlüssel bilden, und dass die SqlDataSource s-`SelectCommand` diese Spalten zurückgibt. Darüber hinaus ist es wichtig, dass der Parameter Name in der SqlDataSource s-`DeleteCommand` auf `@ProductID`festgelegt ist. Wenn die `DataKeyNames`-Eigenschaft nicht festgelegt ist oder der-Parameter nicht `@ProductsID`benannt ist, wird durch Klicken auf die Schaltfläche "Löschen" ein Postback ausgelöst, aber es werden keine Datensätze gelöscht.

In Abbildung 5 wird diese Interaktion grafisch dargestellt. Eine ausführlichere Erläuterung der Ereigniskette, die dem Einfügen, aktualisieren und Löschen von einem datenweb-Steuerelement zugeordnet ist, finden Sie unter unter [Suchen der Ereignisse im Zusammenhang mit dem Tutorial zum Einfügen, aktualisieren und löschen](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) .

![Durch Klicken auf die Schaltfläche "Löschen" in der GridView wird die Delete ()-Methode von SqlDataSource aufgerufen.](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Abbildung 5**: Klicken auf die Schaltfläche "Löschen" in der GridView-Methode ruft die SqlDataSource s-`Delete()` Methode auf

## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Schritt 2: Automatisches Erstellen der`INSERT`-,`UPDATE`-und`DELETE`-Anweisung

Wie in Schritt 1 untersucht, können `INSERT`-, `UPDATE`-und `DELETE`-SQL-Anweisungen über die deklarative Syntax von Eigenschaftenfenster oder Steuerelement s angegeben werden. Dieser Ansatz erfordert jedoch, dass Sie die SQL-Anweisungen manuell manuell schreiben. Dies kann monoton und fehleranfällig sein. Glücklicherweise bietet der Assistent zum Konfigurieren von Datenquellen eine Option, mit der die Anweisungen `INSERT`, `UPDATE`und `DELETE` automatisch generiert werden, wenn Sie den Bildschirm Spalten aus einer Tabellen Sicht angeben verwenden.

Sehen Sie sich diese automatische Generierungs Option an. Fügen Sie dem Designer in `InsertUpdateDelete.aspx` eine DetailsView hinzu, und legen Sie dessen `ID`-Eigenschaft auf `ManageProducts`fest. Wählen Sie als nächstes im Smarttags DetailsView s die Option zum Erstellen einer neuen Datenquelle aus, und erstellen Sie eine SqlDataSource mit dem Namen `ManageProductsDataSource`.

[![erstellen Sie eine neue SqlDataSource mit dem Namen "manageproductdatasource".](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Abbildung 6**: Erstellen einer neuen SqlDataSource mit dem Namen "`ManageProductsDataSource`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))

Wählen Sie im Assistenten zum Konfigurieren von Datenquellen die `NORTHWINDConnectionString` Verbindungs Zeichenfolge aus, und klicken Sie auf Weiter. Lassen Sie auf dem Bildschirm "SELECT-Anweisung konfigurieren" das Optionsfeld Spalten aus einer Tabelle oder Sicht angeben aktiviert, und wählen Sie die `Products` Tabelle aus der Dropdown Liste aus. Wählen Sie in der Liste der Kontrollkästchen die Spalten `ProductID`, `ProductName`, `UnitPrice`und `Discontinued` aus.

[Wenn Sie die Products-Tabelle verwenden, werden die Spalten ProductID, ProductName, UnitPrice und nicht mehr unterstützt. ![](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Abbildung 7**: Zurückgeben der Spalten `ProductID`, `ProductName`, `UnitPrice`und `Discontinued` mithilfe der `Products` Tabelle ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))

Zum automatischen Generieren von `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen auf der Grundlage der ausgewählten Tabelle und Spalten klicken Sie auf die Schaltfläche Erweitert, und aktivieren Sie das Kontrollkästchen `INSERT`generieren, `UPDATE`und `DELETE` Anweisungen.

![Aktivieren Sie das Kontrollkästchen INSERT-, Update-und DELETE-Anweisungen generieren.](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Abbildung 8**: Aktivieren des Kontrollkästchens `INSERT`generieren, `UPDATE`und `DELETE` Anweisungen

Das Kontrollkästchen `INSERT`generieren, `UPDATE`und `DELETE` Anweisungen kann nur überprüft werden, wenn die ausgewählte Tabelle über einen Primärschlüssel verfügt und die Primärschlüssel Spalte (oder Spalten) in der Liste der zurückgegebenen Spalten enthalten ist. Das Kontrollkästchen "vollständige Parallelität verwenden", das nach dem Aktivieren des Kontrollkästchens "`INSERT`generieren", "`UPDATE`" und "`DELETE`" ausgewählt wird, erweitert die `WHERE` Klauseln in den resultierenden `UPDATE`-und `DELETE`-Anweisungen, um die Steuerung der vollständigen Parallelität Lassen Sie dieses Kontrollkästchen vorerst deaktiviert. im nächsten Tutorial untersuchen wir die optimistische Parallelität mit dem SqlDataSource-Steuerelement.

Nachdem Sie das Kontrollkästchen `INSERT`generieren, `UPDATE`und `DELETE` Anweisungen aktiviert haben, klicken Sie auf OK, um zum Bildschirm Select-Anweisung konfigurieren zurückzukehren. Klicken Sie dann auf weiter und dann auf Fertigstellen, um den Assistenten zum Konfigurieren von Datenquellen abzuschließen. Nachdem Sie den Assistenten abgeschlossen haben, fügt Visual Studio boundfields der DetailsView für die Spalten `ProductID`, `ProductName`und `UnitPrice` und ein CheckBoxField für die Spalte `Discontinued` hinzu. Aktivieren Sie aus dem Smarttag DetailsView s die Option Paging aktivieren, damit der Benutzer, der diese Seite besucht, die Produkte durchlaufen kann. Löschen Sie auch die Eigenschaften `Width` und `Height` DetailsView.

Beachten Sie, dass das Smarttags die Optionen Aktivieren von einfügen, Aktivieren der Bearbeitung und Aktivieren von Lösch Optionen enthält. Dies liegt daran, dass SqlDataSource Werte für die `InsertCommand`, `UpdateCommand`und `DeleteCommand`enthält, wie die folgende deklarative Syntax zeigt:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Beachten Sie, dass für das SqlDataSource-Steuerelement Werte automatisch für die Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` festgelegt wurden. Die Spalten Gruppe, auf die in den Eigenschaften `InsertCommand` und `UpdateCommand` verwiesen wird, basieren auf den Spalten in der `SELECT`-Anweisung. Das heißt, anstatt *jede* Products-Spalte in der `InsertCommand` und `UpdateCommand`zu haben, gibt es nur die Spalten, die im `SelectCommand` angegeben sind (weniger `ProductID`, das weggelassen wird, da es sich um eine [`IDENTITY` Spalte](http://www.sqlteam.com/item.asp?ItemID=102)handelt, deren Wert bei der Bearbeitung nicht geändert werden kann und die beim Einfügen automatisch zugewiesen wird). Außerdem gibt es für jeden Parameter in den Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` entsprechende Parameter in den `InsertParameters`-, `UpdateParameters`-und `DeleteParameters`-Auflistungen.

Um die Daten Änderungs Features von DetailsView zu aktivieren, aktivieren Sie die Option Einfügen aktivieren, Bearbeiten aktivieren und löschen in der Smarttags aktivieren. Dadurch wird ein CommandField mit den Eigenschaften `ShowInsertButton`, `ShowEditButton`und `ShowDeleteButton` auf `true`festgelegt.

Besuchen Sie die Seite in einem Browser, und notieren Sie sich die Schaltflächen Bearbeiten, löschen und neu in der DetailsView. Wenn Sie auf die Schaltfläche "Bearbeiten" klicken, wird die DetailsView in den Bearbeitungsmodus umgewandelt, in dem jedes BoundField angezeigt wird, dessen `ReadOnly`-Eigenschaft auf `false` (Standard) als Textfeld festgelegt ist, und das CheckBoxField als Kontrollkästchen.

[![der Standard Bearbeitungs Schnittstelle der DetailsView s](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Abbildung 9**: die Standard Bearbeitungs Schnittstelle der DetailsView s ([Klicken Sie, um das Bild in voller Größe anzuzeigen](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))

Auf ähnliche Weise können Sie das derzeit ausgewählte Produkt löschen oder dem System ein neues Produkt hinzufügen. Da die `InsertCommand`-Anweisung nur mit den Spalten `ProductName`, `UnitPrice`und `Discontinued` verwendet werden kann, haben die anderen Spalten entweder `NULL` oder deren Standardwert beim Einfügen von der Datenbank zugewiesen. Ebenso wie bei ObjectDataSource tritt beim Versuch, die `INSERT` Anweisung auszuführen, ein SQL-Fehler auf, wenn in der `InsertCommand` keine Datenbanktabellen Spalten vorhanden sind, die `NULL` s nicht zulassen, und kein Standardwert vorhanden ist.

> [!NOTE]
> In den DetailsView s-und-Bearbeitungs Schnittstellen fehlt eine Anpassung oder Validierung. Zum Hinzufügen von Validierungs Steuerelementen oder zum Anpassen der Schnittstellen müssen Sie boundfields in templatefields konvertieren. Weitere Informationen finden Sie in den Tutorials zum [Hinzufügen von Validierungs Steuerelementen zu den Bearbeitungs-und einfügeschnittstellen](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) und zum [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

Beachten Sie außerdem, dass für das Aktualisieren und Löschen der DetailsView den aktuellen Product s `DataKey` Wert verwendet, der nur vorhanden ist, wenn die `DataKeyNames`-Eigenschaft konfiguriert ist. Wenn die Bearbeitung oder Löschung scheinbar keine Auswirkung hat, stellen Sie sicher, dass die `DataKeyNames`-Eigenschaft festgelegt ist.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Einschränkungen beim automatischen Erstellen von SQL-Anweisungen

Da die Option `INSERT`generieren, `UPDATE`und `DELETE` Anweisungen nur verfügbar ist, wenn Sie Spalten aus einer Tabelle auswählen, müssen Sie für komplexere Abfragen eigene `INSERT`-, `UPDATE`-und `DELETE` Anweisungen schreiben, wie in Schritt 1. SQL `SELECT`-Anweisungen verwenden `JOIN` s häufig, um Daten aus einer oder mehreren Nachschlage Tabellen zu Anzeige Zwecken zurückzuschalten (z. b. beim Anzeigen der `Categories` Tabelle s `CategoryName` Feld beim Anzeigen von Produktinformationen). Gleichzeitig empfiehlt es sich, den Benutzern das Bearbeiten, aktualisieren oder Einfügen von Daten in die Kern Tabelle (in diesem Fall`Products`) zu ermöglichen.

Während die Anweisungen `INSERT`, `UPDATE`und `DELETE` manuell eingegeben werden können, sollten Sie sich den folgenden Zeit speichenden Tipp ansehen. Richten Sie zunächst SqlDataSource so ein, dass Daten direkt aus der `Products` Tabelle abgerufen werden. Verwenden Sie den Assistenten zum Konfigurieren von Datenquellen zum Angeben von Spalten aus einer Tabelle oder Sicht, sodass Sie automatisch die Anweisungen `INSERT`, `UPDATE`und `DELETE` generieren können. Nachdem Sie den Assistenten abgeschlossen haben, wählen Sie die Option zum Konfigurieren von SelectQuery aus dem Eigenschaftenfenster aus (oder wechseln Sie alternativ zum Assistenten zum Konfigurieren von Datenquellen, verwenden Sie jedoch die Option benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben). Aktualisieren Sie dann die `SELECT`-Anweisung, sodass Sie die `JOIN`-Syntax enthält. Diese Technik bietet zeitsparende Vorteile der automatisch generierten SQL-Anweisungen und ermöglicht eine stärker angepasste `SELECT`-Anweisung.

Eine weitere Einschränkung der automatischen Erstellung der `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen besteht darin, dass die Spalten in den `INSERT`-und `UPDATE` Anweisungen auf den Spalten basieren, die von der `SELECT`-Anweisung zurückgegeben werden. Wir müssen jedoch möglicherweise mehr oder weniger Felder aktualisieren oder einfügen. Beispielsweise möchten Sie im Beispiel aus Schritt 2 vielleicht, dass die `UnitPrice` BoundField schreibgeschützt ist. In diesem Fall sollte Sie nicht in der `UpdateCommand`angezeigt werden. Möglicherweise möchten wir auch den Wert eines Tabellen Felds festlegen, das nicht in der GridView angezeigt wird. Wenn Sie beispielsweise einen neuen Datensatz hinzufügen, möchten wir möglicherweise, dass der `QuantityPerUnit` Wert auf TODO festgelegt ist.

Wenn solche Anpassungen erforderlich sind, müssen Sie Sie entweder über die Eigenschaftenfenster, die Option benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben im Assistenten oder über die deklarative Syntax manuell vornehmen.

> [!NOTE]
> Beachten Sie beim Hinzufügen von Parametern, die keine entsprechenden Felder im datenweb-Steuerelement aufweisen, dass diese Parameterwerte auf irgendeine Weise den Werten zugewiesen werden müssen. Diese Werte können direkt in der `InsertCommand` oder `UpdateCommand`fest codiert werden. kann aus einer vordefinierten Quelle (der QueryString, dem Sitzungszustand, den websteuer Elementen auf der Seite usw.) stammen. oder kann Programm gesteuert zugewiesen werden, wie im vorherigen Tutorial gezeigt.

## <a name="summary"></a>Summary

Damit die datenweb Steuerelemente ihre integrierten Funktionen zum Einfügen, bearbeiten und löschen nutzen können, muss das Datenquellen-Steuerelement, an das Sie gebunden sind, eine solche Funktionalität bieten. Für SqlDataSource bedeutet dies, dass `INSERT`-, `UPDATE`-und `DELETE`-SQL-Anweisungen den Eigenschaften `InsertCommand`, `UpdateCommand`und `DeleteCommand` zugewiesen werden müssen. Diese Eigenschaften und die entsprechenden Parameter Sammlungen können manuell hinzugefügt oder automatisch mithilfe des Assistenten zum Konfigurieren von Datenquellen generiert werden. In diesem Tutorial haben wir beide Verfahren untersucht.

Wir haben die Verwendung der optimistischen Parallelität mit ObjectDataSource im Tutorial zum [Implementieren von optimistischer](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Parallelität untersucht. Das SqlDataSource-Steuerelement bietet auch Unterstützung für vollständige Parallelität. Wie in Schritt 2 erwähnt, bietet der Assistent beim automatischen Erstellen der `INSERT`-, `UPDATE`-und `DELETE`-Anweisung eine Option zur Verwendung optimistischer Parallelität. Wie wir im nächsten Tutorial sehen werden, ändert die Verwendung von optimistischer Parallelität mit der SqlDataSource die `WHERE`-Klauseln in den `UPDATE`-und `DELETE`-Anweisungen, um sicherzustellen, dass die Werte für die anderen Spalten seit dem letzten Anzeigen der Daten auf der Seite nicht geändert wurden.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Weiter](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
