---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualisieren, löschen und Erstellen von Daten mit Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474135"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="40f9e-104">Aktualisieren, löschen und Erstellen von Daten mit Modell Bindung und Web Forms</span><span class="sxs-lookup"><span data-stu-id="40f9e-104">Updating, deleting, and creating data with model binding and web forms</span></span>

<span data-ttu-id="40f9e-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="40f9e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="40f9e-106">In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="40f9e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="40f9e-107">Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="40f9e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="40f9e-108">Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.</span><span class="sxs-lookup"><span data-stu-id="40f9e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="40f9e-109">In diesem Tutorial wird gezeigt, wie Sie Daten mit Modell Bindung erstellen, aktualisieren und löschen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="40f9e-110">Legen Sie die folgenden Eigenschaften fest:</span><span class="sxs-lookup"><span data-stu-id="40f9e-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="40f9e-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="40f9e-111">DeleteMethod</span></span>
> - <span data-ttu-id="40f9e-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="40f9e-112">InsertMethod</span></span>
> - <span data-ttu-id="40f9e-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="40f9e-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="40f9e-114">Diese Eigenschaften erhalten den Namen der Methode, die den entsprechenden Vorgang behandelt.</span><span class="sxs-lookup"><span data-stu-id="40f9e-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="40f9e-115">Innerhalb dieser Methode stellen Sie die Logik zum interagieren mit den Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="40f9e-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="40f9e-116">Dieses Tutorial baut auf dem Projekt auf, das im ersten [Teil](retrieving-data.md) der Reihe erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="40f9e-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="40f9e-117">Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) .</span><span class="sxs-lookup"><span data-stu-id="40f9e-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="40f9e-118">Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="40f9e-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="40f9e-119">Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="40f9e-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="40f9e-120">Was Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="40f9e-120">What you'll build</span></span>

<span data-ttu-id="40f9e-121">In diesem Tutorial gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="40f9e-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="40f9e-122">Hinzufügen dynamischer Datenvorlagen</span><span class="sxs-lookup"><span data-stu-id="40f9e-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="40f9e-123">Aktivieren des Aktualisierens und Löschens von Daten durch Modell Bindungsmethoden</span><span class="sxs-lookup"><span data-stu-id="40f9e-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="40f9e-124">Anwenden von Daten Validierungsregeln: Aktivieren Sie das Erstellen eines neuen Datensatzes in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="40f9e-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="40f9e-125">Hinzufügen dynamischer Datenvorlagen</span><span class="sxs-lookup"><span data-stu-id="40f9e-125">Add dynamic data templates</span></span>

