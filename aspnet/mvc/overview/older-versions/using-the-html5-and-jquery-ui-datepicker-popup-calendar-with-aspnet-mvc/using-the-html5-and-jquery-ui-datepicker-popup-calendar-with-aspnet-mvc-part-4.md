---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.net MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 583e782641efea9a9517edb31f7718b28203d756
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433251"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 4

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.NET MVC-Webanwendung.

### <a name="adding-a-template-for-editing-dates"></a>Hinzufügen einer Vorlage zum Bearbeiten von Datumsangaben

In diesem Abschnitt erstellen Sie eine Vorlage zum Bearbeiten von Datumsangaben, die angewendet werden, wenn ASP.NET MVC eine Benutzeroberfläche zum Bearbeiten von Modell Eigenschaften anzeigt, die mit der **Date** -Enumeration des [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attributs gekennzeichnet sind. Die Vorlage stellt nur das Datum dar. die Zeit wird nicht angezeigt. In der Vorlage verwenden Sie den [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) -Popup Kalender, um eine Möglichkeit zum Bearbeiten von Datumsangaben zu bieten.

Öffnen Sie zunächst die Datei *Movie.cs* , und fügen Sie das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut mit der **Date** -Enumeration der `ReleaseDate`-Eigenschaft hinzu, wie im folgenden Code gezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Dieser Code bewirkt, dass das `ReleaseDate`-Feld in Anzeige Vorlagen und Bearbeitungs Vorlagen ohne die Uhrzeit angezeigt wird. Wenn Ihre Anwendung eine *Date. cshtml* -Vorlage im Ordner " *views\shared\editortemplates* " oder im Ordner " *views\movies\editortemplates* " enthält, wird diese Vorlage zum renderingbeliebiger `DateTime` Eigenschaft während der Bearbeitung verwendet. Andernfalls zeigt das integrierte ASP.net-Vorlagen System die Eigenschaft als Datum an.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie einen Link bearbeiten aus, um zu überprüfen, ob im Eingabefeld für das Veröffentlichungsdatum nur das Datum angezeigt wird.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

Erweitern Sie in **Projektmappen-Explorer**den Ordner *Ansichten* , erweitern Sie den Ordner frei *gegeben* , und klicken Sie dann mit der rechten Maustaste auf den Ordner *views\shared\editortemplates* .

Klicken Sie auf **Hinzufügen**und dann **auf anzeigen**. Das Dialogfeld **Ansicht hinzufügen** wird angezeigt.

Geben Sie im Feld **Ansichts Name** &quot;Datum&quot;ein.

Aktivieren Sie das Kontrollkästchen **als Teilansicht erstellen** . Stellen Sie sicher, dass die Kontrollkästchen **Layout oder Master Seite verwenden** und **eine stark typisierte Ansicht erstellen** nicht aktiviert sind.

Klicken Sie auf **Hinzufügen**. Die Vorlage " *views\shared\editor templates\date.cshtml* " wird erstellt.

Fügen Sie der Vorlage " *views\shared\editor templates\date.cshtml* " den folgenden Code hinzu.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

In der ersten Zeile wird das Modell als `DateTime` Typ deklariert. Obwohl Sie den Modelltyp nicht in Bearbeitungs-und Anzeige Vorlagen deklarieren müssen, ist es eine bewährte Vorgehensweise, dass Sie die Kompilierzeit Überprüfung des Modells erhalten, das an die Ansicht übermittelt wird. (Ein weiterer Vorteil ist, dass Sie dann IntelliSense für das Modell in der Ansicht in Visual Studio erhalten.) Wenn der Modelltyp nicht deklariert ist, betrachtet ASP.NET MVC ihn als [dynamischen](https://msdn.microsoft.com/library/dd264741.aspx) Typ, und es ist keine Typüberprüfung zur Kompilierzeit vorhanden. Wenn Sie das Modell als `DateTime` Typ deklarieren, wird es stark typisiert.

Die zweite Zeile ist nur ein literales HTML-Markup, das &quot;mithilfe der Datums Vorlage&quot; vor einem Datumsfeld anzeigt. Diese Zeile wird temporär verwendet, um zu überprüfen, ob diese Datums Vorlage verwendet wird.

Die nächste Zeile ist ein [HTML. TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) -Hilfsprogramm, das ein `input` Feld rendert, das ein Textfeld ist. Der dritte Parameter für das Hilfsprogramm verwendet einen anonymen Typ, um die-Klasse für das Textfeld auf `datefield` und den Typ `date`festzulegen. (Da `class` in C#reserviert ist, müssen Sie das `@` Zeichen verwenden, um das `class`-Attribut im C# Parser zu Escapezeichen zu versehen.)

Der `date` Typ ist ein HTML5-Eingabetyp, der HTML5-fähigen Browsern das Rendering eines HTML5-Kalender Steuer Elements ermöglicht. Später fügen Sie JavaScript hinzu, um den jQuery-DatePicker mithilfe der `datefield`-Klasse mit dem `Html.TextBox`-Element zu verbinden.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Sie können überprüfen, ob die `ReleaseDate`-Eigenschaft in der Bearbeitungs Ansicht die Vorlage bearbeiten verwendet, da die Vorlage &quot;mithilfe von Datums Vorlagen&quot; direkt vor dem Eingabefeld `ReleaseDate` Text anzeigt, wie in der folgenden Abbildung dargestellt:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Zeigen Sie in Ihrem Browser die Quelle der Seite an. (Klicken Sie beispielsweise mit der rechten Maustaste auf die Seite, und wählen Sie **Quelle anzeigen**aus.) Im folgenden Beispiel wird ein Teil des Markups für die Seite gezeigt, das die `class`-und `type` Attribute im gerenderten HTML-Code veranschaulicht.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Kehren Sie zur Vorlage " *views\shared\editor templates\date.cshtml* " zurück, und entfernen Sie die &quot;mithilfe der Datums Vorlage&quot; Markup. Die fertige Vorlage sieht nun wie folgt aus:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Hinzufügen eines jQuery UI DatePicker-Popup Kalenders mithilfe von nuget

In diesem Abschnitt fügen Sie den [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) -Popup Kalender zur Date-Edit-Vorlage hinzu. Die [jQuery UI](http://jqueryui.com/) -Bibliothek bietet Unterstützung für Animationen, erweiterte Effekte und anpassbare Widgets. Es basiert auf der jQuery-JavaScript-Bibliothek. Der DatePicker-Popup Kalender macht es einfach und natürlich, Datumsangaben mithilfe eines Kalenders einzugeben, anstatt eine Zeichenfolge einzugeben. Der Popup Kalender schränkt Benutzer auch auf rechtliche Daten ein – bei einem normalen Text Eintrag für ein Datum können Sie z. b. `2/33/1999` (am 33. Februar, 1999) eingeben, aber der [jQuery UI DatePicker](http://jqueryui.com/demos/datepicker/) -Popup Kalender lässt dies nicht zu.

Zunächst müssen Sie die jQuery-UI-Bibliotheken installieren. Dazu verwenden Sie nuget, ein Paket-Manager, der in SP1-Versionen von Visual Studio 2010 und Visual Web Developer enthalten ist.

Klicken Sie in Visual Web Developer im **Menü Extras** auf **nuget-Paket-Manager** , und wählen Sie dann **nuget-Pakete verwalten**aus.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Hinweis: Wenn der **nuget-Paket-Manager** -Befehl **im Menü Extras** nicht angezeigt wird, müssen Sie nuget installieren, indem Sie die Anweisungen auf der nuget-Website [Installieren](http://docs.nuget.org/docs/start-here/installing-nuget) .   
  
Wenn Sie Visual Studio anstelle von Visual Web Developer verwenden, wählen Sie im Menü **Extras den** Befehl **nuget-Paket-Manager** und dann **Bibliothekspaket Verweis hinzufügen**aus.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

Klicken Sie im Dialogfeld **mvcmovie-nuget-Pakete verwalten** auf die Registerkarte **Online** , und geben Sie dann &quot;jQuery. UI&quot; in das Suchfeld ein. Wählen Sie j **Query UI Widgets: DatePicker**aus, und wählen Sie dann die Schaltfläche **Installieren** aus.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

Nuget fügt diese Debugversionen und die minimalen Versionen von jQuery UI Core und der jQuery UI Date Picker dem Projekt hinzu:

- *"jQuery. UI. Core. js"*
- *jQuery. UI. Core. min. js*
- *"jQuery. UI. DatePicker. js"*
- *jQuery. UI. DatePicker. min. js*

Hinweis: die Debugversionen (die Dateien ohne die Erweiterung " *. min. js* ") sind für das Debuggen hilfreich, aber an einer Produktions Website würden Sie nur die minierten Versionen einschließen.

Um die jQuery-Datumsauswahl tatsächlich verwenden zu können, müssen Sie ein jQuery-Skript erstellen, mit dem das Calendar-widget mit der Bearbeitungs Vorlage eingebunden wird. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Scripts* , und wählen Sie **Hinzufügen**, **Neues Element**und dann **JScript-Datei**aus. Nennen Sie die Datei *datepickerready. js*.

Fügen Sie der Datei " *datepickerready. js* " den folgenden Code hinzu:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Wenn Sie mit jQuery nicht vertraut sind, finden Sie hier eine kurze Erläuterung dazu: die erste Zeile ist die &quot;jQuery Ready&quot;-Funktion, die aufgerufen wird, wenn alle DOM-Elemente in einer Seite geladen wurden. Die zweite Zeile wählt alle DOM-Elemente aus, die den Klassennamen `datefield`haben, und ruft dann die `datepicker`-Funktion für jedes dieser Elemente auf. (Denken Sie daran, dass Sie die `datefield`-Klasse der Vorlage " *views\shared\editortemplates\date.cshtml* " zuvor in diesem Tutorial hinzugefügt haben.)

Öffnen Sie als nächstes die Datei " *views\shared\\_Layout. cshtml* ". Sie müssen Verweise auf die folgenden Dateien hinzufügen, die alle erforderlich sind, damit Sie die Datumsauswahl verwenden können:

- *Content/Themes/Base/jQuery. UI. Core. CSS*
- *Content/Themes/Base/jQuery. UI. DatePicker. CSS*
- *Content/Themes/Base/jQuery. UI. Theme. CSS*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. min. js*
- *Datepickerready. js*

Das folgende Beispiel zeigt den tatsächlichen Code, den Sie am Ende des `head`-Elements in der Datei *views\shared\\_Layout. cshtml* hinzufügen sollten.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Der Abschnitt Complete `head` wird hier angezeigt:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Die [URL-Inhalts](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) Hilfsmethode konvertiert den Ressourcen Pfad in einen absoluten Pfad. Sie müssen `@URL.Content` verwenden, um auf diese Ressourcen korrekt zu verweisen, wenn die Anwendung auf IIS ausgeführt wird.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie einen Bearbeitungs Link aus, und platzieren Sie die Einfügemarke im **ReleaseDate** -Feld. Der jQuery UI-Popup Kalender wird angezeigt.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Wie die meisten jQuery-Steuerelemente ermöglicht Ihnen das DatePicker-Steuerelement eine umfassende Anpassung. Weitere Informationen finden Sie unter [visuelle Anpassung: Entwerfen eines jQuery UI-Designs](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) auf der [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) -Website.

### <a name="supporting-the-html5-date-input-control"></a>Unterstützen des HTML5-Datums Eingabe-Steuer Elements

Da mehr Browser HTML5 unterstützen, sollten Sie die native HTML5-Eingabe verwenden, wie z. b. das `date` input-Element, und den jQuery-Benutzeroberflächen Kalender nicht verwenden. Sie können der Anwendung Logik hinzufügen, um automatisch HTML5-Steuerelemente zu verwenden, wenn Sie vom Browser unterstützt werden. Ersetzen Sie dazu den Inhalt der Datei " *datepickerready. js* " durch Folgendes:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

In der ersten Zeile dieses Skripts wird modernizr verwendet, um zu überprüfen, ob HTML5-Datums Eingaben unterstützt werden Wenn dies nicht unterstützt wird, wird stattdessen die Datumsauswahl der jQuery-Benutzeroberfläche eingebunden. ([Modernizr](http://www.modernizr.com/docs/) ist eine Open-Source-JavaScript-Bibliothek, die die Verfügbarkeit von systemeigenen Implementierungen von HTML5 und CSS3 erkennt. Modernizr ist in allen neuen ASP.NET MVC-Projekten enthalten, die Sie erstellen.)

Nachdem Sie diese Änderung vorgenommen haben, können Sie Sie mit einem Browser testen, der HTML5 unterstützt, wie z. b. Opera 11. Führen Sie die Anwendung mithilfe eines HTML5-kompatiblen Browsers aus, und bearbeiten Sie einen filmeintrag. Das HTML5-Datums Steuerelement wird anstelle des jQuery UI-Popup Kalenders verwendet:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Da in neuen Versionen von Browsern HTML5 inkrementell implementiert wird, ist es jetzt sinnvoll, Ihrer Website Code hinzuzufügen, der eine Vielzahl von HTML5-Unterstützung bietet. Beispielsweise wird unten ein robusteres *datepickerready. js* -Skript angezeigt, mit dem Ihre Site Browser unterstützen kann, die nur teilweise das HTML5-Datums Steuerelement unterstützen.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Dieses Skript wählt HTML5-`input` Elemente vom Typ `date` aus, die das HTML5-Datums Steuerelement nicht vollständig unterstützen. Für diese Elemente verknüpft Sie den jQuery UI-Popup Kalender und ändert dann das `type`-Attribut von `date` in `text`. Durch Ändern des `type`-Attributs von `date` in `text`wird die Unterstützung von teilweiser HTML5-Datumsangaben entfernt. Ein noch robusteres *datepickerready. js* -Skript finden Sie unter [jsfiddle](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Hinzufügen von Datumsangaben, die NULL-Werte zulassen

Wenn Sie eine der vorhandenen Datums Vorlagen verwenden und ein NULL-Datum übergeben, erhalten Sie einen Laufzeitfehler. Damit die Datums Vorlagen stabiler werden, ändern Sie Sie, um NULL-Werte zu behandeln. Zum unterstützen von Datumsangaben, die NULL-Werte zulassen, ändern Sie den Code in *views\shared\displaytemplates\datetime.cshtml* in den folgenden Code:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Der Code gibt eine leere Zeichenfolge zurück, wenn das Modell **null**ist.

Ändern Sie den Code in der Datei *views\shared\editor templates\date.cshtml* wie folgt:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Wenn dieser Code ausgeführt wird und das Modell nicht NULL ist, wird der `DateTime` Wert des Modells verwendet. Wenn das Modell NULL ist, wird stattdessen das aktuelle Datum verwendet.

### <a name="wrapup"></a>Wrapup

In diesem Lernprogramm wurden die Grundlagen von ASP.net-Vorlagen behandelt, und es wird gezeigt, wie Sie den jQuery UI DatePicker-Popup Kalender in einer ASP.NET MVC-Anwendung verwenden. Weitere Informationen finden Sie in den folgenden Ressourcen:

- Informationen zur Lokalisierung finden Sie unter rajeesh es Blog [jqueryui DatePicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Weitere Informationen zur jQuery-Benutzeroberfläche finden Sie unter [jQuery UI](http://docs.jquery.com/UI).
- Informationen dazu, wie das DatePicker-Steuerelement lokalisiert wird, finden Sie unter [UI/DatePicker/Lokalisierung](http://docs.jquery.com/UI/Datepicker/Localization).
- Weitere Informationen zu den ASP.NET-MVC-Vorlagen finden Sie in der Blog Reihe von Brad Wilson unter [ASP.NET MVC 2-Vorlagen](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Obwohl die Reihe für ASP.NET MVC 2 geschrieben wurde, gilt das Material weiterhin für die aktuelle Version von ASP.NET MVC.

> [!div class="step-by-step"]
> [Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
