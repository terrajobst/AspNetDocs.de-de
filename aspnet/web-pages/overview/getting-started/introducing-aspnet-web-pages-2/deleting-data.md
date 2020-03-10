---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Einführung in das ASP.net Web Pages Löschen von Datenbankdaten | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie Sie einen einzelnen Datenbankeintrag löschen. Es wird davon ausgegangen, dass Sie die Reihe durch Aktualisieren von Datenbankdaten in ASP.net Web PA abgeschlossen haben...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510459"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="37eb9-104">Einführung in ASP.net Web Pages Löschen von Datenbankdaten</span><span class="sxs-lookup"><span data-stu-id="37eb9-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>

<span data-ttu-id="37eb9-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="37eb9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="37eb9-106">In diesem Tutorial wird gezeigt, wie Sie einen einzelnen Datenbankeintrag löschen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="37eb9-107">Dabei wird davon ausgegangen, dass Sie die Reihe durch Aktualisieren von Daten [Bank Daten in ASP.net Web Pages](updating-data.md)abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="37eb9-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="37eb9-108">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="37eb9-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="37eb9-109">Auswählen eines einzelnen Datensatzes aus einer Auflistung von Datensätzen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="37eb9-110">Löschen eines einzelnen Datensatzes aus einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="37eb9-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="37eb9-111">Überprüfen, ob in einem Formular auf eine bestimmte Schaltfläche geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="37eb9-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="37eb9-112">Erörterte Features und Technologien:</span><span class="sxs-lookup"><span data-stu-id="37eb9-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="37eb9-113">Das `WebGrid`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="37eb9-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="37eb9-114">Der SQL `Delete`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="37eb9-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="37eb9-115">Die `Database.Execute`-Methode, um einen SQL `Delete`-Befehl auszuführen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="37eb9-116">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="37eb9-116">What You'll Build</span></span>

<span data-ttu-id="37eb9-117">Im vorherigen Tutorial haben Sie gelernt, wie ein vorhandener Daten Bank Datensatz aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="37eb9-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="37eb9-118">Dieses Lernprogramm ist ähnlich, mit der Ausnahme, dass Sie es löschen, anstatt den Datensatz zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="37eb9-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="37eb9-119">Die Prozesse sind sehr identisch, mit dem Unterschied, dass das Löschen einfacher ist, sodass dieses Tutorial kurz ist.</span><span class="sxs-lookup"><span data-stu-id="37eb9-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="37eb9-120">Auf der Seite " *Filme* " Aktualisieren Sie das `WebGrid` Hilfsprogramm, sodass neben den einzelnen Filmen ein **Lösch** Link angezeigt wird, der dem zuvor hinzugefügten **Bearbeitungs** Link entspricht.</span><span class="sxs-lookup"><span data-stu-id="37eb9-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Seite "Filme", die einen Lösch Link für jeden Film anzeigt](deleting-data/_static/image1.png)

<span data-ttu-id="37eb9-122">Beim Klicken auf den Link **Löschen** gelangen Sie wie bei der Bearbeitung zu einer anderen Seite, auf der sich die Filminformationen bereits in einem Formular befinden:</span><span class="sxs-lookup"><span data-stu-id="37eb9-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Movie Page mit angezeigter Film löschen](deleting-data/_static/image2.png)

