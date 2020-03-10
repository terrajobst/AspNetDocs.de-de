---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Zugreifen auf die Daten des Modells von einem Controller | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437367"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="2377d-104">Zugreifen auf Modelldaten anhand eines Controllers</span><span class="sxs-lookup"><span data-stu-id="2377d-104">Accessing your Model's Data from a Controller</span></span>

<span data-ttu-id="2377d-105">von [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2377d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="2377d-106">Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2377d-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2377d-107">Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.</span><span class="sxs-lookup"><span data-stu-id="2377d-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2377d-108">Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.</span><span class="sxs-lookup"><span data-stu-id="2377d-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="2377d-109">In diesem Abschnitt erstellen wir eine neue Klasse "moviescontroller" und schreiben Code, mit dem die Filmdaten abgerufen und mit einer Ansichts Vorlage wieder im Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2377d-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="2377d-110">Klicken Sie mit der rechten Maustaste auf den Ordner Controllers, und erstellen Sie einen neuen moviescontroller.</span><span class="sxs-lookup"><span data-stu-id="2377d-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="2377d-111">[Controller hinzufügen ![](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2377d-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="2377d-112">Dadurch wird eine neue Datei "MoviesController.cs" unterhalb unseres Ordners "\controllers" in unserem Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="2377d-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="2377d-113">Aktualisieren Sie den "" von "", um die Liste der Filme aus unserer neu aufgefüllten Datenbank abzurufen.</span><span class="sxs-lookup"><span data-stu-id="2377d-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="2377d-114">Wir führen eine LINQ-Abfrage aus, sodass nur Filme abgerufen werden, die nach dem Sommer von 1984 veröffentlicht wurden.</span><span class="sxs-lookup"><span data-stu-id="2377d-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="2377d-115">Wir benötigen eine Ansichts Vorlage, um die Liste der Filme wiederherzustellen. Klicken Sie also mit der rechten Maustaste auf die Methode, und wählen Sie Ansicht hinzufügen aus, um Sie zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2377d-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="2377d-116">Im Dialogfeld Ansicht hinzufügen geben wir an, dass wir eine Liste&lt;Filme. Models. Movie&gt; an unsere Ansichts Vorlage übergeben.</span><span class="sxs-lookup"><span data-stu-id="2377d-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="2377d-117">Im Gegensatz zu den vorherigen Zeiten haben wir das Dialogfeld Ansicht hinzufügen verwendet und eine "leere" Vorlage erstellt. in diesem Fall geben wir an, dass Visual Studio automatisch eine Ansichts Vorlage für uns mit einem Standard Inhalt erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="2377d-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="2377d-118">Hierzu wählen Sie im Dropdown Menü "Inhalt anzeigen" das Listenelement aus.</span><span class="sxs-lookup"><span data-stu-id="2377d-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="2377d-119">Beachten Sie, dass Sie, wenn Sie eine neue Klasse erstellt haben, die Anwendung kompilieren müssen, damit Sie im Dialog Feld "Ansicht hinzufügen" angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2377d-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Ansicht hinzufügen](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="2377d-121">Wenn Sie auf Hinzufügen klicken, generiert das System automatisch den Code für eine Ansicht für uns, in der die Liste der Filme angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2377d-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="2377d-122">Dies ist ein guter Zeitpunkt, die &lt;H2-&gt; Überschrift in "My Movie List" zu ändern, wie dies bereits zuvor in der Hallo Welt Ansicht der Fall war.</span><span class="sxs-lookup"><span data-stu-id="2377d-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="2377d-123">[![Filme-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2377d-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="2377d-124">Führen Sie die Anwendung aus, und besuchen Sie/Movies in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="2377d-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="2377d-125">Nun haben wir mit einer einfachen Abfrage innerhalb des Controllers Daten aus der Datenbank abgerufen und die Daten an eine Ansicht zurückgegeben, die über Filme weiß.</span><span class="sxs-lookup"><span data-stu-id="2377d-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="2377d-126">Diese Ansicht durchläuft dann die Liste der Filme und erstellt eine Tabelle mit Daten für uns.</span><span class="sxs-lookup"><span data-stu-id="2377d-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="2377d-127">[![Movie List-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="2377d-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="2377d-128">Mit dieser Anwendung werden die Funktionen "Bearbeiten", "Details" und "Löschen" nicht implementiert. Daher benötigen wir nicht die Standard Verknüpfungen, die von der Gerüst Vorlage für uns erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="2377d-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="2377d-129">Öffnen Sie die Datei/Movies/Index.aspx, und entfernen Sie Sie.</span><span class="sxs-lookup"><span data-stu-id="2377d-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="2377d-130">Im folgenden finden Sie den Quellcode für das, was unsere aktualisierte Ansichts Vorlage aussehen sollte, nachdem wir diese Änderungen vorgenommen haben:</span><span class="sxs-lookup"><span data-stu-id="2377d-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="2377d-131">Es erstellt Links, die wir nicht benötigen, daher werden wir Sie für dieses Beispiel löschen.</span><span class="sxs-lookup"><span data-stu-id="2377d-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="2377d-132">Wir behalten uns jedoch den neuen Link "neu erstellen" vor. Dies ist der nächste Schritt.</span><span class="sxs-lookup"><span data-stu-id="2377d-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="2377d-133">So sieht unsere App aus, in der diese Spalte entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="2377d-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="2377d-134">[![Movie List-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="2377d-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="2377d-135">Wir verfügen jetzt über eine einfache Auflistung unserer Filmdaten.</span><span class="sxs-lookup"><span data-stu-id="2377d-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="2377d-136">Wenn wir jedoch auf den Link "Create New" (neu erstellen) klicken, wird eine Fehlermeldung angezeigt, da diese nicht verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="2377d-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="2377d-137">Wir implementieren eine Create Action-Methode und ermöglichen einem Benutzer die Eingabe neuer Filme in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2377d-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2377d-138">[Zurück](getting-started-with-mvc-part4.md)
> [Weiter](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="2377d-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
