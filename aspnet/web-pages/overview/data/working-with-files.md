---
uid: web-pages/overview/data/working-with-files
title: Arbeiten mit Dateien auf einer ASP.net Web Pages-(Razor-) Website | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie Dateien lesen, schreiben, anfügen, löschen und hochladen.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 684c47a8a8480dc040e5144144577c94c35d39e5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521883"
---
# <a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Arbeiten mit Dateien auf einer ASP.net Web Pages-(Razor-) Website

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Dateien auf einer ASP.net Web Pages (Razor-Website) lesen, schreiben, anfügen, löschen und hochladen.
> 
> > [!NOTE]
> > Wenn Sie Bilder hochladen und bearbeiten möchten (z. b. kippen oder Ändern der Größe), finden Sie weitere Informationen unter [Arbeiten mit Images an einer ASP.net Web Pages Website](/aspnet/web-pages/overview/ui-layouts-and-themes/9-working-with-images).
> 
> 
> **Lernen Sie Folgendes:** 
> 
> - Es wird erläutert, wie Sie eine Textdatei erstellen und Daten in diese schreiben.
> - Informationen zum Anfügen von Daten an eine vorhandene Datei.
> - Lesen einer Datei und anzeigen aus dieser Datei.
> - Löschen von Dateien von einer Website.
> - So können Sie Benutzern das Hochladen einer Datei oder mehrerer Dateien ermöglichen.
> 
> Dies sind die ASP.net-Programmierfunktionen, die im Artikel eingeführt wurden:
> 
> - Das `File`-Objekt, das eine Möglichkeit zum Verwalten von Dateien bietet.
> - Das `FileUpload`-Hilfsprogramm.
> - Das `Path`-Objekt, das Methoden bereitstellt, mit denen Sie Pfad-und Dateinamen bearbeiten können.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Im Tutorial verwendete Software Versionen
> 
> 
> - ASP.net Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Tutorial funktioniert auch mit webmatrix 3.

<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Erstellen einer Textdatei und Schreiben von Daten in diese Datei

Sie können nicht nur eine Datenbank auf Ihrer Website verwenden, sondern auch mit Dateien arbeiten. Beispielsweise können Sie Textdateien als einfache Methode zum Speichern von Daten für die Website verwenden. (Eine Textdatei, die zum Speichern von Daten verwendet wird, wird manchmal als *Flatfile*bezeichnet.) Text Dateien können sich in unterschiedlichen Formaten befinden, z. b *. txt*-, *XML*-oder *CSV* -Dateien (durch Trennzeichen getrennte Werte).

Wenn Sie Daten in einer Textdatei speichern möchten, können Sie die `File.WriteAllText`-Methode verwenden, um die zu erstellende Datei und die Daten anzugeben, die in die Datei geschrieben werden sollen. In diesem Verfahren erstellen Sie eine Seite, die ein einfaches Formular mit drei `input` Elementen (Vorname, Nachname und e-Mail-Adresse) und einer Schaltfläche zum **senden** enthält. Wenn der Benutzer das Formular sendet, speichern Sie die Eingabe des Benutzers in einer Textdatei.

