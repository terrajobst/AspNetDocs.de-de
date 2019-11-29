---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633175"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="65156-104">Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms</span><span class="sxs-lookup"><span data-stu-id="65156-104">Retrieving and displaying data with model binding and web forms</span></span>

> <span data-ttu-id="65156-105">In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="65156-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="65156-106">Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="65156-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="65156-107">Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.</span><span class="sxs-lookup"><span data-stu-id="65156-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="65156-108">Das Modell bindungsmuster funktioniert mit jeder Datenzugriffs Technologie.</span><span class="sxs-lookup"><span data-stu-id="65156-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="65156-109">In diesem Tutorial verwenden Sie Entity Framework, aber Sie können die Datenzugriffs Technologie verwenden, die Ihnen am meisten vertraut ist.</span><span class="sxs-lookup"><span data-stu-id="65156-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="65156-110">Über ein Daten gebundenes Server Steuerelement, z. b. ein GridView-, ListView-, DetailsView-oder FormView-Steuerelement, geben Sie die Namen der Methoden an, die zum auswählen, aktualisieren, löschen und Erstellen von Daten verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="65156-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="65156-111">In diesem Tutorial geben Sie einen Wert für die SelectMethod-Methode an.</span><span class="sxs-lookup"><span data-stu-id="65156-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="65156-112">Innerhalb dieser Methode stellen Sie die Logik zum Abrufen der Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="65156-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="65156-113">Im nächsten Tutorial legen Sie Werte für UpdateMethod, DeleteMethod und InsertMethod fest.</span><span class="sxs-lookup"><span data-stu-id="65156-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="65156-114">Sie können das gesamte Projekt in C# oder Visual Basic [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) .</span><span class="sxs-lookup"><span data-stu-id="65156-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="65156-115">Der herunterladbare Code funktioniert mit Visual Studio 2012 und höher.</span><span class="sxs-lookup"><span data-stu-id="65156-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="65156-116">Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Vorlage in Visual Studio 2017 unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="65156-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="65156-117">In diesem Tutorial führen Sie die Anwendung in Visual Studio aus.</span><span class="sxs-lookup"><span data-stu-id="65156-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="65156-118">Sie können die Anwendung auch für einen Hostinganbieter bereitstellen und über das Internet verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="65156-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="65156-119">Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einer</span><span class="sxs-lookup"><span data-stu-id="65156-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
> <span data-ttu-id="65156-120">[Kostenloses Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="65156-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="65156-121">Informationen zum Bereitstellen eines Visual Studio-Webprojekts für die Azure App Service von Web-Apps finden Sie unter [ASP.net Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series.</span><span class="sxs-lookup"><span data-stu-id="65156-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="65156-122">In diesem Tutorial erfahren Sie außerdem, wie Sie Entity Framework Code First-Migrationen zum Bereitstellen Ihrer SQL Server-Datenbank in Azure SQL-Datenbank verwenden.</span><span class="sxs-lookup"><span data-stu-id="65156-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="65156-123">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="65156-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="65156-124">Microsoft Visual Studio 2017 oder Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="65156-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="65156-125">Dieses Tutorial funktioniert auch mit Visual Studio 2012 und Visual Studio 2013. es gibt jedoch einige Unterschiede in der Benutzeroberfläche und der Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="65156-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="65156-126">Was Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="65156-126">What you'll build</span></span>

<span data-ttu-id="65156-127">In diesem Tutorial gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="65156-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="65156-128">Erstellen von Datenobjekten, die eine Universität mit Studenten widerspiegeln, die in Kursen angemeldet sind</span><span class="sxs-lookup"><span data-stu-id="65156-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="65156-129">Erstellen von Datenbanktabellen aus den Objekten</span><span class="sxs-lookup"><span data-stu-id="65156-129">Build database tables from the objects</span></span>
* <span data-ttu-id="65156-130">Auffüllen der Datenbank mit Testdaten</span><span class="sxs-lookup"><span data-stu-id="65156-130">Populate the database with test data</span></span>
* <span data-ttu-id="65156-131">Anzeigen von Daten in einem Webformular</span><span class="sxs-lookup"><span data-stu-id="65156-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="65156-132">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="65156-132">Create the project</span></span>

1. <span data-ttu-id="65156-133">Erstellen Sie in Visual Studio 2017 ein Projekt mit dem Namen " **contosouniversitymodelbinding**" der **ASP.NET-Webanwendung (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="65156-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Projekt erstellen](retrieving-data/_static/image19.png)

2. <span data-ttu-id="65156-135">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="65156-135">Select **OK**.</span></span> <span data-ttu-id="65156-136">Das Dialogfeld zum Auswählen einer Vorlage wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="65156-136">The dialog box to select a template appears.</span></span>

   ![Auswählen von Web Forms](retrieving-data/_static/image3.png)

3. <span data-ttu-id="65156-138">Wählen Sie die **Web Forms** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="65156-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="65156-139">Ändern Sie ggf. die Authentifizierung in **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="65156-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="65156-140">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="65156-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="65156-141">Website Darstellung ändern</span><span class="sxs-lookup"><span data-stu-id="65156-141">Modify site appearance</span></span>

   <span data-ttu-id="65156-142">Nehmen Sie einige Änderungen vor, um das Erscheinungsbild der Website anzupassen.</span><span class="sxs-lookup"><span data-stu-id="65156-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="65156-143">Öffnen Sie die Datei Site. Master.</span><span class="sxs-lookup"><span data-stu-id="65156-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="65156-144">Ändern Sie den Titel, um die "The **University** " und nicht die **ASP.NET-Anwendung**anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="65156-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="65156-145">Ändern Sie den Header Text vom **Anwendungsnamen** in die **Datei**"Configuration Manager".</span><span class="sxs-lookup"><span data-stu-id="65156-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="65156-146">Ändern Sie die Navigations Header Links zu den entsprechenden Websites.</span><span class="sxs-lookup"><span data-stu-id="65156-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="65156-147">Entfernen Sie die Links für " **about** " und " **Contact** ", und verknüpfen Sie die Seite mit der Seite " **Studenten** ", die Sie erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="65156-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="65156-148">Speichern Sie Site. Master.</span><span class="sxs-lookup"><span data-stu-id="65156-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="65156-149">Hinzufügen eines Webformulars zum Anzeigen von Studenten Daten</span><span class="sxs-lookup"><span data-stu-id="65156-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="65156-150">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** und dann **Neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="65156-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="65156-151">Wählen Sie im Dialogfeld **Neues Element hinzufügen** das **Webformular mit der Vorlage Master Seite** aus, und nennen Sie es **students. aspx**.</span><span class="sxs-lookup"><span data-stu-id="65156-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Seite erstellen](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="65156-153">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="65156-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="65156-154">Wählen Sie für die Master Seite des Webformulars die Option **Site. Master**aus.</span><span class="sxs-lookup"><span data-stu-id="65156-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="65156-155">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="65156-155">Select **OK**.</span></span>

## <a name="add-the-data-model"></a><span data-ttu-id="65156-156">Hinzufügen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="65156-156">Add the data model</span></span>

<span data-ttu-id="65156-157">Fügen Sie im Ordner **Models** eine Klasse mit dem Namen **UniversityModels.cs**hinzu.</span><span class="sxs-lookup"><span data-stu-id="65156-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="65156-158">Klicken Sie mit der rechten Maustaste auf **Modelle**, und wählen Sie **Hinzufügen**und **Neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="65156-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="65156-159">Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="65156-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="65156-160">Klicken Sie im linken Navigationsmenü auf **Code**und dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="65156-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Modell Klasse erstellen](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="65156-162">Benennen Sie die Klasse **UniversityModels.cs** , und wählen Sie **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="65156-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="65156-163">Definieren Sie in dieser Datei die Klassen `SchoolContext`, `Student`, `Enrollment`und `Course` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="65156-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="65156-164">Die `SchoolContext`-Klasse wird von `DbContext`abgeleitet, die die Datenbankverbindung und die Änderungen an den Daten verwaltet.</span><span class="sxs-lookup"><span data-stu-id="65156-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="65156-165">Beachten Sie in der `Student`-Klasse die Attribute, die auf die Eigenschaften `FirstName`, `LastName`und `Year` angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="65156-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="65156-166">In diesem Tutorial werden diese Attribute für die Datenüberprüfung verwendet.</span><span class="sxs-lookup"><span data-stu-id="65156-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="65156-167">Um den Code zu vereinfachen, werden nur diese Eigenschaften mit Daten Validierungs Attributen gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="65156-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="65156-168">In einem echten Projekt würden Sie Validierungs Attribute auf alle Eigenschaften anwenden, die überprüft werden müssen.</span><span class="sxs-lookup"><span data-stu-id="65156-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="65156-169">Speichern Sie UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="65156-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="65156-170">Einrichten der Datenbank auf der Grundlage von Klassen</span><span class="sxs-lookup"><span data-stu-id="65156-170">Set up the database based on classes</span></span>

<span data-ttu-id="65156-171">In diesem Tutorial werden [Code First-Migrationen](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) zum Erstellen von Objekten und Datenbanktabellen verwendet.</span><span class="sxs-lookup"><span data-stu-id="65156-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="65156-172">In diesen Tabellen werden Informationen zu den Studenten und deren Kursen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="65156-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="65156-173">Wählen **Sie** Extras > **nuget-Paket-Manager** > Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="65156-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="65156-174">Führen Sie in der **Paket-Manager-Konsole**den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="65156-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="65156-175">Wenn der Befehl erfolgreich abgeschlossen wurde, wird eine Meldung angezeigt, die besagt, dass Migrationen aktiviert wurden.</span><span class="sxs-lookup"><span data-stu-id="65156-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Migrationen aktivieren](retrieving-data/_static/image8.png)

      <span data-ttu-id="65156-177">Beachten Sie, dass eine Datei mit dem Namen *Configuration.cs* erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="65156-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="65156-178">Die `Configuration`-Klasse verfügt über eine `Seed`-Methode, mit der die Datenbanktabellen mit Testdaten vorab aufgefüllt werden können.</span><span class="sxs-lookup"><span data-stu-id="65156-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="65156-179">Vorab Auffüllen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="65156-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="65156-180">Öffnen Sie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="65156-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="65156-181">Fügen Sie der `Seed` -Methode folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="65156-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="65156-182">Fügen Sie außerdem eine `using`-Anweisung für den `ContosoUniversityModelBinding. Models`-Namespace hinzu.</span><span class="sxs-lookup"><span data-stu-id="65156-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="65156-183">Speichern Sie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="65156-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="65156-184">Führen Sie in der Paket-Manager-Konsole den Befehl **Add-Migration Initial**aus.</span><span class="sxs-lookup"><span data-stu-id="65156-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="65156-185">Führen Sie den Befehl **Update-Database**aus.</span><span class="sxs-lookup"><span data-stu-id="65156-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="65156-186">Wenn Sie beim Ausführen dieses Befehls eine Ausnahme erhalten, können sich die Werte für `StudentID` und `CourseID` von den Werten der `Seed`-Methode unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="65156-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="65156-187">Öffnen Sie diese Datenbanktabellen, und suchen Sie nach vorhandenen Werten für `StudentID` und `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="65156-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="65156-188">Fügen Sie diese Werte dem Code für das Seeding der `Enrollments` Tabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="65156-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="65156-189">Hinzufügen eines GridView-Steuer Elements</span><span class="sxs-lookup"><span data-stu-id="65156-189">Add a GridView control</span></span>

<span data-ttu-id="65156-190">Mit aufgefüllten Datenbankdaten sind Sie nun bereit, diese Daten abzurufen und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="65156-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="65156-191">Öffnen Sie students. aspx.</span><span class="sxs-lookup"><span data-stu-id="65156-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="65156-192">Suchen Sie den `MainContent` Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="65156-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="65156-193">Fügen Sie innerhalb dieses Platzhalters ein **GridView** -Steuerelement hinzu, das diesen Code enthält.</span><span class="sxs-lookup"><span data-stu-id="65156-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="65156-194">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="65156-194">Things to note:</span></span>
   * <span data-ttu-id="65156-195">Beachten Sie den Wert, der für die `SelectMethod`-Eigenschaft im GridView-Element festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="65156-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="65156-196">Dieser Wert gibt die Methode an, die zum Abrufen von GridView-Daten verwendet wird, die Sie im nächsten Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="65156-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="65156-197">Die `ItemType`-Eigenschaft wird auf die zuvor erstellte `Student` Klasse festgelegt.</span><span class="sxs-lookup"><span data-stu-id="65156-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="65156-198">Diese Einstellung ermöglicht es Ihnen, auf Klasseneigenschaften im Markup zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="65156-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="65156-199">Die `Student`-Klasse verfügt beispielsweise über eine Auflistung mit dem Namen `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="65156-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="65156-200">Sie können `Item.Enrollments` verwenden, um diese Auflistung abzurufen, und dann die [LINQ-Syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) verwenden, um die registrierten Gutschriften des Studenten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="65156-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="65156-201">Speichern Sie students. aspx.</span><span class="sxs-lookup"><span data-stu-id="65156-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="65156-202">Hinzufügen von Code zum Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="65156-202">Add code to retrieve data</span></span>

   <span data-ttu-id="65156-203">Fügen Sie in der Code Behind-Datei students. aspx die für den `SelectMethod` Wert angegebene Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="65156-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="65156-204">Öffnen Sie students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="65156-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="65156-205">Fügen Sie `using`-Anweisungen für die `ContosoUniversityModelBinding. Models`-und `System.Data.Entity`-Namespaces hinzu.</span><span class="sxs-lookup"><span data-stu-id="65156-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="65156-206">Fügen Sie die für `SelectMethod`angegebene Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="65156-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="65156-207">Die `Include`-Klausel verbessert die Abfrageleistung, ist aber nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="65156-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="65156-208">Ohne die `Include`-Klausel werden die Daten mithilfe [*Lazy Loading*](https://en.wikipedia.org/wiki/Lazy_loading)abgerufen. Dies umfasst das Senden einer separaten Abfrage an die Datenbank, wenn verknüpfte Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="65156-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="65156-209">Mit der `Include`-Klausel werden Daten mithilfe *Eager Loading*abgerufen, was bedeutet, dass eine einzelne Datenbankabfrage alle zugehörigen Daten abruft.</span><span class="sxs-lookup"><span data-stu-id="65156-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="65156-210">Wenn verwandte Daten nicht verwendet werden, ist Eager Loading weniger effizient, da mehr Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="65156-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="65156-211">In diesem Fall bietet Eager Loading jedoch die beste Leistung, da die zugehörigen Daten für jeden Datensatz angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="65156-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="65156-212">Weitere Informationen zu Leistungs Überlegungen beim Laden von verknüpften Daten finden Sie im Artikel **Lazy, eifrig und Explizites Laden verwandter Daten** im Artikel [Lesen verwandter Daten mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .</span><span class="sxs-lookup"><span data-stu-id="65156-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="65156-213">Standardmäßig werden die Daten nach den Werten der als Schlüssel markierten Eigenschaft sortiert.</span><span class="sxs-lookup"><span data-stu-id="65156-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="65156-214">Sie können eine `OrderBy`-Klausel hinzufügen, um einen anderen Sortier Wert anzugeben.</span><span class="sxs-lookup"><span data-stu-id="65156-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="65156-215">In diesem Beispiel wird die standardmäßige `StudentID`-Eigenschaft zum Sortieren verwendet.</span><span class="sxs-lookup"><span data-stu-id="65156-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="65156-216">Im Artikel [Sortieren, Paging und Filtern von Daten](sorting-paging-and-filtering-data.md) ist der Benutzer in der Lage, eine Spalte für die Sortierung auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="65156-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="65156-217">Speichern Sie students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="65156-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="65156-218">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="65156-218">Run your application</span></span> 

<span data-ttu-id="65156-219">Führen Sie Ihre Webanwendung (**F5**) aus, und navigieren Sie zur Seite " **Studenten** ", auf der Folgendes angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="65156-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Daten anzeigen](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="65156-221">Automatische Generierung von Modell Bindungsmethoden</span><span class="sxs-lookup"><span data-stu-id="65156-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="65156-222">Wenn Sie diese tutorialreihe durcharbeiten, können Sie einfach den Code aus dem Tutorial in Ihr Projekt kopieren.</span><span class="sxs-lookup"><span data-stu-id="65156-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="65156-223">Ein Nachteil dieses Ansatzes ist jedoch, dass Sie die von Visual Studio bereitgestellte Funktion möglicherweise nicht kennen, um automatisch Code für Modell Bindungsmethoden zu generieren.</span><span class="sxs-lookup"><span data-stu-id="65156-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="65156-224">Wenn Sie an Ihren eigenen Projekten arbeiten, können Sie mithilfe der automatischen Codegenerierung Zeit sparen und einen Eindruck davon gewinnen, wie ein Vorgang implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="65156-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="65156-225">In diesem Abschnitt wird das Feature zur automatischen Codegenerierung beschrieben.</span><span class="sxs-lookup"><span data-stu-id="65156-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="65156-226">Dieser Abschnitt dient nur zu Informationszwecken und enthält keinen Code, den Sie in Ihrem Projekt implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="65156-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="65156-227">Wenn Sie einen Wert für die Eigenschaften `SelectMethod`, `UpdateMethod`, `InsertMethod`oder `DeleteMethod` im Markup Code festlegen, können Sie die Option **neue Methode erstellen** auswählen.</span><span class="sxs-lookup"><span data-stu-id="65156-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Erstellen einer Methode](retrieving-data/_static/image18.png)

<span data-ttu-id="65156-229">Visual Studio erstellt nicht nur eine Methode im Code-Behind mit der richtigen Signatur, sondern generiert auch Implementierungs Code, um den Vorgang auszuführen.</span><span class="sxs-lookup"><span data-stu-id="65156-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="65156-230">Wenn Sie zuerst die `ItemType`-Eigenschaft festlegen, bevor Sie die Funktion zur automatischen Codegenerierung verwenden, verwendet der generierte Code diesen Typ für die Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="65156-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="65156-231">Wenn Sie z. b. die `UpdateMethod`-Eigenschaft festlegen, wird der folgende Code automatisch generiert:</span><span class="sxs-lookup"><span data-stu-id="65156-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="65156-232">Dieser Code muss dem Projekt nicht hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="65156-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="65156-233">Im nächsten Tutorial implementieren Sie Methoden zum Aktualisieren, löschen und Hinzufügen neuer Daten.</span><span class="sxs-lookup"><span data-stu-id="65156-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="65156-234">Summary</span><span class="sxs-lookup"><span data-stu-id="65156-234">Summary</span></span>

<span data-ttu-id="65156-235">In diesem Tutorial haben Sie Datenmodell Klassen erstellt und eine Datenbank aus diesen Klassen generiert.</span><span class="sxs-lookup"><span data-stu-id="65156-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="65156-236">Sie haben die Datenbanktabellen mit Testdaten ausgefüllt.</span><span class="sxs-lookup"><span data-stu-id="65156-236">You filled the database tables with test data.</span></span> <span data-ttu-id="65156-237">Sie haben die Modell Bindung verwendet, um Daten aus der Datenbank abzurufen, und dann die Daten in einer GridView angezeigt.</span><span class="sxs-lookup"><span data-stu-id="65156-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="65156-238">Im nächsten [Tutorial](updating-deleting-and-creating-data.md) dieser Reihe aktivieren Sie das Aktualisieren, löschen und Erstellen von Daten.</span><span class="sxs-lookup"><span data-stu-id="65156-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="65156-239">Nächste</span><span class="sxs-lookup"><span data-stu-id="65156-239">Next</span></span>](updating-deleting-and-creating-data.md)