<span data-ttu-id="37eb9-124">Sie können dann auf die Schaltfläche klicken, um den Datensatz dauerhaft zu löschen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="37eb9-125">Hinzufügen eines Lösch Links zur Filmliste</span><span class="sxs-lookup"><span data-stu-id="37eb9-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="37eb9-126">Sie beginnen mit dem Hinzufügen eines **Lösch** Links zum `WebGrid`-Hilfsprogramm.</span><span class="sxs-lookup"><span data-stu-id="37eb9-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="37eb9-127">Dieser Link ähnelt dem Link **Bearbeiten** , den Sie in einem vorherigen Tutorial hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="37eb9-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="37eb9-128">Öffnen Sie die Datei *Movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="37eb9-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="37eb9-129">Ändern Sie das `WebGrid` Markup im Text der Seite, indem Sie eine Spalte hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="37eb9-130">Hier ist das geänderte Markup:</span><span class="sxs-lookup"><span data-stu-id="37eb9-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="37eb9-131">Die neue Spalte lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="37eb9-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="37eb9-132">Die Art und Weise, in der das Raster konfiguriert ist, ist die Spalte **Bearbeiten** im Raster ganz links, und die Spalte **Löschen** ist ganz rechts.</span><span class="sxs-lookup"><span data-stu-id="37eb9-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="37eb9-133">(Es gibt jetzt ein Komma hinter der Spalte "`Year`", falls Sie dies nicht bemerkt haben.) Es gibt nichts besonderes, wo diese Link Spalten zu finden sind, und Sie können Sie einfach nebeneinander platzieren.</span><span class="sxs-lookup"><span data-stu-id="37eb9-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="37eb9-134">In diesem Fall sind Sie getrennt, damit Sie nicht mehr gemischt werden können.</span><span class="sxs-lookup"><span data-stu-id="37eb9-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Seite "Filme" mit den Links "Bearbeiten" und "Details" gekennzeichnet, um anzuzeigen, dass Sie nicht nebeneinander sind](deleting-data/_static/image3.png)

<span data-ttu-id="37eb9-136">Die neue Spalte zeigt einen Link (`<a>`-Element) an, dessen Text "Delete" lautet.</span><span class="sxs-lookup"><span data-stu-id="37eb9-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="37eb9-137">Das Ziel des Links (dessen `href`-Attribut) ist Code, der letztendlich in etwa diese URL aufgelöst wird, wobei der `id` Wert für jeden Film anders ist:</span><span class="sxs-lookup"><span data-stu-id="37eb9-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="37eb9-138">Mit diesem Link wird eine Seite mit dem Namen *deletemovie* aufgerufen und die ID des von Ihnen ausgewählten Films übergeben.</span><span class="sxs-lookup"><span data-stu-id="37eb9-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="37eb9-139">In diesem Tutorial wird nicht ausführlich erläutert, wie dieser Link erstellt wird, da er fast identisch mit dem **Bearbeitungs** Link aus dem vorherigen Tutorial ist ([Aktualisieren von Datenbankdaten in ASP.net Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="37eb9-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="37eb9-140">Erstellen der Seite "Löschen"</span><span class="sxs-lookup"><span data-stu-id="37eb9-140">Creating the Delete Page</span></span>

<span data-ttu-id="37eb9-141">Nun können Sie die Seite erstellen, die als Ziel für den **Lösch** Link im Raster verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="37eb9-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="37eb9-142">**Wichtig** Das Verfahren zum ersten auswählen eines zu löschenden Datensatzes und zum anschließenden Verwenden einer separaten Seite und Schaltfläche, um den Prozess zu bestätigen, ist für die Sicherheit äußerst wichtig.</span><span class="sxs-lookup"><span data-stu-id="37eb9-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="37eb9-143">Wie Sie bereits in den vorherigen Tutorials gelesen haben *, sollten Sie* eine *beliebige* Änderung an Ihrer Website durchführen, indem Sie eine Form &mdash;, die einen HTTP Post-Vorgang verwendet.</span><span class="sxs-lookup"><span data-stu-id="37eb9-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="37eb9-144">Wenn Sie den Standort durch Klicken auf einen Link (d. h. mithilfe eines Get-Vorgangs) ändern können, können die Benutzer einfache Anforderungen an Ihre Website senden und die Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="37eb9-145">Auch ein Suchengine-Crawler, der Ihre Website indiziert, könnte versehentlich Daten durch die folgenden Links löschen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="37eb9-146">Wenn Ihre APP Benutzern das Ändern eines Datensatzes ermöglicht, müssen Sie den Datensatz für die Bearbeitung auf jeden Fall an den Benutzer übernehmen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="37eb9-147">Möglicherweise ist es aber verlockend, diesen Schritt zum Löschen eines Datensatzes zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="37eb9-148">Überspringen Sie diesen Schritt jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="37eb9-148">Don't skip that step, though.</span></span> <span data-ttu-id="37eb9-149">(Außerdem ist es für Benutzer hilfreich, den Datensatz anzuzeigen und zu bestätigen, dass der von Ihnen beabsichtigte Datensatz gelöscht wird.)</span><span class="sxs-lookup"><span data-stu-id="37eb9-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="37eb9-150">In einem nachfolgenden Tutorial finden Sie Informationen zum Hinzufügen von Anmelde Funktionen, damit sich ein Benutzer vor dem Löschen eines Datensatzes anmelden muss.</span><span class="sxs-lookup"><span data-stu-id="37eb9-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>

