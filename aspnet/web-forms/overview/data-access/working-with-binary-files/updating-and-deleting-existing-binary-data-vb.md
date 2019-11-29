---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Aktualisieren und Löschen von vorhandenen Binärdaten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir gesehen, wie das GridView-Steuerelement das Bearbeiten und Löschen von Textdaten vereinfacht. In diesem Tutorial sehen Sie, wie das GridView-Steuerelement auch die...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 27ff6941008b4e7bf6d632e4c248fd1d35fb3589
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621238"
---
# <a name="updating-and-deleting-existing-binary-data-vb"></a>Aktualisieren und Löschen von vorhandenen Binärdaten (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) oder [PDF herunterladen](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> In den vorherigen Tutorials haben wir gesehen, wie das GridView-Steuerelement das Bearbeiten und Löschen von Textdaten vereinfacht. In diesem Tutorial sehen Sie, wie das GridView-Steuerelement auch das Bearbeiten und Löschen von Binärdaten ermöglicht, unabhängig davon, ob Binärdaten in der Datenbank gespeichert oder im Dateisystem gespeichert werden.

## <a name="introduction"></a>Einführung

In den letzten drei Tutorials haben wir einige Funktionen zum Arbeiten mit Binärdaten hinzugefügt. Wir haben begonnen, der `Categories` Tabelle eine `BrochurePath` Spalte hinzuzufügen und die Architektur entsprechend zu aktualisieren. Wir haben auch Methoden für die Datenzugriffs Ebene und die Geschäftslogik Schicht hinzugefügt, um mit der `Picture` Spalte der Tabelle "categories" zu arbeiten, die die binären Inhalte einer Bilddatei enthält. Wir haben Webseiten erstellt, um die Binärdaten in einem GridView-Download Link für die Broschüre darzustellen, wobei das Bild der Kategorie s in einem `<img>`-Element angezeigt wird und eine DetailsView hinzugefügt wurde, um Benutzern das Hinzufügen einer neuen Kategorie und das Hochladen der zugehörigen Grafik-und Bilddaten zu ermöglichen.

Alles, was noch implementiert werden muss, ist die Möglichkeit, vorhandene Kategorien zu bearbeiten und zu löschen, die wir in diesem Tutorial mithilfe der integrierten Funktionen zum Bearbeiten und Löschen von GridView-Funktionen ausführen. Wenn eine Kategorie bearbeitet wird, kann der Benutzer optional ein neues Bild hochladen, oder die Kategorie kann die vorhandene Kategorie weiterhin verwenden. Für die-Broschüre können Sie entweder die vorhandene-Broschüre verwenden, eine neue-Broschüre hochladen oder angeben, dass der Kategorie kein Prospekt mehr zugeordnet ist. Legen Sie Los!

## <a name="step-1-updating-the-data-access-layer"></a>Schritt 1: Aktualisieren der Datenzugriffs Ebene

Die DAL verfügt über automatisch generierte `Insert`-, `Update`-und `Delete`-Methoden, aber diese Methoden wurden basierend auf der `CategoriesTableAdapter` s-Haupt Abfrage generiert, die nicht die `Picture` Spalte enthält. Daher enthalten die Methoden `Insert` und `Update` keine Parameter zum Angeben der Binärdaten für das Bild der Kategorie s. Wie im [vorherigen Tutorial](including-a-file-upload-option-when-adding-a-new-record-vb.md)beschrieben, müssen wir eine neue TableAdapter-Methode zum Aktualisieren der `Categories` Tabelle erstellen, wenn Sie Binärdaten angeben.

Öffnen Sie das typisierte DataSet, und klicken Sie im Designer mit der rechten Maustaste auf den Header `CategoriesTableAdapter` s, und wählen Sie im Kontextmenü Abfrage hinzufügen aus, um den Assistenten zum Konfigurieren von TableAdapter-Abfragen zu starten. Mit diesem Assistenten werden Sie gefragt, wie die TableAdapter-Abfrage auf die Datenbank zugreifen soll. Wählen Sie SQL-Anweisungen verwenden, und klicken Sie auf weiter Im nächsten Schritt werden Sie aufgefordert, den Typ der zu generierenden Abfrage einzugeben. Da wir eine Abfrage neu erstellen, um der `Categories` Tabelle einen neuen Datensatz hinzuzufügen, wählen Sie aktualisieren aus, und klicken Sie auf Weiter.

[![wählen Sie die Option "Aktualisieren"](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Abbildung 1**: Auswählen der Option "Aktualisieren" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image3.png))

Wir müssen nun die `UPDATE` SQL-Anweisung angeben. Der Assistent schlägt automatisch eine `UPDATE` Anweisung vor, die der Haupt Abfrage TableAdapter s entspricht (eine, die die Werte `CategoryName`, `Description`und `BrochurePath` aktualisiert). Ändern Sie die-Anweisung so, dass die `Picture`-Spalte zusammen mit einem `@Picture` Parameter enthalten ist, wie im folgenden Beispiel:

