---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Arbeiten mit Bildern auf einer ASP.net Web Pages-(Razor-) Website | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie Bilder (Größe, kippen und Hinzufügen von Wasserzeichen) in Ihrer Website hinzufügen, anzeigen und bearbeiten.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 53514b3c314fc182a43c82974ffcfa8158a636a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512877"
---
# <a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Arbeiten mit Bildern auf einer ASP.net Web Pages-(Razor-) Website

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Artikel wird das Hinzufügen, anzeigen und Bearbeiten von Bildern (Ändern der Größe, kippen und Hinzufügen von Wasserzeichen) auf einer ASP.net Web Pages-Website (Razor-Website) veranschaulicht.
> 
> Sie lernen Folgendes:
> 
> - Gewusst wie: Dynamisches Hinzufügen eines Bilds zu einer Seite.
> - So ermöglichen Sie Benutzern das Hochladen eines Bilds.
> - Ändern der Größe eines Bilds.
> - Vorgehensweise beim Kippen oder Drehen eines Bilds.
> - Vorgehensweise beim Hinzufügen eines Wasserzeichens zu einem Bild
> - Verwendung eines Bilds als Wasserzeichen.
> 
> Dies sind die ASP.net-Programmierfunktionen, die im Artikel eingeführt wurden:
> 
> - Das `WebImage`-Hilfsprogramm.
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

<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Dynamisches Hinzufügen eines Bilds zu einer Webseite

Während Sie die Website entwickeln, können Sie Ihrer Website und einzelnen Seiten Bilder hinzufügen. Sie können Benutzern auch das Hochladen von Bildern gestatten, was für Aufgaben wie das Hinzufügen eines Profil Fotos nützlich sein kann.

Wenn ein Image bereits auf Ihrer Website verfügbar ist und Sie es nur auf einer Seite anzeigen möchten, verwenden Sie ein HTML-`<img>` Element wie folgt:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Manchmal müssen Sie jedoch in der Lage sein, Bilder dynamisch &#8212; anzuzeigen, d. h., Sie wissen nicht, welches Bild angezeigt werden soll, bis die Seite ausgeführt wird.

Das Verfahren in diesem Abschnitt zeigt, wie ein Bild dynamisch angezeigt wird, wenn Benutzer den Bilddateinamen aus einer Liste von Image Namen angeben. Sie wählen den Namen des Bilds aus einer Dropdown Liste aus. Wenn Sie die Seite übermitteln, wird das ausgewählte Bild angezeigt.

![Klang](9-working-with-images/_static/image1.jpg "ch9images-1. jpg")

1. Erstellen Sie in webmatrix eine neue Website.
2. Fügen Sie eine neue Seite mit dem Namen " *dynamicimage. cshtml*" hinzu.
3. Fügen Sie im Stamm Ordner der Website einen neuen Ordner hinzu, und nennen Sie ihn *Images*.
4. Fügen Sie dem soeben erstellten Ordner *Images* vier Bilder hinzu. (Alle Abbilder, die Sie praktisch haben, werden jedoch auf eine Seite passen.) Benennen Sie die Images *Photo1. jpg*, *Photo2. jpg*, *Photo3. jpg*und *Photo4. jpg*um. (In diesem Verfahren verwenden Sie *Photo4. jpg* nicht, aber Sie verwenden es später in diesem Artikel.)
5. Vergewissern Sie sich, dass die vier Bilder nicht als schreibgeschützt gekennzeichnet sind.
6. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Der Text der Seite enthält eine Dropdown Liste (ein `<select>` Element) mit dem Namen `photoChoice`. Die Liste enthält drei Optionen, und das `value`-Attribut jeder Listen Option hat den Namen eines der Images, die Sie im Ordner *Images* abgelegt haben. Im Wesentlichen kann der Benutzer in der Liste einen anzeigen Amen wie &quot;Foto 1&quot;auswählen und übergibt dann den Namen der *JPG* -Datei, wenn die Seite übermittelt wird.

    Im Code können Sie die Auswahl des Benutzers (d. h. den Bilddatei Namen) aus der Liste durchlesen `Request["photoChoice"]`erhalten. Sie sehen zunächst, ob eine Auswahl vorliegt. Wenn dies der Fall ist, erstellen Sie einen Pfad für das Image, das aus dem Namen des Ordners für die Bilder und dem Bilddateinamen des Benutzers besteht. (Wenn Sie versucht haben, einen Pfad zu erstellen, aber keine `Request["photoChoice"]`, erhalten Sie einen Fehler.) Dies führt zu einem relativen Pfad wie diesem:

    *Images/Photo1. jpg*

    Der Pfad wird in der Variablen mit dem Namen `imagePath` gespeichert, die Sie später auf der Seite benötigen.

    Im Text gibt es auch ein `<img>`-Element, das zum Anzeigen des Bilds verwendet wird, das der Benutzer ausgewählt hat. Das `src`-Attribut ist nicht auf einen Dateinamen oder eine URL festgelegt, wie Sie es tun würden, um ein statisches Element anzuzeigen. Stattdessen wird Sie auf "`@imagePath`" festgelegt, was bedeutet, dass Sie Ihren Wert aus dem Pfad erhält, den Sie im Code festlegen.

    Wenn die Seite zum ersten Mal ausgeführt wird, gibt es jedoch kein Bild, das angezeigt werden kann, da der Benutzer nichts ausgewählt hat. Dies bedeutet normalerweise, dass das `src`-Attribut leer ist und das Bild als roter &quot;x&quot; angezeigt wird (oder was der Browser rendert, wenn ein Bild nicht gefunden werden kann). Um dies zu verhindern, platzieren Sie das `<img>`-Element in einem `if` Block, der testet, ob die `imagePath` Variable etwas enthält. Wenn der Benutzer eine Auswahl getroffen hat, `imagePath` den Pfad enthält. Wenn der Benutzer kein Bild ausgewählt hat oder wenn die Seite das erste Mal angezeigt wird, wird das `<img>` Element nicht gerade gerendert.
