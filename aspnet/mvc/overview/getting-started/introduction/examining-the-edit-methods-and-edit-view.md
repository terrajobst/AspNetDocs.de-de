---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Untersuchen der Bearbeitungsmethoden und Bearbeiten der Ansicht | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 6cef963910b957e8b4ad7c7909385f6dbdff95c1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456062"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Überprüfen der Edit-Methoden und -Ansicht

von [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

In diesem Abschnitt untersuchen Sie die generierten `Edit` Aktionsmethoden und-Sichten für den Film Controller. Zuerst wird jedoch eine kurze Umleitung durchführen, um das Veröffentlichungsdatum besser aussehen zu lassen. Öffnen Sie die Datei *models\muvie.cs* , und fügen Sie die unten gezeigten markierten Zeilen hinzu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Sie können auch die Datums Kultur speziell wie folgt festlegen:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Wir behandeln [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) im nächsten Tutorial. Das [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx)-Attribut gibt an, was für den Namen eines Felds angezeigt werden soll (in diesem Fall „Release Date“ anstatt „ReleaseDate“). Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) -Attribut gibt den Typ der Daten an. in diesem Fall handelt es sich um ein Datum, sodass die im Feld gespeicherten Zeit Informationen nicht angezeigt werden. Das [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) -Attribut wird für einen Fehler im Chrome-Browser benötigt, der Datumsformate falsch rendert.

