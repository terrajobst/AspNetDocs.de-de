---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Einschließen einer dateiuploadoption beim Hinzufügen eines neuen Datensatzes (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie Sie eine Weboberfläche erstellen, die es dem Benutzer ermöglicht, Textdaten einzugeben und Binärdateien hochzuladen. So veranschaulichen Sie die verfügbaren Optionen...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 8edaf1754eddd7b03f1c323d1bee13238582fc99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78475341"
---
# <a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Einschließen einer Dateiuploadoption beim Hinzufügen eines neuen Datensatzes (VB)

von [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-app herunterladen](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) oder [PDF herunterladen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> In diesem Tutorial wird gezeigt, wie Sie eine Weboberfläche erstellen, die es dem Benutzer ermöglicht, Textdaten einzugeben und Binärdateien hochzuladen. Zum Veranschaulichen der Optionen, die zum Speichern von Binärdaten verfügbar sind, wird eine Datei in der Datenbank gespeichert, während die andere im Dateisystem gespeichert wird.

## <a name="introduction"></a>Einführung

In den beiden vorherigen Tutorials haben wir Techniken zum Speichern von Binärdaten untersucht, die dem Anwendungs-Datenmodell zugeordnet sind. es wird erläutert, wie das FileUpload-Steuerelement verwendet wird, um Dateien vom Client an den Webserver zu senden, und wie diese Binärdaten in einem Data W angezeigt werden. EB-Steuerelement. Es ist jedoch noch zu besprechen, wie hochgeladene Daten mit dem Datenmodell verknüpft werden.

In diesem Tutorial wird eine Webseite erstellt, um eine neue Kategorie hinzuzufügen. Zusätzlich zu den Textfeldern für die Kategorien "Name" und "Description" muss auf dieser Seite zwei FileUpload-Steuerelemente für die neue Kategorie "s" und eine für die Broschüre enthalten sein. Das hochgeladene Bild wird direkt in der neuen Spalte "Datensatz s `Picture`" gespeichert, während die Broschüre im Ordner "`~/Brochures`" mit dem Pfad zu der Datei gespeichert wird, die in der `BrochurePath` Spalte "neuer Datensatz" gespeichert ist.

Vor dem Erstellen dieser neuen Webseite müssen wir die Architektur aktualisieren. Die Haupt Abfrage `CategoriesTableAdapter` s ruft nicht die `Picture` Spalte ab. Folglich enthält die automatisch generierte `Insert` Methode nur Eingaben für die Felder "`CategoryName`", "`Description`" und "`BrochurePath`". Daher müssen wir eine zusätzliche Methode im TableAdapter erstellen, die alle vier `Categories` Felder auffordert. Die `CategoriesBLL`-Klasse in der Geschäftslogik Schicht muss ebenfalls aktualisiert werden.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Schritt 1: Hinzufügen einer`InsertWithPicture`Methode zum`CategoriesTableAdapter`

Beim Erstellen des `CategoriesTableAdapter` im Tutorial [Erstellen einer Datenzugriffs Ebene](../introduction/creating-a-data-access-layer-vb.md) haben wir es so konfiguriert, dass basierend auf der Haupt Abfrage automatisch `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen generiert werden. Darüber hinaus haben wir den TableAdapter angewiesen, den direkten DB-Ansatz zu verwenden, der die Methoden `Insert`, `Update`und `Delete`erstellt hat. Diese Methoden führen die automatisch generierten `INSERT`-, `UPDATE`-und `DELETE`-Anweisungen aus und akzeptieren folglich Eingabeparameter basierend auf den Spalten, die von der Haupt Abfrage zurückgegeben werden. Im Tutorial [Hochladen von Dateien](uploading-files-vb.md) haben wir die Haupt Abfrage `CategoriesTableAdapter` s erweitert, um die `BrochurePath` Spalte zu verwenden.

Da die Haupt Abfrage von `CategoriesTableAdapter` s nicht auf die `Picture` Spalte verweist, können wir weder einen neuen Datensatz hinzufügen noch einen vorhandenen Datensatz mit einem Wert für die `Picture` Spalte aktualisieren. Um diese Informationen zu erfassen, können Sie entweder eine neue Methode im TableAdapter erstellen, die speziell zum Einfügen eines Datensatzes mit Binärdaten verwendet wird, oder Sie können die automatisch generierte `INSERT` Anweisung anpassen. Das Problem bei der Anpassung der automatisch generierten `INSERT`-Anweisung besteht darin, dass unsere Anpassungen durch den Assistenten überschrieben werden. Stellen Sie sich beispielsweise vor, dass die `INSERT`-Anweisung so angepasst wurde, dass Sie die Verwendung der `Picture` Spalte einschließt. Dadurch wird die TableAdapter s-`Insert`-Methode so aktualisiert, dass ein zusätzlicher Eingabeparameter für die Binärdaten der Kategorie s angezeigt wird. Wir könnten dann eine Methode in der Geschäftslogik Schicht erstellen, um diese dal-Methode zu verwenden und diese BLL-Methode über die Darstellungs Schicht aufzurufen, und alles funktioniert wunderbar. Das heißt, bis zum nächsten Mal, als wir den TableAdapter mit dem TableAdapter-Konfigurations-Assistenten konfiguriert haben. Sobald der Assistent abgeschlossen ist, werden die Anpassungen an der `INSERT`-Anweisung überschrieben, die `Insert`-Methode wird auf das alte Formular zurückgesetzt, und der Code würde nicht mehr kompiliert werden.

> [!NOTE]
> Diese Verärgerung ist ein nicht Problem, wenn Sie gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen verwenden. In einem zukünftigen Tutorial wird die Verwendung von gespeicherten Prozeduren anstelle von Ad-hoc-SQL-Anweisungen in der Datenzugriffs Ebene erläutert.

Um diese potenziellen Probleme zu vermeiden, sollten Sie nicht die automatisch generierten SQL-Anweisungen anpassen, sondern stattdessen eine neue Methode für den TableAdapter erstellen. Diese Methode mit dem Namen `InsertWithPicture`akzeptiert Werte für die Spalten `CategoryName`, `Description`, `BrochurePath`und `Picture` und führt eine `INSERT` Anweisung aus, die alle vier Werte in einem neuen Datensatz speichert.

Öffnen Sie das typisierte DataSet, und klicken Sie im Designer mit der rechten Maustaste auf den `CategoriesTableAdapter` s-Header, und wählen Sie im Kontextmenü Abfrage hinzufügen aus. Dadurch wird der Konfigurations-Assistent für TableAdapter-Abfragen gestartet, in dem Sie gefragt werden, wie die TableAdapter-Abfrage auf die Datenbank zugreifen soll. Wählen Sie SQL-Anweisungen verwenden, und klicken Sie auf weiter Im nächsten Schritt werden Sie aufgefordert, den Typ der zu generierenden Abfrage einzugeben. Da wir eine Abfrage neu erstellen, um der `Categories` Tabelle einen neuen Datensatz hinzuzufügen, wählen Sie einfügen aus, und klicken Sie auf Weiter.

[![wählen Sie die Option Einfügen aus.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Abbildung 1**: Auswählen der Option "Einfügen" ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))

Wir müssen nun die `INSERT` SQL-Anweisung angeben. Der Assistent schlägt automatisch eine `INSERT` Anweisung vor, die der TableAdapter s-Haupt Abfrage entspricht. In diesem Fall ist es eine `INSERT`-Anweisung, die die Werte `CategoryName`, `Description`und `BrochurePath` einfügt. Aktualisieren Sie die-Anweisung so, dass die `Picture`-Spalte zusammen mit einem `@Picture` Parameter enthalten ist, wie im folgenden Beispiel:

[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

Auf dem letzten Bildschirm des Assistenten werden Sie aufgefordert, die neue TableAdapter-Methode zu benennen. Geben Sie `InsertWithPicture`, und klicken Sie auf Fertigstellen

[![Name der neuen TableAdapter-Methode insertwithpicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Abbildung 2**: Benennen der neuen TableAdapter-Methode `InsertWithPicture` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))

## <a name="step-2-updating-the-business-logic-layer"></a>Schritt 2: Aktualisieren der Geschäftslogik Ebene

Da die Präsentationsschicht nur eine Schnittstelle mit der Geschäftslogik Schicht herstellen sollte, anstatt Sie zu umgehen, um direkt zur Datenzugriffs Ebene zu wechseln, müssen wir eine BLL-Methode erstellen, die die soeben erstellte dal-Methode aufruft (`InsertWithPicture`). Erstellen Sie für dieses Tutorial eine Methode in der `CategoriesBLL`-Klasse mit dem Namen `InsertWithPicture`, die als Eingabe drei `String` s und ein `Byte` Array akzeptiert. Die `String` Eingabeparameter gelten für den Namen, die Beschreibung und den Dateipfad der Kategorie, während das `Byte` Array für den binären Inhalt des Bild der Kategorie s steht. Wie der folgende Code zeigt, ruft diese BLL-Methode die entsprechende dal-Methode auf:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Stellen Sie sicher, dass Sie das typisierte Dataset gespeichert haben, bevor Sie der BLL die `InsertWithPicture`-Methode hinzufügen. Da der `CategoriesTableAdapter`-Klassen Code automatisch basierend auf dem typisierten DataSet generiert wird, wird die `Adapter`-Eigenschaft nicht über die `InsertWithPicture`-Methode informiert, wenn Sie die Änderungen am typisierten Dataset zuerst speichern.

## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Schritt 3: Auflisten der vorhandenen Kategorien und ihrer Binärdaten

In diesem Tutorial erstellen wir eine Seite, die es einem Endbenutzer ermöglicht, dem System eine neue Kategorie hinzuzufügen, die ein Bild und eine Broschüre für die neue Kategorie bereitstellt. Im [vorherigen Tutorial](displaying-binary-data-in-the-data-web-controls-vb.md) haben wir eine GridView mit einem TemplateField-und ImageField-Element verwendet, um den Namen, die Beschreibung, das Bild und einen Link für die einzelnen Kategorien anzuzeigen. Lassen Sie diese Funktionalität für dieses Tutorial replizieren, indem Sie eine Seite erstellen, die alle vorhandenen Kategorien auflistet und neue erstellt werden kann.

Öffnen Sie zunächst die Seite `DisplayOrDownload.aspx` aus dem Ordner `BinaryData`. Wechseln Sie zur Quell Ansicht, und kopieren Sie die deklarative Syntax GridView und ObjectDataSource s, indem Sie Sie in das `<asp:Content>`-Element in `UploadInDetailsView.aspx`einfügen. Vergessen Sie auch nicht, die `GenerateBrochureLink`-Methode aus der Code Behind-Klasse `DisplayOrDownload.aspx` in `UploadInDetailsView.aspx`zu kopieren.

[![kopieren Sie die deklarative Syntax aus displayordownload. aspx in uploadindetailsview. aspx, und fügen Sie Sie ein.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Abbildung 3**: Kopieren und Einfügen der deklarativen Syntax aus `DisplayOrDownload.aspx` in `UploadInDetailsView.aspx` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))

Nachdem Sie die deklarative Syntax und `GenerateBrochureLink` Methode auf die `UploadInDetailsView.aspx` Seite kopiert haben, können Sie die Seite über einen Browser anzeigen, um sicherzustellen, dass alles ordnungsgemäß kopiert wurde. Es sollte eine GridView mit den acht Kategorien angezeigt werden, die einen Link zum Herunterladen der Broschüre sowie das Bild der Kategorie enthält.

[![Sie nun jede Kategorie zusammen mit den zugehörigen Binärdaten sehen.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Abbildung 4**: Sie sollten nun jede Kategorie zusammen mit den zugehörigen Binärdaten sehen ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))

## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Schritt 4: Konfigurieren des`CategoriesDataSource`für die Unterstützung des Einfügens

Der von `Categories` GridView verwendete `CategoriesDataSource` ObjectDataSource bietet derzeit keine Möglichkeit zum Einfügen von Daten. Um das Einfügen über dieses Datenquellen-Steuerelement zu unterstützen, müssen wir die `Insert` Methode einer Methode im zugrunde liegenden Objekt `CategoriesBLL`zuordnen. Insbesondere möchten wir ihn der `CategoriesBLL` Methode zuordnen, die wir in Schritt 2 `InsertWithPicture`hinzugefügt haben.

Klicken Sie zunächst auf den Link Datenquelle konfigurieren des Smarttags ObjectDataSource s. Der erste Bildschirm zeigt das Objekt, für das die Datenquelle konfiguriert ist, `CategoriesBLL`. Belassen Sie diese Einstellung unverändert, und klicken Sie auf Weiter, um mit dem Bildschirm Daten Methoden definieren fortzufahren. Wechseln Sie zur Registerkarte Einfügen, und wählen Sie die `InsertWithPicture`-Methode aus der Dropdown Liste aus. Klicken Sie auf Fertigstellen, um den Assistenten abzuschließen.

[![konfigurieren Sie ObjectDataSource für die Verwendung der insertwithpicture-Methode.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Abbildung 5**: Konfigurieren von ObjectDataSource für die Verwendung der `InsertWithPicture`-Methode ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))

> [!NOTE]
> Nachdem Sie den Assistenten abgeschlossen haben, fragt Visual Studio möglicherweise, ob Sie Felder und Schlüssel aktualisieren möchten. Dadurch werden die Felder für die datenweb-Steuerelemente neu generiert. Wählen Sie Nein aus, da bei Auswahl von ja alle Feld Anpassungen überschrieben werden, die Sie möglicherweise vorgenommen haben.

Nachdem Sie den Assistenten abgeschlossen haben, enthält die ObjectDataSource nun einen Wert für die `InsertMethod`-Eigenschaft und `InsertParameters` für die vier Kategoriespalten, wie das folgende deklaratives Markup veranschaulicht:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Schritt 5: Erstellen der einfügeschnittstelle

Das DetailsView [-](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)Steuerelement bietet eine integrierte einfügeschnittstelle, die beim Arbeiten mit einem Datenquellen-Steuerelement, das das Einfügen unterstützt, verwendet werden kann. Fügen Sie dieser Seite oberhalb der GridView ein DetailsView-Steuerelement hinzu, das die einfügeschnittstelle permanent Rendering, sodass ein Benutzer schnell eine neue Kategorie hinzufügen kann. Beim Hinzufügen einer neuen Kategorie in der DetailsView wird die darunter liegende GridView automatisch aktualisiert und die neue Kategorie angezeigt.

Ziehen Sie zunächst eine DetailsView aus der Toolbox auf den Designer oberhalb der GridView, indem Sie die `ID`-Eigenschaft auf `NewCategory` festlegen und die Eigenschaftswerte `Height` und `Width` löschen. Binden Sie das DetailsView s-Smarttag an den vorhandenen `CategoriesDataSource` und aktivieren Sie dann das Kontrollkästchen einfügen aktivieren.

[![die DetailsView an die Kategorie "categoriesdatasource" binden und Einfügen aktivieren.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Abbildung 6**: Binden der DetailsView an den `CategoriesDataSource` und Aktivieren des[Einfügens (Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))

Um die DetailsView dauerhaft in der Einfügungs Schnittstelle zu erzeugen, legen Sie die `DefaultMode`-Eigenschaft auf `Insert`fest.

Beachten Sie, dass die DetailsView fünf boundfields-`CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`und `BrochurePath` enthält, obwohl das `CategoryID` BoundField nicht in der einfügeschnittstelle gerendert wird, da dessen `InsertVisible`-Eigenschaft auf `False`festgelegt ist. Diese boundfields sind vorhanden, da es sich um die Spalten handelt, die von der `GetCategories()`-Methode zurückgegeben werden, was die ObjectDataSource aufruft, um Ihre Daten abzurufen. Zum einfügen möchten wir jedoch nicht, dass der Benutzer einen Wert für `NumberOfProducts`angeben soll. Außerdem müssen wir zulassen, dass Sie ein Bild für die neue Kategorie hochladen und eine PDF-Datei für die Broschüre hochladen.

Entfernen Sie das `NumberOfProducts` BoundField vollständig aus der DetailsView, und aktualisieren Sie dann die `HeaderText` Eigenschaften der `CategoryName` und `BrochurePath` boundfields zu Category bzw.-Prospekt. Konvertieren Sie als nächstes das `BrochurePath` BoundField in ein TemplateField-Element, und fügen Sie ein neues TemplateField-Element für das Bild hinzu. Geben Sie diesem neuen TemplateField einen `HeaderText` Wert Bild an. Verschieben Sie das `Picture` TemplateField, sodass es zwischen der `BrochurePath` TemplateField und CommandField liegt.

![Die DetailsView an die Kategorie "kategoriesdatasource" binden und Einfügen aktivieren](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Abbildung 7**: Binden der DetailsView an den `CategoriesDataSource` und Aktivieren des Einfügens

Wenn Sie die `BrochurePath` BoundField im Dialogfeld Felder bearbeiten in ein TemplateField konvertiert haben, enthält das TemplateField eine `ItemTemplate`, `EditItemTemplate`und `InsertItemTemplate`. Es ist jedoch nur die `InsertItemTemplate` erforderlich, sodass Sie die anderen beiden Vorlagen entfernen können. Nun sollte die deklarative Syntax der DetailsView s wie folgt aussehen:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Hinzufügen von FileUpload-Steuerelementen für die Felder ""

Derzeit enthält das `BrochurePath` TemplateField s-`InsertItemTemplate` ein Textfeld, während das `Picture` TemplateField keine Vorlagen enthält. Wir müssen diese beiden TemplateField s-`InsertItemTemplate` en aktualisieren, damit Sie FileUpload-Steuerelemente verwenden können.

Wählen Sie im Smarttags DetailsView s die Option Vorlagen bearbeiten aus, und wählen Sie dann die `InsertItemTemplate` `BrochurePath` TemplateField s aus der Dropdown Liste aus. Entfernen Sie das Textfeld, und ziehen Sie dann ein FileUpload-Steuerelement aus der Toolbox in die Vorlage. Legen Sie die `ID` für das FileUpload-Steuerelement auf `BrochureUpload`fest. Fügen Sie der `Picture` TemplateField s `InsertItemTemplate`ein FileUpload-Steuerelement hinzu. Legen Sie die `ID` dieses FileUpload-Steuer Elements auf `PictureUpload`fest.

[![dem InsertItemTemplate ein FileUpload-Steuerelement hinzufügen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Abbildung 8**: Hinzufügen eines FileUpload-Steuer Elements zum `InsertItemTemplate` ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))

Nachdem Sie diese Ergänzungen erstellt haben, lauten die deklarative Syntax von TemplateField s wie folgt:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Wenn ein Benutzer eine neue Kategorie hinzufügt, möchten wir sicherstellen, dass die Broschüre und das Bild den richtigen Dateityp haben. Der Benutzer muss für die-Broschüre eine PDF-Datei angeben. Für das Bild benötigen wir den Benutzer, um eine Bilddatei hochzuladen, aber *Wir gestatten eine Bilddatei* oder nur Bilddateien eines bestimmten Typs, wie z. b. "GIFs" oder "jpgs"? Um unterschiedliche Dateitypen zu ermöglichen, müssen wir das `Categories` Schema so erweitern, dass es eine Spalte enthält, die den Dateityp erfasst, sodass dieser Typ über `Response.ContentType` in `DisplayCategoryPicture.aspx`an den Client gesendet werden kann. Da wir nicht über eine solche Spalte verfügen, wäre es ratsam, Benutzer so einzuschränken, dass Sie nur einen bestimmten Bild Dateityp bereitstellen. Die vorhandenen Images der `Categories` Tabelle sind Bitmaps, aber JPGs sind ein geeignetere Dateiformat für Images, die über das Web bereitgestellt werden.

Wenn ein Benutzer einen falschen Dateityp hochlädt, muss der Einfügevorgang abgebrochen und eine Meldung angezeigt werden, die auf das Problem hinweist. Fügen Sie ein Label-websteuer Element unterhalb der DetailsView hinzu. Legen Sie die `ID`-Eigenschaft auf `UploadWarning`, löschen Sie die `Text`-Eigenschaft, legen Sie die `CssClass`-Eigenschaft auf Warning und die `Visible`-und `EnableViewState`-Eigenschaften auf `False`fest. Die `Warning` CSS-Klasse wird in `Styles.css` definiert und rendert den Text in einer großen, roten, kursiv formatierten Schriftart.

> [!NOTE]
> Im Idealfall werden die `CategoryName`-und `Description` boundfields in templatefields konvertiert und ihre einfügeschnittstellen angepasst. Die `Description` einfügeschnittstelle wäre z. b. wahrscheinlich besser durch ein mehrzeilige Textfeld geeignet. Da die `CategoryName` Spalte `NULL` Werte nicht akzeptiert, sollte ein "Requirements dfieldvalidator"-Element hinzugefügt werden, um sicherzustellen, dass der Benutzer einen Wert für den neuen Kategorienamen bereitstellt. Diese Schritte werden dem Reader als Übung überlassen. Weitere Informationen zum Anpassen der Daten Änderungs Schnittstellen finden Sie unter [Anpassen der Daten Änderungs Schnittstelle](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Schritt 6: Speichern der hochgeladenen Broschüre im Datei System des Webserver s

Wenn Benutzer die Werte für eine neue Kategorie eingeben und auf die Schaltfläche einfügen klicken, wird ein Postback durchgeführt, und der Einfügungs Workflow wird ausgeführt. Zuerst wird das Ereignis "DetailsView s [`ItemInserting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) " ausgelöst. Im nächsten Schritt wird die ObjectDataSource s-`Insert()`-Methode aufgerufen, was dazu führt, dass der `Categories` Tabelle ein neuer Datensatz hinzugefügt wird. Danach wird das Ereignis "DetailsView s [`ItemInserted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) " ausgelöst.

Bevor die ObjectDataSource s-`Insert()` Methode aufgerufen wird, müssen wir zuerst sicherstellen, dass die entsprechenden Dateitypen vom Benutzer hochgeladen wurden, und dann die PDF-PDF-Datei im Webserver Dateisystem speichern. Erstellen Sie einen Ereignishandler für das DetailsView s `ItemInserting`-Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

Der Ereignishandler beginnt mit dem Verweis auf das `BrochureUpload` FileUpload-Steuerelement aus den DetailsView s-Vorlagen. Wenn eine Broschüre hochgeladen wurde, wird die Erweiterung für hochgeladene Dateien untersucht. , Wenn die Erweiterung nicht ist. PDF, dann wird eine Warnung angezeigt, die Einfügung wird abgebrochen, und die Ausführung des Ereignis Handlers wird beendet.

> [!NOTE]
> Die Verwendung der Erweiterung für hochgeladene Dateien ist keine sichere Methode, um sicherzustellen, dass es sich bei der hochgeladenen Datei um ein PDF-Dokument handelt. Der Benutzer kann über ein gültiges PDF-Dokument mit der Erweiterung `.Brochure`verfügen oder ein nicht-PDF-Dokument und eine `.pdf` Erweiterung erhalten. Der binäre Inhalt der Datei muss Programm gesteuert überprüft werden, um den Dateityp genauer zu überprüfen. Solch gründliche Ansätze sind jedoch häufig übertrieben. die Überprüfung der Erweiterung ist für die meisten Szenarien ausreichend.

Wie im Tutorial [Hochladen von Dateien](uploading-files-vb.md) erläutert, muss beim Speichern von Dateien im Dateisystem darauf geachtet werden, dass ein Benutzer das Hochladen anderer e nicht überschreibt. In diesem Tutorial versuchen wir, den gleichen Namen wie die hochgeladene Datei zu verwenden. Wenn bereits eine Datei im `~/Brochures` Verzeichnis mit demselben Dateinamen vorhanden ist, fügen wir eine Zahl am Ende an, bis ein eindeutiger Name gefunden wird. Wenn der Benutzer z. b. eine Datei mit dem Namen `Meats.pdf`hochlädt, aber bereits eine Datei mit dem Namen `Meats.pdf` im Ordner `~/Brochures`, ändern wir den Namen der gespeicherten Datei in `Meats-1.pdf`. Wenn dies der Fall ist, versuchen wir es `Meats-2.pdf`usw., bis ein eindeutiger Dateiname gefunden wird.

Im folgenden Code wird die [`File.Exists(path)`-Methode](https://msdn.microsoft.com/library/system.io.file.exists.aspx) verwendet, um zu bestimmen, ob eine Datei bereits mit dem angegebenen Dateinamen vorhanden ist. Wenn dies der Fall ist, werden neue Dateinamen für die Broschüre weiterhin ausprobiert, bis kein Konflikt gefunden wird.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Nachdem ein gültiger Dateiname gefunden wurde, muss die Datei im Dateisystem gespeichert werden, und der Wert von ObjectDataSource s `brochurePath``InsertParameter` muss aktualisiert werden, damit der Dateiname in die Datenbank geschrieben wird. Wie bereits im Tutorial Hochladen von *Dateien* gezeigt, kann die Datei mithilfe der `SaveAs(path)` Methode FileUpload-Steuerelement s gespeichert werden. Um den Parameter ObjectDataSource s `brochurePath` zu aktualisieren, verwenden Sie die `e.Values` Auflistung.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Schritt 7: Speichern des hochgeladenen Bilds in der Datenbank

Zum Speichern des hochgeladenen Bilds im neuen `Categories` Datensatz müssen wir den hochgeladenen Binär Inhalt dem ObjectDataSource s-`picture` Parameter im `ItemInserting` Ereignis DetailsView s zuweisen. Bevor wir diese Zuweisung vornehmen, müssen wir jedoch zunächst sicherstellen, dass das hochgeladene Bild ein JPG und kein anderer Bildtyp ist. Verwenden Sie wie in Schritt 6 die Dateierweiterung für hochgeladene Bilder, um den Typ zu ermitteln.

Obwohl die `Categories` Tabelle `NULL` Werte für die `Picture` Spalte zulässt, haben alle Kategorien derzeit ein Bild. Erzwingen Sie, dass der Benutzer ein Bild bereitstellt, wenn Sie auf dieser Seite eine neue Kategorie hinzufügen. Der folgende Code überprüft, um sicherzustellen, dass ein Bild hochgeladen wurde und über eine entsprechende Erweiterung verfügt.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Dieser Code sollte *vor* dem Code aus Schritt 6 platziert werden, sodass der Ereignishandler bei einem Problem mit dem Bild Upload beendet wird, bevor die Datei im Dateisystem gespeichert wird.

Wenn Sie davon ausgehen, dass eine geeignete Datei hochgeladen wurde, weisen Sie den hochgeladenen binären Inhalt dem Wert des Bildparameter s mit der folgenden Codezeile zu:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>Der Complete`ItemInserting`-Ereignis Handler

Der Vollständigkeit halber wird hier der `ItemInserting` Ereignishandler in seiner Gesamtheit aufgeführt:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Schritt 8: Beheben der`DisplayCategoryPicture.aspx`Seite

Nehmen Sie sich einen Moment Zeit, um die einfügeschnittstelle und den `ItemInserting` Ereignishandler zu testen, der in den letzten Schritten erstellt wurde. Besuchen Sie die Seite `UploadInDetailsView.aspx` über einen Browser, und versuchen Sie, eine Kategorie hinzuzufügen, aber lassen Sie das Bild Weg, oder geben Sie ein nicht-JPG-Bild oder eine nicht-PDF-Datei an. In jedem dieser Fälle wird eine Fehlermeldung angezeigt, und der INSERT-Workflow wurde abgebrochen.

[![eine Warnmeldung angezeigt wird, wenn ein ungültiger Dateityp hochgeladen wird](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Abbildung 9**: eine Warnmeldung wird angezeigt, wenn ein ungültiger Dateityp hochgeladen wird ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))

Wenn Sie sich vergewissert haben, dass für die Seite ein Bild hochgeladen werden muss, und nicht-PDF-oder nicht-JPG-Dateien akzeptieren, fügen Sie eine neue Kategorie mit einem gültigen JPG-Bild hinzu, und lassen Sie das Feld für die Broschüre leer. Nachdem Sie auf die Schaltfläche Einfügen geklickt haben, wird die Seite Postback, und der `Categories` Tabelle wird ein neuer Datensatz hinzugefügt, wobei die binären Inhalte der hochgeladenen Bilder direkt in der Datenbank gespeichert werden. Die GridView wird aktualisiert und zeigt eine Zeile für die neu hinzugefügte Kategorie an, aber wie in Abbildung 10 gezeigt, wird das Bild der neuen Kategorie nicht ordnungsgemäß gerendert.

[![das Bild der neuen Kategorie s nicht angezeigt wird.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Abbildung 10**: das Bild der neuen Kategorie s wird nicht angezeigt ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))

Der Grund für die nicht Anzeige des neuen Bilds ist, dass die `DisplayCategoryPicture.aspx` Seite, die ein bestimmtes Bild der Kategorie zurückgibt, so konfiguriert ist, dass Bitmaps mit einem OLE-Header verarbeitet werden. Dieser 78-Byte-Header wird aus den Binär Inhalten der `Picture` Spalte entfernt, bevor Sie an den Client zurückgesendet werden. Die JPG-Datei, die wir soeben für die neue Kategorie hochgeladen haben, verfügt jedoch nicht über diesen OLE-Header. Daher werden gültige, erforderliche Bytes aus den Binärdaten des Images entfernt.

Da jetzt sowohl Bitmaps mit OLE-Headern als auch JPGs in der `Categories` Tabelle vorhanden sind, müssen Sie `DisplayCategoryPicture.aspx` aktualisieren, damit der OLE-Header für die ursprünglichen acht Kategorien entfernt wird, und dieses entfernen für die neueren Kategoriedatensätze umgeht. Im nächsten Tutorial wird erläutert, wie Sie ein vorhandenes Image eines Datensatzes aktualisieren, und wir aktualisieren alle alten Kategoriebilder, sodass es sich um JPGs handelt. Verwenden Sie den folgenden Code jedoch in `DisplayCategoryPicture.aspx`, um die OLE-Header nur für die ursprünglichen acht Kategorien zu entfernen:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Mit dieser Änderung wird das JPG-Bild jetzt ordnungsgemäß in der GridView gerendert.

[![die JPG-Bilder für neue Kategorien korrekt gerendert werden](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Abbildung 11**: die JPG-Bilder für neue Kategorien werden korrekt gerendert ([Klicken Sie, um das Bild in voller Größe anzuzeigen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))

## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Schritt 9: Löschen der Broschüre im Falle einer Ausnahme

Eine der Herausforderungen beim Speichern von Binärdaten auf dem Dateisystem des Webservers besteht darin, dass eine Verbindung zwischen dem Datenmodell und den zugehörigen Binärdaten getrennt wird. Wenn ein Datensatz gelöscht wird, müssen daher die entsprechenden Binärdaten im Dateisystem ebenfalls entfernt werden. Dies kann auch beim Einfügen von auftreten. Betrachten Sie das folgende Szenario: ein Benutzer fügt eine neue Kategorie hinzu und gibt ein gültiges Bild und eine neue Broschüre an. Wenn Sie auf die Schaltfläche einfügen klicken, erfolgt ein Postback, und das DetailsView s-`ItemInserting` Ereignis wird ausgelöst, und die-Datei wird im Dateisystem des Webserver s gespeichert. Im nächsten Schritt wird die ObjectDataSource s-`Insert()`-Methode aufgerufen, die die `CategoriesBLL` Class s `InsertWithPicture`-Methode aufruft, die die `CategoriesTableAdapter` s `InsertWithPicture`-Methode aufruft.

Was geschieht, wenn die Datenbank offline ist, oder wenn ein Fehler in der `INSERT` SQL-Anweisung vorliegt? Wenn die Einfügung eindeutig ist, wird der Datenbank keine neue Kategoriezeile hinzugefügt. Die hochgeladene Infodatei befindet sich jedoch immer noch auf dem Dateisystem des Webservers! Diese Datei muss im Falle einer Ausnahme während des einfügeworkflows gelöscht werden.

Wie bereits im Tutorial [Behandeln von Ausnahmen der BLL-und Dal-Ebene in einer ASP.NET-Seite](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) erläutert, wird eine Ausnahme durch die verschiedenen Ebenen durchlaufen, wenn eine Ausnahme innerhalb der tiefen der Architektur ausgelöst wird. Auf der Darstellungs Ebene können wir ermitteln, ob eine Ausnahme vom DetailsView s `ItemInserted`-Ereignis aufgetreten ist. Dieser Ereignishandler stellt auch die Werte der ObjectDataSource s `InsertParameters`bereit. Daher können wir einen Ereignishandler für das `ItemInserted` Ereignis erstellen, das überprüft, ob eine Ausnahme aufgetreten ist. wenn dies der Fall ist, wird die durch den ObjectDataSource s `brochurePath` Parameter angegebene Datei gelöscht:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Zusammenfassung

Zum Bereitstellen einer webbasierten Schnittstelle zum Hinzufügen von Datensätzen, die Binärdaten enthalten, müssen mehrere Schritte ausgeführt werden. Wenn die Binärdaten direkt in der Datenbank gespeichert werden, ist es wahrscheinlich, dass Sie die Architektur aktualisieren und bestimmte Methoden hinzufügen, um den Fall zu behandeln, in dem Binärdaten eingefügt werden. Nachdem die Architektur aktualisiert wurde, ist der nächste Schritt das Erstellen der einfügeschnittstelle, die mit einer DetailsView durchgeführt werden kann, die so angepasst wurde, dass für jedes binäre Datenfeld ein FileUpload-Steuerelement enthalten ist. Die hochgeladenen Daten können dann im Dateisystem des Webserver s gespeichert oder einem Datenquellen Parameter im Ereignishandler DetailsView s `ItemInserting` zugewiesen werden.

Das Speichern von Binärdaten im Dateisystem erfordert mehr Planung als das direkte Speichern von Daten in der Datenbank. Ein Benennungs Schema muss ausgewählt werden, um zu verhindern, dass ein Benutzer das Hochladen von anderen en überschreibt. Außerdem müssen zusätzliche Schritte ausgeführt werden, um die hochgeladene Datei zu löschen, wenn die Datenbank nicht eingefügt werden kann.

Wir haben nun die Möglichkeit, dem System neue Kategorien mit einer-Grafik und einem Bild hinzuzufügen, aber wir haben noch erfahren, wie Sie Binärdaten für eine vorhandene Kategorie aktualisieren oder die Binärdaten für eine gelöschte Kategorie ordnungsgemäß entfernen. Diese beiden Themen werden im nächsten Tutorial erläutert.

Fröhliche Programmierung!

## <a name="about-the-author"></a>Zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor der sieben ASP/ASP. net-Bücher und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), hat seit 1998 mit Microsoft-Webtechnologien gearbeitet. Scott arbeitet als unabhängiger Berater, Ausbilder und Writer. Sein letztes Buch ist [*Sams Teach Yourself ASP.NET 2,0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er kann übermitchell@4GuysFromRolla.comerreicht werden [.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonders vielen Dank

Diese tutorialreihe wurde von vielen hilfreichen Reviewern geprüft. Die führenden Reviewer für dieses Tutorial waren Dave Gardner, Teresa Murphy und Bernadette Leigh. Möchten Sie meine bevorstehenden MSDN-Artikel überprüfen? Wenn dies der Fall ist, können Sie eine Zeile in [mitchell@4GuysFromRolla.comablegen.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](displaying-binary-data-in-the-data-web-controls-vb.md)
> [Weiter](updating-and-deleting-existing-binary-data-vb.md)
