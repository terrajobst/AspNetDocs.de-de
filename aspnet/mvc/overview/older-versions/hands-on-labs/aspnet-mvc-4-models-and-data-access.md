---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4-Modelle und Datenzugriff | Microsoft-Dokumentation
author: rick-anderson
description: 'Hinweis: in dieser praktischen Übungseinheit wird davon ausgegangen, dass Sie über grundlegende Kenntnisse von ASP.NET MVC verfügen. Wenn Sie ASP.NET MVC noch nicht verwendet haben, empfehlen wir, dass Sie ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451467"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="25db0-104">ASP.NET MVC 4 – Modelle und Datenzugriff</span><span class="sxs-lookup"><span data-stu-id="25db0-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="25db0-105">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="25db0-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="25db0-106">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="25db0-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="25db0-107">Diese praktische Übung geht davon aus, dass Sie über grundlegende Kenntnisse von **ASP.NET MVC**verfügen.</span><span class="sxs-lookup"><span data-stu-id="25db0-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="25db0-108">Wenn Sie **ASP.NET MVC** noch nicht verwendet haben, empfehlen wir Ihnen, **ASP.NET MVC 4-Grundlagen** praktische Übungseinheiten zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="25db0-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="25db0-109">Diese Übungseinheit führt Sie durch die Verbesserungen und neuen Features, die Sie zuvor beschrieben haben, indem Sie kleinere Änderungen an einer im Quellordner bereitgestellten beispielweb Anwendung anwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-110">Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="25db0-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="25db0-111">Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Modelle und Datenzugriff](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="25db0-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="25db0-112">In der praktischen Übungseinheit **ASP.NET MVC-Grundlagen** haben Sie hart codierte Daten von den Controllern an die Ansichts Vorlagen übergeben.</span><span class="sxs-lookup"><span data-stu-id="25db0-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="25db0-113">Um jedoch eine echte Webanwendung zu erstellen, empfiehlt es sich, eine echte Datenbank zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="25db0-114">Diese praktische Übungseinheit zeigt Ihnen, wie Sie eine Datenbank-Engine zum Speichern und Abrufen der Daten verwenden, die für die Music Store-Anwendung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="25db0-115">Um dies zu erreichen, beginnen Sie mit einer vorhandenen Datenbank, und erstellen Sie die Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="25db0-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="25db0-116">In dieser Übungseinheit treffen Sie den **Database First** Ansatz und den Ansatz der **Code First** .</span><span class="sxs-lookup"><span data-stu-id="25db0-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="25db0-117">Sie können jedoch auch den **Model First** Ansatz verwenden, das gleiche Modell mit den Tools erstellen und anschließend die Datenbank daraus generieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="25db0-118">![Database First im Vergleich zu Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First im Vergleich zu Model First")</span><span class="sxs-lookup"><span data-stu-id="25db0-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="25db0-119">*Database First im Vergleich zu Model First*</span><span class="sxs-lookup"><span data-stu-id="25db0-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="25db0-120">Nachdem Sie das Modell erstellt haben, nehmen Sie die richtigen Anpassungen in StoreController vor, um die Speicher Sichten mit den Daten aus der Datenbank bereitzustellen, anstatt hart codierte Daten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="25db0-121">Sie müssen keine Änderungen an den Ansichts Vorlagen vornehmen, da der StoreController die gleichen ViewModels an die Ansichts Vorlagen zurückgibt, obwohl die Daten dieses Mal aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="25db0-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="25db0-122">**Der Code First Ansatz**</span><span class="sxs-lookup"><span data-stu-id="25db0-122">**The Code First Approach**</span></span>

<span data-ttu-id="25db0-123">Mithilfe des Code First Ansatzes können wir das Modell aus dem Code definieren, ohne Klassen zu erstellen, die im Allgemeinen mit dem Framework gekoppelt sind.</span><span class="sxs-lookup"><span data-stu-id="25db0-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="25db0-124">In Code First werden Modell Objekte mit POCOS definiert, &quot;Plain Old CLR Objects&quot;.</span><span class="sxs-lookup"><span data-stu-id="25db0-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="25db0-125">POCOS sind einfache einfache Klassen, die keine Vererbung aufweisen und keine Schnittstellen implementieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="25db0-126">Wir können die Datenbank automatisch aus Ihnen generieren, oder Sie können eine vorhandene Datenbank verwenden und die Klassen Zuordnung aus dem Code generieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="25db0-127">Der Vorteil dieser Vorgehensweise besteht darin, dass das Modell unabhängig vom Persistenzframework (in diesem Fall Entity Framework) ist, da die POCOS-Klassen nicht mit dem Mapping-Framework gekoppelt sind.</span><span class="sxs-lookup"><span data-stu-id="25db0-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-128">Diese Übungseinheit basiert auf ASP.NET MVC 4 und einer Version der Music Store-Beispielanwendung, die so angepasst und minimiert ist, dass Sie nur den in dieser praktischen Übungseinheit gezeigten Features entspricht.</span><span class="sxs-lookup"><span data-stu-id="25db0-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="25db0-129">Wenn Sie die gesamte **Music Store** -Lernprogramm Anwendung erkunden möchten, finden Sie Sie in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="25db0-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="25db0-130">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="25db0-130">Prerequisites</span></span>

<span data-ttu-id="25db0-131">Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:</span><span class="sxs-lookup"><span data-stu-id="25db0-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="25db0-132">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang A](#AppendixA) ).</span><span class="sxs-lookup"><span data-stu-id="25db0-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="25db0-133">Einrichten</span><span class="sxs-lookup"><span data-stu-id="25db0-133">Setup</span></span>

<span data-ttu-id="25db0-134">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="25db0-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="25db0-135">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="25db0-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="25db0-136">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="25db0-137">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.</span><span class="sxs-lookup"><span data-stu-id="25db0-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="25db0-138">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="25db0-138">Exercises</span></span>