<span data-ttu-id="40f9e-126">Zum Bereitstellen der optimalen Benutzer Leistung und Minimieren der Code Wiederholung verwenden Sie dynamische Datenvorlagen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="40f9e-127">Sie können vorgefertigte dynamische Datenvorlagen problemlos in Ihre vorhandene Site integrieren, indem Sie ein nuget-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="40f9e-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="40f9e-128">Installieren Sie die **dynamicdatatemplatescs**über die **nuget-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="40f9e-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![dynamische Datenvorlagen](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="40f9e-130">Beachten Sie, dass Ihr Projekt jetzt einen Ordner mit dem Namen " **DynamicData**" enthält.</span><span class="sxs-lookup"><span data-stu-id="40f9e-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="40f9e-131">In diesem Ordner finden Sie die Vorlagen, die automatisch auf dynamische Steuerelemente in ihren Webformularen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="40f9e-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![Ordner für dynamische Daten](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="40f9e-133">Aktualisieren und löschen aktivieren</span><span class="sxs-lookup"><span data-stu-id="40f9e-133">Enable updating and deleting</span></span>

<span data-ttu-id="40f9e-134">Das Aktualisieren und Löschen von Datensätzen in der Datenbank durch Benutzer ist dem Prozess zum Abrufen von Daten sehr ähnlich.</span><span class="sxs-lookup"><span data-stu-id="40f9e-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="40f9e-135">In den Eigenschaften **UpdateMethod** und **DeleteMethod** geben Sie die Namen der Methoden an, die diese Vorgänge ausführen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="40f9e-136">Mit einem GridView-Steuerelement können Sie auch die automatische Generierung von Schaltflächen zum Bearbeiten und Löschen angeben.</span><span class="sxs-lookup"><span data-stu-id="40f9e-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="40f9e-137">Der folgende markierte Code zeigt die Ergänzungen zum GridView-Code.</span><span class="sxs-lookup"><span data-stu-id="40f9e-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="40f9e-138">Fügen Sie in der Code Behind-Datei eine using-Anweisung für **System. Data. Entity. Infrastructure**hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f9e-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="40f9e-139">Fügen Sie dann die folgenden Aktualisierungs-und Löschmethoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f9e-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="40f9e-140">Die **tryupdatemodel** -Methode wendet die übereinstimmenden Daten gebundenen Werte aus dem Webformular auf das Datenelement an.</span><span class="sxs-lookup"><span data-stu-id="40f9e-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="40f9e-141">Das Datenelement wird basierend auf dem Wert des ID-Parameters abgerufen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="40f9e-142">Validierungsanforderungen erzwingen</span><span class="sxs-lookup"><span data-stu-id="40f9e-142">Enforce validation requirements</span></span>

<span data-ttu-id="40f9e-143">Die Validierungs Attribute, die Sie auf die Eigenschaften "FirstName", "LastName" und "Year" in der Klasse "Student" angewendet haben, werden beim Aktualisieren der Daten automatisch erzwungen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="40f9e-144">Die DynamicField-Steuerelemente fügen Client-und Server Validierungs Steuerelemente basierend auf den Validierungs Attributen hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f9e-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="40f9e-145">Die Eigenschaften FirstName und LastName sind beide erforderlich.</span><span class="sxs-lookup"><span data-stu-id="40f9e-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="40f9e-146">FirstName darf nicht länger als 20 Zeichen sein, und LastName darf nicht länger als 40 Zeichen sein.</span><span class="sxs-lookup"><span data-stu-id="40f9e-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="40f9e-147">Year muss ein gültiger Wert für die "akadecyear"-Enumeration sein.</span><span class="sxs-lookup"><span data-stu-id="40f9e-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="40f9e-148">Wenn der Benutzer gegen eine der Überprüfungsanforderungen verstößt, wird das Update nicht fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="40f9e-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="40f9e-149">Fügen Sie über der GridView ein ValidationSummary-Steuerelement hinzu, um die Fehlermeldung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="40f9e-150">Legen Sie die Eigenschaft **showmodelstateerrors** auf **true**fest, um die Validierungs Fehler aus der Modell Bindung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="40f9e-151">Führen Sie die-Webanwendung aus, und aktualisieren und löschen Sie alle Datensätze.</span><span class="sxs-lookup"><span data-stu-id="40f9e-151">Run the web application, and update and delete any of the records.</span></span>

![Aktualisieren von Daten](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="40f9e-153">Beachten Sie, dass der Wert für die Year-Eigenschaft im Bearbeitungsmodus automatisch als Dropdown Liste gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="40f9e-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="40f9e-154">Die Year-Eigenschaft ist ein Enumerationswert, und die Vorlage für dynamische Daten für einen Enumerationswert gibt eine Dropdown Liste zum Bearbeiten an.</span><span class="sxs-lookup"><span data-stu-id="40f9e-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="40f9e-155">Sie finden diese Vorlage, indem Sie die- **Enumeration\_Datei "Edit. ascx** " im Ordner " **DynamicData**/**FieldTemplates** " öffnen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="40f9e-156">Wenn Sie gültige Werte angeben, wird das Update erfolgreich abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="40f9e-157">Wenn Sie gegen eine der Überprüfungsanforderungen verstoßen, wird das Update nicht fortgesetzt, und es wird eine Fehlermeldung oberhalb des Rasters angezeigt.</span><span class="sxs-lookup"><span data-stu-id="40f9e-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Fehlermeldung](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="40f9e-159">Neue Datensätze hinzufügen</span><span class="sxs-lookup"><span data-stu-id="40f9e-159">Add new records</span></span>

<span data-ttu-id="40f9e-160">Das GridView-Steuerelement enthält nicht die **InsertMethod** -Eigenschaft und kann daher nicht zum Hinzufügen eines neuen Datensatzes mit Modell Bindung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="40f9e-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="40f9e-161">Sie finden die InsertMethod-Eigenschaft in den Steuerelementen **FormView**, **DetailsView**oder **ListView** .</span><span class="sxs-lookup"><span data-stu-id="40f9e-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="40f9e-162">In diesem Tutorial verwenden Sie ein FormView-Steuerelement, um einen neuen Datensatz hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="40f9e-163">Fügen Sie zunächst einen Link zur neuen Seite hinzu, die Sie zum Hinzufügen eines neuen Datensatzes erstellen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="40f9e-164">Fügen Sie oberhalb von ValidationSummary Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="40f9e-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="40f9e-165">Der neue Link wird am oberen Rand des Inhalts für die Seite "Studenten" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="40f9e-165">The new link will appear at the top of the content for the Students page.</span></span>

![neuer Link](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="40f9e-167">Fügen Sie dann ein neues Webformular mithilfe einer Master Seite hinzu, und nennen Sie es " **addstudent**".</span><span class="sxs-lookup"><span data-stu-id="40f9e-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="40f9e-168">Wählen Sie Site. Master als Master Seite aus.</span><span class="sxs-lookup"><span data-stu-id="40f9e-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="40f9e-169">Sie werden die Felder zum Hinzufügen eines neuen Studenten mithilfe eines **dynamicenti-** Steuer Elements gereinigen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="40f9e-170">Das dynamikty-Steuerelement rendert diese bearbeitbaren Eigenschaften in der Klasse, die in der ItemType-Eigenschaft angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="40f9e-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="40f9e-171">Die StudentID-Eigenschaft wurde mit dem Attribut **[gerüstcolumn (false)]** gekennzeichnet, sodass es nicht gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="40f9e-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="40f9e-172">Fügen Sie im Platzhalter mainContent der Seite addstudent den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f9e-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="40f9e-173">Fügen Sie in der Code Behind-Datei (AddStudent.aspx.cs) eine **using** -Anweisung für den **contosouniversitymodelbinding. Models** -Namespace hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f9e-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="40f9e-174">Fügen Sie dann die folgenden Methoden hinzu, um anzugeben, wie ein neuer Datensatz und ein Ereignishandler für die Schaltfläche Abbrechen eingefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="40f9e-175">Speichern Sie alle Änderungen.</span><span class="sxs-lookup"><span data-stu-id="40f9e-175">Save all of the changes.</span></span>

<span data-ttu-id="40f9e-176">Führen Sie die Webanwendung aus, und erstellen Sie einen neuen Studenten.</span><span class="sxs-lookup"><span data-stu-id="40f9e-176">Run the web application and create a new student.</span></span>

![neuen Studenten hinzufügen](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="40f9e-178">Klicken Sie auf **Einfügen** , und beachten Sie, dass der neue Student erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="40f9e-178">Click **Insert** and notice the new student has been created.</span></span>

![neuen Studenten anzeigen](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="40f9e-180">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="40f9e-180">Conclusion</span></span>

<span data-ttu-id="40f9e-181">In diesem Tutorial haben Sie das Aktualisieren, löschen und Erstellen von Daten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="40f9e-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="40f9e-182">Sie haben bei der Interaktion mit den Daten sichergestellt, dass Validierungsregeln angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="40f9e-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="40f9e-183">Im nächsten [Tutorial](sorting-paging-and-filtering-data.md) dieser Reihe aktivieren Sie das Sortieren, Paging und das Filtern von Daten.</span><span class="sxs-lookup"><span data-stu-id="40f9e-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40f9e-184">[Zurück](retrieving-data.md)
> [Weiter](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="40f9e-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
