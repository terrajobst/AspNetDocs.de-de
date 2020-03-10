---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Verwenden von Abfrage Zeichenfolgen-Werten zum Filtern von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519081"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="4efd3-104">Verwenden von Abfrage Zeichenfolgen-Werten zum Filtern von Daten mit Modell Bindung und Web Forms</span><span class="sxs-lookup"><span data-stu-id="4efd3-104">Using query string values to filter data with model binding and web forms</span></span>

<span data-ttu-id="4efd3-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4efd3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4efd3-106">In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="4efd3-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4efd3-107">Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="4efd3-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4efd3-108">Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.</span><span class="sxs-lookup"><span data-stu-id="4efd3-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4efd3-109">In diesem Tutorial wird gezeigt, wie ein Wert in der Abfrage Zeichenfolge übergeben wird und wie dieser Wert zum Abrufen von Daten über die Modell Bindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4efd3-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="4efd3-110">Dieses Tutorial baut auf dem Projekt auf, das in den [vorherigen](retrieving-data.md) Teilen der Reihe erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="4efd3-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="4efd3-111">Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) .</span><span class="sxs-lookup"><span data-stu-id="4efd3-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4efd3-112">Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4efd3-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4efd3-113">Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="4efd3-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="4efd3-114">Was Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="4efd3-114">What you'll build</span></span>

<span data-ttu-id="4efd3-115">In diesem Tutorial gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="4efd3-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4efd3-116">Fügen Sie eine neue Seite hinzu, um die registrierten Kurse für einen Schüler/Student anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4efd3-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="4efd3-117">Registrierte Kurse für den ausgewählten Studenten basierend auf einem Wert in der Abfrage Zeichenfolge abrufen</span><span class="sxs-lookup"><span data-stu-id="4efd3-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="4efd3-118">Fügen Sie der neuen Seite einen Hyperlink mit einem Abfrage Zeichen folgen Wert aus der Rasteransicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="4efd3-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="4efd3-119">Die Schritte in diesem Tutorial ähneln denen im vorherigen [Tutorial](sorting-paging-and-filtering-data.md) zum Filtern der angezeigten Studenten basierend auf der Benutzer Auswahl in einer Dropdown Liste.</span><span class="sxs-lookup"><span data-stu-id="4efd3-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="4efd3-120">In diesem Tutorial haben Sie das **Control** -Attribut in der Select-Methode verwendet, um anzugeben, dass der Parameterwert von einem-Steuerelement stammt.</span><span class="sxs-lookup"><span data-stu-id="4efd3-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="4efd3-121">In diesem Tutorial verwenden Sie das Attribut **QueryString** in der Select-Methode, um anzugeben, dass der Parameterwert aus der Abfrage Zeichenfolge stammt.</span><span class="sxs-lookup"><span data-stu-id="4efd3-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="4efd3-122">Neue Seite zum Anzeigen der Kurse eines Studenten hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4efd3-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="4efd3-123">Fügen Sie ein neues Webformular hinzu, das die Master Seite Site. Master verwendet, und benennen Sie die Seiten **Kurse**.</span><span class="sxs-lookup"><span data-stu-id="4efd3-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="4efd3-124">Fügen Sie in der Datei " **Courses. aspx** " eine Rasteransicht hinzu, um die Kurse für den ausgewählten Studenten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4efd3-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="4efd3-125">Definieren der Select-Methode</span><span class="sxs-lookup"><span data-stu-id="4efd3-125">Define the select method</span></span>

<span data-ttu-id="4efd3-126">In **Courses.aspx.cs**fügen Sie die Select-Methode mit dem Namen hinzu, den Sie in der **SelectMethod** -Eigenschaft der Rasteransicht angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="4efd3-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="4efd3-127">In dieser Methode definieren Sie die Abfrage zum Abrufen der Kurse eines Studenten und geben an, dass der Parameter von einem Abfrage Zeichen folgen Wert mit dem gleichen Namen wie der Parameter stammt.</span><span class="sxs-lookup"><span data-stu-id="4efd3-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="4efd3-128">Zuerst müssen Sie die folgenden **using** -Anweisungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4efd3-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="4efd3-129">Fügen Sie dann Courses.aspx.cs den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="4efd3-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="4efd3-130">Das QueryString-Attribut bedeutet, dass der-Parameter in dieser Methode automatisch ein Abfrage Zeichen folgen Wert mit dem Namen "StudentID" zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="4efd3-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="4efd3-131">Hyperlink mit Abfrage Zeichen folgen Wert hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4efd3-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="4efd3-132">In der Rasteransicht auf "students. aspx" fügen Sie ein Hyperlink-Feld hinzu, das mit der neuen Seite "Kurse" verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="4efd3-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="4efd3-133">Der Hyperlink enthält einen Abfrage Zeichen folgen Wert mit der ID des Studenten.</span><span class="sxs-lookup"><span data-stu-id="4efd3-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="4efd3-134">Fügen Sie in students. aspx das folgende Feld zu den Spalten der Rasteransicht direkt unterhalb des Felds für das Gesamtguthaben hinzu.</span><span class="sxs-lookup"><span data-stu-id="4efd3-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="4efd3-135">Führen Sie die Anwendung aus, und beachten Sie, dass die Rasteransicht jetzt den Link "Kurse" enthält.</span><span class="sxs-lookup"><span data-stu-id="4efd3-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Hyperlink hinzufügen](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="4efd3-137">Wenn Sie auf einen der Links klicken, sehen Sie die angemeldeten Kurse des Studenten.</span><span class="sxs-lookup"><span data-stu-id="4efd3-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Kurse anzeigen](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="4efd3-139">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="4efd3-139">Conclusion</span></span>

<span data-ttu-id="4efd3-140">In diesem Tutorial haben Sie einen Link mit einem Abfrage Zeichen folgen Wert hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4efd3-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="4efd3-141">Sie haben diesen Abfrage Zeichen folgen Wert für den Parameterwert in der Select-Methode verwendet.</span><span class="sxs-lookup"><span data-stu-id="4efd3-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="4efd3-142">Im nächsten [Tutorial](adding-business-logic-layer.md)verschieben Sie den Code aus den Code-Behind-Dateien in eine Geschäftslogik Schicht und eine Datenzugriffs Schicht.</span><span class="sxs-lookup"><span data-stu-id="4efd3-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4efd3-143">[Zurück](integrating-jquery-ui.md)
> [Weiter](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="4efd3-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
