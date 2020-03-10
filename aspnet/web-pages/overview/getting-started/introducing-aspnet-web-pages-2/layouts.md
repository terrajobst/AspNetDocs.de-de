---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Einführung in ASP.net Web Pages: Erstellen eines konsistenten Layouts | Microsoft-Dokumentation'
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie mithilfe von Layouts ein konsistentes Erscheinungsbild der Seiten auf einer Website erstellen, die ASP.net Web Pages verwendet. Es wird davon ausgegangen, dass Sie die...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422745"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Einführung in ASP.net Web Pages: Erstellen eines konsistenten Layouts

von [Tom fitzmacken](https://github.com/tfitzmac)

> In diesem Tutorial wird gezeigt, wie Sie mithilfe von *Layouts* ein konsistentes Erscheinungsbild der Seiten auf einer Website erstellen, die ASP.net Web Pages verwendet. Dabei wird davon ausgegangen, dass Sie die Reihe durch Löschen von Daten [Bank Daten in ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584)abgeschlossen haben.
> 
> Sie lernen Folgendes:
> 
> - Was eine Layoutseite ist.
> - Gewusst wie: Kombinieren von Layoutseiten mit dynamischem Inhalt.
> - Übergeben von Werten an eine Layoutseite.

## <a name="about-layouts"></a>Informationen zu Layouts

Die bisher erstellten Seiten sind vollständig, eigenständige Seiten. Alle gehören zu derselben Website, haben aber keine gemeinsamen Elemente oder Standard aussehen.

Die meisten Websites verfügen über ein konsistentes Aussehen und Layout. Wenn Sie z. b. die [Microsoft.com/Web](https://www.microsoft.com/web/) -Website aufrufen und sehen, sehen Sie, dass die Seiten alle einem allgemeinen Layout und einem visuellen Design entsprechen:

![Microsoft.com/Web Website Seite mit dem Layout des Headers, Navigationsbereichs, Inhalts Bereich und Fußzeile](layouts/_static/image1.png)

Eine *ineffiziente* Möglichkeit, dieses Layout zu erstellen, besteht darin, einen Header, eine Navigationsleiste und eine Fußzeile separat auf jeder Seite zu definieren. Sie würden jedes Mal dasselbe Markup duplizieren. Wenn Sie etwas ändern möchten (z. b. die Fußzeile aktualisieren), müssten Sie jede Seite separat ändern.

An dieser Stelle kommen *Layoutseiten* ins Spiel. In ASP.net Web Pages können Sie eine Layoutseite definieren, die einen gesamten Container für Seiten auf Ihrer Website bereitstellt. Die Layoutseite kann z. b. die Kopfzeile, den Navigationsbereich und die Fußzeile enthalten. Die Layoutseite enthält einen Platzhalter, bei dem der Hauptinhalt angezeigt wird.

Anschließend können Sie einzelne Inhaltsseiten definieren, die das Markup und den Code nur für diese Seite enthalten. Inhaltsseiten müssen keine kompletten HTML-Seiten sein. Sie müssen nicht einmal über ein `<body>` Element verfügen. Außerdem verfügen Sie über eine Codezeile, die angibt, ASP.net auf welcher Layoutseite der Inhalt angezeigt werden soll. Im folgenden finden Sie ein Bild, das ungefähr die Funktionsweise dieser Beziehung zeigt:

![Konzeptionelles Diagramm, das zwei Inhaltsseiten und eine Layoutseite anzeigt, an die Sie passen](layouts/_static/image2.png)

Diese Interaktion ist leicht zu verstehen, wenn Sie in Aktion angezeigt wird. In diesem Tutorial ändern Sie die Seiten Ihrer Filme, sodass Sie ein Layout verwenden.

## <a name="adding-a-layout-page"></a>Hinzufügen einer Layoutseite

Zunächst erstellen Sie eine Layoutseite, die ein typisches Seitenlayout mit einer Kopfzeile, einer Fußzeile und einem Bereich für den Hauptinhalt definiert. Fügen Sie auf der webblogesmovies-Website eine cshtml-Seite mit dem Namen *\_"Layout. cshtml*" hinzu.

Der führende Unterstrich (`_`) ist wichtig. Wenn der Name einer Seite mit einem Unterstrich beginnt, sendet ASP.net diese Seite nicht direkt an den Browser. Mit dieser Konvention können Sie Seiten definieren, die für Ihre Website erforderlich sind, aber die Benutzer nicht in der Lage sein sollten, direkt anzufordern.

Ersetzen Sie den Inhalt der Seite durch Folgendes:

[!code-html[Main](layouts/samples/sample1.html)]

Wie Sie sehen können, ist dieses Markup nur HTML, das `<div>` Elemente verwendet, um drei Abschnitte auf der Seite zu definieren, sowie ein `<div>` Element, das die drei Abschnitte enthalten soll. Der Fußzeile enthält ein wenig Razor-Code: `@DateTime.Now.Year`, der das aktuelle Jahr an dieser Stelle auf der Seite Rendering.

Beachten Sie, dass es einen Link zu einem Stylesheet namens " *Movies. CSS*" gibt. Das Stylesheet ist der Ort, an dem die Details des physischen Layouts der Elemente definiert werden. Das Erstellen Sie in einem Moment.

Das einzige ungewöhnliche Feature in dieser *\_"Layout. cshtml* "-Seite ist die `@Render.Body()` Zeile. Dies ist der Platzhalter, bei dem der Inhalt angezeigt wird, wenn dieses Layout mit einer anderen Seite zusammengeführt wird.

## <a name="adding-a-css-file"></a>Hinzufügen einer CSS-Datei

Die bevorzugte Methode zum Definieren der tatsächlichen Anordnung (d. h. Darstellung) von Elementen auf der Seite ist die Verwendung von Cascading Stylesheet (CSS)-Regeln. Daher erstellen Sie eine *CSS* -Datei, die die Regeln für das neue Layout enthält.

Wählen Sie in webmatrix den Stamm der Website aus. Klicken Sie dann auf der Registerkarte **Dateien** des Menübands auf den Pfeil unter der Schaltfläche **neu** , und klicken Sie dann auf **neuer Ordner**.

![Die Option "neuer Ordner" unter "neu" im Menüband.](layouts/_static/image3.png)

Benennen Sie die neuen Ordner *Stile*.

![Benennen des neuen Ordners "Stile"](layouts/_static/image4.png)

Erstellen Sie im Ordner "neue *Stile* " eine Datei mit dem Namen " *Movies. CSS*".

![Erstellen einer neuen Movies. CSS-Datei](layouts/_static/image5.png)

Ersetzen Sie den Inhalt der neuen *CSS* -Datei durch Folgendes:

[!code-css[Main](layouts/samples/sample2.css)]

Wir werden nicht viel über diese CSS-Regeln sprechen, außer um zwei Dinge zu beachten. Eine besteht darin, dass die Regeln zusätzlich zum Festlegen von Schriftarten und Größen die absolute Positionierung verwenden, um die Position des Headers, der Fußzeile und des Hauptinhalts Bereichs festzulegen. Wenn Sie noch nicht mit der Positionierung in CSS vertraut sind, können Sie das Tutorial zur [CSS-Positionierung](http://www.w3schools.com/css/css_positioning.asp) auf der W3Schools-Website lesen.

Beachten Sie auch, dass die Stilregeln, die ursprünglich in der Datei *Movies. cshtml* definiert wurden, im unteren Bereich kopiert wurden. Diese Regeln wurden in der Einführung in die [Anzeige von Daten mit ASP.net Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) Tutorial verwendet, um das `WebGrid`-Hilfsprogramm zum rendermarkup zu machen, das der Tabelle Streifen hinzugefügt hat. (Wenn Sie eine *CSS* -Datei für Stildefinitionen verwenden möchten, können Sie auch die Stilregeln für die gesamte Website darin platzieren.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Aktualisieren der Filme Datei zur Verwendung des Layouts

Nun können Sie die vorhandenen Dateien an Ihrer Site aktualisieren, um das neue Layout zu verwenden. Öffnen Sie die Datei *Movies. cshtml* . Fügen Sie oben als erste Codezeile Folgendes hinzu:

[!code-csharp[Main](layouts/samples/sample3.cs)]

Die Seite wird jetzt wie folgt gestartet:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Diese eine Codezeile teilt ASP.net mit, dass die Seite mit der Datei " *\_Layout. cshtml* " zusammengeführt werden soll, wenn die Seite " *Filme* " ausgeführt wird.

Da die Datei *Movies. cshtml* nun eine Layoutseite verwendet, können Sie das Markup von der Seite *Movies. cshtml* entfernen, die durch die Datei *\_Layout. cshtml* übernommen wird. Entfernen Sie die `<!DOCTYPE>`, `<html>`und `<body>` öffnenden und schließenden Tags. Nehmen Sie das gesamte `<head>` Element und seinen Inhalt, das die Stilregeln für das Raster enthält, da Sie diese Regeln jetzt in einer *CSS* -Datei haben. Wenn Sie sich dabei befinden, ändern Sie das vorhandene `<h1>` Element in ein `<h2>` Element. Sie verfügen bereits über ein `<h1>` Element auf der Layoutseite. Ändern Sie den `<h2>` Text in "list movies" (Filme auflisten).

Normalerweise müssten Sie diese Arten von Änderungen nicht auf einer Inhaltsseite vornehmen. Wenn Sie Ihre Website mit einer Layoutseite auslagern, erstellen Sie Inhaltsseiten ohne alle diese Elemente, die mit beginnen. In diesem Fall wandeln Sie jedoch eine eigenständige Seite in eine ein, die ein Layout verwendet, sodass es ein wenig Bereinigungs Vorgang gibt.

Wenn Sie fertig sind, sieht die Seite *Movies. cshtml* wie folgt aus:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testen des Layouts

Nun können Sie sehen, wie das Layout aussieht. Klicken Sie in webmatrix mit der rechten Maustaste auf die Seite *Movies. cshtml* , und wählen Sie **in Browser starten aus**. Wenn der Browser die Seite anzeigt, sieht er wie folgt aus:

![Filme Seite mit einem Layout gerendert](layouts/_static/image6.png)

ASP.net hat den Inhalt der Seite Movies. cshtml auf der Seite *\_Layout. cshtml* zusammengeführt, auf der sich die `RenderBody`-Methode befindet. Und natürlich verweist die *\_"Layout. cshtml* "-Seite auf eine *CSS* -Datei, die das Aussehen der Seite definiert.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aktualisieren der addmovie-Seite zur Verwendung des Layouts

Der eigentliche Vorteil von Layouts ist, dass Sie Sie für alle Seiten auf Ihrer Website verwenden können. Öffnen Sie die Seite " *addmovie. cshtml* ".

Beachten Sie, dass die *addmovie. cshtml* -Seite ursprünglich einige CSS-Regeln enthielt, um das Aussehen von Validierungs Fehlermeldungen zu definieren. Da Sie jetzt über eine *CSS* -Datei für Ihre Website verfügen, können Sie diese Regeln in die *CSS* -Datei verschieben. Entfernen Sie diese aus der Datei " *addmovie. cshtml* ", und fügen Sie Sie am Ende der Datei " *Movies. CSS* " hinzu. Sie verschieben die folgenden Regeln:

[!code-css[Main](layouts/samples/sample6.css)]

Nehmen Sie nun dieselben Änderungen in " *addmovie. cshtml* " vor, die Sie für *Filme. cshtml* – hinzu `Layout="~/_Layout.cshtml;` gefügt haben, und entfernen Sie das HTML-Markup, das nun nicht mehr vorhanden ist. Ändern Sie das `<h1>`-Element in `<h2>`. Wenn Sie fertig sind, sieht die Seite wie im folgenden Beispiel aus:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Führen Sie die Seite aus. Nun sieht es wie in der folgenden Abbildung aus:

![Seite "Filme hinzufügen" mithilfe eines Layouts gerendert](layouts/_static/image7.png)

Sie möchten ähnliche Änderungen an den Seiten der Website vornehmen – *editmovie. cshtml* und *deletemovie. cshtml*. Bevor Sie jedoch Vorgehen, können Sie eine weitere Änderung am Layout vornehmen, wodurch es etwas flexibler wird.

## <a name="passing-title-information-to-the-layout-page"></a>Übergeben von Titelinformationen an die Layoutseite

Die *\_Seite Layout. cshtml* , die Sie erstellt haben, verfügt über ein `<title>` Element, das auf "My Movie Site" festgelegt ist. In den meisten Browsern wird der Inhalt dieses Elements als Text auf einer Registerkarte angezeigt:

![Der &lt;Titel der Seite&gt; Element, das auf einer Browser Registerkarte angezeigt wird.](layouts/_static/image8.png)

Diese Titelinformationen sind generisch. Angenommen, Sie möchten, dass der Titeltext für die aktuelle Seite genauer ist. (Der Titeltext wird auch von Suchmaschinen verwendet, um zu bestimmen, worum es sich bei Ihrer Seite handelt.) Sie können Informationen von einer Inhaltsseite wie *Movies. cshtml* oder *addmovie. cshtml* an die Layoutseite übergeben und diese Informationen dann verwenden, um anzupassen, was die Layoutseite rendert.

Öffnen Sie die Seite *Movies. cshtml* erneut. Fügen Sie im Code oben die folgende Zeile hinzu:

[!code-csharp[Main](layouts/samples/sample8.cs)]

Das `Page`-Objekt ist auf allen *. cshtml* -Seiten verfügbar und dient zu diesem Zweck zum Freigeben von Informationen zwischen einer Seite und dem Layout.

Öffnen Sie die Seite *\_Layout. cshtml* . Ändern Sie das `<title>`-Element, sodass es wie dieses Markup aussieht:

[!code-html[Main](layouts/samples/sample9.html)]

Dieser Code rendert, was sich in der `Page.Title`-Eigenschaft direkt an dieser Stelle auf der Seite befindet.

Führen Sie die Seite *Movies. cshtml* aus. Dieses Mal zeigt die Registerkarte Browser an, was Sie als Wert von `Page.Title`:

![Eine Browser Registerkarte, die den dynamisch erstellten Titel anzeigt](layouts/_static/image9.png)

Wenn Sie möchten, können Sie die Seitenquelle im Browser anzeigen. Sie können sehen, dass das `<title>`-Element als `<title>List Movies</title>`gerendert wird.

> [!TIP] 
> 
> **Das Page-Objekt**
> 
> Ein nützliches Feature von `Page` ist, dass es sich um ein dynamisches Objekt handelt – die Eigenschaft `Title` ist kein fester oder reservierter Name. Sie können einen *beliebigen* Namen für einen Wert des `Page` Objekts verwenden. So können Sie z. b. den Titel einfach mithilfe einer Eigenschaft namens `Page.CurrentName` oder `Page.MyPage`übertragen. Die einzige Einschränkung besteht darin, dass der Name den normalen Regeln für die benannten Eigenschaften entsprechen muss. (Der Name darf z. b. kein Leerzeichen enthalten.)
> 
> Sie können eine beliebige Anzahl von Werten übergeben, indem Sie das `Page`-Objekt verwenden. Wenn Sie Movie-Informationen an die Layoutseite übergeben möchten, können Sie Werte wie `Page.MovieTitle` und `Page.Genre` und `Page.MovieYear`übergeben. (Oder andere Namen, die Sie zum Speichern der Informationen erfunden haben.) Die einzige Voraussetzung – die wahrscheinlich offensichtlich ist – besteht darin, dass Sie die gleichen Namen auf der Inhaltsseite und der Layoutseite verwenden müssen.
> 
> Die Informationen, die Sie mithilfe des `Page` Objekts übergeben, sind nicht auf Text beschränkt, der auf der Layoutseite angezeigt werden soll. Sie können einen Wert an die Layoutseite übergeben. Anschließend kann der Code auf der Layoutseite den Wert verwenden, um zu entscheiden, ob ein Abschnitt der Seite angezeigt werden soll, welche *CSS* -Datei verwendet werden soll usw. Die Werte, die Sie im `Page` Objekt übergeben, entsprechen allen anderen Werten, die Sie im Code verwenden. Die Werte stammen nur aus der Inhaltsseite und werden an die Layoutseite übermittelt.

Öffnen Sie die Seite " *addmovie. cshtml* ", und fügen Sie am Anfang des Codes eine Zeile hinzu, die einen Titel für die Seite " *addmovie. cshtml* " enthält:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Führen Sie die Seite *addmovie. cshtml* aus. Hier sehen Sie den neuen Titel:

![Eine Browser Registerkarte mit dem Titel "Hinzufügen von Filmen", der dynamisch erstellt wurde](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aktualisieren der verbleibenden Seiten zur Verwendung des Layouts

Nun können Sie die verbleibenden Seiten auf Ihrer Website so beenden, dass Sie das neue Layout verwenden. Öffnen Sie die Datei " *editmovie. cshtml* " und " *deletemovie. cshtml* ", und nehmen Sie die gleichen Änderungen in den einzelnen Änderungen vor.

Fügen Sie die Codezeile hinzu, die mit der Layoutseite verknüpft ist:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Fügen Sie eine Zeile hinzu, um den Titel der Seite festzulegen:

[!code-csharp[Main](layouts/samples/sample12.cs)]

oder:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Entfernen Sie alle überflüssigen HTML-Markup – im Grunde lassen Sie nur die Bits, die sich innerhalb des `<body>` Elements befinden (plus den Codeblock oben).

Ändern Sie das `<h1>`-Element, sodass es ein `<h2>` Element ist.

Wenn Sie diese Änderungen vorgenommen haben, testen Sie jeden, und stellen Sie sicher, dass er ordnungsgemäß angezeigt wird und dass der Titel korrekt ist.

## <a name="parting-thoughts-about-layout-pages"></a>Teilen von Gedanken über Layoutseiten

In diesem Tutorial haben Sie eine *\_"Layout. cshtml* "-Seite erstellt und die `RenderBody`-Methode verwendet, um Inhalte von einer anderen Seite zusammenzuführen. Das ist das grundlegende Muster für die Verwendung von Layouts in Webseiten.

Layoutseiten verfügen über zusätzliche Funktionen, die hier nicht behandelt wurden. Beispielsweise können Sie Layoutseiten verschachteln – eine Layoutseite kann wiederum auf eine andere verweisen. Die Verwendung von Netz Layouts kann nützlich sein, wenn Sie mit Unterabschnitten einer Website arbeiten, für die unterschiedliche Layouts erforderlich sind. Sie können auch zusätzliche Methoden (z. b. `RenderSection`) verwenden, um benannte Abschnitte auf der Layoutseite einzurichten.

Die Kombination von Layoutseiten und *CSS* -Dateien ist leistungsstark. Wie Sie in der nächsten tutorialreihe sehen werden, können Sie in webmatrix eine Website auf der Grundlage einer *Vorlage*erstellen, die Ihnen eine Website mit vorgefertigten Funktionen bietet. Die Vorlagen nutzen Layoutseiten und CSS zum Erstellen von Websites, die hervorragend Aussehen und Features wie Menüs aufweisen. Hier sehen Sie einen Screenshot der Startseite einer Website, die auf einer Vorlage basiert und Features anzeigt, die Layoutseiten und CSS verwenden:

![Layout, das von der webmatrix-Website Vorlage erstellt wurde, zeigt den Header, Navigationsbereich, Inhalts Bereich, optionalen Abschnitt und Anmelde Verknüpfungen an](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Vervollständigen der Auflistung für Movie Page (aktualisiert zur Verwendung einer Layoutseite)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Complete Page Listing for Add Movie Page (für Layout aktualisiert)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Complete Page Listing for DELETE Movie Page (für Layout aktualisiert)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Complete Page Listing for Movie Page Page (für Layout aktualisiert)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Nächste nächste

Im nächsten Tutorial erfahren Sie, wie Sie Ihre Website im Internet veröffentlichen, damit Sie von allen Benutzern angezeigt wird.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erstellen eines konsistenten Erscheinungsbild](https://go.microsoft.com/fwlink/?LinkID=202891) – ein Artikel, der weitere Details zum Arbeiten mit Layouts bereitstellt. Außerdem wird beschrieben, wie Sie einen Wert an eine Layoutseite übergeben, die einen Teil des Inhalts anzeigt oder ausblendet.
- Geschachtelte [Layoutseiten mit Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) – Mike Brind Blogs ein Beispiel dafür, wie Layoutseiten geschachtelt werden. (Enthält einen Download der Seiten.)

> [!div class="step-by-step"]
> [Zurück](deleting-data.md)
> [Weiter](publishing.md)