<span data-ttu-id="37eb9-151">Erstellen Sie eine Seite mit dem Namen *deletemovie. cshtml* , und ersetzen Sie die Elemente in der Datei durch das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="37eb9-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="37eb9-152">Dieses Markup ähnelt den *editmovie* -Seiten, mit dem Unterschied, dass das Markup `<span>` Elemente enthält, anstatt Textfelder (`<input type="text">`) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="37eb9-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="37eb9-153">Es gibt nichts, was bearbeitet werden muss.</span><span class="sxs-lookup"><span data-stu-id="37eb9-153">There's nothing here to edit.</span></span> <span data-ttu-id="37eb9-154">Sie müssen lediglich die Filmdetails anzeigen, damit Benutzer sicherstellen können, dass Sie den richtigen Film löschen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="37eb9-155">Das Markup enthält bereits einen Link, mit dem der Benutzer zur filmauflistungs Seite zurückkehren kann.</span><span class="sxs-lookup"><span data-stu-id="37eb9-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="37eb9-156">Wie auf der Seite *editmovie* wird die ID des ausgewählten Films in einem ausgeblendeten Feld gespeichert.</span><span class="sxs-lookup"><span data-stu-id="37eb9-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="37eb9-157">(Er wird an die Seite an erster Stelle als Abfrage Zeichenfolge-Wert an die Seite geleitet.) Es gibt einen `Html.ValidationSummary`-Befehl, mit dem Validierungs Fehler angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="37eb9-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="37eb9-158">In diesem Fall könnte der Fehler darin bestehen, dass keine Film-ID an die Seite oder die Film-ID ungültig ist.</span><span class="sxs-lookup"><span data-stu-id="37eb9-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="37eb9-159">Diese Situation kann eintreten, wenn jemand diese Seite ausgeführt hat, ohne zuvor einen Film auf der Seite " *Filme* " auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="37eb9-160">Die Schaltflächen Beschriftung lautet **Delete Movie**, und das Name-Attribut ist auf `buttonDelete`festgelegt.</span><span class="sxs-lookup"><span data-stu-id="37eb9-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="37eb9-161">Das `name`-Attribut wird im Code verwendet, um die Schaltfläche zu identifizieren, die das Formular übermittelt hat.</span><span class="sxs-lookup"><span data-stu-id="37eb9-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="37eb9-162">Sie müssen Code in 1 schreiben) lesen Sie die Filmdetails, wenn die Seite zum ersten Mal angezeigt wird, und 2), und löschen Sie den Film, wenn der Benutzer auf die Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="37eb9-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="37eb9-163">Hinzufügen von Code zum Lesen eines einzelnen Films</span><span class="sxs-lookup"><span data-stu-id="37eb9-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="37eb9-164">Fügen Sie am oberen Rand der Seite *deletemovie. cshtml* den folgenden Codeblock hinzu:</span><span class="sxs-lookup"><span data-stu-id="37eb9-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="37eb9-165">Dieses Markup ist mit dem entsprechenden Code auf der Seite *editmovie* identisch.</span><span class="sxs-lookup"><span data-stu-id="37eb9-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="37eb9-166">Er ruft die Film-ID aus der Abfrage Zeichenfolge ab und verwendet die ID zum Lesen eines Datensatzes aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="37eb9-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="37eb9-167">Der Code enthält den Validierungstest (`IsInt()` und `row != null`), um sicherzustellen, dass die an die Seite übergebenen Movie-ID gültig ist.</span><span class="sxs-lookup"><span data-stu-id="37eb9-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="37eb9-168">Beachten Sie, dass dieser Code nur bei der erstmaligen Ausführung der Seite ausgeführt werden sollte.</span><span class="sxs-lookup"><span data-stu-id="37eb9-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="37eb9-169">Sie möchten den Film Daten Satz nicht erneut aus der Datenbank lesen, wenn der Benutzer auf die Schaltfläche " **Film löschen** " klickt.</span><span class="sxs-lookup"><span data-stu-id="37eb9-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="37eb9-170">Daher befindet sich der Code zum Lesen des Films in einem Test, der `if(!IsPost)` &mdash; heißt, *Wenn die Anforderung kein Post-Vorgang ist (Formular Übermittlung)* .</span><span class="sxs-lookup"><span data-stu-id="37eb9-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="37eb9-171">Hinzufügen von Code zum Löschen des ausgewählten Films</span><span class="sxs-lookup"><span data-stu-id="37eb9-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="37eb9-172">Wenn Sie den Film löschen möchten, wenn der Benutzer auf die Schaltfläche klickt, fügen Sie den folgenden Code direkt innerhalb der schließenden geschweiften Klammer des `@`-Blocks ein:</span><span class="sxs-lookup"><span data-stu-id="37eb9-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="37eb9-173">Dieser Code ähnelt dem Code zum Aktualisieren eines vorhandenen Datensatzes, aber einfacher.</span><span class="sxs-lookup"><span data-stu-id="37eb9-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="37eb9-174">Der Code führt im Grunde eine SQL `Delete`-Anweisung aus.</span><span class="sxs-lookup"><span data-stu-id="37eb9-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="37eb9-175">Wie auf der *editmovie* -Seite befindet sich der Code in einem `if(IsPost)`-Block.</span><span class="sxs-lookup"><span data-stu-id="37eb9-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="37eb9-176">Dieses Mal ist die `if()` Bedingung etwas komplizierter:</span><span class="sxs-lookup"><span data-stu-id="37eb9-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="37eb9-177">Hier gibt es zwei Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-177">There are two conditions here.</span></span> <span data-ttu-id="37eb9-178">Der erste besteht darin, dass die Seite übermittelt wird, wie Sie vor &mdash; `if(IsPost)`gesehen haben.</span><span class="sxs-lookup"><span data-stu-id="37eb9-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="37eb9-179">Die zweite Bedingung ist `!Request["buttonDelete"].IsEmpty()`. Dies bedeutet, dass die Anforderung über ein Objekt mit dem Namen `buttonDelete`verfügt.</span><span class="sxs-lookup"><span data-stu-id="37eb9-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="37eb9-180">Zugegeben, es ist eine indirekte Methode, um zu testen, welche Schaltfläche das Formular übermittelt hat.</span><span class="sxs-lookup"><span data-stu-id="37eb9-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="37eb9-181">Wenn ein Formular mehrere Sende Schaltflächen enthält, wird nur der Name der Schaltfläche, auf die geklickt wurde, in der Anforderung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="37eb9-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="37eb9-182">Wenn der Name einer bestimmten Schaltfläche in der Anforderung &mdash; oder wie im Code angegeben erscheint, ist diese Schaltfläche logisch, wenn diese Schaltfläche nicht leer ist &mdash; das ist die Schaltfläche, die das Formular übermittelt hat.</span><span class="sxs-lookup"><span data-stu-id="37eb9-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="37eb9-183">Der `&&`-Operator bedeutet "and" (logisches and).</span><span class="sxs-lookup"><span data-stu-id="37eb9-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="37eb9-184">Daher ist die gesamte `if` Bedingung...</span><span class="sxs-lookup"><span data-stu-id="37eb9-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="37eb9-185">*Diese Anforderung ist ein Post (keine erstmalige Anforderung).*</span><span class="sxs-lookup"><span data-stu-id="37eb9-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="37eb9-186">AND</span><span class="sxs-lookup"><span data-stu-id="37eb9-186">AND</span></span>  
  
