---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Hinzufügen einer Geschäftslogik Ebene zu einem Projekt, das Modell Bindung und Web Forms verwendet | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515427"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="e7c2f-104">Hinzufügen einer Geschäftslogik Ebene zu einem Projekt, das Modell Bindung und Web Forms verwendet</span><span class="sxs-lookup"><span data-stu-id="e7c2f-104">Adding business logic layer to a project that uses model binding and web forms</span></span>

<span data-ttu-id="e7c2f-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e7c2f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e7c2f-106">In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e7c2f-107">Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="e7c2f-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e7c2f-108">Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e7c2f-109">In diesem Tutorial wird gezeigt, wie die Modell Bindung mit einer Geschäftslogik Schicht verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="e7c2f-110">Sie legen den oncallingdatamethods-Member fest, um anzugeben, dass ein anderes Objekt als die aktuelle Seite verwendet wird, um die Daten Methoden aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="e7c2f-111">Dieses Tutorial baut auf dem Projekt auf, das in den [vorherigen](retrieving-data.md) Teilen der Reihe erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="e7c2f-112">Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) .</span><span class="sxs-lookup"><span data-stu-id="e7c2f-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e7c2f-113">Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e7c2f-114">Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="e7c2f-115">Was Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="e7c2f-115">What you'll build</span></span>

<span data-ttu-id="e7c2f-116">Mithilfe der Modell Bindung können Sie Ihren Daten Interaktions Code entweder in der Code Behind-Datei für eine Webseite oder in einer separaten Geschäftslogik Klasse platzieren.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="e7c2f-117">In den vorherigen Tutorials wurde gezeigt, wie die Code-Behind-Dateien für den Daten Interaktions Code verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="e7c2f-118">Diese Vorgehensweise funktioniert bei kleinen Standorten, kann jedoch zu einer Code Wiederholung und einer größeren Schwierigkeit bei der Verwaltung einer großen Website führen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="e7c2f-119">Es kann auch sehr schwierig sein, Code Programm gesteuert zu testen, der sich in Code Behind-Dateien befindet, da keine Abstraktions Ebene vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="e7c2f-120">Um den Daten Interaktions Code zu zentralisieren, können Sie eine Geschäftslogik Ebene erstellen, die die gesamte Logik für die Interaktion mit Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="e7c2f-121">Anschließend wird die Geschäftslogik Schicht auf Ihren Webseiten aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="e7c2f-122">In diesem Tutorial wird gezeigt, wie Sie den gesamten Code, den Sie in den vorherigen Tutorials geschrieben haben, in eine Geschäftslogik Schicht verschieben und diesen Code dann auf den Seiten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="e7c2f-123">In diesem Tutorial gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="e7c2f-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e7c2f-124">Verschieben Sie den Code aus Code-Behind-Dateien in eine Geschäftslogik Ebene.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="e7c2f-125">Ändern Sie die Daten gebundenen Steuerelemente, um die Methoden in der Geschäftslogik Schicht aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="e7c2f-126">Geschäftslogik Ebene erstellen</span><span class="sxs-lookup"><span data-stu-id="e7c2f-126">Create business logic layer</span></span>

<span data-ttu-id="e7c2f-127">Nun erstellen Sie die-Klasse, die von den Webseiten aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="e7c2f-128">Die Methoden in dieser Klasse ähneln den Methoden, die Sie in den vorherigen Tutorials verwendet haben, und enthalten die Wert Anbieter Attribute.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="e7c2f-129">Fügen Sie zunächst einen neuen Ordner namens **BLL**hinzu.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-129">First, add a new folder called **BLL**.</span></span>

