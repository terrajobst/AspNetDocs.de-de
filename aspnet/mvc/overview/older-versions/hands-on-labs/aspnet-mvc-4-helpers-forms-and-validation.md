---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4-Hilfsprogramme, Formulare und Validierung | Microsoft-Dokumentation
author: rick-anderson
description: In ASP.NET MVC 4-Modellen und in der praktischen Übungseinheit für den Datenzugriff haben Sie Daten aus der Datenbank geladen und angezeigt. In dieser praktischen Übungseinheit fügen Sie den...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433791"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="e4b97-104">ASP.NET MVC 4 – Hilfsprogramme, Formulare und Überprüfung</span><span class="sxs-lookup"><span data-stu-id="e4b97-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="e4b97-105">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e4b97-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e4b97-106">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="e4b97-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e4b97-107">In **ASP.NET MVC 4-Modellen und** in der praktischen Übungseinheit für den Datenzugriff haben Sie Daten aus der Datenbank geladen und angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="e4b97-108">In dieser praktischen Übungseinheit fügen Sie der **Music Store** -Anwendung die Möglichkeit hinzu, diese Daten zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="e4b97-109">Mit diesem Ziel erstellen Sie zunächst den Controller, der die Aktionen zum Erstellen, lesen, aktualisieren und löschen (CRUD) von Alben unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="e4b97-110">Sie generieren eine Index Ansichts Vorlage, die die ASP.NET MVC-Gerüstbau Funktion nutzt, um die Eigenschaften der Alben in einer HTML-Tabelle anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="e4b97-111">Um diese Ansicht zu erweitern, fügen Sie ein benutzerdefiniertes HTML-Hilfsobjekt hinzu, mit dem lange Beschreibungen abgeschnitten werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="e4b97-112">Anschließend fügen Sie die Ansichten "Bearbeiten" und "erstellen" hinzu, mit denen Sie die Alben in der Datenbank mit Hilfe von Formular Elementen wie Dropdown Listen ändern können.</span><span class="sxs-lookup"><span data-stu-id="e4b97-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="e4b97-113">Schließlich können Sie Benutzer ein Album löschen und verhindern, dass Sie falsche Daten eingeben, indem Sie Ihre Eingaben validieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="e4b97-114">Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse von **ASP.NET MVC**verfügen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="e4b97-115">Wenn Sie **ASP.NET MVC** noch nicht verwendet haben, empfehlen wir Ihnen, **ASP.NET MVC-Grundlagen** im praktischen Lab zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="e4b97-116">Diese Übungseinheit führt Sie durch die Verbesserungen und neuen Features, die Sie zuvor beschrieben haben, indem Sie kleinere Änderungen an einer im Quellordner bereitgestellten beispielweb Anwendung anwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="e4b97-117">Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e4b97-118">Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Hilfsprogramme, Formularen und Validierung](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e4b97-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e4b97-119">Ziele</span><span class="sxs-lookup"><span data-stu-id="e4b97-119">Objectives</span></span>

<span data-ttu-id="e4b97-120">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4b97-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="e4b97-121">Erstellen eines Controllers zur Unterstützung von CRUD-Vorgängen</span><span class="sxs-lookup"><span data-stu-id="e4b97-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="e4b97-122">Generieren einer Index Sicht zum Anzeigen von Entitäts Eigenschaften in einer HTML-Tabelle</span><span class="sxs-lookup"><span data-stu-id="e4b97-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="e4b97-123">Hinzufügen benutzerdefinierter HTML-Hilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="e4b97-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="e4b97-124">Erstellen und Anpassen einer Bearbeitungs Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="e4b97-125">Unterscheiden zwischen Aktionsmethoden, die entweder auf HTTP-GET oder http-Post-Aufrufe reagieren</span><span class="sxs-lookup"><span data-stu-id="e4b97-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="e4b97-126">Hinzufügen und Anpassen einer CREATE VIEW</span><span class="sxs-lookup"><span data-stu-id="e4b97-126">Add and customize a Create View</span></span>
- <span data-ttu-id="e4b97-127">Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="e4b97-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="e4b97-128">Validieren von Benutzereingaben</span><span class="sxs-lookup"><span data-stu-id="e4b97-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e4b97-129">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="e4b97-129">Prerequisites</span></span>

<span data-ttu-id="e4b97-130">Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:</span><span class="sxs-lookup"><span data-stu-id="e4b97-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e4b97-131">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).</span><span class="sxs-lookup"><span data-stu-id="e4b97-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e4b97-132">Einrichten</span><span class="sxs-lookup"><span data-stu-id="e4b97-132">Setup</span></span>

<span data-ttu-id="e4b97-133">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="e4b97-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="e4b97-134">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e4b97-135">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e4b97-136">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang B: Verwenden von Code Ausschnitten](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e4b97-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e4b97-137">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="e4b97-137">Exercises</span></span>