<span data-ttu-id="37eb9-187">*Die Schaltfläche `buttonDelete`* *war die Schaltfläche, die das Formular übermittelt hat.*</span><span class="sxs-lookup"><span data-stu-id="37eb9-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="37eb9-188">Dieses Formular (auf dieser Seite) enthält nur eine Schaltfläche, sodass der zusätzliche Test für `buttonDelete` technisch nicht erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="37eb9-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="37eb9-189">Dennoch sind Sie im Begriff, einen Vorgang durchzuführen, mit dem Daten dauerhaft entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="37eb9-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="37eb9-190">Daher sollten Sie so sicher wie möglich sein, dass Sie den Vorgang nur ausführen, wenn der Benutzer ihn explizit angefordert hat.</span><span class="sxs-lookup"><span data-stu-id="37eb9-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="37eb9-191">Nehmen Sie beispielsweise an, dass Sie diese Seite später erweitert und ihr weitere Schaltflächen hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="37eb9-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="37eb9-192">Selbst dann wird der Code, der den Film löscht, nur dann ausgeführt, wenn auf die Schaltfläche "`buttonDelete`" geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="37eb9-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="37eb9-193">Wie auf der Seite *editmovie* erhalten Sie die ID aus dem ausgeblendeten Feld und führen dann den SQL-Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="37eb9-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="37eb9-194">Die Syntax für die `Delete`-Anweisung lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="37eb9-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="37eb9-195">Es ist wichtig, dass Sie die `WHERE`-Klausel und die ID einschließen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="37eb9-196">Wenn Sie die WHERE-Klausel weglassen, *werden alle Datensätze in der Tabelle gelöscht*.</span><span class="sxs-lookup"><span data-stu-id="37eb9-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="37eb9-197">Wie Sie gesehen haben, übergeben Sie den ID-Wert an den SQL-Befehl, indem Sie einen Platzhalter verwenden.</span><span class="sxs-lookup"><span data-stu-id="37eb9-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="37eb9-198">Testen des Film Löschvorgangs</span><span class="sxs-lookup"><span data-stu-id="37eb9-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="37eb9-199">Jetzt können Sie testen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-199">Now you can test.</span></span> <span data-ttu-id="37eb9-200">Führen Sie die Seite *Filme* aus, und klicken Sie auf **Löschen** neben einem Film.</span><span class="sxs-lookup"><span data-stu-id="37eb9-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="37eb9-201">Wenn die Seite *deletemovie* angezeigt wird, klicken Sie auf **Film löschen**.</span><span class="sxs-lookup"><span data-stu-id="37eb9-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Löschen der Filmseite mit hervorgehobener Schaltfläche "Movie](deleting-data/_static/image4.png)

