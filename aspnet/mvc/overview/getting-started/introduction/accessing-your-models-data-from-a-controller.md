---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Zugreifen auf die Daten des Modells von einem Controller | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: e01953dcfb2abf2db53a8aa869aa75b40485daca
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519088"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="5b146-102">Zugreifen auf Modelldaten anhand eines Controllers</span><span class="sxs-lookup"><span data-stu-id="5b146-102">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="5b146-103">von [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="5b146-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="5b146-104">In diesem Abschnitt erstellen Sie eine neue `MoviesController`-Klasse und schreiben Code, mit dem die Filmdaten abgerufen und im Browser mithilfe einer Ansichts Vorlage angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5b146-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="5b146-105">**Erstellen Sie die Anwendung** , und fahren Sie mit dem nächsten Schritt fort.</span><span class="sxs-lookup"><span data-stu-id="5b146-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="5b146-106">Wenn Sie die Anwendung nicht erstellen, erhalten Sie einen Fehler beim Hinzufügen eines Controllers.</span><span class="sxs-lookup"><span data-stu-id="5b146-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="5b146-107">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller* , und klicken Sie dann auf **Hinzufügen**und dann auf **Controller**</span><span class="sxs-lookup"><span data-stu-id="5b146-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="5b146-108">Klicken Sie im Dialogfeld **Gerüst hinzufügen** auf **MVC 5-Controller mit Ansichten, verwenden Sie Entity Framework**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b146-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="5b146-109">Wählen Sie **Movie (mvcmovie. Models)** für die Modell Klasse aus.</span><span class="sxs-lookup"><span data-stu-id="5b146-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="5b146-110">Wählen Sie für die Datenkontext Klasse die Option " **wviedbcontext (mvcmovie. Models)** " aus.</span><span class="sxs-lookup"><span data-stu-id="5b146-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="5b146-111">Geben Sie für den Controller Namen den Namen **moviescontroller**ein.</span><span class="sxs-lookup"><span data-stu-id="5b146-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="5b146-112">In der folgenden Abbildung wird das Dialogfeld abgeschlossen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b146-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="5b146-113">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b146-113">Click **Add**.</span></span> <span data-ttu-id="5b146-114">(Wenn Sie eine Fehlermeldung erhalten, haben Sie die Anwendung wahrscheinlich nicht erstellt, bevor Sie mit dem Hinzufügen des Controllers begonnen haben.) Visual Studio erstellt die folgenden Dateien und Ordner:</span><span class="sxs-lookup"><span data-stu-id="5b146-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="5b146-115">*Eine MoviesController.cs* -Datei im *Controller* Ordner.</span><span class="sxs-lookup"><span data-stu-id="5b146-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="5b146-116">Ein Ordner " *views\movies* ".</span><span class="sxs-lookup"><span data-stu-id="5b146-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="5b146-117">" *Create. cshtml", "Delete. cshtml", "Details. cshtml", "Edit. cshtml*" und " *Index. cshtml* " im neuen Ordner " *views\movies* ".</span><span class="sxs-lookup"><span data-stu-id="5b146-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="5b146-118">Visual Studio erstellte automatisch die [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) -Aktionsmethoden und-Sichten (Create, Read, Update und DELETE) für Sie (die automatische Erstellung von CRUD-Aktionsmethoden und-Sichten wird als Gerüstbau bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="5b146-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="5b146-119">Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit der Sie Film Einträge erstellen, auflisten, bearbeiten und löschen können.</span><span class="sxs-lookup"><span data-stu-id="5b146-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="5b146-120">Führen Sie die Anwendung aus, und klicken Sie auf den Link **MVC Movie** (oder navigieren Sie zum `Movies` Controller, indem Sie */Movies* an die URL in der Adressleiste Ihres Browsers Anhängen).</span><span class="sxs-lookup"><span data-stu-id="5b146-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="5b146-121">Da die Anwendung auf dem Standard Routing basiert (definiert in der *App\_start\routeconfig.cs* -Datei), wird die Browser Anforderungs `http://localhost:xxxxx/Movies` an die Standard `Index` Aktionsmethode des `Movies` Controllers weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="5b146-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="5b146-122">Das heißt, die Browser Anforderung `http://localhost:xxxxx/Movies` ist praktisch identisch mit der Browser Anforderung `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="5b146-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="5b146-123">Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="5b146-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="5b146-124">Erstellen eines Films</span><span class="sxs-lookup"><span data-stu-id="5b146-124">Creating a Movie</span></span>

