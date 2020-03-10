---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Erstellen einer Datenbank | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469671"
---
# <a name="creating-a-database"></a><span data-ttu-id="ee82b-104">Erstellen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="ee82b-104">Creating a Database</span></span>

<span data-ttu-id="ee82b-105">von [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="ee82b-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="ee82b-106">Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ee82b-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="ee82b-107">Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.</span><span class="sxs-lookup"><span data-stu-id="ee82b-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="ee82b-108">Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.</span><span class="sxs-lookup"><span data-stu-id="ee82b-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="ee82b-109">In diesem Abschnitt erstellen wir eine neue SQL Express-Datenbank, mit der wir die Filmdaten speichern und abrufen.</span><span class="sxs-lookup"><span data-stu-id="ee82b-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="ee82b-110">Wählen Sie in der Visual Web Developer-IDE Ansicht aus. Server-Explorer.</span><span class="sxs-lookup"><span data-stu-id="ee82b-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="ee82b-111">Klicken Sie mit der rechten Maustaste auf Datenverbindungen, und klicken Sie auf Verbindung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ee82b-111">Right click on Data Connections and click Add Connection...</span></span>

![Addconnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="ee82b-113">Wählen Sie im Dialogfeld Datenquelle auswählen Microsoft SQL Server aus, und klicken Sie dann auf Weiter.</span><span class="sxs-lookup"><span data-stu-id="ee82b-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="ee82b-114">Geben Sie im Dialogfeld Verbindung hinzufügen für Ihren Server Namen ".\sqlexpress" ein, und geben Sie "Movies" als Namen für die neue Datenbank ein.</span><span class="sxs-lookup"><span data-stu-id="ee82b-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="ee82b-115">[Dialog !["Verbindung hinzufügen"](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="ee82b-116">Klicken Sie auf OK, und Sie werden gefragt, ob Sie die Datenbank erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="ee82b-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="ee82b-117">Wählen Sie ja aus.</span><span class="sxs-lookup"><span data-stu-id="ee82b-117">Select yes.</span></span>

<span data-ttu-id="ee82b-118">[![Filme erstellen?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="ee82b-119">Nun haben Sie eine leere Datenbank in Server-Explorer.</span><span class="sxs-lookup"><span data-stu-id="ee82b-119">Now you've got an empty database in Server Explorer.</span></span>

![Neue Tabelle hinzufügen](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="ee82b-121">Klicken Sie mit der rechten Maustaste auf Tabellen und dann auf Tabelle hinzufügen</span><span class="sxs-lookup"><span data-stu-id="ee82b-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="ee82b-122">Die Tabellen-Designer wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ee82b-122">The Table Designer will appear.</span></span> <span data-ttu-id="ee82b-123">Fügen Sie Spalten für ID, Titel, ReleaseDate, Genre und Preis hinzu.</span><span class="sxs-lookup"><span data-stu-id="ee82b-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="ee82b-124">Klicken Sie mit der rechten Maustaste auf die Spalte ID, und klicken Sie auf Primärschlüssel festlegen</span><span class="sxs-lookup"><span data-stu-id="ee82b-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="ee82b-125">So sieht mein Entwurfs Bereich aus.</span><span class="sxs-lookup"><span data-stu-id="ee82b-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="ee82b-126">[Datenbanktabellen-Editor ![](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="ee82b-127">Wählen Sie außerdem die Spalte ID aus, und ändern Sie Unterspalten Eigenschaften unten "Identitäts Spezifikation" in "Ja".</span><span class="sxs-lookup"><span data-stu-id="ee82b-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="ee82b-128">[![IsIdentity-Column-Eigenschaften](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="ee82b-129">Wenn Sie den Vorgang abgeschlossen haben, klicken Sie auf der Symbolleiste auf das Symbol speichern, oder wählen Sie Datei aus. Speichern Sie aus dem Menü, und benennen Sie die Tabelle "**Movie**" (Singular).</span><span class="sxs-lookup"><span data-stu-id="ee82b-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="ee82b-130">Wir haben eine Datenbank und eine Tabelle!</span><span class="sxs-lookup"><span data-stu-id="ee82b-130">We've got a database and a table!</span></span>

<span data-ttu-id="ee82b-131">[![Namen auswählen](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="ee82b-132">Wechseln Sie zurück zu Server-Explorer, klicken Sie mit der rechten Maustaste auf die Tabelle Movie, und wählen Sie dann Tabellendaten anzeigen aus.</span><span class="sxs-lookup"><span data-stu-id="ee82b-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="ee82b-133">Geben Sie einige Filme ein, damit unsere Datenbank über einige Daten verfügt.</span><span class="sxs-lookup"><span data-stu-id="ee82b-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="ee82b-134">[Bearbeitung der Datenbanktabelle ![](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="ee82b-135">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="ee82b-135">Creating a Model</span></span>

<span data-ttu-id="ee82b-136">Wechseln Sie nun zurück zum Projektmappen-Explorer auf der rechten Seite der IDE, klicken Sie mit der rechten Maustaste auf den Ordner Modelle, und wählen Sie hinzufügen aus. Neues Element.</span><span class="sxs-lookup"><span data-stu-id="ee82b-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="ee82b-137">[![addnewmudelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="ee82b-138">Wir werden ein Entitäts Modell aus unserer neuen Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="ee82b-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="ee82b-139">Dadurch wird dem Projekt eine Reihe von Klassen hinzugefügt, die es uns erleichtern, die Daten in unserer Datenbank abzufragen und zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="ee82b-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="ee82b-140">Wählen Sie auf der linken Seite des Dialog Felds den Knoten Daten aus, und wählen Sie dann die Vorlage ADO.NET Entity Data Model Item aus.</span><span class="sxs-lookup"><span data-stu-id="ee82b-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="ee82b-141">Nennen Sie die Datei Movies. edmx.</span><span class="sxs-lookup"><span data-stu-id="ee82b-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="ee82b-142">[![addnewdatamodel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="ee82b-143">Klicken Sie auf die Schaltfläche „Hinzufügen“.</span><span class="sxs-lookup"><span data-stu-id="ee82b-143">Click the "Add" button.</span></span> <span data-ttu-id="ee82b-144">Daraufhin wird der Assistent "Entity Data Model" gestartet.</span><span class="sxs-lookup"><span data-stu-id="ee82b-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="ee82b-145">Wählen Sie im neu angezeigten Dialogfeld die Option aus Datenbank generieren aus.</span><span class="sxs-lookup"><span data-stu-id="ee82b-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="ee82b-146">Da wir soeben eine Datenbank erstellt haben, müssen wir nur die Entity Framework über unsere neue Datenbank und Ihre Tabelle informieren.</span><span class="sxs-lookup"><span data-stu-id="ee82b-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="ee82b-147">Klicken Sie auf Weiter, um die Datenbankverbindung in der Konfiguration unserer Webanwendung zu speichern.</span><span class="sxs-lookup"><span data-stu-id="ee82b-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="ee82b-148">Aktivieren Sie nun das Kontrollkästchen Tabellen und Movie, und klicken Sie auf Fertigstellen.</span><span class="sxs-lookup"><span data-stu-id="ee82b-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="ee82b-149">[![Entity Data Model-Assistenten](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="ee82b-150">Nun können wir unsere neue Film Tabelle im Entity Framework Designer sehen und über Code darauf zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ee82b-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="ee82b-151">[![Filme-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="ee82b-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="ee82b-152">Auf der Entwurfs Oberfläche sehen Sie eine "Movie"-Klasse.</span><span class="sxs-lookup"><span data-stu-id="ee82b-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="ee82b-153">Diese Klasse wird der "Movie"-Tabelle in der Datenbank zugeordnet, und jede darin enthaltenen Eigenschaften wird einer Spalte mit der Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ee82b-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="ee82b-154">Jede Instanz einer "Movie"-Klasse entspricht einer Zeile in der Tabelle "Movie".</span><span class="sxs-lookup"><span data-stu-id="ee82b-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="ee82b-155">Wenn Sie die vom Entity Framework verwendeten standardmäßigen Benennungs-und Mapping-Konventionen nicht kennen, können Sie den Entity Framework Designer verwenden, um Sie zu ändern oder anzupassen.</span><span class="sxs-lookup"><span data-stu-id="ee82b-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="ee82b-156">Für diese Anwendung verwenden wir die Standardwerte und speichern die Datei einfach unverändert.</span><span class="sxs-lookup"><span data-stu-id="ee82b-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="ee82b-157">Nun arbeiten wir mit echten Daten.</span><span class="sxs-lookup"><span data-stu-id="ee82b-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee82b-158">[Zurück](getting-started-with-mvc-part3.md)
> [Weiter](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="ee82b-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
