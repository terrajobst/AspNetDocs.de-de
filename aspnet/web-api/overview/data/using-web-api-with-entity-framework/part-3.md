---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Verwenden Sie Code First-Migrationen, um die Datenbank zu Seed | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449115"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="511d7-102">Verwenden Sie Code First-Migrationen, um die Datenbank zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="511d7-102">Use Code First Migrations to Seed the Database</span></span>

<span data-ttu-id="511d7-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="511d7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="511d7-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="511d7-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="511d7-105">In diesem Abschnitt verwenden Sie [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) in EF, um die Datenbank mit Testdaten zu durchführen.</span><span class="sxs-lookup"><span data-stu-id="511d7-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="511d7-106">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="511d7-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="511d7-107">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="511d7-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="511d7-108">Dieser Befehl fügt Ihrem Projekt einen Ordner namens Migrationen sowie eine Codedatei mit dem Namen Configuration.cs im Migrations Ordner hinzu.</span><span class="sxs-lookup"><span data-stu-id="511d7-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="511d7-109">Öffnen Sie die Datei Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="511d7-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="511d7-110">Fügen Sie die folgende **using** -Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="511d7-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="511d7-111">Fügen Sie dann der **Configuration. Seed** -Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="511d7-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="511d7-112">Geben Sie im Fenster der Paket-Manager-Konsole die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="511d7-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="511d7-113">Der erste Befehl generiert Code, mit dem die Datenbank erstellt wird, und der zweite Befehl führt den Code aus.</span><span class="sxs-lookup"><span data-stu-id="511d7-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="511d7-114">Die Datenbank wird mithilfe von [localdb](https://msdn.microsoft.com/library/hh510202.aspx)lokal erstellt.</span><span class="sxs-lookup"><span data-stu-id="511d7-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="511d7-115">Erkunden Sie die API (optional)</span><span class="sxs-lookup"><span data-stu-id="511d7-115">Explore the API (Optional)</span></span>

<span data-ttu-id="511d7-116">Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="511d7-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="511d7-117">Visual Studio startet IIS Express und führt Ihre Web-App aus.</span><span class="sxs-lookup"><span data-stu-id="511d7-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="511d7-118">Visual Studio öffnet dann einen Browser und öffnet die Startseite der app.</span><span class="sxs-lookup"><span data-stu-id="511d7-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="511d7-119">Wenn Visual Studio ein Webprojekt ausführt, wird eine Portnummer zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="511d7-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="511d7-120">In der folgenden Abbildung ist die Portnummer 50524.</span><span class="sxs-lookup"><span data-stu-id="511d7-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="511d7-121">Wenn Sie die Anwendung ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="511d7-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="511d7-122">Die Startseite wird mit ASP.NET MVC implementiert.</span><span class="sxs-lookup"><span data-stu-id="511d7-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="511d7-123">Am oberen Rand der Seite befindet sich ein Link mit dem Text "API".</span><span class="sxs-lookup"><span data-stu-id="511d7-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="511d7-124">Über diesen Link gelangen Sie zu einer automatisch generierten Hilfeseite für die Web-API.</span><span class="sxs-lookup"><span data-stu-id="511d7-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="511d7-125">(Informationen dazu, wie diese Hilfeseite generiert wird und wie Sie der Seite Ihre eigene Dokumentation hinzufügen können, finden Sie unter [Erstellen von Hilfeseiten für ASP.net-Web-API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Sie können auf die Links der Hilfeseite klicken, um Details zur API anzuzeigen, einschließlich des Anforderungs-und Antwort Formats.</span><span class="sxs-lookup"><span data-stu-id="511d7-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="511d7-126">Die API ermöglicht CRUD-Vorgänge für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="511d7-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="511d7-127">Im folgenden wird die-API zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="511d7-127">The following summarizes the API.</span></span>