1. Erstellen Sie einen neuen Ordner mit dem Namen *App\_Daten*, wenn dieser noch nicht vorhanden ist.
2. Erstellen Sie im Stammverzeichnis Ihrer Website eine neue Datei mit dem Namen " *UserData. cshtml*".
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Das HTML-Markup erstellt das Formular mit den drei Textfeldern. Im Code verwenden Sie die `IsPost`-Eigenschaft, um zu bestimmen, ob die Seite übermittelt wurde, bevor Sie mit der Verarbeitung beginnen.

    Die erste Aufgabe besteht darin, die Benutzereingaben zu erhalten und Variablen zuzuweisen. Der Code verkettet dann die Werte der separaten Variablen zu einer durch Trennzeichen getrennten Zeichenfolge, die dann in einer anderen Variablen gespeichert wird. Beachten Sie, dass das Komma Trennzeichen eine Zeichenfolge ist, die in Anführungszeichen (",") enthalten ist, da Sie buchstäblich ein Komma in die große Zeichenfolge einbetten, die Sie erstellen. Am Ende der Daten, die Sie miteinander verketten, fügen Sie `Environment.NewLine`hinzu. Dadurch wird ein Zeilenumbruch (ein Zeilenumbruch Zeichen) hinzugefügt. Die Erstellung mit all dieser Verkettung ist eine Zeichenfolge, die wie folgt aussieht:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Mit einem unsichtbaren Zeilenumbruch am Ende.)

    Anschließend erstellen Sie eine Variable (`dataFile`), die den Speicherort und den Namen der Datei enthält, in der die Daten gespeichert werden sollen. Das Festlegen des Speicher Orts erfordert eine besondere Behandlung. In Websites ist es eine bewährte Methode, Code auf absolute Pfade wie " *c:\folder\file.txt* " für Dateien auf dem Webserver zu verweisen. Wenn eine Website verschoben wird, ist ein absoluter Pfad falsch. Darüber hinaus wissen Sie für eine gehostete Site (im Gegensatz zu auf Ihrem eigenen Computer) in der Regel nicht einmal, was der richtige Pfad ist, wenn Sie den Code schreiben.

    Manchmal benötigen Sie jedoch einen kompletten Pfad, um eine Datei zu schreiben. Die Lösung besteht darin, die `MapPath`-Methode des `Server`-Objekts zu verwenden. Dadurch wird der gesamte Pfad zu Ihrer Website zurückgegeben. Wenn Sie den Pfad für den Website Stamm erhalten möchten, können Sie den `~`-Operator (um das virtuelle Stammverzeichnis der Site erneut vorzugeben), um `MapPath`zu erhalten. (Sie können auch einen Unterordner Namen an ihn übergeben, z. b. *~/App\_Data/* , um den Pfad für diesen Unterordner zu erhalten.) Anschließend können Sie zusätzliche Informationen zu den von der Methode zurückgegebenen Informationen verketten, um einen vollständigen Pfad zu erstellen. In diesem Beispiel fügen Sie einen Dateinamen hinzu. (Weitere Informationen zum Arbeiten mit Datei-und Ordner Pfaden finden Sie unter [Einführung in ASP.net Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Die Datei wird im Ordner *App\_Data* gespeichert. Dieser Ordner ist ein spezieller Ordner in ASP.net, der zum Speichern von Datendateien verwendet wird, wie unter Einführung in das [Arbeiten mit einer Datenbank an ASP.net Web Pages Websites](https://go.microsoft.com/fwlink/?LinkId=195209)beschrieben.

    Die `WriteAllText`-Methode des `File`-Objekts schreibt die Daten in die Datei. Diese Methode nimmt zwei Parameter an: den Namen (mit Pfad) der Datei, in die geschrieben werden soll, und die tatsächlich zu schreibenden Daten. Beachten Sie, dass der Name des ersten Parameters ein `@` Zeichen als Präfix aufweist. Dies weist ASP.net an, dass Sie einen ausführlichen Zeichenfolgenliteralen bereitstellen und dass Zeichen wie "/" nicht auf besondere Weise interpretiert werden sollten. (Weitere Informationen finden Sie unter [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Damit Ihr Code Dateien im *App-\_Daten* Ordner speichert, benötigt die Anwendung Lese-/Schreibberechtigungen für diesen Ordner. Auf dem Entwicklungs Computer ist dies in der Regel kein Problem. Wenn Sie Ihre Website jedoch auf dem Webserver eines hostinganbieters veröffentlichen, müssen Sie diese Berechtigungen möglicherweise explizit festlegen. Wenn Sie diesen Code auf dem Server eines hostinganbieters ausführen und Fehler erhalten, wenden Sie sich an den Hostinganbieter, um herauszufinden, wie diese Berechtigungen festgelegt werden.

- Führen Sie die Seite in einem Browser aus. 

    ![](working-with-files/_static/image1.jpg)
- Geben Sie Werte in die Felder ein, und klicken Sie dann auf **senden**.
- Schließen Sie den Browser.
- Kehren Sie zum Projekt zurück, und aktualisieren Sie die Ansicht.
- Öffnen Sie die Datei " *Data. txt* ". Die Daten, die Sie im Formular übermittelt haben, befinden sich in der Datei. 

    ![Klang](working-with-files/_static/image2.jpg)
- Schließen Sie die Datei " *Data. txt* ".

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Anfügen von Daten an eine vorhandene Datei

Im vorherigen Beispiel haben Sie `WriteAllText` verwendet, um eine Textdatei zu erstellen, die nur eine Datenquelle hat. Wenn Sie die Methode erneut aufzurufen und denselben Dateinamen übergeben, wird die vorhandene Datei vollständig überschrieben. Nachdem Sie jedoch eine Datei erstellt haben, möchten Sie häufig neue Daten am Ende der Datei hinzufügen. Hierfür können Sie die `AppendAllText`-Methode des `File`-Objekts verwenden.

1. Erstellen Sie auf der Website eine Kopie der Datei " *UserData. cshtml* ", und benennen Sie die Kopie " *userdatamultiple. cshtml*".
2. Ersetzen Sie den Codeblock vor dem öffnenden `<!DOCTYPE html>`-Tag durch den folgenden Codeblock: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Dieser Code weist eine Änderung im vorherigen Beispiel auf. Anstatt `WriteAllText`zu verwenden, wird `the AppendAllText` Methode verwendet. Die Methoden sind ähnlich, mit dem Unterschied, dass `AppendAllText` die Daten am Ende der Datei hinzufügt. Wie bei `WriteAllText`erstellt `AppendAllText` die Datei, wenn Sie nicht bereits vorhanden ist.
3. Führen Sie die Seite in einem Browser aus.
4. Geben Sie Werte für die Felder ein, und klicken Sie dann auf **senden**.
5. Fügen Sie weitere Daten hinzu, und senden Sie das Formular erneut.
6. Kehren Sie zum Projekt zurück, klicken Sie mit der rechten Maustaste auf den Projektordner, und klicken Sie dann auf **Aktualisieren**.
7. Öffnen Sie die Datei " *Data. txt* ". Sie enthält jetzt die neuen Daten, die Sie soeben eingegeben haben. 

    ![Klang](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lesen und Anzeigen von Daten aus einer Datei

Auch wenn Sie keine Daten in eine Textdatei schreiben müssen, müssen Sie möglicherweise manchmal Daten aus einer Datei lesen. Zu diesem Zweck können Sie das `File`-Objekt wieder verwenden. Sie können das `File`-Objekt verwenden, um jede Zeile einzeln (durch Zeilenumbrüche getrennt) zu lesen oder um einzelne Elemente unabhängig davon zu lesen, wie Sie getrennt sind.

In diesem Verfahren wird gezeigt, wie Sie die Daten lesen und anzeigen, die Sie im vorherigen Beispiel erstellt haben.

1. Erstellen Sie im Stammverzeichnis Ihrer Website eine neue Datei mit dem Namen " *displayData. cshtml*".
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Der Code beginnt mit dem Lesen der im vorherigen Beispiel erstellten Datei in eine Variable namens `userData`mithilfe dieses Methoden Aufrufes:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Der Code hierfür ist innerhalb einer `if`-Anweisung. Wenn Sie eine Datei lesen möchten, empfiehlt es sich, die `File.Exists`-Methode zu verwenden, um zuerst festzustellen, ob die Datei verfügbar ist. Der Code überprüft außerdem, ob die Datei leer ist.

    Der Text der Seite enthält zwei `foreach` Schleifen, von denen eine in der anderen geschachtelt ist. Die äußere `foreach` Schleife erhält jeweils eine Zeile aus der Datendatei. In diesem Fall werden die Zeilen in der Datei &#8212; durch Zeilenumbrüche definiert, d. h., jedes Datenelement befindet sich in einer eigenen Zeile. Die äußere Schleife erstellt ein neues Element (`<li>`-Element) in einer geordneten Liste (`<ol>`-Element).

    Die innere Schleife teilt jede Daten Zeile mithilfe eines Kommas als Trennzeichen in Elemente (Felder). (Auf der Grundlage des vorherigen Beispiels bedeutet dies, dass jede Zeile drei Felder &#8212; mit dem Vornamen, Nachnamen und der e-Mail-Adresse enthält, die jeweils durch ein Komma getrennt sind.) Die innere Schleife erstellt außerdem eine `<ul>` Liste und zeigt ein Listenelement für jedes Feld in der Daten Zeile an.

    Der Code veranschaulicht, wie zwei Datentypen verwendet werden, ein Array und der `char` Datentyp. Das Array ist erforderlich, da die `File.ReadAllLines`-Methode Daten als Array zurückgibt. Der `char`-Datentyp ist erforderlich, da die `Split`-Methode eine `array` zurückgibt, in der jedes Element vom Typ `char`ist. (Informationen zu Arrays finden Sie unter [Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Führen Sie die Seite in einem Browser aus. Die Daten, die Sie für die vorherigen Beispiele eingegeben haben, werden angezeigt. 

    ![Klang](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Anzeigen von Daten aus einer durch Trennzeichen getrennten Microsoft Excel-Datei**
> 
> Sie können Microsoft Excel verwenden, um die in einer Kalkulations Tabelle enthaltenen Daten als durch Trennzeichen getrennte Datei (*CSV* -Datei) zu speichern. Wenn Sie dies tun, wird die Datei im nur-Text-Format gespeichert, nicht im Excel-Format. Jede Zeile in der Tabelle wird durch einen Zeilenumbruch in der Textdatei getrennt, und jedes Datenelement wird durch ein Komma getrennt. Sie können den im vorherigen Beispiel gezeigten Code verwenden, um eine durch Kommas getrennte Excel-Datei zu lesen, indem Sie einfach den Namen der Datendatei im Code ändern.

<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Löschen von Dateien

Zum Löschen von Dateien von Ihrer Website können Sie die `File.Delete`-Methode verwenden. In diesem Verfahren wird gezeigt, wie Sie es Benutzern ermöglichen, ein Image (*JPG* -Datei) aus einem Ordner " *Images* " zu löschen, wenn Sie den Namen der Datei kennen.

> [!NOTE] 
> 
> **Wichtig** In einer Produktions Website beschränken Sie in der Regel, wer Änderungen an den Daten vornehmen darf. Informationen zum Einrichten der Mitgliedschaft und zu Methoden zum Autorisieren von Benutzern für die Ausführung von Aufgaben auf dem Standort finden Sie unter [Hinzufügen von Sicherheit und Mitgliedschaft zu einem ASP.net Web Pages-Standort](https://go.microsoft.com/fwlink/?LinkId=202904).

1. Erstellen Sie auf der Website einen Unterordner mit dem Namen *Images*.
2. Kopieren Sie mindestens eine *JPG* -Datei in den Ordner *Images* .
3. Erstellen Sie im Stammverzeichnis der Website eine neue Datei mit dem Namen " *filedelete. cshtml*".
4. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Diese Seite enthält ein Formular, in dem Benutzer den Namen einer Bilddatei eingeben können. Sie geben die Dateinamenerweiterung *. jpg* nicht ein. indem Sie den Dateinamen auf diese Weise einschränken, verhindern Sie, dass Benutzer beliebige Dateien auf Ihrer Website löschen.

    Der Code liest den Dateinamen, den der Benutzer eingegeben hat, und erstellt dann einen kompletten Pfad. Um den Pfad zu erstellen, verwendet der Code den aktuellen Website Pfad (wie von der `Server.MapPath`-Methode zurückgegeben), den Ordnernamen des *Images* , den Namen, den der Benutzer bereitgestellt hat, und ". jpg" als Literalzeichenfolge.

    Um die Datei zu löschen, ruft der Code die `File.Delete`-Methode auf und übergibt dabei den vollständigen Pfad, den Sie soeben erstellt haben. Am Ende des Markups zeigt der Code eine Bestätigungsmeldung an, dass die Datei gelöscht wurde.
5. Führen Sie die Seite in einem Browser aus. 

    ![Klang](working-with-files/_static/image5.jpg)
6. Geben Sie den Namen der zu löschenden Datei ein, und klicken Sie dann auf **senden**. Wenn die Datei gelöscht wurde, wird der Name der Datei am unteren Rand der Seite angezeigt.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Zulassen, dass Benutzer eine Datei hochladen

Mit dem `FileUpload`-Hilfsprogramm können Benutzer Dateien auf Ihre Website hochladen. Das folgende Verfahren zeigt, wie Sie es Benutzern ermöglichen können, eine einzelne Datei hochzuladen.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, wenn Sie Sie noch nicht hinzugefügt haben.
2. Erstellen Sie im Ordner *App\_Data* einen neuen Ordner, und nennen Sie ihn *UploadedFiles*.
3. Erstellen Sie im Stammverzeichnis eine neue Datei mit dem Namen *FileUpload. cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Der Textteil der Seite verwendet das `FileUpload`-Hilfsprogramm, um das uploadfeld und die Schaltflächen zu erstellen, mit denen Sie wahrscheinlich vertraut sind:

    ![Klang](working-with-files/_static/image6.jpg)

    Die Eigenschaften, die Sie für das `FileUpload`-Hilfsprogramm festlegen, geben an, dass ein einzelnes Feld für die Datei hochgeladen werden soll und dass die Schaltfläche "Senden" den **Upload**lesen soll. (Weitere Felder werden später in diesem Artikel hinzugefügt.)

    Wenn der Benutzer auf **hochladen**klickt, ruft der Code am oberen Rand der Seite die Datei ab und speichert Sie. Das `Request` Objekt, das Sie normalerweise zum erhalten von Werten aus Formularfeldern verwenden, verfügt auch über ein `Files` Array, das die hochgeladenen Dateien (oder Dateien) enthält. Sie können einzelne Dateien aus bestimmten Positionen im Array &#8212; abgleichen, um beispielsweise die erste hochgeladene Datei zu erhalten. Sie erhalten `Request.Files[0]`, um die zweite Datei zu erhalten, `Request.Files[1]`und so weiter. (Beachten Sie, dass beim Programmieren die Zählung in der Regel bei Null beginnt.)

    Wenn Sie eine hochgeladene Datei abrufen, platzieren Sie Sie in einer Variablen (hier `uploadedFile`), damit Sie Sie bearbeiten können. Zum Ermitteln des Namens der hochgeladenen Datei erhalten Sie einfach die `FileName`-Eigenschaft. Wenn der Benutzer jedoch eine Datei hochlädt, enthält `FileName` den ursprünglichen Namen des Benutzers, der den gesamten Pfad enthält. Dies könnte wie folgt aussehen:

    *C:\users\public\sample.txt*

    Sie möchten jedoch nicht alle Pfadinformationen, da dies der Pfad auf dem Computer des Benutzers ist, nicht auf Ihrem Server. Sie möchten nur den eigentlichen Dateinamen (*Sample. txt*). Sie können nur die Datei aus einem Pfad entfernen, indem Sie die `Path.GetFileName`-Methode wie folgt verwenden:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Das `Path`-Objekt ist ein Hilfsprogramm, das über eine Reihe von Methoden verfügt, die Sie zum Entfernen von Pfaden, Kombinieren von Pfaden usw. verwenden können.

    Nachdem Sie den Namen der hochgeladenen Datei erhalten haben, können Sie einen neuen Pfad erstellen, in dem Sie die hochgeladene Datei auf Ihrer Website speichern möchten. In diesem Fall kombinieren Sie `Server.MapPath`, die Ordnernamen (*App\_Data/UploadedFiles*) und den neu entfernen Dateinamen, um einen neuen Pfad zu erstellen. Sie können dann die `SaveAs`-Methode der hochgeladenen Datei aufzurufen, um die Datei zu speichern.
5. Führen Sie die Seite in einem Browser aus. 

    ![Klang](working-with-files/_static/image7.jpg)
6. Klicken Sie auf **Durchsuchen** , und wählen Sie eine Datei zum Hochladen aus. 

    ![Klang](working-with-files/_static/image8.jpg)

    Das Textfeld neben der Schaltfläche **Durchsuchen** enthält den Pfad und den Datei Speicherort.

    ![Klang](working-with-files/_static/image9.jpg)
7. Klicken Sie auf **Hochladen**.
8. Klicken Sie auf der Website mit der rechten Maustaste auf den Projektordner, und klicken Sie dann auf **Aktualisieren**.
9. Öffnen Sie den Ordner *UploadedFiles* . Die Datei, die Sie hochgeladen haben, befindet sich im Ordner. 

    ![Klang](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Zulassen, dass Benutzer mehrere Dateien hochladen

Im vorherigen Beispiel können Benutzer eine Datei hochladen. Sie können jedoch die `FileUpload`-Hilfsprogramm verwenden, um mehrere Dateien gleichzeitig hochzuladen. Dies ist praktisch für Szenarien wie das Hochladen von Fotos, bei denen das Hochladen einer Datei zu einem Zeitpunkt mühsam ist. (Informationen zum Hochladen von Fotos bei der [Arbeit mit Bildern finden Sie auf einer ASP.net Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202897).) In diesem Beispiel wird gezeigt, wie Sie es den Benutzern ermöglichen, zwei gleichzeitig hochzuladen. Sie können jedoch auch das gleiche Verfahren verwenden, um mehr als das hochzuladen.

1. Fügen Sie die ASP.net-webhilfsbibliothek Ihrer Website hinzu, wie unter [Installieren von Hilfsprogrammen an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372)beschrieben, falls Sie dies noch nicht getan haben.
2. Erstellen Sie eine neue Seite mit dem Namen *fileuploadmultiple. cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    In diesem Beispiel ist das `FileUpload`-Hilfsprogramm im Text der Seite so konfiguriert, dass Benutzer standardmäßig zwei Dateien hochladen können. Da `allowMoreFilesToBeAdded` auf `true`festgelegt ist, rendert das Hilfsprogramm einen Link, mit dem Benutzer weitere Uploadfelder hinzufügen können:

    ![Klang](working-with-files/_static/image11.jpg)

    Zum Verarbeiten der Dateien, die der Benutzer hochlädt, verwendet der Code die gleiche grundlegende Technik, die Sie im &#8212; vorherigen Beispiel verwendet haben, um eine Datei aus `Request.Files` zu erhalten und dann zu speichern. (Einschließlich der verschiedenen Dinge, die Sie tun müssen, um den richtigen Dateinamen und-Pfad zu erhalten.) Die Innovation ist dieses Mal, dass der Benutzer möglicherweise mehrere Dateien hochlädt, und Sie wissen nicht viele. Um dies zu ermitteln, können Sie `Request.Files.Count`abrufen.

    Mit dieser Zahl können Sie durch `Request.Files`Schleifen, jede Datei abrufen und speichern. Wenn Sie eine Reihe bekannter Zeiten durch eine Auflistung Schleifen möchten, können Sie wie folgt eine `for`-Schleife verwenden:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Die Variable `i` ist nur ein temporärer Wert, der von 0 (null) zu der von Ihnen festgelegten Obergrenze wechselt. In diesem Fall ist die Obergrenze die Anzahl von Dateien. Aber da der Zähler bei Null beginnt, wie es für das zählen von Szenarien in ASP.net typisch ist, ist die Obergrenze tatsächlich ein kleiner als die Dateianzahl. (Wenn drei Dateien hochgeladen werden, ist die Anzahl 0 (null) bis 2.)

    Die `uploadedCount` Variable umfasst alle Dateien, die erfolgreich hochgeladen und gespeichert wurden. Mit diesem Code wird die Möglichkeit berücksichtigt, dass eine erwartete Datei möglicherweise nicht hochgeladen werden kann.
4. Führen Sie die Seite in einem Browser aus. Der Browser zeigt die Seite und die beiden Upload-Felder an.
5. Wählen Sie zwei hoch zuladende Dateien aus.
6. Klicken Sie auf **weitere Datei hinzufügen**. Auf der Seite wird ein neues uploadfeld angezeigt. 

    ![Klang](working-with-files/_static/image12.jpg)
7. Klicken Sie auf **Hochladen**.
8. Klicken Sie auf der Website mit der rechten Maustaste auf den Projektordner, und klicken Sie dann auf **Aktualisieren**.
9. Öffnen Sie den Ordner *UploadedFiles* , um die erfolgreich hochgeladenen Dateien anzuzeigen.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Arbeiten mit Images an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exporting to a CSV File (Exportieren als CSV-Datei)](https://msdn.microsoft.com/library/ms155919.aspx)