![Ordner hinzufügen](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="e7c2f-131">Erstellen Sie im Ordner BLL eine neue Klasse mit dem Namen **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="e7c2f-132">Sie enthält alle Daten Vorgänge, deren Größe sich in Code Behind-Dateien ursprünglich geändert hat.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="e7c2f-133">Die Methoden sind fast identisch mit den Methoden in der Code-Behind-Datei, enthalten jedoch einige Änderungen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="e7c2f-134">Die wichtigste Änderung ist, dass Sie den Code nicht mehr in einer Instanz der **Page** -Klasse ausführen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="e7c2f-135">Die Page-Klasse enthält die **tryupdatemodel** -Methode und die **modelstate** -Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="e7c2f-136">Wenn dieser Code in eine Geschäftslogik Schicht verschoben wird, verfügen Sie nicht mehr über eine Instanz der Page-Klasse, um diese Member aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="e7c2f-137">Um dieses Problem zu umgehen, müssen Sie jeder Methode, die auf tryupdatemodel oder modelstate zugreift, einen **modelmethodcontext** -Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="e7c2f-138">Sie verwenden diesen modelmethodcontext-Parameter zum Aufrufen von tryupdatemodel oder zum Abrufen von modelstate.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="e7c2f-139">Sie müssen nichts auf der Webseite ändern, um diesen neuen Parameter zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="e7c2f-140">Ersetzen Sie den Code in SchoolBL.cs durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="e7c2f-141">Überarbeiten vorhandener Seiten zum Abrufen von Daten aus der Geschäftslogik Schicht</span><span class="sxs-lookup"><span data-stu-id="e7c2f-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="e7c2f-142">Schließlich konvertieren Sie die Seiten "students. aspx", "addstudent. aspx" und "Courses. aspx" aus der Verwendung von Abfragen in der Code-Behind-Datei in die Geschäftslogik Schicht.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="e7c2f-143">Löschen Sie in den Code Behind-Dateien für Studenten, addstudent und Kurse die folgenden Abfrage Methoden, oder kommentieren Sie Sie aus:</span><span class="sxs-lookup"><span data-stu-id="e7c2f-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="e7c2f-144">studentsgrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="e7c2f-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="e7c2f-145">studentsgrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="e7c2f-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="e7c2f-146">studentsgrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="e7c2f-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="e7c2f-147">addstudentform\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="e7c2f-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="e7c2f-148">coursesgrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="e7c2f-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="e7c2f-149">Sie sollten jetzt keinen Code in der Code Behind-Datei haben, die sich auf Daten Vorgänge bezieht.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="e7c2f-150">Der **oncallingdatamethods** -Ereignishandler ermöglicht es Ihnen, ein Objekt anzugeben, das für die Daten Methoden verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="e7c2f-151">Fügen Sie in students. aspx einen Wert für diesen Ereignishandler hinzu, und ändern Sie die Namen der Daten Methoden in die Namen der Methoden in der Geschäftslogik Klasse.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="e7c2f-152">Definieren Sie in der Code Behind-Datei für students. aspx den Ereignishandler für das Ereignis callingdatamethods.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="e7c2f-153">In diesem Ereignishandler geben Sie die Geschäftslogik Klasse für Daten Vorgänge an.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="e7c2f-154">Nehmen Sie in addstudent. aspx ähnliche Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="e7c2f-155">Nehmen Sie in "Courses. aspx" ähnliche Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="e7c2f-156">Führen Sie die Anwendung aus, und beachten Sie, dass alle Seiten wie zuvor funktionieren.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="e7c2f-157">Die Validierungs Logik funktioniert ebenfalls ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e7c2f-158">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="e7c2f-158">Conclusion</span></span>

<span data-ttu-id="e7c2f-159">In diesem Tutorial haben Sie die Anwendung so strukturiert, dass Sie eine Datenzugriffs Schicht und eine Geschäftslogik Schicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="e7c2f-160">Sie haben angegeben, dass die Daten Steuerelemente ein Objekt verwenden, das nicht die aktuelle Seite für Daten Vorgänge ist.</span><span class="sxs-lookup"><span data-stu-id="e7c2f-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e7c2f-161">Previous</span><span class="sxs-lookup"><span data-stu-id="e7c2f-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