| <span data-ttu-id="511d7-128">Authors</span><span class="sxs-lookup"><span data-stu-id="511d7-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="511d7-129">GET api/authors</span><span class="sxs-lookup"><span data-stu-id="511d7-129">GET api/authors</span></span> | <span data-ttu-id="511d7-130">Alle Autoren erhalten.</span><span class="sxs-lookup"><span data-stu-id="511d7-130">Get all authors.</span></span> |
| <span data-ttu-id="511d7-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="511d7-131">GET api/authors/{id}</span></span> | <span data-ttu-id="511d7-132">Erstellen Sie einen Autor anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="511d7-132">Get an author by ID.</span></span> |
| <span data-ttu-id="511d7-133">POST /api/authors</span><span class="sxs-lookup"><span data-stu-id="511d7-133">POST /api/authors</span></span> | <span data-ttu-id="511d7-134">Erstellen Sie einen neuen Autor.</span><span class="sxs-lookup"><span data-stu-id="511d7-134">Create a new author.</span></span> |
| <span data-ttu-id="511d7-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="511d7-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="511d7-136">Aktualisiert einen vorhandenen Autor.</span><span class="sxs-lookup"><span data-stu-id="511d7-136">Update an existing author.</span></span> |
| <span data-ttu-id="511d7-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="511d7-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="511d7-138">Löschen Sie einen Autor.</span><span class="sxs-lookup"><span data-stu-id="511d7-138">Delete an author.</span></span> |

| <span data-ttu-id="511d7-139">Bücher</span><span class="sxs-lookup"><span data-stu-id="511d7-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="511d7-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="511d7-140">GET /api/books</span></span> | <span data-ttu-id="511d7-141">Alle Bücher erhalten.</span><span class="sxs-lookup"><span data-stu-id="511d7-141">Get all books.</span></span> |
| <span data-ttu-id="511d7-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="511d7-142">GET /api/books/{id}</span></span> | <span data-ttu-id="511d7-143">Sie erhalten ein Buch anhand der ID.</span><span class="sxs-lookup"><span data-stu-id="511d7-143">Get a book by ID.</span></span> |
| <span data-ttu-id="511d7-144">POST /api/books</span><span class="sxs-lookup"><span data-stu-id="511d7-144">POST /api/books</span></span> | <span data-ttu-id="511d7-145">Erstellen Sie ein neues Buch.</span><span class="sxs-lookup"><span data-stu-id="511d7-145">Create a new book.</span></span> |
| <span data-ttu-id="511d7-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="511d7-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="511d7-147">Aktualisieren eines vorhandenen Buchs.</span><span class="sxs-lookup"><span data-stu-id="511d7-147">Update an existing book.</span></span> |
| <span data-ttu-id="511d7-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="511d7-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="511d7-149">Löschen eines Buchs.</span><span class="sxs-lookup"><span data-stu-id="511d7-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="511d7-150">Anzeigen der Datenbank (optional)</span><span class="sxs-lookup"><span data-stu-id="511d7-150">View the Database (Optional)</span></span>

<span data-ttu-id="511d7-151">Wenn Sie den Update-Database-Befehl ausgeführt haben, hat EF die Datenbank erstellt und die `Seed`-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="511d7-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="511d7-152">Wenn Sie die Anwendung lokal ausführen, verwendet EF [localdb](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="511d7-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="511d7-153">Sie können die Datenbank in Visual Studio anzeigen.</span><span class="sxs-lookup"><span data-stu-id="511d7-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="511d7-154">Wählen Sie im Menü **Ansicht** die Option **SQL Server-Objekt-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="511d7-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="511d7-155">Geben Sie im Dialogfeld **Verbindung mit Server herstellen** im Bearbeitungsfeld **Server Name den Namen** "(localdb) \v11.0" ein.</span><span class="sxs-lookup"><span data-stu-id="511d7-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="511d7-156">Belassen Sie die **Authentifizierungs** Option als "Windows-Authentifizierung".</span><span class="sxs-lookup"><span data-stu-id="511d7-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="511d7-157">Klicken Sie auf **Verbinden**.</span><span class="sxs-lookup"><span data-stu-id="511d7-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="511d7-158">Visual Studio stellt eine Verbindung mit localdb her und zeigt die vorhandenen Datenbanken im SQL Server-Objekt-Explorer Fenster an.</span><span class="sxs-lookup"><span data-stu-id="511d7-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="511d7-159">Sie können die Knoten erweitern, um die von EF erstellten Tabellen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="511d7-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="511d7-160">Um die Daten anzuzeigen, klicken Sie mit der rechten Maustaste auf eine Tabelle, und wählen Sie **Daten anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="511d7-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="511d7-161">Der folgende Screenshot zeigt die Ergebnisse für die Bücher Tabelle.</span><span class="sxs-lookup"><span data-stu-id="511d7-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="511d7-162">Beachten Sie, dass EF die Datenbank mit den Seed-Daten aufgefüllt hat und die Tabelle den Fremdschlüssel für die Autoren Tabelle enthält.</span><span class="sxs-lookup"><span data-stu-id="511d7-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="511d7-163">[Zurück](part-2.md)
> [Weiter](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="511d7-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
