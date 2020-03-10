---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Hinzufügen einer Spalte zum Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437223"
---
# <a name="adding-a-column-to-the-model"></a><span data-ttu-id="4cabc-104">Hinzufügen der Spalte zum Modell</span><span class="sxs-lookup"><span data-stu-id="4cabc-104">Adding a Column to the Model</span></span>

<span data-ttu-id="4cabc-105">von [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="4cabc-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="4cabc-106">Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="4cabc-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="4cabc-107">Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.</span><span class="sxs-lookup"><span data-stu-id="4cabc-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="4cabc-108">Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.</span><span class="sxs-lookup"><span data-stu-id="4cabc-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="4cabc-109">In diesem Abschnitt wird erläutert, wie wir Änderungen am Schema unserer Datenbank vornehmen und die Änderungen in unserer Anwendung behandeln können.</span><span class="sxs-lookup"><span data-stu-id="4cabc-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="4cabc-110">Fügen Sie der Movie-Tabelle eine Spalte "Rating" hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cabc-110">Let's add a "Rating" Column to the Movie table.</span></span> <span data-ttu-id="4cabc-111">Wechseln Sie zurück zur IDE, und klicken Sie auf den Datenbank-Explorer.</span><span class="sxs-lookup"><span data-stu-id="4cabc-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="4cabc-112">Klicken Sie mit der rechten Maustaste auf die Tabelle Movie, und wählen Sie Tabelle öffnen</span><span class="sxs-lookup"><span data-stu-id="4cabc-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="4cabc-113">Fügen Sie eine Spalte "Rating" wie unten gezeigt hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cabc-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="4cabc-114">Da wir jetzt über keine Bewertungen verfügen, kann die Spalte NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="4cabc-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="4cabc-115">Klicken Sie auf Speichern.</span><span class="sxs-lookup"><span data-stu-id="4cabc-115">Click Save.</span></span>

<span data-ttu-id="4cabc-116">[![Bearbeitung der Film Tabelle](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4cabc-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="4cabc-117">Kehren Sie als nächstes zum Projektmappen-Explorer zurück, und öffnen Sie die Datei "Movies. edmx" (die sich im Ordner "\models" befindet).</span><span class="sxs-lookup"><span data-stu-id="4cabc-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="4cabc-118">Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche (der weiße Bereich), und wählen Sie Modell aus Datenbank aktualisieren aus.</span><span class="sxs-lookup"><span data-stu-id="4cabc-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="4cabc-119">[![Filme-Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4cabc-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="4cabc-120">Dadurch wird der Update-Assistent gestartet.</span><span class="sxs-lookup"><span data-stu-id="4cabc-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="4cabc-121">Klicken Sie darin auf die Registerkarte Aktualisieren und dann auf Fertigstellen.</span><span class="sxs-lookup"><span data-stu-id="4cabc-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="4cabc-122">Unsere Movie-Modell Klasse wird dann mit der neuen Spalte aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="4cabc-122">Our Movie model class will then be updated with the new column.</span></span>

![Update-Assistent (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="4cabc-124">Nachdem Sie auf Fertigstellen geklickt haben, können Sie sehen, dass die neue Bewertungs Spalte der Movie-Entität in unserem Modell hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="4cabc-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="4cabc-125">[![Movie-Entität](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="4cabc-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="4cabc-126">Wir haben eine Spalte im Datenbankmodell hinzugefügt, aber die Ansichten wissen es nicht.</span><span class="sxs-lookup"><span data-stu-id="4cabc-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="4cabc-127">Aktualisieren von Sichten mit Modelländerungen</span><span class="sxs-lookup"><span data-stu-id="4cabc-127">Update Views with Model Changes</span></span>

<span data-ttu-id="4cabc-128">Es gibt mehrere Möglichkeiten, unsere Ansichts Vorlagen zu aktualisieren, um die neue Bewertungs Spalte widerzuspiegeln.</span><span class="sxs-lookup"><span data-stu-id="4cabc-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="4cabc-129">Da wir diese Ansichten erstellt haben, indem Sie Sie über das Dialogfeld "Ansicht hinzufügen" erstellt haben, können wir Sie löschen und erneut erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cabc-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="4cabc-130">Allerdings haben Benutzer in der Regel bereits Änderungen an Ihren Ansichts Vorlagen von der anfänglichen Gerüst Generierung vorgenommen, und Sie möchten Felder manuell hinzufügen oder löschen, genauso wie das ID-Feld für Create.</span><span class="sxs-lookup"><span data-stu-id="4cabc-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="4cabc-131">Öffnen Sie die Vorlage "\views\movies\index.aspx", und fügen Sie dem Anfang der Film Tabelle eine &lt;Th-&gt;Bewertung&lt;/Th-&gt; hinzu.</span><span class="sxs-lookup"><span data-stu-id="4cabc-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="4cabc-132">Ich habe meine nach Genre hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4cabc-132">I added mine after Genre.</span></span> <span data-ttu-id="4cabc-133">Fügen Sie dann in derselben Spaltenposition, aber unten unten eine Zeile hinzu, um unsere neue Bewertung auszugeben.</span><span class="sxs-lookup"><span data-stu-id="4cabc-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="4cabc-134">Unsere endgültige Vorlage "index. aspx" sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="4cabc-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="4cabc-135">Öffnen Sie dann die Vorlage "\views\movies\kreate.aspx", und fügen Sie eine Bezeichnung und ein Textfeld für die neue Bewertungs Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="4cabc-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="4cabc-136">Unsere endgültige Vorlage "Create. aspx" sieht wie folgt aus, und wir ändern den Titel des Browsers und den sekundären &lt;H2&gt; Titel in etwas wie "Erstellen eines Films", während wir hier sind!</span><span class="sxs-lookup"><span data-stu-id="4cabc-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="4cabc-137">Führen Sie Ihre APP aus, und jetzt haben Sie ein neues Feld in der Datenbank, das der Seite erstellen hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="4cabc-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="4cabc-138">Fügen Sie einen neuen Film hinzu: dieses Mal mit einer Bewertung, und klicken Sie auf erstellen.</span><span class="sxs-lookup"><span data-stu-id="4cabc-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="4cabc-139">[![Erstellen eines Films: Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="4cabc-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="4cabc-140">Nachdem Sie auf Erstellen klicken, werden Sie an die Index Seite gesendet, auf der Sie einen neuen Film mit der neuen Bewertungs Spalte in der Datenbank auflisten.</span><span class="sxs-lookup"><span data-stu-id="4cabc-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="4cabc-141">[![Movie List-Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="4cabc-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="4cabc-142">In diesem grundlegenden Tutorial haben Sie mit dem Erstellen von Controllern begonnen, indem Sie Ihnen Ansichten zuordnen und hart codierte Daten übergeben.</span><span class="sxs-lookup"><span data-stu-id="4cabc-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="4cabc-143">Anschließend haben wir eine Datenbank erstellt und entworfen und einige Daten in Sie eingefügt.</span><span class="sxs-lookup"><span data-stu-id="4cabc-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="4cabc-144">Wir haben die Daten aus der Datenbank abgerufen und in einer HTML-Tabelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4cabc-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="4cabc-145">Anschließend haben wir ein Erstellungs Formular hinzugefügt, mit dem der Benutzer der Datenbank selbst innerhalb der Webanwendung Daten hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="4cabc-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="4cabc-146">Wir haben die Validierung hinzugefügt und dann die Validierung für JavaScript auf Clientseite verwendet.</span><span class="sxs-lookup"><span data-stu-id="4cabc-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="4cabc-147">Schließlich haben wir die Datenbank so geändert, dass Sie eine neue Datenspalte enthält, und dann unsere beiden Seiten aktualisiert, um diese neuen Daten zu erstellen und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4cabc-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="4cabc-148">Ich empfehle Ihnen jetzt, das Tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" in der Zwischenebene und die vielen Videos und Ressourcen in [https://asp.net/mvc](https://asp.net/mvc) kennenzulernen, um noch mehr über ASP.NET MVC zu erfahren.</span><span class="sxs-lookup"><span data-stu-id="4cabc-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="4cabc-149">Viel Spaß!</span><span class="sxs-lookup"><span data-stu-id="4cabc-149">Enjoy!</span></span>

- <span data-ttu-id="4cabc-150">Scott Hanselman- [http://hanselman.com](http://hanselman.com) und [@shanselman](http://twitter.com/shanselman) auf Twitter.</span><span class="sxs-lookup"><span data-stu-id="4cabc-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4cabc-151">Previous</span><span class="sxs-lookup"><span data-stu-id="4cabc-151">Previous</span></span>](getting-started-with-mvc-part7.md)
