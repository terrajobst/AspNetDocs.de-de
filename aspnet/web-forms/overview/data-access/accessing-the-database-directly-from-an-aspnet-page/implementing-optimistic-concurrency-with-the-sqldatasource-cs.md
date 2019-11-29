---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implementieren von optimistischer Parallelität mit SqlDataSourceC#() | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial überprüfen wir die Grundlagen der Steuerung der vollständigen Parallelität und untersuchen dann, wie Sie mit dem SqlDataSource-Steuerelement implementiert werden.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 87fca52e2e8be844411b2fff8382c6002eccbe09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598184"
---
# <a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementieren von optimistischer Parallelität mit dem SqlDataSource-Steuerelement (C#)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) oder [PDF herunterladen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> In diesem Tutorial überprüfen wir die Grundlagen der Steuerung der vollständigen Parallelität und untersuchen dann, wie Sie mit dem SqlDataSource-Steuerelement implementiert werden.

## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde erläutert, wie Sie dem SqlDataSource-Steuerelement Einfüge-, Aktualisierungs-und Löschfunktionen hinzufügen. Kurz gesagt, um diese Features bereitzustellen, mussten wir die entsprechende `INSERT`-, `UPDATE`-oder `DELETE` SQL-Anweisung in den Eigenschaften der `InsertCommand`-, `UpdateCommand`-oder `DeleteCommand`-Eigenschaft des Steuer Elements angeben, zusammen mit den entsprechenden Parametern in den `InsertParameters`-, `UpdateParameters`-und `DeleteParameters`-Auflistungen. Diese Eigenschaften und Auflistungen können manuell angegeben werden. die Schaltfläche "Erweiterte Datenquellen-Assistent" enthält jedoch das Kontrollkästchen "`INSERT`generieren", "`UPDATE`" und "`DELETE`", mit dem diese Anweisungen automatisch auf der Grundlage der `SELECT` Anweisung erstellt werden.

Zusammen mit dem Kontrollkästchen `INSERT`generieren, `UPDATE`und `DELETE`-Anweisung enthält das Dialogfeld Erweiterte SQL-Generierungs Optionen eine Option zum Verwenden von optimistischer Parallelität (siehe Abbildung 1). Wenn dieses Kontrollkästchen aktiviert ist, werden die `WHERE`-Klauseln in den automatisch generierten `UPDATE` und `DELETE`-Anweisungen so geändert, dass Sie nur das Update oder den Löschvorgang ausführen, wenn die zugrunde liegenden Datenbankdaten seit dem letzten Laden der Daten im Raster nicht geändert wurden.