<span data-ttu-id="e4b97-138">Die folgenden Übungen bilden diese praktische Übungseinheit:</span><span class="sxs-lookup"><span data-stu-id="e4b97-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="e4b97-139">Erstellen des Store Manager-Controllers und seiner Index Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="e4b97-140">Hinzufügen eines HTML-Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="e4b97-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="e4b97-141">Erstellen der Bearbeitungs Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="e4b97-142">Hinzufügen einer CREATE VIEW</span><span class="sxs-lookup"><span data-stu-id="e4b97-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="e4b97-143">Behandeln der Löschung</span><span class="sxs-lookup"><span data-stu-id="e4b97-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="e4b97-144">Hinzufügen der Validierung</span><span class="sxs-lookup"><span data-stu-id="e4b97-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="e4b97-145">Verwenden von unaufdringlichen jQuery auf Client Seite</span><span class="sxs-lookup"><span data-stu-id="e4b97-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="e4b97-146">Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e4b97-147">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="e4b97-148">Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**</span><span class="sxs-lookup"><span data-stu-id="e4b97-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="e4b97-149">Übung 1: Erstellen des Store Manager-Controllers und seiner Index Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="e4b97-150">In dieser Übung erfahren Sie, wie Sie einen neuen Controller für die Unterstützung von CRUD-Vorgängen erstellen, seine Index Aktionsmethode anpassen, um eine Liste der Alben aus der Datenbank zurückzugeben, und schließlich eine Index Ansichts Vorlage erzeugen, die den ASP.NET MVC-Gerüstbau nutzt. Funktion, mit der die Eigenschaften der Alben in einer HTML-Tabelle angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="e4b97-151">Aufgabe 1: Erstellen von storemanagercontroller</span><span class="sxs-lookup"><span data-stu-id="e4b97-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="e4b97-152">In dieser Aufgabe erstellen Sie einen neuen Controller mit dem Namen **storemanagercontroller** , um CRUD-Vorgänge zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="e4b97-153">Öffnen Sie **die Projekt** Mappe "Start", die sich unter **Quelle/EX1-kreatingderstoremanagercontroller/BEGIN/** Folder befindet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="e4b97-154">Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-155">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-156">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-157">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-158">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-159">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-160">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-161">Fügen Sie einen neuen Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="e4b97-161">Add a new controller.</span></span> <span data-ttu-id="e4b97-162">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Controllers** innerhalb der Projektmappen-Explorer, und wählen Sie **Hinzufügen** und dann den **Controller** -Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="e4b97-163">Ändern Sie den **Controller** **Namen** in **storemanagercontroller** , und stellen Sie sicher, dass die Option **MVC-Controller mit leeren Lese-/Schreibaktionen** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="e4b97-164">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-164">Click **Add**.</span></span>

    <span data-ttu-id="e4b97-165">![Controller Dialog hinzufügen](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Controller Dialog hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="e4b97-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="e4b97-166">*Controller Dialog hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="e4b97-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="e4b97-167">Eine neue Controller Klasse wird generiert.</span><span class="sxs-lookup"><span data-stu-id="e4b97-167">A new Controller class is generated.</span></span> <span data-ttu-id="e4b97-168">Da Sie angegeben haben, dass Aktionen für Lese-/Schreibzugriff hinzugefügt werden, werden Stubmethoden für diese, häufige CRUD-Aktionen erstellt, bei denen TODO-Kommentare ausgefüllt sind, um die anwendungsspezifische Logik einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="e4b97-169">Aufgabe 2: Anpassen des StoreManager-Indexes</span><span class="sxs-lookup"><span data-stu-id="e4b97-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="e4b97-170">In dieser Aufgabe wird die Aktionsmethode des StoreManager-Indexes angepasst, um eine Ansicht mit der Liste der Alben aus der Datenbank zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="e4b97-171">Fügen Sie in der storemanagercontroller-Klasse die folgenden *using* -Direktiven hinzu.</span><span class="sxs-lookup"><span data-stu-id="e4b97-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="e4b97-172">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-EX1 mithilfe von mvcmusicstore*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="e4b97-173">Fügen Sie dem **storemanagercontroller** ein Feld hinzu, um eine Instanz von " **musicstoreentities** " zu speichern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="e4b97-174">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und-Formulare und-Validierung-EX1 musicstoreentities*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="e4b97-175">Implementieren Sie die storemanagercontroller-Index Aktion, um eine Ansicht mit der Liste der Alben zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="e4b97-176">Die Aktions Logik des Controllers ähnelt der zuvor geschriebenen Index Aktion von StoreController.</span><span class="sxs-lookup"><span data-stu-id="e4b97-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="e4b97-177">Verwenden Sie LINQ, um alle Alben abzurufen, einschließlich Genre-und Künstlerinformationen für die Anzeige.</span><span class="sxs-lookup"><span data-stu-id="e4b97-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="e4b97-178">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-EX1 storemanagercontroller-Index*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="e4b97-179">Aufgabe 3: Erstellen der Index Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="e4b97-180">In dieser Aufgabe erstellen Sie die Vorlage Index Ansicht, um die Liste der vom **StoreManager** -Controller zurückgegebenen Alben anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="e4b97-181">Vor dem Erstellen der neuen Ansichts Vorlage sollten Sie das Projekt so erstellen, dass das **Dialog Feld "Ansicht hinzufügen** " die zu verwendende **Album** Klasse kennt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="e4b97-182">**Build auswählen | Erstellen Sie mvcmusicstore** , um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="e4b97-183">Klicken Sie mit der rechten Maustaste in die **Index** Aktionsmethode, und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="e4b97-184">Dadurch wird das Dialog **Feld Ansicht hinzufügen** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="e4b97-185">![Ansicht hinzufügen](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ansicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="e4b97-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="e4b97-186">*Hinzufügen einer Sicht aus der Index-Methode*</span><span class="sxs-lookup"><span data-stu-id="e4b97-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="e4b97-187">Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Ansichts Name **Index**lautet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="e4b97-188">Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus, und wählen Sie in der Dropdown-Datei der **Modell Klasse** die Option **Album (mvcmusicstore. Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="e4b97-189">Wählen Sie in der Dropdown Liste **Gerüst Vorlage** die Option **Liste** aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="e4b97-190">Belassen Sie die **Ansichts-Engine** **Razor** und die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="e4b97-191">![Hinzufügen einer Index Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Hinzufügen einer Index Ansicht")</span><span class="sxs-lookup"><span data-stu-id="e4b97-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="e4b97-192">*Hinzufügen einer Index Ansicht*</span><span class="sxs-lookup"><span data-stu-id="e4b97-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="e4b97-193">Aufgabe 4: Anpassen des Gerüsts der Index Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="e4b97-194">In dieser Aufgabe passen Sie die einfache Ansichts Vorlage an, die mit ASP.NET MVC-Gerüstbau Funktion erstellt wurde, damit die gewünschten Felder angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="e4b97-195">Die **Gerüstbau** Unterstützung in ASP.NET MVC generiert eine einfache Ansichts Vorlage, die alle Felder im Album Modell auflistet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="e4b97-196">**Gerüstbau** bietet einen schnellen Einstieg in eine stark typisierte Ansicht: statt die Ansichts Vorlage manuell schreiben zu müssen, generiert Gerüstbau schnell eine Standardvorlage, und Sie können den generierten Code ändern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>

1. <span data-ttu-id="e4b97-197">Überprüfen Sie den erstellten Code.</span><span class="sxs-lookup"><span data-stu-id="e4b97-197">Review the code created.</span></span> <span data-ttu-id="e4b97-198">Die generierte Liste der Felder ist Teil der folgenden HTML-Tabelle, die von **Gerüstbau** zum Anzeigen von Tabellendaten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="e4b97-199">Ersetzen Sie die **&lt;Tabelle&gt;** Code durch den folgenden Code, um nur die Felder **Genre**, **Maler**, **Album Titel**und **Preis** anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="e4b97-200">Hierdurch werden die Spalten " **albumId** " und " **Album** " angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="e4b97-201">Außerdem werden die Spalten GenreID und artistId geändert, um die Eigenschaften der verknüpften Klasse von **Artist.Name** und **Genre.Name**anzuzeigen und den Link **Details** zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="e4b97-202">Ändern Sie die folgenden Beschreibungen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e4b97-203">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="e4b97-204">In dieser Aufgabe überprüfen Sie, ob die Vorlage für die Vorlage " **StoreManager** **Index** View" nach dem Entwurf der vorherigen Schritte eine Liste von Alben anzeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="e4b97-205">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-206">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-206">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-207">Ändern Sie die URL in **/StoreManager** , um zu überprüfen, ob eine Liste mit Alben angezeigt wird, die Ihren **Titel**, Ihren **Künstler** und Ihr **Genre**anzeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="e4b97-208">![Durchsuchen der Liste der Alben](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Durchsuchen der Liste der Alben")</span><span class="sxs-lookup"><span data-stu-id="e4b97-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="e4b97-209">*Durchsuchen der Liste der Alben*</span><span class="sxs-lookup"><span data-stu-id="e4b97-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="e4b97-210">Übung 2: Hinzufügen eines HTML-Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="e4b97-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="e4b97-211">Die Seite "StoreManager Index" hat ein mögliches Problem: die Eigenschaften "Titel" und "Name des Künstlers" können beide lang genug sein, um die Tabellen Formatierung auszulösen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="e4b97-212">In dieser Übung erfahren Sie, wie Sie ein benutzerdefiniertes HTML-Hilfsprogramm hinzufügen, um diesen Text abzuschneiden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="e4b97-213">In der folgenden Abbildung können Sie sehen, wie das Format aufgrund der Länge des Texts geändert wird, wenn Sie eine kleine Browsergröße verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="e4b97-214">![Durchsuchen der Liste der Alben ohne Abgeschnittener Text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Durchsuchen der Liste der Alben ohne Abgeschnittener Text")</span><span class="sxs-lookup"><span data-stu-id="e4b97-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="e4b97-215">*Durchsuchen der Liste der Alben ohne Abgeschnittener Text*</span><span class="sxs-lookup"><span data-stu-id="e4b97-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="e4b97-216">Aufgabe 1: Erweitern des HTML-Hilfsprogramms</span><span class="sxs-lookup"><span data-stu-id="e4b97-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="e4b97-217">In dieser Aufgabe fügen Sie dem **HTML** -Objekt, das in ASP.NET MVC-Ansichten verfügbar gemacht wird, eine **neue Methode hinzu** .</span><span class="sxs-lookup"><span data-stu-id="e4b97-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="e4b97-218">Zu diesem Zweck implementieren Sie eine **Erweiterungsmethode** für die integrierte **System. Web. MVC. htmlhelper** -Klasse, die von ASP.NET MVC bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="e4b97-219">Weitere Informationen zu **Erweiterungs Methoden**finden Sie in diesem MSDN-Artikel.</span><span class="sxs-lookup"><span data-stu-id="e4b97-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="e4b97-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4b97-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>

1. <span data-ttu-id="e4b97-221">Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX2-addinganhtmlhelper/BEGIN/** Folder".</span><span class="sxs-lookup"><span data-stu-id="e4b97-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="e4b97-222">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e4b97-223">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-224">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-225">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-226">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-227">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-228">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-229">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-230">Öffnen Sie die Index Ansicht von StoreManager.</span><span class="sxs-lookup"><span data-stu-id="e4b97-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="e4b97-231">Zu diesem Zweck erweitern Sie im Projektmappen-Explorer den Ordner " **views** " **, und öffnen Sie dann die Datei** " **Index. cshtml** ".</span><span class="sxs-lookup"><span data-stu-id="e4b97-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="e4b97-232">Fügen Sie den folgenden Code unterhalb der <strong>@model</strong> -Direktive hinzu, um die <strong>TRUNCATE</strong> Helper-Methode zu definieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="e4b97-233">Aufgabe 2: Abschneiden von Text auf der Seite</span><span class="sxs-lookup"><span data-stu-id="e4b97-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="e4b97-234">In dieser Aufgabe verwenden Sie die **TRUNCATE** -Methode, um den Text in der Ansichts Vorlage abzuschneiden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="e4b97-235">Öffnen Sie die Index Ansicht von StoreManager.</span><span class="sxs-lookup"><span data-stu-id="e4b97-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="e4b97-236">Zu diesem Zweck erweitern Sie im Projektmappen-Explorer den Ordner " **views** " **, und öffnen Sie dann die Datei** " **Index. cshtml** ".</span><span class="sxs-lookup"><span data-stu-id="e4b97-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="e4b97-237">Ersetzen Sie die Zeilen, die den **Namen des Künstlers** und den **Titel**des Albums anzeigen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="e4b97-238">Ersetzen Sie hierzu die folgenden Zeilen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e4b97-239">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="e4b97-240">In dieser Aufgabe testen Sie, ob die Vorlage " **StoreManager** **Index** View" den Titel und den Namen des Albums abschneidet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="e4b97-241">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-242">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-242">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-243">Ändern Sie die URL in **/StoreManager** , um zu überprüfen, ob lange Texte in der Spalte **Titel** und **Künstler** abgeschnitten sind.</span><span class="sxs-lookup"><span data-stu-id="e4b97-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="e4b97-244">![Abgeschnittene Titel und Künstlernamen](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Abgeschnittene Titel und Künstlernamen")</span><span class="sxs-lookup"><span data-stu-id="e4b97-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="e4b97-245">*Abgeschnittene Titel und Künstlernamen*</span><span class="sxs-lookup"><span data-stu-id="e4b97-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="e4b97-246">Übung 3: Erstellen der Bearbeitungs Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="e4b97-247">In dieser Übung erfahren Sie, wie Sie ein Formular erstellen, mit dem Store-Manager ein Album bearbeiten können.</span><span class="sxs-lookup"><span data-stu-id="e4b97-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="e4b97-248">Sie durchsucht die **/StoreManager/Edit/ID** -URL (**ID** ist die eindeutige ID des zu bearbeitenden Albums) und führt somit einen HTTP-GET-Befehl an den Server aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="e4b97-249">Die Controller Bearbeitungs Aktionsmethode Ruft das entsprechende Album aus der Datenbank ab, erstellt ein **storemanagerviewmodel** -Objekt, um es zu Kapseln (zusammen mit einer Liste von Künstlern und Genres), und übergibt es dann an eine Ansichts Vorlage, um die HTML-Seite wieder an den Benutzer zu rendern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="e4b97-250">Diese Seite enthält ein **&lt;Formular&gt;** Element mit Textfeldern und Dropdown Listen zum Bearbeiten der Album Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="e4b97-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="e4b97-251">Sobald der Benutzer die Album Formular Werte aktualisiert und auf die Schaltfläche **Speichern** klickt, werden die Änderungen über einen HTTP-Post-Rückruf an **/StoreManager/Edit/ID**übermittelt. Obwohl die URL mit der des letzten Aufrufes übereinstimmt, identifiziert ASP.NET MVC, dass es sich hierbei um eine HTTP-Post-Methode handelt, und führt daher eine andere Bearbeitungs Aktionsmethode aus (eine mit **[HttpPost]** versehen).</span><span class="sxs-lookup"><span data-stu-id="e4b97-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="e4b97-252">Aufgabe 1: Implementieren der HTTP-Get Edit-Aktionsmethode</span><span class="sxs-lookup"><span data-stu-id="e4b97-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="e4b97-253">In dieser Aufgabe implementieren Sie die HTTP-Get-Version der Edit-Aktionsmethode, um das entsprechende Album aus der Datenbank abzurufen, sowie eine Liste aller Genres und Künstler.</span><span class="sxs-lookup"><span data-stu-id="e4b97-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="e4b97-254">Diese Daten werden in das im letzten Schritt definierte **storemanagerviewmodel** -Objekt Paketieren, das dann an eine Ansichts Vorlage übergeben wird, um die Antwort mit zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="e4b97-255">Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX3-kreatingdie EditView/BEGIN/** Folder".</span><span class="sxs-lookup"><span data-stu-id="e4b97-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="e4b97-256">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e4b97-257">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-258">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-259">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-260">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-261">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-262">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-263">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-264">Öffnen Sie die **storemanagercontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="e4b97-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="e4b97-265">Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="e4b97-266">Ersetzen Sie die **HTTP-Get Edit** Action-Methode durch den folgenden Code, um das passende **Album** sowie die Listen **Genres** und **Künstler** abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="e4b97-267">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und-Formulare und-Validierung-EX3 storemanagercontroller HTTP-Get Edit Action*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-268">Sie verwenden " **System. Web. MVC** **SelectList** " für Künstler und Genres anstelle der Liste " **System. Collections. Generic** ".</span><span class="sxs-lookup"><span data-stu-id="e4b97-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="e4b97-269">**SelectList** ist eine saubere Möglichkeit zum Auffüllen von HTML-Dropdown Listen und zum Verwalten von Dingen wie der aktuellen Auswahl.</span><span class="sxs-lookup"><span data-stu-id="e4b97-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="e4b97-270">Durch das Instanziieren und spätere Einrichten dieser ViewModel-Objekte in der Controller Aktion wird das Bearbeitungs Formular Szenario bereinigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="e4b97-271">Aufgabe 2: Erstellen der Bearbeitungs Ansicht</span><span class="sxs-lookup"><span data-stu-id="e4b97-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="e4b97-272">In dieser Aufgabe erstellen Sie eine Vorlage zum Bearbeiten der Ansicht, in der die Album Eigenschaften später angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="e4b97-273">Erstellen Sie die Bearbeitungs Ansicht.</span><span class="sxs-lookup"><span data-stu-id="e4b97-273">Create the Edit View.</span></span> <span data-ttu-id="e4b97-274">Klicken Sie dazu mit der rechten Maustaste in die **Edit** -Aktionsmethode, und wählen Sie **Ansicht hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="e4b97-275">Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Name der Ansicht " **Bearbeiten**" lautet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="e4b97-276">Aktivieren Sie das Kontrollkästchen für **eine stark typisierte Ansicht erstellen** , und wählen Sie im Dropdown **Feld Datenklasse anzeigen** die Option **Album (mvcmusicstore. Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="e4b97-277">Wählen Sie im Dropdown Feld **Gerüst Vorlage** die Option **Bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="e4b97-278">Belassen Sie die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="e4b97-279">![Hinzufügen einer Bearbeitungs Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Hinzufügen einer Bearbeitungs Ansicht")</span><span class="sxs-lookup"><span data-stu-id="e4b97-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="e4b97-280">*Hinzufügen einer Bearbeitungs Ansicht*</span><span class="sxs-lookup"><span data-stu-id="e4b97-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e4b97-281">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="e4b97-282">In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** - **Bearbeitungs** Ansicht" die Eigenschaften Werte für das als Parameter übergebenen Album angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="e4b97-283">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-284">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-284">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-285">Ändern Sie die URL in **/StoreManager/Edit/1** , um zu überprüfen, ob die Eigenschaften Werte für das übergebenen Album angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="e4b97-286">![Anzeigen der Bearbeitungs Ansicht des Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Anzeigen der Bearbeitungs Ansicht des Albums")</span><span class="sxs-lookup"><span data-stu-id="e4b97-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="e4b97-287">*Anzeigen der Bearbeitungs Ansicht des Albums*</span><span class="sxs-lookup"><span data-stu-id="e4b97-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="e4b97-288">Aufgabe 4: Implementieren von Dropdown-Vorgängen für die Vorlage "Album-Editor"</span><span class="sxs-lookup"><span data-stu-id="e4b97-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="e4b97-289">In dieser Aufgabe fügen Sie der Ansichts Vorlage, die in der letzten Aufgabe erstellt wurde, Dropdown Listen hinzu, sodass der Benutzer aus einer Liste von Künstlern und Genres auswählen kann.</span><span class="sxs-lookup"><span data-stu-id="e4b97-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="e4b97-290">Ersetzen Sie den gesamten **Album** -FIELDSET-Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4b97-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-291">Ein **HTML. DropDownList** -Hilfsprogramm wurde zum Rendering von Dropdown Listen für die Auswahl von Künstlern und Genres hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="e4b97-292">Die an **HTML. DropDownList** über gebenden Parameter lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e4b97-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="e4b97-293">Der Name des Formular Felds ( **&quot;artistId&quot;** ).</span><span class="sxs-lookup"><span data-stu-id="e4b97-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="e4b97-294">Die **SelectList** der Werte für die Dropdown Liste.</span><span class="sxs-lookup"><span data-stu-id="e4b97-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e4b97-295">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="e4b97-296">In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** - **Bearbeitungs** Ansicht" Dropdowns anstelle von Textfeldern für die Felder "Interpret" und "Genre" angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="e4b97-297">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-298">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-298">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-299">Ändern Sie die URL in **/StoreManager/Edit/1** , um zu überprüfen, ob Dropdown-Zeichen anstelle von Textfeldern für Text und Genre-ID angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="e4b97-300">![Durchsuchen der Bearbeitungs Ansicht des Albums mit Dropdowns](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Durchsuchen der Bearbeitungs Ansicht des Albums mit Dropdowns")</span><span class="sxs-lookup"><span data-stu-id="e4b97-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="e4b97-301">*Anzeigen der Bearbeitungs Ansicht des Albums, dieses Mal mit Dropdown Listen*</span><span class="sxs-lookup"><span data-stu-id="e4b97-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="e4b97-302">Aufgabe 6: Implementieren der HTTP-Post-Bearbeitungs Aktionsmethode</span><span class="sxs-lookup"><span data-stu-id="e4b97-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="e4b97-303">Nun, da die Bearbeitungs Ansicht erwartungsgemäß angezeigt wird, müssen Sie die Methode "http-Post Edit Action" implementieren, um die am Album vorgenommenen Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="e4b97-304">Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="e4b97-305">Öffnen Sie **storemanagercontroller** aus dem Ordner **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="e4b97-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="e4b97-306">Ersetzen Sie den **http-Post Edit** Action-Methoden Code durch Folgendes (Beachten Sie, dass die Methode, die ersetzt werden muss, eine überladene Version ist, die zwei Parameter empfängt):</span><span class="sxs-lookup"><span data-stu-id="e4b97-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="e4b97-307">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-EX3 storemanagercontroller HTTP-Post Edit Action*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-308">Diese Methode wird ausgeführt, wenn der Benutzer auf die Schaltfläche **Save (speichern** ) der Ansicht klickt und eine HTTP-Post-Anweisung der Formular Werte an den Server ausführt, um Sie in der Datenbank beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="e4b97-309">Der Decorator **[HttpPost]** gibt an, dass die-Methode für diese HTTP-Post-Szenarien verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="e4b97-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="e4b97-310">Die-Methode nimmt ein **Album** -Objekt an.</span><span class="sxs-lookup"><span data-stu-id="e4b97-310">The method takes an **Album** object.</span></span> <span data-ttu-id="e4b97-311">ASP.NET MVC erstellt automatisch das Album-Objekt aus dem &lt;Formular&gt; Werte.</span><span class="sxs-lookup"><span data-stu-id="e4b97-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="e4b97-312">Die-Methode führt die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="e4b97-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="e4b97-313">Wenn das Modell gültig ist:</span><span class="sxs-lookup"><span data-stu-id="e4b97-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="e4b97-314">Aktualisieren Sie den Album Eintrag im Kontext, um ihn als geändertes Objekt zu markieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="e4b97-315">Speichern Sie die Änderungen, und leiten Sie Sie an die Index Sicht um.</span><span class="sxs-lookup"><span data-stu-id="e4b97-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="e4b97-316">Wenn das Modell nicht gültig ist, wird der viewbag mit dem **GenreID** und **artistId**aufgefüllt. Anschließend wird die Ansicht mit dem empfangenen Album-Objekt zurückgegeben, um dem Benutzer die Ausführung erforderlicher Updates zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="e4b97-317">Aufgabe 7: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="e4b97-318">In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager-Bearbeitungs** Ansicht" tatsächlich die aktualisierten Album Daten in der Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="e4b97-319">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-320">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-320">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-321">Ändern Sie die URL in **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="e4b97-322">Ändern Sie den Titel des Albums in **Laden** , und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="e4b97-323">Überprüfen Sie, ob der Titel des Albums in der Liste der Alben tatsächlich geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="e4b97-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="e4b97-324">![Aktualisieren eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Aktualisieren eines Albums")</span><span class="sxs-lookup"><span data-stu-id="e4b97-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="e4b97-325">*Aktualisieren eines Albums*</span><span class="sxs-lookup"><span data-stu-id="e4b97-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="e4b97-326">Übung 4: Hinzufügen einer CREATE VIEW</span><span class="sxs-lookup"><span data-stu-id="e4b97-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="e4b97-327">Nun, da der **storemanagercontroller** die **Bearbeitungs** Fähigkeit unterstützt, erfahren Sie in dieser Übung, wie Sie eine CREATE VIEW-Vorlage hinzufügen, um Store Manager neue Alben zur Anwendung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="e4b97-328">Wie bei den Bearbeitungsfunktionen implementieren Sie das Erstellungs Szenario mit zwei separaten Methoden in der **storemanagercontroller** -Klasse:</span><span class="sxs-lookup"><span data-stu-id="e4b97-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="e4b97-329">Eine Aktionsmethode zeigt ein leeres Formular an, wenn die Speicher-Manager die **/StoreManager/Create** -URL zum ersten Mal besuchen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="e4b97-330">Eine zweite Aktionsmethode behandelt das Szenario, in dem der Speicher-Manager im Formular auf die Schaltfläche **Speichern** klickt und die Werte als HTTP-Post an die **/StoreManager/Create** -URL übergibt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="e4b97-331">Aufgabe 1: Implementieren der HTTP-Get Create Action-Methode</span><span class="sxs-lookup"><span data-stu-id="e4b97-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="e4b97-332">In dieser Aufgabe implementieren Sie die HTTP-Get-Version der Create Action-Methode, um eine Liste aller Genres und Künstler abzurufen, Verpacken diese Daten in ein **storemanagerviewmodel** -Objekt, das dann an eine Ansichts Vorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="e4b97-333">Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/Ex4-addingacreateview/BEGIN/** Folder".</span><span class="sxs-lookup"><span data-stu-id="e4b97-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="e4b97-334">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e4b97-335">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-336">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-337">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-338">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-339">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-340">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-341">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-342">Öffnen Sie die **storemanagercontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="e4b97-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="e4b97-343">Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="e4b97-344">Ersetzen Sie den Code der **Create** Action-Methode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4b97-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="e4b97-345">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-Ex4 storemanagercontroller HTTP-Get Create Action*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="e4b97-346">Aufgabe 2: Hinzufügen der CREATE VIEW</span><span class="sxs-lookup"><span data-stu-id="e4b97-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="e4b97-347">In dieser Aufgabe fügen Sie die Vorlage CREATE VIEW hinzu, in der ein neues (leeres) Album Formular angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="e4b97-348">Klicken Sie mit der rechten Maustaste in die Methode **Create** Action, und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="e4b97-349">Dadurch wird das Dialogfeld Ansicht hinzufügen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="e4b97-350">Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Name der Sicht **Create**lautet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="e4b97-351">Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus, und wählen Sie in der Dropdown-Dropdown- **Vorlage** für **Modell Klasse** die Option **Album (mvcmusicstore. Models)** **aus.**</span><span class="sxs-lookup"><span data-stu-id="e4b97-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="e4b97-352">Belassen Sie die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="e4b97-353">![Hinzufügen einer CREATE VIEW](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-CREATE-VIEW. png")</span><span class="sxs-lookup"><span data-stu-id="e4b97-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="e4b97-354">*Hinzufügen der CREATE VIEW*</span><span class="sxs-lookup"><span data-stu-id="e4b97-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="e4b97-355">Aktualisieren Sie die Felder **GenreID** und **artistId** so, dass Sie eine Dropdown Liste verwenden, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="e4b97-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e4b97-356">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="e4b97-357">In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** **Create** View" ein leeres Album Formular angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="e4b97-358">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-359">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-359">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-360">Ändern Sie die URL in **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="e4b97-361">Überprüfen Sie, ob ein leeres Formular zum Ausfüllen der neuen Album Eigenschaften angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="e4b97-362">![Erstellen einer Ansicht mit einem leeren Formular](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Erstellen einer Ansicht mit einem leeren Formular")</span><span class="sxs-lookup"><span data-stu-id="e4b97-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="e4b97-363">*Erstellen einer Ansicht mit einem leeren Formular*</span><span class="sxs-lookup"><span data-stu-id="e4b97-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="e4b97-364">Aufgabe 4: Implementieren der HTTP-Post-Aktionsmethode "Create"</span><span class="sxs-lookup"><span data-stu-id="e4b97-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="e4b97-365">In dieser Aufgabe implementieren Sie die HTTP-Post-Version der Create Action-Methode, die aufgerufen wird, wenn ein Benutzer auf die Schaltfläche **Speichern** klickt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="e4b97-366">Die-Methode sollte das neue Album in der-Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="e4b97-367">Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="e4b97-368">Öffnen Sie die **storemanagercontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="e4b97-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="e4b97-369">Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="e4b97-370">Ersetzen Sie den Code der Create Action-Methode von **http-Post** durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4b97-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="e4b97-371">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung-Ex4 storemanagercontroller HTTP-Post Create Action*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-372">Die CREATE-Aktion ähnelt der vorherigen Edit-Aktionsmethode, aber anstatt das-Objekt als geändert festzulegen, wird es dem Kontext hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="e4b97-373">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="e4b97-374">In dieser Aufgabe testen Sie, ob Sie auf der Seite " **StoreManager Create** View" ein neues Album erstellen und dann zur Ansicht "StoreManager-Index" umgeleitet werden können.</span><span class="sxs-lookup"><span data-stu-id="e4b97-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="e4b97-375">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-376">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-376">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-377">Ändern Sie die URL in **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="e4b97-378">Füllen Sie alle Formularfelder mit Daten für ein neues Album aus, wie in der folgenden Abbildung dargestellt:</span><span class="sxs-lookup"><span data-stu-id="e4b97-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="e4b97-379">![Erstellen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Erstellen eines Albums")</span><span class="sxs-lookup"><span data-stu-id="e4b97-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="e4b97-380">*Erstellen eines Albums*</span><span class="sxs-lookup"><span data-stu-id="e4b97-380">*Creating an Album*</span></span>
3. <span data-ttu-id="e4b97-381">Vergewissern Sie sich, dass Sie zur Ansicht "StoreManager Index" umgeleitet werden, die das soeben erstellte neue Album enthält.</span><span class="sxs-lookup"><span data-stu-id="e4b97-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="e4b97-382">![Neues Album erstellt](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Neues Album erstellt")</span><span class="sxs-lookup"><span data-stu-id="e4b97-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="e4b97-383">*Neues Album erstellt*</span><span class="sxs-lookup"><span data-stu-id="e4b97-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="e4b97-384">Übung 5: Behandeln von Lösch Vorgängen</span><span class="sxs-lookup"><span data-stu-id="e4b97-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="e4b97-385">Die Möglichkeit zum Löschen von Alben ist noch nicht implementiert.</span><span class="sxs-lookup"><span data-stu-id="e4b97-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="e4b97-386">Im folgenden wird diese Übung verwendet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-386">This is what this exercise will be about.</span></span> <span data-ttu-id="e4b97-387">Wie zuvor implementieren Sie das Lösch Szenario mit zwei separaten Methoden in der **storemanagercontroller** -Klasse:</span><span class="sxs-lookup"><span data-stu-id="e4b97-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="e4b97-388">Eine Aktionsmethode zeigt ein Bestätigungsformular an</span><span class="sxs-lookup"><span data-stu-id="e4b97-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="e4b97-389">Eine zweite Aktionsmethode behandelt die Formular Übermittlung.</span><span class="sxs-lookup"><span data-stu-id="e4b97-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="e4b97-390">Aufgabe 1: Implementieren der HTTP-Get DELETE-Aktionsmethode</span><span class="sxs-lookup"><span data-stu-id="e4b97-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="e4b97-391">In dieser Aufgabe implementieren Sie die HTTP-Get-Version der DELETE-Aktionsmethode, um die Informationen zum Album abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="e4b97-392">Öffnen Sie die Projekt Mappe " **Anfang** " unter **Quelle/EX5-handlinglöschung/Anfang/** Ordner.</span><span class="sxs-lookup"><span data-stu-id="e4b97-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="e4b97-393">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e4b97-394">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-395">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-396">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-397">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-398">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-399">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-400">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-401">Öffnen Sie die **storemanagercontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="e4b97-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="e4b97-402">Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="e4b97-403">Die Aktion zum Löschen des Controllers ist exakt identisch mit der vorherigen Aktion für den Speicher Details-Controller: Sie fragt das **Album** -Objekt mithilfe der in der URL angegebenen **ID** aus der Datenbank ab und gibt die entsprechende **Ansicht**zurück.</span><span class="sxs-lookup"><span data-stu-id="e4b97-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="e4b97-404">Ersetzen Sie hierzu den Methoden Code "http-get **Delete** Action" durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4b97-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="e4b97-405">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung EX5 Behandlung von Lösch Vorgängen HTTP-Get delete action*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="e4b97-406">Klicken Sie mit der rechten Maustaste in die Methode **Delete** Action, und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="e4b97-407">Dadurch wird das Dialogfeld Ansicht hinzufügen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="e4b97-408">Überprüfen Sie im Dialogfeld Ansicht hinzufügen, ob der Name der Sicht **gelöscht**ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="e4b97-409">Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus, und wählen Sie in der Dropdown-Datei der **Modell Klasse** die Option **Album (mvcmusicstore. Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="e4b97-410">Wählen Sie aus der Dropdown- **Vorlage für Gerüst Vorlagen** die Option **Löschen** aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="e4b97-411">Belassen Sie die anderen Felder mit ihrem Standardwert, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="e4b97-412">![Hinzufügen einer Lösch Ansicht](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Hinzufügen einer Lösch Ansicht")</span><span class="sxs-lookup"><span data-stu-id="e4b97-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="e4b97-413">*Hinzufügen einer Lösch Ansicht*</span><span class="sxs-lookup"><span data-stu-id="e4b97-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="e4b97-414">Die Vorlage löschen zeigt alle Felder aus dem Modell an.</span><span class="sxs-lookup"><span data-stu-id="e4b97-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="e4b97-415">Es wird nur der Titel des Albums angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-415">You will show only the album's title.</span></span> <span data-ttu-id="e4b97-416">Ersetzen Sie dazu den Inhalt der Sicht durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e4b97-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="e4b97-417">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="e4b97-418">In dieser Aufgabe testen Sie, ob auf der Seite " **StoreManager** **Delete** View" (Löschvorgang löschen) ein Bestätigungsformular angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="e4b97-419">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-420">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-420">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-421">Ändern Sie die URL in **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="e4b97-422">Wählen Sie ein zu Lösch Endes Album aus, indem Sie auf **Löschen** klicken und überprüfen, ob die neue Ansicht hochgeladen</span><span class="sxs-lookup"><span data-stu-id="e4b97-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="e4b97-423">![Löschen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Löschen eines Albums")</span><span class="sxs-lookup"><span data-stu-id="e4b97-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="e4b97-424">*Löschen eines Albums*</span><span class="sxs-lookup"><span data-stu-id="e4b97-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="e4b97-425">Aufgabe 3: Implementieren der HTTP-Post-Aktionsmethode "Delete"</span><span class="sxs-lookup"><span data-stu-id="e4b97-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="e4b97-426">In dieser Aufgabe implementieren Sie die HTTP-Post-Version der DELETE-Aktionsmethode, die aufgerufen wird, wenn ein Benutzer auf die Schaltfläche " **Löschen** " klickt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="e4b97-427">Die-Methode sollte das-Album in der-Datenbank löschen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="e4b97-428">Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="e4b97-429">Öffnen Sie die **storemanagercontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="e4b97-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="e4b97-430">Erweitern Sie hierzu den Ordner **Controller** , und doppelklicken Sie auf **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="e4b97-431">Ersetzen Sie den Code der Methode " **http-Post DELETE** Action" durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e4b97-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="e4b97-432">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und Formulare und Validierung EX5 Behandlung von Lösch Vorgängen HTTP-Post DELETE-Aktion*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e4b97-433">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="e4b97-434">In dieser Aufgabe testen Sie, ob Sie auf der Seite " **StoreManager DELETE** View" ein Album löschen und dann zur Ansicht "StoreManager-Index" umgeleitet werden können.</span><span class="sxs-lookup"><span data-stu-id="e4b97-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="e4b97-435">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-436">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-436">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-437">Ändern Sie die URL in **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="e4b97-438">Klicken Sie auf löschen, um ein zu Lösch Endes Album auszuwählen **.**</span><span class="sxs-lookup"><span data-stu-id="e4b97-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="e4b97-439">Bestätigen Sie den Löschvorgang durch Klicken auf die Schaltfläche **Löschen**</span><span class="sxs-lookup"><span data-stu-id="e4b97-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="e4b97-440">![Löschen eines Albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Löschen eines Albums")</span><span class="sxs-lookup"><span data-stu-id="e4b97-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="e4b97-441">*Löschen eines Albums*</span><span class="sxs-lookup"><span data-stu-id="e4b97-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="e4b97-442">Vergewissern Sie sich, dass das Album gelöscht wurde, da es nicht auf der **Index** Seite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="e4b97-443">Übung 6: Hinzufügen der Validierung</span><span class="sxs-lookup"><span data-stu-id="e4b97-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="e4b97-444">Derzeit führen die von Ihnen erstellten Formulare zum Erstellen und bearbeiten keine Überprüfung durch.</span><span class="sxs-lookup"><span data-stu-id="e4b97-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="e4b97-445">Wenn der Benutzer ein Pflichtfeld leer lässt oder Buchstaben im Feld Price (Preis) eingeben, wird der erste Fehler, den Sie erhalten, von der Datenbank ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="e4b97-446">Sie können der Anwendung eine Validierung hinzufügen, indem Sie Ihrer Modell Klasse Daten Anmerkungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="e4b97-447">Mit Daten Anmerkungen können Sie die Regeln beschreiben, die Sie auf die Modell Eigenschaften anwenden möchten, und ASP.NET MVC übernimmt die Erzwingung und Anzeige entsprechender Nachrichten für Benutzer.</span><span class="sxs-lookup"><span data-stu-id="e4b97-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="e4b97-448">Aufgabe 1: Hinzufügen von Daten Anmerkungen</span><span class="sxs-lookup"><span data-stu-id="e4b97-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="e4b97-449">In dieser Aufgabe fügen Sie dem Album Modelldaten Anmerkungen hinzu, mit denen die Seite erstellen und bearbeiten bei Bedarf Validierungs Meldungen anzeigen kann.</span><span class="sxs-lookup"><span data-stu-id="e4b97-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="e4b97-450">Bei einer einfachen Modell Klasse wird das Hinzufügen einer Daten Anmerkung nur durch Hinzufügen einer **using** -Anweisung für **System. ComponentModel. DataAnnotation**behandelt. Anschließend wird ein **[Required]** -Attribut in den entsprechenden Eigenschaften platziert.</span><span class="sxs-lookup"><span data-stu-id="e4b97-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="e4b97-451">Im folgenden Beispiel wird die **Name** -Eigenschaft als Pflichtfeld in der Ansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="e4b97-452">Dies ist ein wenig komplexer in Fällen wie diese Anwendung, in der die Entity Data Model generiert wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="e4b97-453">Wenn Sie Daten Anmerkungen direkt zu den Modellklassen hinzugefügt haben, werden Sie überschrieben, wenn Sie das Modell aus der Datenbank aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="e4b97-454">Stattdessen können Sie metadatenpartielle Klassen verwenden, die zum Speichern der Anmerkungen vorhanden sind und den Modellklassen mit dem **[MetadataType]** -Attribut zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="e4b97-455">Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Quelle/Ex6-addingvalidation/BEGIN/** Folder".</span><span class="sxs-lookup"><span data-stu-id="e4b97-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="e4b97-456">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e4b97-457">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-458">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-459">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-460">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-461">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-462">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-463">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-464">Öffnen Sie die **Album.cs** aus dem Ordner " **Models** ".</span><span class="sxs-lookup"><span data-stu-id="e4b97-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="e4b97-465">Ersetzen Sie **Album.cs** -Inhalt durch den hervorgehobenen Code, sodass er wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="e4b97-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4b97-466">Die Zeile **[Display Format (ConvertEmptyStringToNull = false)]** gibt an, dass leere Zeichen folgen aus dem Modell nicht in NULL konvertiert werden, wenn das Datenfeld in der Datenquelle aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="e4b97-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="e4b97-467">Mit dieser Einstellung wird eine Ausnahme vermieden, wenn die Entity Framework dem Modell NULL-Werte zuweist, bevor die Daten Anmerkung die Felder überprüft.</span><span class="sxs-lookup"><span data-stu-id="e4b97-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="e4b97-468">(Code Ausschnitt- *ASP.NET MVC 4-Hilfsprogramme und-Formulare und-Validierung Ex6, partielle Klasse von Album-Metadaten*)</span><span class="sxs-lookup"><span data-stu-id="e4b97-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-469">Diese partielle **Album** Klasse verfügt über ein **metadataType** -Attribut, das auf die Klasse " **albummetadata** " für die Daten Anmerkungen zeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="e4b97-470">Dies sind einige der Attribute für die Daten Anmerkung, die Sie verwenden, um das Album Modell zu kommentieren:</span><span class="sxs-lookup"><span data-stu-id="e4b97-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="e4b97-471">Required: gibt an, dass die Eigenschaft ein Pflichtfeld ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="e4b97-472">Display Name: definiert den Text, der für Formularfelder und Validierungs Nachrichten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="e4b97-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="e4b97-473">DisplayFormat: gibt an, wie Datenfelder angezeigt und formatiert werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="e4b97-474">StringLength: definiert eine maximale Länge für ein Zeichen folgen Feld.</span><span class="sxs-lookup"><span data-stu-id="e4b97-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="e4b97-475">Range: gibt einen maximalen und minimalen Wert für ein numerisches Feld an.</span><span class="sxs-lookup"><span data-stu-id="e4b97-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="e4b97-476">Gerüst Column-ermöglicht das Ausblenden von Feldern aus Editor Formularen</span><span class="sxs-lookup"><span data-stu-id="e4b97-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="e4b97-477">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e4b97-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="e4b97-478">In dieser Aufgabe testen Sie mithilfe der in der letzten Aufgabe ausgewählten anzeigen Amen, ob die Felder "erstellen" und "Bearbeiten"-Felder überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="e4b97-479">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="e4b97-480">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-480">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-481">Ändern Sie die URL in **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="e4b97-482">Stellen Sie sicher, dass die anzeigen Amen denen in der partiellen Klasse entsprechen (z. b. **Album Art-URL** anstelle von **albumarturl**).</span><span class="sxs-lookup"><span data-stu-id="e4b97-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="e4b97-483">Klicken Sie auf **Erstellen**, ohne das Formular zu füllen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="e4b97-484">Vergewissern Sie sich, dass Sie die entsprechenden Validierungs Meldungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="e4b97-485">![Überprüfte Felder auf der Seite "erstellen"](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Überprüfte Felder auf der Seite "erstellen"")</span><span class="sxs-lookup"><span data-stu-id="e4b97-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="e4b97-486">*Überprüfte Felder auf der Seite "erstellen"*</span><span class="sxs-lookup"><span data-stu-id="e4b97-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="e4b97-487">Sie können überprüfen, ob das gleiche auf der Seite **Bearbeiten** auftritt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="e4b97-488">Ändern Sie die URL in **/StoreManager/Edit/1** , und überprüfen Sie, ob die anzeigen Amen denen in der partiellen Klasse entsprechen (z. b. **Album Art-URL** anstelle von **albumarturl**).</span><span class="sxs-lookup"><span data-stu-id="e4b97-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="e4b97-489">Leeren Sie die Felder **Titel** und **Preis** , und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="e4b97-490">Vergewissern Sie sich, dass Sie die entsprechenden Validierungs Meldungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-490">Verify that you get the corresponding validation messages.</span></span>

    ![Überprüfte Felder auf der Seite "Bearbeiten"](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="e4b97-492">*Überprüfte Felder auf der Seite "Bearbeiten"*</span><span class="sxs-lookup"><span data-stu-id="e4b97-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="e4b97-493">Übung 7: Verwenden von unaufdringlichen jQuery auf Client Seite</span><span class="sxs-lookup"><span data-stu-id="e4b97-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="e4b97-494">In dieser Übung erfahren Sie, wie Sie auf Clientseite die unaufdringliche jquery-Validierung von MVC 4 aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="e4b97-495">Die unaufdringliche jQuery verwendet das Data-AJAX-Präfix JavaScript, um Aktionsmethoden auf dem Server aufzurufen, anstatt Inline Client Skripts auszulösen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="e4b97-496">Aufgabe 1: Ausführen der Anwendung vor dem Aktivieren von unaufdringlichem jQuery</span><span class="sxs-lookup"><span data-stu-id="e4b97-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="e4b97-497">In dieser Aufgabe führen Sie die Anwendung aus, bevor Sie jQuery einschließen, damit beide Validierungs Modelle verglichen werden können.</span><span class="sxs-lookup"><span data-stu-id="e4b97-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="e4b97-498">Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/Ex7-unauffällig sivejqueryvalidation/BEGIN/** Folder".</span><span class="sxs-lookup"><span data-stu-id="e4b97-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="e4b97-499">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e4b97-500">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e4b97-501">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e4b97-502">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e4b97-503">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="e4b97-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e4b97-504">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e4b97-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e4b97-505">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e4b97-506">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="e4b97-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e4b97-507">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="e4b97-508">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-508">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-509">Durchsuchen Sie **/StoreManager/Create** , und klicken Sie auf **Erstellen** , ohne das Formular auszufüllen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="e4b97-510">![Client Validierung deaktiviert](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client Validierung deaktiviert")</span><span class="sxs-lookup"><span data-stu-id="e4b97-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="e4b97-511">*Client Validierung deaktiviert*</span><span class="sxs-lookup"><span data-stu-id="e4b97-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="e4b97-512">Öffnen Sie im Browser den HTML-Quellcode:</span><span class="sxs-lookup"><span data-stu-id="e4b97-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="e4b97-513">Aufgabe 2: Aktivieren der unaufdringlichen Client Validierung</span><span class="sxs-lookup"><span data-stu-id="e4b97-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="e4b97-514">In dieser Aufgabe aktivieren Sie die jQuery- **unaufdringliche Client Validierung** aus der **Web. config** -Datei, die in allen neuen ASP.NET MVC 4-Projekten standardmäßig auf false festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="e4b97-515">Außerdem fügen Sie die erforderlichen Skripts Verweise hinzu, um die unaufdringliche Client Validierung in jQuery zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="e4b97-516">Öffnen Sie die Datei **Web. config** im Projektstamm Verzeichnis, und stellen Sie sicher, dass die Schlüsselwerte **clientvalidationaktivierter** und **unauffällig sivejavascriptenabled** auf **true**festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="e4b97-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-517">Sie können die Client Validierung auch durch Code auf Global.asax.cs aktivieren, um dieselben Ergebnisse zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="e4b97-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="e4b97-518">**Htmlhelper. clientvalidationaktivierte = true;**</span><span class="sxs-lookup"><span data-stu-id="e4b97-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="e4b97-519">Außerdem können Sie einem beliebigen Controller das clientvalidationaktivierte-Attribut zuweisen, um ein benutzerdefiniertes Verhalten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="e4b97-520">Öffnen Sie **Create. cshtml** unter **views\storemanager**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="e4b97-521">Stellen Sie sicher, dass die folgenden Skriptdateien, **jQuery. Validate** und **jQuery. Validate. unaufdringlich**, über das &quot; **~/Bundles/jqueryval**&quot; Bundle in der Ansicht referenziert werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="e4b97-522">Alle diese jQuery-Bibliotheken sind in den neuen Projekten von MVC 4 enthalten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="e4b97-523">Weitere Bibliotheken finden Sie im Ordner **"/Scripts"** Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="e4b97-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="e4b97-524">Damit diese Validierungs Bibliotheken funktionieren, müssen Sie einen Verweis auf die jQuery-frameworkbibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="e4b97-525">Da dieser Verweis bereits in der **\_Layout. cshtml** -Datei hinzugefügt wurde, müssen Sie ihn nicht in dieser speziellen Ansicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="e4b97-526">Aufgabe 3: Ausführen der Anwendung mithilfe der unaufdringlichen jquery-Validierung</span><span class="sxs-lookup"><span data-stu-id="e4b97-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="e4b97-527">In dieser Aufgabe testen Sie, ob die Vorlage "Create View" von " **StoreManager** " die Client seitige Validierung mithilfe von jQuery-Bibliotheken durchführt, wenn der Benutzer ein neues Album erstellt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="e4b97-528">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e4b97-529">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="e4b97-529">The project starts in the Home page.</span></span> <span data-ttu-id="e4b97-530">Durchsuchen Sie **/StoreManager/Create** , und klicken Sie auf **Erstellen** , ohne das Formular auszufüllen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="e4b97-531">![Client Validierung mit aktiviertem jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client Validierung mit aktiviertem jQuery")</span><span class="sxs-lookup"><span data-stu-id="e4b97-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="e4b97-532">*Client Validierung mit aktiviertem jQuery*</span><span class="sxs-lookup"><span data-stu-id="e4b97-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="e4b97-533">Öffnen Sie im Browser den Quellcode für CREATE VIEW:</span><span class="sxs-lookup"><span data-stu-id="e4b97-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="e4b97-534">Für jede Client Validierungs Regel fügt unaufdringliche jQuery ein Attribut mit Data-Val-*RuleName*=&quot;*Message*&quot;hinzu.</span><span class="sxs-lookup"><span data-stu-id="e4b97-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="e4b97-535">Im folgenden finden Sie eine Liste von Tags, die unaufdringliche jQuery in das HTML-Eingabefeld eingefügt werden, um die Client Validierung auszuführen:</span><span class="sxs-lookup"><span data-stu-id="e4b97-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="e4b97-536">Data-Val</span><span class="sxs-lookup"><span data-stu-id="e4b97-536">Data-val</span></span>
   > - <span data-ttu-id="e4b97-537">Data-Val-Number</span><span class="sxs-lookup"><span data-stu-id="e4b97-537">Data-val-number</span></span>
   > - <span data-ttu-id="e4b97-538">Data-Val-Range</span><span class="sxs-lookup"><span data-stu-id="e4b97-538">Data-val-range</span></span>
   > - <span data-ttu-id="e4b97-539">Data-Val-Range-min/Data-Val-Range-Max</span><span class="sxs-lookup"><span data-stu-id="e4b97-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="e4b97-540">Data-Val-required</span><span class="sxs-lookup"><span data-stu-id="e4b97-540">Data-val-required</span></span>
   > - <span data-ttu-id="e4b97-541">Daten-Val-Länge</span><span class="sxs-lookup"><span data-stu-id="e4b97-541">Data-val-length</span></span>
   > - <span data-ttu-id="e4b97-542">Data-Val-length-Max/Data-Val-length-min</span><span class="sxs-lookup"><span data-stu-id="e4b97-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="e4b97-543">Alle Datenwerte werden mit der Modell **Daten**Anmerkung gefüllt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="e4b97-544">Anschließend kann die gesamte Logik, die auf der Serverseite funktioniert, auf der Clientseite ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e4b97-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="e4b97-545">Beispielsweise hat Price Attribute die folgende Daten Anmerkung im Modell:</span><span class="sxs-lookup"><span data-stu-id="e4b97-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="e4b97-546">Nachdem Sie unaufdringliche jQuery verwendet haben, lautet der generierte Code wie folgt:</span><span class="sxs-lookup"><span data-stu-id="e4b97-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e4b97-547">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="e4b97-547">Summary</span></span>

<span data-ttu-id="e4b97-548">Wenn Sie diese praktische Übungseinheit durcharbeiten, haben Sie gelernt, wie Sie es Benutzern ermöglichen können, die in der Datenbank gespeicherten Daten mit folgendem zu ändern:</span><span class="sxs-lookup"><span data-stu-id="e4b97-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="e4b97-549">Controller Aktionen wie Index, CREATE, Edit, DELETE</span><span class="sxs-lookup"><span data-stu-id="e4b97-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="e4b97-550">ASP.NET MVC-Gerüstbau Funktion zum Anzeigen von Eigenschaften in einer HTML-Tabelle</span><span class="sxs-lookup"><span data-stu-id="e4b97-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="e4b97-551">Benutzerdefinierte HTML-Hilfsprogramme zur Verbesserung der Benutzer Leistung</span><span class="sxs-lookup"><span data-stu-id="e4b97-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="e4b97-552">Aktionsmethoden, die entweder auf HTTP-GET oder http-Post-Aufrufe reagieren</span><span class="sxs-lookup"><span data-stu-id="e4b97-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="e4b97-553">Eine freigegebene Editor Vorlage für ähnliche Ansichts Vorlagen wie CREATE und Edit</span><span class="sxs-lookup"><span data-stu-id="e4b97-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="e4b97-554">Formularelemente wie Dropdown-Elemente</span><span class="sxs-lookup"><span data-stu-id="e4b97-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="e4b97-555">Daten Anmerkungen für die Modell Validierung</span><span class="sxs-lookup"><span data-stu-id="e4b97-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="e4b97-556">Client seitige Validierung mit der jQuery-unaufdringlichen Bibliothek</span><span class="sxs-lookup"><span data-stu-id="e4b97-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e4b97-557">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="e4b97-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e4b97-558">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e4b97-559">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="e4b97-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e4b97-560">Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e4b97-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e4b97-561">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e4b97-562">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="e4b97-562">Click on **Install Now**.</span></span> <span data-ttu-id="e4b97-563">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e4b97-564">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e4b97-565">![Installieren von Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e4b97-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e4b97-566">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e4b97-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e4b97-567">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="e4b97-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="e4b97-569">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="e4b97-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e4b97-570">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="e4b97-570">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="e4b97-572">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="e4b97-572">*Installation progress*</span></span>
6. <span data-ttu-id="e4b97-573">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-573">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="e4b97-575">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="e4b97-575">*Installation completed*</span></span>
7. <span data-ttu-id="e4b97-576">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="e4b97-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e4b97-577">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="e4b97-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="e4b97-579">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="e4b97-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="e4b97-580">Anhang B: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="e4b97-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="e4b97-581">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e4b97-582">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e4b97-583">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="e4b97-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e4b97-584">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="e4b97-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e4b97-585">***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***</span><span class="sxs-lookup"><span data-stu-id="e4b97-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e4b97-586">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e4b97-587">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="e4b97-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e4b97-588">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="e4b97-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e4b97-589">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="e4b97-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e4b97-590">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="e4b97-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e4b97-591">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="e4b97-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e4b97-592">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="e4b97-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="e4b97-593">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="e4b97-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e4b97-594">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="e4b97-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e4b97-595">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="e4b97-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e4b97-596">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="e4b97-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e4b97-597">So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e4b97-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e4b97-598">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="e4b97-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e4b97-599">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="e4b97-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e4b97-600">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="e4b97-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e4b97-601">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="e4b97-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e4b97-602">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="e4b97-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e4b97-603">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="e4b97-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e4b97-604">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="e4b97-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