<span data-ttu-id="37eb9-203">Wenn Sie auf die Schaltfläche klicken, löscht der Code die Filme und kehrt zur Filmliste zurück.</span><span class="sxs-lookup"><span data-stu-id="37eb9-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="37eb9-204">Dort können Sie nach dem gelöschten Film suchen und sich vergewissern, dass er gelöscht wurde.</span><span class="sxs-lookup"><span data-stu-id="37eb9-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="37eb9-205">Nächste nächste</span><span class="sxs-lookup"><span data-stu-id="37eb9-205">Coming Up Next</span></span>

<span data-ttu-id="37eb9-206">Das nächste Tutorial zeigt, wie Sie allen Seiten auf Ihrer Website ein gängiges Aussehen und Layout zuordnen.</span><span class="sxs-lookup"><span data-stu-id="37eb9-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="37eb9-207">Vervollständigen der Auflistung für Movie Page (aktualisiert mit Links zum Löschen)</span><span class="sxs-lookup"><span data-stu-id="37eb9-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="37eb9-208">Vervollständigen der Liste für die deletemovie-Seite</span><span class="sxs-lookup"><span data-stu-id="37eb9-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="37eb9-209">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="37eb9-209">Additional Resources</span></span>

- [<span data-ttu-id="37eb9-210">Einführung in die ASP.net-Webprogrammierung mithilfe der Razor-Syntax</span><span class="sxs-lookup"><span data-stu-id="37eb9-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="37eb9-211">[SQL DELETE-Anweisung](http://www.w3schools.com/sql/sql_delete.asp) auf der W3Schools-Website</span><span class="sxs-lookup"><span data-stu-id="37eb9-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37eb9-212">[Zurück](updating-data.md)
> [Weiter](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="37eb9-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
