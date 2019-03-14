---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Abrufen und Anzeigen von Daten mit model-Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056597"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="f955c-104">Abrufen und Anzeigen von Daten mit modellbindung und Web forms</span><span class="sxs-lookup"><span data-stu-id="f955c-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="f955c-105">Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="f955c-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="f955c-106">Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement).</span><span class="sxs-lookup"><span data-stu-id="f955c-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="f955c-107">Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.</span><span class="sxs-lookup"><span data-stu-id="f955c-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="f955c-108">Der Modell-bindungsmuster funktioniert mit jedem datenzugriffstechnologie.</span><span class="sxs-lookup"><span data-stu-id="f955c-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="f955c-109">In diesem Tutorial verwenden Sie Entity Framework, jedoch können Sie die neue datenzugriffstechnologie, die Sie bereits bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="f955c-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="f955c-110">Von einem datengebundenen Serversteuerelement, z. B. eine GridView, ListView, DetailsView oder FormView-Steuerelement geben Sie den Namen der Methoden zum auswählen, aktualisieren, löschen und Erstellen von Daten verwendet.</span><span class="sxs-lookup"><span data-stu-id="f955c-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="f955c-111">In diesem Tutorial werden Sie einen Wert für die SelectMethod angeben.</span><span class="sxs-lookup"><span data-stu-id="f955c-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="f955c-112">In dieser Methode geben Sie die Logik zum Abrufen der Daten.</span><span class="sxs-lookup"><span data-stu-id="f955c-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="f955c-113">Im nächsten Tutorial legen Sie Werte für UpdateMethod, DeleteMethod und InsertMethod fest.</span><span class="sxs-lookup"><span data-stu-id="f955c-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="f955c-114">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) in das vollständige Projekt C# oder Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="f955c-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="f955c-115">Der herunterladbare Code funktioniert mit Visual Studio 2012 und höher.</span><span class="sxs-lookup"><span data-stu-id="f955c-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="f955c-116">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2017-Vorlage, die in diesem Tutorial gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="f955c-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="f955c-117">In diesem Tutorial führen Sie die Anwendung in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f955c-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="f955c-118">Sie können auch Bereitstellen der Anwendung bei einem Hostinganbieter und über das Internet verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="f955c-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="f955c-119">Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem</span><span class="sxs-lookup"><span data-stu-id="f955c-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="f955c-120">[Kostenloses Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="f955c-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="f955c-121">Informationen dazu, wie Sie ein Visual Studio-Webprojekt für Azure App Service-Web-Apps bereitstellen, finden Sie unter den [ASP.NET-webbereitstellung mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Reihe.</span><span class="sxs-lookup"><span data-stu-id="f955c-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="f955c-122">In diesem Tutorial wird gezeigt, wie Entity Framework Code First-Migrationen zu verwenden, um Ihre SQL Server-Datenbank in Azure SQL-Datenbank bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f955c-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f955c-123">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f955c-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="f955c-124">Microsoft Visual Studio 2017 "oder" Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="f955c-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="f955c-125">In diesem Tutorial funktioniert auch mit Visual Studio 2012 und Visual Studio 2013, aber es gibt einige Unterschiede in der Vorlage "Benutzer"-Schnittstelle und Projekt.</span><span class="sxs-lookup"><span data-stu-id="f955c-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f955c-126">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="f955c-126">What you'll build</span></span>

<span data-ttu-id="f955c-127">In diesem Tutorial müssen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="f955c-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="f955c-128">Erstellen Sie Datenobjekte, die eine Universität mit Schüler/Studenten, die Lehrveranstaltungen widerspiegeln.</span><span class="sxs-lookup"><span data-stu-id="f955c-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="f955c-129">Erstellen von Tabellen aus den Objekten</span><span class="sxs-lookup"><span data-stu-id="f955c-129">Build database tables from the objects</span></span>
* <span data-ttu-id="f955c-130">Auffüllen der Datenbank mit Testdaten</span><span class="sxs-lookup"><span data-stu-id="f955c-130">Populate the database with test data</span></span>
* <span data-ttu-id="f955c-131">Anzeigen von Daten in einem Web form</span><span class="sxs-lookup"><span data-stu-id="f955c-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="f955c-132">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="f955c-132">Create the project</span></span>

1. <span data-ttu-id="f955c-133">Erstellen Sie in Visual Studio 2017 eine **ASP.NET-Webanwendung ((.NET Framework)** -Projekt namens **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="f955c-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Projekt erstellen](retrieving-data/_static/image19.png)

2. <span data-ttu-id="f955c-135">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f955c-135">Select **OK**.</span></span> <span data-ttu-id="f955c-136">Das Dialogfeld zum Auswählen einer Vorlage wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f955c-136">The dialog box to select a template appears.</span></span>

   ![Wählen Sie die Web forms](retrieving-data/_static/image3.png)

3. <span data-ttu-id="f955c-138">Wählen Sie die **Web Forms** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f955c-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="f955c-139">Ändern Sie die Authentifizierung, bei Bedarf **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="f955c-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="f955c-140">Klicken Sie auf **OK**, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f955c-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="f955c-141">Erscheinungsbild der Website geändert werden.</span><span class="sxs-lookup"><span data-stu-id="f955c-141">Modify site appearance</span></span>

   <span data-ttu-id="f955c-142">Stellen Sie einige Änderungen an den Standort Darstellung anpassen.</span><span class="sxs-lookup"><span data-stu-id="f955c-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="f955c-143">Öffnen Sie die Datei Site.Master.</span><span class="sxs-lookup"><span data-stu-id="f955c-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="f955c-144">Ändern Sie den Titel anzuzeigende **Contoso University** und nicht **My ASP.NET Application**.</span><span class="sxs-lookup"><span data-stu-id="f955c-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="f955c-145">Ändern Sie den Headertext von **Anwendungsname** zu **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="f955c-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="f955c-146">Ändern Sie die Header-Navigationslinks, auf die entsprechende anpassen.</span><span class="sxs-lookup"><span data-stu-id="f955c-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="f955c-147">Entfernen Sie die Links, um **zu** und **wenden Sie sich an** und verknüpfen Sie stattdessen mit einem **Schüler/Studenten** Seite, die Sie erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="f955c-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="f955c-148">Speichern Sie Site.Master.</span><span class="sxs-lookup"><span data-stu-id="f955c-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="f955c-149">Fügen Sie ein Webformular zum Anzeigen der Studentendaten</span><span class="sxs-lookup"><span data-stu-id="f955c-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="f955c-150">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen** und dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="f955c-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="f955c-151">In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **Webformular mit Gestaltungsvorlage** Vorlage und nennen Sie sie **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="f955c-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Seite erstellen](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="f955c-153">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="f955c-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="f955c-154">Wählen Sie für das Web Form die Masterseite **Site.Master**.</span><span class="sxs-lookup"><span data-stu-id="f955c-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="f955c-155">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f955c-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="f955c-156">Datenmodell hinzufügen</span><span class="sxs-lookup"><span data-stu-id="f955c-156">Add the data model</span></span>

<span data-ttu-id="f955c-157">In der **Modelle** Ordner, fügen Sie eine Klasse, die mit dem Namen **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="f955c-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="f955c-158">Mit der rechten Maustaste **Modelle**Option **hinzufügen**, und klicken Sie dann **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="f955c-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="f955c-159">Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f955c-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="f955c-160">Wählen Sie im linken Navigationsmenü im **Code**, klicken Sie dann **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="f955c-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Erstellen von Model-Klasse](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="f955c-162">Nennen Sie die Klasse **UniversityModels.cs** , und wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f955c-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="f955c-163">In dieser Datei definieren die `SchoolContext`, `Student`, `Enrollment`, und `Course` Klassen wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f955c-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="f955c-164">Die `SchoolContext` Klasse leitet sich von `DbContext`, die Verbindung mit der Datenbank verwaltet und Änderungen in den Daten.</span><span class="sxs-lookup"><span data-stu-id="f955c-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="f955c-165">In der `Student` Klasse, beachten Sie, dass Attribute angewendet werden, um die `FirstName`, `LastName`, und `Year` Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="f955c-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="f955c-166">Dieses Tutorial verwendet diese Attribute für die datenüberprüfung.</span><span class="sxs-lookup"><span data-stu-id="f955c-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="f955c-167">Um den Code zu vereinfachen, werden nur diese Eigenschaften mit datenvalidierung Attributen markiert.</span><span class="sxs-lookup"><span data-stu-id="f955c-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="f955c-168">In einem echten Projekt würde Sie Validierungsattribute, die allen Eigenschaften, die Validierung erforderlich anwenden.</span><span class="sxs-lookup"><span data-stu-id="f955c-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="f955c-169">Save UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="f955c-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="f955c-170">Richten Sie die Datenbank auf Basis von Klassen</span><span class="sxs-lookup"><span data-stu-id="f955c-170">Set up the database based on classes</span></span>

<span data-ttu-id="f955c-171">Dieses Tutorial verwendet [Code First-Migrationen](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) Objekte erstellen und Datenbanktabellen.</span><span class="sxs-lookup"><span data-stu-id="f955c-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="f955c-172">Diese Tabellen speichern Informationen über die Schüler/Studenten und ihre Kurse.</span><span class="sxs-lookup"><span data-stu-id="f955c-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="f955c-173">Wählen Sie **Tools** > **NuGet Paket-Manager** > **Paket-Manager Konsole**.</span><span class="sxs-lookup"><span data-stu-id="f955c-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="f955c-174">In **-Paket-Manager-Konsole**, führen Sie diesen Befehl:</span><span class="sxs-lookup"><span data-stu-id="f955c-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="f955c-175">Wenn der Befehl erfolgreich abgeschlossen wurde, Meldung eine darauf hinweist, dass Migrationen aktiviert wurden.</span><span class="sxs-lookup"><span data-stu-id="f955c-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Aktivieren von Migrationen](retrieving-data/_static/image8.png)

      <span data-ttu-id="f955c-177">Beachten Sie, dass eine Datei mit dem Namen *Configuration.cs* erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="f955c-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="f955c-178">Die `Configuration` -Klasse verfügt über eine `Seed` -Methode, die die Datenbanktabellen mit Testdaten vorab auffüllen kann.</span><span class="sxs-lookup"><span data-stu-id="f955c-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="f955c-179">Die Datenbank vorab auffüllen</span><span class="sxs-lookup"><span data-stu-id="f955c-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="f955c-180">Öffnen Sie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="f955c-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="f955c-181">Fügen Sie der `Seed` -Methode folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f955c-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="f955c-182">Fügen Sie außerdem eine `using` -Anweisung für die `ContosoUniversityModelBinding. Models` Namespace.</span><span class="sxs-lookup"><span data-stu-id="f955c-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="f955c-183">Configuration.cs zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f955c-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="f955c-184">Führen Sie in der Paket-Manager-Konsole den Befehl **hinzufügen-Migration Initial**.</span><span class="sxs-lookup"><span data-stu-id="f955c-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="f955c-185">Führen Sie den Befehl **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="f955c-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="f955c-186">Wenn ein Fehler, beim Ausführen dieses Befehls auftritt, der `StudentID` und `CourseID` Werte möglicherweise sich von der `Seed` Methode Werte.</span><span class="sxs-lookup"><span data-stu-id="f955c-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="f955c-187">Öffnen Sie die Datenbanktabellen und suchen Sie die vorhandenen Werte für `StudentID` und `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="f955c-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="f955c-188">Fügen Sie diese Werte auf den Code für das seeding der `Enrollments` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="f955c-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="f955c-189">Hinzufügen einer GridView-Steuerelement</span><span class="sxs-lookup"><span data-stu-id="f955c-189">Add a GridView control</span></span>

<span data-ttu-id="f955c-190">Mit Datenbankdaten aufgefüllten können Sie nun die Daten abzurufen und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f955c-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="f955c-191">Öffnen Sie Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="f955c-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="f955c-192">Suchen Sie die `MainContent` Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="f955c-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="f955c-193">In dieser Platzhalter Hinzufügen einer **GridView** -Steuerelement, das dieser Code enthält.</span><span class="sxs-lookup"><span data-stu-id="f955c-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="f955c-194">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="f955c-194">Things to note:</span></span>
   * <span data-ttu-id="f955c-195">Beachten Sie, dass der Wert festgelegt wird, für die `SelectMethod` Eigenschaft im GridView-Element.</span><span class="sxs-lookup"><span data-stu-id="f955c-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="f955c-196">Dieser Wert gibt die Methode zum Abrufen von GridView-Daten, die Sie im nächsten Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="f955c-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="f955c-197">Die `ItemType` -Eigenschaftensatz auf die `Student` Klasse, die zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f955c-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="f955c-198">Mit dieser Einstellung können Sie Eigenschaften der Klasse im Markup zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="f955c-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="f955c-199">Z. B. die `Student` -Klasse verfügt über eine Sammlung namens `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="f955c-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="f955c-200">Können Sie `Item.Enrollments` dieser Sammlung abrufen und anschließend [LINQ-Syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) zum Abrufen von jedem Teilnehmer der Gutschrift Summe registriert.</span><span class="sxs-lookup"><span data-stu-id="f955c-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="f955c-201">Students.aspx zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f955c-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="f955c-202">Fügen Sie Code zum Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="f955c-202">Add code to retrieve data</span></span>

   <span data-ttu-id="f955c-203">Fügen Sie in der Students.aspx Code-Behind-Datei für die angegebene Methode der `SelectMethod` Wert.</span><span class="sxs-lookup"><span data-stu-id="f955c-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="f955c-204">Öffnen Sie Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="f955c-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="f955c-205">Hinzufügen `using` -Anweisungen für die `ContosoUniversityModelBinding. Models` und `System.Data.Entity` Namespaces.</span><span class="sxs-lookup"><span data-stu-id="f955c-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="f955c-206">Fügen Sie die Methode, die Sie für angegebene `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="f955c-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="f955c-207">Die `Include` -Klausel wird die abfrageleistung verbessert, jedoch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f955c-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="f955c-208">Ohne die `Include` -Klausel, die Daten werden abgerufen, mithilfe von [ *Lazy Load*](https://en.wikipedia.org/wiki/Lazy_loading), hierbei ist, senden eine separate Abfrage an die Datenbank jedes Mal bezogene Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="f955c-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="f955c-209">Mit der `Include` -Klausel, die Daten werden abgerufen, mithilfe von *Eager Load*, was bedeutet, dass eine einzige Datenbankabfrage Ruft alle verknüpften Daten ab.</span><span class="sxs-lookup"><span data-stu-id="f955c-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="f955c-210">Verwandte Daten verwendet wird, ist eager Loading weniger effizient, da mehr Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="f955c-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="f955c-211">Allerdings bietet in diesem Fall unverzüglichem Laden Sie die beste Leistung daran, dass die verknüpften Daten für jeden Datensatz angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f955c-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="f955c-212">Weitere Informationen zu Überlegungen zur Leistung beim Laden von Daten, die verknüpft werden, finden Sie unter den **explizite Laden von verknüpften Daten, mittels Eager Load und Lazy** im Abschnitt der [verwandte Lesen von Daten mit dem Entity Framework in einer ASP.NET-Anwendung MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Artikel.</span><span class="sxs-lookup"><span data-stu-id="f955c-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="f955c-213">Standardmäßig ist die Daten durch die Werte der Eigenschaft als Schlüssel markiert sortiert.</span><span class="sxs-lookup"><span data-stu-id="f955c-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="f955c-214">Sie können Hinzufügen einer `OrderBy` -Klausel, um einen der anderen Art Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="f955c-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="f955c-215">In diesem Beispiel ist die Standardeinstellung `StudentID` Eigenschaft wird für die Sortierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="f955c-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="f955c-216">In der [sortieren, Paging und Filtern von Daten](sorting-paging-and-filtering-data.md) Artikel, der Benutzer wird aktiviert, um eine Spalte für die Sortierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="f955c-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="f955c-217">Students.aspx.cs zu speichern.</span><span class="sxs-lookup"><span data-stu-id="f955c-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="f955c-218">Führen Sie die Anwendung</span><span class="sxs-lookup"><span data-stu-id="f955c-218">Run your application</span></span> 

<span data-ttu-id="f955c-219">Führen Sie Ihre Webanwendung (**F5**) und navigieren Sie zu der **Schüler/Studenten** Seite, das Folgendes anzeigt:</span><span class="sxs-lookup"><span data-stu-id="f955c-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Anzeigen von Daten](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="f955c-221">Automatische Generierung von SMO-Methoden-Bindung</span><span class="sxs-lookup"><span data-stu-id="f955c-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="f955c-222">Wenn dieser tutorialreihe durcharbeiten, können Sie den Code einfach aus dem Lernprogramm zu Ihrem Projekt kopieren.</span><span class="sxs-lookup"><span data-stu-id="f955c-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="f955c-223">Ein Nachteil dieses Ansatzes ist jedoch, dass Sie nicht über die Funktion, die von Visual Studio zum automatischen Generieren von Code für die Bindung SMO-Methoden bereitgestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="f955c-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="f955c-224">Bei der Arbeit an Ihre eigenen Projekte sparen automatische codegenerierung Sie Zeit und die Hilfe, die Sie wissen, wie Implementieren eines Vorgangs erhalten.</span><span class="sxs-lookup"><span data-stu-id="f955c-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="f955c-225">Dieser Abschnitt beschreibt die automatischen Code Generation-Funktion.</span><span class="sxs-lookup"><span data-stu-id="f955c-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="f955c-226">In diesem Abschnitt dient nur zu Informationszwecken und keinen Code, die, den Sie in Ihrem Projekt implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="f955c-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="f955c-227">Beim Festlegen eines Werts für die `SelectMethod`, `UpdateMethod`, `InsertMethod`, oder `DeleteMethod` Eigenschaften im Markupcode, Sie können auswählen, die **neue Methode erstellen** Option.</span><span class="sxs-lookup"><span data-stu-id="f955c-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Erstellen Sie eine Methode](retrieving-data/_static/image18.png)

<span data-ttu-id="f955c-229">Visual Studio nicht nur eine Methode im Code-Behind mit der richtigen Signatur erstellt, sondern generiert auch den Implementierungscode, um den Vorgang auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f955c-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="f955c-230">Wenn Sie zuerst festlegen, die `ItemType` Eigenschaft, bevor Sie die automatische codegenerierung mit Features, die generierten Code verwendet, die eingeben, und für die Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="f955c-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="f955c-231">Beispielsweise wird beim Festlegen der `UpdateMethod` -Eigenschaft, den folgenden Code wird automatisch generiert:</span><span class="sxs-lookup"><span data-stu-id="f955c-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="f955c-232">Dieser Code muss in diesem Fall nicht zu Ihrem Projekt hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="f955c-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="f955c-233">Im nächsten Tutorial implementieren Sie Methoden zum Aktualisieren, löschen und neue Daten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f955c-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="f955c-234">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="f955c-234">Summary</span></span>

<span data-ttu-id="f955c-235">In diesem Tutorial erstellten Data Model-Klassen und eine Datenbank aus den Klassen generiert.</span><span class="sxs-lookup"><span data-stu-id="f955c-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="f955c-236">Sie können Tabellen der Datenbank mit Testdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="f955c-236">You filled the database tables with test data.</span></span> <span data-ttu-id="f955c-237">Sie wurden die modellbindung zum Abrufen von Daten aus der Datenbank verwendet, und klicken Sie dann die Daten in einer GridView-Ansicht angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f955c-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="f955c-238">In den nächsten [Tutorial](updating-deleting-and-creating-data.md) in dieser Reihe werde Sie aktivieren, aktualisieren, löschen und Erstellen von Daten.</span><span class="sxs-lookup"><span data-stu-id="f955c-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f955c-239">Nächste</span><span class="sxs-lookup"><span data-stu-id="f955c-239">Next</span></span>](updating-deleting-and-creating-data.md)