7. Speichern Sie die Datei, und führen Sie die Seite in einem Browser aus. (Stellen Sie sicher, dass die Seite im Arbeitsbereich " **Dateien** " ausgewählt ist, bevor Sie Sie ausführen.)
8. Wählen Sie ein Bild aus der Dropdown Liste aus, und klicken Sie dann auf **Beispiel Bild**. Stellen Sie sicher, dass Sie unterschiedliche Images für unterschiedliche Optionen sehen.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Hochladen eines Bilds

Im vorherigen Beispiel wurde gezeigt, wie Sie ein Bild dynamisch anzeigen, aber es funktioniert nur mit Bildern, die bereits auf Ihrer Website vorhanden waren. In diesem Verfahren wird gezeigt, wie Sie Benutzern das Hochladen eines Bilds ermöglichen, das dann auf der Seite angezeigt wird. In ASP.net können Sie Bilder im laufenden Betrieb mit dem `WebImage`-Hilfsprogramm manipulieren, das über Methoden zum Erstellen, bearbeiten und Speichern von Bildern verfügt. Das `WebImage`-Hilfsprogramm unterstützt alle gängigen webimage Dateitypen, einschließlich *JPG*, *PNG*und *BMP*. In diesem Artikel verwenden Sie *JPG* -Images, aber Sie können jeden der Bildtypen verwenden.

![Klang](9-working-with-images/_static/image2.jpg "ch9images-2. jpg")

