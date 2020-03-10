---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen | Microsoft-Dokumentation
author: rick-anderson
description: Wenn Sie mit ASP.NET MVC 4-Controller Methoden vertraut sind oder die &quot;Hilfsprogramme, Formulare und Validierung&quot; praktische Übungseinheit abgeschlossen haben, sollten Sie sich bewusst sein...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484641"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="55768-103">ASP.NET MVC 4 – Gerüstbau und Migrationen mit dem Entity Framework</span><span class="sxs-lookup"><span data-stu-id="55768-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="55768-104">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="55768-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="55768-105">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="55768-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="55768-106">Wenn Sie mit ASP.NET MVC 4-Controller Methoden vertraut sind oder die &quot;Hilfsprogramme, Formulare und Validierungs&quot; praktische Übungseinheit abgeschlossen haben, sollten Sie beachten, dass viele der Logik zum Erstellen, aktualisieren, auflisten und entfernen beliebiger Daten Entitäten, die Sie in der Anwendung wiederholt.</span><span class="sxs-lookup"><span data-stu-id="55768-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="55768-107">Wenn Ihr Modell über mehrere Klassen zum Bearbeiten verfügt, werden Sie wahrscheinlich viel Zeit aufwenden, um die Post-und Get Action-Methoden für jeden Entitäts Vorgang und jede der Ansichten zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="55768-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="55768-108">In dieser Übungseinheit erfahren Sie, wie Sie den ASP.NET MVC 4-Gerüstbau zum automatischen Generieren der Baseline der CRUD (Create, Read, Update und DELETE) Ihrer Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="55768-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="55768-109">Beginnend mit einer einfachen Modell Klasse und, ohne eine einzige Codezeile zu schreiben, erstellen Sie einen Controller, der alle CRUD-Vorgänge sowie alle notwendigen Sichten enthält.</span><span class="sxs-lookup"><span data-stu-id="55768-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="55768-110">Nachdem Sie die einfache Lösung erstellt und ausgeführt haben, wird die Anwendungsdatenbank sowie die MVC-Logik und-Sichten für die Datenbearbeitung generiert.</span><span class="sxs-lookup"><span data-stu-id="55768-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="55768-111">Darüber hinaus erfahren Sie, wie einfach es ist, Entity Framework Migrationen zu verwenden, um Modell Updates in der gesamten Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="55768-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="55768-112">Mit Entity Framework Migrationen können Sie Ihre Datenbank ändern, nachdem das Modell mit einfachen Schritten geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="55768-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="55768-113">Mit all diesen Bedenken können Sie Webanwendungen effizienter erstellen und pflegen und dabei die neuesten Features von ASP.NET MVC 4 nutzen.</span><span class="sxs-lookup"><span data-stu-id="55768-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="55768-114">Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das in den [Releases Microsoft-Web/webcamptrainingkit](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="55768-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="55768-115">Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4 Entity Framework Gerüstbau und Migrationen](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="55768-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="55768-116">Ziele</span><span class="sxs-lookup"><span data-stu-id="55768-116">Objectives</span></span>

<span data-ttu-id="55768-117">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="55768-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="55768-118">Verwenden Sie ASP.net Gerüstbau für CRUD-Vorgänge in Controllern.</span><span class="sxs-lookup"><span data-stu-id="55768-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="55768-119">Ändern Sie das Datenbankmodell mithilfe Entity Framework Migrationen.</span><span class="sxs-lookup"><span data-stu-id="55768-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="55768-120">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="55768-120">Prerequisites</span></span>

<span data-ttu-id="55768-121">Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:</span><span class="sxs-lookup"><span data-stu-id="55768-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="55768-122">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).</span><span class="sxs-lookup"><span data-stu-id="55768-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="55768-123">Einrichten</span><span class="sxs-lookup"><span data-stu-id="55768-123">Setup</span></span>

<span data-ttu-id="55768-124">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="55768-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="55768-125">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="55768-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="55768-126">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="55768-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="55768-127">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang B: Verwenden von Code Ausschnitten](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="55768-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="55768-128">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="55768-128">Exercises</span></span>

<span data-ttu-id="55768-129">In der folgenden Übung wird diese praktische Übungseinheit eingerichtet:</span><span class="sxs-lookup"><span data-stu-id="55768-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="55768-130">Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework Migrationen</span><span class="sxs-lookup"><span data-stu-id="55768-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="55768-131">Diese Übung wird von einem **End** -Ordner mit der resultierenden Lösung begleitet, die Sie nach Abschluss der Übung erhalten sollten.</span><span class="sxs-lookup"><span data-stu-id="55768-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="55768-132">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Arbeiten mit der Übung benötigen.</span><span class="sxs-lookup"><span data-stu-id="55768-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>

<span data-ttu-id="55768-133">Geschätzte Zeit bis zum Abschluss dieses Labs: **30 Minuten**</span><span class="sxs-lookup"><span data-stu-id="55768-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="55768-134">Übung 1: Verwenden von ASP.NET MVC 4-Gerüstbau mit Entity Framework Migrationen</span><span class="sxs-lookup"><span data-stu-id="55768-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="55768-135">ASP.NET MVC-Gerüstbau bietet eine schnelle Möglichkeit, die CRUD-Vorgänge auf standardisierte Weise zu generieren und so die erforderliche Logik zu erstellen, mit der Ihre Anwendung mit der Datenbankschicht interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="55768-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="55768-136">In dieser Übung erfahren Sie, wie Sie ASP.NET MVC 4 Gerüstbau mit Code First verwenden, um die CRUD-Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="55768-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="55768-137">Anschließend erfahren Sie, wie Sie das Modell aktualisieren können, indem Sie die Änderungen in der Datenbank mithilfe Entity Framework Migrationen anwenden.</span><span class="sxs-lookup"><span data-stu-id="55768-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="55768-138">Aufgabe 1: Erstellen eines neuen ASP.NET MVC 4-Projekts mithilfe eines Gerüsts</span><span class="sxs-lookup"><span data-stu-id="55768-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="55768-139">Wenn Sie nicht bereits geöffnet ist, starten Sie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="55768-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="55768-140">**Datei auswählen | Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="55768-140">Select **File | New Project**.</span></span> <span data-ttu-id="55768-141">Klicken Sie im Dialogfeld Neues Projekt unter **dem C# Visual | Webabschnitt** wählen Sie **ASP.NET MVC 4-Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="55768-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="55768-142">Benennen Sie das Projekt mit **MVC4andEFMigrations** , und legen Sie den Speicherort auf den Ordner **source\ex1-usingmvc4gerüschesfmigrationen** dieses Labs fest.</span><span class="sxs-lookup"><span data-stu-id="55768-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="55768-143">Legen Sie den Projektmappennamen auf **beginnen** fest, und stellen Sie sicher, dass Projektmappenverzeichnis **Erstellen**</span><span class="sxs-lookup"><span data-stu-id="55768-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="55768-144">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="55768-144">Click **OK**.</span></span>

    <span data-ttu-id="55768-145">![Neues ASP.NET MVC 4-Projekt (Dialog Feld)](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Neues ASP.NET MVC 4-Projekt (Dialog Feld)")</span><span class="sxs-lookup"><span data-stu-id="55768-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="55768-146">*Neues ASP.NET MVC 4-Projekt (Dialog Feld)*</span><span class="sxs-lookup"><span data-stu-id="55768-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="55768-147">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Vorlage **Internet Anwendung** aus, und vergewissern Sie sich, dass **Razor** die ausgewählte **Ansichts-Engine**ist.</span><span class="sxs-lookup"><span data-stu-id="55768-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="55768-148">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="55768-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="55768-149">![Neu ASP.NET MVC 4-Internet Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Neu ASP.NET MVC 4-Internet Anwendung")</span><span class="sxs-lookup"><span data-stu-id="55768-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="55768-150">*Neu ASP.NET MVC 4-Internet Anwendung*</span><span class="sxs-lookup"><span data-stu-id="55768-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="55768-151">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Modelle** , und wählen Sie **hinzufügen aus. Klasse** zum Erstellen einer einfachen Klasse Person (poco).</span><span class="sxs-lookup"><span data-stu-id="55768-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="55768-152">Nennen Sie es **Person** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="55768-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="55768-153">Öffnen Sie die Klasse Person, und fügen Sie die folgenden Eigenschaften ein.</span><span class="sxs-lookup"><span data-stu-id="55768-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="55768-154">(Code Ausschnitt- *ASP.NET MVC 4 und Entity Framework Migrationen-EX1 Person-Eigenschaften*)</span><span class="sxs-lookup"><span data-stu-id="55768-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="55768-155">Klicken Sie auf **Erstellen | Erstellen** Sie die Lösung, um die Änderungen zu speichern und das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="55768-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="55768-156">![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Erstellen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="55768-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="55768-157">*Erstellen der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="55768-157">*Building the Application*</span></span>
7. <span data-ttu-id="55768-158">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **hinzufügen aus. Controller**.</span><span class="sxs-lookup"><span data-stu-id="55768-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="55768-159">Benennen Sie den Controller *personcontroller* , und vervollständigen Sie die **Gerüstbau Optionen** mit den folgenden Werten.</span><span class="sxs-lookup"><span data-stu-id="55768-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="55768-160">Wählen Sie in der Dropdown Liste **Vorlage** den **MVC-Controller mit Lese-/Schreibaktionen und-Sichten mit Entity Framework** Option aus.</span><span class="sxs-lookup"><span data-stu-id="55768-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="55768-161">Wählen Sie in der Dropdown Liste **Modell Klasse** die **Person** -Klasse aus.</span><span class="sxs-lookup"><span data-stu-id="55768-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="55768-162">Wählen Sie in der Liste **Datenkontext Klasse** die Option **&lt;neuer Datenkontext...&gt;** aus.</span><span class="sxs-lookup"><span data-stu-id="55768-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="55768-163">Wählen Sie einen beliebigen Namen, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="55768-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="55768-164">Vergewissern Sie sich, dass in der Dropdown Liste **Sichten** die Option **Razor** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="55768-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="55768-165">![Hinzufügen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Hinzufügen des Person-Controllers mit Gerüstbau")</span><span class="sxs-lookup"><span data-stu-id="55768-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="55768-166">*Hinzufügen des Person-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="55768-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="55768-167">Klicken Sie auf **Hinzufügen** , um den neuen Controller für Person mit Gerüst zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="55768-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="55768-168">Sie haben nun die Controller Aktionen und die Ansichten generiert.</span><span class="sxs-lookup"><span data-stu-id="55768-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="55768-169">![Nach dem Erstellen des Person-Controllers mit Gerüstbau](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Nach dem Erstellen des Person-Controllers mit Gerüstbau")</span><span class="sxs-lookup"><span data-stu-id="55768-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="55768-170">*Nach dem Erstellen des Person-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="55768-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="55768-171">Öffnen Sie die **personcontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="55768-171">Open **PersonController** class.</span></span> <span data-ttu-id="55768-172">Beachten Sie, dass die vollständigen CRUD-Aktionsmethoden automatisch generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="55768-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="55768-173">![Innerhalb des Person-Controllers](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Innerhalb des Person-Controllers")</span><span class="sxs-lookup"><span data-stu-id="55768-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="55768-174">*Innerhalb des Person-Controllers*</span><span class="sxs-lookup"><span data-stu-id="55768-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="55768-175">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="55768-175">Task 2- Running the application</span></span>

<span data-ttu-id="55768-176">An diesem Punkt wurde die Datenbank noch nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="55768-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="55768-177">In dieser Aufgabe führen Sie die Anwendung zum ersten Mal aus und testen die CRUD-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="55768-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="55768-178">Die Datenbank wird im Handumdrehen mit Code First erstellt.</span><span class="sxs-lookup"><span data-stu-id="55768-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="55768-179">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="55768-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="55768-180">Fügen Sie im Browser **/Person** zur URL hinzu, um die Person-Seite zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="55768-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="55768-181">![Anwendung zuerst ausführen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Anwendung zuerst ausführen")</span><span class="sxs-lookup"><span data-stu-id="55768-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="55768-182">*Anwendung: erste Testlauf*</span><span class="sxs-lookup"><span data-stu-id="55768-182">*Application: first run*</span></span>
3. <span data-ttu-id="55768-183">Sie werden nun die Personen Seiten untersuchen und die CRUD-Vorgänge testen.</span><span class="sxs-lookup"><span data-stu-id="55768-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="55768-184">Klicken Sie auf **neu erstellen** , um eine neue Person hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="55768-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="55768-185">Geben Sie einen Vornamen und einen Nachnamen ein, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="55768-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="55768-186">![Hinzufügen einer neuen Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Hinzufügen einer neuen Person")</span><span class="sxs-lookup"><span data-stu-id="55768-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="55768-187">*Hinzufügen einer neuen Person*</span><span class="sxs-lookup"><span data-stu-id="55768-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="55768-188">In der Liste der Personen können Sie Elemente löschen, bearbeiten oder hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="55768-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="55768-189">![Personen Liste](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Personen Liste")</span><span class="sxs-lookup"><span data-stu-id="55768-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="55768-190">*Personen Liste*</span><span class="sxs-lookup"><span data-stu-id="55768-190">*Person list*</span></span>
    3. <span data-ttu-id="55768-191">Klicken Sie auf **Details** , um die Details der Person zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="55768-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="55768-192">![Personen Details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Personen Details")</span><span class="sxs-lookup"><span data-stu-id="55768-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="55768-193">*Personen Details*</span><span class="sxs-lookup"><span data-stu-id="55768-193">*Person's details*</span></span>
