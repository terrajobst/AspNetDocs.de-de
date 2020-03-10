---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Verwenden des Hilfsprogramms "Dropdown List" mit ASP.NET MVC | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6375bb2be158cea18309ffa71c71ac3e67bc91ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433077"
---
# <a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Verwenden des DropDownList-Hilfsprogramms mit ASP.NET MVC

von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit dem [Dropdown List](https://msdn.microsoft.com/library/dd492948.aspx) -Hilfsprogramm und dem [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) -Hilfsprogramm in einer ASP.NET MVC-Webanwendung. Sie können Microsoft Visual Web Developer 2010 Express Service Pack 1 verwenden, bei dem es sich um eine kostenlose Version von Microsoft Visual Studio handelt, um das Tutorial zu befolgen. Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen. Sie können alle Komponenten installieren, indem Sie auf den folgenden Link klicken: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten einzeln mithilfe der folgenden Links installieren:

- Erforderliche Komponenten für [Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [ASP.NET MVC 3-Tools aktualisieren](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Laufzeit + Tool Unterstützung)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die erforderlichen Komponenten, indem Sie auf den folgenden Link klicken: [Visual Studio 2010 Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). In diesem Tutorial wird davon ausgegangen, dass Sie das Tutorial "Einführung [in ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder das[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) -Tutorial abgeschlossen haben oder mit der ASP.NET MVC-Entwicklung vertraut sind. Dieses Tutorial beginnt mit einem geänderten Projekt aus dem [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) -Tutorial. Sie können das Startprojekt mit dem folgenden Link herunterladen, um [die C# Version herunterzuladen](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Ein Visual Web Developer-Projekt mit dem abgeschlossenen C# Tutorial-Quellcode ist für dieses Thema verfügbar. [Herunterladen](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Sie lernen Folgendes

Sie erstellen Aktionsmethoden und-Sichten, die mithilfe der [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) -Hilfe eine Kategorie auswählen. Außerdem verwenden Sie **jQuery** , um das Dialogfeld INSERT Category hinzuzufügen, das verwendet werden kann, wenn eine neue Kategorie (z. b. Genre oder Künstlerin) benötigt wird. Im folgenden finden Sie einen Screenshot der Ansicht erstellen, in der Links zum Hinzufügen eines neuen Genres und zum Hinzufügen eines neuen Künstlers angezeigt werden.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Erlernte Fertigkeiten

Folgendes können Sie lernen:

- Verwenden des Hilfsprogramms [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) zum Auswählen von Kategoriedaten.
- Hinzufügen eines **jQuery** -Dialog Felds zum Hinzufügen neuer Kategorien.

### <a name="getting-started"></a>Erste Schritte

Starten Sie, indem Sie das Starter-Projekt mit dem folgenden Link herunter [laden: herunterladen](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). Klicken Sie in Windows-Explorer mit der rechten Maustaste auf die Datei *DDL\_Starter. zip* , und wählen Sie Eigenschaften aus. Wählen Sie im Dialogfeld **Eigenschaften von DDL\_Starter. zip** die Option Sperre Entsperren aus.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Klicken Sie mit der rechten Maustaste auf die Datei DDL\_Starter. zip, und wählen Sie **Alle extrahieren** , um die Datei zu entpacken Öffnen Sie die Datei " *startmusicstore. sln* " mit Visual Web Developer 2010 Express ("Visual Web Developer" oder "VWD" für kurze Zeit) oder Visual Studio 2010.

Drücken Sie STRG + F5, um die Anwendung auszuführen, und klicken Sie auf den Link **Testen** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Wählen Sie den Link **Filmkategorie (einfach) auswählen** aus. Eine Movie Type SELECT-Liste wird angezeigt, und es wird eine Komödie mit dem ausgewählten Wert angezeigt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Klicken Sie mit der rechten Maustaste in den Browser, und wählen Sie Quelle anzeigen Der HTML-Code für die Seite wird angezeigt. Der folgende Code zeigt den HTML-Code für das SELECT-Element.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Sie sehen, dass jedes Element in der Auswahlliste einen Wert hat (0 für Action, 1 für Drama, 2 für Comedy und 3 für Science-Fiction) und einen anzeigen Amen (Action, Drama, Comedy und Science-Fiction). Der obige Code ist Standard-HTML für eine SELECT-Liste.

Ändern Sie die Auswahlliste in Drama, und klicken Sie auf die Schaltfläche **senden** . Die URL im Browser ist `http://localhost:2468/Home/CategoryChosen?MovieType=1`, und die Seite zeigt an, dass **Sie Folgendes ausgewählt haben: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Öffnen Sie die Datei " *controllers\homecontroller.cs* ", und untersuchen Sie die `SelectCategory`-Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

Das [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) -Hilfsprogramm, das zum Erstellen einer HTML-Auswahlliste verwendet wird, erfordert ein **IEnumerable-&lt;SelectListItem-&gt;** , entweder explizit oder implizit. Das heißt, Sie können die **IEnumerable-&lt;SelectListItem-&gt;** explizit an [das Dropdown List](https://msdn.microsoft.com/library/dd492738.aspx) -Hilfsprogramm übergeben, oder Sie können die **IEnumerable&lt;SelectListItem-&gt;** mit dem gleichen Namen für das **SelectListItem** -Element [in die Model](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) -Eigenschaft einfügen. Das implizite und explizite übergeben von **SelectListItem** wird im nächsten Teil des Tutorials behandelt. Der obige Code zeigt die einfachste Möglichkeit, ein **IEnumerable-&lt;SelectListItem-&gt;** zu erstellen und mit Text und Werten zu füllen. Beachten Sie, dass für den `Comedy`[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) die [ausgewählte](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) Eigenschaft auf **true** festgelegt ist **. Dadurch wird** die gerenderte SELECT-Liste als ausgewähltes Element in der Liste als ausgewähltes Element angezeigt.

Die oben erstellte **IEnumerable-&lt;SelectListItem-&gt;** wird der [viewbag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) mit dem Namen "movietype" hinzugefügt. Auf diese Weise übergeben wir die **IEnumerable-&lt;SelectListItem-&gt;** implizit an das unten gezeigte [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) -Hilfsprogramm.

Öffnen Sie die Datei " *views\home\selectcategory.cshtml* ", und untersuchen Sie das Markup.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

In der dritten Zeile haben wir das Layout auf views/Shared/\_Simple\_Layout. cshtml festgelegt. Dies ist eine vereinfachte Version der standardlayoutdatei. Dies geschieht, um die Anzeige und den gerenderten HTML-Code beizubehalten.

In diesem Beispiel wird der Status der Anwendung nicht geändert, weshalb wir die Daten über **HTTP Get**, nicht über **http Post**senden. Informationen [zum Auswählen von HTTP GET oder Post finden Sie](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist)im W3C-Abschnitt Quick Checkliste. Da wir die Anwendung nicht ändern und das Formular veröffentlichen, verwenden wir die [HTML. BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) -Überladung, die es uns ermöglicht, die Aktionsmethode, den Controller und die Formular Methode (**http Post** oder **HTTP Get**) anzugeben. In der Regel enthalten Sichten die [HTML. BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) -Überladung, die keine Parameter annimmt. Die Version No Parameter hat standardmäßig die Bereitstellung der Formulardaten an die Post-Version der gleichen Aktionsmethode und des gleichen Controllers.

Die folgende Zeile

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

übergibt ein Zeichen folgen Argument an das **Dropdown List** -Hilfsprogramm. Diese Zeichenfolge "movietype" in unserem Beispiel hat zwei Dinge:

- Sie stellt den Schlüssel für das **Dropdown List** -Hilfsprogramm bereit, um eine **IEnumerable-&lt;SelectListItem-&gt;** im **viewbag**zu finden.
- Es ist an das "movietype"-Formular Elementdaten gebunden. Wenn die Sende Methode " **HTTP Get**" ist, handelt es sich bei `MovieType` um eine Abfrage Zeichenfolge. Wenn die Submit-Methode **http Post**ist, wird `MovieType` im Nachrichtentext hinzugefügt. Die folgende Abbildung zeigt die Abfrage Zeichenfolge mit dem Wert 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

Der folgende Code zeigt die `CategoryChosen` Methode, an die das Formular übermittelt wurde.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Navigieren Sie zurück zur Testseite, und wählen Sie den Link **HTML SelectList** aus. Die HTML-Seite rendert ein SELECT-Element ähnlich der einfachen ASP.NET MVC-Testseite. Klicken Sie mit der rechten Maustaste auf das Browserfenster und wählen Sie **Quelle anzeigen** Das HTML-Markup für die Auswahlliste ist im Grunde identisch. Testen Sie die HTML-Seite. Sie funktioniert wie die ASP.NET MVC-Aktionsmethode und-Ansicht, die wir zuvor getestet haben.

### <a name="improving-the-movie-select-list-with-enums"></a>Verbessern der Filmauswahl Liste mit enums

Wenn die Kategorien in der Anwendung korrigiert sind und sich nicht ändern, können Sie die Vorteile von Enumerationen nutzen, um den Code robuster und einfacher zu erweitern. Wenn Sie eine neue Kategorie hinzufügen, wird der richtige Kategoriewert generiert. Vermeidet beim Hinzufügen einer neuen Kategorie den Wert zum Kopieren und einfügen, aber vergessen Sie nicht, den Kategoriewert zu aktualisieren.

Öffnen Sie die Datei " *controllers\homecontroller.cs* ", und untersuchen Sie den folgenden Code:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

Die [Enumerations`eMovieCategories` erfasst die vier](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) Filmtypen. Die `SetViewBagMovieType`-Methode erstellt die **IEnumerable-&lt;SelectListItem-&gt;** aus **der `eMovieCategories`** Enumeration und legt die `Selected`-Eigenschaft aus dem `selectedMovie`-Parameter fest. Die `SelectCategoryEnum` Aktionsmethode verwendet dieselbe Ansicht wie die `SelectCategory` Aktionsmethode.

Navigieren Sie zur Testseite, und klicken Sie auf den `Select Movie Category (Enum)` Link. Diesmal wird anstelle eines angezeigten Werts (Zahl) eine Zeichenfolge angezeigt, die die Enumeration darstellt.

### <a name="posting-enum-values"></a>Posten von Enumerationswerten

HTML-Formulare werden normalerweise verwendet, um Daten an den Server zu senden. Der folgende Code zeigt die `HTTP GET`-und `HTTP POST` Versionen der `SelectCategoryEnumPost`-Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Durch Übergeben einer `eMovieCategories` Enumeration an die `POST`-Methode können wir sowohl den Enumerationswert als auch die enumerationszeichenfolge extrahieren. Führen Sie das Beispiel aus, und navigieren Sie zur Testseite. Klicken Sie auf den `Select Movie Category(Enum Post)` Link. Wählen Sie einen Filmtyp aus, und klicken Sie auf die Schaltfläche Senden. Die Anzeige zeigt sowohl den Wert als auch den Namen des Film Typs an.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Erstellen eines SELECT-Elements für mehrere Abschnitte

Das [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) -HTML-Hilfsprogramm rendert das HTML-`<select>`-Element mit dem `multiple`-Attribut, das es Benutzern ermöglicht, mehrere Auswahlmöglichkeiten zu treffen. Navigieren Sie zum Link "Test", und wählen Sie dann den Link **Mehrfachauswahl Land** aus. Mit der gerenderten Benutzeroberfläche können Sie mehrere Länder auswählen. In der folgenden Abbildung sind Kanada und China ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Untersuchen des multiselectcountry-Codes

Überprüfen Sie den folgenden Code in der Datei " *controllers\homecontroller.cs* ".

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

Die `GetCountries`-Methode erstellt eine Liste von Ländern und übergibt sie an den `MultiSelectList`-Konstruktor. Die `MultiSelectList`-Konstruktorüberladung, die in der obigen Methode `GetCountries` verwendet wird, benötigt vier Parameter:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *Items*: ein [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) -Element, das die Elemente in der Liste enthält. Im obigen Beispiel ist die Liste der Länder.
2. *DataValueField*: der Name der Eigenschaft in der **IEnumerable** -Liste, die den Wert enthält. Im obigen Beispiel wird die `ID`-Eigenschaft.
3. *DataTextField*: der Name der Eigenschaft in der **IEnumerable** -Liste, die die anzuzeigenden Informationen enthält. Im obigen Beispiel wird die `name`-Eigenschaft.
4. *selectedvalues*: die Liste der ausgewählten Werte.

Im obigen Beispiel übergibt die `MultiSelectCountry`-Methode einen `null` Wert für die ausgewählten Länder, sodass keine Länder ausgewählt werden, wenn die Benutzeroberfläche angezeigt wird. Der folgende Code zeigt das Razor-Markup, das zum Rendering der `MultiSelectCountry` Ansicht verwendet wird.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

Die oben verwendete HTML- [hilfslistbox](https://msdn.microsoft.com/library/dd470200.aspx) -Methode nimmt zwei Parameter an, den Namen der zu bindenden Eigenschaft und die [multiselectlist](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) , die die Auswahl Optionen und-Werte enthält. Der obige `ViewBag.YouSelected` Code wird verwendet, um die Werte der Länder anzuzeigen, die Sie beim Senden des Formulars ausgewählt haben. Überprüfen Sie die HTTP-Post-Überladung der `MultiSelectCountry`-Methode.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

Die dynamische `ViewBag.YouSelected`-Eigenschaft enthält die ausgewählten Länder, die für den `Countries` Eintrag in der Formular Auflistung abgerufen wurden. In dieser Version wird der getländer-Methode eine Liste der ausgewählten Länder angezeigt, sodass beim Anzeigen der `MultiSelectCountry` Ansicht die ausgewählten Länder in der Benutzeroberfläche ausgewählt werden.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Ausgewähltes auswählen eines SELECT-Elements mit dem von der ausgewählten jQuery-Plug-in

Das [ausgewählte](https://harvesthq.github.com/chosen/) jQuery-Plug-in für die Auswahl kann einem HTML-&lt;&gt; Element hinzugefügt werden, um eine benutzerfreundliche Benutzeroberfläche zu erstellen. Die folgenden Images veranschaulichen das von der [Auswahl gewählte](https://harvesthq.github.com/chosen/) jQuery-Plug-in mit `MultiSelectCountry` Sicht.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

In den beiden folgenden Abbildungen ist **Kanada** ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

In der obigen Abbildung ist Kanada ausgewählt und enthält ein **x** , auf das Sie klicken können, um die Auswahl zu entfernen. In der folgenden Abbildung sind Kanada, China und Japan ausgewählt.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Einbinden des von der Ernte ausgewählten jQuery-Plug-ins

Der folgende Abschnitt ist leichter zu befolgen, wenn Sie mit jQuery vertraut sind. Wenn Sie jQuery bisher noch nie verwendet haben, können Sie eines der folgenden jQuery-Tutorials ausprobieren.

- [Funktionsweise von jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) durch [John Resig](http://ejohn.org/)
- [Getting Started with jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) von [Jörn zaefferer](http://bassistance.de/)
- [Live Beispiele für jQuery](http://codylindley.com/blogstuff/js/jquery/#) von [Cody Lindley](http://codylindley.com/)

Das ausgewählte Plug-in ist in den Starter-und abgeschlossenen Beispielprojekten enthalten, die dieses Tutorial begleiten. Für dieses Tutorial müssen Sie nur jQuery verwenden, um Sie mit der Benutzeroberfläche zu verbinden. Um das in einem ASP.NET MVC-Projekt ausgewählte jQuery-Plug-in zu verwenden, müssen Sie folgende Schritte ausführen:

1. Das ausgewählte Plug-in von [GitHub](https://github.com/harvesthq/chosen/)herunterladen. Dieser Schritt wurde für Sie ausgeführt.
2. Fügen Sie den ausgewählten Ordner Ihrem ASP.NET-MVC-Projekt hinzu. Fügen Sie die Assets aus dem im vorherigen Schritt heruntergeladenen Plug-in in den ausgewählten Ordner ein. Dieser Schritt wurde für Sie ausgeführt.
3. Verbinden Sie das ausgewählte Plug-in mit der **Dropdown List** -oder **ListBox** -HTML-Hilfsmethode.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Das ausgewählte Plug-in wird in die Ansicht "multiselectcountry" eingefügt.

Öffnen Sie die Datei *views\home\multiselectcountry.cshtml* , und fügen Sie der `Html.ListBox`einen `htmlAttributes`-Parameter hinzu. Der Parameter, den Sie hinzufügen, enthält einen Klassennamen für die SELECT-Liste (`@class = "chzn-select"`). Der abgeschlossene Code wird unten angezeigt:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

Im obigen Code fügen wir das HTML-Attribut und den Attribut Wert `class = "chzn-select"`hinzu. Das \@ Zeichen vorangehende Klasse hat nichts mit der Razor-Ansichts-Engine zu tun. `class` ist ein [ C# Schlüsselwort](https://msdn.microsoft.com/library/x53a06bb.aspx). C#Schlüsselwörter können nicht als Bezeichner verwendet werden, es sei denn, Sie schließen \@ als Präfix ein. Im obigen Beispiel ist `@class` ein gültiger Bezeichner, aber die **Klasse** ist nicht, weil **Class** ein Schlüsselwort ist.

Fügen Sie Verweise auf die *ausgewählten/ausgewählten. jQuery. js* -und ausgewählten */ausgewählten CSS* -Dateien hinzu. Die *ausgewählte/ausgewählte. jQuery. js-Datei* implementiert die Funktionalität des ausgewählten Plug-ins. Die *ausgewählte/ausgewählte CSS-* Datei stellt die Formatierung bereit. Fügen Sie diese Verweise am Ende der Datei " *views\home\multiselectcountry.cshtml* " hinzu. Der folgende Code zeigt, wie auf das ausgewählte Plug-in verwiesen wird.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Aktivieren Sie das ausgewählte Plug-in mithilfe des Klassen namens, der im **HTML. ListBox** -Code verwendet wird. Im obigen Beispiel ist der Klassenname `chzn-select`. Fügen Sie am unteren Rand der Ansichts Datei *views\home\multiselectcountry.cshtml* die folgende Zeile hinzu. Diese Zeile aktiviert das ausgewählte Plug-in.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

Die folgende Zeile ist die Syntax zum aufzurufen der jQuery-Ready-Funktion, die das DOM-Element mit dem Klassennamen `chzn-select`auswählt.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Der aus dem obigen-Befehl zurückgegebene umschließende Satz wendet dann die ausgewählte Methode (`.chosen();`) an, die das ausgewählte Plug-in verknüpft.

Der folgende Code zeigt die abgeschlossene Ansichts Datei " *views\home\multiselectcountry.cshtml* ".

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Führen Sie die Anwendung aus, und navigieren Sie zur Ansicht `MultiSelectCountry`. Versuchen Sie, Länder hinzuzufügen und zu löschen. Der bereitgestellte Beispiel Download enthält auch eine `MultiCountryVM` Methode und Sicht, die die multiselectcountry-Funktionalität mithilfe eines Ansichts Modells anstelle eines **viewbag**implementiert.

Im nächsten Abschnitt erfahren Sie, wie der ASP.NET MVC-Gerüstbau Mechanismus mit dem **DropDownList** -Hilfsprogramm funktioniert.

> [!div class="step-by-step"]
> [Weiter](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
