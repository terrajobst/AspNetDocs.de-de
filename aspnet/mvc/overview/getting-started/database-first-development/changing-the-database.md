---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: Ändern der Datenbank für EF-Database First mit ASP.NET MVC-App'
description: Dieses Tutorial konzentriert sich auf die Aktualisierung der Datenbankstruktur und die Weitergabe dieser Änderung in der gesamten Webanwendung.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499521"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="b73ea-103">Tutorial: Ändern der Datenbank für EF-Database First mit ASP.NET MVC-App</span><span class="sxs-lookup"><span data-stu-id="b73ea-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="b73ea-104">Mithilfe von MVC, Entity Framework und ASP.net-Gerüstbau können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b73ea-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b73ea-105">In dieser tutorialreihe wird gezeigt, wie Sie automatisch Code generieren, mit dem Benutzerdaten in einer Datenbanktabelle anzeigen, bearbeiten, erstellen und löschen können.</span><span class="sxs-lookup"><span data-stu-id="b73ea-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b73ea-106">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="b73ea-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="b73ea-107">Dieses Tutorial konzentriert sich auf die Aktualisierung der Datenbankstruktur und die Weitergabe dieser Änderung in der gesamten Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="b73ea-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="b73ea-108">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="b73ea-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b73ea-109">Hinzufügen eines Spaltennamens</span><span class="sxs-lookup"><span data-stu-id="b73ea-109">Add a column</span></span>
> * <span data-ttu-id="b73ea-110">Hinzufügen der Eigenschaft zu den Ansichten</span><span class="sxs-lookup"><span data-stu-id="b73ea-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b73ea-111">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b73ea-111">Prerequisites</span></span>