4. <span data-ttu-id="55768-194">Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.</span><span class="sxs-lookup"><span data-stu-id="55768-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="55768-195">Beachten Sie, dass Sie in der gesamten Anwendung den gesamten CRUD für die Person-Entität erstellt haben, ohne eine einzige Codezeile schreiben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="55768-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="55768-196">Aufgabe 3: Aktualisieren der Datenbank mithilfe Entity Framework Migrationen</span><span class="sxs-lookup"><span data-stu-id="55768-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="55768-197">In dieser Aufgabe aktualisieren Sie die Datenbank, indem Sie Entity Framework Migrationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="55768-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="55768-198">Sie werden feststellen, wie einfach es ist, das Modell zu ändern und die Änderungen in ihren Datenbanken mithilfe der Entity Framework Migrations Funktion widerzuspiegeln.</span><span class="sxs-lookup"><span data-stu-id="55768-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="55768-199">Öffnen Sie die Paket-Manager-Konsole.</span><span class="sxs-lookup"><span data-stu-id="55768-199">Open the Package Manager Console.</span></span> <span data-ttu-id="55768-200">Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="55768-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="55768-201">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="55768-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="55768-202">PMC</span><span class="sxs-lookup"><span data-stu-id="55768-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="55768-203">![Aktivieren von Migrationen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Aktivieren von Migrationen")</span><span class="sxs-lookup"><span data-stu-id="55768-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="55768-204">*Aktivieren von Migrationen*</span><span class="sxs-lookup"><span data-stu-id="55768-204">*Enabling migrations*</span></span>

    <span data-ttu-id="55768-205">Mit dem Befehl enable-Migration wird der Ordner **Migrationen** erstellt, in dem ein Skript zum Initialisieren der Datenbank enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="55768-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="55768-206">![Migrations Ordner](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations Ordner")</span><span class="sxs-lookup"><span data-stu-id="55768-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="55768-207">*Migrations Ordner*</span><span class="sxs-lookup"><span data-stu-id="55768-207">*Migrations folder*</span></span>
