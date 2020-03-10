---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Hinzufügen eines neuen Felds zum Film Modell und zur Tabelle | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: eine aktualisierte Version dieses Tutorials ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, aber viel einfacher zu befolgen und zu demonstrieren...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498513"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="ea40c-104">Hinzufügen eines neuen Felds zum Modell und zur Tabelle eines Films</span><span class="sxs-lookup"><span data-stu-id="ea40c-104">Adding a New Field to the Movie Model and Table</span></span>

<span data-ttu-id="ea40c-105">von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea40c-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ea40c-106">Eine aktualisierte Version dieses Tutorials ist [hier](../../getting-started/introduction/getting-started.md) verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea40c-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ea40c-107">Es ist sicherer, ist viel einfacher zu befolgen und zeigt mehr Features.</span><span class="sxs-lookup"><span data-stu-id="ea40c-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="ea40c-108">In diesem Abschnitt verwenden Sie Entity Framework Code First-Migrationen, um einige Änderungen an den Modellklassen zu migrieren, sodass die Änderung auf die Datenbank angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="ea40c-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="ea40c-109">Wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank zu erstellen, wie in diesem Tutorial bereits erwähnt, fügt Code First der Datenbank eine Tabelle hinzu, um zu verfolgen, ob das Schema der Datenbank mit den Modellklassen synchronisiert ist, von denen es generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="ea40c-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="ea40c-110">Wenn Sie nicht synchron sind, löst die Entity Framework einen Fehler aus.</span><span class="sxs-lookup"><span data-stu-id="ea40c-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="ea40c-111">Dies vereinfacht das Auffinden von Problemen bei der Entwicklung, die Sie möglicherweise zur Laufzeit nur (aufgrund von unkomplizierter Fehlern) finden.</span><span class="sxs-lookup"><span data-stu-id="ea40c-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="ea40c-112">Einrichten von Code First-Migrationen für Modelländerungen</span><span class="sxs-lookup"><span data-stu-id="ea40c-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="ea40c-113">Wenn Sie Visual Studio 2012 verwenden, doppelklicken Sie auf die Datei *Movies. mdf* von Projektmappen-Explorer, um das Daten Bank Tool zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="ea40c-114">Visual Studio Express für das Web Datenbank-Explorer angezeigt wird, wird in Visual Studio 2012 Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="ea40c-115">Wenn Sie Visual Studio 2010 verwenden, verwenden Sie SQL Server-Objekt-Explorer.</span><span class="sxs-lookup"><span data-stu-id="ea40c-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="ea40c-116">Klicken Sie im Daten Bank Tool (Datenbank-Explorer, Server-Explorer oder SQL Server-Objekt-Explorer) mit der rechten Maustaste auf `MovieDBContext`, und wählen Sie löschen aus, um die Filme-Datenbank zu **Löschen** .</span><span class="sxs-lookup"><span data-stu-id="ea40c-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="ea40c-117">Navigieren Sie zurück zu Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="ea40c-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="ea40c-118">Klicken Sie mit der rechten Maustaste auf die Datei " *Movies. mdf* ", und wählen Sie **Löschen** aus, um die Datei</span><span class="sxs-lookup"><span data-stu-id="ea40c-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="ea40c-119">Erstellen Sie die Anwendung, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="ea40c-120">Klicken Sie im Menü **Extras** auf **NuGet-Paket-Manager** und dann auf **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="ea40c-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Pack-Mann hinzufügen](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="ea40c-122">Geben Sie im Fenster der **Paket-Manager-Konsole** an der `PM>` Eingabeaufforderung "Enable-Migrations-contexttyk-Name mvcmovie. Models. muviedbcontext" ein.</span><span class="sxs-lookup"><span data-stu-id="ea40c-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="ea40c-123">Mit dem Befehl **enable-Migrationen** (siehe oben) wird eine *Configuration.cs* -Datei in einem neuen *Migrations* Ordner erstellt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="ea40c-124">Visual Studio öffnet die Datei *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="ea40c-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="ea40c-125">Ersetzen Sie die `Seed`-Methode in der Datei *Configuration.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ea40c-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="ea40c-126">Klicken Sie unter **`Movie` mit der** rechten Maustaste auf die rote Wellenlinie, und wählen Sie dann **Auflösen** und dann **mvcmovie. Models aus.**</span><span class="sxs-lookup"><span data-stu-id="ea40c-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="ea40c-127">Dadurch wird die folgende using-Anweisung hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ea40c-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="ea40c-128">Code First-Migrationen wird die `Seed`-Methode nach jeder Migration aufgerufen (d. h., **Update-Database** wird in der Paket-Manager-Konsole aufgerufen), und diese Methode aktualisiert Zeilen, die bereits eingefügt wurden, oder fügt Sie ein, wenn Sie noch nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="ea40c-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>