Führen Sie die Anwendung aus, und navigieren Sie zum `Movies` Controller. Halten Sie den Mauszeiger auf einen **Bearbeitungs** Link, um die URL anzuzeigen, mit der er verknüpft ist.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Der **Bearbeitungs** Link wurde durch die `Html.ActionLink`-Methode in der Ansicht *views\movies\index.cshtml* generiert:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Das `Html`-Objekt ist ein Hilfsprogramm, das über eine Eigenschaft der [System. Web. MVC. webviewpage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) -Basisklasse verfügbar gemacht wird. Die `ActionLink`-Methode des-Hilfsprogramms erleichtert das dynamische Generieren von HTML-Hyperlinks, die mit Aktionsmethoden auf Controllern verknüpft sind. Das erste Argument für die `ActionLink`-Methode ist der zu Rendering-Linktext (z. b. `<a>Edit Me</a>`). Das zweite Argument ist der Name der aufzurufenden Aktionsmethode (in diesem Fall die `Edit` Aktion). Das abschließende Argument ist ein [Anonymes Objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , das die Routendaten generiert (in diesem Fall die ID 4).

Der generierte Link, der in der vorherigen Abbildung angezeigt wird, ist `http://localhost:1234/Movies/Edit/4`. Die Standardroute (festgelegt in *App\_start\routeconfig.cs*) übernimmt das URL-Muster `{controller}/{action}/{id}`. Daher übersetzt ASP.net `http://localhost:1234/Movies/Edit/4` in eine Anforderung an die `Edit` Aktionsmethode des `Movies` Controllers, wobei der-Parameter `ID` gleich 4 ist. Überprüfen Sie den folgenden Code aus der *App\_start\routeconfig.cs* -Datei. Die [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) -Methode wird verwendet, um HTTP-Anforderungen an den korrekten Controller und die richtige Aktionsmethode weiterzuleiten und den optionalen ID-Parameter bereitzustellen. Die [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) -Methode wird auch von den [htmlhilfsobjeglern](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) verwendet, z. b. `ActionLink`, um URLs für den Controller, die Aktionsmethode und alle Routendaten zu generieren.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Sie können auch Aktionsmethoden Parameter mithilfe einer Abfrage Zeichenfolge übergeben. Beispielsweise übergibt die URL `http://localhost:1234/Movies/Edit?ID=3` auch den-Parameter `ID` von 3 an die `Edit`-Aktionsmethode des `Movies` Controllers.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Öffnen Sie den `Movies` Controller. Die zwei `Edit` Aktionsmethoden sind unten dargestellt.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `HttpPost`-Attribut vorangestellt ist. Dieses Attribut gibt an, dass die Überladung der `Edit`-Methode nur für Post-Anforderungen aufgerufen werden kann. Sie könnten das `HttpGet`-Attribut auf die erste Bearbeitungsmethode anwenden, dies ist jedoch nicht notwendig, da es sich hierbei um den Standardwert handelt. (Wir verweisen auf Aktionsmethoden, denen das `HttpGet`-Attribut implizit als `HttpGet` Methoden zugewiesen wird.) Das [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) -Attribut ist ein weiterer wichtiger Sicherheitsmechanismus, der die Übertragung von Daten in Ihrem Modell durch Hacker verhindert. Sie sollten nur Eigenschaften in das BIND-Attribut einschließen, das Sie ändern möchten. Informationen über das Überschreiben und das BIND-Attribut finden Sie im [Sicherheitshinweis für die Überschreibung](https://go.microsoft.com/fwlink/?LinkId=317598). Im einfachen Modell, das in diesem Tutorial verwendet wird, werden alle Daten im Modell gebunden. Das [validateantiforgerytoken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) -Attribut wird verwendet, um die Fälschung einer Anforderung zu verhindern, und wird mit `@Html.AntiForgeryToken()` in der Bearbeitungs Ansichts Datei (*views\movies\edit.cshtml*) gekoppelt, ein Teil wird unten angezeigt:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` generiert ein ausgeblendetes Formular-antifälschungstoken, das in der `Edit`-Methode des `Movies` Controllers entsprechen muss. Weitere Informationen zu Website übergreifender Anforderungs Fälschung (auch als XSRF oder CSRF bezeichnet) finden Sie in meinem Tutorial [XSRF/CSRF-Verhinderung in MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Die `HttpGet` `Edit`-Methode nimmt den Movie ID-Parameter an, sucht den Film mithilfe der Entity Framework `Find`-Methode und gibt den ausgewählten Film an die Bearbeitungs Ansicht zurück. Wenn ein Film nicht gefunden werden kann, wird [httpnotfound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) zurückgegeben. Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt. Das folgende Beispiel zeigt die Bearbeitungs Ansicht, die vom Visual Studio-Gerüstbau System generiert wurde:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Beachten Sie, dass die Ansichts Vorlage über eine `@model MvcMovie.Models.Movie`-Anweisung am Anfang der Datei verfügt – Dies gibt an, dass die Sicht erwartet, dass das Modell für die Ansichts Vorlage vom Typ `Movie`ist.

Der Gerüst Code verwendet mehrere *Hilfsmethoden* , um das HTML-Markup zu optimieren. Das [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) -Hilfsprogramm zeigt den Namen des Felds an (&quot;Titel&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;oder &quot;Preis&quot;). Das [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) -Hilfsprogramm rendert ein HTML-`<input>` Element. Das [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) -Hilfsprogramm zeigt alle Validierungs Meldungen an, die dieser Eigenschaft zugeordnet sind.

Führen Sie die Anwendung aus, und navigieren Sie zur URL */Movies* . Klicken Sie auf einen Link **Bearbeiten**. Zeigen Sie im Browser den Quelltext für die Seite an. Der HTML-Code für das Formular Element ist unten dargestellt.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Die `<input>` Elemente befinden sich in einem HTML-`<form>` Element, dessen `action`-Attribut für die Bereitstellung an die */Movies/Edit* -URL festgelegt ist. Die Formulardaten werden an den Server gesendet, wenn auf die Schaltfläche " **Speichern** " geklickt wird. Die zweite Zeile zeigt das ausgeblendete [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) -Token, das vom `@Html.AntiForgeryToken()`-Befehl generiert wurde.

## <a name="processing-the-post-request"></a>Verarbeiten der POST-Anforderung

Die folgende Liste zeigt die `HttpPost`-Version der `Edit`-Aktionsmethode.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Das [validateantiforgerytoken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) -Attribut überprüft das [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) -Token, das durch den `@Html.AntiForgeryToken()`-Aufrufe in der-Sicht generiert wurde.

Der [ASP.NET MVC-Modell Binder](https://msdn.microsoft.com/library/dd410405.aspx) nimmt die übermittelten Formular Werte an und erstellt ein `Movie` Objekt, das als `movie`-Parameter übergeben wird. Die `ModelState.IsValid` überprüft, ob die Daten, die im Formular übermittelt werden, zum Ändern (bearbeiten oder aktualisieren) eines `Movie` Objekts verwendet werden können. Wenn die Daten gültig sind, werden die Filmdaten in der `Movies`-Auflistung des `db`(`MovieDBContext` Instanz) gespeichert. Die neuen Filmdaten werden in der Datenbank gespeichert, indem die `SaveChanges`-Methode `MovieDBContext`aufgerufen wird. Nach dem Speichern der Daten leitet der Code den Benutzer an die `Index`-Aktionsmethode der `MoviesController`-Klasse weiter, die die Filmsammlung einschließlich der gerade vorgenommenen Änderungen anzeigt.

Sobald die Client seitige Validierung festlegt, dass der Wert eines Felds ungültig ist, wird eine Fehlermeldung angezeigt. Wenn JavaScript deaktiviert ist, wird die Client seitige Validierung deaktiviert. Der Server erkennt jedoch, dass die bereitgestellte Werte nicht gültig sind und die Formular Werte mit Fehlermeldungen erneut angezeigt werden.

Die Überprüfung wird später in diesem Tutorial ausführlicher überprüft.

Die `Html.ValidationMessageFor`-Hilfsprogramme in der Ansicht " *Edit. cshtml* " zeigen die entsprechenden Fehlermeldungen an.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Alle `HttpGet` Methoden folgen einem ähnlichen Muster. Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten im Fall von `Index`) und übergeben das Modell an die Ansicht. Die `Create`-Methode übergibt ein leeres Movie-Objekt an die CREATE-Sicht. Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `HttpPost`-Überladung der Methode. Das Ändern von Daten in einer HTTP Get-Methode stellt ein Sicherheitsrisiko dar, wie im Blogbeitrag Entry [ASP.NET MVC Tip #46 – use Delete Links (Erstellen von Sicherheitslücken](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)). Das Ändern von Daten in einer Get-Methode verstößt auch gegen bewährte HTTP-Methoden und das architektonische [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) -Muster, das angibt, dass Get-Anforderungen den Zustand der Anwendung nicht ändern sollten. Die Durchführung eines GET-Vorgangs sollte also eine sichere Operation sein, die keine Nebenwirkungen hat und die permanenten Daten nicht ändert.

## <a name="jquery-validation-for-non-english-locales"></a>jQuery-Validierung für nicht englische Gebiets Schemas

Wenn Sie einen US-englischen Computer verwenden, können Sie diesen Abschnitt überspringen und mit dem nächsten Tutorial fortfahren. Sie können die Globalize-Version dieses Tutorials [hier](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475)herunterladen. Ein hervorragendes zwei teilige Tutorial zur Internationalisierung finden Sie unter [Nadeem ASP.NET MVC 5-Internationalisierung](http://afana.me/post/aspnet-mvc-internationalization.aspx).

> [!NOTE]
> um die jQuery-Validierung für nicht englische Gebiets Schemas zu unterstützen, die ein Komma (&quot;&quot;) für einen Dezimaltrennzeichen und nicht-US-englische Datumsformate verwenden, müssen Sie *Globalize. js* und ihre spezifische *Kulturen/Globalize. Cultures. js* -Datei (von [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) und JavaScript einschließen, um `Globalize.parseFloat`zu verwenden. Sie können die nicht englische jquery-Validierung von nuget erhalten. (Installieren Sie Globalize nicht, wenn Sie ein englisches Gebiets Schema verwenden.)

1. Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und klicken Sie dann auf **nuget-Pakete für**Projekt Mappe verwalten.

    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. Klicken Sie im linken Bereich auf <strong>Durchsuchen *.</strong>*  (Weitere Informationen finden Sie in der Abbildung unten.)
3. Geben Sie im Eingabefeld * Globalize * * ein.

    Wählen Sie ![](examining-the-edit-methods-and-edit-view/_static/image6.png) `jQuery.Validation.Globalize``MvcMovie` aus, und klicken Sie auf **Installieren**. Die *script\jquery.globalize\globalize.js* -Datei wird dem Projekt hinzugefügt. Der Ordner * script\jquery.globalize\cultures\* enthält viele Kultur-JavaScript-Dateien. Beachten Sie, dass es für die Installation dieses Pakets fünf Minuten dauern kann.

   Der folgende Code zeigt die Änderungen an der Datei "views\movies\edit.cshtml":

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Um zu vermeiden, dass dieser Code in jeder Bearbeitungs Ansicht wiederholt wird, können Sie ihn in die Layoutdatei verschieben. Informationen zum Optimieren des Skript Downloads finden Sie in meinem Tutorial [bündeln und Mini](../../performance/bundling-and-minification.md)mieren.

Weitere Informationen finden Sie unter [ASP.NET MVC 3-Internationalisierung](http://afana.me/post/aspnet-mvc-internationalization.aspx) und [ASP.NET MVC 3-Internationalisierung-Part 2 (nerddinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Wenn Sie keine Überprüfung in Ihrem Gebiets Schema durchführen können, können Sie als temporäre Korrektur erzwingen, dass Ihr Computer US-Englisch verwendet, oder Sie können JavaScript in Ihrem Browser deaktivieren. Um die Verwendung von US-Englisch auf dem Computer zu erzwingen, können Sie das Globalisierungs Element der Projektstamm Datei *Web. config* hinzufügen. Der folgende Code zeigt das Globalisierungs Element, bei dem die Kultur auf USA Englisch festgelegt ist.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>Im nächsten Tutorial implementieren wir die Suchfunktion.

> [!div class="step-by-step"]
> [Zurück](accessing-your-models-data-from-a-controller.md)
> [Weiter](adding-search.md)
