---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Erstellen einer MVC 3-Anwendung mit Razor und unaufdringlichem JavaScript | Microsoft-Dokumentation
author: microsoft
description: Die Webanwendung User List Sample zeigt, wie einfach es ist, ASP.NET MVC 3-Anwendungen mithilfe der Razor-Ansichts-Engine zu erstellen. Die Beispielanwendung s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434997"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="2a0b4-104">Erstellen einer MVC 3-Anwendung mit Razor und unaufdringlichem JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a0b4-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>

<span data-ttu-id="2a0b4-105">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2a0b4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="2a0b4-106">Die Webanwendung User List Sample zeigt, wie einfach es ist, ASP.NET MVC 3-Anwendungen mithilfe der Razor-Ansichts-Engine zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="2a0b4-107">Die Beispielanwendung zeigt, wie die neue Razor-Ansichts-Engine mit ASP.NET MVC Version 3 und Visual Studio 2010 verwendet wird, um eine fiktive Benutzer Listen-Website zu erstellen, die Funktionen wie das Erstellen, anzeigen, bearbeiten und Löschen von Benutzern umfasst.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="2a0b4-108">In diesem Tutorial werden die Schritte beschrieben, die zum Erstellen des Benutzer Listen Beispiels ASP.NET MVC 3-Anwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="2a0b4-109">Ein Visual Studio-Projekt C# mit und VB-Quellcode ist für dieses Thema verfügbar: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="2a0b4-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="2a0b4-110">Wenn Sie Fragen zu diesem Tutorial haben, veröffentlichen Sie Sie im [MVC-Forum](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a0b4-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>

## <a name="overview"></a><span data-ttu-id="2a0b4-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2a0b4-111">Overview</span></span>

<span data-ttu-id="2a0b4-112">Die Anwendung, die Sie entwickeln, ist eine einfache Benutzer Listen-Website.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="2a0b4-113">Benutzer können Benutzerinformationen eingeben, anzeigen und aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-113">Users can enter, view, and update user information.</span></span>