* [<span data-ttu-id="b73ea-112">Erstellen von Sichten</span><span class="sxs-lookup"><span data-stu-id="b73ea-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="b73ea-113">Hinzufügen eines Spaltennamens</span><span class="sxs-lookup"><span data-stu-id="b73ea-113">Add a column</span></span>

<span data-ttu-id="b73ea-114">Wenn Sie die Struktur einer Tabelle in der Datenbank aktualisieren, müssen Sie sicherstellen, dass die Änderung an das Datenmodell, die Ansichten und den Controller weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b73ea-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="b73ea-115">In diesem Tutorial fügen Sie der Tabelle "Student" eine neue Spalte hinzu, um den Vornamen des Studenten aufzuzeichnen.</span><span class="sxs-lookup"><span data-stu-id="b73ea-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="b73ea-116">Um diese Spalte hinzuzufügen, öffnen Sie das Datenbankprojekt, und öffnen Sie die Datei Student. SQL.</span><span class="sxs-lookup"><span data-stu-id="b73ea-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="b73ea-117">Fügen Sie entweder über den Designer oder den T-SQL-Code eine Spalte mit dem Namen **MiddleName** ein, die ein nvarchar (50) ist, und lässt NULL-Werte zu.</span><span class="sxs-lookup"><span data-stu-id="b73ea-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="b73ea-118">Stellen Sie diese Änderung in der lokalen Datenbank bereit, indem Sie das Datenbankprojekt (oder F5) starten.</span><span class="sxs-lookup"><span data-stu-id="b73ea-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="b73ea-119">Das neue Feld wird der Tabelle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b73ea-119">The new field is added to the table.</span></span> <span data-ttu-id="b73ea-120">Wenn Sie im SQL Server-Objekt-Explorer nicht angezeigt wird, klicken Sie im Bereich auf die Schaltfläche Aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="b73ea-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![neue Spalte anzeigen](changing-the-database/_static/image2.png)

<span data-ttu-id="b73ea-122">Die neue Spalte ist in der Datenbanktabelle vorhanden, Sie ist jedoch zurzeit nicht in der Datenmodell Klasse vorhanden.</span><span class="sxs-lookup"><span data-stu-id="b73ea-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="b73ea-123">Sie müssen das Modell aktualisieren, um die neue Spalte hinzufügen zu können.</span><span class="sxs-lookup"><span data-stu-id="b73ea-123">You must update the model to include your new column.</span></span> <span data-ttu-id="b73ea-124">Öffnen Sie im Ordner **Models** die Datei " **\tosomodel. edmx** ", um das Modell Diagramm anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b73ea-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="b73ea-125">Beachten Sie, dass das Student-Modell nicht die MiddleName-Eigenschaft enthält.</span><span class="sxs-lookup"><span data-stu-id="b73ea-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="b73ea-126">Klicken Sie mit der rechten Maustaste auf eine beliebige Stelle auf der Entwurfs Oberfläche, und wählen Sie **Modell aus Datenbank aktualisieren aus**</span><span class="sxs-lookup"><span data-stu-id="b73ea-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="b73ea-127">Wählen Sie im Update-Assistenten die Registerkarte **Aktualisieren** aus, und wählen Sie dann **Tabellen** > **dbo** > **Student**aus.</span><span class="sxs-lookup"><span data-stu-id="b73ea-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="b73ea-128">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="b73ea-128">Click **Finish**.</span></span>

<span data-ttu-id="b73ea-129">Nachdem der Update Vorgang abgeschlossen ist, enthält das Daten Bank Diagramm die neue **MiddleName** -Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b73ea-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="b73ea-130">Speichern Sie die Datei " **\desomodel. edmx** ".</span><span class="sxs-lookup"><span data-stu-id="b73ea-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="b73ea-131">Sie müssen diese Datei speichern, damit die neue Eigenschaft an die **Student.cs** -Klasse weitergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b73ea-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="b73ea-132">Sie haben nun die Datenbank und das Modell aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="b73ea-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="b73ea-133">Erstellen Sie die Projektmappe.</span><span class="sxs-lookup"><span data-stu-id="b73ea-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="b73ea-134">Hinzufügen der Eigenschaft zu den Ansichten</span><span class="sxs-lookup"><span data-stu-id="b73ea-134">Add the property to the views</span></span>

<span data-ttu-id="b73ea-135">Leider enthalten die Ansichten noch nicht die neue Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b73ea-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="b73ea-136">Zum Aktualisieren der Ansichten stehen Ihnen zwei Optionen zur Verfügung: Sie können die Ansichten entweder erneut generieren, indem Sie erneut Gerüstbau für die Klasse "Student" hinzufügen, oder Sie können die neue Eigenschaft manuell zu Ihren vorhandenen Ansichten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b73ea-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="b73ea-137">In diesem Tutorial fügen Sie das Gerüst erneut hinzu, da Sie keine angepassten Änderungen an den automatisch generierten Sichten vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="b73ea-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="b73ea-138">Sie sollten die-Eigenschaft manuell hinzufügen, wenn Sie Änderungen an den Sichten vorgenommen haben und diese Änderungen nicht verlieren möchten.</span><span class="sxs-lookup"><span data-stu-id="b73ea-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="b73ea-139">Um sicherzustellen, dass die Ansichten neu erstellt werden, löschen Sie den Ordner **Students** unter **views**, und löschen Sie den **studentscontroller**.</span><span class="sxs-lookup"><span data-stu-id="b73ea-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="b73ea-140">Klicken Sie dann mit der rechten Maustaste auf den Ordner **Controllers** , und fügen Sie Gerüstbau für das **Student** Model</span><span class="sxs-lookup"><span data-stu-id="b73ea-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="b73ea-141">Nennen Sie den Controller " **studentscontroller**".</span><span class="sxs-lookup"><span data-stu-id="b73ea-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="b73ea-142">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="b73ea-142">Select **Add**.</span></span>

<span data-ttu-id="b73ea-143">Erstellen Sie die Projektmappe erneut.</span><span class="sxs-lookup"><span data-stu-id="b73ea-143">Build the solution again.</span></span> <span data-ttu-id="b73ea-144">Die Ansichten enthalten nun die MiddleName-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="b73ea-144">The views now contain the MiddleName property.</span></span>

![mittleren Namen anzeigen](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="b73ea-146">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b73ea-146">Next steps</span></span>

<span data-ttu-id="b73ea-147">In diesem Tutorial führen Sie Folgendes durch:</span><span class="sxs-lookup"><span data-stu-id="b73ea-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b73ea-148">Hinzugefügte Spalte</span><span class="sxs-lookup"><span data-stu-id="b73ea-148">Added a column</span></span>
> * <span data-ttu-id="b73ea-149">Die Eigenschaft wurde den Ansichten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b73ea-149">Added the property to the views</span></span>

<span data-ttu-id="b73ea-150">Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie die Ansicht zum Anzeigen von Details zu einem Student-Datensatz anpassen.</span><span class="sxs-lookup"><span data-stu-id="b73ea-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b73ea-151">Anpassen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="b73ea-151">Customize a view</span></span>](customizing-a-view.md)