---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Hinzufügen einer neuen Kategorie zu "Dropdown List" mithilfe der jQuery-Benutzeroberfläche | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: cb9053593e2ea788638aec063c845cb91121861b
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075111"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="43d8d-102">Hinzufügen einer neuen Kategorie zu DropDownList mithilfe der jQuery-Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="43d8d-102">Adding a New Category to the DropDownList using jQuery UI</span></span>

<span data-ttu-id="43d8d-103">von [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="43d8d-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="43d8d-104">Das HTML-`Select`-Tag eignet sich ideal für die Darstellung einer Liste von Daten fester Kategorie. häufig müssen Sie jedoch eine neue Kategorie hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="43d8d-105">Angenommen, wir möchten das Genre "Opera" den Kategorien in unserer Datenbank hinzufügen?</span><span class="sxs-lookup"><span data-stu-id="43d8d-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="43d8d-106">In diesem Abschnitt verwenden wir die jQuery-Benutzeroberfläche, um ein Dialogfeld hinzuzufügen, das zum Hinzufügen einer neuen Kategorie verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="43d8d-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="43d8d-107">In der folgenden Abbildung wird gezeigt, wie die Benutzeroberfläche im Browser vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="43d8d-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="43d8d-108">Wenn ein Benutzer den Link **neues Genre hinzufügen** auswählt, wird der Benutzer in einem Popup Dialogfeld zur Eingabe eines neuen Genre namens (und optional einer Beschreibung) aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="43d8d-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="43d8d-109">Die folgende Abbildung zeigt das Popup Dialogfeld **Genre hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="43d8d-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="43d8d-110">Wenn ein neuer Genre Name eingegeben wird und die Schaltfläche " **Speichern** " gedrückt wird, geschieht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="43d8d-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="43d8d-111">Ein AJAX-Befehl sendet die Daten an die Create-Methode des Genre Controllers, der das neue Genre in der Datenbank speichert und die neuen Genre Informationen (Genre Name und ID) als JSON zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="43d8d-112">JavaScript fügt die neuen Genre Daten der Auswahlliste hinzu.</span><span class="sxs-lookup"><span data-stu-id="43d8d-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="43d8d-113">JavaScript macht das neue Genre zum ausgewählten Element.</span><span class="sxs-lookup"><span data-stu-id="43d8d-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="43d8d-114">In der folgenden Abbildung wurde **Opera** der Datenbank hinzugefügt und in der Dropdown Liste **Genre** ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="43d8d-115">Öffnen Sie die Datei *views\storemanager\kreate.cshtml* , und ersetzen Sie das Genre Markup durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="43d8d-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="43d8d-116">Die `_ChooseGenre` partielle Ansicht enthält die gesamte Logik zum Verbinden von JavaScript und jQuery, die zum Implementieren des Features neues Genre hinzufügen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="43d8d-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="43d8d-117">Nachdem wir den Code abgeschlossen haben, ist es einfach, mit der Benutzeroberfläche des Künstlers gleich zu Vorgehen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="43d8d-118">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *views\storemanager* , und wählen Sie **Hinzufügen**und dann **anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="43d8d-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="43d8d-119">Geben Sie in der Eingabe für den **Ansichts Namen** `_ChooseGenre` klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="43d8d-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="43d8d-120">Ersetzen Sie das Markup in der Datei *views\storemanager\\_ChooseGenre. cshtml* durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="43d8d-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="43d8d-121">Die erste Zeile deklariert, dass wir eine `Album` als Modell übergeben, genau die gleiche Modell Anweisung, die in der Create-Ansicht gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="43d8d-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="43d8d-122">**Die nächsten Zeilen sind das Markup** -Hilfsobjekt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="43d8d-123">Die nächste Zeile ist der **Dropdown List** -Hilfsobjekt, genau das gleiche wie in der ursprünglichen CREATE-Sicht.</span><span class="sxs-lookup"><span data-stu-id="43d8d-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="43d8d-124">Die nächste Zeile fügt einen Link mit dem Namen `Add New Genre`hinzu und formatiert ihn wie eine Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="43d8d-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="43d8d-125">Die Zeile, die `ValidationMessageFor` enthält, wird direkt aus der Create-Ansicht kopiert.</span><span class="sxs-lookup"><span data-stu-id="43d8d-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="43d8d-126">Die folgenden Zeilen:</span><span class="sxs-lookup"><span data-stu-id="43d8d-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="43d8d-127">erstellt ein verborgenes div mit der ID `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="43d8d-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="43d8d-128">Wir verwenden jQuery, um das Dialogfeld **Genre hinzufügen** mit der ID `genreDialog` in diesem DIV-Ereignis zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="43d8d-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="43d8d-129">Die letzten zwei Skript Tags enthalten Links zu den JavaScript-Dateien, die zum Implementieren des Features neues Genre hinzufügen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="43d8d-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="43d8d-130">Die Datei */Scripts/chooseGenre.js* wird für Sie im Projekt bereitgestellt. Wir werden Sie später in diesem Tutorial untersuchen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="43d8d-131">Führen Sie die Anwendung aus, und klicken Sie auf die Schaltfläche **neues Genre hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="43d8d-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="43d8d-132">Geben Sie im Dialogfeld **Genre hinzufügen** im Feld **Name die Bezeichnung** **Opera** ein.</span><span class="sxs-lookup"><span data-stu-id="43d8d-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="43d8d-133">Klicken Sie auf die Schaltfläche **Save** .</span><span class="sxs-lookup"><span data-stu-id="43d8d-133">Click the **Save** button.</span></span> <span data-ttu-id="43d8d-134">Ein AJAX-Befehl erstellt die Kategorie "Opera", füllt die Dropdown Liste mit "Opera" auf und legt Opera als ausgewähltes Genre fest.</span><span class="sxs-lookup"><span data-stu-id="43d8d-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="43d8d-135">Geben Sie einen Künstler, Titel und Preis ein, und wählen Sie dann die Schaltfläche **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="43d8d-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="43d8d-136">Wenn Sie einen Preis unter $8,99 eingeben, wird das neue Album oben in der Index Sicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="43d8d-137">Vergewissern Sie sich, dass der neue Album Eintrag in der Datenbank gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="43d8d-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="43d8d-138">Erstellen Sie ein neues Genre mit nur einem Buchstaben.</span><span class="sxs-lookup"><span data-stu-id="43d8d-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="43d8d-139">Der folgende Code in der Datei *models\genre.cs* legt die minimale und maximale Länge des Genre namensfest.</span><span class="sxs-lookup"><span data-stu-id="43d8d-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="43d8d-140">Client seitige Validierungs Berichte Sie müssen eine Zeichenfolge zwischen 2 und 20 Zeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="43d8d-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="43d8d-141">Es wird geprüft, wie ein neues Genre zur Datenbank und zur Auswahlliste hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="43d8d-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="43d8d-142">Öffnen Sie die Datei *scripung\choosegenre.js* , und untersuchen Sie den Code.</span><span class="sxs-lookup"><span data-stu-id="43d8d-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="43d8d-143">In der zweiten Zeile wird die ID `genreDialog` verwendet, um ein Dialogfeld für das div-Tag in der Datei *views\storemanager\\_ChooseGenre. cshtml* zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="43d8d-144">Die meisten benannten Parameter sind selbsterklärend.</span><span class="sxs-lookup"><span data-stu-id="43d8d-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="43d8d-145">Der `autoOpen`-Parameter ist auf false festgelegt. Wenn Sie die Schaltfläche **Genre erstellen** auswählen, wird der Dialog explizit geöffnet (Dies wird in diesem Abschnitt beschrieben).</span><span class="sxs-lookup"><span data-stu-id="43d8d-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="43d8d-146">Das Dialogfeld verfügt über zwei Schaltflächen, **Speichern** und **Abbrechen**.</span><span class="sxs-lookup"><span data-stu-id="43d8d-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="43d8d-147">Die Schaltfläche **Abbrechen** schließt das Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="43d8d-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="43d8d-148">Der folgende Code zeigt die Funktion zum **Speichern** von Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="43d8d-149">Die `var createGenreForm` wird aus der `createGenreForm`-ID ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="43d8d-150">Die `createGenreForm`-ID wurde im folgenden Code in der Datei *views\genre\\_CreateGenre. cshtml* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="43d8d-151">Die in der Datei *views\genre\\_CreateGenre. cshtml* verwendete hilfsüberladung [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) generiert HTML mit einem action-Attribut, das die URL zum Senden des Formulars enthält.</span><span class="sxs-lookup"><span data-stu-id="43d8d-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="43d8d-152">Sie können dies sehen, indem Sie die Seite Album erstellen in einem Browser anzeigen und im Browser Quelle anzeigen auswählen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="43d8d-153">Das folgende Markup zeigt den generierten HTML-Code, der das Formular-Tag enthält.</span><span class="sxs-lookup"><span data-stu-id="43d8d-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="43d8d-154">Die jQuery-`$.post` Zeile führt einen AJAX-Rückruf für das action-Attribut (`/StoreManager/Create`) aus und übergibt die Daten aus dem Dialogfeld " **Genre erstellen** ".</span><span class="sxs-lookup"><span data-stu-id="43d8d-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="43d8d-155">Die Daten bestehen aus dem Namen des neuen Genres und einer optionalen Beschreibung.</span><span class="sxs-lookup"><span data-stu-id="43d8d-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="43d8d-156">Wenn der AJAX-Befehl erfolgreich ausgeführt wird, werden der neue Genre Name und-Wert dem SELECT-Markup hinzugefügt, und das neue Genre wird auf den ausgewählten Wert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="43d8d-157">Da dieses dynamisch generierte Markup ist, wird die neue Auswahl Option nicht angezeigt, indem die Quelle im Browser angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="43d8d-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="43d8d-158">Sie können den neuen HTML-Code mit den IE 9 F12-Entwicklertools anzeigen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="43d8d-159">Drücken Sie zum Anzeigen der neuen Select-Option in Internet Explorer 9 die Taste F12, um die F12-Entwicklertools zu starten.</span><span class="sxs-lookup"><span data-stu-id="43d8d-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="43d8d-160">Navigieren Sie zur Seite erstellen, und fügen Sie ein neues Genre hinzu, damit das neue Genre in der Auswahlliste Genre ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="43d8d-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="43d8d-161">In den F12-Entwicklertools:</span><span class="sxs-lookup"><span data-stu-id="43d8d-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="43d8d-162">Wählen Sie die Registerkarte HTML aus.</span><span class="sxs-lookup"><span data-stu-id="43d8d-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="43d8d-163">Klicken Sie auf das Aktualisierungs Symbol.</span><span class="sxs-lookup"><span data-stu-id="43d8d-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="43d8d-164">Geben Sie im Suchfeld den Suchbegriff GenreID ein.</span><span class="sxs-lookup"><span data-stu-id="43d8d-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="43d8d-165">Mithilfe des nächsten Symbols</span><span class="sxs-lookup"><span data-stu-id="43d8d-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="43d8d-166">Navigieren Sie zum folgenden Tag auswählen:</span><span class="sxs-lookup"><span data-stu-id="43d8d-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="43d8d-167">Erweitern Sie den Wert der letzten Option.</span><span class="sxs-lookup"><span data-stu-id="43d8d-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="43d8d-168">Der folgende Code in der Datei *scripung\choosegenre.js* zeigt, wie die Schaltfläche **neues Genre hinzufügen** mit dem Click-Ereignis verbunden wird und wie das Dialogfeld **neues Genre hinzufügen** erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="43d8d-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="43d8d-169">In der ersten Zeile wird eine Klickfunktion erstellt, die an die Schaltfläche **neues Genre hinzufügen** angeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="43d8d-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="43d8d-170">Das folgende Markup aus der Datei views\storemanager\\_ChooseGenre. cshtml zeigt, wie die Schaltfläche **neues Genre hinzufügen** erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="43d8d-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="43d8d-171">Mit der Load-Methode wird das Dialogfeld Genre hinzufügen erstellt und geöffnet, und die jQuery-`parse`-Methode wird aufgerufen, sodass die Client Validierung für Daten erfolgt, die im Dialog</span><span class="sxs-lookup"><span data-stu-id="43d8d-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="43d8d-172">In diesem Abschnitt haben Sie erfahren, wie Sie ein Dialogfeld erstellen, das verwendet werden kann, um einer Auswahlliste neue Kategoriedaten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="43d8d-173">Sie können das gleiche Verfahren zum Erstellen einer Benutzeroberfläche ausführen, um der Auswahlliste des Künstlers einen neuen Interpreten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="43d8d-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="43d8d-174">Dieses Tutorial bietet einen Überblick über die Arbeit mit der **Dropdown Liste**ASP.NET MVC-HTML-Hilfsobjekt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="43d8d-175">Weitere Informationen zum Arbeiten mit der **Dropdown Liste**finden Sie im Abschnitt Additions Verweise weiter unten.</span><span class="sxs-lookup"><span data-stu-id="43d8d-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="43d8d-176">Teilen Sie uns mit, ob dieses Tutorial hilfreich ist.</span><span class="sxs-lookup"><span data-stu-id="43d8d-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="43d8d-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="43d8d-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="43d8d-178">Zusätzliche Referenzen</span><span class="sxs-lookup"><span data-stu-id="43d8d-178">Additional References</span></span>

- <span data-ttu-id="43d8d-179">[ASP.NET MVC – Cascading Dropdown listet Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) von [Radu enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="43d8d-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="43d8d-180">[Ausgewählt](https://harvesthq.github.com/chosen/) Ein JavaScript-Plug-in, das die Mehrfachauswahl und Filterung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="43d8d-180">[Chosen](https://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="43d8d-181">Mitwirkende</span><span class="sxs-lookup"><span data-stu-id="43d8d-181">Contributors</span></span>

- [<span data-ttu-id="43d8d-182">Radu-Anmeldung</span><span class="sxs-lookup"><span data-stu-id="43d8d-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="43d8d-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="43d8d-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="43d8d-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="43d8d-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="43d8d-185">Prüfer</span><span class="sxs-lookup"><span data-stu-id="43d8d-185">Reviewers</span></span>

- <span data-ttu-id="43d8d-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="43d8d-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="43d8d-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="43d8d-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="43d8d-188">Mike Papst</span><span class="sxs-lookup"><span data-stu-id="43d8d-188">Mike Pope</span></span>
- <span data-ttu-id="43d8d-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="43d8d-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="43d8d-190">Previous</span><span class="sxs-lookup"><span data-stu-id="43d8d-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
