---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortieren, Paging und Filtern von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441063"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="ef266-104">Sortieren, Paging und Filtern von Daten mit Modell Bindung und Web Forms</span><span class="sxs-lookup"><span data-stu-id="ef266-104">Sorting, paging, and filtering data with model binding and web forms</span></span>

<span data-ttu-id="ef266-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ef266-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ef266-106">In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="ef266-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ef266-107">Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="ef266-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ef266-108">Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.</span><span class="sxs-lookup"><span data-stu-id="ef266-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ef266-109">In diesem Tutorial wird gezeigt, wie Sie die Daten mithilfe der Modell Bindung sortieren, Paging und Filtern können.</span><span class="sxs-lookup"><span data-stu-id="ef266-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="ef266-110">Dieses Tutorial baut auf dem Projekt auf, das im ersten [Teil](retrieving-data.md) der Reihe erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ef266-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="ef266-111">Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) .</span><span class="sxs-lookup"><span data-stu-id="ef266-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ef266-112">Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ef266-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ef266-113">Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="ef266-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="ef266-114">Was Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="ef266-114">What you'll build</span></span>

<span data-ttu-id="ef266-115">In diesem Tutorial gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="ef266-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ef266-116">Sortieren und Paging der Daten aktivieren</span><span class="sxs-lookup"><span data-stu-id="ef266-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="ef266-117">Ermöglicht das Filtern der Daten basierend auf einer Auswahl durch den Benutzer.</span><span class="sxs-lookup"><span data-stu-id="ef266-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="ef266-118">Hinzufügen von Sortierung</span><span class="sxs-lookup"><span data-stu-id="ef266-118">Add sorting</span></span>

<span data-ttu-id="ef266-119">Das Aktivieren der Sortierung in der GridView ist sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="ef266-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="ef266-120">Legen Sie in der Datei "Student. aspx" in der GridView einfach " **allowsortier** " auf " **true** " fest.</span><span class="sxs-lookup"><span data-stu-id="ef266-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="ef266-121">Es ist nicht erforderlich, für jede Spalte einen **SortExpression** -Wert festzulegen, da die Daten aus automatisch verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ef266-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="ef266-122">Die GridView ändert die Abfrage so, dass Sie die Reihenfolge der Daten nach dem ausgewählten Wert einschließt.</span><span class="sxs-lookup"><span data-stu-id="ef266-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="ef266-123">Der hervorgehobene Code unten zeigt, was Sie zum Aktivieren der Sortierung vornehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="ef266-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="ef266-124">Führen Sie die Webanwendung aus, und testen Sie das Sortieren von Student-Datensätzen anhand der Werte in verschiedenen Spalten</span><span class="sxs-lookup"><span data-stu-id="ef266-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Studenten sortieren](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="ef266-126">Hinzufügen von Paging</span><span class="sxs-lookup"><span data-stu-id="ef266-126">Add paging</span></span>

<span data-ttu-id="ef266-127">Das Aktivieren von Paging ist ebenfalls sehr einfach.</span><span class="sxs-lookup"><span data-stu-id="ef266-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="ef266-128">Legen Sie in der GridView die **AllowPaging** -Eigenschaft auf **true** fest, und legen Sie die **PageSize** -Eigenschaft auf die Anzahl der Datensätze fest, die auf jeder Seite angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="ef266-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="ef266-129">In diesem Tutorial können Sie es auf 4 festlegen.</span><span class="sxs-lookup"><span data-stu-id="ef266-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="ef266-130">Führen Sie die-Webanwendung aus, und beachten Sie, dass die Datensätze nun auf mehrere Seiten aufgeteilt sind und höchstens vier Datensätze auf einer einzelnen Seite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ef266-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Paging hinzufügen](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="ef266-132">Durch die verzögerte Abfrage Ausführung wird die Effizienz der Anwendung verbessert.</span><span class="sxs-lookup"><span data-stu-id="ef266-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="ef266-133">Anstatt das gesamte Dataset abzurufen, ändert die GridView die Abfrage so, dass nur die Datensätze für die aktuelle Seite abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="ef266-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="ef266-134">Datensätze nach Benutzer Auswahl Filtern</span><span class="sxs-lookup"><span data-stu-id="ef266-134">Filter records by user selection</span></span>

