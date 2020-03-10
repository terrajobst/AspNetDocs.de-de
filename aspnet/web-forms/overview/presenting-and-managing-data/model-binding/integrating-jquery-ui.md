---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrieren von jQuery UI DatePicker in die Modell Bindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht. Die Modell Bindung führt zu einer geraden Daten Interaktion-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521979"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="0112e-104">Integrieren von jQuery UI DatePicker in die Modell Bindung und Web Forms</span><span class="sxs-lookup"><span data-stu-id="0112e-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>

<span data-ttu-id="0112e-105">von [Tom fitzmacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0112e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0112e-106">In dieser tutorialreihe werden grundlegende Aspekte der Verwendung der Modell Bindung mit einem ASP.net Web Forms-Projekt veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="0112e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="0112e-107">Die Modell Bindung sorgt für eine genauere Daten Interaktion als bei der Verarbeitung von Datenquellen Objekten (z. b. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="0112e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="0112e-108">Diese Serie beginnt mit Einführungs Material und wechselt in spätere Tutorials zu erweiterten Konzepten.</span><span class="sxs-lookup"><span data-stu-id="0112e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="0112e-109">In diesem Tutorial wird gezeigt, wie Sie das jQuery UI [DatePicker-Widget](http://jqueryui.com/datepicker/) einem Webformular hinzufügen und die Modell Bindung verwenden, um die Datenbank mit dem ausgewählten Wert zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="0112e-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="0112e-110">Dieses Tutorial baut auf dem Projekt auf, das in den [ersten](retrieving-data.md) und [zweiten](updating-deleting-and-creating-data.md) Teilen der Reihe erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0112e-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="0112e-111">Sie können das gesamte Projekt in C# oder VB [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) .</span><span class="sxs-lookup"><span data-stu-id="0112e-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="0112e-112">Der herunterladbare Code funktioniert entweder mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0112e-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="0112e-113">Dabei wird die Vorlage Visual Studio 2012 verwendet, die sich geringfügig von der in diesem Tutorial gezeigten Visual Studio 2013 Vorlage unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="0112e-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="0112e-114">Was Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="0112e-114">What you'll build</span></span>

<span data-ttu-id="0112e-115">In diesem Tutorial gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="0112e-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="0112e-116">Fügen Sie dem Modell eine Eigenschaft hinzu, um das Registrierungsdatum des Studenten aufzuzeichnen.</span><span class="sxs-lookup"><span data-stu-id="0112e-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="0112e-117">Ermöglicht dem Benutzer die Auswahl des anmeldeanmeldedatums mithilfe des jQuery UI DatePicker-Widgets.</span><span class="sxs-lookup"><span data-stu-id="0112e-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="0112e-118">Erzwingen von Validierungsregeln für das anmelderungs Datum</span><span class="sxs-lookup"><span data-stu-id="0112e-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="0112e-119">Das jQuery UI DatePicker-Widget ermöglicht es Benutzern, auf einfache Weise ein Datum aus einem Kalender auszuwählen, der angezeigt wird, wenn der Benutzer mit dem Feld interagiert.</span><span class="sxs-lookup"><span data-stu-id="0112e-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="0112e-120">Das Verwenden dieses Widgets kann für Benutzer bequemer sein als das manuelle Eingeben eines Datums.</span><span class="sxs-lookup"><span data-stu-id="0112e-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="0112e-121">Die Integration des DatePicker-Widgets in eine Seite, die die Modell Bindung für Daten Vorgänge verwendet, erfordert nur eine kleine Menge zusätzlicher Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="0112e-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="0112e-122">Neue Eigenschaft zum Modell hinzufügen</span><span class="sxs-lookup"><span data-stu-id="0112e-122">Add a new property to the model</span></span>

<span data-ttu-id="0112e-123">Zuerst fügen Sie Ihrem Studenten Modell eine **DateTime** -Eigenschaft hinzu und migrieren diese Änderung in die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0112e-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="0112e-124">Öffnen Sie **UniversityModels.cs**, und fügen Sie dem Student-Modell den hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="0112e-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="0112e-125">Das **RangeAttribute-Attribut** ist enthalten, um Validierungsregeln für die Eigenschaft zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="0112e-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="0112e-126">Für dieses Tutorial gehen wir davon aus, dass die "Configuration Manager-Universität" am 1. Januar 2013 gegründet wurde und daher frühere Registrierungsdaten nicht gültig sind.</span><span class="sxs-lookup"><span data-stu-id="0112e-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="0112e-127">Fügen Sie im Paketverwaltung Fenster eine Migration hinzu, indem Sie den Befehl **Add-Migration addenrollmentdate**ausführen.</span><span class="sxs-lookup"><span data-stu-id="0112e-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="0112e-128">Beachten Sie, dass der Migrations Code der Tabelle "Student" die neue Spalte "DateTime" hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="0112e-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="0112e-129">Um den Wert abzugleichen, den Sie in RangeAttribute angegeben haben, fügen Sie einen Standardwert für die neue Spalte hinzu, wie im folgenden hervorgehobenen Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="0112e-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="0112e-130">Speichern Sie die Änderungen an der Migrations Datei.</span><span class="sxs-lookup"><span data-stu-id="0112e-130">Save your change to the migration file.</span></span>

<span data-ttu-id="0112e-131">Sie müssen die Daten nicht erneut als Ausgangswert für die Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="0112e-131">You do not need to seed the data again.</span></span> <span data-ttu-id="0112e-132">Öffnen Sie daher **Configuration.cs** im Migrations Ordner, und entfernen Sie den Code in der **Seed** -Methode, oder kommentieren Sie ihn aus.</span><span class="sxs-lookup"><span data-stu-id="0112e-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="0112e-133">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="0112e-133">Save and close the file.</span></span>

<span data-ttu-id="0112e-134">Führen Sie nun den Befehl **Update-Database**aus.</span><span class="sxs-lookup"><span data-stu-id="0112e-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="0112e-135">Beachten Sie, dass die Spalte jetzt in der Datenbank vorhanden ist und dass alle vorhandenen Datensätze den Standardwert für "anmelmentdate" aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0112e-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="0112e-136">Dynamische Steuerelemente zum Registrierungsdatum hinzufügen</span><span class="sxs-lookup"><span data-stu-id="0112e-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="0112e-137">Nun fügen Sie Steuerelemente zum Anzeigen und Bearbeiten des Anmeldedatums hinzu.</span><span class="sxs-lookup"><span data-stu-id="0112e-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="0112e-138">An diesem Punkt wird der Wert in einem Textfeld bearbeitet.</span><span class="sxs-lookup"><span data-stu-id="0112e-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="0112e-139">Später in diesem Tutorial ändern Sie das Textfeld in das jQuery-DatePicker-Widget.</span><span class="sxs-lookup"><span data-stu-id="0112e-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="0112e-140">Zunächst ist es wichtig zu beachten, dass Sie keine Änderungen an der **addstudent. aspx** -Datei vornehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="0112e-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="0112e-141">Die neue Eigenschaft wird automatisch durch das DynamicEntity-Steuerelement dargestellt.</span><span class="sxs-lookup"><span data-stu-id="0112e-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="0112e-142">Öffnen Sie **students. aspx**, und fügen Sie den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="0112e-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="0112e-143">Führen Sie die Anwendung aus, und beachten Sie, dass Sie den Wert des Anmeldungs Datums festlegen können, indem Sie ein Datum eingeben.</span><span class="sxs-lookup"><span data-stu-id="0112e-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="0112e-144">Beim Hinzufügen eines neuen Studenten:</span><span class="sxs-lookup"><span data-stu-id="0112e-144">When adding a new student:</span></span>

![Datum festlegen](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="0112e-146">Oder Bearbeiten eines vorhandenen Werts:</span><span class="sxs-lookup"><span data-stu-id="0112e-146">Or, editing an existing value:</span></span>

![Datum bearbeiten](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="0112e-148">Das Eingeben des Datums funktioniert, ist jedoch möglicherweise nicht die Kundenfreundlichkeit, die Sie bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="0112e-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="0112e-149">Im nächsten Abschnitt aktivieren Sie die Auswahl eines Datums in einem Kalender.</span><span class="sxs-lookup"><span data-stu-id="0112e-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="0112e-150">Installieren des nuget-Pakets für die Verwendung der jQuery-Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="0112e-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="0112e-151">Das nuget-Paket für die Benutzer **Oberfläche von Juice** ermöglicht eine einfache Integration der jQuery-UI-Widgets in Ihre Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="0112e-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="0112e-152">Um dieses Paket zu verwenden, installieren Sie es über nuget.</span><span class="sxs-lookup"><span data-stu-id="0112e-152">To use this package, install it through NuGet.</span></span>

![Hinzufügen einer Benutzeroberfläche](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="0112e-154">Die von Ihnen installierte Version der Juice-Benutzeroberfläche kann einen Konflikt mit der Version von jQuery in der Anwendung verursachen.</span><span class="sxs-lookup"><span data-stu-id="0112e-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="0112e-155">Bevor Sie mit diesem Tutorial fortfahren, versuchen Sie, die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0112e-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="0112e-156">Wenn ein JavaScript-Fehler auftritt, müssen Sie die jQuery-Version abstimmen.</span><span class="sxs-lookup"><span data-stu-id="0112e-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="0112e-157">Sie können entweder die erwartete Version von jQuery zum Skript Ordner hinzufügen (Version 1.8.2 zum Zeitpunkt des Schreibens dieses Tutorials) oder in Site. Master den Pfad zu der jQuery-Datei angeben.</span><span class="sxs-lookup"><span data-stu-id="0112e-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="0112e-158">Anpassen der DateTime-Vorlage an das DatePicker-widget</span><span class="sxs-lookup"><span data-stu-id="0112e-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="0112e-159">Sie fügen das DatePicker-Widget zur Vorlage für dynamische Daten hinzu, um einen DateTime-Wert zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="0112e-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="0112e-160">Indem das Widget der Vorlage hinzugefügt wird, wird es automatisch in der Form zum Hinzufügen eines neuen Studenten und in der Rasteransicht zum Bearbeiten von Schülern gerendert.</span><span class="sxs-lookup"><span data-stu-id="0112e-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="0112e-161">Öffnen Sie **DateTime\_"Edit. ascx**", und fügen Sie den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="0112e-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="0112e-162">In der Code Behind-Datei legen Sie die minimalen und maximalen Datumsangaben für "DatePicker" fest.</span><span class="sxs-lookup"><span data-stu-id="0112e-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="0112e-163">Wenn Sie diese Werte festlegen, werden Benutzer daran gehindert, zu ungültigen Datumsangaben zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="0112e-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="0112e-164">Sie rufen die minimalen und maximalen Werte aus **RangeAttribute** der DateTime-Eigenschaft ab, sofern vorhanden.</span><span class="sxs-lookup"><span data-stu-id="0112e-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="0112e-165">Öffnen Sie **DateTime-\_Edit.ascx.cs**, und fügen Sie der Seite\_Load-Methode den folgenden hervorgehobenen Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="0112e-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="0112e-166">Führen Sie die-Webanwendung aus, und navigieren Sie zur addstudent-Seite.</span><span class="sxs-lookup"><span data-stu-id="0112e-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="0112e-167">Geben Sie Werte für die Felder an. Beachten Sie, dass beim Klicken auf das Textfeld für das Registrierungsdatum der Kalender angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0112e-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Datumsauswahl](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="0112e-169">Wählen Sie ein Datum aus, und klicken Sie auf **Einfügen**.</span><span class="sxs-lookup"><span data-stu-id="0112e-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="0112e-170">Das RangeAttribute erzwingt die Validierung auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="0112e-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="0112e-171">Durch Festlegen der MinDate-Eigenschaft für DatePicker wenden Sie außerdem eine Validierung auf dem Client an.</span><span class="sxs-lookup"><span data-stu-id="0112e-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="0112e-172">Der Kalender ermöglicht dem Benutzer nicht, zu einem Datum vor dem Wert von MinDate zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="0112e-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="0112e-173">Wenn Sie einen Datensatz in der Rasteransicht bearbeiten, wird auch der Kalender angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0112e-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="0112e-175">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="0112e-175">Conclusion</span></span>

<span data-ttu-id="0112e-176">In diesem Tutorial haben Sie gelernt, wie Sie ein jQuery-Widget in ein Webformular einbinden, das eine Modell Bindung verwendet.</span><span class="sxs-lookup"><span data-stu-id="0112e-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="0112e-177">Im nächsten [Tutorial](using-query-string-values-to-retrieve-data.md)verwenden Sie einen Abfrage Zeichen folgen Wert, wenn Sie Daten auswählen.</span><span class="sxs-lookup"><span data-stu-id="0112e-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0112e-178">[Zurück](sorting-paging-and-filtering-data.md)
> [Weiter](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="0112e-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
