---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Verwenden des Entity Framework 4,0 und des ObjectDataSource-Steuer Elements, Teil 3: Sortieren und Filtern | Microsoft-Dokumentation'
author: tdykstra
description: Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte mit Entity Framework 4,0 erstellt wurde. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512715"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="7e514-104">Verwenden des Entity Framework 4,0 und des ObjectDataSource-Steuer Elements, Teil 3: Sortieren und Filtern</span><span class="sxs-lookup"><span data-stu-id="7e514-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>

<span data-ttu-id="7e514-105">von [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7e514-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="7e514-106">Diese tutorialreihe basiert auf der Webanwendung der Website von "Web", die in der tutorialreihe für die ersten Schritte [mit Entity Framework 4,0](https://asp.net/entity-framework/tutorials#Getting%20Started) erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="7e514-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="7e514-107">Wenn Sie die vorherigen Tutorials nicht durchgearbeitet haben, können Sie die Anwendung, die Sie erstellt haben, als Ausgangspunkt für dieses Tutorial [herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) .</span><span class="sxs-lookup"><span data-stu-id="7e514-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="7e514-108">Sie können auch [die Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die von der kompletten tutorialreihe erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7e514-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="7e514-109">Wenn Sie Fragen zu den Tutorials haben, können Sie Sie im ASP.net- [Entity Framework Forum](https://forums.asp.net/1227.aspx)Posten.</span><span class="sxs-lookup"><span data-stu-id="7e514-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>

<span data-ttu-id="7e514-110">Im vorherigen Tutorial haben Sie das Repository-Muster in einer n-Tier-Webanwendung implementiert, die die Entity Framework und das `ObjectDataSource`-Steuerelement verwendet.</span><span class="sxs-lookup"><span data-stu-id="7e514-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="7e514-111">In diesem Tutorial wird gezeigt, wie Sie das Sortieren und Filtern durchführen und Master/Detail-Szenarien verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="7e514-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="7e514-112">Die folgenden Erweiterungen werden der Seite " *Departments. aspx* " hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="7e514-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="7e514-113">Ein Textfeld, in dem Benutzer Abteilungen nach Namen auswählen können.</span><span class="sxs-lookup"><span data-stu-id="7e514-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="7e514-114">Eine Liste der Kurse für jede Abteilung, die im Raster angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="7e514-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="7e514-115">Die Sortier Fähigkeit durch Klicken auf Spaltenüberschriften.</span><span class="sxs-lookup"><span data-stu-id="7e514-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="7e514-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7e514-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="7e514-117">Hinzufügen der Möglichkeit zum Sortieren von GridView-Spalten</span><span class="sxs-lookup"><span data-stu-id="7e514-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="7e514-118">Öffnen Sie die Seite " *Departments. aspx* ", und fügen Sie dem `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`ein `SortParameterName="sortExpression"` Attribut hinzu.</span><span class="sxs-lookup"><span data-stu-id="7e514-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="7e514-119">(Später erstellen Sie eine `GetDepartments`-Methode, die einen Parameter mit dem Namen "`sortExpression`" annimmt.) Das Markup für das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="7e514-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="7e514-120">Fügen Sie das `AllowSorting="true"`-Attribut zum öffnenden Tag des `GridView` Steuer Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="7e514-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="7e514-121">Das Markup für das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="7e514-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="7e514-122">Legen Sie in *Departments.aspx.cs*die Standard Sortierreihenfolge fest, indem Sie die `Sort` Methode des `GridView`-Steuer Elements von der `Page_Load`-Methode aufrufen:</span><span class="sxs-lookup"><span data-stu-id="7e514-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="7e514-123">Sie können Code hinzufügen, der die Geschäftslogik Klasse oder die Repository-Klasse sortiert oder filtert.</span><span class="sxs-lookup"><span data-stu-id="7e514-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="7e514-124">Wenn Sie in der Geschäftslogik Klasse Vorgehen, wird die Sortierung oder Filterung durchgeführt, nachdem die Daten aus der Datenbank abgerufen wurden, da die Geschäftslogik Klasse mit einem vom Repository zurückgegebenen `IEnumerable` Objekt arbeitet.</span><span class="sxs-lookup"><span data-stu-id="7e514-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="7e514-125">Wenn Sie Sortier-und Filterungs Code in der Repository-Klasse hinzufügen und diesen Vorgang vor dem Konvertieren eines LINQ-Ausdrucks oder einer Objekt Abfrage in ein `IEnumerable` Objekt ausführen, werden die Befehle zur Verarbeitung an die Datenbank weitergegeben, was in der Regel effizienter ist.</span><span class="sxs-lookup"><span data-stu-id="7e514-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="7e514-126">In diesem Tutorial implementieren Sie das Sortieren und Filtern auf eine Weise, die bewirkt, dass die Verarbeitung von der Datenbank ausgeführt wird, d. –. im Repository.</span><span class="sxs-lookup"><span data-stu-id="7e514-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="7e514-127">Zum Hinzufügen der Sortierfunktion müssen Sie der Repository-Schnittstelle und den Repository-Klassen sowie der Geschäftslogik Klasse eine neue Methode hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7e514-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="7e514-128">Fügen Sie in der Datei *ISchoolRepository.cs* eine neue `GetDepartments`-Methode hinzu, die einen `sortExpression` Parameter annimmt, der zum Sortieren der Liste der zurückgegebenen Abteilungen verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="7e514-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="7e514-129">Der `sortExpression`-Parameter gibt die Spalte an, nach der sortiert werden soll, und die Sortierrichtung.</span><span class="sxs-lookup"><span data-stu-id="7e514-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="7e514-130">Fügen Sie der Datei *SchoolRepository.cs* Code für die neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="7e514-131">Ändern Sie die vorhandene Parameter lose `GetDepartments`-Methode, um die neue Methode aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="7e514-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="7e514-132">Fügen Sie im Testprojekt die folgende neue Methode zu *MockSchoolRepository.cs*hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="7e514-133">Wenn Sie Komponententests erstellen, die von dieser Methode abhängen, die eine sortierte Liste zurückgibt, müssen Sie die Liste vor der Rückgabe sortieren.</span><span class="sxs-lookup"><span data-stu-id="7e514-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="7e514-134">In diesem Tutorial erstellen Sie keine Tests, sodass die Methode nur die unsortierte Liste der Abteilungen zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="7e514-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="7e514-135">Fügen Sie in der Datei *SchoolBL.cs* der Geschäftslogik Klasse die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="7e514-136">Dieser Code übergibt den Sort-Parameter an die Repository-Methode.</span><span class="sxs-lookup"><span data-stu-id="7e514-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="7e514-137">Führen Sie die Seite " *Departments. aspx* " aus.</span><span class="sxs-lookup"><span data-stu-id="7e514-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="7e514-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7e514-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="7e514-139">Sie können jetzt auf eine beliebige Spaltenüberschrift klicken, um nach dieser Spalte zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="7e514-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="7e514-140">Wenn die Spalte bereits sortiert ist, wird durch Klicken auf die Überschrift die Sortierreihenfolge umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="7e514-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="7e514-141">Hinzufügen eines Suchfelds</span><span class="sxs-lookup"><span data-stu-id="7e514-141">Adding a Search Box</span></span>

<span data-ttu-id="7e514-142">In diesem Abschnitt fügen Sie ein Such Textfeld hinzu, verknüpfen es mit dem `ObjectDataSource`-Steuerelement mithilfe eines Control-Parameters und fügen der Geschäftslogik Klasse eine Methode hinzu, um das Filtern zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7e514-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="7e514-143">Öffnen Sie die Seite " *Departments. aspx* ", und fügen Sie das folgende Markup zwischen der Überschrift und dem ersten `ObjectDataSource` Steuerelement hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="7e514-144">Gehen Sie im `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="7e514-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="7e514-145">Fügen Sie ein `SelectParameters`-Element für einen Parameter mit dem Namen `nameSearchString` hinzu, der den im `SearchTextBox`-Steuerelement eingegebenen Wert abruft.</span><span class="sxs-lookup"><span data-stu-id="7e514-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="7e514-146">Ändern Sie den Wert des `SelectMethod` Attributs in `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="7e514-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="7e514-147">(Sie erstellen diese Methode später.)</span><span class="sxs-lookup"><span data-stu-id="7e514-147">(You'll create this method later.)</span></span>

<span data-ttu-id="7e514-148">Das Markup für das `ObjectDataSource`-Steuerelement ähnelt nun dem folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7e514-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="7e514-149">Fügen Sie in *ISchoolRepository.cs*eine `GetDepartmentsByName` Methode hinzu, die sowohl `sortExpression` als auch `nameSearchString` Parameter annimmt:</span><span class="sxs-lookup"><span data-stu-id="7e514-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="7e514-150">Fügen Sie in *SchoolRepository.cs*die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="7e514-151">In diesem Code wird eine `Where`-Methode verwendet, um Elemente auszuwählen, die die Such Zeichenfolge enthalten.</span><span class="sxs-lookup"><span data-stu-id="7e514-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="7e514-152">Wenn die Such Zeichenfolge leer ist, werden alle Datensätze ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="7e514-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="7e514-153">Beachten Sie Folgendes: Wenn Sie Methodenaufrufe in einer Anweisung wie diesem angeben (`Include`und dann `OrderBy`, dann `Where`), muss die `Where` Methode immer "Last" lauten.</span><span class="sxs-lookup"><span data-stu-id="7e514-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="7e514-154">Ändern Sie die vorhandene `GetDepartments`-Methode, die einen `sortExpression`-Parameter annimmt, um die neue Methode aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="7e514-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="7e514-155">Fügen Sie in *MockSchoolRepository.cs* im Testprojekt die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="7e514-156">Fügen Sie in *SchoolBL.cs*die folgende neue Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="7e514-157">Führen Sie die Seite *Departments. aspx* aus, und geben Sie eine Such Zeichenfolge ein, um sicherzustellen, dass die Auswahl Logik funktioniert</span><span class="sxs-lookup"><span data-stu-id="7e514-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="7e514-158">Lassen Sie das Textfeld leer, und versuchen Sie eine Suche, um sicherzustellen, dass alle Datensätze zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7e514-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="7e514-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7e514-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="7e514-160">Hinzufügen einer Detail Spalte für jede Raster Zeile</span><span class="sxs-lookup"><span data-stu-id="7e514-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="7e514-161">Als nächstes möchten Sie alle Kurse der einzelnen Abteilungen sehen, die in der rechten Zelle des Rasters angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7e514-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="7e514-162">Zu diesem Zweck verwenden Sie ein geändertes `GridView`-Steuerelement und verbinden es mit Daten aus der `Courses`-Navigations Eigenschaft der `Department`-Entität.</span><span class="sxs-lookup"><span data-stu-id="7e514-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="7e514-163">Öffnen Sie " *Departments. aspx* ", und geben Sie im Markup für das `GridView` Steuerelement einen Handler für das `RowDataBound` Ereignis an.</span><span class="sxs-lookup"><span data-stu-id="7e514-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="7e514-164">Das Markup für das öffnende Tag des-Steuer Elements ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="7e514-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="7e514-165">Fügen Sie ein neues `TemplateField`-Element nach dem `Administrator` Vorlagen Feld hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="7e514-166">Dieses Markup erstellt ein geschsted `GridView` Steuerelement, das die Kursnummer und den Titel einer Liste von Kursen anzeigt.</span><span class="sxs-lookup"><span data-stu-id="7e514-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="7e514-167">Es wird keine Datenquelle angegeben, da Sie Sie im Code in den `RowDataBound`-Handler übernehmen.</span><span class="sxs-lookup"><span data-stu-id="7e514-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="7e514-168">Öffnen Sie *Departments.aspx.cs* , und fügen Sie den folgenden Handler für das `RowDataBound`-Ereignis hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e514-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="7e514-169">Dieser Code Ruft die `Department`-Entität aus den Ereignis Argumenten ab, konvertiert die `Courses`-Navigations Eigenschaft in eine `List`-Auflistung und stellt der der-Auflistung hinzugefügten `GridView`.</span><span class="sxs-lookup"><span data-stu-id="7e514-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="7e514-170">Öffnen Sie die Datei *SchoolRepository.cs* , und geben Sie Eager Loading für die `Courses` Navigations Eigenschaft an, indem Sie die `Include`-Methode in der Objekt Abfrage aufrufen, die Sie in der `GetDepartmentsByName`-Methode erstellen.</span><span class="sxs-lookup"><span data-stu-id="7e514-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="7e514-171">Die `return`-Anweisung in der `GetDepartmentsByName`-Methode ähnelt nun dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="7e514-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="7e514-172">Führen Sie die Seite aus.</span><span class="sxs-lookup"><span data-stu-id="7e514-172">Run the page.</span></span> <span data-ttu-id="7e514-173">Zusätzlich zu den Sortier-und Filterfunktionen, die Sie zuvor hinzugefügt haben, zeigt das GridView-Steuerelement jetzt geschamelte Kursdetails für jede Abteilung an.</span><span class="sxs-lookup"><span data-stu-id="7e514-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="7e514-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7e514-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="7e514-175">Dadurch wird die Einführung in Sortier-, Filter-und Master/Detail-Szenarien vervollständigt.</span><span class="sxs-lookup"><span data-stu-id="7e514-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="7e514-176">Im nächsten Tutorial erfahren Sie, wie Sie Parallelität verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="7e514-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e514-177">[Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="7e514-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
