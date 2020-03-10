---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 2 | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.net MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 325cc90eb6e717c47863eda6253e0d48d796386b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498417"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Verwenden des HTML5 und jQuery UI DatePicker-Popup Kalenders mit ASP.NET MVC-Part 2

von [Rick Anderson](https://twitter.com/RickAndMSFT)

> Dieses Tutorial vermittelt Ihnen die Grundlagen der Arbeit mit Editor-Vorlagen, Anzeige Vorlagen und des jQuery UI DatePicker-Popup Kalenders in einer ASP.NET MVC-Webanwendung.

## <a name="adding-an-automatic-datetime-template"></a>Hinzufügen einer automatischen DateTime-Vorlage

Im ersten Teil dieses Tutorials haben Sie erfahren, wie Sie dem Modell Attribute hinzufügen können, um die Formatierung explizit anzugeben, und wie Sie die Vorlage, die zum Rendering des Modells verwendet wird, explizit angeben können. Beispielsweise gibt das [Display Format](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut im folgenden Code explizit die Formatierung für die `ReleaseDate`-Eigenschaft an.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

Im folgenden Beispiel gibt das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) -Attribut mit der `Date`-Enumeration an, dass die Datums Vorlage zum Rendering des Modells verwendet werden soll. Wenn in Ihrem Projekt keine Datums Vorlage vorhanden ist, wird die integrierte Datums Vorlage verwendet.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

ASP. MVC kann die Typübereinstimmung mithilfe der Konvention-über-Konfiguration durchführen, indem nach einer Vorlage gesucht wird, die mit dem Namen eines Typs übereinstimmt. Auf diese Weise können Sie eine Vorlage erstellen, die Daten automatisch ohne Verwendung von Attributen oder Code formatiert. In diesem Teil des Tutorials erstellen Sie eine Vorlage, die automatisch auf Modell Eigenschaften vom Typ " [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)" angewendet wird. Sie müssen ein Attribut oder eine andere Konfiguration nicht verwenden, um anzugeben, dass die Vorlage zum Rendering aller Modell Eigenschaften vom Typ " [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)" verwendet werden soll.

Außerdem lernen Sie, wie Sie die Anzeige einzelner Eigenschaften oder einzelner Felder anpassen können.

Entfernen Sie zunächst vorhandene Formatierungsinformationen, und zeigen Sie vollständige Datumsangaben in der Anwendung an.

Öffnen Sie die Datei *Movie.cs* , und kommentieren Sie das `DataType`-Attribut für die `ReleaseDate`-Eigenschaft aus:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Beachten Sie, dass die `ReleaseDate`-Eigenschaft jetzt das Datum und die Uhrzeit anzeigt, da dies die Standardeinstellung ist, wenn keine Formatierungsinformationen bereitgestellt werden.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Hinzufügen von CSS-Stilen zum Testen neuer Vorlagen

Bevor Sie eine Vorlage zum Formatieren von Datumsangaben erstellen, fügen Sie einige CSS-Stilregeln hinzu, die Sie auf die neuen Vorlagen anwenden können. Dadurch können Sie überprüfen, ob die gerenderte Seite die neue Vorlage verwendet.

Öffnen Sie die Datei " *content\site.cs*", und fügen Sie am Ende der Datei die folgenden CSS-Regeln hinzu:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Hinzufügen von DateTime-Anzeige Vorlagen

Nun können Sie die neue Vorlage erstellen. Erstellen Sie im Ordner *views\movies* einen Ordner *DisplayTemplates* .

Erstellen Sie im Ordner *views\shared* einen Ordner *DisplayTemplates* und einen Ordner *editortemplates* .

Die Anzeige Vorlagen im Ordner *views\shared\displaytemplates* werden von allen Controllern verwendet. Die Anzeige Vorlagen im Ordner *views\muvie\displaytemplates* werden nur vom `Movie` Controller verwendet. (Wenn in beiden Ordnern eine Vorlage mit demselben Namen angezeigt wird, hat die Vorlage im Ordner " *views\muvie\displaytemplates* " – d. h. die spezifischere Vorlage – Vorrang vor den vom `Movie` Controller zurückgegebenen Sichten.)

Erweitern Sie in **Projektmappen-Explorer**den Ordner *Ansichten* , erweitern Sie den Ordner frei *gegeben* , und klicken Sie dann mit der rechten Maustaste auf den Ordner *views\shared\displaytemplates* .

Klicken Sie auf **Hinzufügen** und dann **auf anzeigen**. Das Dialogfeld **Ansicht hinzufügen** wird angezeigt.

Geben Sie im Feld **Ansichts Name** `DateTime`ein. (Sie müssen diesen Namen verwenden, um mit dem Namen des Typs zu vergleichen.)

Aktivieren Sie das Kontrollkästchen **als Teilansicht erstellen** . Stellen Sie sicher, dass die Kontrollkästchen **Layout oder Master Seite verwenden** und **eine stark typisierte Ansicht erstellen** nicht aktiviert sind.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Klicken Sie auf **Hinzufügen**. Eine Vorlage " *DateTime. cshtml* " wird in " *views\shared\displaytemplates*" erstellt.

In der folgenden Abbildung wird der Ordner *views* in **Projektmappen-Explorer** angezeigt, nachdem die Vorlagen für die `DateTime` Anzeige und den Editor erstellt wurden.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Öffnen Sie die Datei " *views\shared\displaytemplates\datetime.cshtml* ", und fügen Sie das folgende Markup hinzu, das die [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) -Methode verwendet, um die Eigenschaft als Datum ohne Zeit zu formatieren. (Das `{0:d}` Format gibt das kurze Datumsformat an.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Wiederholen Sie diesen Schritt, um eine `DateTime` Vorlage im Ordner " *views\muvie\displaytemplates* " zu erstellen. Verwenden Sie den folgenden Code in der Datei " *views\muvie\displaytemplates\datetime.cshtml* ".

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

Die `loud-1` CSS-Klasse bewirkt, dass das Datum in fettem roten Text angezeigt wird. Sie haben die `loud-1` CSS-Klasse als temporäre Maßnahme hinzugefügt, sodass Sie leicht erkennen können, wann diese bestimmte Vorlage verwendet wird.

Sie haben nun erstellt und angepasste Vorlagen verwendet, die ASP.net zum Anzeigen von Datumsangaben verwenden wird. Die allgemeinere Vorlage (im Ordner " *views\shared\displaytemplates* ") zeigt ein einfaches kurzes Datum an. Die Vorlage speziell für den `Movie` Controller (im Ordner " *views\movies\displaytemplates* ") zeigt ein kurzes Datum an, das ebenfalls als fett formatierten roten Text formatiert ist.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Der Browser rendert die Index Ansicht für die Anwendung.

Die `ReleaseDate`-Eigenschaft zeigt nun das Datum in einer fetten roten Schriftart ohne die Uhrzeit an. Auf diese Weise können Sie sicherstellen, dass die `DateTime` auf Vorlagen basierende Hilfe im Ordner *views\movies\displaytemplates* im freigegebenen Ordner (*views\shared\displaytemplates*) über die `DateTime` auf Vorlagen basierende Hilfe ausgewählt wird.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Benennen Sie nun die Datei " *views\movies\displaytemplates\datetime.cshtml* " in " *views\movies\displaytemplates\louddatetime.cshtml*" um.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Dieses Mal zeigt die `ReleaseDate`-Eigenschaft ein Datum ohne die Uhrzeit und ohne die rote Schriftart an. Dies veranschaulicht, dass eine Vorlage mit dem Namen des Datentyps (in diesem Fall `DateTime`) automatisch verwendet wird, um alle Modell Eigenschaften dieses Typs anzuzeigen. Nachdem Sie die Datei " *DateTime. cshtml* " in " *louddatetime. cshtml*" umbenannt haben, hat ASP.NET keine Vorlage mehr im Ordner " *views\movies\displaytemplates* " gefunden. Daher wurde die Vorlage " *DateTime. cshtml* " aus dem Ordner "* views\movies\shared\*" verwendet.

(Bei der Vorlagen Übereinstimmung wird die Groß-/Kleinschreibung nicht beachtet, sodass Sie den Namen der Vorlagen Datei in beliebiger Schreibweise Beispielsweise entsprechen *DateTime. cshtml, DateTime. cshtml*und *DateTime. cshtml* dem `DateTime`-Typ.)

Zum überprüfen: an diesem Punkt wird das Feld "`ReleaseDate`" mithilfe der Vorlage " *views\movies\displaytemplates\datetime.cshtml* " angezeigt, in der die Daten in einem kurzen Datumsformat angezeigt werden. andernfalls wird kein spezielles Format hinzugefügt.

### <a name="using-uihint-to-specify-a-display-template"></a>Verwenden von UIHint zum Angeben einer Anzeige Vorlage

Wenn Ihre Webanwendung über viele `DateTime` Felder verfügt und Sie standardmäßig alle oder die meisten von Ihnen im reinen Datumsformat anzeigen möchten, ist die Vorlage " *DateTime. cshtml* " ein guter Ansatz. Aber was geschieht, wenn Sie ein paar Datumsangaben haben, an denen Sie das vollständige Datum und die Uhrzeit anzeigen möchten? Kein Problem. Sie können eine zusätzliche Vorlage erstellen und das [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) -Attribut verwenden, um die Formatierung für das vollständige Datum und die Uhrzeit anzugeben. Anschließend können Sie diese Vorlage selektiv anwenden. Sie können das Attribut " [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) " auf Modell Ebene verwenden, oder Sie können die Vorlage in einer Ansicht angeben. In diesem Abschnitt erfahren Sie, wie Sie das `UIHint`-Attribut verwenden, um die Formatierung für einige Instanzen von Datums-/Uhrzeitfeldern selektiv zu ändern.

Öffnen Sie die Datei " *views\movies\displaytemplates\louddatetime.cshtml* ", und ersetzen Sie den vorhandenen Code durch Folgendes:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Dies bewirkt, dass das vollständige Datum und die Uhrzeit angezeigt werden, und fügt die CSS-Klasse hinzu, die den Text grün und groß macht.

Öffnen Sie die Datei *Movie.cs* , und fügen Sie das Attribut " [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) " zur `ReleaseDate`-Eigenschaft hinzu, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Dies weist ASP.NET MVC an, dass die " *louddatetime. cshtml* "-Vorlage verwendet wird, wenn die `ReleaseDate`-Eigenschaft (insbesondere nicht nur ein `DateTime` Objekt) angezeigt wird.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Beachten Sie, dass die `ReleaseDate`-Eigenschaft jetzt das Datum und die Uhrzeit in einer großen grünen Schriftart anzeigt.

Kehren Sie zum `UIHint`-Attribut in der *Movie.cs* -Datei zurück, und kommentieren Sie es aus, sodass die Vorlage " *louddatetime. cshtml* " nicht verwendet wird. Erneutes Ausführen der Anwendung Das Veröffentlichungsdatum wird nicht groß und grün angezeigt. Dadurch wird sichergestellt, dass die Vorlage " *views\shared\displaytemplates\datetime.cshtml* " in den Ansichten "index" und "Details" verwendet wird.

Wie bereits erwähnt, können Sie auch eine Vorlage in einer Ansicht anwenden, mit der Sie die Vorlage auf eine einzelne Instanz einiger Daten anwenden können. Öffnen Sie die Ansicht *views\movies\details.cshtml* . Fügen Sie `"LoudDateTime"` als zweiten Parameter des [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) -Aufrufes für das `ReleaseDate` Feld hinzu. Der fertige Code sieht wie folgt aus:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Dies gibt an, dass die `LoudDateTime`-Vorlage verwendet werden soll, um die Model-Eigenschaft anzuzeigen, unabhängig davon, welche Attribute auf das Modell angewendet werden.

Drücken Sie STRG+F5, um die Anwendung auszuführen.

Vergewissern Sie sich, dass auf der Seite Movie Index die Vorlage *views\shared\displaytemplates\datetime.cshtml* (rot Fett) verwendet wird und die Seite " *movie\details* " die Vorlage " *views\movies\displaytemplates\louddatetime.cshtml* " verwendet (groß und grün).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Im nächsten Abschnitt erstellen Sie eine Vorlage für einen komplexen Typ.

> [!div class="step-by-step"]
> [Zurück](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Weiter](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
