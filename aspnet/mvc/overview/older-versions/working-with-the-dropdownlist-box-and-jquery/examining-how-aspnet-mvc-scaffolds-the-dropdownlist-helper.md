---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Untersuchen, wie ASP.NET MVC das Dropdown List-Hilfsprogramm einrichtet | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498357"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Überprüfen des Gerüstbaus durch ASP.NET MVC für das DropDownList-Hilfsprogramm

von [Rick Anderson](https://twitter.com/RickAndMSFT)

Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Controllers* , und wählen Sie dann **Controller hinzufügen**aus. Nennen Sie den Controller **storemanagercontroller**. Legen Sie die Optionen für das Dialogfeld **Controller hinzufügen** fest, wie in der folgenden Abbildung dargestellt.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Bearbeiten Sie die Ansicht *storemanager\index.cshtml* , und entfernen Sie `AlbumArtUrl`. Durch das Entfernen von `AlbumArtUrl` wird die Präsentation besser lesbar. Der fertige Code wird nachfolgend dargestellt.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Öffnen Sie die Datei " *controllers\storemanagercontroller.cs* ", und suchen Sie nach der `Index`-Methode. Fügen Sie die `OrderBy`-Klausel hinzu, damit die Alben nach Preis sortiert werden. Der gesamte Code ist unten dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Das Sortieren nach Preis vereinfacht das Testen von Änderungen an der Datenbank. Wenn Sie die Edit-und Create-Methoden testen, können Sie einen niedrigen Preis verwenden, damit die gespeicherten Daten zuerst angezeigt werden.

Öffnen Sie die Datei " *storemanager\edit.cshtml* ". Fügen Sie die folgende Zeile direkt nach dem Legenden-Tag hinzu.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Der folgende Code zeigt den Kontext dieser Änderung:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

Der `AlbumId` ist erforderlich, um Änderungen an einem Album Daten Satz vorzunehmen.

Drücken Sie STRG+F5, um die Anwendung auszuführen. Wählen Sie den Link **Admin** aus, und klicken Sie dann auf den Link **neu erstellen** , um ein neues Album zu erstellen. Vergewissern Sie sich, dass die Albuminformationen gespeichert wurden. Bearbeiten Sie ein Album, und überprüfen Sie, ob die vorgenommenen Änderungen persistent sind.

### <a name="the-album-schema"></a>Das Album Schema

Der durch den MVC-Gerüstbau Mechanismus erstellte `StoreManager` Controller ermöglicht CRUD (Create, Read, Update, DELETE)-Zugriff auf die Alben in der Music Store-Datenbank. Das Schema für die Albuminformationen wird unten dargestellt:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

In der `Albums` Tabelle werden das Genre und die Beschreibung des Albums nicht gespeichert. es wird ein Fremdschlüssel in der `Genres` Tabelle gespeichert. Die `Genres` Tabelle enthält den Namen und die Beschreibung des Genres. Ebenso enthält die `Albums` Tabelle nicht den Namen des Album-Künstlers, sondern einen Fremdschlüssel für die `Artists` Tabelle. Die `Artists` Tabelle enthält den Namen des Künstlers. Wenn Sie die Daten in der `Albums` Tabelle untersuchen, können Sie sehen, dass jede Zeile einen Fremdschlüssel für die `Genres` Tabelle und einen Fremdschlüssel für die `Artists` Tabelle enthält. Die folgende Abbildung zeigt einige Tabellendaten aus der `Albums` Tabelle.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Das HTML-Tag auswählen

Das HTML-`<select>`-Element (durch das HTML- [Dropdown List](https://msdn.microsoft.com/library/dd492948.aspx) -Hilfsprogramm erstellt) wird verwendet, um eine komplette Liste von Werten (z. b. die Liste der Genres) anzuzeigen. Wenn bei Formular bearbeiten der aktuelle Wert bekannt ist, kann in der Auswahlliste der aktuelle Wert angezeigt werden. Wir haben dies zuvor gesehen, als wir den ausgewählten Wert auf " **Comedy**" festgelegt haben. Die Auswahlliste eignet sich ideal zum Anzeigen von Kategorie-oder Fremdschlüssel Daten. Das `<select>`-Element für den Genre Fremdschlüssel zeigt die Liste der möglichen Genre Namen an, aber wenn Sie das Formular speichern, wird die Genre-Eigenschaft mit dem Genre-Fremdschlüssel Wert und nicht mit dem angezeigten Genre Namen aktualisiert. In der Abbildung unten ist das Genre **Disco** , und der Künstler ist **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Untersuchen des ASP.NET MVC-Gerüst Codes

Öffnen Sie die Datei " *controllers\storemanagercontroller.cs* ", und suchen Sie nach der `HTTP GET Create`-Methode.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Die `Create`-Methode fügt dem `ViewBag`zwei [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) -Objekte hinzu, eine, die die Genre Informationen enthalten soll, und eine, die die Informationen des Künstlers enthalten soll. Die oben verwendete [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) -Konstruktorüberladung erfordert drei Argumente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *Items*: ein [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) -Element, das die Elemente in der Liste enthält. Im obigen Beispiel wird die Liste der von `db.Genres`zurückgegebenen Genres angezeigt.
2. *DataValueField*: der Name der Eigenschaft in der **IEnumerable** -Liste, die den Schlüsselwert enthält. Im obigen Beispiel `GenreId` und `ArtistId`.
3. *DataTextField*: der Name der Eigenschaft in der **IEnumerable** -Liste, die die anzuzeigenden Informationen enthält. In der Tabelle "Artists" und "Genre" wird das `name`-Feld verwendet.

Öffnen Sie die Datei *views\storemanager\kreate.cshtml* , und untersuchen Sie das `Html.DropDownList`-Hilfsprogramm Markup für das Genre-Feld.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Die erste Zeile zeigt, dass die CREATE VIEW ein `Album` Modell annimmt. In der oben gezeigten `Create`-Methode wurde kein Modell weitergegeben, sodass die Sicht ein **null** -`Album` Modell erhält. An diesem Punkt erstellen wir ein neues Album, damit keine `Album` Daten dafür vorhanden sind.

Die oben gezeigte [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) -Überladung übernimmt den Namen des Felds, das an das Modell gebunden werden soll. Dieser Name wird auch verwendet, um nach einem **viewbag** -Objekt zu suchen, das ein [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) -Objekt enthält. Mit dieser Überladung müssen Sie das **viewbag-SelectList** -Objekt `GenreId`benennen. Der zweite Parameter (`String.Empty`) ist der Text, der angezeigt wird, wenn kein Element ausgewählt ist. Das ist genau das, was wir bei der Erstellung eines neuen Albums wollen. Wenn Sie den zweiten Parameter entfernt und den folgenden Code verwendet haben:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Die Auswahlliste wird standardmäßig auf das erste Element oder "Rock" in unserem Beispiel eingestellt.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Untersuchen der `HTTP POST Create`-Methode.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Diese Überladung der `Create`-Methode nimmt ein `album` Objekt an, das vom ASP.NET MVC-Modell Bindungssystem aus den veröffentlichten Formular Werten erstellt wird. Wenn Sie ein neues Album übermitteln und der Modell Zustand gültig ist und keine Datenbankfehler vorliegen, wird das neue Album der Datenbank hinzugefügt. Die folgende Abbildung zeigt die Erstellung eines neuen Albums.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Sie können das [Tool "Tools](http://www.fiddler2.com/fiddler2/) " verwenden, um die von der ASP.NET MVC-Modell Bindung zum Erstellen des Album Objekts verwendeten Formular Werte zu untersuchen.

[https://login.microsoftonline.com/consumers/](![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactoring der viewbag-SelectList-Erstellung

Sowohl die `Edit`-Methode als auch die `HTTP POST Create`-Methode verfügen über identischen Code zum Einrichten der **SelectList** in der **viewbag**. Im Sinne von [trocken](http://en.wikipedia.org/wiki/Don't_repeat_yourself)gestalten wir diesen Code um. Dieser umgestaltete Code wird später verwendet.

Erstellen Sie eine neue Methode zum Hinzufügen eines Genres und einer Interpreten- **SelectList** zum **viewbag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Ersetzen Sie die beiden Zeilen, indem Sie die `ViewBag` in jeder der Methoden `Create` und `Edit` durch einen Aufrufder `SetGenreArtistViewBag`-Methode festlegen. Der fertige Code wird nachfolgend dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Erstellen Sie ein neues Album, und bearbeiten Sie ein Album, um die Änderungen zu überprüfen.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Explizites Übergeben der SelectList an die Dropdown Liste

Die vom ASP.NET MVC-Gerüstbau erstellten Ansichten zum Erstellen und Bearbeiten verwenden die folgende **DropDownList** -Überladung:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Das `DropDownList` Markup für die Ansicht erstellen ist unten dargestellt.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Da die `ViewBag`-Eigenschaft für den `SelectList` `GenreId`benannt ist, verwendet das **DropDownList** -Hilfsprogramm die `GenreId`**SelectList** in der **viewbag**. In der folgenden **DropDownList** -Überladung wird der `SelectList` explizit übermittelt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Öffnen Sie die Datei *views\storemanager\edit.cshtml* , und ändern Sie den **DropDownList** -Befehl, um die **SelectList**explizit zu übergeben. verwenden Sie dazu die oben genannte Überladung. Führen Sie dies für die Genre Kategorie aus. Der abgeschlossene Code wird unten angezeigt:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Führen Sie die Anwendung aus, klicken Sie auf den Link **Admin** , navigieren Sie zu einem Jazz-Album, und wählen Sie den Link **Bearbeiten** aus.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Anstatt Jazz als aktuell ausgewähltes Genre anzuzeigen, wird Rock angezeigt. Wenn das Zeichen folgen Argument (die zu bindende Eigenschaft) und das **SelectList** -Objekt denselben Namen haben, wird der ausgewählte Wert nicht verwendet. Wenn kein ausgewählter Wert angegeben ist, wird der Browser standardmäßig auf das erste Element in der **SelectList**(im obigen Beispiel **Rock** ) gesetzt. Dies ist eine bekannte Einschränkung der **Dropdown List** -Hilfsmethode.

Öffnen Sie die Datei *controllers\storemanagercontroller.cs* , und ändern Sie die Namen der **SelectList** -Objekte in `Genres` und `Artists`. Der abgeschlossene Code wird unten angezeigt:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Die Namen Genres und Künstler sind bessere Namen für die Kategorien, da Sie mehr als nur die ID jeder Kategorie enthalten. Das Refactoring, das wir zuvor durchgeführt haben. Anstatt den **viewbag** in vier Methoden zu ändern, wurden unsere Änderungen in der `SetGenreArtistViewBag`-Methode isoliert.

Ändern Sie den **DropDownList** -Befehl in den Ansichten erstellen und bearbeiten, um die neuen **SelectList** -Namen zu verwenden. Das neue Markup für die Bearbeitungs Ansicht wird unten dargestellt:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Die CREATE-Sicht erfordert eine leere Zeichenfolge, um zu verhindern, dass das erste Element in der SelectList angezeigt wird.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Erstellen Sie ein neues Album, und bearbeiten Sie ein Album, um die Änderungen zu überprüfen. Testen Sie den Bearbeitungs Code, indem Sie ein Album mit einem anderen Genre als "Rock" auswählen.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Verwenden eines Ansichts Modells mit dem Dropdown List-Hilfsprogramm

Erstellen Sie im Ordner "ViewModels" eine neue Klasse mit dem Namen `AlbumSelectListViewModel`. Ersetzen Sie den Code in der `AlbumSelectListViewModel`-Klasse durch Folgendes:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Der `AlbumSelectListViewModel`-Konstruktor nimmt ein Album, eine Liste von Künstlern und Genres und erstellt ein Objekt, das das Album und eine `SelectList` für Genres und Künstler enthält.

Erstellen Sie das Projekt, damit das `AlbumSelectListViewModel` verfügbar ist, wenn Sie im nächsten Schritt eine Ansicht erstellen.

Fügen Sie der `StoreManagerController`eine `EditVM` Methode hinzu. Der fertige Code wird nachfolgend dargestellt.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Klicken Sie mit der rechten Maustaste auf `AlbumSelectListViewModel`, und wählen Sie dann **Auflösen**und dann **mvcmusicstore. ViewModels;** aus.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternativ können Sie die folgende using-Anweisung hinzufügen:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Klicken Sie mit der rechten Maustaste auf `EditVM` und wählen Sie **Ansicht** Verwenden Sie die unten gezeigten Optionen.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Wählen Sie **Hinzufügen**aus, und ersetzen Sie dann den Inhalt der Datei *views\storemanager\editvm.cshtml* durch Folgendes:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Das `EditVM` Markup ähnelt dem ursprünglichen `Edit` Markup mit den folgenden Ausnahmen.

- Modell Eigenschaften in der `Edit` Ansicht haben die Form `model.property`(z. b. `model.Title`). Modell Eigenschaften in der `EditVm` Ansicht haben die Form `model.Album.property`(z. b. `model.Album.Title`). Dies liegt daran, dass der `EditVM` Ansicht ein Container für eine `Album`und nicht eine `Album` wie in der `Edit` Ansicht übermittelt wird.
- Der zweite Parameter **DropDownList** stammt aus dem Ansichts Modell, nicht aus dem **viewbag**.
- Das **BeginForm** -Hilfsprogramm in der `EditVM` Ansicht wird explizit an die `Edit` Aktionsmethode zurückgesendet. Durch das Zurücksenden an die `Edit` Aktion müssen wir keine `HTTP POST EditVM` Aktion schreiben und die `HTTP POST` `Edit` Aktion wieder verwenden.

Führen Sie die Anwendung aus, und bearbeiten Sie ein Album. Ändern Sie die URL, um `EditVM`zu verwenden. Ändern Sie ein Feld, und klicken Sie auf die Schaltfläche speichern, um **sicher** zustellen, dass der Code funktioniert

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Welchen Ansatz sollten Sie verwenden?

Alle drei gezeigten Ansätze sind akzeptabel. Viele Entwickler bevorzugen die explizite Übergabe der `SelectList` an die `DropDownList` mithilfe der `ViewBag`. Diese Vorgehensweise bietet den zusätzlichen Vorteil, dass Sie einen geeigneteren Namen für die Sammlung verwenden können. Der einzige Nachteil ist, dass Sie dem `ViewBag SelectList` Objekt nicht denselben Namen wie die Model-Eigenschaft benennen können.

Einige Entwickler bevorzugen den ViewModel-Ansatz. Andere betrachten das ausführlichere Markup und das generierte HTML des ViewModel-Ansatzes als Nachteil.

In diesem Abschnitt haben wir drei Ansätze zur Verwendung der **Dropdown Liste** mit Kategoriedaten kennengelernt. Im nächsten Abschnitt erfahren Sie, wie Sie eine neue Kategorie hinzufügen.

> [!div class="step-by-step"]
> [Zurück](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Weiter](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
