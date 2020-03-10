---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: Erstellen der Webanwendung und der Datenmodelle für EF-Database First mit ASP.NET MVC'
description: Dieses Tutorial konzentriert sich auf das Erstellen der Webanwendung und das Erstellen der Datenmodelle basierend auf den Datenbanktabellen.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499527"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="b4af4-103">Tutorial: Erstellen der Webanwendung und der Datenmodelle für EF-Database First mit ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b4af4-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="b4af4-104">Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b4af4-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b4af4-105">In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können.</span><span class="sxs-lookup"><span data-stu-id="b4af4-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b4af4-106">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="b4af4-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="b4af4-107">Dieses Tutorial konzentriert sich auf das Erstellen der Webanwendung und das Erstellen der Datenmodelle basierend auf den Datenbanktabellen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="b4af4-108">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="b4af4-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b4af4-109">Erstellen einer ASP.NET-Web-App</span><span class="sxs-lookup"><span data-stu-id="b4af4-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="b4af4-110">Generieren der Modelle</span><span class="sxs-lookup"><span data-stu-id="b4af4-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4af4-111">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b4af4-111">Prerequisites</span></span>

* [<span data-ttu-id="b4af4-112">Ersten Schritte mit Entity Framework 6 Database First mithilfe von MVC 5</span><span class="sxs-lookup"><span data-stu-id="b4af4-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="b4af4-113">Erstellen einer ASP.NET-Web-App</span><span class="sxs-lookup"><span data-stu-id="b4af4-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="b4af4-114">Erstellen Sie in einer neuen Projekt Mappe oder in derselben Projekt Mappe wie das Datenbankprojekt in Visual Studio ein neues Projekt, und wählen Sie die Vorlage **ASP.NET-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="b4af4-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="b4af4-115">Nennen Sie das Projekt **conjesite**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-115">Name the project **ContosoSite**.</span></span>

![Erstellen des Projekts](creating-the-web-application/_static/image1.png)

<span data-ttu-id="b4af4-117">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-117">Click **OK**.</span></span>

<span data-ttu-id="b4af4-118">Wählen Sie im Fenster Neues ASP.net-Projekt die **MVC** -Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="b4af4-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="b4af4-119">Sie können den **Host in der Cloud** -Option vorerst löschen, da Sie die Anwendung später in der Cloud bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="b4af4-120">Klicken Sie auf **OK** , um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="b4af4-121">Das Projekt wird mit den Standard Dateien und-Ordnern erstellt.</span><span class="sxs-lookup"><span data-stu-id="b4af4-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="b4af4-122">In diesem Tutorial wird Entity Framework 6 verwendet.</span><span class="sxs-lookup"><span data-stu-id="b4af4-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="b4af4-123">Im Fenster "nuget-Pakete verwalten" können Sie die Version der Entity Framework im Projekt überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="b4af4-124">Aktualisieren Sie ggf. Ihre Version von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b4af4-124">If necessary, update your version of Entity Framework.</span></span>

![Version anzeigen](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="b4af4-126">Generieren der Modelle</span><span class="sxs-lookup"><span data-stu-id="b4af4-126">Generate the models</span></span>

<span data-ttu-id="b4af4-127">Nun erstellen Sie Entity Framework Modelle aus den Datenbanktabellen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="b4af4-128">Diese Modelle sind Klassen, die Sie verwenden, um mit den Daten zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="b4af4-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="b4af4-129">Jedes Modell spiegelt eine Tabelle in der Datenbank wider und enthält Eigenschaften, die den Spalten in der Tabelle entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="b4af4-130">Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , und wählen Sie **Hinzufügen** und **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="b4af4-131">Wählen Sie im Fenster Neues Element hinzufügen im linken Bereich **Daten** aus, und **ADO.NET Entity Data Model** aus den Optionen im mittleren Bereich.</span><span class="sxs-lookup"><span data-stu-id="b4af4-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="b4af4-132">Benennen Sie die neue Modell **Datei mit**dem Namen "".</span><span class="sxs-lookup"><span data-stu-id="b4af4-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="b4af4-133">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-133">Click **Add**.</span></span>

<span data-ttu-id="b4af4-134">Wählen Sie im Assistenten für Entity Data Model den **EF-Designer aus der Datenbank aus**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="b4af4-135">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-135">Click **Next**.</span></span>

<span data-ttu-id="b4af4-136">Wenn Sie in Ihrer Entwicklungsumgebung Datenbankverbindungen definiert haben, wird möglicherweise eine dieser Verbindungen vorab ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="b4af4-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="b4af4-137">Sie möchten jedoch eine neue Verbindung mit der Datenbank erstellen, die Sie im ersten Teil dieses Tutorials erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b4af4-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="b4af4-138">Klicken Sie auf die Schaltfläche **neue Verbindung** .</span><span class="sxs-lookup"><span data-stu-id="b4af4-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="b4af4-139">Geben Sie im Verbindungs Eigenschaftenfenster den Namen des lokalen Servers an, auf dem die Datenbank erstellt wurde (in diesem Fall **(localdb) \projectv13**).</span><span class="sxs-lookup"><span data-stu-id="b4af4-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="b4af4-140">Nachdem Sie den Servernamen bereitgestellt haben, wählen Sie die Conto souniversitydata aus den verfügbaren Datenbanken aus.</span><span class="sxs-lookup"><span data-stu-id="b4af4-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Verbindungs Eigenschaften festlegen](creating-the-web-application/_static/image8.png)

<span data-ttu-id="b4af4-142">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-142">Click **OK**.</span></span>

<span data-ttu-id="b4af4-143">Die richtigen Verbindungs Eigenschaften werden nun angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b4af4-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="b4af4-144">Sie können den Standardnamen für die Verbindung in der Datei "Web. config" verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4af4-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="b4af4-145">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-145">Click **Next**.</span></span>

<span data-ttu-id="b4af4-146">Wählen Sie die neueste Version von Entity Framework aus.</span><span class="sxs-lookup"><span data-stu-id="b4af4-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="b4af4-147">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-147">Click **Next**.</span></span>

<span data-ttu-id="b4af4-148">Wählen Sie **Tabellen** aus, um Modelle für alle drei Tabellen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="b4af4-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="b4af4-149">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="b4af4-149">Click **Finish**.</span></span>

<span data-ttu-id="b4af4-150">Wenn Sie eine Sicherheitswarnung erhalten, klicken Sie auf **OK** , um die Ausführung der Vorlage fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="b4af4-151">Die Modelle werden aus den Datenbanktabellen generiert, und es wird ein Diagramm angezeigt, in dem die Eigenschaften und Beziehungen zwischen den Tabellen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b4af4-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Diagramm des Modells](creating-the-web-application/_static/image11.png)