1. Fügen Sie eine neue Seite hinzu, und nennen Sie Sie *UploadImage. cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Der Textkörper des Texts verfügt über ein `<input type="file">`-Element, mit dem Benutzer eine hoch zuladende Datei auswählen können. Wenn Sie auf **senden**klicken, wird die Datei, die Sie ausgewählt hat, zusammen mit dem Formular übermittelt.

    Um das hochgeladene Image zu erhalten, verwenden Sie das `WebImage`-Hilfsprogramm, das über alle nützlichen Methoden zum Arbeiten mit Bildern verfügt. Insbesondere verwenden Sie `WebImage.GetImageFromRequest`, um das hochgeladene Bild (sofern vorhanden) zu erhalten, und speichern es in einer Variablen mit dem Namen `photo`.

    Ein Großteil der Arbeit in diesem Beispiel umfasst das Einrichten und Festlegen von Datei-und Pfadnamen. Das Problem besteht darin, dass Sie den Namen (und nur den Namen) des Bilds, das der Benutzer hochgeladen hat, und dann einen neuen Pfad zum Speichern des Images erstellen möchten. Da Benutzer potenziell mehrere Bilder hochladen können, die denselben Namen aufweisen, verwenden Sie einen gewissen zusätzlichen Code, um eindeutige Namen zu erstellen und sicherzustellen, dass Benutzer vorhandene Bilder nicht überschreiben.

    Wenn ein Bild tatsächlich hochgeladen wurde (der Test `if (photo != null)`), erhalten Sie den Image Namen aus der `FileName`-Eigenschaft des Images. Wenn der Benutzer das Image hochlädt, enthält `FileName` den ursprünglichen Namen des Benutzers, der den Pfad des Benutzer Computers enthält. Dies könnte wie folgt aussehen:

    *C:\users\joe\pictures\samplephoto1.jpg*

    Sie möchten nicht alle Pfadinformationen, obwohl &#8212; Sie nur den eigentlichen Dateinamen (*SamplePhoto1. jpg*) benötigen. Sie können nur die Datei aus einem Pfad entfernen, indem Sie die `Path.GetFileName`-Methode wie folgt verwenden:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Anschließend erstellen Sie einen neuen eindeutigen Dateinamen, indem Sie dem ursprünglichen Namen eine GUID hinzufügen. (Weitere Informationen zu GUIDs finden Sie unter [Informationen zu GUIDs](#SB_AboutGUIDs) weiter unten in diesem Artikel.) Anschließend erstellen Sie einen kompletten Pfad, den Sie zum Speichern des Bilds verwenden können. Der Speicherpfad besteht aus dem neuen Dateinamen, dem Ordner (Images) und dem aktuellen Speicherort der Website.

    > [!NOTE]
    > Damit Ihr Code Dateien im Ordner *Images* speichern kann, benötigt die Anwendung Lese-/Schreibberechtigungen für diesen Ordner. Auf dem Entwicklungs Computer ist dies in der Regel kein Problem. Wenn Sie Ihre Website jedoch auf dem Webserver eines hostinganbieters veröffentlichen, müssen Sie diese Berechtigungen möglicherweise explizit festlegen. Wenn Sie diesen Code auf dem Server eines hostinganbieters ausführen und Fehler erhalten, wenden Sie sich an den Hostinganbieter, um herauszufinden, wie diese Berechtigungen festgelegt werden.

    Zum Schluss übergeben Sie den Speicherpfad an die `Save`-Methode des `WebImage`-Hilfsprogramms. Dadurch wird das hochgeladene Bild unter seinem neuen Namen gespeichert. Die Save-Methode sieht wie folgt aus: `photo.Save(@"~\" + imagePath)`. Der gesamte Pfad wird an `@"~\"`angehängt. Dies ist der aktuelle Speicherort der Website. (Informationen zum `~`-Operator finden Sie unter [Einführung in die ASP.net-Webprogrammierung mit der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Wie im vorherigen Beispiel enthält der Text der Seite ein `<img>`-Element zum Anzeigen des Bilds. Wenn `imagePath` festgelegt wurde, wird das `<img>` Element gerendert, und sein `src`-Attribut wird auf den `imagePath` Wert festgelegt.
3. Führen Sie die Seite in einem Browser aus.
4. Laden Sie ein Bild hoch, und stellen Sie sicher, dass es auf der Seite angezeigt wird.
5. Öffnen Sie an Ihrer Website den Ordner *Images* . Sie sehen, dass eine neue Datei hinzugefügt wurde, deren Dateiname in etwa wie folgt aussieht: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto. png*

    Dies ist das Bild, das Sie mit einer GUID hochgeladen haben, der der Name vorangestellt ist. (Ihre eigene Datei hat eine andere GUID und wird wahrscheinlich anders benannt als *MyPhoto. png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Informationen zu GUIDs
> 
> Eine GUID (Global-Unique ID) ist ein Bezeichner, der normalerweise in einem Format wie dem folgenden gerendert wird: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Die Zahlen und Buchstaben (von A-F) unterscheiden sich für jede GUID, aber Sie folgen dem Muster der Verwendung von Gruppen mit 8-4-4-4-12 Zeichen. (Technisch gesehen handelt es sich bei einer GUID um eine 16-Byte-/128-Bit-Zahl.) Wenn Sie eine GUID benötigen, können Sie spezialisierten Code aufzurufen, der eine GUID für Sie generiert. Die Idee hinter GUIDs besteht darin, dass die sich ergebende Zahl zwischen der großen Größe der Zahl (3,4 x 10<sup>38</sup>) und dem Algorithmus für die Erstellung der Zahl praktisch garantiert eine der Typen ist. GUIDs sind daher eine gute Möglichkeit, Namen für Dinge zu generieren, wenn Sie garantieren müssen, dass Sie nicht denselben Namen zweimal verwenden. Der Nachteil ist natürlich, dass GUIDs nicht besonders benutzerfreundlich sind. Sie werden daher tendenziell verwendet, wenn der Name nur im Code verwendet wird.

<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Größenänderungen bei Bildern

Wenn Ihre Website Images von einem Benutzer akzeptiert, können Sie die Größe der Images ändern, bevor Sie Sie anzeigen oder speichern. Hierfür können Sie das `WebImage`-Hilfsprogramm wieder verwenden.

Diese Prozedur zeigt, wie die Größe eines hochgeladenen Bilds geändert wird, um eine Miniaturansicht zu erstellen und dann die Miniaturansicht und das ursprüngliche Bild auf der Website zu speichern Sie zeigen die Miniaturansicht auf der Seite an und verwenden einen Hyperlink, um Benutzer an das Bild in voller Größe umzuleiten.

![Klang](9-working-with-images/_static/image3.jpg "ch9images-3. jpg")

1. Fügen Sie eine neue Seite mit dem Namen " *Miniatur. cshtml*" hinzu.
2. Erstellen Sie im Ordner *Images* einen Unterordner mit dem Namen *Thumbs*.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Dieser Code ähnelt dem Code aus dem vorherigen Beispiel. Der Unterschied besteht darin, dass dieser Code das Bild zweimal speichert, einmal normal und einmal, nachdem Sie eine Miniaturansicht des Bilds erstellt haben. Zuerst erhalten Sie das hochgeladene Image und speichern es im Ordner *Images* . Anschließend erstellen Sie einen neuen Pfad für das Miniaturbild. Um die Miniaturansicht tatsächlich zu erstellen, rufen Sie die `Resize`-Methode des `WebImage`-Hilfsprogramms auf, um ein 60-Pixel-Bild mit 60 Pixel zu erstellen. Das Beispiel zeigt, wie Sie das Seitenverhältnis beibehalten und verhindern können, dass das Bild vergrößert wird (für den Fall, dass die neue Größe das Bild tatsächlich vergrößert). Das Image, das in der Größe geändert wird, wird dann im Unterordner *Thumbs* gespeichert.

    Am Ende des Markups verwenden Sie das gleiche `<img>`-Element mit dem dynamischen `src` Attribut, das Sie in den vorherigen Beispielen gesehen haben, um das Bild bedingt anzuzeigen. In diesem Fall zeigen Sie die Miniaturansicht an. Außerdem verwenden Sie ein `<a>`-Element, um einen Hyperlink zur großen Version des Bilds zu erstellen. Wie beim `src`-Attribut des `<img>`-Elements legen Sie das `href`-Attribut des `<a>`-Elements dynamisch auf den Wert in `imagePath`fest. Um sicherzustellen, dass der Pfad als URL funktionieren kann, übergeben Sie `imagePath` an die `Html.AttributeEncode`-Methode, die reservierte Zeichen im Pfad in Zeichen konvertiert, die in einer URL OK sind.
4. Führen Sie die Seite in einem Browser aus.
5. Hochladen eines Fotos und überprüfen, ob die Miniaturansicht angezeigt wird.
6. Klicken Sie auf die Miniaturansicht, um das Bild in voller Größe anzuzeigen.
7. Beachten Sie, dass in den *Bildern* und *Bildern/Daumen*neue Dateien hinzugefügt wurden.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Drehen und Kippen eines Bilds

Mit dem `WebImage`-Hilfsprogramm können Sie auch Bilder kippen und drehen. Diese Prozedur zeigt, wie Sie ein Bild vom Server erhalten, das Bild nach oben (vertikal) kippen, speichern und dann auf der Seite das gedrehte Bild anzeigen. In diesem Beispiel verwenden Sie nur eine Datei, die bereits auf dem Server vorhanden ist (*Photo2. jpg*). In einer echten Anwendung würden Sie wahrscheinlich ein Bild kippen, dessen Name Sie dynamisch erhalten, wie in den vorherigen Beispielen gezeigt.

![Klang](9-working-with-images/_static/image4.jpg "ch9images-4. jpg")

1. Fügen Sie eine neue Seite mit dem Namen *flipimage. cshtml*hinzu.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Im Code wird das `WebImage`-Hilfsprogramm verwendet, um ein Bild vom Server zu erhalten. Sie erstellen den Pfad zum Image mit dem Verfahren, das Sie in früheren Beispielen zum Speichern von Bildern verwendet haben, und übergeben diesen Pfad, wenn Sie ein Image mit `WebImage`erstellen:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Wenn ein Bild gefunden wird, erstellen Sie einen neuen Pfad und Dateinamen, wie in den vorherigen Beispielen gezeigt. Um das Bild zu kippen, wird die `FlipVertical`-Methode aufgerufen, und anschließend speichern Sie das Bild erneut.

    Das Bild wird erneut auf der Seite angezeigt, indem das `<img>`-Element verwendet wird, wobei das `src`-Attribut auf `imagePath`festgelegt ist.
3. Führen Sie die Seite in einem Browser aus. Das Bild für *Photo2. jpg* wird oben angezeigt.
4. Aktualisieren Sie die Seite, oder fordern Sie die Seite erneut an, um anzuzeigen, dass das Bild von rechts nach oben gekippt wird.

Zum Drehen eines Bilds verwenden Sie denselben Code, mit dem Unterschied, dass Sie anstelle des `FlipVertical` oder `FlipHorizontal``RotateLeft` oder `RotateRight`aufrufen.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Hinzufügen eines Wasserzeichens zu einem Bild

Wenn Sie Ihrer Website Bilder hinzufügen, empfiehlt es sich, dem Bild ein Wasserzeichen hinzuzufügen, bevor Sie es speichern oder auf einer Seite anzeigen können. Benutzer verwenden häufig Wasserzeichen, um einem Image Copyright Informationen hinzuzufügen oder um Ihren Geschäftsnamen ankündigen zu können.

![Klang](9-working-with-images/_static/image5.jpg "ch9images-5. jpg")

1. Fügen Sie eine neue Seite mit dem Namen " *Watermark. cshtml*" hinzu.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Dieser Code ähnelt dem Code in der *flipimage. cshtml* -Seite von früher (obwohl dieses Mal die Datei *Photo3. jpg* verwendet). Um das Wasserzeichen hinzuzufügen, müssen Sie die `AddTextWatermark`-Methode des `WebImage`-Hilfsprogramms vor dem Speichern des Bilds aufzurufen. Im `AddTextWatermark`-Befehl übergeben Sie den Text &quot;meinem Wasserzeichen&quot;, legen die Schriftart Farbe auf gelb fest und legen die Schriftfamilie auf Arial fest. (Obwohl es hier nicht angezeigt wird, können Sie mit dem `WebImage`-Hilfsprogramm auch Deckkraft, Schriftfamilie und Schrift Grad sowie die Position des Wasserzeichen Texts angeben.) Wenn Sie das Abbild speichern, darf es nicht schreibgeschützt sein.

    Wie Sie bereits gesehen haben, wird das Bild auf der Seite mit dem `<img>`-Element angezeigt, wobei das src-Attribut auf `@imagePath`festgelegt ist.
3. Führen Sie die Seite in einem Browser aus. Beachten Sie den Text "My Watermark" in der unteren rechten Ecke des Bilds.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Verwenden eines Bilds als Wasserzeichen

Anstatt Text für ein Wasserzeichen zu verwenden, können Sie ein anderes Bild verwenden. Manchmal werden Bilder wie ein Firmenlogo als Wasserzeichen verwendet, oder es wird ein Wasserzeichen Bild anstelle von Text für Copyright Informationen verwendet.

![Klang](9-working-with-images/_static/image6.jpg "ch9images-6. jpg")

1. Fügen Sie eine neue Seite mit dem Namen " *imagewatermark. cshtml*" hinzu.
2. Fügen Sie dem Ordner *Images* ein Bild hinzu, das Sie als Logo verwenden können, und benennen Sie das Image *mycompanylogo. jpg*um. Dieses Bild sollte ein Bild sein, das Sie eindeutig sehen können, wenn es auf 80 Pixel breit und 20 Pixel hoch festgelegt ist.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Dies ist eine weitere Variation des Codes aus früheren Beispielen. In diesem Fall wird `AddImageWatermark` aufgerufen, um dem Zielbild (*Photo3. jpg*) das Wasserzeichen Bild hinzuzufügen, bevor Sie das Bild speichern. Wenn Sie `AddImageWatermark`aufzurufen, legen Sie die Breite auf 80 Pixel und die Höhe auf 20 Pixel fest. Das Image " *mycompanylogo. jpg* " wird horizontal in der Mitte ausgerichtet und vertikal am unteren Rand des zielbilds ausgerichtet. Die Deckkraft ist auf 100% festgelegt, und der Abstand wird auf 10 Pixel festgelegt. Wenn das Wasserzeichen Bild größer als das Zielbild ist, wird nichts passieren. Wenn das Wasserzeichen Bild größer als das Zielbild ist und Sie den Leerraum für das Bild Wasserzeichen auf NULL festlegen, wird das Wasserzeichen ignoriert.

    Wie zuvor zeigen Sie das Bild mithilfe des `<img>`-Elements und eines dynamischen `src`-Attributs an.
4. Führen Sie die Seite in einem Browser aus. Beachten Sie, dass das Wasserzeichen Bild unten im Hauptbild angezeigt wird.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Arbeiten mit Dateien an einer ASP.net Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202896)

[Einführung in ASP.net Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587)