<span data-ttu-id="25db0-139">Diese praktische Übungseinheit besteht aus den folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="25db0-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="25db0-140">Übung 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="25db0-141">Übung 2: Erstellen einer Datenbank mit Code First</span><span class="sxs-lookup"><span data-stu-id="25db0-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="25db0-142">Übung 3: Abfragen der Datenbank mit Parametern</span><span class="sxs-lookup"><span data-stu-id="25db0-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="25db0-143">Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="25db0-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="25db0-144">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="25db0-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="25db0-145">Geschätzte Zeit bis zum Abschluss dieses Labs: **35 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="25db0-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="25db0-146">Übung 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="25db0-147">In dieser Übung erfahren Sie, wie Sie eine Datenbank mit den Tabellen der Anwendung "Musicstore" zur Projekt Mappe hinzufügen, um Ihre Daten zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="25db0-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="25db0-148">Nachdem die Datenbank mit dem Modell generiert und der Projekt Mappe hinzugefügt wurde, ändern Sie die StoreController-Klasse, um die Ansichts Vorlage mit den Daten aus der Datenbank bereitzustellen, anstatt hart codierte Werte zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="25db0-149">Aufgabe 1: Hinzufügen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="25db0-150">In dieser Aufgabe fügen Sie der Projekt Mappe eine bereits erstellte Datenbank mit den Haupt Tabellen der "Musicstore"-Anwendung hinzu.</span><span class="sxs-lookup"><span data-stu-id="25db0-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="25db0-151">Öffnen Sie die Projekt Mappe " **Anfang** " unter **Quell/EX1-addingadatabasedbfirst/BEGIN/** Folder.</span><span class="sxs-lookup"><span data-stu-id="25db0-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="25db0-152">Sie müssen einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="25db0-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="25db0-153">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="25db0-154">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="25db0-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="25db0-155">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="25db0-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="25db0-156">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="25db0-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="25db0-157">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="25db0-158">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="25db0-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="25db0-159">Fügen Sie die **mvcmusicstore** -Datenbankdatei hinzu.</span><span class="sxs-lookup"><span data-stu-id="25db0-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="25db0-160">In dieser praktischen Übungseinheit verwenden Sie eine bereits erstellte Datenbank mit dem Namen " **mvcmusicstore. mdf**".</span><span class="sxs-lookup"><span data-stu-id="25db0-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="25db0-161">Klicken Sie hierzu mit der rechten Maustaste auf **App\_Daten** Ordner, zeigen Sie auf **Hinzufügen** , und klicken Sie dann auf **Vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="25db0-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="25db0-162">Navigieren Sie zu **\source\assets** , und wählen Sie die Datei " **mvcmusicstore. mdf** " aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="25db0-163">![Hinzufügen eines vorhandenen Elements](aspnet-mvc-4-models-and-data-access/_static/image2.png "Hinzufügen eines vorhandenen Elements")</span><span class="sxs-lookup"><span data-stu-id="25db0-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="25db0-164">*Hinzufügen eines vorhandenen Elements*</span><span class="sxs-lookup"><span data-stu-id="25db0-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="25db0-165">![Mvcmusicstore. MDF-Datenbankdatei](aspnet-mvc-4-models-and-data-access/_static/image3.png "Mvcmusicstore. MDF-Datenbankdatei")</span><span class="sxs-lookup"><span data-stu-id="25db0-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="25db0-166">*Mvcmusicstore. MDF-Datenbankdatei*</span><span class="sxs-lookup"><span data-stu-id="25db0-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="25db0-167">Die Datenbank wurde dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="25db0-167">The database has been added to the project.</span></span> <span data-ttu-id="25db0-168">Auch wenn sich die Datenbank in der Lösung befindet, können Sie Sie Abfragen und aktualisieren, da Sie auf einem anderen Datenbankserver gehostet wurde.</span><span class="sxs-lookup"><span data-stu-id="25db0-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="25db0-169">![Mvcmusicstore-Datenbank in Projektmappen-Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "Mvcmusicstore-Datenbank in Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="25db0-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="25db0-170">*Mvcmusicstore-Datenbank in Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="25db0-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="25db0-171">Überprüfen Sie die Verbindung mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="25db0-171">Verify the connection to the database.</span></span> <span data-ttu-id="25db0-172">Doppelklicken Sie hierzu auf " **mvcmusicstore. mdf** ", um eine Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="25db0-173">![Verbindung mit "mvcmusicstore. mdf" wird hergestellt](aspnet-mvc-4-models-and-data-access/_static/image5.png "Verbindung mit "mvcmusicstore. mdf" wird hergestellt")</span><span class="sxs-lookup"><span data-stu-id="25db0-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="25db0-174">*Verbindung mit "mvcmusicstore. mdf" wird hergestellt*</span><span class="sxs-lookup"><span data-stu-id="25db0-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="25db0-175">Aufgabe 2: Erstellen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="25db0-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="25db0-176">In dieser Aufgabe erstellen Sie ein Datenmodell, um mit der Datenbank zu interagieren, die in der vorherigen Aufgabe hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="25db0-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="25db0-177">Erstellen Sie ein Datenmodell, das die Datenbank darstellt.</span><span class="sxs-lookup"><span data-stu-id="25db0-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="25db0-178">Klicken Sie hierzu in Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , zeigen Sie auf **Hinzufügen** , und klicken Sie dann auf **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="25db0-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="25db0-179">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model** Element.</span><span class="sxs-lookup"><span data-stu-id="25db0-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="25db0-180">Ändern Sie den Namen des Datenmodells in **storedb. edmx** , und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="25db0-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="25db0-181">![Hinzufügen der storedb ADO.NET-Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Hinzufügen der storedb ADO.NET-Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="25db0-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="25db0-182">*Hinzufügen der storedb ADO.NET-Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="25db0-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="25db0-183">Der **Entity Data Model-Assistent** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25db0-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="25db0-184">Dieser Assistent führt Sie durch die Erstellung der Modell Ebene.</span><span class="sxs-lookup"><span data-stu-id="25db0-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="25db0-185">Da das Modell basierend auf der zuvor hinzugefügten Datenbank erstellt werden sollte, wählen Sie **aus Datenbank generieren aus** , und klicken Sie auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="25db0-185">Since the model should be created based on the existing database recently added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="25db0-186">![Auswählen des Modell Inhalts](aspnet-mvc-4-models-and-data-access/_static/image7.png "Auswählen des Modell Inhalts")</span><span class="sxs-lookup"><span data-stu-id="25db0-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="25db0-187">*Auswählen des Modell Inhalts*</span><span class="sxs-lookup"><span data-stu-id="25db0-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="25db0-188">Da Sie ein Modell aus einer Datenbank erstellen, müssen Sie die zu verwendende Verbindung angeben.</span><span class="sxs-lookup"><span data-stu-id="25db0-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="25db0-189">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="25db0-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="25db0-190">Wählen Sie **Microsoft SQL Server Datenbankdatei** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="25db0-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="25db0-191">![Datenquelle auswählen](aspnet-mvc-4-models-and-data-access/_static/image8.png "Datenquelle auswählen")</span><span class="sxs-lookup"><span data-stu-id="25db0-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="25db0-192">*Dialogfeld "Datenquelle auswählen"*</span><span class="sxs-lookup"><span data-stu-id="25db0-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="25db0-193">Klicken Sie auf **Durchsuchen** , und wählen Sie die Datenbank **mvcmusicstore. mdf** aus, die Sie im Ordner **App\_Data** finden, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="25db0-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="25db0-194">![Verbindungs Eigenschaften](aspnet-mvc-4-models-and-data-access/_static/image9.png "Verbindungseigenschaften")</span><span class="sxs-lookup"><span data-stu-id="25db0-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="25db0-195">*Verbindungs Eigenschaften*</span><span class="sxs-lookup"><span data-stu-id="25db0-195">*Connection properties*</span></span>
6. <span data-ttu-id="25db0-196">Die generierte Klasse muss den gleichen Namen wie die Entitäts Verbindungs Zeichenfolge haben. ändern Sie den Namen in " **musicstoreentities** ", und klicken Sie auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="25db0-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="25db0-197">![Auswählen der Datenverbindung](aspnet-mvc-4-models-and-data-access/_static/image10.png "Auswählen der Datenverbindung")</span><span class="sxs-lookup"><span data-stu-id="25db0-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="25db0-198">*Auswählen der Datenverbindung*</span><span class="sxs-lookup"><span data-stu-id="25db0-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="25db0-199">Wählen Sie die zu verwendenden Datenbankobjekte aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-199">Choose the database objects to use.</span></span> <span data-ttu-id="25db0-200">Wenn das Entitäts Modell nur die Tabellen der Datenbank verwendet, wählen Sie die Option **Tabellen** aus, und vergewissern Sie sich, dass die Optionen für die Option " **Fremdschlüssel Spalten in das Modell einbeziehen** " und "pluralisieren" bzw. "Singular" für **generierte Objektnamen** ebenfalls ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="25db0-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="25db0-201">Ändern Sie den Modell Namespace in **mvcmusicstore. Model** , und klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="25db0-202">![Auswählen der Datenbankobjekte](aspnet-mvc-4-models-and-data-access/_static/image11.png "Auswählen der Datenbankobjekte")</span><span class="sxs-lookup"><span data-stu-id="25db0-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="25db0-203">*Auswählen der Datenbankobjekte*</span><span class="sxs-lookup"><span data-stu-id="25db0-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="25db0-204">Wenn ein Dialogfeld mit einer Sicherheitswarnung angezeigt wird, klicken Sie auf **OK** , um die Vorlage auszuführen und die Klassen für die Modell Entitäten zu generieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="25db0-205">Ein Entitäts Diagramm für die Datenbank wird angezeigt, während eine separate Klasse erstellt wird, die jede Tabelle der Datenbank zuordnet.</span><span class="sxs-lookup"><span data-stu-id="25db0-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="25db0-206">Beispielsweise wird die Tabelle **Alben** durch eine **Album** -Klasse dargestellt, wobei jede Spalte in der Tabelle einer Klassen Eigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="25db0-207">Dies ermöglicht es Ihnen, Objekte abzufragen und zu bearbeiten, die Zeilen in der Datenbank darstellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="25db0-208">![Entitäts Diagramm](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entitätsdiagramm")</span><span class="sxs-lookup"><span data-stu-id="25db0-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="25db0-209">*Entitäts Diagramm*</span><span class="sxs-lookup"><span data-stu-id="25db0-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="25db0-210">Die T4-Vorlagen (. tt) führen Code aus, um die Entitäts Klassen zu generieren, und überschreiben die vorhandenen Klassen mit demselben Namen.</span><span class="sxs-lookup"><span data-stu-id="25db0-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="25db0-211">In diesem Beispiel wurden die Klassen &quot;Album&quot;, &quot;Genre&quot; und &quot;Künstlerin&quot; mit dem generierten Code überschrieben.</span><span class="sxs-lookup"><span data-stu-id="25db0-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="25db0-212">Aufgabe 3: entwickeln der Anwendung</span><span class="sxs-lookup"><span data-stu-id="25db0-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="25db0-213">In dieser Aufgabe überprüfen Sie, dass das Projekt erfolgreich mithilfe der neuen Datenmodell Klassen erstellt wurde, obwohl die Modell Generierung die Modelle " **Album**", " **Genre** " und " **Artist** " entfernt hat.</span><span class="sxs-lookup"><span data-stu-id="25db0-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="25db0-214">Erstellen Sie das Projekt, indem Sie das Menü Element **Build** auswählen und dann **mvcmusicstore erstellen**.</span><span class="sxs-lookup"><span data-stu-id="25db0-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="25db0-215">![Projekt wird aufgebaut](aspnet-mvc-4-models-and-data-access/_static/image13.png "Projekt wird aufgebaut")</span><span class="sxs-lookup"><span data-stu-id="25db0-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="25db0-216">*Projekt wird aufgebaut*</span><span class="sxs-lookup"><span data-stu-id="25db0-216">*Building the project*</span></span>
2. <span data-ttu-id="25db0-217">Das Projekt wurde erfolgreich erstellt.</span><span class="sxs-lookup"><span data-stu-id="25db0-217">The project builds successfully.</span></span> <span data-ttu-id="25db0-218">Warum funktioniert es noch?</span><span class="sxs-lookup"><span data-stu-id="25db0-218">Why does it still work?</span></span> <span data-ttu-id="25db0-219">Dies funktioniert, da die Datenbanktabellen Felder enthalten, die die Eigenschaften enthalten, die Sie im **Album** "entfernte Klassen" und im **Genre**verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="25db0-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="25db0-220">![Builds erfolgreich](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds erfolgreich")</span><span class="sxs-lookup"><span data-stu-id="25db0-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="25db0-221">*Builds erfolgreich*</span><span class="sxs-lookup"><span data-stu-id="25db0-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="25db0-222">Während der Designer die Entitäten in einem Diagramm Format anzeigt, handelt es C# sich um Klassen.</span><span class="sxs-lookup"><span data-stu-id="25db0-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="25db0-223">Erweitern Sie den Knoten **storedb. edmx** im Projektmappen-Explorer und dann **StoreDB.tt**, werden die neuen generierten Entitäten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25db0-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="25db0-224">![Generierte Dateien](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generierte Dateien")</span><span class="sxs-lookup"><span data-stu-id="25db0-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="25db0-225">*Generierte Dateien*</span><span class="sxs-lookup"><span data-stu-id="25db0-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="25db0-226">Aufgabe 4: Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="25db0-227">In dieser Aufgabe aktualisieren Sie die StoreController-Klasse, sodass anstelle von hart codierten Daten die Datenbank abgefragt wird, um die Informationen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="25db0-228">Öffnen Sie " **controllers\storecontroller.cs** ", und fügen Sie der Klasse das folgende Feld hinzu, um eine Instanz der Klasse " **musicstoreentities** " mit dem Namen " **storedb**" zu speichern:</span><span class="sxs-lookup"><span data-stu-id="25db0-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="25db0-229">(Code Ausschnitt- *Modelle und Datenzugriff-EX1 storedb*)</span><span class="sxs-lookup"><span data-stu-id="25db0-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="25db0-230">Die Klasse " **musicstoreentities** " macht eine Auflistungs Eigenschaft für jede Tabelle in der Datenbank verfügbar.</span><span class="sxs-lookup"><span data-stu-id="25db0-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="25db0-231">Aktualisieren Sie die Methode zum **Durchsuchen** von Aktionen, um ein Genre mit allen **Alben**abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="25db0-232">(Code Ausschnitt- *Modelle und Datenzugriff-EX1 Store durchsuchen*)</span><span class="sxs-lookup"><span data-stu-id="25db0-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="25db0-233">Sie verwenden eine Funktion von .net mit dem Namen **LINQ** (Language-Integrated Query), um stark typisierte Abfrage Ausdrücke für diese Auflistungen zu schreiben, die Code für die Datenbank ausführen und Objekte zurückgeben, für die Sie programmieren können.</span><span class="sxs-lookup"><span data-stu-id="25db0-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="25db0-234">Weitere Informationen zu LINQ finden Sie auf der [MSDN-Website](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="25db0-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="25db0-235">Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="25db0-236">(Code Ausschnitt- *Modelle und Datenzugriff-EX1-Speicher Index*)</span><span class="sxs-lookup"><span data-stu-id="25db0-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="25db0-237">Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen und die Auflistung in eine Liste umzuwandeln.</span><span class="sxs-lookup"><span data-stu-id="25db0-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="25db0-238">(Code Ausschnitt- *Modelle und Datenzugriff-EX1 Store genremenu*)</span><span class="sxs-lookup"><span data-stu-id="25db0-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="25db0-239">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="25db0-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="25db0-240">In dieser Aufgabe überprüfen Sie, ob auf der Seite Speicher Index nun die in der Datenbank gespeicherten Genres anstelle der hart codierten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="25db0-241">Es ist nicht erforderlich, die Ansichts Vorlage zu ändern, da **StoreController** dieselben Entitäten wie zuvor zurückgibt, obwohl die Daten dieses Zeitraums aus der Datenbank stammen.</span><span class="sxs-lookup"><span data-stu-id="25db0-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="25db0-242">Erstellen Sie die Lösung neu, und drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="25db0-243">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="25db0-243">The project starts in the Home page.</span></span> <span data-ttu-id="25db0-244">Vergewissern Sie sich, dass das Menü der **Genres** keine hart codierte Liste mehr ist und die Daten direkt aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="25db0-246">*Durchsuchen von Genres aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="25db0-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="25db0-247">Navigieren Sie nun zu einem beliebigen Genre, und überprüfen Sie, ob die Alben aus der Datenbank aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="25db0-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="25db0-248">![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image17.png "Durchsuchen von Alben aus der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="25db0-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="25db0-249">*Durchsuchen von Alben aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="25db0-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="25db0-250">Übung 2: Erstellen einer Datenbank mit Code First</span><span class="sxs-lookup"><span data-stu-id="25db0-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="25db0-251">In dieser Übung erfahren Sie, wie Sie den Code First Ansatz verwenden, um eine Datenbank mit den Tabellen der "Musicstore"-Anwendung zu erstellen und um auf die Daten zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="25db0-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="25db0-252">Nachdem das Modell generiert wurde, ändern Sie StoreController, um die Ansichts Vorlage mit den Daten aus der Datenbank bereitzustellen, anstatt hart codierte Werte zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-253">Wenn Sie Übung 1 abgeschlossen haben und bereits mit dem Database First Ansatz gearbeitet haben, erfahren Sie nun, wie Sie die gleichen Ergebnisse mit einem anderen Prozess erhalten.</span><span class="sxs-lookup"><span data-stu-id="25db0-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="25db0-254">Die Aufgaben, die in Übung 1 häufig ausgeführt werden, wurden gekennzeichnet, um das Lesen zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="25db0-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="25db0-255">Wenn Sie Übung 1 nicht abgeschlossen haben, aber den Code First Ansatz erlernen möchten, können Sie mit dieser Übung beginnen und eine vollständige Abdeckung des Themas erhalten.</span><span class="sxs-lookup"><span data-stu-id="25db0-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="25db0-256">Aufgabe 1: Auffüllen von Beispiel Daten</span><span class="sxs-lookup"><span data-stu-id="25db0-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="25db0-257">In dieser Aufgabe füllen Sie die Datenbank mit Beispiel Daten auf, wenn Sie anfänglich mit Code First erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-257">In this task, you will populate the database with sample data when it is initially created using Code-First.</span></span>

1. <span data-ttu-id="25db0-258">Öffnen Sie die Projekt Mappe " **Anfang** " unter " **Source/EX2-"-kreatingadatabasecoentfirst/BEGIN/** Folder ".</span><span class="sxs-lookup"><span data-stu-id="25db0-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="25db0-259">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="25db0-260">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="25db0-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="25db0-261">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="25db0-262">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="25db0-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="25db0-263">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="25db0-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="25db0-264">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="25db0-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="25db0-265">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="25db0-266">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="25db0-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="25db0-267">Fügen Sie die Datei **SampleData.cs** dem Ordner **Models** hinzu.</span><span class="sxs-lookup"><span data-stu-id="25db0-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="25db0-268">Klicken Sie hierzu mit der rechten Maustaste auf den Ordner **Modelle** , zeigen Sie auf **Hinzufügen** , und klicken Sie dann auf **Vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="25db0-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="25db0-269">Navigieren Sie zu **\source\assets** , und wählen Sie die Datei **SampleData.cs** aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="25db0-270">![Beispiel Daten Auffüllen von Code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Beispiel Daten Auffüllen von Code")</span><span class="sxs-lookup"><span data-stu-id="25db0-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="25db0-271">*Beispiel Daten Auffüllen von Code*</span><span class="sxs-lookup"><span data-stu-id="25db0-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="25db0-272">Öffnen Sie die Datei **Global.asax.cs** , und fügen Sie die folgenden *using* -Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="25db0-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="25db0-273">(Code Ausschnitt- *Modelle und Datenzugriff EX2 globale asax*-using-Direktiven)</span><span class="sxs-lookup"><span data-stu-id="25db0-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="25db0-274">Fügen Sie in der **Anwendung\_Start ()** -Methode die folgende Zeile hinzu, um den datenbankinitialisierer festzulegen.</span><span class="sxs-lookup"><span data-stu-id="25db0-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="25db0-275">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Global asax setinitializer*)</span><span class="sxs-lookup"><span data-stu-id="25db0-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="25db0-276">Aufgabe 2: Konfigurieren der Verbindung mit der Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="25db0-277">Nachdem Sie dem Projekt bereits eine Datenbank hinzugefügt haben, schreiben Sie die Verbindungs Zeichenfolge in der Datei " **Web. config** ".</span><span class="sxs-lookup"><span data-stu-id="25db0-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="25db0-278">Fügen Sie eine Verbindungs Zeichenfolge in **Web. config**hinzu. Öffnen Sie hierzu " **Web. config** " unter "Projektstamm", und ersetzen Sie die Verbindungs Zeichenfolge "DefaultConnection" im Abschnitt " **&lt;connectionStrings&gt;** " durch diese Zeile:</span><span class="sxs-lookup"><span data-stu-id="25db0-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="25db0-279">![Speicherort der Datei Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Speicherort der Datei Web. config")</span><span class="sxs-lookup"><span data-stu-id="25db0-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="25db0-280">*Speicherort der Datei Web. config*</span><span class="sxs-lookup"><span data-stu-id="25db0-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="25db0-281">Aufgabe 3: Arbeiten mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="25db0-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="25db0-282">Nachdem Sie die Verbindung mit der Datenbank bereits konfiguriert haben, verknüpfen Sie das Modell mit den Datenbanktabellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="25db0-283">In dieser Aufgabe erstellen Sie eine-Klasse, die mit Code First mit der-Datenbank verknüpft wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="25db0-284">Beachten Sie, dass eine vorhandene poco-Modell Klasse vorhanden ist, die geändert werden sollte.</span><span class="sxs-lookup"><span data-stu-id="25db0-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-285">Wenn Sie Übung 1 abgeschlossen haben, werden Sie feststellen, dass dieser Schritt von einem Assistenten durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="25db0-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="25db0-286">Wenn Sie Code First, erstellen Sie manuell Klassen, die mit Daten Entitäten verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="25db0-287">Öffnen Sie das poco-Modellklassen **Genre** aus dem Projektordner **Models** , und fügen Sie eine ID ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="25db0-288">Verwenden Sie eine int-Eigenschaft mit dem Namen **GenreID**.</span><span class="sxs-lookup"><span data-stu-id="25db0-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="25db0-289">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First Genre*)</span><span class="sxs-lookup"><span data-stu-id="25db0-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="25db0-290">Zum Arbeiten mit Code First Konventionen muss das Klassen Genre über eine Primärschlüssel Eigenschaft verfügen, die automatisch erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="25db0-291">Weitere Informationen zu Code First Konventionen finden Sie in diesem [MSDN-Artikel](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="25db0-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="25db0-292">Öffnen Sie nun das poco-Modellklassen- **Album** aus dem Projektordner **Models** , und schließen Sie die Fremdschlüssel ein, und erstellen Sie Eigenschaften mit den Namen **GenreID** und **artistId**.</span><span class="sxs-lookup"><span data-stu-id="25db0-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="25db0-293">Diese Klasse verfügt bereits über den **GenreID** für den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="25db0-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="25db0-294">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First Album*)</span><span class="sxs-lookup"><span data-stu-id="25db0-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="25db0-295">Öffnen Sie die poco- **Modell Klasse,** und schließen Sie die **artistId-** Eigenschaft ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="25db0-296">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First Künstlerin*)</span><span class="sxs-lookup"><span data-stu-id="25db0-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="25db0-297">Klicken Sie mit der rechten Maustaste auf den Projektordner **Modelle** , **und wählen Sie -Klasse**.</span><span class="sxs-lookup"><span data-stu-id="25db0-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="25db0-298">Nennen Sie die Datei **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="25db0-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="25db0-299">Klicken Sie dann auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="25db0-299">Then, click **Add.**</span></span>

    <span data-ttu-id="25db0-300">![Hinzufügen einer Klasse](aspnet-mvc-4-models-and-data-access/_static/image20.png "Hinzufügen einer Klasse")</span><span class="sxs-lookup"><span data-stu-id="25db0-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="25db0-301">*Hinzufügen eines neuen Elements*</span><span class="sxs-lookup"><span data-stu-id="25db0-301">*Adding a new item*</span></span>

    <span data-ttu-id="25db0-302">![Hinzufügen eines Klasse2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Hinzufügen eines Klasse2")</span><span class="sxs-lookup"><span data-stu-id="25db0-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="25db0-303">*Hinzufügen einer Klasse*</span><span class="sxs-lookup"><span data-stu-id="25db0-303">*Adding a class*</span></span>
5. <span data-ttu-id="25db0-304">Öffnen Sie die soeben erstellte Klasse **MusicStoreEntities.cs**, und fügen Sie die Namespaces **System. Data. Entity** und **System. Data. Entity. Infrastructure**ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="25db0-305">Ersetzen Sie die Klassen Deklaration, um die **dbcontext** -Klasse zu erweitern: Deklarieren Sie ein öffentliches **dbset** , und überschreiben Sie die **onmodelcreating**</span><span class="sxs-lookup"><span data-stu-id="25db0-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="25db0-306">Nach diesem Schritt erhalten Sie eine Domänen Klasse, mit der das Modell mit dem Entity Framework verknüpft wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="25db0-307">Ersetzen Sie dazu den Klassen Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="25db0-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="25db0-308">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Code First musicstoreentities*)</span><span class="sxs-lookup"><span data-stu-id="25db0-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="25db0-309">Mit Entity Framework **dbcontext** und **dbset** können Sie das poco-Klassen Genre Abfragen.</span><span class="sxs-lookup"><span data-stu-id="25db0-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="25db0-310">Wenn Sie die **onmodelcreating** -Methode erweitern, geben Sie im **Code** an, wie das Genre einer Datenbanktabelle zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="25db0-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="25db0-311">Weitere Informationen zu dbcontext und dbset finden Sie in diesem MSDN-Artikel: [Link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="25db0-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="25db0-312">Aufgabe 4: Abfragen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="25db0-313">In dieser Aufgabe aktualisieren Sie die StoreController-Klasse so, dass Sie anstelle von hart codierten Daten aus der Datenbank abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-314">Diese Aufgabe wird in Übung 1 häufig durchführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="25db0-315">Wenn Sie Übung 1 abgeschlossen haben, werden Sie feststellen, dass diese Schritte in beiden Ansätzen identisch sind (Database First oder Code First).</span><span class="sxs-lookup"><span data-stu-id="25db0-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="25db0-316">Sie unterscheiden sich in der Art und Weise, wie die Daten mit dem Modell verknüpft sind, aber der Zugriff auf Daten Entitäten ist vom Controller aus noch transparent.</span><span class="sxs-lookup"><span data-stu-id="25db0-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>

1. <span data-ttu-id="25db0-317">Öffnen Sie " **controllers\storecontroller.cs** ", und fügen Sie der Klasse das folgende Feld hinzu, um eine Instanz der Klasse " **musicstoreentities** " mit dem Namen " **storedb**" zu speichern:</span><span class="sxs-lookup"><span data-stu-id="25db0-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="25db0-318">(Code Ausschnitt- *Modelle und Datenzugriff-EX1 storedb*)</span><span class="sxs-lookup"><span data-stu-id="25db0-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="25db0-319">Die Klasse " **musicstoreentities** " macht eine Auflistungs Eigenschaft für jede Tabelle in der Datenbank verfügbar.</span><span class="sxs-lookup"><span data-stu-id="25db0-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="25db0-320">Aktualisieren Sie die Methode zum **Durchsuchen** von Aktionen, um ein Genre mit allen **Alben**abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="25db0-321">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Store durchsuchen*)</span><span class="sxs-lookup"><span data-stu-id="25db0-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="25db0-322">Sie verwenden eine Funktion von .net mit dem Namen **LINQ** (Language-Integrated Query), um stark typisierte Abfrage Ausdrücke für diese Auflistungen zu schreiben, die Code für die Datenbank ausführen und Objekte zurückgeben, für die Sie programmieren können.</span><span class="sxs-lookup"><span data-stu-id="25db0-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="25db0-323">Weitere Informationen zu LINQ finden Sie auf der [MSDN-Website](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="25db0-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="25db0-324">Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="25db0-325">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Store-Index*)</span><span class="sxs-lookup"><span data-stu-id="25db0-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="25db0-326">Aktualisieren Sie die **Index** Aktionsmethode, um alle Genres abzurufen und die Auflistung in eine Liste umzuwandeln.</span><span class="sxs-lookup"><span data-stu-id="25db0-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="25db0-327">(Code Ausschnitt- *Modelle und Datenzugriff-EX2 Store genremenu*)</span><span class="sxs-lookup"><span data-stu-id="25db0-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="25db0-328">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="25db0-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="25db0-329">In dieser Aufgabe überprüfen Sie, ob auf der Seite Speicher Index nun die in der Datenbank gespeicherten Genres anstelle der hart codierten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="25db0-330">Die Ansichts Vorlage muss nicht geändert werden, da der **StoreController** das gleiche **storeindexviewmodel** zurückgibt wie zuvor, aber dieses Mal werden die Daten aus der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="25db0-331">Erstellen Sie die Lösung neu, und drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="25db0-332">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="25db0-332">The project starts in the Home page.</span></span> <span data-ttu-id="25db0-333">Vergewissern Sie sich, dass das Menü der **Genres** keine hart codierte Liste mehr ist und die Daten direkt aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="25db0-335">*Durchsuchen von Genres aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="25db0-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="25db0-336">Navigieren Sie nun zu einem beliebigen Genre, und überprüfen Sie, ob die Alben aus der Datenbank aufgefüllt</span><span class="sxs-lookup"><span data-stu-id="25db0-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="25db0-337">![Durchsuchen von Alben aus der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image23.png "Durchsuchen von Alben aus der Datenbank")</span><span class="sxs-lookup"><span data-stu-id="25db0-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="25db0-338">*Durchsuchen von Alben aus der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="25db0-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="25db0-339">Übung 3: Abfragen der Datenbank mit Parametern</span><span class="sxs-lookup"><span data-stu-id="25db0-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="25db0-340">In dieser Übung erfahren Sie, wie Sie die Datenbank mithilfe von Parametern Abfragen und wie Sie die Abfrageergebnis Strukturierung verwenden, ein Feature, mit dem die Anzahl der Datenbankzugriffe auf eine effizientere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-341">Weitere Informationen zur Abfrageergebnis Strukturierung finden Sie im folgenden [MSDN-Artikel](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="25db0-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="25db0-342">Aufgabe 1: Ändern von StoreController zum Abrufen von Alben aus der Datenbank</span><span class="sxs-lookup"><span data-stu-id="25db0-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="25db0-343">In dieser Aufgabe ändern Sie die **StoreController** -Klasse so, dass Sie auf die Datenbank zugreift, um Alben aus einem bestimmten Genre abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="25db0-344">Öffnen Sie die Projekt Mappe " **Begin** " im Ordner " **source\ex3-queryingthedatabasewithparameterscodefirst\begin** ", wenn Sie den Code-First-Ansatz oder den Ordner " **source\ex3-queryingthedatabasewithparametersdbfirst\begin** " verwenden möchten, wenn Sie den Daten Bank ersten Ansatz verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="25db0-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="25db0-345">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="25db0-346">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="25db0-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="25db0-347">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="25db0-348">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="25db0-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="25db0-349">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="25db0-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="25db0-350">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="25db0-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="25db0-351">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="25db0-352">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="25db0-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="25db0-353">Öffnen Sie die **StoreController** -Klasse, um die **Browse** -Aktionsmethode zu ändern.</span><span class="sxs-lookup"><span data-stu-id="25db0-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="25db0-354">Erweitern Sie dazu im **Projektmappen-Explorer**den Ordner **Controller** , und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="25db0-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="25db0-355">Ändern Sie die Methode zum **Durchsuchen** von Aktionen, um Alben für ein bestimmtes Genre abzurufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="25db0-356">Ersetzen Sie hierzu den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="25db0-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="25db0-357">(Code Ausschnitt- *Modelle und Datenzugriff-EX3 StoreController browsemethod*)</span><span class="sxs-lookup"><span data-stu-id="25db0-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="25db0-358">Um eine Auflistung der Entität aufzufüllen, müssen Sie die **include** -Methode verwenden, um anzugeben, dass auch die Alben abgerufen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="25db0-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="25db0-359">Sie können das verwenden. **Single ()** -Erweiterung in LINQ, da in diesem Fall nur ein Genre für ein Album erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="25db0-360">Die **Single ()** -Methode nimmt einen Lambda-Ausdruck als Parameter an, der in diesem Fall ein einzelnes Genre Objekt angibt, sodass der Name mit dem definierten Wert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="25db0-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="25db0-361">Sie profitieren von einer Funktion, die es Ihnen ermöglicht, auch andere Verwandte Entitäten anzugeben, die Sie laden möchten, wenn das Genre Objekt abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="25db0-362">Diese Funktion wird als **Abfrageergebnis Strukturierung**bezeichnet und ermöglicht es Ihnen, die Anzahl der zum Abrufen von Informationen benötigten Zeitpunkten für den Zugriff auf die Datenbank zu reduzieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="25db0-363">In diesem Szenario möchten Sie die Alben für das Genre, das Sie abrufen, vorab abrufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="25db0-364">Die Abfrage enthält **Genres. include (&quot;Alben&quot;)** , um anzugeben, dass Sie auch Verwandte Alben benötigen.</span><span class="sxs-lookup"><span data-stu-id="25db0-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="25db0-365">Dies führt zu einer effizienteren Anwendung, da Sie sowohl Genre-als auch Album Daten in einer einzelnen Daten Bank Anforderung abruft.</span><span class="sxs-lookup"><span data-stu-id="25db0-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="25db0-366">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="25db0-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="25db0-367">In dieser Aufgabe werden Sie die Anwendung ausführen und Alben eines bestimmten Genres aus der Datenbank abrufen.</span><span class="sxs-lookup"><span data-stu-id="25db0-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="25db0-368">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="25db0-369">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="25db0-369">The project starts in the Home page.</span></span> <span data-ttu-id="25db0-370">Ändern Sie die URL in **/Store/Browse? Genre = Pop** , um zu überprüfen, ob die Ergebnisse aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="25db0-371">![Durchsuchen nach Genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Durchsuchen nach Genre")</span><span class="sxs-lookup"><span data-stu-id="25db0-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="25db0-372">*Durchsuchen/Store/Browse? Genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="25db0-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="25db0-373">Aufgabe 3: Zugreifen auf Alben nach ID</span><span class="sxs-lookup"><span data-stu-id="25db0-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="25db0-374">In dieser Aufgabe wiederholen Sie das vorherige Verfahren, um Alben anhand ihrer ID zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="25db0-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="25db0-375">Schließen Sie den Browser bei Bedarf, um zu Visual Studio zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="25db0-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="25db0-376">Öffnen Sie die **StoreController** -Klasse, um die **Detail** Aktionsmethode zu ändern.</span><span class="sxs-lookup"><span data-stu-id="25db0-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="25db0-377">Erweitern Sie dazu im **Projektmappen-Explorer**den Ordner **Controller** , und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="25db0-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="25db0-378">Ändern Sie die **Detail** Aktionsmethode, um die Details der Alben basierend auf Ihrer **ID**abzurufen. Ersetzen Sie hierzu den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="25db0-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="25db0-379">(Code Ausschnitt- *Modelle und Datenzugriff-EX3 StoreController detailsmethod*)</span><span class="sxs-lookup"><span data-stu-id="25db0-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="25db0-380">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="25db0-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="25db0-381">In dieser Aufgabe führen Sie die Anwendung in einem Webbrowser aus und erhalten die Details des Albums anhand ihrer ID.</span><span class="sxs-lookup"><span data-stu-id="25db0-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="25db0-382">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="25db0-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="25db0-383">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="25db0-383">The project starts in the Home page.</span></span> <span data-ttu-id="25db0-384">Ändern Sie die URL in **/Store/Details/51** , oder Durchsuchen Sie die Genres, und wählen Sie ein Album aus, um sicherzustellen, dass die Ergebnisse aus der Datenbank abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="25db0-385">![Details zum Durchsuchen](aspnet-mvc-4-models-and-data-access/_static/image25.png "Details zum Durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="25db0-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="25db0-386">*Durchsuchen/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="25db0-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="25db0-387">Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="25db0-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="25db0-388">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="25db0-388">Summary</span></span>

<span data-ttu-id="25db0-389">Durch die Durchführung dieser praktischen Übungseinheit haben Sie die Grundlagen von ASP.NET MVC-Modellen und-Datenzugriff unter Verwendung des **Database First** Ansatzes und des **Code First** Ansatzes kennengelernt:</span><span class="sxs-lookup"><span data-stu-id="25db0-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="25db0-390">Vorgehensweise beim Hinzufügen einer Datenbank zur Projekt Mappe, um Ihre Daten zu nutzen</span><span class="sxs-lookup"><span data-stu-id="25db0-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="25db0-391">Aktualisieren von Controllern zum Bereitstellen von Ansichts Vorlagen mit den Daten, die aus der Datenbank entnommen werden, anstelle von hart codierten Daten</span><span class="sxs-lookup"><span data-stu-id="25db0-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="25db0-392">Abfragen der Datenbank mithilfe von Parametern</span><span class="sxs-lookup"><span data-stu-id="25db0-392">How to query the database using parameters</span></span>
- <span data-ttu-id="25db0-393">Verwendung der Abfrageergebnis Strukturierung, ein Feature, das die Anzahl der Datenbankzugriffe reduziert und Daten effizienter abruft</span><span class="sxs-lookup"><span data-stu-id="25db0-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="25db0-394">Verwenden von Database First-und Code First Ansätzen in Microsoft Entity Framework zum Verknüpfen der Datenbank mit dem Modell</span><span class="sxs-lookup"><span data-stu-id="25db0-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="25db0-395">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="25db0-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="25db0-396">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="25db0-397">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="25db0-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="25db0-398">Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="25db0-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="25db0-399">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="25db0-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="25db0-400">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="25db0-400">Click on **Install Now**.</span></span> <span data-ttu-id="25db0-401">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="25db0-402">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="25db0-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="25db0-403">![Installieren von Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="25db0-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="25db0-404">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="25db0-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="25db0-405">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="25db0-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="25db0-407">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="25db0-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="25db0-408">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="25db0-408">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="25db0-410">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="25db0-410">*Installation progress*</span></span>
6. <span data-ttu-id="25db0-411">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-411">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="25db0-413">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="25db0-413">*Installation completed*</span></span>
7. <span data-ttu-id="25db0-414">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="25db0-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="25db0-415">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="25db0-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="25db0-417">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="25db0-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="25db0-418">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="25db0-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="25db0-419">In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="25db0-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="25db0-420">Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="25db0-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="25db0-421">Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="25db0-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="25db0-422">Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="25db0-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="25db0-423">Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-423">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="25db0-424">![Anmelden bei Windows Azure-Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Anmelden bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="25db0-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="25db0-425">*Anmelden bei Windows Azure Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="25db0-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="25db0-426">Klicken Sie in der Befehlsleiste auf **neu** .</span><span class="sxs-lookup"><span data-stu-id="25db0-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="25db0-427">![Erstellen einer neuen Website](aspnet-mvc-4-models-and-data-access/_static/image32.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="25db0-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="25db0-428">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="25db0-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="25db0-429">Klicken Sie auf **Compute** - | **Website**.</span><span class="sxs-lookup"><span data-stu-id="25db0-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="25db0-430">Wählen Sie dann **schneller** Fassung aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="25db0-431">Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="25db0-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="25db0-432">Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="25db0-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="25db0-433">Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="25db0-434">Sie enthält keine Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="25db0-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="25db0-435">![Erstellen einer neuen Website mithilfe der schneller Fassung](aspnet-mvc-4-models-and-data-access/_static/image33.png "Erstellen einer neuen Website mithilfe der schneller Fassung")</span><span class="sxs-lookup"><span data-stu-id="25db0-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="25db0-436">*Erstellen einer neuen Website mithilfe der schneller Fassung*</span><span class="sxs-lookup"><span data-stu-id="25db0-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="25db0-437">Warten Sie, bis die neue **Website** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="25db0-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="25db0-438">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte.</span><span class="sxs-lookup"><span data-stu-id="25db0-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="25db0-439">Überprüfen Sie, ob die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="25db0-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="25db0-440">![Navigieren zur neuen Website](aspnet-mvc-4-models-and-data-access/_static/image34.png "Navigieren zur neuen Website")</span><span class="sxs-lookup"><span data-stu-id="25db0-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="25db0-441">*Navigieren zur neuen Website*</span><span class="sxs-lookup"><span data-stu-id="25db0-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="25db0-442">![Website wird ausgeführt](aspnet-mvc-4-models-and-data-access/_static/image35.png "Website wird ausgeführt")</span><span class="sxs-lookup"><span data-stu-id="25db0-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="25db0-443">*Website wird ausgeführt*</span><span class="sxs-lookup"><span data-stu-id="25db0-443">*Web site running*</span></span>
6. <span data-ttu-id="25db0-444">Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="25db0-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="25db0-445">![Öffnen der Website-Verwaltungs Seiten](aspnet-mvc-4-models-and-data-access/_static/image36.png "Öffnen der Website-Verwaltungs Seiten")</span><span class="sxs-lookup"><span data-stu-id="25db0-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="25db0-446">*Öffnen der Website-Verwaltungs Seiten*</span><span class="sxs-lookup"><span data-stu-id="25db0-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="25db0-447">Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .</span><span class="sxs-lookup"><span data-stu-id="25db0-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="25db0-448">Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="25db0-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="25db0-449">Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="25db0-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="25db0-450">**Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="25db0-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="25db0-451">![Das Website-Veröffentlichungs Profil wird heruntergeladen.](aspnet-mvc-4-models-and-data-access/_static/image37.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")</span><span class="sxs-lookup"><span data-stu-id="25db0-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="25db0-452">*Das Website-Veröffentlichungs Profil wird heruntergeladen.*</span><span class="sxs-lookup"><span data-stu-id="25db0-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="25db0-453">Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="25db0-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="25db0-454">In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="25db0-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="25db0-455">![Die Veröffentlichungs Profil Datei wird gespeichert.](aspnet-mvc-4-models-and-data-access/_static/image38.png "Das Veröffentlichungs Profil wird gespeichert.")</span><span class="sxs-lookup"><span data-stu-id="25db0-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="25db0-456">*Die Veröffentlichungs Profil Datei wird gespeichert.*</span><span class="sxs-lookup"><span data-stu-id="25db0-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="25db0-457">Aufgabe 2: Konfigurieren des Datenbankservers</span><span class="sxs-lookup"><span data-stu-id="25db0-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="25db0-458">Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="25db0-459">Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="25db0-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="25db0-460">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="25db0-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="25db0-461">Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen.</span><span class="sxs-lookup"><span data-stu-id="25db0-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="25db0-462">Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen.</span><span class="sxs-lookup"><span data-stu-id="25db0-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="25db0-463">Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="25db0-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="25db0-464">Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="25db0-465">![Dashboard des SQL-Datenbankservers](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard des SQL-Datenbankservers")</span><span class="sxs-lookup"><span data-stu-id="25db0-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="25db0-466">*Dashboard des SQL-Datenbankservers*</span><span class="sxs-lookup"><span data-stu-id="25db0-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="25db0-467">In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen.</span><span class="sxs-lookup"><span data-stu-id="25db0-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="25db0-468">Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](aspnet-mvc-4-models-and-data-access/_static/image40.png)</span><span class="sxs-lookup"><span data-stu-id="25db0-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Client-IP-Adresse](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="25db0-470">*Client-IP-Adresse*</span><span class="sxs-lookup"><span data-stu-id="25db0-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="25db0-471">Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="25db0-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Änderungen bestätigen](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="25db0-473">*Änderungen bestätigen*</span><span class="sxs-lookup"><span data-stu-id="25db0-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="25db0-474">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="25db0-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="25db0-475">Kehren Sie zur ASP.NET MVC 4-Lösung zurück.</span><span class="sxs-lookup"><span data-stu-id="25db0-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="25db0-476">Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="25db0-477">![Veröffentlichen der Anwendung](aspnet-mvc-4-models-and-data-access/_static/image43.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="25db0-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="25db0-478">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="25db0-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="25db0-479">Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.</span><span class="sxs-lookup"><span data-stu-id="25db0-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="25db0-480">![Importieren des Veröffentlichungs Profils](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importieren des Veröffentlichungs Profils")</span><span class="sxs-lookup"><span data-stu-id="25db0-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="25db0-481">*Veröffentlichungs Profil wird importiert.*</span><span class="sxs-lookup"><span data-stu-id="25db0-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="25db0-482">Klicken Sie auf **Verbindung**überprüfen.</span><span class="sxs-lookup"><span data-stu-id="25db0-482">Click **Validate Connection**.</span></span> <span data-ttu-id="25db0-483">Klicken Sie nach Abschluss der Überprüfung auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="25db0-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="25db0-484">Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="25db0-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="25db0-485">![Die Verbindung wird überprüft.](aspnet-mvc-4-models-and-data-access/_static/image45.png "Die Verbindung wird überprüft.")</span><span class="sxs-lookup"><span data-stu-id="25db0-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="25db0-486">*Die Verbindung wird überprüft.*</span><span class="sxs-lookup"><span data-stu-id="25db0-486">*Validating connection*</span></span>
4. <span data-ttu-id="25db0-487">Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="25db0-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="25db0-488">![Webbereitstellungs Konfiguration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Webbereitstellungs Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="25db0-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="25db0-489">*Webbereitstellungs Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="25db0-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="25db0-490">Konfigurieren Sie die Datenbankverbindung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="25db0-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="25db0-491">Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="25db0-492">Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="25db0-493">Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="25db0-494">Geben Sie einen neuen Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="25db0-494">Type a new database name.</span></span>

     <span data-ttu-id="25db0-495">![Konfigurieren der Ziel Verbindungs Zeichenfolge](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="25db0-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="25db0-496">*Konfigurieren der Ziel Verbindungs Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="25db0-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="25db0-497">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="25db0-497">Then click **OK**.</span></span> <span data-ttu-id="25db0-498">Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="25db0-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="25db0-499">![Erstellen der Datenbank](aspnet-mvc-4-models-and-data-access/_static/image48.png "Erstellen der Daten Bank Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="25db0-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="25db0-500">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="25db0-500">*Creating the database*</span></span>
7. <span data-ttu-id="25db0-501">Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25db0-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="25db0-502">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="25db0-502">Then click **Next**.</span></span>

    <span data-ttu-id="25db0-503">![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](aspnet-mvc-4-models-and-data-access/_static/image49.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")</span><span class="sxs-lookup"><span data-stu-id="25db0-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="25db0-504">*Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="25db0-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="25db0-505">Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="25db0-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="25db0-506">![Veröffentlichen der Webanwendung](aspnet-mvc-4-models-and-data-access/_static/image50.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="25db0-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="25db0-507">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="25db0-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="25db0-508">Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="25db0-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="25db0-509">Anhang C: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="25db0-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="25db0-510">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="25db0-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="25db0-511">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="25db0-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="25db0-512">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-models-and-data-access/_static/image51.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="25db0-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="25db0-513">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="25db0-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="25db0-514">***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***</span><span class="sxs-lookup"><span data-stu-id="25db0-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="25db0-515">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="25db0-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="25db0-516">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="25db0-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="25db0-517">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="25db0-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="25db0-518">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="25db0-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="25db0-519">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="25db0-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="25db0-520">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-models-and-data-access/_static/image52.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="25db0-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="25db0-521">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="25db0-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="25db0-522">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-models-and-data-access/_static/image53.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="25db0-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="25db0-523">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="25db0-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="25db0-524">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-models-and-data-access/_static/image54.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="25db0-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="25db0-525">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="25db0-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="25db0-526">So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="25db0-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="25db0-527">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="25db0-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="25db0-528">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="25db0-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="25db0-529">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="25db0-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="25db0-530">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-models-and-data-access/_static/image55.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="25db0-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="25db0-531">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="25db0-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="25db0-532">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-models-and-data-access/_static/image56.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="25db0-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="25db0-533">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="25db0-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