![Im Dialog Feld Erweiterte SQL-Generierungs Optionen können Sie die Unterstützung für optimistische Parallelität hinzufügen.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Abbildung 1**: Sie können im Dialog Feld "Erweiterte SQL-Generierungs Optionen" Unterstützung für vollständige Parallelität hinzufügen

Im Tutorial zum [Implementieren von optimistischer](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Parallelität haben wir die Grundlagen der Steuerung der vollständigen Parallelität und die Vorgehensweise zum Hinzufügen zu ObjectDataSource untersucht. In diesem Tutorial werden die Grundlagen der Steuerung der vollständigen Parallelität behandelt und dann erläutert, wie Sie mithilfe von SqlDataSource implementiert wird.

## <a name="a-recap-of-optimistic-concurrency"></a>Eine Zusammenfassung der optimistischen Parallelität

Für Webanwendungen, die mehreren, gleichzeitigen Benutzern erlauben, dieselben Daten zu bearbeiten oder zu löschen, besteht die Möglichkeit, dass ein Benutzer möglicherweise versehentlich andere Änderungen überschreibt. Im Tutorial zum [Implementieren von optimistischer](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Parallelität habe ich Folgendes Beispiel angegeben:

Stellen Sie sich vor, dass zwei Benutzer, jisun und Sam, eine Seite in einer Anwendung besuchen, die es Besuchern gestattet hat, Produkte über ein GridView-Steuerelement zu aktualisieren und zu löschen. Beide klicken auf die Bearbeitungs Schaltfläche für Chai ungefähr zur gleichen Zeit. Jisun ändert den Produktnamen in Chai Tea und klickt auf die Schaltfläche Aktualisieren. Das Ergebnis ist eine `UPDATE`-Anweisung, die an die Datenbank gesendet wird, die *alle* aktualisierbaren Felder für das Produkt festlegt (obwohl jisun nur ein Feld aktualisiert hat, `ProductName`). Zu diesem Zeitpunkt hat die Datenbank die Werte "Chai Tea", "Kategorie Getränke", "Lieferant Exotic Liquids" usw. für dieses Produkt. Allerdings zeigt der Bildschirm GridView auf Sam s den Produktnamen in der bearbeitbaren GridView-Zeile weiterhin als Chai an. Einige Sekunden nach dem Commit der jisun s-Änderungen aktualisiert Sam die Kategorie in Bedingungen und klickt auf aktualisieren. Dies führt dazu, dass eine `UPDATE`-Anweisung an die Datenbank gesendet wird, die den Produktnamen auf Chai festlegt, die `CategoryID` auf die zugehörige Kategorie-ID der Bedingungen usw. Änderungen an der jisun s-Änderung am Produktnamen wurden überschrieben.

In Abbildung 2 wird diese Interaktion veranschaulicht.

[![wenn zwei Benutzer gleichzeitig einen Datensatz aktualisieren, besteht die Möglichkeit, dass ein Benutzer Änderungen an den anderen en überschreibt.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Abbildung 2**: Wenn zwei Benutzer gleichzeitig einen Datensatz aktualisieren, gibt es möglicherweise eine Änderung an einem Benutzer, um die anderen en zu überschreiben ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png)).

Um dieses Szenario zu verhindern, muss eine Form der Parallelitäts [Steuerung](http://en.wikipedia.org/wiki/Concurrency_control) implementiert werden. Vollständige Parallelität der Schwerpunkt dieses Lernprogramms liegt auf der Annahme, dass es [in der zwischen](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) Zeit zu Parallelitäts Konflikten kommt und der Großteil der Zeit solche Konflikte nicht verursacht. Wenn z. b. ein Konflikt auftritt, wird dem Benutzer durch die Steuerung der vollständigen Parallelität lediglich mitgeteilt, dass die Änderungen gespeichert werden können, weil ein anderer Benutzer die gleichen Daten geändert hat.

> [!NOTE]
> Bei Anwendungen, in denen davon ausgegangen wird, dass es zu vielen Parallelitäts Konflikten kommt oder solche Konflikte nicht toleriert werden, kann stattdessen die pessimistische Parallelitäts Steuerung verwendet werden. Eine ausführlichere Erläuterung zur Steuerung der pessimistischen Parallelität finden Sie im Tutorial zum [Implementieren von optimistischer](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Parallelität.

Durch die Steuerung der vollständigen Parallelität wird sichergestellt, dass der zu Aktualisier Ende oder zu löschende Datensatz dieselben Werte hat wie beim Start des Aktualisierungs-oder Löschvorgangs. Wenn Sie z. b. in einem bearbeitbaren GridView-Steuerelement auf die Schaltfläche Bearbeiten klicken, werden die Werte des Datensatz s aus der Datenbank gelesen und in Textfeldern und anderen websteuer Elementen angezeigt. Diese ursprünglichen Werte werden von der GridView gespeichert. Nachdem der Benutzer die Änderungen vorgenommen und auf die Schaltfläche Aktualisieren geklickt hat, muss in der `UPDATE`-Anweisung die ursprünglichen Werte zuzüglich der neuen Werte berücksichtigt werden, und der zugrunde liegende Datenbankdaten Satz muss nur aktualisiert werden, wenn die ursprünglichen Werte, die der Benutzer mit der Bearbeitung begonnen hat, mit den Werten in der Datenbank identisch sind. In Abbildung 3 wird diese Abfolge von Ereignissen dargestellt.

[![, dass das Update oder der Löschvorgang erfolgreich ist, müssen die ursprünglichen Werte den aktuellen Daten bankwerten entsprechen.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Abbildung 3**: damit das Update oder der Löschvorgang erfolgreich ausgeführt werden kann, müssen die ursprünglichen Werte den aktuellen Daten bankwerten entsprechen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png)).

Es gibt verschiedene Ansätze für die Implementierung von optimistischer Parallelität (siehe die [Aktualisierungs Logik für optimistische](http://www.eggheadcafe.com/articles/20050719.asp) Parallelität von [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp)für einen kurzen Blick auf eine Reihe von Optionen). Die von SqlDataSource verwendete Technik (sowie die in unserer Datenzugriffs Schicht verwendeten ADO.net typisierten Datasets) erweitert die `WHERE`-Klausel so, dass Sie einen Vergleich aller ursprünglichen Werte einschließt. Mit der folgenden `UPDATE` Anweisung werden z. b. der Name und der Preis eines Produkts nur aktualisiert, wenn die aktuellen Daten Bank Werte den Werten entsprechen, die ursprünglich beim Aktualisieren des Datensatzes in der GridView abgerufen wurden. Die Parameter `@ProductName` und `@UnitPrice` enthalten die neuen Werte, die vom Benutzer eingegeben wurden, während `@original_ProductName` und `@original_UnitPrice` die Werte enthalten, die beim Klicken auf die Schaltfläche Bearbeiten ursprünglich in die GridView geladen wurden:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Wie wir in diesem Tutorial sehen werden, ist das Aktivieren der Steuerung der optimistischen Parallelität mit SqlDataSource so einfach wie das Aktivieren eines Kontrollkästchens.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Schritt 1: Erstellen einer SqlDataSource, die optimistische Parallelität unterstützt

Öffnen Sie zunächst die Seite `OptimisticConcurrency.aspx` aus dem Ordner `SqlDataSource`. Ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox auf den Designer, und legen Sie dessen `ID`-Eigenschaft auf `ProductsDataSourceWithOptimisticConcurrency`fest. Klicken Sie anschließend auf den Link Datenquelle konfigurieren des Smarttags für die Steuerelemente. Wählen Sie auf dem ersten Bildschirm des Assistenten die Option zum Arbeiten mit dem `NORTHWINDConnectionString` aus, und klicken Sie auf Weiter.

[![Sie die Verwendung von NorthwindConnectionString.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Abbildung 4**: Wählen Sie die Arbeit mit dem `NORTHWINDConnectionString` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))

In diesem Beispiel fügen wir eine GridView hinzu, mit der Benutzer die `Products` Tabelle bearbeiten können. Wählen Sie daher im Bildschirm "SELECT-Anweisung konfigurieren" in der Dropdown Liste die `Products` Tabelle aus, und wählen Sie die Spalten "`ProductID`", "`ProductName`", "`UnitPrice`" und "`Discontinued`" aus, wie in Abbildung 5 dargestellt.

[Geben Sie ![aus der Tabelle Products die Spalten ProductID, ProductName, UnitPrice und nicht mehr unterstützt zurück.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Abbildung 5**`Products`: Zurückgeben der Spalten "`ProductID`", "`ProductName`", "`UnitPrice`" und "`Discontinued`" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))

Nachdem Sie die Spalten auswählen, klicken Sie auf die Schaltfläche Erweitert, um das Dialogfeld Erweiterte SQL-Generierungs Optionen zu aktivieren. Überprüfen Sie die Anweisungen `INSERT`generieren, `UPDATE`und `DELETE`, und verwenden Sie die Kontrollkästchen für die vollständige Parallelität, und klicken Sie auf OK (Weitere Informationen finden Sie in Abbildung 1). Beenden Sie den Assistenten, indem Sie auf weiter und dann auf Fertigstellen klicken

Nachdem Sie den Assistenten zum Konfigurieren von Datenquellen abgeschlossen haben, nehmen Sie sich einen Moment Zeit, um die resultierenden `DeleteCommand` und `UpdateCommand` Eigenschaften sowie die `DeleteParameters`-und `UpdateParameters` Auflistungen Die einfachste Möglichkeit hierzu besteht darin, in der unteren linken Ecke auf die Registerkarte "Quelle" zu klicken, um die deklarative Syntax der Seite anzuzeigen. Dort finden Sie einen `UpdateCommand` Wert von:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Mit sieben Parametern in der `UpdateParameters`-Sammlung:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Ebenso sollten die `DeleteCommand`-Eigenschaft und die `DeleteParameters` Auflistung wie folgt aussehen:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Zusätzlich zur Erweiterung der `WHERE` Klauseln der Eigenschaften `UpdateCommand` und `DeleteCommand` (und zum Hinzufügen der zusätzlichen Parameter zu den jeweiligen Parameter Auflistungen), werden bei Auswahl der Option optimistische Parallelität verwenden zwei weitere Eigenschaften angepasst:

- Ändert die [`ConflictDetection`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) von `OverwriteChanges` (Standard) in `CompareAllValues`
- Ändert die [`OldValuesParameterFormatString`-Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) von {0} (Standardeinstellung) in den ursprünglichen\_{0}.

Wenn das datenweb-Steuerelement die SqlDataSource s-`Update()` oder `Delete()`-Methode aufruft, übergibt es die ursprünglichen Werte. Wenn die Eigenschaft SqlDataSource s `ConflictDetection` auf `CompareAllValues`festgelegt ist, werden diese ursprünglichen Werte dem Befehl hinzugefügt. Die `OldValuesParameterFormatString`-Eigenschaft stellt das Benennungs Muster bereit, das für diese ursprünglichen Wert Parameter verwendet wird. Der Assistent zum Konfigurieren von Datenquellen verwendet die ursprüngliche\_{0} und benennt jeden ursprünglichen Parameter in den Eigenschaften `UpdateCommand` und `DeleteCommand` sowie `UpdateParameters` und `DeleteParameters` Sammlungen entsprechend.

> [!NOTE]
> Da die Einfügefunktionen des SqlDataSource-Steuer Elements nicht verwendet werden, können Sie die `InsertCommand`-Eigenschaft und deren `InsertParameters` Auflistung entfernen.

## <a name="correctly-handlingnullvalues"></a>Ordnungsgemäße Behandlung von`NULL`Werten

Leider funktionieren die vom Assistenten zum Konfigurieren von Datenquellen beim Verwenden von optimistischer Parallelität automatisch generierten erweiterten `UPDATE` und `DELETE` Anweisungen *nicht* mit Datensätzen, die `NULL` Werte enthalten. Sehen Sie sich die folgenden `UpdateCommand`an, um zu sehen, warum die SqlDataSource s

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

Die `UnitPrice` Spalte in der `Products` Tabelle kann `NULL` Werte aufweisen. Wenn ein bestimmter Datensatz über einen `NULL` Wert für `UnitPrice`verfügt, wird der `WHERE`-klauselteil `[UnitPrice] = @original_UnitPrice` *immer* auf false ausgewertet, da `NULL = NULL` immer false zurückgibt. Daher können Datensätze, die `NULL` Werte enthalten, nicht bearbeitet oder gelöscht werden, da die `WHERE` Klauseln `UPDATE` und `DELETE` keine zu aktualisierenden oder zu löschenden Zeilen zurückgeben.

> [!NOTE]
> Dieser Fehler wurde zuerst an Microsoft im Juni 2004 in [SqlDataSource](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) ausgegeben, es werden falsche SQL-Anweisungen generiert, und es wird geplant, dass er in der nächsten Version von ASP.net korrigiert wird.

Um dieses Problem zu beheben, müssen Sie die `WHERE`-Klauseln in den Eigenschaften `UpdateCommand` und `DeleteCommand` für **alle** Spalten, die `NULL` Werte aufweisen können, manuell aktualisieren. Ändern Sie im allgemeinen `[ColumnName] = @original_ColumnName` in:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Diese Änderung kann direkt über das deklarative Markup, über die Optionen UpdateQuery oder DeleteQuery aus dem Eigenschaftenfenster oder über die Registerkarten aktualisieren und löschen in der Option benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben in den Optionen Configure Data (benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur angeben) vorgenommen werden. Quellen-Assistent. Diese Änderung muss für *jede* Spalte in der `UpdateCommand`-und `DeleteCommand` s `WHERE`-Klausel vorgenommen werden, die `NULL` Werte enthalten kann.

Wenn Sie dies auf unser Beispiel anwenden, führt dies zu den folgenden geänderten `UpdateCommand` und `DeleteCommand` Werten:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Schritt 2: Hinzufügen einer GridView-Option mit Optionen zum Bearbeiten und löschen

Wenn SqlDataSource für die Unterstützung optimistischer Parallelität konfiguriert ist, besteht nur noch das Hinzufügen eines datenweb-Steuer Elements zu der Seite, die diese Parallelitäts Steuerung verwendet. Fügen Sie für dieses Tutorial eine GridView hinzu, die sowohl die Funktion "Bearbeiten" als auch "Löschen" bereitstellt. Ziehen Sie hierzu eine GridView aus der Toolbox auf den Designer, und legen Sie den `ID` auf `Products`fest. Binden Sie das GridView s-Smarttag an das `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource-Steuerelement, das Sie in Schritt 1 hinzugefügt haben. Überprüfen Sie abschließend die Optionen Aktivieren der Bearbeitung und Löschen des Smarttags.

[![die GridView an SqlDataSource binden und das Bearbeiten und löschen aktivieren.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Abbildung 6**: Binden der GridView an die SqlDataSource und Aktivieren der Bearbeitung und Löschung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))

Nachdem Sie die GridView hinzugefügt haben, konfigurieren Sie die Darstellung, indem Sie die `ProductID` BoundField entfernen, die `ProductName` BoundField s `HeaderText`-Eigenschaft in Product ändern und das `UnitPrice` BoundField aktualisieren, sodass die `HeaderText`-Eigenschaft einfach Price ist. Im Idealfall verbessern wir die Bearbeitungs Schnittstelle so, dass Sie ein "Requirements dfieldvalidator" für den `ProductName` Wert und einen CompareValidator für den `UnitPrice` Wert enthält (um sicherzustellen, dass es sich um einen ordnungsgemäß formatierten numerischen Wert handelt). Ausführliche Informationen zur Anpassung der GridView s-Bearbeitungs Schnittstelle finden Sie im Tutorial [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

> [!NOTE]
> Der GridView s-Ansichts Zustand muss aktiviert sein, da die ursprünglichen Werte, die von der GridView an die SqlDataSource übergeben werden, im Ansichts Zustand gespeichert werden.

Nachdem Sie diese Änderungen an der GridView vorgenommen haben, sollten das deklarative GridView-und SqlDataSource-Markup in etwa wie folgt aussehen:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Um die Steuerung der vollständigen Parallelität in Aktion zu sehen, öffnen Sie zwei Browserfenster, und laden Sie die `OptimisticConcurrency.aspx` Seite in beiden. Klicken Sie auf die Bearbeitungs Schaltflächen für das erste Produkt in beiden Browsern. Ändern Sie in einem Browser den Produktnamen, und klicken Sie auf aktualisieren. Der Browser wird Postback durchführt, und die GridView kehrt in den Vorbearbeitungs Modus zurück und zeigt den neuen Produktnamen für den soeben bearbeiteten Datensatz an.

Ändern Sie im zweiten Browserfenster den Preis (aber belassen Sie den Produktnamen als ursprünglichen Wert), und klicken Sie auf aktualisieren. Beim Postback kehrt das Raster in seinen vorab Bearbeitungsmodus zurück, aber die Änderung am Preis wird nicht aufgezeichnet. Der zweite Browser zeigt den gleichen Wert wie der erste, den neuen Produktnamen mit dem alten Preis. Die im zweiten Browserfenster vorgenommenen Änderungen sind verloren gegangen. Außerdem gingen die Änderungen nicht in Ruhe, da es keine Ausnahme oder Meldung gab, die darauf hinweist, dass gerade eine Parallelitäts Verletzung aufgetreten ist.

[![die Änderungen im zweiten Browser Fenster im Hintergrund verloren.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Abbildung 7**: die Änderungen im zweiten Browser Fenster gingen im Hintergrund verloren ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))

Der Grund, warum für die zweiten Browser Änderungen kein Commit ausgeführt wurde, war, weil die `UPDATE` Anweisung s `WHERE`-Klausel alle Datensätze herausgefiltert hat und daher keine Auswirkung auf Zeilen hatte. Sehen Sie sich die `UPDATE`-Anweisung erneut an:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Wenn das zweite Browserfenster den Datensatz aktualisiert, stimmt der in der `WHERE`-Klausel angegebene ursprüngliche Produktname nicht mit dem vorhandenen Produktnamen (da er vom ersten Browser geändert wurde) ab. Daher gibt die-Anweisung `[ProductName] = @original_ProductName` false zurück, und die `UPDATE` wirkt sich nicht auf Datensätze aus.

> [!NOTE]
> DELETE funktioniert auf die gleiche Weise. Wenn zwei Browserfenster geöffnet sind, beginnen Sie, indem Sie ein bestimmtes Produkt mit einem bestimmten Produkt bearbeiten und die Änderungen dann speichern. Nachdem Sie die Änderungen in einem Browser gespeichert haben, klicken Sie auf die Schaltfläche Löschen für das gleiche Produkt in der anderen. Da die ursprünglichen Werte in der `WHERE`-Klausel der `DELETE`-Anweisung nicht mit der Anweisung identisch sind, schlägt der Löschvorgang fehl.

Aus Sicht des Endbenutzers im zweiten Browserfenster kehrt das Raster nach dem Klicken auf die Schaltfläche "Aktualisieren" in den Vorbearbeitungs Modus zurück, die Änderungen sind jedoch verloren gegangen. Es gibt jedoch kein visuelles Feedback, dass Ihre Änderungen nicht geändert wurden. Wenn eine Änderung des Benutzers an einer Parallelitäts Verletzung verloren geht, benachrichtigen wir Sie im Idealfall und behalten das Raster ggf. im Bearbeitungsmodus. Sehen Sie sich an, wie Sie dies erreichen können.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Schritt 3: bestimmen, wann eine Parallelitäts Verletzung aufgetreten ist

Da die von Ihnen vorgenommenen Änderungen durch eine Parallelitäts Verletzung abgelehnt werden, wäre es sinnvoll, den Benutzer zu benachrichtigen, wenn eine Parallelitäts Verletzung aufgetreten ist. Um den Benutzer zu warnen, fügen Sie ein Label-websteuer Element oben auf der Seite mit dem Namen `ConcurrencyViolationMessage` hinzu, dessen `Text`-Eigenschaft die folgende Meldung anzeigt: Sie haben versucht, einen Datensatz zu aktualisieren oder zu löschen, der von einem anderen Benutzer gleichzeitig aktualisiert wurde. Überprüfen Sie die Änderungen des anderen Benutzers, und wiederholen Sie dann das Update oder den Löschvorgang. Legen Sie für die Bezeichnung Control s `CssClass`-Eigenschaft den Wert Warning fest. dabei handelt es sich um eine in `Styles.css` definierte CSS-Klasse, die Text in einer roten, kursiv, Fett und großen Schriftart anzeigt. Legen Sie abschließend die Eigenschaften der Bezeichnung s `Visible` und `EnableViewState` auf `false`fest. Dadurch wird die Bezeichnung ausgeblendet, außer nur bei den Postbacks, bei denen die `Visible`-Eigenschaft explizit auf `true`festgelegt wird.

[![der Seite ein Label-Steuerelement hinzufügen, um die Warnung anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Abbildung 8**: Hinzufügen eines Label-Steuer Elements zur Seite, um die Warnung anzuzeigen ([Klicken Sie, um das Bild in voller Größe](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png)anzuzeigen)

Wenn Sie ein Update oder eine Löschung ausführen, werden die GridView s `RowUpdated` und `RowDeleted` Ereignishandler ausgelöst, nachdem das Datenquellen-Steuerelement das angeforderte Update oder den angeforderten Löschvorgang durchgeführt hat. Wir können bestimmen, wie viele Zeilen vom Vorgang von diesen Ereignis Handlern betroffen sind. Wenn keine Zeilen betroffen sind, möchten wir die `ConcurrencyViolationMessage` Bezeichnung anzeigen.

Erstellen Sie einen Ereignishandler für die Ereignisse `RowUpdated` und `RowDeleted`, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

In beiden Ereignis Handlern wird die `e.AffectedRows`-Eigenschaft überprüft. Wenn Sie gleich 0 ist, legen Sie die Eigenschaft `ConcurrencyViolationMessage` Bezeichnung s `Visible` auf `true`fest. Im `RowUpdated`-Ereignishandler wird auch das GridView-Objekt angewiesen, im Bearbeitungsmodus zu bleiben, indem die `KeepInEditMode`-Eigenschaft auf true festgelegt wird. Dabei müssen wir die Daten erneut an das Raster binden, damit die anderen Benutzerdaten in die Bearbeitungs Schnittstelle geladen werden. Dies wird durch Aufrufen der GridView s-`DataBind()`-Methode erreicht.

Wie Abbildung 9 zeigt, wird bei diesen beiden Ereignis Handlern eine sehr merkliche Meldung angezeigt, wenn eine Parallelitäts Verletzung auftritt.

[![eine Meldung in Bezug auf eine Parallelitäts Verletzung angezeigt wird.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Abbildung 9**: eine Meldung wird angezeigt, wenn eine Parallelitäts Verletzung vorliegt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))

## <a name="summary"></a>Summary

Beim Erstellen einer Webanwendung, bei der mehrere gleichzeitige Benutzer möglicherweise dieselben Daten bearbeiten, ist es wichtig, die Optionen für die Parallelitäts Steuerung zu beachten. Standardmäßig verwenden die ASP.net-datenweb Steuerelemente und Datenquellen-Steuerelemente keine Parallelitäts Steuerung. Wie in diesem Tutorial gezeigt, ist die Implementierung der Steuerung der vollständigen Parallelität mit SqlDataSource relativ schnell und einfach. SqlDataSource verarbeitet den größten Teil der Arbeit-Anweisung für die hinzugefügten erweiterten `WHERE` Klauseln zu den automatisch generierten `UPDATE` und `DELETE` Anweisungen, aber es gibt einige Feinheiten bei der Behandlung von `NULL` Wert Spalten, wie im Abschnitt ordnungsgemäße Behandlung `NULL` Werte erläutert.

In diesem Tutorial wird die Untersuchung von SqlDataSource abgeschlossen. Unsere verbleibenden Lernprogramme werden mithilfe von ObjectDataSource und mehrstufigen Architektur wieder mit Daten arbeiten.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [Weiter](querying-data-with-the-sqldatasource-control-vb.md)