<span data-ttu-id="b4af4-153">Der Ordner Models enthält jetzt viele neue Dateien im Zusammenhang mit den Modellen, die aus der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="b4af4-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="b4af4-154">Die **ContosoModel.Context.cs** -Datei enthält eine Klasse, die von der **dbcontext** -Klasse abgeleitet ist, und stellt eine-Eigenschaft für jede Modell Klasse bereit, die einer Datenbanktabelle entspricht.</span><span class="sxs-lookup"><span data-stu-id="b4af4-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="b4af4-155">Die **Course.cs**-, **Enrollment.cs**-und **Student.cs** -Dateien enthalten die Modellklassen, die die Datenbanktabellen darstellen.</span><span class="sxs-lookup"><span data-stu-id="b4af4-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="b4af4-156">Beim Arbeiten mit Gerüstbau werden sowohl die Kontext Klasse als auch die Modellklassen verwendet.</span><span class="sxs-lookup"><span data-stu-id="b4af4-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="b4af4-157">Bevor Sie mit diesem Tutorial fortfahren, erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="b4af4-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="b4af4-158">Im nächsten Abschnitt generieren Sie Code auf der Grundlage der Datenmodelle, aber dieser Abschnitt funktioniert nicht, wenn das Projekt nicht erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="b4af4-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4af4-159">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b4af4-159">Next steps</span></span>

<span data-ttu-id="b4af4-160">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="b4af4-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b4af4-161">Erstellen einer ASP.net-Web-App</span><span class="sxs-lookup"><span data-stu-id="b4af4-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="b4af4-162">Generierte Modelle</span><span class="sxs-lookup"><span data-stu-id="b4af4-162">Generated the models</span></span>

<span data-ttu-id="b4af4-163">Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Code generieren, der auf den Datenmodellen basiert.</span><span class="sxs-lookup"><span data-stu-id="b4af4-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b4af4-164">Erstellen von Sichten</span><span class="sxs-lookup"><span data-stu-id="b4af4-164">Generating views</span></span>](generating-views.md)