3. <span data-ttu-id="55768-208">Öffnen Sie die Datei **Configuration.cs** im Migrations Ordner.</span><span class="sxs-lookup"><span data-stu-id="55768-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="55768-209">Suchen Sie den Klassenkonstruktor, und ändern Sie den Wert **automaticmigrationsenabled** in *true*.</span><span class="sxs-lookup"><span data-stu-id="55768-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="55768-210">Öffnen Sie die Person-Klasse, und fügen Sie ein-Attribut für den Vornamen der Person hinzu.</span><span class="sxs-lookup"><span data-stu-id="55768-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="55768-211">Mit diesem neuen Attribut ändern Sie das Modell.</span><span class="sxs-lookup"><span data-stu-id="55768-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="55768-212">**Build auswählen | Erstellen** Sie die Projekt Mappe im Menü, um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="55768-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="55768-213">![Erstellen der Anwendung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Erstellen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="55768-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="55768-214">*Erstellen der Anwendung*</span><span class="sxs-lookup"><span data-stu-id="55768-214">*Building the application*</span></span>
6. <span data-ttu-id="55768-215">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="55768-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="55768-216">PMC</span><span class="sxs-lookup"><span data-stu-id="55768-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="55768-217">Mit diesem Befehl wird nach Änderungen in den Datenobjekten gesucht, und dann werden die erforderlichen Befehle zum Ändern der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="55768-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="55768-218">![Hinzufügen eines mittleren namens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Hinzufügen eines mittleren namens")</span><span class="sxs-lookup"><span data-stu-id="55768-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="55768-219">*Hinzufügen eines mittleren namens*</span><span class="sxs-lookup"><span data-stu-id="55768-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="55768-220">Optionale Sie können den folgenden Befehl ausführen, um ein SQL-Skript mit dem differenziellen Update zu generieren.</span><span class="sxs-lookup"><span data-stu-id="55768-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="55768-221">Auf diese Weise können Sie die Datenbank manuell aktualisieren (in diesem Fall nicht erforderlich) oder die Änderungen in anderen Datenbanken anwenden:</span><span class="sxs-lookup"><span data-stu-id="55768-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="55768-222">PMC</span><span class="sxs-lookup"><span data-stu-id="55768-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="55768-223">![Erstellen eines SQL-Skripts](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generieren eines SQL-Skripts")</span><span class="sxs-lookup"><span data-stu-id="55768-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="55768-224">*Erstellen eines SQL-Skripts*</span><span class="sxs-lookup"><span data-stu-id="55768-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="55768-225">![SQL-Skript Aktualisierung](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL-Skript Aktualisierung")</span><span class="sxs-lookup"><span data-stu-id="55768-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="55768-226">*SQL-Skript Aktualisierung*</span><span class="sxs-lookup"><span data-stu-id="55768-226">*SQL Script update*</span></span>
8. <span data-ttu-id="55768-227">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein, um die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="55768-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="55768-228">PMC</span><span class="sxs-lookup"><span data-stu-id="55768-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="55768-229">![Aktualisieren der Datenbank](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Aktualisieren der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="55768-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="55768-230">*Aktualisieren der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="55768-230">*Updating the Database*</span></span>

    <span data-ttu-id="55768-231">Dadurch wird die **MiddleName** -Spalte in der **People** -Tabelle hinzugefügt, um der aktuellen Definition der **Person** -Klasse zu entsprechen.</span><span class="sxs-lookup"><span data-stu-id="55768-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="55768-232">Nachdem die Datenbank aktualisiert wurde, klicken Sie mit der rechten Maustaste auf den Ordner Controller, und wählen Sie **Hinzufügen | Controller** zum erneuten Hinzufügen des Person-Controllers (mit denselben Werten vervollständigen).</span><span class="sxs-lookup"><span data-stu-id="55768-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="55768-233">Dadurch werden die vorhandenen Methoden und Sichten aktualisiert, die das neue Attribut hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="55768-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="55768-234">![Hinzufügen eines Controller Updates](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Hinzufügen eines Controller Updates")</span><span class="sxs-lookup"><span data-stu-id="55768-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="55768-235">*Aktualisieren des Controllers*</span><span class="sxs-lookup"><span data-stu-id="55768-235">*Updating the controller*</span></span>
10. <span data-ttu-id="55768-236">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="55768-236">Click **Add**.</span></span> <span data-ttu-id="55768-237">Wählen Sie dann die Werte **Überschreibungs PersonController.cs** und zugeordnete **Sichten überschreiben** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="55768-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Hinzufügen eines Controller-überschreiben](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="55768-239">*Aktualisieren des Controllers*</span><span class="sxs-lookup"><span data-stu-id="55768-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="55768-240">Task4-Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="55768-240">Task4- Running the application</span></span>

1. <span data-ttu-id="55768-241">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="55768-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="55768-242">Öffnen Sie **/Person**.</span><span class="sxs-lookup"><span data-stu-id="55768-242">Open **/Person**.</span></span> <span data-ttu-id="55768-243">Beachten Sie, dass die Daten beibehalten wurden, während die Spalte "Middle Name" hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="55768-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="55768-244">![Mittlerer Name hinzugefügt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Mittlerer Name hinzugefügt")</span><span class="sxs-lookup"><span data-stu-id="55768-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="55768-245">*Mittlerer Name hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="55768-245">*Middle Name added*</span></span>
3. <span data-ttu-id="55768-246">Wenn Sie auf **Bearbeiten**klicken, können Sie der aktuellen Person einen mittleren Namen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="55768-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="55768-247">![Edition des mittleren namens](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Edition des mittleren namens")</span><span class="sxs-lookup"><span data-stu-id="55768-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="55768-248">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="55768-248">Summary</span></span>

<span data-ttu-id="55768-249">In dieser praktischen Übungseinheit haben Sie einfache Schritte zum Erstellen von CRUD-Vorgängen mit ASP.NET MVC 4-Gerüstbau mithilfe einer beliebigen Modell Klasse kennengelernt.</span><span class="sxs-lookup"><span data-stu-id="55768-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="55768-250">Anschließend haben Sie gelernt, wie Sie ein End-to-End-Update in Ihrer Anwendung durchführen, und zwar von der Datenbank zu den Sichten, indem Sie Entity Framework Migrationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="55768-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="55768-251">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="55768-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="55768-252">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="55768-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="55768-253">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="55768-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="55768-254">Wechseln Sie zu [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="55768-254">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="55768-255">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="55768-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="55768-256">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="55768-256">Click on **Install Now**.</span></span> <span data-ttu-id="55768-257">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="55768-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="55768-258">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="55768-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="55768-259">![Installieren von Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="55768-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="55768-260">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="55768-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="55768-261">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="55768-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="55768-263">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="55768-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="55768-264">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="55768-264">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="55768-266">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="55768-266">*Installation progress*</span></span>
6. <span data-ttu-id="55768-267">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="55768-267">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="55768-269">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="55768-269">*Installation completed*</span></span>
7. <span data-ttu-id="55768-270">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="55768-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="55768-271">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="55768-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="55768-273">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="55768-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="55768-274">Anhang B: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="55768-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="55768-275">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="55768-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="55768-276">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="55768-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="55768-277">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="55768-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="55768-278">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="55768-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="55768-279">***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***</span><span class="sxs-lookup"><span data-stu-id="55768-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="55768-280">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="55768-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="55768-281">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="55768-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="55768-282">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="55768-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="55768-283">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="55768-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="55768-284">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="55768-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="55768-285">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="55768-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="55768-286">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="55768-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="55768-287">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="55768-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="55768-288">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="55768-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="55768-289">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="55768-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="55768-290">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="55768-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="55768-291">So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="55768-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="55768-292">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="55768-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="55768-293">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="55768-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="55768-294">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="55768-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="55768-295">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="55768-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="55768-296">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="55768-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="55768-297">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="55768-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="55768-298">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="55768-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
