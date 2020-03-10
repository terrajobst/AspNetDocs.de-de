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
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="48a41-102">Überprüfen des Gerüstbaus durch ASP.NET MVC für das DropDownList-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="48a41-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>

<span data-ttu-id="48a41-103">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48a41-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="48a41-104">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Controllers* , und wählen Sie dann **Controller hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="48a41-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="48a41-105">Nennen Sie den Controller **storemanagercontroller**.</span><span class="sxs-lookup"><span data-stu-id="48a41-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="48a41-106">Legen Sie die Optionen für das Dialogfeld **Controller hinzufügen** fest, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="48a41-107">Bearbeiten Sie die Ansicht *storemanager\index.cshtml* , und entfernen Sie `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="48a41-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="48a41-108">Durch das Entfernen von `AlbumArtUrl` wird die Präsentation besser lesbar.</span><span class="sxs-lookup"><span data-stu-id="48a41-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="48a41-109">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="48a41-110">Öffnen Sie die Datei " *controllers\storemanagercontroller.cs* ", und suchen Sie nach der `Index`-Methode.</span><span class="sxs-lookup"><span data-stu-id="48a41-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="48a41-111">Fügen Sie die `OrderBy`-Klausel hinzu, damit die Alben nach Preis sortiert werden.</span><span class="sxs-lookup"><span data-stu-id="48a41-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="48a41-112">Der gesamte Code ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="48a41-113">Das Sortieren nach Preis vereinfacht das Testen von Änderungen an der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="48a41-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="48a41-114">Wenn Sie die Edit-und Create-Methoden testen, können Sie einen niedrigen Preis verwenden, damit die gespeicherten Daten zuerst angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="48a41-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="48a41-115">Öffnen Sie die Datei " *storemanager\edit.cshtml* ".</span><span class="sxs-lookup"><span data-stu-id="48a41-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="48a41-116">Fügen Sie die folgende Zeile direkt nach dem Legenden-Tag hinzu.</span><span class="sxs-lookup"><span data-stu-id="48a41-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="48a41-117">Der folgende Code zeigt den Kontext dieser Änderung:</span><span class="sxs-lookup"><span data-stu-id="48a41-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="48a41-118">Der `AlbumId` ist erforderlich, um Änderungen an einem Album Daten Satz vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="48a41-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="48a41-119">Drücken Sie STRG+F5, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="48a41-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="48a41-120">Wählen Sie den Link **Admin** aus, und klicken Sie dann auf den Link **neu erstellen** , um ein neues Album zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="48a41-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="48a41-121">Vergewissern Sie sich, dass die Albuminformationen gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="48a41-121">Verify the album information was saved.</span></span> <span data-ttu-id="48a41-122">Bearbeiten Sie ein Album, und überprüfen Sie, ob die vorgenommenen Änderungen persistent sind.</span><span class="sxs-lookup"><span data-stu-id="48a41-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="48a41-123">Das Album Schema</span><span class="sxs-lookup"><span data-stu-id="48a41-123">The Album Schema</span></span>

<span data-ttu-id="48a41-124">Der durch den MVC-Gerüstbau Mechanismus erstellte `StoreManager` Controller ermöglicht CRUD (Create, Read, Update, DELETE)-Zugriff auf die Alben in der Music Store-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="48a41-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="48a41-125">Das Schema für die Albuminformationen wird unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="48a41-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="48a41-126">In der `Albums` Tabelle werden das Genre und die Beschreibung des Albums nicht gespeichert. es wird ein Fremdschlüssel in der `Genres` Tabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="48a41-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="48a41-127">Die `Genres` Tabelle enthält den Namen und die Beschreibung des Genres.</span><span class="sxs-lookup"><span data-stu-id="48a41-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="48a41-128">Ebenso enthält die `Albums` Tabelle nicht den Namen des Album-Künstlers, sondern einen Fremdschlüssel für die `Artists` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="48a41-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="48a41-129">Die `Artists` Tabelle enthält den Namen des Künstlers.</span><span class="sxs-lookup"><span data-stu-id="48a41-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="48a41-130">Wenn Sie die Daten in der `Albums` Tabelle untersuchen, können Sie sehen, dass jede Zeile einen Fremdschlüssel für die `Genres` Tabelle und einen Fremdschlüssel für die `Artists` Tabelle enthält.</span><span class="sxs-lookup"><span data-stu-id="48a41-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="48a41-131">Die folgende Abbildung zeigt einige Tabellendaten aus der `Albums` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="48a41-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="48a41-132">Das HTML-Tag auswählen</span><span class="sxs-lookup"><span data-stu-id="48a41-132">The HTML Select Tag</span></span>

<span data-ttu-id="48a41-133">Das HTML-`<select>`-Element (durch das HTML- [Dropdown List](https://msdn.microsoft.com/library/dd492948.aspx) -Hilfsprogramm erstellt) wird verwendet, um eine komplette Liste von Werten (z. b. die Liste der Genres) anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="48a41-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="48a41-134">Wenn bei Formular bearbeiten der aktuelle Wert bekannt ist, kann in der Auswahlliste der aktuelle Wert angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="48a41-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="48a41-135">Wir haben dies zuvor gesehen, als wir den ausgewählten Wert auf " **Comedy**" festgelegt haben.</span><span class="sxs-lookup"><span data-stu-id="48a41-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="48a41-136">Die Auswahlliste eignet sich ideal zum Anzeigen von Kategorie-oder Fremdschlüssel Daten.</span><span class="sxs-lookup"><span data-stu-id="48a41-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="48a41-137">Das `<select>`-Element für den Genre Fremdschlüssel zeigt die Liste der möglichen Genre Namen an, aber wenn Sie das Formular speichern, wird die Genre-Eigenschaft mit dem Genre-Fremdschlüssel Wert und nicht mit dem angezeigten Genre Namen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="48a41-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="48a41-138">In der Abbildung unten ist das Genre **Disco** , und der Künstler ist **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="48a41-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="48a41-139">Untersuchen des ASP.NET MVC-Gerüst Codes</span><span class="sxs-lookup"><span data-stu-id="48a41-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="48a41-140">Öffnen Sie die Datei " *controllers\storemanagercontroller.cs* ", und suchen Sie nach der `HTTP GET Create`-Methode.</span><span class="sxs-lookup"><span data-stu-id="48a41-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="48a41-141">Die `Create`-Methode fügt dem `ViewBag`zwei [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) -Objekte hinzu, eine, die die Genre Informationen enthalten soll, und eine, die die Informationen des Künstlers enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="48a41-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="48a41-142">Die oben verwendete [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) -Konstruktorüberladung erfordert drei Argumente:</span><span class="sxs-lookup"><span data-stu-id="48a41-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="48a41-143">*Items*: ein [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) -Element, das die Elemente in der Liste enthält.</span><span class="sxs-lookup"><span data-stu-id="48a41-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="48a41-144">Im obigen Beispiel wird die Liste der von `db.Genres`zurückgegebenen Genres angezeigt.</span><span class="sxs-lookup"><span data-stu-id="48a41-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="48a41-145">*DataValueField*: der Name der Eigenschaft in der **IEnumerable** -Liste, die den Schlüsselwert enthält.</span><span class="sxs-lookup"><span data-stu-id="48a41-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="48a41-146">Im obigen Beispiel `GenreId` und `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="48a41-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="48a41-147">*DataTextField*: der Name der Eigenschaft in der **IEnumerable** -Liste, die die anzuzeigenden Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="48a41-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="48a41-148">In der Tabelle "Artists" und "Genre" wird das `name`-Feld verwendet.</span><span class="sxs-lookup"><span data-stu-id="48a41-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="48a41-149">Öffnen Sie die Datei *views\storemanager\kreate.cshtml* , und untersuchen Sie das `Html.DropDownList`-Hilfsprogramm Markup für das Genre-Feld.</span><span class="sxs-lookup"><span data-stu-id="48a41-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="48a41-150">Die erste Zeile zeigt, dass die CREATE VIEW ein `Album` Modell annimmt.</span><span class="sxs-lookup"><span data-stu-id="48a41-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="48a41-151">In der oben gezeigten `Create`-Methode wurde kein Modell weitergegeben, sodass die Sicht ein **null** -`Album` Modell erhält.</span><span class="sxs-lookup"><span data-stu-id="48a41-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="48a41-152">An diesem Punkt erstellen wir ein neues Album, damit keine `Album` Daten dafür vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="48a41-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="48a41-153">Die oben gezeigte [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) -Überladung übernimmt den Namen des Felds, das an das Modell gebunden werden soll.</span><span class="sxs-lookup"><span data-stu-id="48a41-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="48a41-154">Dieser Name wird auch verwendet, um nach einem **viewbag** -Objekt zu suchen, das ein [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) -Objekt enthält.</span><span class="sxs-lookup"><span data-stu-id="48a41-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="48a41-155">Mit dieser Überladung müssen Sie das **viewbag-SelectList** -Objekt `GenreId`benennen.</span><span class="sxs-lookup"><span data-stu-id="48a41-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="48a41-156">Der zweite Parameter (`String.Empty`) ist der Text, der angezeigt wird, wenn kein Element ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="48a41-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="48a41-157">Das ist genau das, was wir bei der Erstellung eines neuen Albums wollen.</span><span class="sxs-lookup"><span data-stu-id="48a41-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="48a41-158">Wenn Sie den zweiten Parameter entfernt und den folgenden Code verwendet haben:</span><span class="sxs-lookup"><span data-stu-id="48a41-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="48a41-159">Die Auswahlliste wird standardmäßig auf das erste Element oder "Rock" in unserem Beispiel eingestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="48a41-160">Untersuchen der `HTTP POST Create`-Methode.</span><span class="sxs-lookup"><span data-stu-id="48a41-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="48a41-161">Diese Überladung der `Create`-Methode nimmt ein `album` Objekt an, das vom ASP.NET MVC-Modell Bindungssystem aus den veröffentlichten Formular Werten erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="48a41-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="48a41-162">Wenn Sie ein neues Album übermitteln und der Modell Zustand gültig ist und keine Datenbankfehler vorliegen, wird das neue Album der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48a41-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="48a41-163">Die folgende Abbildung zeigt die Erstellung eines neuen Albums.</span><span class="sxs-lookup"><span data-stu-id="48a41-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="48a41-164">Sie können das [Tool "Tools](http://www.fiddler2.com/fiddler2/) " verwenden, um die von der ASP.NET MVC-Modell Bindung zum Erstellen des Album Objekts verwendeten Formular Werte zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="48a41-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="48a41-165">[https://login.microsoftonline.com/consumers/](![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png)).</span><span class="sxs-lookup"><span data-stu-id="48a41-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="48a41-166">Refactoring der viewbag-SelectList-Erstellung</span><span class="sxs-lookup"><span data-stu-id="48a41-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="48a41-167">Sowohl die `Edit`-Methode als auch die `HTTP POST Create`-Methode verfügen über identischen Code zum Einrichten der **SelectList** in der **viewbag**.</span><span class="sxs-lookup"><span data-stu-id="48a41-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="48a41-168">Im Sinne von [trocken](http://en.wikipedia.org/wiki/Don't_repeat_yourself)gestalten wir diesen Code um.</span><span class="sxs-lookup"><span data-stu-id="48a41-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="48a41-169">Dieser umgestaltete Code wird später verwendet.</span><span class="sxs-lookup"><span data-stu-id="48a41-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="48a41-170">Erstellen Sie eine neue Methode zum Hinzufügen eines Genres und einer Interpreten- **SelectList** zum **viewbag**.</span><span class="sxs-lookup"><span data-stu-id="48a41-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="48a41-171">Ersetzen Sie die beiden Zeilen, indem Sie die `ViewBag` in jeder der Methoden `Create` und `Edit` durch einen Aufrufder `SetGenreArtistViewBag`-Methode festlegen.</span><span class="sxs-lookup"><span data-stu-id="48a41-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="48a41-172">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="48a41-173">Erstellen Sie ein neues Album, und bearbeiten Sie ein Album, um die Änderungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="48a41-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="48a41-174">Explizites Übergeben der SelectList an die Dropdown Liste</span><span class="sxs-lookup"><span data-stu-id="48a41-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="48a41-175">Die vom ASP.NET MVC-Gerüstbau erstellten Ansichten zum Erstellen und Bearbeiten verwenden die folgende **DropDownList** -Überladung:</span><span class="sxs-lookup"><span data-stu-id="48a41-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="48a41-176">Das `DropDownList` Markup für die Ansicht erstellen ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="48a41-177">Da die `ViewBag`-Eigenschaft für den `SelectList` `GenreId`benannt ist, verwendet das **DropDownList** -Hilfsprogramm die `GenreId`**SelectList** in der **viewbag**.</span><span class="sxs-lookup"><span data-stu-id="48a41-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="48a41-178">In der folgenden **DropDownList** -Überladung wird der `SelectList` explizit übermittelt.</span><span class="sxs-lookup"><span data-stu-id="48a41-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="48a41-179">Öffnen Sie die Datei *views\storemanager\edit.cshtml* , und ändern Sie den **DropDownList** -Befehl, um die **SelectList**explizit zu übergeben. verwenden Sie dazu die oben genannte Überladung.</span><span class="sxs-lookup"><span data-stu-id="48a41-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="48a41-180">Führen Sie dies für die Genre Kategorie aus.</span><span class="sxs-lookup"><span data-stu-id="48a41-180">Do this for the Genre category.</span></span> <span data-ttu-id="48a41-181">Der abgeschlossene Code wird unten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="48a41-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="48a41-182">Führen Sie die Anwendung aus, klicken Sie auf den Link **Admin** , navigieren Sie zu einem Jazz-Album, und wählen Sie den Link **Bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="48a41-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="48a41-183">Anstatt Jazz als aktuell ausgewähltes Genre anzuzeigen, wird Rock angezeigt.</span><span class="sxs-lookup"><span data-stu-id="48a41-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="48a41-184">Wenn das Zeichen folgen Argument (die zu bindende Eigenschaft) und das **SelectList** -Objekt denselben Namen haben, wird der ausgewählte Wert nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="48a41-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="48a41-185">Wenn kein ausgewählter Wert angegeben ist, wird der Browser standardmäßig auf das erste Element in der **SelectList**(im obigen Beispiel **Rock** ) gesetzt.</span><span class="sxs-lookup"><span data-stu-id="48a41-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="48a41-186">Dies ist eine bekannte Einschränkung der **Dropdown List** -Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="48a41-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="48a41-187">Öffnen Sie die Datei *controllers\storemanagercontroller.cs* , und ändern Sie die Namen der **SelectList** -Objekte in `Genres` und `Artists`.</span><span class="sxs-lookup"><span data-stu-id="48a41-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="48a41-188">Der abgeschlossene Code wird unten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="48a41-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="48a41-189">Die Namen Genres und Künstler sind bessere Namen für die Kategorien, da Sie mehr als nur die ID jeder Kategorie enthalten.</span><span class="sxs-lookup"><span data-stu-id="48a41-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="48a41-190">Das Refactoring, das wir zuvor durchgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="48a41-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="48a41-191">Anstatt den **viewbag** in vier Methoden zu ändern, wurden unsere Änderungen in der `SetGenreArtistViewBag`-Methode isoliert.</span><span class="sxs-lookup"><span data-stu-id="48a41-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="48a41-192">Ändern Sie den **DropDownList** -Befehl in den Ansichten erstellen und bearbeiten, um die neuen **SelectList** -Namen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="48a41-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="48a41-193">Das neue Markup für die Bearbeitungs Ansicht wird unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="48a41-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="48a41-194">Die CREATE-Sicht erfordert eine leere Zeichenfolge, um zu verhindern, dass das erste Element in der SelectList angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="48a41-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="48a41-195">Erstellen Sie ein neues Album, und bearbeiten Sie ein Album, um die Änderungen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="48a41-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="48a41-196">Testen Sie den Bearbeitungs Code, indem Sie ein Album mit einem anderen Genre als "Rock" auswählen.</span><span class="sxs-lookup"><span data-stu-id="48a41-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="48a41-197">Verwenden eines Ansichts Modells mit dem Dropdown List-Hilfsprogramm</span><span class="sxs-lookup"><span data-stu-id="48a41-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="48a41-198">Erstellen Sie im Ordner "ViewModels" eine neue Klasse mit dem Namen `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="48a41-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="48a41-199">Ersetzen Sie den Code in der `AlbumSelectListViewModel`-Klasse durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="48a41-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="48a41-200">Der `AlbumSelectListViewModel`-Konstruktor nimmt ein Album, eine Liste von Künstlern und Genres und erstellt ein Objekt, das das Album und eine `SelectList` für Genres und Künstler enthält.</span><span class="sxs-lookup"><span data-stu-id="48a41-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="48a41-201">Erstellen Sie das Projekt, damit das `AlbumSelectListViewModel` verfügbar ist, wenn Sie im nächsten Schritt eine Ansicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="48a41-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="48a41-202">Fügen Sie der `StoreManagerController`eine `EditVM` Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="48a41-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="48a41-203">Der fertige Code wird nachfolgend dargestellt.</span><span class="sxs-lookup"><span data-stu-id="48a41-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="48a41-204">Klicken Sie mit der rechten Maustaste auf `AlbumSelectListViewModel`, und wählen Sie dann **Auflösen**und dann **mvcmusicstore. ViewModels;** aus.</span><span class="sxs-lookup"><span data-stu-id="48a41-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="48a41-205">Alternativ können Sie die folgende using-Anweisung hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="48a41-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="48a41-206">Klicken Sie mit der rechten Maustaste auf `EditVM` und wählen Sie **Ansicht**</span><span class="sxs-lookup"><span data-stu-id="48a41-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="48a41-207">Verwenden Sie die unten gezeigten Optionen.</span><span class="sxs-lookup"><span data-stu-id="48a41-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="48a41-208">Wählen Sie **Hinzufügen**aus, und ersetzen Sie dann den Inhalt der Datei *views\storemanager\editvm.cshtml* durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="48a41-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="48a41-209">Das `EditVM` Markup ähnelt dem ursprünglichen `Edit` Markup mit den folgenden Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="48a41-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="48a41-210">Modell Eigenschaften in der `Edit` Ansicht haben die Form `model.property`(z. b. `model.Title`).</span><span class="sxs-lookup"><span data-stu-id="48a41-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="48a41-211">Modell Eigenschaften in der `EditVm` Ansicht haben die Form `model.Album.property`(z. b. `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="48a41-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="48a41-212">Dies liegt daran, dass der `EditVM` Ansicht ein Container für eine `Album`und nicht eine `Album` wie in der `Edit` Ansicht übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="48a41-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="48a41-213">Der zweite Parameter **DropDownList** stammt aus dem Ansichts Modell, nicht aus dem **viewbag**.</span><span class="sxs-lookup"><span data-stu-id="48a41-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="48a41-214">Das **BeginForm** -Hilfsprogramm in der `EditVM` Ansicht wird explizit an die `Edit` Aktionsmethode zurückgesendet.</span><span class="sxs-lookup"><span data-stu-id="48a41-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="48a41-215">Durch das Zurücksenden an die `Edit` Aktion müssen wir keine `HTTP POST EditVM` Aktion schreiben und die `HTTP POST` `Edit` Aktion wieder verwenden.</span><span class="sxs-lookup"><span data-stu-id="48a41-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="48a41-216">Führen Sie die Anwendung aus, und bearbeiten Sie ein Album.</span><span class="sxs-lookup"><span data-stu-id="48a41-216">Run the application and edit an album.</span></span> <span data-ttu-id="48a41-217">Ändern Sie die URL, um `EditVM`zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="48a41-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="48a41-218">Ändern Sie ein Feld, und klicken Sie auf die Schaltfläche speichern, um **sicher** zustellen, dass der Code funktioniert</span><span class="sxs-lookup"><span data-stu-id="48a41-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="48a41-219">Welchen Ansatz sollten Sie verwenden?</span><span class="sxs-lookup"><span data-stu-id="48a41-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="48a41-220">Alle drei gezeigten Ansätze sind akzeptabel.</span><span class="sxs-lookup"><span data-stu-id="48a41-220">All three approaches shown are acceptable.</span></span> <span data-ttu-id="48a41-221">Viele Entwickler bevorzugen die explizite Übergabe der `SelectList` an die `DropDownList` mithilfe der `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="48a41-221">Many developers prefer to explicitly pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="48a41-222">Diese Vorgehensweise bietet den zusätzlichen Vorteil, dass Sie einen geeigneteren Namen für die Sammlung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="48a41-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="48a41-223">Der einzige Nachteil ist, dass Sie dem `ViewBag SelectList` Objekt nicht denselben Namen wie die Model-Eigenschaft benennen können.</span><span class="sxs-lookup"><span data-stu-id="48a41-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="48a41-224">Einige Entwickler bevorzugen den ViewModel-Ansatz.</span><span class="sxs-lookup"><span data-stu-id="48a41-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="48a41-225">Andere betrachten das ausführlichere Markup und das generierte HTML des ViewModel-Ansatzes als Nachteil.</span><span class="sxs-lookup"><span data-stu-id="48a41-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="48a41-226">In diesem Abschnitt haben wir drei Ansätze zur Verwendung der **Dropdown Liste** mit Kategoriedaten kennengelernt.</span><span class="sxs-lookup"><span data-stu-id="48a41-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="48a41-227">Im nächsten Abschnitt erfahren Sie, wie Sie eine neue Kategorie hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="48a41-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48a41-228">[Zurück](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Weiter](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="48a41-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