<span data-ttu-id="5b146-125">Klicken Sie auf den Link **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5b146-125">Select the **Create New** link.</span></span> <span data-ttu-id="5b146-126">Geben Sie einige Details zu einem Film ein, und klicken Sie dann auf die Schaltfläche **Erstellen** .</span><span class="sxs-lookup"><span data-stu-id="5b146-126">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="5b146-127">Sie können möglicherweise keine Dezimaltrennzeichen oder Kommas in das Feld Price eingeben.</span><span class="sxs-lookup"><span data-stu-id="5b146-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="5b146-128">Um die jQuery-Validierung für nicht englische Gebiets Schemas zu unterstützen, die ein Komma (&quot;&quot;) für einen Dezimaltrennzeichen und nicht-US-englische Datumsformate verwenden, müssen Sie *Globalize. js* und ihre spezifische *Kulturen/Globalize. Cultures. js* -Datei (von [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) und JavaScript einschließen, um `Globalize.parseFloat` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="5b146-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="5b146-129">Im nächsten Tutorial zeige ich Ihnen, wie dies geschieht.</span><span class="sxs-lookup"><span data-stu-id="5b146-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="5b146-130">Geben Sie einstweilen ganze Zahlen wie 10 ein.</span><span class="sxs-lookup"><span data-stu-id="5b146-130">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="5b146-131">Wenn Sie auf die Schaltfläche **Erstellen** klicken, wird das Formular an den Server gesendet, auf dem die Filminformationen in der Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="5b146-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="5b146-132">Sie werden dann zur URL */Movies* umgeleitet, auf der Sie den neu erstellten Film in der Auflistung sehen können.</span><span class="sxs-lookup"><span data-stu-id="5b146-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="5b146-133">Erstellen Sie ein paar weitere Filmeinträge.</span><span class="sxs-lookup"><span data-stu-id="5b146-133">Create a couple more movie entries.</span></span> <span data-ttu-id="5b146-134">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.</span><span class="sxs-lookup"><span data-stu-id="5b146-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="5b146-135">Untersuchen des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="5b146-135">Examining the Generated Code</span></span>

<span data-ttu-id="5b146-136">Öffnen Sie die Datei *controllers\moviescontroller.cs* , und untersuchen Sie die generierte `Index`-Methode.</span><span class="sxs-lookup"><span data-stu-id="5b146-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="5b146-137">Unten wird ein Teil des Movie-Controllers mit der `Index`-Methode angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b146-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="5b146-138">Eine Anforderung an den `Movies` Controller gibt alle Einträge in der `Movies` Tabelle zurück und übergibt die Ergebnisse an die `Index` Sicht.</span><span class="sxs-lookup"><span data-stu-id="5b146-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="5b146-139">Die folgende Zeile aus der `MoviesController`-Klasse instanziiert einen Film Daten Bank Kontext, wie zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="5b146-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="5b146-140">Sie können den Film Daten Bank Kontext verwenden, um Filme abzufragen, zu bearbeiten und zu löschen.</span><span class="sxs-lookup"><span data-stu-id="5b146-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="5b146-141">Stark typisierte Modelle und das @model-Schlüsselwort</span><span class="sxs-lookup"><span data-stu-id="5b146-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="5b146-142">An früherer Stelle in diesem Tutorial haben Sie erfahren, wie ein Controllerdaten oder Objekte mithilfe des `ViewBag` Objekts an eine Ansichts Vorlage übergeben kann.</span><span class="sxs-lookup"><span data-stu-id="5b146-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="5b146-143">Der `ViewBag` ist ein dynamisches Objekt, das eine bequeme, spät gebundene Methode zum Übergeben von Informationen an eine Ansicht bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="5b146-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="5b146-144">MVC bietet auch die Möglichkeit, *stark* typisierte Objekte an eine Ansichts Vorlage zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="5b146-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="5b146-145">Dieser stark typisierte Ansatz ermöglicht eine bessere Kompilierzeit Überprüfung Ihres Codes und umfangreicheres [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) im Visual Studio-Editor.</span><span class="sxs-lookup"><span data-stu-id="5b146-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="5b146-146">Der Gerüstbau Mechanismus in Visual Studio verwendet diesen Ansatz (d. h. die Übergabe eines *stark* typisierten Modells) mit der `MoviesController`-Klasse und Anzeige Vorlagen, als die Methoden und Sichten erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="5b146-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="5b146-147">Untersuchen Sie in der Datei " *controllers\moviescontroller.cs* " die generierte `Details`-Methode.</span><span class="sxs-lookup"><span data-stu-id="5b146-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="5b146-148">Die `Details`-Methode ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="5b146-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="5b146-149">Der `id`-Parameter wird im Allgemeinen als Routendaten übergeben, z. b. `http://localhost:1234/movies/details/1` legt den Controller auf den Movie-Controller, die Aktion zum `details` und die `id` auf 1 fest.</span><span class="sxs-lookup"><span data-stu-id="5b146-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="5b146-150">Außerdem können Sie die ID wie folgt mit einer Abfrage Zeichenfolge übergeben:</span><span class="sxs-lookup"><span data-stu-id="5b146-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="5b146-151">Wenn eine `Movie` gefunden wird, wird eine Instanz des `Movie` Modells an die `Details` Ansicht übermittelt:</span><span class="sxs-lookup"><span data-stu-id="5b146-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="5b146-152">Überprüfen Sie den Inhalt der Datei " *views\movies\details.cshtml* ":</span><span class="sxs-lookup"><span data-stu-id="5b146-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="5b146-153">Durch Einschließen einer `@model`-Anweisung am Anfang der Ansichts Vorlagen Datei können Sie den Objekttyp angeben, den die Sicht erwartet.</span><span class="sxs-lookup"><span data-stu-id="5b146-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="5b146-154">Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="5b146-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="5b146-155">Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir.</span><span class="sxs-lookup"><span data-stu-id="5b146-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="5b146-156">Beispielsweise übergibt der Code in der " *Details. cshtml* "-Vorlage jedes filmfeld an den `DisplayNameFor`-und [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) -HTML-Hilfsprogramm mit dem stark typisierten `Model`-Objekt.</span><span class="sxs-lookup"><span data-stu-id="5b146-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="5b146-157">Die `Create`-und `Edit`-Methoden und-Ansichts Vorlagen übergeben auch ein Movie Model-Objekt.</span><span class="sxs-lookup"><span data-stu-id="5b146-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="5b146-158">Überprüfen Sie die Ansichts Vorlage *Index. cshtml* und die `Index`-Methode in der Datei *MoviesController.cs* .</span><span class="sxs-lookup"><span data-stu-id="5b146-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="5b146-159">Beachten Sie, wie der Code beim Aufrufen der `View` Helper-Methode in der `Index` Action-Methode ein [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) -Objekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="5b146-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="5b146-160">Der Code übergibt dann diese `Movies` Liste von der `Index` Aktionsmethode an die Ansicht:</span><span class="sxs-lookup"><span data-stu-id="5b146-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="5b146-161">Beim Erstellen des Film Controllers enthielt Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei " *Index. cshtml* ":</span><span class="sxs-lookup"><span data-stu-id="5b146-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="5b146-162">Diese `@model` Direktive ermöglicht Ihnen den Zugriff auf die Liste der Filme, die der Controller an die Ansicht übermittelt hat, indem er ein `Model` Objekt verwendet, das stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="5b146-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="5b146-163">In der Vorlage *Index. cshtml* durchläuft der Code z. b. die Filme durch Ausführen einer `foreach`-Anweisung über das stark typisierte `Model` Objekt:</span><span class="sxs-lookup"><span data-stu-id="5b146-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="5b146-164">Da das `Model` Objekt stark typisiert ist (als `IEnumerable<Movie>`-Objekt), wird jedes `item`-Objekt in der Schleife als `Movie`typisiert.</span><span class="sxs-lookup"><span data-stu-id="5b146-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="5b146-165">Dies bedeutet neben anderen Vorteilen, dass Sie die Kompilierzeit Überprüfung des Codes und die vollständige IntelliSense-Unterstützung im Code-Editor erhalten:</span><span class="sxs-lookup"><span data-stu-id="5b146-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![Modelintellisense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="5b146-167">Arbeiten mit SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="5b146-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="5b146-168">Entity Framework Code First festgestellt, dass die angegebene Daten bankverbindungs Zeichenfolge auf eine `Movies` Datenbank verweist, die noch nicht vorhanden war, sodass Code First die Datenbank automatisch erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="5b146-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="5b146-169">Sie können überprüfen, ob er erstellt wurde, indem Sie im Ordner *App\_Data*</span><span class="sxs-lookup"><span data-stu-id="5b146-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="5b146-170">Wenn die Datei *Movies. mdf* nicht angezeigt wird, klicken Sie auf der **Projektmappen-Explorer** Symbolleiste auf die Schaltfläche **alle Dateien anzeigen** , klicken Sie auf die Schaltfläche **Aktualisieren** , und erweitern Sie dann den Ordner *App\_Data* .</span><span class="sxs-lookup"><span data-stu-id="5b146-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="5b146-171">Doppelklicken Sie auf *Movies. mdf* , um den **Server-Explorer**zu öffnen, und erweitern Sie dann den Ordner **Tabellen** , um die Tabelle Movies anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5b146-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="5b146-172">Beachten Sie das Schlüsselsymbol neben ID.</span><span class="sxs-lookup"><span data-stu-id="5b146-172">Note the key icon next to ID.</span></span> <span data-ttu-id="5b146-173">Standardmäßig wird von EF eine Eigenschaft mit dem Namen ID als Primärschlüssel erstellt.</span><span class="sxs-lookup"><span data-stu-id="5b146-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="5b146-174">Weitere Informationen zu EF und MVC finden Sie im hervorragenden Tutorial zu Tom Dykstra in [MVC und EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="5b146-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="5b146-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="5b146-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="5b146-176">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movies`, und wählen Sie **Tabellendaten anzeigen** aus, um die erstellten Daten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5b146-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="5b146-177">Klicken Sie mit der rechten Maustaste auf die Tabelle `Movies`, und wählen Sie **Tabellen Definition öffnen** aus, um die Tabellenstruktur anzuzeigen, die Entity Framework für Sie erstellt Code First.</span><span class="sxs-lookup"><span data-stu-id="5b146-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="5b146-178">Beachten Sie, dass das Schema der `Movies` Tabelle der `Movie` Klasse zugeordnet ist, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="5b146-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="5b146-179">Entity Framework Code First dieses Schema basierend auf Ihrer `Movie`-Klasse automatisch für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="5b146-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="5b146-180">Wenn Sie fertig sind, schließen Sie die Verbindung, indem Sie mit der rechten Maustaste auf " *muviedbcontext* " klicken und **Verbindung schließen**auswählen</span><span class="sxs-lookup"><span data-stu-id="5b146-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="5b146-181">(Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie das Projekt das nächste Mal ausführen).</span><span class="sxs-lookup"><span data-stu-id="5b146-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

<span data-ttu-id="5b146-182">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Dateien.</span><span class="sxs-lookup"><span data-stu-id="5b146-182">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="5b146-183">Im nächsten Tutorial untersuchen wir den restlichen Code und fügen eine `SearchIndex` Methode und eine `SearchIndex` Ansicht hinzu, mit der Sie in dieser Datenbank nach Filmen suchen können.</span><span class="sxs-lookup"><span data-stu-id="5b146-183">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="5b146-184">Weitere Informationen zur Verwendung von Entity Framework mit MVC finden Sie unter [Erstellen eines Entity Framework Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="5b146-184">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5b146-185">[Zurück](creating-a-connection-string.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="5b146-185">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