<span data-ttu-id="ef266-135">Die Modell Bindung fügt mehrere Attribute hinzu, mit denen Sie festlegen können, wie der Wert für einen Parameter in einer Modell Bindungsmethode festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="ef266-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="ef266-136">Diese Attribute befinden sich im **System. Web. modelbinding** -Namespace.</span><span class="sxs-lookup"><span data-stu-id="ef266-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="ef266-137">Dazu zählen:</span><span class="sxs-lookup"><span data-stu-id="ef266-137">They include:</span></span>

- <span data-ttu-id="ef266-138">Control</span><span class="sxs-lookup"><span data-stu-id="ef266-138">Control</span></span>
- <span data-ttu-id="ef266-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="ef266-139">Cookie</span></span>
- <span data-ttu-id="ef266-140">Formular</span><span class="sxs-lookup"><span data-stu-id="ef266-140">Form</span></span>
- <span data-ttu-id="ef266-141">Profil</span><span class="sxs-lookup"><span data-stu-id="ef266-141">Profile</span></span>
- <span data-ttu-id="ef266-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="ef266-142">QueryString</span></span>
- <span data-ttu-id="ef266-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="ef266-143">RouteData</span></span>
- <span data-ttu-id="ef266-144">Sitzung</span><span class="sxs-lookup"><span data-stu-id="ef266-144">Session</span></span>
- <span data-ttu-id="ef266-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="ef266-145">UserProfile</span></span>
- <span data-ttu-id="ef266-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="ef266-146">ViewState</span></span>

<span data-ttu-id="ef266-147">In diesem Tutorial verwenden Sie den Wert eines Steuer Elements, um zu filtern, welche Datensätze in der GridView angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ef266-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="ef266-148">Sie fügen das **Control** -Attribut der Abfrage Methode hinzu, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="ef266-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="ef266-149">In einem [späteren](using-query-string-values-to-retrieve-data.md) Tutorial wenden Sie das Attribut **QueryString** auf einen Parameter an, um anzugeben, dass der Parameterwert aus dem Wert einer Abfrage Zeichenfolge stammt.</span><span class="sxs-lookup"><span data-stu-id="ef266-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="ef266-150">Fügen Sie zunächst oberhalb von ValidationSummary eine Dropdown Liste hinzu, um zu filtern, welche Studenten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ef266-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="ef266-151">Ändern Sie in der Code Behind-Datei die Select-Methode so, dass Sie einen Wert aus dem-Steuerelement empfängt, und legen Sie den Namen des-Parameters auf den Namen des Steuer Elements fest, das den Wert bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="ef266-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="ef266-152">Um das Steuerelement Attribut aufzulösen, müssen Sie eine **using** -Anweisung für den **System. Web. modelbinding** -Namespace hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ef266-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="ef266-153">Der folgende Code zeigt die ausgewählte Methode zum Filtern der zurückgegebenen Daten basierend auf dem Wert der Dropdown Liste.</span><span class="sxs-lookup"><span data-stu-id="ef266-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="ef266-154">Durch das Hinzufügen eines Steuerelement Attributs vor einem Parameter wird angegeben, dass der Wert für diesen Parameter von einem Steuerelement mit dem gleichen Namen stammt.</span><span class="sxs-lookup"><span data-stu-id="ef266-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="ef266-155">Führen Sie die Webanwendung aus, und wählen Sie in der Dropdown Liste verschiedene Werte aus, um die Liste der Studenten zu filtern.</span><span class="sxs-lookup"><span data-stu-id="ef266-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Schüler/Studenten Filtern](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="ef266-157">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ef266-157">Conclusion</span></span>

<span data-ttu-id="ef266-158">In diesem Tutorial haben Sie das Sortieren und Paging der Daten aktiviert.</span><span class="sxs-lookup"><span data-stu-id="ef266-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="ef266-159">Sie haben auch das Filtern der Daten nach dem Wert eines Steuer Elements aktiviert.</span><span class="sxs-lookup"><span data-stu-id="ef266-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="ef266-160">Im nächsten [Tutorial](integrating-jquery-ui.md) verbessern Sie die Benutzeroberfläche, indem Sie ein jQuery UI-Widget in die Vorlage für dynamische Daten integrieren.</span><span class="sxs-lookup"><span data-stu-id="ef266-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef266-161">[Zurück](updating-deleting-and-creating-data.md)
> [Weiter](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="ef266-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