[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Auf dem letzten Bildschirm des Assistenten werden Sie aufgefordert, die neue TableAdapter-Methode zu benennen. Geben Sie `UpdateWithPicture`, und klicken Sie auf Fertigstellen

[![Name der neuen TableAdapter-Methode updatewithpicture](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Abbildung 2**: Benennen der neuen TableAdapter-Methode `UpdateWithPicture` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image6.png))

## <a name="step-2-adding-the-business-logic-layer-methods"></a>Schritt 2: Hinzufügen der Geschäftslogik Schicht-Methoden

Zusätzlich zum Aktualisieren der dal müssen wir die BLL aktualisieren, um Methoden zum Aktualisieren und Löschen einer Kategorie einzubeziehen. Dabei handelt es sich um die Methoden, die von der Präsentationsebene aufgerufen werden.

Zum Löschen einer Kategorie können wir die automatisch generierte `CategoriesTableAdapter` s `Delete`-Methode verwenden. Fügen Sie der `CategoriesBLL`-Klasse die folgende Methode hinzu:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

In diesem Tutorial erstellen Sie zwei Methoden zum Aktualisieren einer Kategorie, die die Daten des binären Bilds erwartet und die `UpdateWithPicture` Methode aufruft, die wir soeben der `CategoriesTableAdapter` hinzugefügt haben, und eine andere Methode, die nur die Werte `CategoryName`, `Description`und `BrochurePath` akzeptiert und `CategoriesTableAdapter` Anweisung `Update` Class s automatisch generierte verwendet. Der Grund für die Verwendung von zwei Methoden besteht darin, dass ein Benutzer unter bestimmten Umständen das Bild der Kategorie mit seinen anderen Feldern aktualisieren möchte. in diesem Fall muss der Benutzer das neue Bild hochladen. Die Binärdaten der hochgeladenen Bilder können dann in der `UPDATE`-Anweisung verwendet werden. In anderen Fällen ist der Benutzer möglicherweise nur an der Aktualisierung des Namens und der Beschreibung interessiert. Wenn die `UPDATE`-Anweisung jedoch auch die Binärdaten für die Spalte `Picture` erwartet, müssen wir diese Informationen ebenfalls bereitstellen. Dies erfordert einen zusätzlichen Trip zur Datenbank, um die Bilddaten für den zu bearbeitenden Datensatz wieder aufzunehmen. Daher benötigen wir zwei `UPDATE` Methoden. Die Geschäftslogik Schicht bestimmt, welche verwendet werden soll, je nachdem, ob beim Aktualisieren der Kategorie Bilddaten bereitgestellt werden.

Um dies zu vereinfachen, fügen Sie zwei Methoden zur `CategoriesBLL`-Klasse hinzu, die beide den Namen `UpdateCategory`haben. Der erste Wert sollte drei `String` s, ein `Byte` Array und eine `Integer` als Eingabeparameter akzeptieren. die zweite, nur drei `String` s und eine `Integer`. Die `String` Eingabeparameter gelten für die Kategorien "Name", "Beschreibung" und "Dateipfad". das `Byte` Array ist für den binären Inhalt des kategoriebilds bestimmt, und der `Integer` identifiziert die `CategoryID` des zu aktualisierenden Datensatzes. Beachten Sie, dass die erste Überladung den zweiten aufruft, wenn das übergebenen `Byte` Array `Nothing`ist:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Schritt 3: Kopieren über die INSERT-und View-Funktion

Im [vorherigen Tutorial](including-a-file-upload-option-when-adding-a-new-record-vb.md) haben wir eine Seite mit dem Namen `UploadInDetailsView.aspx` erstellt, die alle Kategorien in einer GridView aufgelistet hat, und eine DetailsView zum Hinzufügen neuer Kategorien zum System bereitgestellt. In diesem Tutorial wird das GridView-Projekt erweitert, um Unterstützung für die Bearbeitung und Löschung zu bieten. Anstatt die Arbeit von `UploadInDetailsView.aspx`fortzusetzen, können Sie die Änderungen an diesem Tutorial in der `UpdatingAndDeleting.aspx` Seite des gleichen Ordners `~/BinaryData`durchführen. Kopieren Sie das deklarative Markup und den Code aus `UploadInDetailsView.aspx` in `UpdatingAndDeleting.aspx`, und fügen Sie es ein.

Öffnen Sie zunächst die Seite `UploadInDetailsView.aspx`. Kopieren Sie die gesamte deklarative Syntax innerhalb des `<asp:Content>` Elements, wie in Abbildung 3 gezeigt. Öffnen Sie als nächstes `UpdatingAndDeleting.aspx`, und fügen Sie dieses Markup innerhalb seines `<asp:Content>` Elements ein. Kopieren Sie auf ähnliche Weise den Code aus der Code Behind-Klasse der `UploadInDetailsView.aspx` Page s in `UpdatingAndDeleting.aspx`.

[![das deklarative Markup aus "uploadindetailsview. aspx" Kopieren](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Abbildung 3**: Kopieren des deklarativen Markups aus `UploadInDetailsView.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image9.png))

Nachdem Sie das deklarative Markup und den Code kopiert haben, besuchen Sie `UpdatingAndDeleting.aspx`. Die gleiche Ausgabe sollte angezeigt werden, und Sie erhalten dieselbe Benutzer Darstellung wie bei der `UploadInDetailsView.aspx` Seite aus dem vorherigen Tutorial.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Schritt 4: Hinzufügen der Unterstützung für das Löschen zu ObjectDataSource und GridView

Wie bereits in der Übersicht über das [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) beschrieben, bietet das GridView integrierte Löschfunktionen. diese Funktionen können im Tick eines Kontrollkästchens aktiviert werden, wenn die zugrunde liegende Datenquelle des Rasters das Löschen unterstützt. Derzeit wird das Löschen von "ObjectDataSource", an das die GridView gebunden ist (`CategoriesDataSource`), nicht unterstützt.

Um dieses Problem zu beheben, klicken Sie auf die Option Datenquelle konfigurieren des Smarttags ObjectDataSource s, um den Assistenten zu starten. Der erste Bildschirm zeigt, dass ObjectDataSource für die Zusammenarbeit mit der `CategoriesBLL`-Klasse konfiguriert ist. Treffen Sie auf Weiter. Zurzeit werden nur die Eigenschaften "ObjectDataSource s `InsertMethod`" und "`SelectMethod`" angegeben. Der Assistent hat jedoch die Dropdown Listen auf den Registerkarten aktualisieren und löschen automatisch mit den Methoden `UpdateCategory` und `DeleteCategory` aufgefüllt. Dies liegt daran, dass in der `CategoriesBLL`-Klasse diese Methoden mithilfe der `DataObjectMethodAttribute` als Standardmethoden zum Aktualisieren und löschen gekennzeichnet wurden.

Legen Sie vorerst die Dropdown Liste Update Registerkarte auf (keine) fest. lassen Sie die Dropdown Liste für die Registerkarte löschen jedoch auf `DeleteCategory`festgelegt. Wir kehren zu diesem Assistenten in Schritt 6 zurück, um Aktualisierungs Unterstützung hinzuzufügen.

[![Konfigurieren von ObjectDataSource für die Verwendung der deletecategory-Methode](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Abbildung 4**: Konfigurieren von ObjectDataSource für die Verwendung der `DeleteCategory`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image12.png))

> [!NOTE]
> Nachdem Sie den Assistenten abgeschlossen haben, fragt Visual Studio möglicherweise, ob Sie Felder und Schlüssel aktualisieren möchten. Dadurch werden die Felder für die datenweb-Steuerelemente neu generiert. Wählen Sie Nein aus, da bei Auswahl von ja alle Feld Anpassungen überschrieben werden, die Sie möglicherweise vorgenommen haben.

ObjectDataSource enthält nun einen Wert für die `DeleteMethod`-Eigenschaft sowie eine `DeleteParameter`. Wenn Sie den Assistenten verwenden, um die Methoden anzugeben, legt Visual Studio die ObjectDataSource s `OldValuesParameterFormatString`-Eigenschaft auf `original_{0}`fest, wodurch Probleme mit den Aufrufen der Update-und Delete-Methode verursacht werden. Löschen Sie diese Eigenschaft daher vollständig, oder setzen Sie Sie auf den Standardwert (`{0}`) zurück. Wenn Sie den Arbeitsspeicher für diese ObjectDataSource-Eigenschaft aktualisieren müssen, finden Sie weitere Informationen unter Tutorial zum [Einfügen, aktualisieren und Löschen von Daten](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) .

Nachdem Sie den Assistenten abgeschlossen und die `OldValuesParameterFormatString`behoben haben, sollte das deklarative Markup von ObjectDataSource s in etwa wie folgt aussehen:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Fügen Sie nach dem Konfigurieren von ObjectDataSource der GridView-Funktion Löschfunktionen hinzu, indem Sie das Kontrollkästchen "Löschen" im GridView s-Smarttag aktivieren. Dadurch wird der GridView ein CommandField hinzugefügt, dessen `ShowDeleteButton`-Eigenschaft auf `True`festgelegt ist.

[![Aktivieren der Unterstützung für das Löschen in der GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Abbildung 5**: Aktivieren der Unterstützung für das Löschen in der GridView ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image15.png))

Nehmen Sie sich einen Moment Zeit, um die Lösch Funktionalität zu testen. Es gibt einen Fremdschlüssel zwischen der `Products` Tabelle s `CategoryID` und der `Categories` Tabelle s `CategoryID`. Daher erhalten Sie eine Ausnahme Ausnahme bei der FOREIGN KEY-Einschränkung, wenn Sie versuchen, eine der ersten acht Kategorien zu löschen. Um diese Funktionalität zu testen, fügen Sie eine neue Kategorie hinzu, die sowohl eine Broschüre als auch ein Bild bereitstellt. Meine Test Kategorie, wie in Abbildung 6 gezeigt, enthält eine Test Prospekt Datei namens `Test.pdf` und ein Testbild. Abbildung 7 zeigt die GridView, nachdem die Test Kategorie hinzugefügt wurde.

[![eine Test Kategorie mit einer Broschüre und einem Image hinzufügen](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen einer Test Kategorie mit einer Broschüre und einem Bild ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image18.png))

[![nach dem Einfügen der Test Kategorie wird Sie in der GridView angezeigt.](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Abbildung 7**: nach dem Einfügen der Test Kategorie wird diese in der GridView angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image21.png))

Aktualisieren Sie in Visual Studio die Projektmappen-Explorer. Nun sollte eine neue Datei im Ordner "`~/Brochures`" angezeigt werden, `Test.pdf` (siehe Abbildung 8).

Klicken Sie dann in der Zeile Test Kategorie auf den Link löschen, sodass die Seite Postback und die `CategoriesBLL` Klasse `DeleteCategory` Methode ausgelöst wird. Dadurch wird die DAL s-`Delete`-Methode aufgerufen, wodurch die entsprechende `DELETE` Anweisung an die Datenbank gesendet wird. Die Daten werden dann in die GridView-Ansicht zurückgesetzt, und das Markup wird an den Client zurückgesendet, wobei die Test Kategorie nicht mehr vorhanden ist.

Während der Lösch Workflow den testkategoriedatensatz erfolgreich aus der `Categories` Tabelle entfernt hat, hat er seine Infodatei nicht aus dem Dateisystem des Webserver s entfernt. Aktualisieren Sie die Projektmappen-Explorer, und Sie sehen, dass `Test.pdf` sich noch im `~/Brochures` Ordner befindet.

![Die Datei "Test. pdf" wurde nicht aus dem Datei System des Webserver s gelöscht.](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Abbildung 8**: die `Test.pdf` Datei wurde nicht aus dem Datei System des Webserver s gelöscht.

## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Schritt 5: Entfernen der Datei mit der gelöschten Kategorie s

Eine der Nachteile beim Speichern von Binärdaten, die sich außerhalb der Datenbank befinden, besteht darin, dass zum Bereinigen dieser Dateien zusätzliche Schritte ausgeführt werden müssen, wenn der zugehörige Datenbankdaten Satz gelöscht wird. GridView und ObjectDataSource stellen Ereignisse bereit, die vor und nach der Ausführung des Lösch Befehls ausgelöst werden. Wir müssen tatsächlich Ereignishandler für die Ereignisse vor und nach der Aktion erstellen. Bevor der `Categories` Datensatz gelöscht wird, müssen wir den Pfad der PDF-Datei festlegen, aber wir möchten die PDF-Datei nicht löschen, bevor die Kategorie gelöscht wird, falls eine Ausnahme vorliegt und die Kategorie nicht gelöscht wird.

Das GridView s [`RowDeleting`-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) wird ausgelöst, bevor der Befehl "ObjectDataSource s Delete" aufgerufen wurde, während das [`RowDeleted` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) nach ausgelöst wird. Erstellen Sie Ereignishandler für diese beiden Ereignisse mit dem folgenden Code:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

Im Ereignishandler `RowDeleting` wird die `CategoryID` der zu löschenden Zeile aus der GridView s-`DataKeys` Collection abgerufen, auf die in diesem Ereignishandler über die `e.Keys` Auflistung zugegriffen werden kann. Als nächstes wird die `CategoriesBLL`-Klasse `GetCategoryByCategoryID(categoryID)` aufgerufen, um Informationen über den Daten Satz zurückzugeben, der gelöscht wird. Wenn das zurückgegebene `CategoriesDataRow` Objekt über einen Wert verfügt, der nicht`NULL``BrochurePath` ist, wird es in der Seiten Variablen `deletedCategorysPdfPath` gespeichert, sodass die Datei im `RowDeleted` Ereignishandler gelöscht werden kann.

> [!NOTE]
> Anstatt die `BrochurePath` Details für den `Categories` Datensatz abzurufen, der im `RowDeleting`-Ereignishandler gelöscht wird, könnten wir die `BrochurePath` der GridView s `DataKeyNames`-Eigenschaft hinzufügen und auf den Wert des Datensatzes über die `e.Keys` Auflistung zugreifen. Dies würde die Ansichts Zustands Größe des GridView-s leicht erhöhen, würde jedoch die benötigte Menge an Code reduzieren und eine Fahrt zur Datenbank speichern.

Nachdem der zugrunde liegende DELETE-Befehl von ObjectDataSource s aufgerufen wurde, wird der GridView s `RowDeleted`-Ereignishandler ausgelöst. Wenn beim Löschen der Daten keine Ausnahmen vorhanden waren und ein Wert für `deletedCategorysPdfPath`vorhanden ist, wird die PDF-Datei aus dem Dateisystem gelöscht. Beachten Sie, dass dieser zusätzliche Code nicht erforderlich ist, um die dem Bild zugeordneten Binärdaten der Kategorie s zu bereinigen. Dies ist der Fall, da die Bilddaten direkt in der Datenbank gespeichert werden. beim Löschen der `Categories` Zeile werden auch die Bild Daten der Kategorie gelöscht.

Nachdem Sie die beiden Ereignishandler hinzugefügt haben, führen Sie diesen Testfall erneut aus. Beim Löschen der Kategorie wird auch die zugehörige PDF-Datei gelöscht.

Das Aktualisieren eines vorhandenen Datensatzes mit zugeordneten Binärdaten stellt einige interessante Herausforderungen dar. Im restlichen Teil dieses Tutorials wird das Hinzufügen von Aktualisierungs Funktionen für die Broschüre und das Bild erläutert. In Schritt 6 werden Techniken zum Aktualisieren der Informationen zur Broschüre erläutert, während Schritt 7 das Aktualisieren des Bilds untersucht.

## <a name="step-6-updating-a-category-s-brochure"></a>Schritt 6: Aktualisieren einer Kategorie-s-Broschüre

Wie in der Übersicht über das Einfügen, aktualisieren und Löschen von Daten erläutert wird, bietet die GridView integrierte Bearbeitungs Unterstützung auf Zeilenebene, die durch den Tick eines Kontrollkästchens implementiert werden kann, wenn die zugrunde liegende Datenquelle entsprechend konfiguriert ist. Derzeit ist die `CategoriesDataSource` ObjectDataSource noch nicht für die Unterstützung der Aktualisierung konfiguriert. Fügen Sie diese also in hinzu.

Klicken Sie im Assistenten für ObjectDataSource s auf den Link Datenquelle konfigurieren, und fahren Sie mit dem zweiten Schritt fort. Aufgrund der `DataObjectMethodAttribute`, die in `CategoriesBLL`verwendet werden, sollte die Dropdown Liste Update automatisch mit der `UpdateCategory`-Überladung aufgefüllt werden, die vier Eingabeparameter akzeptiert (für alle Spalten, aber `Picture`). Ändern Sie diese, sodass Sie die-Überladung mit fünf Parametern verwendet.

[![konfigurieren Sie ObjectDataSource für die Verwendung der UpdateCategory-Methode, die einen Parameter für das Bild enthält.](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Abbildung 9**: Konfigurieren von ObjectDataSource für die Verwendung der `UpdateCategory`-Methode, die einen Parameter für `Picture` enthält ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image24.png))

ObjectDataSource enthält nun einen Wert für die `UpdateMethod`-Eigenschaft sowie die entsprechenden `UpdateParameter` s. Wie in Schritt 4 notiert, legt Visual Studio die Eigenschaft ObjectDataSource s `OldValuesParameterFormatString` auf `original_{0}` fest, wenn der Assistent zum Konfigurieren von Datenquellen verwendet wird. Dies führt zu Problemen mit den Aufrufen der Update-und Delete-Methode. Löschen Sie diese Eigenschaft daher vollständig, oder setzen Sie Sie auf den Standardwert (`{0}`) zurück.

Nachdem Sie den Assistenten abgeschlossen und die `OldValuesParameterFormatString`behoben haben, sollte das deklarative Markup von ObjectDataSource s wie folgt aussehen:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Um die integrierten Funktionen für GridView s zu aktivieren, aktivieren Sie die Option zum Aktivieren der Bearbeitung aus dem GridView s-Smarttag. Dadurch wird die Eigenschaft CommandField s `ShowEditButton` auf `True`festgelegt, was dazu führt, dass eine Bearbeitungs Schaltfläche (und die Schaltflächen aktualisieren und Abbrechen für die Zeile, die bearbeitet wird) hinzugefügt wird.

[![Konfigurieren der GridView zum unterstützen der Bearbeitung](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Abbildung 10**: Konfigurieren der GridView zum unterstützen der Bearbeitung ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image27.png))

Besuchen Sie die Seite über einen Browser, und klicken Sie auf eine der Bearbeitungs Schaltflächen der Zeile. Die `CategoryName`-und `Description` boundfields werden als Textfelder gerendert. In der `BrochurePath` TemplateField fehlt ein `EditItemTemplate`, sodass die `ItemTemplate` einen Link zur Broschüre angezeigt wird. Das `Picture` ImageField wird als Textfeld gerendert, dessen `Text`-Eigenschaft dem Wert des ImageField s-`DataImageUrlField` Werts zugewiesen ist, in diesem Fall `CategoryID`.

[![der GridView eine Bearbeitungs Schnittstelle für "brochurepath" fehlt.](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Abbildung 11**: in der GridView fehlt eine Bearbeitungs Schnittstelle für `BrochurePath` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image30.png))

## <a name="customizing-thebrochurepaths-editing-interface"></a>Anpassen der`BrochurePath`s-Bearbeitungs Schnittstelle

Wir müssen eine Bearbeitungs Schnittstelle für die `BrochurePath` TemplateField erstellen, die dem Benutzer Folgendes ermöglicht:

- Die Kategorie "s" unverändert lassen,
- Aktualisieren Sie die kategoriekategorie, indem Sie eine neue Broschüre hochladen.
- Entfernen Sie die Broschüre der Kategorie s ganz (falls die Kategorie nicht mehr über eine zugehörige Broschüre verfügt).

Wir müssen auch die Bearbeitungs Schnittstelle `Picture` ImageField s aktualisieren, aber dies wird in Schritt 7 angezeigt.

Klicken Sie in der GridView s Smarttags auf den Link Vorlagen bearbeiten, und wählen Sie in der Dropdown Liste die `BrochurePath` TemplateField s `EditItemTemplate` aus. Fügen Sie dieser Vorlage ein RadioButton List-websteuer Element hinzu, und legen Sie dessen `ID`-Eigenschaft auf `BrochureOptions` und dessen `AutoPostBack`-Eigenschaft auf `True`fest. Klicken Sie im Eigenschaftenfenster auf die Auslassungs Punkte in der `Items`-Eigenschaft, sodass der `ListItem` Auflistungs-Editor angezeigt wird. Fügen Sie die folgenden drei Optionen mit den `Value` s 1, 2 bzw. 3 hinzu:

- Aktuelle Broschüre verwenden
- Aktuelle Broschüre entfernen
- Neue Broschüre hochladen

Legen Sie die erste `ListItem` s `Selected`-Eigenschaft auf `True`fest.

![Hinzufügen von drei ListItems zur RadioButton List](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Abbildung 12**: Hinzufügen von drei `ListItem` s zur RadioButton List

Fügen Sie unterhalb der RadioButtonList ein FileUpload-Steuerelement mit dem Namen `BrochureUpload`hinzu. Legen Sie die `Visible`-Eigenschaft auf `False`fest.

[![dem EditItemTemplate ein RadioButton List-Steuerelement und ein FileUpload-Steuerelement hinzufügen](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Abbildung 13**: Hinzufügen eines RadioButton List-Steuer Elements und eines FileUpload-Steuer Elements zum `EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image33.png))

Diese RadioButtonList bietet die drei Optionen für den Benutzer. Die Idee ist, dass das FileUpload-Steuerelement nur angezeigt wird, wenn die letzte Option Neue Broschüre hochladen ausgewählt ist. Erstellen Sie hierzu einen Ereignishandler für das RadioButton List s `SelectedIndexChanged`-Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Da sich das RadioButton List-Steuerelement und das FileUpload-Steuerelement in einer Vorlage befinden, müssen wir ein wenig Code schreiben, um Programm gesteuert auf diese Steuerelemente zuzugreifen. Dem `SelectedIndexChanged`-Ereignishandler wird ein Verweis auf die RadioButtonList im `sender`-Eingabeparameter übergeben. Um das FileUpload-Steuerelement zu erhalten, müssen wir das übergeordnete Steuerelement RadioButton List s erhalten und die `FindControl("controlID")`-Methode von dort aus verwenden. Sobald ein Verweis auf die Steuerelemente RadioButton List und FileUpload vorhanden ist, wird die Eigenschaft FileUpload-Steuerelement s `Visible` nur auf `True` festgelegt, wenn das RadioButton List s-`SelectedValue` gleich 3 ist. Dies ist der `Value` für die `ListItem`neue Broschüre hochladen.

Wenn dieser Code vorhanden ist, nehmen Sie sich einen Moment Zeit, um die Bearbeitungs Schnittstelle zu testen. Klicken Sie für eine Zeile auf die Schaltfläche Bearbeiten. Anfänglich sollte die Option Aktuelle Broschüre verwenden ausgewählt werden. Durch das Ändern des ausgewählten Indexes wird ein Postback verursacht. Wenn die dritte Option ausgewählt ist, wird das Steuerelement FileUpload angezeigt, andernfalls wird es ausgeblendet. In Abbildung 14 wird die Bearbeitungs Schnittstelle angezeigt, wenn auf die Schaltfläche Bearbeiten geklickt wird. Abbildung 15 zeigt die Benutzeroberfläche, nachdem die Option Neue Broschüre hochladen ausgewählt ist.

[![Anfangs ist die Option Aktuelle Broschüre verwenden ausgewählt.](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Abbildung 14**: anfänglich ist die Option Aktuelle Broschüre verwenden ausgewählt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image36.png))

[![wählen Sie die Option Neue Broschüre hochladen zeigt das FileUpload-Steuerelement an.](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Abbildung 15**: Auswählen der Option "neue Broschüre hochladen" zeigt das FileUpload-Steuerelement[an (Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image39.png))

## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Speichern der Datei mit der Datei und Aktualisieren der`BrochurePath`Spalte

Wenn auf die Schaltfläche zum Aktualisieren von GridView s geklickt wird, wird das `RowUpdating` Ereignis ausgelöst. Der Update-Befehl von ObjectDataSource s wird aufgerufen, und dann wird das GridView s `RowUpdated`-Ereignis ausgelöst. Wie beim Lösch Workflow müssen auch Ereignishandler für beide Ereignisse erstellt werden. Im Ereignishandler `RowUpdating` müssen wir basierend auf der `SelectedValue` der `BrochureOptions` RadioButton List festlegen, welche Aktion ausgeführt werden soll:

- Wenn die `SelectedValue` 1 ist, möchten wir weiterhin dieselbe `BrochurePath` Einstellung verwenden. Daher müssen wir den Parameter ObjectDataSource s `brochurePath` auf den vorhandenen `BrochurePath` Wert des Datensatzes festlegen, der aktualisiert wird. Der ObjectDataSource s-`brochurePath` Parameter kann mit `e.NewValues["brochurePath"] = value`festgelegt werden.
- Wenn das `SelectedValue` 2 ist, möchten wir den Wert für Datensatz s `BrochurePath` auf `NULL`festlegen. Dies kann durch Festlegen des Parameters ObjectDataSource s `brochurePath` auf `Nothing`erreicht werden, was dazu führt, dass in der `UPDATE` Anweisung eine Datenbank `NULL` verwendet wird. Wenn eine vorhandene Datei entfernt wird, die entfernt wird, müssen Sie die vorhandene Datei löschen. Dies ist jedoch nur dann der Fall, wenn das Update abgeschlossen ist, ohne dass eine Ausnahme ausgelöst wird.
- Wenn die `SelectedValue` 3 ist, sollten Sie sicherstellen, dass der Benutzer eine PDF-Datei hochgeladen und anschließend im Dateisystem gespeichert und den Wert für den Datensatz s `BrochurePath` Spalte aktualisiert. Wenn eine vorhandene, zu ersetzende Infodatei vorhanden ist, müssen wir außerdem die vorherige Datei löschen. Dies ist jedoch nur dann der Fall, wenn das Update abgeschlossen ist, ohne dass eine Ausnahme ausgelöst wird.

Die erforderlichen Schritte, die ausgeführt werden müssen, wenn das RadioButton List s-`SelectedValue` 3 ist, sind nahezu identisch mit den Schritten, die vom DetailsView s `ItemInserting`-Ereignishandler verwendet werden. Dieser Ereignishandler wird ausgeführt, wenn ein neuer Kategoriedatensatz aus dem DetailsView-Steuerelement hinzugefügt wird, das im [vorherigen Tutorial](including-a-file-upload-option-when-adding-a-new-record-vb.md)hinzugefügt wurde. Daher ist es uns, diese Funktionalität in separate Methoden zu umgestalten. Insbesondere habe ich die allgemeinen Funktionen in zwei Methoden verschoben:

- `ProcessBrochureUpload(FileUpload, out bool)` akzeptiert als Eingabe eine FileUpload-Steuerelement Instanz und einen booleschen Ausgabewert, der angibt, ob der DELETE-oder Edit-Vorgang fortgesetzt werden soll, oder ob er aufgrund eines Validierungs Fehlers abgebrochen werden soll. Diese Methode gibt den Pfad zur gespeicherten Datei oder `null` zurück, wenn keine Datei gespeichert wurde.
- `DeleteRememberedBrochurePath` löscht die Datei, die durch den Pfad in der Seiten Variablen angegeben wird `deletedCategorysPdfPath`, wenn `deletedCategorysPdfPath` nicht `null`ist.

Der Code für diese beiden Methoden folgt. Beachten Sie die Ähnlichkeit zwischen `ProcessBrochureUpload` und dem Ereignishandler DetailsView s `ItemInserting` aus dem vorherigen Tutorial. In diesem Tutorial haben Sie die Ereignishandler der DetailsView s so aktualisiert, dass diese neuen Methoden verwendet werden. Laden Sie den diesem Tutorial zugeordneten Code herunter, um die Änderungen an den Ereignis Handlern für die DetailsView s anzuzeigen.

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Die GridView s `RowUpdating`-und `RowUpdated`-Ereignishandler verwenden die Methoden `ProcessBrochureUpload` und `DeleteRememberedBrochurePath`, wie im folgenden Code gezeigt:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Beachten Sie, dass der `RowUpdating` Ereignishandler eine Reihe von Bedingungs Anweisungen verwendet, um die entsprechende Aktion auf der Grundlage des `BrochureOptions` RadioButtonList s `SelectedValue`-Eigenschafts Werts auszuführen.

Wenn Sie diesen Code erstellt haben, können Sie eine Kategorie bearbeiten und die aktuelle Broschüre verwenden, ohne eine beliebige Broschüre verwenden oder eine neue hochladen. Probieren Sie es aus. Legen Sie Haltepunkte im `RowUpdating` fest, und `RowUpdated` Sie Ereignishandler, um einen Eindruck vom Workflow zu erhalten.

## <a name="step-7-uploading-a-new-picture"></a>Schritt 7: Hochladen eines neuen Bilds

Die `Picture` Bearbeitungs Schnittstelle von ImageField s wird als Textfeld gerendert, das mit dem Wert aus der `DataImageUrlField`-Eigenschaft aufgefüllt wird. Während des Bearbeitungs Workflows übergibt die GridView einen Parameter an die ObjectDataSource mit dem Parameter s Name the value of the ImageField s `DataImageUrlField` Eigenschaft und dem Wert des Parameters s den Wert, der in das Textfeld in der Bearbeitungs Schnittstelle eingegeben wurde. Dieses Verhalten ist geeignet, wenn das Image als Datei auf dem Dateisystem gespeichert wird und die `DataImageUrlField` die vollständige URL des Bilds enthält. Unter diesen Umständen zeigt die Bearbeitungs Schnittstelle die Bild-URL im Textfeld an, das der Benutzer ändern und wieder in der Datenbank speichern kann. Diese Standardschnittstelle lässt nicht zu, dass der Benutzer ein neues Bild hochladen kann, aber die URL des Bilds kann vom aktuellen Wert in einen anderen geändert werden. Für dieses Tutorial reicht die Standard Bearbeitungs Schnittstelle von ImageField s jedoch nicht aus, da die `Picture` Binärdaten direkt in der Datenbank gespeichert werden und die `DataImageUrlField`-Eigenschaft nur den `CategoryID`enthält.

Um besser zu verstehen, was in diesem Tutorial passiert, wenn ein Benutzer eine Zeile mit einem ImageField bearbeitet, sehen Sie sich das folgende Beispiel an: ein Benutzer bearbeitet eine Zeile mit `CategoryID` 10 und bewirkt, dass das `Picture` ImageField als Textfeld mit dem Wert 10 angezeigt wird. Stellen Sie sich vor, dass der Benutzer den Wert in diesem Textfeld in 50 ändert, und klickt auf die Schaltfläche Aktualisieren. Ein Postback tritt auf, und die GridView erstellt zunächst einen Parameter mit dem Namen `CategoryID` mit dem Wert 50. Bevor die GridView diesen Parameter sendet (und die Parameter `CategoryName` und `Description`), werden die Werte aus der `DataKeys` Auflistung hinzugefügt. Daher überschreibt Sie den `CategoryID`-Parameter mit den zugrunde liegenden `CategoryID` Werten der aktuellen Zeile, 10. Kurz gesagt: die Bearbeitungs Schnittstelle von ImageField s wirkt sich nicht auf den Bearbeitungs Workflow für dieses Tutorial aus, da die Namen der ImageField s-`DataImageUrlField`-Eigenschaft und der `DataKey` Wert des Raster s gleich sind.

Obwohl das ImageField das Anzeigen eines Bilds auf der Grundlage von Datenbankdaten vereinfacht, möchten wir in der Bearbeitungs Schnittstelle kein Textfeld bereitstellen. Stattdessen soll ein FileUpload-Steuerelement angeboten werden, das der Endbenutzer verwenden kann, um das Bild der Kategorie s zu ändern. Im Gegensatz zum `BrochurePath` Wert haben wir für diese Tutorials entschieden, dass jede Kategorie ein Bild enthalten muss. Aus diesem Grund muss der Benutzer nicht angeben können, dass es kein zugeordnetes Bild gibt, dass der Benutzer entweder ein neues Bild hochladen oder das aktuelle Bild unverändert lassen möchte.

Um die Bearbeitungs Schnittstelle von ImageField s anzupassen, müssen wir Sie in ein TemplateField-Element konvertieren. Klicken Sie im GridView s-Smarttag auf den Link Spalten bearbeiten, wählen Sie das ImageField aus, und klicken Sie auf das Feld dieses Feld in einen TemplateField-Link konvertieren.

![Konvertieren von ImageField in ein TemplateField-Element](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Abbildung 16**: Konvertieren von ImageField in ein TemplateField-Element

Wenn Sie das ImageField-Element auf diese Weise in ein TemplateField konvertieren, wird ein TemplateField mit zwei Vorlagen generiert. Wie die folgende deklarative Syntax zeigt, enthält die `ItemTemplate` ein Image-websteuer Element, dessen `ImageUrl`-Eigenschaft mithilfe der Datenbindung-Syntax zugewiesen wird, die auf den Eigenschaften `DataImageUrlField` und `DataImageUrlFormatString` von ImageField s basiert. Der `EditItemTemplate` enthält ein Textfeld, dessen `Text`-Eigenschaft an den von der `DataImageUrlField`-Eigenschaft angegebenen Wert gebunden ist.

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Wir müssen die `EditItemTemplate` aktualisieren, um ein FileUpload-Steuerelement zu verwenden. Klicken Sie in der GridView s-Smarttag auf den Link Vorlagen bearbeiten, und wählen Sie dann in der Dropdown Liste die `EditItemTemplate` `Picture` TemplateField s aus. In der Vorlage sollte ein TextBox-Steuer Tool angezeigt werden. Ziehen Sie als nächstes ein FileUpload-Steuerelement aus der Toolbox in die Vorlage, und legen Sie dessen `ID` auf `PictureUpload`fest. Fügen Sie außerdem den Text hinzu, um das Bild der Kategorie s zu ändern, geben Sie ein neues Bild an. Lassen Sie das Feld auch für die Vorlage leer, damit die Kategorie s nicht identisch ist.

[![dem EditItemTemplate ein FileUpload-Steuerelement hinzufügen](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Abbildung 17**: Hinzufügen eines FileUpload-Steuer Elements zum `EditItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image42.png))

Nachdem Sie die Bearbeitungs Schnittstelle angepasst haben, können Sie Ihren Fortschritt in einem Browser anzeigen. Wenn eine Zeile im schreibgeschützten Modus angezeigt wird, wird das Bild der Kategorie s wie zuvor angezeigt, aber durch Klicken auf die Schaltfläche "Bearbeiten" wird die Bild Spalte als Text mit einem FileUpload-Steuerelement gerendert.

[![die Bearbeitungs Schnittstelle ein FileUpload-Steuerelement enthält](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Abbildung 18**: die Bearbeitungs Schnittstelle enthält ein FileUpload-Steuerelement ([Klicken Sie, um das Bild in voller Größe anzuzeigen](updating-and-deleting-existing-binary-data-vb/_static/image45.png))

Beachten Sie, dass ObjectDataSource so konfiguriert ist, dass die `CategoriesBLL` Klasse s `UpdateCategory` Methode aufgerufen wird, die als Eingabe die Binärdaten für das Bild als `Byte` Array akzeptiert. Wenn dieses Array `Nothing`ist, wird jedoch die Alternative `UpdateCategory` Überladung aufgerufen, die die `UPDATE` SQL-Anweisung ausgibt, die die `Picture` Spalte nicht ändert, sodass das aktuelle Bild der Kategorie unverändert bleibt. Daher müssen Sie im GridView s `RowUpdating`-Ereignishandler Programm gesteuert auf das `PictureUpload` FileUpload-Steuerelement verweisen und ermitteln, ob eine Datei hochgeladen wurde. Wenn eine nicht hochgeladen wurde, möchten wir *keinen* Wert für den `picture`-Parameter angeben. Wenn dagegen eine Datei in das `PictureUpload` FileUpload-Steuerelement hochgeladen wurde, möchten wir sicherstellen, dass es sich um eine JPG-Datei handelt. Wenn dies der Fall ist, können wir den binären Inhalt mithilfe des `picture`-Parameters an die ObjectDataSource senden.

Wie bei dem in Schritt 6 verwendeten Code ist ein großer Teil des hier benötigten Codes bereits im `ItemInserting` Ereignishandler DetailsView s vorhanden. Daher habe ich die allgemeinen Funktionen in eine neue Methode geändert, `ValidPictureUpload`und den `ItemInserting`-Ereignishandler aktualisiert, um diese Methode zu verwenden.

Fügen Sie den folgenden Code am Anfang des GridView s `RowUpdating`-Ereignis Handlers ein. Es ist wichtig, dass dieser Code vor dem Code steht, der die Datei der Broschüre speichert, da wir die Broschüre nicht im Dateisystem des Webserver speichern möchten, wenn eine ungültige Bilddatei hochgeladen wird.

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

Die `ValidPictureUpload(FileUpload)`-Methode nimmt ein FileUpload-Steuerelement als einzigen Eingabeparameter an und überprüft die hochgeladene Dateierweiterung, um sicherzustellen, dass die hochgeladene Datei ein JPG ist. Sie wird nur aufgerufen, wenn eine Bilddatei hochgeladen wird. Wenn keine Datei hochgeladen wird, wird der Bildparameter nicht festgelegt und verwendet daher seinen Standardwert `Nothing`. Wenn ein Bild hochgeladen wurde und `ValidPictureUpload` `True`zurückgibt, werden dem `picture`-Parameter die Binärdaten des hochgeladenen Bilds zugewiesen. Wenn die Methode `False`zurückgibt, wird der Update Workflow abgebrochen, und der Ereignishandler wurde beendet.

Der `ValidPictureUpload(FileUpload)`-Methoden Code, der aus dem DetailsView s `ItemInserting`-Ereignishandler umgestaltet wurde, sieht folgendermaßen aus:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Schritt 8: Ersetzen der ursprünglichen Kategoriebilder durch JPGs

Denken Sie daran, dass es sich bei den ursprünglichen acht Kategorien um Bitmapdateien handelt, die in einen OLE-Header Nachdem wir nun die Funktion zum Bearbeiten eines vorhandenen Bild Datensatzes hinzugefügt haben, nehmen Sie sich einen Moment Zeit, um diese Bitmaps durch JPGs zu ersetzen. Wenn Sie weiterhin die aktuellen Kategoriebilder verwenden möchten, können Sie Sie mithilfe der folgenden Schritte in jpgs konvertieren:

1. Speichern Sie die Bitmap-Images auf der Festplatte. Besuchen Sie die `UpdatingAndDeleting.aspx` Seite in Ihrem Browser, und klicken Sie für jede der ersten acht Kategorien mit der rechten Maustaste auf das Bild, und wählen Sie die Option zum Speichern des Bilds aus.
2. Öffnen Sie das Bild im Bild-Editor Ihrer Wahl. Sie können z. b. Microsoft Paint verwenden.
3. Speichert die Bitmap als JPG-Bild.
4. Aktualisieren Sie die kategorieabbildung über die Bearbeitungs Schnittstelle, und verwenden Sie dabei die JPG-Datei.

Nachdem eine Kategorie bearbeitet und das JPG-Bild hochgeladen wurde, wird das Bild im Browser nicht mehr angezeigt, da die `DisplayCategoryPicture.aspx` Seite die ersten 78 Bytes aus den Bildern der ersten acht Kategorien entfernt. Beheben Sie diesen Fehler, indem Sie den Code entfernen, der den OLE-Header entfernt. Anschließend sollte der `DisplayCategoryPicture.aspx``Page_Load` Ereignishandler nur den folgenden Code aufweisen:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> Die `UpdatingAndDeleting.aspx` Seite s zum Einfügen und Bearbeiten von Schnittstellen könnte etwas mehr Arbeit verwenden. Die `CategoryName`-und `Description` boundfields-Eigenschaft in DetailsView und GridView sollten in templatefields konvertiert werden. Da `CategoryName` keine `NULL` Werte zulässt, sollte ein "Requirements dfieldvalidator"-Element hinzugefügt werden. Und das Textfeld "`Description`" sollte wahrscheinlich in ein mehrzeilige Textfeld konvertiert werden. Ich lasse Ihnen diese abschließenden Berührungen als Übung für Sie überlassen.

## <a name="summary"></a>Summary

In diesem Tutorial wird die Arbeit mit Binärdaten behandelt. In diesem Tutorial und den vorherigen drei Informationen wurde erläutert, wie Binärdaten im Dateisystem oder direkt in der Datenbank gespeichert werden können. Ein Benutzer stellt Binärdaten für das System bereit, indem er eine Datei auf der Festplatte auswählt und Sie auf den Webserver hochlädt, wo Sie im Dateisystem gespeichert oder in die Datenbank eingefügt werden kann. ASP.NET 2,0 enthält ein FileUpload-Steuerelement, das die Bereitstellung einer solchen Schnittstelle so einfach wie das ziehen und ablegen erleichtert. Wie im Tutorial [Hochladen von Dateien](uploading-files-vb.md) erwähnt, eignet sich das FileUpload-Steuerelement jedoch nur für relativ kleine Datei Uploads, die im Idealfall einen Megabyte nicht überschreiten. Außerdem wurde erläutert, wie hochgeladene Daten mit dem zugrunde liegenden Datenmodell verknüpft werden, und wie die Binärdaten aus vorhandenen Datensätzen bearbeitet und gelöscht werden.

Unsere nächste Reihe von Tutorials untersucht verschiedene Caching-Techniken. Caching bietet die Möglichkeit, die Gesamtleistung einer Anwendung zu verbessern, indem die Ergebnisse von teuren Vorgängen übernommen und an einem Speicherort gespeichert werden, auf den Sie schneller zugreifen können.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Lead Reviewer für dieses Tutorial war Teresa Murphy. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorheriges](including-a-file-upload-option-when-adding-a-new-record-vb.md)