<span data-ttu-id="ea40c-129">**Drücken Sie STRG + UMSCHALT + B, um das Projekt zu erstellen.** (Die folgenden Schritte schlagen fehl, wenn Sie zu diesem Zeitpunkt nicht erstellen.)</span><span class="sxs-lookup"><span data-stu-id="ea40c-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="ea40c-130">Der nächste Schritt ist das Erstellen einer `DbMigration`-Klasse für die erste Migration.</span><span class="sxs-lookup"><span data-stu-id="ea40c-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="ea40c-131">Bei dieser Migration zu wird eine neue Datenbank erstellt. aus diesem Grund haben Sie die Datei " *Movie. mdf* " in einem vorherigen Schritt gelöscht.</span><span class="sxs-lookup"><span data-stu-id="ea40c-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="ea40c-132">Geben Sie im Fenster der **Paket-Manager-Konsole** den Befehl "Add-Migration Initial" ein, um die anfängliche Migration zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="ea40c-133">Der Name "Initial" ist willkürlich und wird zum Benennen der erstellten Migrations Datei verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea40c-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="ea40c-134">Code First-Migrationen erstellt eine andere Klassendatei im *Migrations* Ordner (mit dem Namen *{DATESTAMP}\_Initial.cs* ), und diese Klasse enthält Code, der das Datenbankschema erstellt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="ea40c-135">Der Dateiname der Migration ist mit einem Zeitstempel versehen, um die Reihenfolge zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="ea40c-136">Untersuchen Sie die Datei " *{DATESTAMP}\_Initial.cs* ", die die Anweisungen zum Erstellen der Film Tabelle für die Movie DB enthält.</span><span class="sxs-lookup"><span data-stu-id="ea40c-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="ea40c-137">Wenn Sie die Datenbank in den folgenden Anweisungen aktualisieren, wird die Datei " *{DATESTAMP}\_Initial.cs* " ausgeführt und das Datenbankschema erstellt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="ea40c-138">Anschließend wird die **Seed** -Methode ausgeführt, um die Datenbank mit Testdaten aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="ea40c-139">Geben Sie in der **Paket-Manager-Konsole**den Befehl "Update-Database" ein, um die Datenbank zu erstellen, und führen Sie die **Seed** -Methode aus.</span><span class="sxs-lookup"><span data-stu-id="ea40c-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="ea40c-140">Wenn Sie eine Fehlermeldung erhalten, die anzeigt, dass eine Tabelle bereits vorhanden und nicht erstellt werden kann, liegt dies wahrscheinlich daran, dass Sie die Anwendung ausgeführt haben, nachdem Sie die Datenbank gelöscht haben und bevor Sie `update-database`ausgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="ea40c-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="ea40c-141">Löschen Sie in diesem Fall die Datei " *Movies. mdf* " erneut, und wiederholen Sie den `update-database` Befehl.</span><span class="sxs-lookup"><span data-stu-id="ea40c-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="ea40c-142">Wenn weiterhin eine Fehlermeldung angezeigt wird, löschen Sie den Ordner "Migrationen" und den Inhalt, und beginnen Sie mit den Anweisungen am oberen Rand dieser Seite (Löschen Sie die Datei " *Movies. mdf* ", und fahren Sie dann mit enable-Migrationen fort).</span><span class="sxs-lookup"><span data-stu-id="ea40c-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="ea40c-143">Führen Sie die Anwendung aus, und navigieren Sie zur URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="ea40c-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="ea40c-144">Die Seed-Daten werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="ea40c-145">Hinzufügen einer Rating-Eigenschaft zum Movie-Modell</span><span class="sxs-lookup"><span data-stu-id="ea40c-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="ea40c-146">Beginnen Sie, indem Sie der vorhandenen `Movie` Klasse eine neue `Rating`-Eigenschaft hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="ea40c-147">Öffnen Sie die Datei *models\muvie.cs* , und fügen Sie die `Rating`-Eigenschaft wie folgt hinzu:</span><span class="sxs-lookup"><span data-stu-id="ea40c-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="ea40c-148">Die gesamte `Movie`-Klasse sieht nun wie der folgende Code aus:</span><span class="sxs-lookup"><span data-stu-id="ea40c-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="ea40c-149">Erstellen Sie die Anwendung mit dem Menübefehl **Build** &gt;**Build Movie** oder durch Drücken von STRG + UMSCHALT + B.</span><span class="sxs-lookup"><span data-stu-id="ea40c-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="ea40c-150">Nachdem Sie die `Model`-Klasse aktualisiert haben, müssen Sie auch die Ansichts Vorlagen *\views\movies\index.cshtml* und *\views\movies\kreate.cshtml* aktualisieren, um die neue `Rating`-Eigenschaft in der Browseransicht anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="ea40c-151">Öffnen Sie die Datei "<em>\views\movies\index.cshtml</em> ", und fügen Sie eine `<th>Rating</th>` Spaltenüberschrift direkt hinter der <strong>Preis</strong> Spalte ein.</span><span class="sxs-lookup"><span data-stu-id="ea40c-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="ea40c-152">Fügen Sie dann am Ende der Vorlage eine `<td>` Spalte hinzu, um den `@item.Rating` Wert zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="ea40c-153">Im folgenden sehen Sie, wie die aktualisierte Vorlage " <em>Index. cshtml</em> " angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="ea40c-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="ea40c-154">Öffnen Sie als nächstes die Datei " *\views\movies\kreate.cshtml* ", und fügen Sie das folgende Markup am Ende des Formulars ein.</span><span class="sxs-lookup"><span data-stu-id="ea40c-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="ea40c-155">Dadurch wird ein Textfeld gerendert, sodass Sie beim Erstellen eines neuen Films eine Bewertung angeben können.</span><span class="sxs-lookup"><span data-stu-id="ea40c-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="ea40c-156">Sie haben nun den Anwendungscode aktualisiert, um die neue `Rating`-Eigenschaft zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="ea40c-157">Führen Sie nun die Anwendung aus, und navigieren Sie zur URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="ea40c-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="ea40c-158">Wenn Sie dies jedoch tun, wird einer der folgenden Fehler angezeigt:</span><span class="sxs-lookup"><span data-stu-id="ea40c-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="ea40c-159">Dieser Fehler wird angezeigt, da die aktualisierte `Movie` Modell Klasse in der Anwendung sich nun von dem Schema der `Movie` Tabelle der vorhandenen Datenbank unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="ea40c-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="ea40c-160">(Die Datenbanktabelle enthält nicht die Spalte `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="ea40c-160">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="ea40c-161">Es gibt mehrere Vorgehensweisen zum Beheben des Fehlers:</span><span class="sxs-lookup"><span data-stu-id="ea40c-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="ea40c-162">Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="ea40c-163">Diese Vorgehensweise ist sehr praktisch, wenn eine aktive Entwicklung für eine Testdatenbank ausgeführt wird. Sie ermöglicht es Ihnen, das Modell und das Datenbankschema schnell zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="ea40c-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="ea40c-164">Der Nachteil besteht jedoch darin, dass Sie vorhandene Daten in der Datenbank verlieren – damit Sie diesen Ansatz *nicht* für eine Produktionsdatenbank verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="ea40c-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="ea40c-165">Die Verwendung eines Initialisierers zum automatischen Seed-Seed einer Datenbank mit Testdaten ist häufig eine produktive Möglichkeit zum Entwickeln einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ea40c-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="ea40c-166">Weitere Informationen zu Entity Framework-datenbankinitialisierern finden Sie unter Tom Dykstra es [ASP.NET MVC/Entity Framework Tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="ea40c-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="ea40c-167">Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="ea40c-168">Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten.</span><span class="sxs-lookup"><span data-stu-id="ea40c-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="ea40c-169">Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="ea40c-170">Verwenden Sie Code First-Migrationen, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ea40c-170">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="ea40c-171">Für dieses Tutorial verwenden wir Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="ea40c-172">Aktualisieren Sie die Seed-Methode, sodass Sie einen Wert für die neue Spalte bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="ea40c-173">Öffnen Sie die Datei migrations\configuration.cs, und fügen Sie jedem Movie-Objekt ein Bewertungsfeld hinzu.</span><span class="sxs-lookup"><span data-stu-id="ea40c-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="ea40c-174">Erstellen Sie die Projekt Mappe, öffnen Sie das Konsolenfenster des **Paket-Managers** , und geben Sie den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="ea40c-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="ea40c-175">Der `add-migration` Befehl weist das Migrations Framework an, das aktuelle Movie-Modell mit dem aktuellen Movie DB-Schema zu untersuchen und den erforderlichen Code zum Migrieren der Datenbank zum neuen Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="ea40c-176">Addratingmig ist willkürlich und wird verwendet, um die Migrations Datei zu benennen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="ea40c-177">Es ist hilfreich, einen aussagekräftigen Namen für den Migrationsschritt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ea40c-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="ea40c-178">Wenn dieser Befehl abgeschlossen ist, öffnet Visual Studio die Klassendatei, die die neue `DbMigration` abgeleitete Klasse definiert, und in der `Up`-Methode sehen Sie den Code, der die neue Spalte erstellt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="ea40c-179">Erstellen Sie die Projekt Mappe, und geben Sie dann den Befehl "Update-Database" in das Fenster der **Paket-Manager-Konsole** ein.</span><span class="sxs-lookup"><span data-stu-id="ea40c-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="ea40c-180">Die folgende Abbildung zeigt die Ausgabe im Fenster der **Paket-Manager-Konsole** (der Datumsstempel "addratingmig" unterscheidet sich.)</span><span class="sxs-lookup"><span data-stu-id="ea40c-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="ea40c-181">Führen Sie die Anwendung erneut aus, und navigieren Sie zur URL/Movies.</span><span class="sxs-lookup"><span data-stu-id="ea40c-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="ea40c-182">Sie können das Feld neue Bewertung sehen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="ea40c-183">Klicken Sie auf den Link **neu erstellen** , um einen neuen Film hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="ea40c-184">Beachten Sie, dass Sie eine Bewertung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="ea40c-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="ea40c-186">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="ea40c-186">Click **Create**.</span></span> <span data-ttu-id="ea40c-187">Der neue Film, einschließlich der Bewertung, wird jetzt in der Liste der Filme angezeigt:</span><span class="sxs-lookup"><span data-stu-id="ea40c-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="ea40c-189">Sie sollten auch das Feld "`Rating`" den Vorlagen "Bearbeiten", "Details" und "SearchIndex" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ea40c-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="ea40c-190">Sie können den Befehl "Update-Database" im Paket- **Manager-Konsolen** Fenster erneut eingeben, und es werden keine Änderungen vorgenommen, da das Schema mit dem Modell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="ea40c-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="ea40c-191">In diesem Abschnitt haben Sie erfahren, wie Sie Modell Objekte ändern und die Datenbank mit den Änderungen synchron halten.</span><span class="sxs-lookup"><span data-stu-id="ea40c-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="ea40c-192">Sie haben auch gelernt, wie Sie eine neu erstellte Datenbank mit Beispiel Daten auffüllen, um Szenarios ausprobieren zu können.</span><span class="sxs-lookup"><span data-stu-id="ea40c-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="ea40c-193">Als nächstes sehen wir uns an, wie Sie den Modellklassen eine umfassendere Validierungs Logik hinzufügen und einige Geschäftsregeln erzwingen können.</span><span class="sxs-lookup"><span data-stu-id="ea40c-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ea40c-194">[Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="ea40c-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