![Beispiel Website](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="2a0b4-115">Sie können das VB-Projekt C# und das abgeschlossene Projekt [hier](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="2a0b4-116">Erstellen der Webanwendung</span><span class="sxs-lookup"><span data-stu-id="2a0b4-116">Creating the Web Application</span></span>

<span data-ttu-id="2a0b4-117">Um das Tutorial zu starten, öffnen Sie Visual Studio 2010, und erstellen Sie ein neues Projekt mithilfe der Vorlage *ASP.NET MVC 3-Webanwendung* .</span><span class="sxs-lookup"><span data-stu-id="2a0b4-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="2a0b4-118">Benennen Sie die Anwendung &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="2a0b4-119">[neues MVC 3-Projekt ![](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="2a0b4-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="2a0b4-120">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 3-Projekt** die Option **Internet Anwendung**aus, wählen Sie die Razor-Ansicht-Engine aus, und klicken Sie dann auf **OK**</span><span class="sxs-lookup"><span data-stu-id="2a0b4-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Neues ASP.NET MVC 3-Projekt Dialogfeld](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="2a0b4-122">In diesem Tutorial verwenden Sie den ASP.net-Mitgliedschafts Anbieter nicht, sodass Sie alle Dateien löschen können, die mit der Anmeldung und der Mitgliedschaft verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="2a0b4-123">Entfernen Sie in **Projektmappen-Explorer**die folgenden Dateien und Verzeichnisse:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="2a0b4-124">*Controllers\accountcontroller*</span><span class="sxs-lookup"><span data-stu-id="2a0b4-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="2a0b4-125">*Models\accountmodels*</span><span class="sxs-lookup"><span data-stu-id="2a0b4-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="2a0b4-126">*Views\shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="2a0b4-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="2a0b4-127">*Views\account* (und alle Dateien in diesem Verzeichnis)</span><span class="sxs-lookup"><span data-stu-id="2a0b4-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln-Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="2a0b4-129">Bearbeiten Sie die Datei <em>\_Layout. cshtml</em> , und ersetzen Sie das Markup im `<div>` Element mit dem Namen `logindisplay` durch die Meldung <em>&quot;</em>Login&quot;deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="2a0b4-130">Das folgende Beispiel zeigt das neue Markup:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="2a0b4-131">Hinzufügen des Modells</span><span class="sxs-lookup"><span data-stu-id="2a0b4-131">Adding the Model</span></span>

<span data-ttu-id="2a0b4-132">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Modelle* , wählen Sie **Hinzufügen**aus, und klicken Sie auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Neue Benutzer-MDL-Klasse](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="2a0b4-134">Geben Sie der Klassen den Namen `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-134">Name the class `UserModel`.</span></span> <span data-ttu-id="2a0b4-135">Ersetzen Sie den Inhalt der *usermodel* -Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="2a0b4-136">Die `UserModel`-Klasse stellt Benutzer dar.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="2a0b4-137">Jeder Member der Klasse wird mit dem [erforderlichen](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) Attribut aus dem [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace kommentiert.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="2a0b4-138">Die Attribute im [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) -Namespace bieten eine automatische Client-und serverseitige Validierung für Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="2a0b4-139">Öffnen Sie die `HomeController`-Klasse, und fügen Sie eine `using`-Direktive hinzu, damit Sie auf die Klassen `UserModel` und `Users` zugreifen können:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="2a0b4-140">Fügen Sie direkt nach der `HomeController` Deklaration den folgenden Kommentar und den Verweis zu einer `Users` Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="2a0b4-141">Bei der `Users`-Klasse handelt es sich um einen vereinfachten, in-Memory-Datenspeicher, den Sie in diesem Tutorial verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="2a0b4-142">In einer echten Anwendung würden Sie eine Datenbank zum Speichern von Benutzerinformationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="2a0b4-143">Die ersten Zeilen der `HomeController`-Datei werden im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="2a0b4-144">Erstellen Sie die Anwendung so, dass das Benutzer Modell im nächsten Schritt für den Gerüstbau-Assistenten verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="2a0b4-145">Erstellen der Standardansicht</span><span class="sxs-lookup"><span data-stu-id="2a0b4-145">Creating the Default View</span></span>

<span data-ttu-id="2a0b4-146">Der nächste Schritt besteht darin, eine Aktionsmethode und Ansicht hinzuzufügen, um die Benutzer anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="2a0b4-147">Löschen Sie die vorhandene Datei " *views\home\index* ".</span><span class="sxs-lookup"><span data-stu-id="2a0b4-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="2a0b4-148">Sie erstellen eine neue *Index* Datei, um die Benutzer anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="2a0b4-149">Ersetzen Sie in der `HomeController`-Klasse den Inhalt der `Index`-Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="2a0b4-150">Klicken Sie mit der rechten Maustaste in die `Index` Methode, und klicken Sie dann auf **Ansicht hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="2a0b4-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Ansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="2a0b4-152">Wählen Sie die Option zum **Erstellen einer stark typisierten Ansicht** aus.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="2a0b4-153">Wählen Sie für **Datenklasse anzeigen**die Option **Mvc3Razor. Models. usermodel**aus.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="2a0b4-154">(Wenn **Mvc3Razor. Models. usermodel** nicht im Feld **Datenklasse anzeigen** angezeigt wird, müssen Sie das Projekt erstellen.) Stellen Sie sicher, dass die Ansichts-Engine auf **Razor**festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="2a0b4-155">Legen Sie **Inhalt anzeigen** auf **auflisten** fest, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-155">Set **View content** to **List** and then click **Add**.</span></span>

![Index Sicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="2a0b4-157">Die neue Ansicht richtet automatisch die Benutzerdaten ein, die an die `Index` Ansicht übermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="2a0b4-158">Überprüfen Sie die neu generierte Datei *views\home\index* .</span><span class="sxs-lookup"><span data-stu-id="2a0b4-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="2a0b4-159">Die Links " **Create New**", " **Edit**", " **Details**" und " **Delete** " funktionieren nicht, aber der Rest der Seite ist funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="2a0b4-160">Führen Sie die Seite aus.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-160">Run the page.</span></span> <span data-ttu-id="2a0b4-161">Eine Liste der Benutzer wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-161">You see a list of users.</span></span>

![Index Seite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="2a0b4-163">Öffnen Sie die Datei " *Index. cshtml* ", und ersetzen Sie das `ActionLink` Markup durch den folgenden Code für **Bearbeiten**, **Details**und **Delete** :</span><span class="sxs-lookup"><span data-stu-id="2a0b4-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="2a0b4-164">Der Benutzername wird als ID verwendet, um den ausgewählten Datensatz in den Links " **Bearbeiten**", " **Details**" und " **Löschen** " zu suchen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="2a0b4-165">Erstellen der Detailansicht</span><span class="sxs-lookup"><span data-stu-id="2a0b4-165">Creating the Details View</span></span>

<span data-ttu-id="2a0b4-166">Der nächste Schritt besteht darin, eine `Details` Aktionsmethode und-Ansicht hinzuzufügen, um Benutzer Details anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="2a0b4-168">Fügen Sie dem Home-Controller die folgende `Details`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="2a0b4-169">Klicken Sie mit der rechten Maustaste in die `Details`-Methode, und wählen Sie dann <strong>Ansicht hinzufügen</strong></span><span class="sxs-lookup"><span data-stu-id="2a0b4-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="2a0b4-170">Vergewissern Sie sich, dass das Feld <strong>Datenklasse anzeigen</strong> das <strong>Mvc3Razor. Models. usermodel</strong>enthält<em>.</em></span><span class="sxs-lookup"><span data-stu-id="2a0b4-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="2a0b4-171">Legen Sie <strong>Inhalt anzeigen</strong> auf <strong>Details</strong> fest, und klicken Sie auf <strong>Hinzufügen</strong>.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Detailansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="2a0b4-173">Führen Sie die Anwendung aus, und wählen Sie einen Link Details aus.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-173">Run the application and select a details link.</span></span> <span data-ttu-id="2a0b4-174">Das automatische Gerüst zeigt jede Eigenschaft im Modell an.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-174">The automatic scaffolding shows each property in the model.</span></span>

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="2a0b4-176">Erstellen der Bearbeitungs Ansicht</span><span class="sxs-lookup"><span data-stu-id="2a0b4-176">Creating the Edit View</span></span>

<span data-ttu-id="2a0b4-177">Fügen Sie dem Home-Controller die folgende `Edit`-Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="2a0b4-178">Fügen Sie eine Ansicht wie in den vorherigen Schritten hinzu, aber legen Sie **Inhalt anzeigen** auf **Bearbeiten**fest.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Bearbeitungs Ansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="2a0b4-180">Führen Sie die Anwendung aus, und bearbeiten Sie den vor-und Nachnamen eines Benutzers.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="2a0b4-181">Wenn Sie gegen `DataAnnotation` Einschränkungen verstoßen, die auf die `UserModel` Klasse angewendet wurden, werden beim Senden des Formulars Validierungs Fehler angezeigt, die vom Servercode erzeugt werden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="2a0b4-182">Wenn Sie z. b. den Vornamen &quot;Ann&quot; ändern, um eine&quot;zu &quot;, wird der folgende Fehler im Formular angezeigt, wenn Sie das Formular senden:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="2a0b4-183">In diesem Tutorial behandeln Sie den Benutzernamen als Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="2a0b4-184">Aus diesem Grund kann die Eigenschaft Benutzername nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="2a0b4-185">Legen Sie in der Datei " *Edit. cshtml* " direkt nach der `Html.BeginForm`-Anweisung den Benutzernamen als ausgeblendetes Feld fest.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="2a0b4-186">Dies bewirkt, dass die-Eigenschaft im Modell übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="2a0b4-187">Das folgende Code Fragment zeigt die Platzierung der `Hidden`-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="2a0b4-188">Ersetzen Sie das `TextBoxFor`-und `ValidationMessageFor` Markup für den Benutzernamen durch einen `DisplayFor`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="2a0b4-189">Die `DisplayFor`-Methode zeigt die Eigenschaft als Schreib geschütztes Element an.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="2a0b4-190">Das folgende Beispiel zeigt das abgeschlossene Markup.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-190">The following example shows the completed markup.</span></span> <span data-ttu-id="2a0b4-191">Die ursprünglichen `TextBoxFor` und `ValidationMessageFor` Aufrufe werden mit den Razor-BEGIN-comment-und End-Comment-Zeichen (`@* *@`) auskommentiert.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="2a0b4-192">Aktivieren der Client seitigen Validierung</span><span class="sxs-lookup"><span data-stu-id="2a0b4-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="2a0b4-193">Um die Client seitige Validierung in ASP.NET MVC 3 zu aktivieren, müssen Sie zwei Flags festlegen, und Sie müssen drei JavaScript-Dateien einschließen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="2a0b4-194">Öffnen Sie die Datei " *Web. config* " der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="2a0b4-195">Überprüfen Sie, `that ClientValidationEnabled` und `UnobtrusiveJavaScriptEnabled` in den Anwendungseinstellungen auf true festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="2a0b4-196">Das folgende Fragment aus der *Web. config* -Stammdatei zeigt die korrekten Einstellungen an:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="2a0b4-197">Wenn Sie `UnobtrusiveJavaScriptEnabled` auf true festlegen, wird eine unaufdringliche und unaufdringliche Client Validierung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="2a0b4-198">Wenn Sie die unaufdringliche Validierung verwenden, werden die Validierungsregeln in HTML5-Attribute umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="2a0b4-199">HTML5-Attributnamen dürfen nur aus Kleinbuchstaben, Ziffern und Bindestrichen bestehen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="2a0b4-200">Wenn Sie `ClientValidationEnabled` auf true festlegen, wird die Client seitige Validierung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="2a0b4-201">Wenn Sie diese Schlüssel in der *Web. config* -Datei der Anwendung festlegen, aktivieren Sie die Client Validierung und unaufdringliches JavaScript für die gesamte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="2a0b4-202">Sie können diese Einstellungen auch in einzelnen Ansichten oder in Controller Methoden aktivieren oder deaktivieren, indem Sie den folgenden Code verwenden:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="2a0b4-203">Außerdem müssen Sie mehrere JavaScript-Dateien in der gerenderten Ansicht einschließen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="2a0b4-204">Eine einfache Möglichkeit, JavaScript in alle Ansichten einzufügen, besteht darin, Sie der Datei " *views\shared\\_Layout. cshtml* " hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="2a0b4-205">Ersetzen Sie das `<head>`-Element der Datei *\_Layout. cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="2a0b4-206">Die ersten beiden jQuery-Skripts werden vom Microsoft AJAX-Content Delivery Network (CDN) gehostet.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="2a0b4-207">Wenn Sie das Microsoft AJAX CDN nutzen, können Sie die Leistung Ihrer Anwendungen erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="2a0b4-208">Führen Sie die Anwendung aus, und klicken Sie auf den Link bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-208">Run the application and click an edit link.</span></span> <span data-ttu-id="2a0b4-209">Anzeigen der Quelle der Seite im Browser.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-209">View the page's source in the browser.</span></span> <span data-ttu-id="2a0b4-210">Die Browser Quelle zeigt viele Attribute der Form `data-val` (für die Datenvalidierung).</span><span class="sxs-lookup"><span data-stu-id="2a0b4-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="2a0b4-211">Wenn die Client Validierung und das unaufdringliche Javascript aktiviert sind, enthalten Eingabefelder mit einer Client Validierungs Regel das `data-val="true"`-Attribut, um eine unaufdringliche Client Validierung zu initiieren.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="2a0b4-212">Beispielsweise wurde das `City`-Feld im Modell mit dem [erforderlichen](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut ergänzt, das im folgenden Beispiel den HTML-Code ergibt:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="2a0b4-213">Für jede Client Validierungs Regel wird ein Attribut hinzugefügt, das die Form `data-val-rulename="message"`hat.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="2a0b4-214">Mithilfe des zuvor gezeigten `City` Field-Beispiels generiert die erforderliche Client Validierungs Regel das `data-val-required`-Attribut, und die Meldung &quot;das Feld "City" ist erforderlich&quot;.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="2a0b4-215">Führen Sie die Anwendung aus, bearbeiten Sie einen der Benutzer, und löschen Sie das Feld `City`.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="2a0b4-216">Wenn Sie das Feld aus dem Feld auslagern, wird eine Client seitige Validierungs Fehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Stadt erforderlich](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="2a0b4-218">Ebenso wird für jeden Parameter in der Client Validierungs Regel ein Attribut hinzugefügt, das die Form `data-val-rulename-paramname=paramvalue`hat.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="2a0b4-219">Beispielsweise wird die `FirstName`-Eigenschaft mit dem [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut kommentiert und gibt eine minimale Länge von 3 und eine maximale Länge von 8 an.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="2a0b4-220">Die Daten Validierungs Regel namens `length` hat den Parameternamen `max` und den Parameterwert 8.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="2a0b4-221">Das folgende Beispiel zeigt den HTML-Code, der für das Feld `FirstName` generiert wird, wenn Sie einen der Benutzer bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="2a0b4-222">Weitere Informationen zur unaufdringlichen Client Validierung finden Sie im Blogeintrag [unaufdringliche Client Validierung in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson es Blog.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="2a0b4-223">In ASP.NET MVC 3 Beta müssen Sie manchmal das Formular übermitteln, um die Client seitige Validierung zu starten.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="2a0b4-224">Dies kann für die endgültige Version geändert werden.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-224">This might be changed for the final release.</span></span>

## <a name="creating-the-create-view"></a><span data-ttu-id="2a0b4-225">Erstellen der CREATE VIEW</span><span class="sxs-lookup"><span data-stu-id="2a0b4-225">Creating the Create View</span></span>

<span data-ttu-id="2a0b4-226">Der nächste Schritt besteht darin, eine `Create` Aktionsmethode und-Ansicht hinzuzufügen, um dem Benutzer zu ermöglichen, einen neuen Benutzer zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="2a0b4-227">Fügen Sie dem Home-Controller die folgende `Create`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="2a0b4-228">Fügen Sie eine Ansicht wie in den vorherigen Schritten hinzu, aber legen Sie **Inhalt anzeigen** auf **Erstellen**fest.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View (Ansicht erstellen)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="2a0b4-230">Führen Sie die Anwendung aus, wählen Sie den Link **Erstellen** aus, und fügen Sie einen neuen Benutzer hinzu.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="2a0b4-231">Die `Create`-Methode nutzt automatisch die Client seitige und serverseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="2a0b4-232">Versuchen Sie, einen Benutzernamen einzugeben, der Leerzeichen enthält, z. b. &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="2a0b4-233">Wenn Sie im Feld Benutzername auf die Registerkarte klicken, wird ein Client seitiger Validierungs Fehler (`White space is not allowed`) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="2a0b4-234">Hinzufügen der Delete-Methode</span><span class="sxs-lookup"><span data-stu-id="2a0b4-234">Add the Delete method</span></span>

<span data-ttu-id="2a0b4-235">Fügen Sie dem Home-Controller die folgende `Delete` Methode hinzu, um das Tutorial abzuschließen:</span><span class="sxs-lookup"><span data-stu-id="2a0b4-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="2a0b4-236">Fügen Sie wie in den vorherigen Schritten eine `Delete` Ansicht hinzu, indem Sie den **Inhalt** auf **Löschen**festlegen.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Ansicht löschen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="2a0b4-238">Sie verfügen jetzt über eine einfache, aber voll funktionsfähige ASP.NET MVC 3-Anwendung mit Validierung.</span><span class="sxs-lookup"><span data-stu-id="2a0b4-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
