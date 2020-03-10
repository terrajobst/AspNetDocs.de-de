---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Praktische Übungseinheit: One ASP.net: Integration ASP.net Web Forms, MVC und Web API | Microsoft-Dokumentation'
author: rick-anderson
description: ASP.net ist ein Framework zum Aufbauen von Websites, Apps und Diensten mit spezialisierten Technologien wie MVC, Web-API und anderen. Mit der Erweiterung ASP.net h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505455"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="5b9f0-104">Praktische Übungseinheit: One ASP.net: Integration ASP.net Web Forms, MVC und Web API</span><span class="sxs-lookup"><span data-stu-id="5b9f0-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>

<span data-ttu-id="5b9f0-105">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5b9f0-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5b9f0-106">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="5b9f0-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="5b9f0-107">ASP.net ist ein Framework zum Aufbauen von Websites, Apps und Diensten mit spezialisierten Technologien wie MVC, Web-API und anderen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="5b9f0-108">Mit der Erweiterung ASP.net hat sich seit der Erstellung und der ausdrücklichen Notwendigkeit, diese Technologien **integriert zu haben**, bewährt</span><span class="sxs-lookup"><span data-stu-id="5b9f0-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="5b9f0-109">Visual Studio 2013 führt ein neues einheitliches Projekt System ein, mit dem Sie eine Anwendung erstellen und alle ASP.NET-Technologien in einem Projekt verwenden können.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="5b9f0-110">Durch dieses Feature entfällt die Notwendigkeit, eine Technologie zu Beginn eines Projekts auszuwählen und sich daran zu halten. stattdessen wird die Verwendung mehrerer ASP.NET-Frameworks in einem Projekt angeregt.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="5b9f0-111">Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="5b9f0-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5b9f0-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5b9f0-113">Ziele</span><span class="sxs-lookup"><span data-stu-id="5b9f0-113">Objectives</span></span>

<span data-ttu-id="5b9f0-114">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5b9f0-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="5b9f0-115">Erstellen einer Website basierend auf dem **One ASP.net** -Projekttyp</span><span class="sxs-lookup"><span data-stu-id="5b9f0-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="5b9f0-116">Verwenden Sie unterschiedliche **ASP.net** -Frameworks wie **MVC** und die **Web-API** im gleichen Projekt.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="5b9f0-117">Identifizieren der Hauptkomponenten einer **ASP.net** -Anwendung</span><span class="sxs-lookup"><span data-stu-id="5b9f0-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="5b9f0-118">Nutzen Sie das **ASP.net Gerüstbau** -Framework, um automatisch Controller und Sichten zum Ausführen von CRUD-Vorgängen basierend auf Ihren Modellklassen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="5b9f0-119">Machen Sie denselben Satz von Informationen in maschinellen und lesbaren Formaten verfügbar, indem Sie das richtige Tool für jeden Auftrag verwenden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5b9f0-120">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5b9f0-120">Prerequisites</span></span>

<span data-ttu-id="5b9f0-121">Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:</span><span class="sxs-lookup"><span data-stu-id="5b9f0-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="5b9f0-122">[Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher</span><span class="sxs-lookup"><span data-stu-id="5b9f0-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="5b9f0-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="5b9f0-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5b9f0-124">Einrichten</span><span class="sxs-lookup"><span data-stu-id="5b9f0-124">Setup</span></span>

<span data-ttu-id="5b9f0-125">Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zuerst Ihre Umgebung einrichten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="5b9f0-126">Öffnen Sie Windows-Explorer, und navigieren Sie zum **Quell** Ordner des Labs.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="5b9f0-127">Klicken Sie mit der rechten Maustaste auf **Setup. cmd** , und wählen Sie **als Administrator ausführen** aus, um den Setup Vorgang zu starten, mit dem die Umgebung konfiguriert wird, und die Visual Studio-Code Ausschnitte für diese Übungseinheit</span><span class="sxs-lookup"><span data-stu-id="5b9f0-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="5b9f0-128">Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="5b9f0-129">Vergewissern Sie sich, dass Sie vor dem Ausführen des Setups alle Abhängigkeiten für dieses Lab geprüft haben.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="5b9f0-130">Verwenden der Code Ausschnitte</span><span class="sxs-lookup"><span data-stu-id="5b9f0-130">Using the Code Snippets</span></span>

<span data-ttu-id="5b9f0-131">Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="5b9f0-132">Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, auf die Sie in Visual Studio 2013 zugreifen können, um das manuelle Hinzufügen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="5b9f0-133">Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner " **Begin** " der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="5b9f0-134">Beachten Sie, dass die während einer Übung hinzugefügten Code Ausschnitte in diesen Projektmappen fehlen und möglicherweise erst nach Abschluss der Übung funktionieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="5b9f0-135">Innerhalb des Quellcodes für eine Übung finden Sie auch einen **endordner** mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="5b9f0-136">Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5b9f0-137">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="5b9f0-137">Exercises</span></span>

<span data-ttu-id="5b9f0-138">Diese praktische Übungseinheit umfasst die folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="5b9f0-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="5b9f0-139">Erstellen eines neuen Web Forms Projekts</span><span class="sxs-lookup"><span data-stu-id="5b9f0-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="5b9f0-140">Erstellen eines MVC-Controllers mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="5b9f0-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="5b9f0-141">Erstellen eines Web-API-Controllers mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="5b9f0-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="5b9f0-142">Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="5b9f0-143">Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="5b9f0-144">Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="5b9f0-145">In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="5b9f0-146">Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="5b9f0-147">Übung 1: Erstellen eines neuen Web Forms Projekts</span><span class="sxs-lookup"><span data-stu-id="5b9f0-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="5b9f0-148">In dieser Übung erstellen Sie eine neue Web Forms Website in Visual Studio 2013 mithilfe der **ASP.net** Unified Project-Funktion, die es Ihnen ermöglicht, Web Forms-, MVC-und Web-API-Komponenten problemlos in dieselbe Anwendung zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="5b9f0-149">Sie werden dann die generierte Lösung durchsuchen und deren Teile identifizieren, und schließlich wird die Website in Aktion angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="5b9f0-150">Aufgabe 1 – Erstellen einer neuen Website mit einer ASP.net-Funktion</span><span class="sxs-lookup"><span data-stu-id="5b9f0-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="5b9f0-151">In dieser Aufgabe beginnen Sie mit dem Erstellen einer neuen Website in Visual Studio basierend auf dem **One ASP.net** -Projekttyp.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="5b9f0-152">**Eine ASP.net** vereinigt alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit, Sie nach Belieben zu mischen und abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="5b9f0-153">Anschließend erkennen Sie die verschiedenen Komponenten von Web Forms, MVC und der Web-API, die nebeneinander innerhalb Ihrer Anwendung nebeneinander stehen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="5b9f0-154">Öffnen Sie **Visual Studio Express 2013 für Web** , und wählen Sie Datei aus.  **Neues Projekt...** , um eine neue Projekt Mappe zu starten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Erstellen eines neuen Projekts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="5b9f0-156">*Erstellen eines neuen Projekts*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="5b9f0-157">Wählen Sie im Dialogfeld **Neues Projekt** unter der Visualisierung **C# ASP.NET Webanwendung aus.** Registerkarte Web, und stellen Sie sicher, dass **.NET Framework 4,5** ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="5b9f0-158">Nennen Sie das Projekt *myhybridsite*, wählen Sie einen **Speicherort** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Neues ASP.NET-Webanwendungs Projekt](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="5b9f0-160">*Erstellen eines neuen ASP.NET-Webanwendungs Projekts*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="5b9f0-161">Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die **Vorlage Web Forms** aus, und wählen Sie die Optionen **MVC** und **Web-API** aus.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="5b9f0-162">Stellen Sie außerdem sicher, dass die **Authentifizierungs** Option auf **einzelne Benutzerkonten**festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="5b9f0-163">Klicken Sie zum Fortsetzen des Vorgangs auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-163">Click **OK** to continue.</span></span>

    ![Erstellen eines neuen Projekts mit der Web Forms Vorlage, einschließlich Web-API und MVC-Komponenten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="5b9f0-165">*Erstellen eines neuen Projekts mit der Web Forms Vorlage, einschließlich Web-API und MVC-Komponenten*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="5b9f0-166">Nun können Sie die Struktur der generierten Lösung untersuchen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-166">You can now explore the structure of the generated solution.</span></span>

    ![Untersuchen der generierten Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="5b9f0-168">*Untersuchen der generierten Lösung*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="5b9f0-169">**Konto:** Dieser Ordner enthält die Web Form-Seiten für die Registrierung, die Anmeldung bei und die Verwaltung der Benutzerkonten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="5b9f0-170">Dieser Ordner wird hinzugefügt, wenn die Authentifizierungs Option **einzelner Benutzerkonten** während der Konfiguration der Web Forms-Projektvorlage ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="5b9f0-171">**Modelle:** Dieser Ordner enthält die Klassen, die Ihre Anwendungsdaten darstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="5b9f0-172">**Controller** und **Ansichten**: diese Ordner sind für die **ASP.NET MVC** -und **ASP.net-Web-API** -Komponenten erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="5b9f0-173">In den nächsten Übungen erfahren Sie mehr über die MVC-und Web-API-Technologien.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="5b9f0-174">Die Dateien " **default. aspx**", " **Contact. aspx** " und " **about. aspx** " sind vordefinierte Web Form-Seiten, die Sie als Ausgangspunkt verwenden können, um die für die Anwendung spezifischen Seiten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="5b9f0-175">Die Programmierlogik dieser Dateien befindet sich in einer separaten Datei, die als &quot;Code-Behind-&quot; Datei bezeichnet wird, die über eine &quot;. aspx. vb&quot;-oder &quot;. aspx.cs&quot;-Erweiterung (abhängig von der verwendeten Sprache) verfügt.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="5b9f0-176">Die Code-Behind-Logik wird auf dem Server ausgeführt und erzeugt dynamisch die HTML-Ausgabe für die Seite.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="5b9f0-177">Auf den Seiten " **Site. Master** " und " **Site. Mobile. Master** " werden das Aussehen und Verhalten und das Standardverhalten aller Seiten in der Anwendung definiert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="5b9f0-178">Doppelklicken Sie auf die Datei " **default. aspx** ", um den Inhalt der Seite zu durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Untersuchen der Seite "default. aspx"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="5b9f0-180">*Untersuchen der Seite "default. aspx"*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-181">Mit der **Page** -Direktive am Anfang der Datei werden die Attribute der Web Forms Seite definiert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="5b9f0-182">Beispielsweise gibt das **MasterPageFile** -Attribut den Pfad zur Master Seite an, in diesem Fall die *Site. Master* -Seite und das erbt-Attribut, das die Code-Behind-Klasse für die zu **erbende** Seite definiert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="5b9f0-183">Diese Klasse befindet sich in der Datei, die durch das **Code Behind** -Attribut bestimmt wird.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="5b9f0-184">Das **ASP: Content** -Steuerelement enthält den eigentlichen Inhalt der Seite (Text, Markup und Steuerelemente) und wird einem **ASP: contentplachalter** -Steuerelement auf der Master Seite zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="5b9f0-185">In diesem Fall wird der Seiten Inhalt innerhalb des *mainContent* -Steuer Elements gerendert, das auf der Seite *Site. Master* definiert ist.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="5b9f0-186">Erweitern Sie den Ordner **App\_Start** , und beachten Sie die Datei **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="5b9f0-187">Visual Studio enthielt diese Datei in die generierte Projekt Mappe, da Sie beim Konfigurieren Ihres Projekts mit der One ASP.net-Vorlage eine Web-API eingeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="5b9f0-188">Öffnen Sie die Datei **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="5b9f0-189">In der *webapiconfig* -Klasse finden Sie die Konfiguration, die der Web-API zugeordnet ist, die http-Routen zu **Web-API-Controllern**zuordnet.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="5b9f0-190">Öffnen Sie die Datei **RouteConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="5b9f0-191">In der *RegisterRoutes* -Methode finden Sie die Konfiguration, die MVC zugeordnet ist, mit der http-Routen zu **MVC-Controllern**zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="5b9f0-192">Aufgabe 2 – Ausführen der Lösung</span><span class="sxs-lookup"><span data-stu-id="5b9f0-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="5b9f0-193">In dieser Aufgabe führen Sie die generierte Lösung aus, untersuchen die APP und einige ihrer Features, wie das Umschreiben von URLs und die integrierte Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="5b9f0-194">Drücken Sie **F5** , oder klicken Sie auf die Schaltfläche **Start** auf der Symbolleiste, um die Projekt Mappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="5b9f0-195">Die Startseite der Anwendung sollte im Browser geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-195">The application home page should open in the browser.</span></span>

    ![Ausführen der Lösung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="5b9f0-197">Überprüfen Sie, ob die Web Forms Seiten aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="5b9f0-198">Fügen Sie zu diesem Zweck **/Contact.aspx** an die URL in der Adressleiste an, und drücken **Sie die Eingabe**Taste.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Friendly URLs](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="5b9f0-200">*Friendly URLs*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-201">Wie Sie sehen können, wird die URL in **/Contact**geändert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="5b9f0-202">Ab **ASP.NET 4**wurden die URL-Routing Funktionen Web Forms hinzugefügt, sodass Sie URLs wie *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* anstelle von *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* schreiben können.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="5b9f0-203">Weitere Informationen finden Sie unter [URL-Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="5b9f0-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="5b9f0-204">Sie werden nun den in die Anwendung integrierten Authentifizierungs Fluss untersuchen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="5b9f0-205">Klicken Sie hierzu in der oberen rechten Ecke der Seite auf **registrieren** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrieren eines neuen Benutzers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="5b9f0-207">*Registrieren eines neuen Benutzers*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-207">*Registering a new user*</span></span>
4. <span data-ttu-id="5b9f0-208">Geben Sie auf der **Register** Seite einen **Benutzernamen** und ein **Kennwort**ein, und klicken Sie dann auf **registrieren**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Seite registrieren](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="5b9f0-210">*Seite registrieren*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-210">*Register page*</span></span>
5. <span data-ttu-id="5b9f0-211">Die Anwendung registriert das neue Konto, und der Benutzer wird authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Benutzer authentifiziert](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="5b9f0-213">*Benutzer authentifiziert*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-213">*User authenticated*</span></span>
6. <span data-ttu-id="5b9f0-214">Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="5b9f0-215">Übung 2: Erstellen eines MVC-Controllers mithilfe eines Gerüsts</span><span class="sxs-lookup"><span data-stu-id="5b9f0-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="5b9f0-216">In dieser Übung nutzen Sie das ASP.net Gerüstbau-Framework, das von Visual Studio bereitgestellt wird, um einen ASP.NET MVC 5-Controller mit Aktionen und Razor-Ansichten zum Durchführen von CRUD-Vorgängen zu erstellen, ohne eine einzige Codezeile zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="5b9f0-217">Der Gerüstbau Prozess verwendet Entity Framework Code First, um den Datenkontext und das Datenbankschema in der SQL-Datenbank zu generieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="5b9f0-218">**Informationen zu Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="5b9f0-219">Entity Framework (EF) ist ein Objekt relationaler Mapper (ORM), mit dem Sie Datenzugriffs Anwendungen erstellen können, indem Sie mit einem konzeptionellen Anwendungsmodell programmieren, anstatt direkt mithilfe eines relationalen Speicher Schemas zu programmieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="5b9f0-220">Der Workflow für die Entity Framework Code First Modellierung ermöglicht Ihnen die Verwendung ihrer eigenen Domänen Klassen, um das Modell darzustellen, das EF beim Ausführen von Abfragen, Änderungs Nachverfolgung und Aktualisierungs Funktionen verwendet.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="5b9f0-221">Wenn Sie den Code First Entwicklungs Workflow verwenden, müssen Sie die Anwendung nicht starten, indem Sie eine Datenbank erstellen oder ein Schema angeben.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="5b9f0-222">Stattdessen können Sie .net-Standardklassen schreiben, die die am besten geeigneten Domänen Modell Objekte für Ihre Anwendung definieren, und Entity Framework die Datenbank für Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="5b9f0-223">Weitere Informationen zu Entity Framework finden Sie [hier](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="5b9f0-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="5b9f0-224">Aufgabe 1 – Erstellen eines neuen Modells</span><span class="sxs-lookup"><span data-stu-id="5b9f0-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="5b9f0-225">Nun definieren Sie eine **Person** -Klasse, bei der es sich um das Modell handelt, das vom Gerüstbau Prozess zum Erstellen des MVC-Controllers und der Ansichten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="5b9f0-226">Zunächst erstellen Sie eine **Person** -Modell Klasse, und die CRUD-Vorgänge im Controller werden automatisch mithilfe von Gerüstbau Features erstellt.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="5b9f0-227">Öffnen Sie **Visual Studio Express 2013 für das Web** und die Projekt Mappe " **myhybridsite. sln** ", die sich im Ordner " **Source/EX2-mvcgerüst/BEGIN** " befindet.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="5b9f0-228">Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="5b9f0-229">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Models** des Projekts **myhybridsite** , und wählen Sie **hinzufügen aus. Klasse...**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Hinzufügen der Person-Modell Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="5b9f0-231">*Hinzufügen der Person-Modell Klasse*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="5b9f0-232">Benennen Sie im Dialogfeld **Neues Element hinzufügen** die Datei *Person.cs* , und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Erstellen der Person-Modell Klasse](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="5b9f0-234">*Erstellen der Person-Modell Klasse*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="5b9f0-235">Ersetzen Sie den Inhalt der **Person.cs** -Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="5b9f0-236">Drücken Sie **STRG + S** , um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="5b9f0-237">(Code Ausschnitt- *bringingtogetheroneaspnet-EX2-Personclass*)</span><span class="sxs-lookup"><span data-stu-id="5b9f0-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="5b9f0-238">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **myhybridsite** , und wählen Sie **Erstellen**aus, oder drücken Sie **STRG + UMSCHALT + B** , um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="5b9f0-239">Aufgabe 2 – Erstellen eines MVC-Controllers</span><span class="sxs-lookup"><span data-stu-id="5b9f0-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="5b9f0-240">Nachdem das **Person** -Modell erstellt wurde, verwenden Sie ASP.NET MVC-Gerüstbau mit Entity Framework, um die CRUD-Controller Aktionen und-Ansichten für **Person**zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="5b9f0-241">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Controllers** des Projekts **myhybridsite** , und wählen Sie **Hinzufügen | Neues Gerüst Element...**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Erstellen eines neuen Gerüst Controllers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="5b9f0-243">*Erstellen eines neuen Gerüst Controllers*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="5b9f0-244">Wählen Sie im Dialogfeld **Gerüst hinzufügen** die Option **MVC 5-Controller mit Ansichten unter Verwendung Entity Framework** aus, und klicken Sie dann auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Auswählen von MVC 5-Controller mit Ansichten und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="5b9f0-246">*Auswählen von MVC 5-Controller mit Ansichten und Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="5b9f0-247">Legen Sie *mvcpersoncontroller* als **Controller Namen**fest, wählen Sie die Option **Async Controller Actions verwenden** aus, und wählen Sie **Person (myhybridsite. Models)** als **Modell Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Hinzufügen eines MVC-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="5b9f0-249">*Hinzufügen eines MVC-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="5b9f0-250">Klicken Sie unter **Datenkontext Klasse**auf **neuer Datenkontext...** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Erstellen eines neuen Daten Kontexts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="5b9f0-252">*Erstellen eines neuen Daten Kontexts*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="5b9f0-253">Benennen Sie im Dialogfeld **neuer Datenkontext** den neuen Datenkontext *personcontext* , und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Erstellen des neuen personcontext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="5b9f0-255">*Erstellen des neuen personcontext-Typs*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="5b9f0-256">Klicken Sie auf **Hinzufügen** , um den neuen Controller für **Person** mit Gerüst zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="5b9f0-257">Visual Studio generiert dann die Controller Aktionen, den Datenkontext der Person und die Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Nach dem Erstellen des MVC-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="5b9f0-259">*Nach dem Erstellen des MVC-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="5b9f0-260">Öffnen Sie die Datei **MvcPersonController.cs** im Ordner **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="5b9f0-261">Beachten Sie, dass die CRUD-Aktionsmethoden automatisch generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b9f0-262">Durch Aktivieren des Kontrollkästchens **Async-Controller Aktionen** aus den Gerüstbau Optionen in den vorherigen Schritten generiert Visual Studio asynchrone Aktionsmethoden für alle Aktionen, die Zugriff auf den Person-Datenkontext einschließen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="5b9f0-263">Es wird empfohlen, asynchrone Aktionsmethoden für lang ausgeführte, nicht CPU-gebundene Anforderungen zu verwenden, um zu verhindern, dass der Webserver während der Verarbeitung der Anforderung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="5b9f0-264">Aufgabe 3 – Ausführen der Lösung</span><span class="sxs-lookup"><span data-stu-id="5b9f0-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="5b9f0-265">In dieser Aufgabe führen Sie die Projekt Mappe erneut aus, um zu überprüfen, ob die Ansichten für die **Person** erwartungsgemäß funktionieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="5b9f0-266">Sie werden eine neue Person hinzufügen, um zu überprüfen, ob Sie erfolgreich in der Datenbank gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="5b9f0-267">Drücken Sie **F5** , um die Projekt Mappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="5b9f0-268">Navigieren Sie zu **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="5b9f0-269">Die Gerüst Ansicht, die die Liste der Personen anzeigt, sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="5b9f0-270">Klicken Sie auf **neu erstellen** , um eine neue Person hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-270">Click **Create New** to add a new person.</span></span>

    ![Navigieren zu den dashboarded-MVC-Ansichten](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="5b9f0-272">*Navigieren zu den dashboarded-MVC-Ansichten*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="5b9f0-273">Geben Sie in der Ansicht **Erstellen** einen **Namen** und ein **Alter** für die Person an, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Hinzufügen einer neuen Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="5b9f0-275">*Hinzufügen einer neuen Person*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-275">*Adding a new person*</span></span>
5. <span data-ttu-id="5b9f0-276">Die neue Person wird der Liste hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-276">The new person is added to the list.</span></span> <span data-ttu-id="5b9f0-277">Klicken Sie in der Liste Element auf **Details** , um die Detailansicht der Person anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="5b9f0-278">Klicken Sie dann in der **Detail** Ansicht auf **zurück zur Liste** , um zur Listenansicht zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Detailansicht der Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="5b9f0-280">*Detailansicht der Person*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-280">*Person's details view*</span></span>
6. <span data-ttu-id="5b9f0-281">Klicken Sie auf den Link **Löschen** , um die Person zu löschen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="5b9f0-282">Klicken Sie in der Ansicht **Löschen** auf **Löschen** , um den Vorgang zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Löschen einer Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="5b9f0-284">*Löschen einer Person*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-284">*Deleting a person*</span></span>
7. <span data-ttu-id="5b9f0-285">Kehren Sie zu Visual Studio zurück, und drücken Sie **UMSCHALT + F5** , um das Debugging zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="5b9f0-286">Übung 3: Erstellen eines Web-API-Controllers mithilfe von Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="5b9f0-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="5b9f0-287">Das Web-API-Framework ist Teil des ASP.net-Stapels und wurde entwickelt, um die Implementierung von http-Diensten zu vereinfachen und im allgemeinen JSON-oder XML-formatierte Daten über eine restliche API zu senden und zu empfangen</span><span class="sxs-lookup"><span data-stu-id="5b9f0-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="5b9f0-288">In dieser Übung verwenden Sie ASP.net Gerüstbau erneut, um einen Web-API-Controller zu generieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="5b9f0-289">Sie verwenden dieselben **Person** -und **personcontext** -Klassen aus der vorherigen Übung, um dieselben Personendaten im JSON-Format bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="5b9f0-290">Sie werden sehen, wie Sie dieselben Ressourcen auf verschiedene Weise in derselben ASP.NET-Anwendung verfügbar machen können.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="5b9f0-291">Aufgabe 1 – Erstellen eines Web-API-Controllers</span><span class="sxs-lookup"><span data-stu-id="5b9f0-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="5b9f0-292">In dieser Aufgabe erstellen Sie einen neuen **Web-API-Controller** , der die Personendaten in einem Computer nutzbaren Format wie JSON verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="5b9f0-293">Wenn Sie nicht bereits geöffnet ist, öffnen Sie **Visual Studio Express 2013 für das Web** , und öffnen Sie die Projekt Mappe " **myhybridsite. sln** ", die sich im Ordner **Quell/EX3-WebAPI/Anfang** befindet.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="5b9f0-294">Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-295">Wenn Sie mit der BEGIN-Projekt Mappe aus Übung 3 beginnen, drücken Sie **STRG + UMSCHALT + B** , um die Projekt Mappe zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="5b9f0-296">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Controllers** des Projekts **myhybridsite** , und wählen Sie **Hinzufügen | Neues Gerüst Element...**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Erstellen eines neuen Gerüst Controllers](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="5b9f0-298">*Erstellen eines neuen Gerüst Controllers*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="5b9f0-299">Wählen Sie im Dialogfeld **Gerüst hinzufügen** im linken Bereich **Web-API** und dann **Web-API 2-Controller mit Aktionen Entity Framework** im mittleren Bereich aus, und klicken Sie dann auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="5b9f0-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="5b9f0-300">![Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="5b9f0-301">*Auswählen von Web-API 2-Controller mit Aktionen und Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="5b9f0-302">Legen Sie *apipersoncontroller* als **Controller Namen**fest, wählen Sie die Option **Async Controller Actions verwenden** aus, und wählen Sie **Person (myhybridsite. Models)** und **personcontext (myhybridsite. Models)** als **Modell** -bzw. **Datenkontext** Klassen aus.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="5b9f0-303">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-303">Then click **Add**.</span></span>

    <span data-ttu-id="5b9f0-304">![Hinzufügen eines Web-API-Controllers mit Gerüstbau](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Hinzufügen eines Web-API-Controllers mit Gerüstbau")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="5b9f0-305">*Hinzufügen eines Web-API-Controllers mit Gerüstbau*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="5b9f0-306">Visual Studio generiert dann die **apipersoncontroller** -Klasse mit den vier CRUD-Aktionen, um mit Ihren Daten zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="5b9f0-307">![Nach dem Erstellen des Web-API-Controllers mit Gerüst](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Nach dem Erstellen des Web-API-Controllers mit Gerüst")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="5b9f0-308">*Nach dem Erstellen des Web-API-Controllers mit Gerüst*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="5b9f0-309">Öffnen Sie die Datei **ApiPersonController.cs** , und überprüfen Sie die *GetPeople* -Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="5b9f0-310">Diese Methode fragt das Datenbankfeld des **personcontext** -Typs ab, um die Personendaten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="5b9f0-311">Beachten Sie nun den Kommentar oberhalb der Methoden Definition.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="5b9f0-312">Sie stellt den URI bereit, der diese Aktion verfügbar macht, die Sie in der nächsten Aufgabe verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b9f0-313">Die Web-API ist standardmäßig so konfiguriert, dass die Abfragen im */API* -Pfad abgefangen werden, um Konflikte mit MVC-Controllern zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="5b9f0-314">Wenn Sie diese Einstellung ändern müssen, finden Sie weitere Informationen unter [Routing in ASP.net-Web-API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5b9f0-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="5b9f0-315">Aufgabe 2 – Ausführen der Lösung</span><span class="sxs-lookup"><span data-stu-id="5b9f0-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="5b9f0-316">In dieser Aufgabe verwenden Sie die Internet Explorer **F12-Entwicklertools** , um die vollständige Antwort vom Web-API-Controller zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="5b9f0-317">Sie werden erfahren, wie Sie Netzwerk Datenverkehr erfassen, um einen besseren Einblick in Ihre Anwendungsdaten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="5b9f0-318">Stellen Sie sicher, dass **Internet Explorer** in der Schaltfläche **Start** auf der Symbolleiste von Visual Studio ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer (Option)](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="5b9f0-320">Die **F12-Entwicklertools** verfügen über eine Vielzahl von Funktionen, die in dieser praktischen Übungseinheit nicht behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="5b9f0-321">Weitere Informationen hierzu finden Sie unter [Verwenden der F12-Entwicklertools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="5b9f0-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>

1. <span data-ttu-id="5b9f0-322">Drücken Sie **F5** , um die Projekt Mappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-323">Damit diese Aufgabe ordnungsgemäß ausgeführt werden kann, muss Ihre Anwendung über Daten verfügen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="5b9f0-324">Wenn die Datenbank leer ist, können Sie zu Aufgabe 3 in Übung 2 zurückkehren und die Schritte zum Erstellen einer neuen Person mithilfe der MVC-Ansichten ausführen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="5b9f0-325">Drücken Sie im Browser die Taste **F12** , um den **Entwicklertools** Bereich zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="5b9f0-326">Drücken Sie **STRG** + **4** , oder klicken Sie auf das **Netzwerk** Symbol, und klicken Sie dann auf die grüne Pfeiltaste, um den Netzwerk Datenverkehr zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="5b9f0-327">![Initiieren der Web-API-Netzwerk Erfassung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiieren der Web-API-Netzwerk Erfassung")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="5b9f0-328">*Initiieren der Web-API-Netzwerk Erfassung*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="5b9f0-329">Fügen Sie **API/apiperson** an die URL in der Adressleiste des Browsers an.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="5b9f0-330">Nun überprüfen Sie die Details der Antwort von **apipersoncontroller**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="5b9f0-331">![Abrufen von Personendaten über die Web-API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Abrufen von Personendaten über die Web-API")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="5b9f0-332">*Abrufen von Personendaten über die Web-API*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-333">Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei durchführen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="5b9f0-334">Lassen Sie das Dialogfeld geöffnet, um den Antwort Inhalt über das Tool Fenster "Entwickler" anzeigen zu können.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="5b9f0-335">Nun überprüfen Sie den Text der Antwort.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="5b9f0-336">Klicken Sie hierzu auf die Registerkarte **Details** , und klicken Sie dann auf **Antworttext**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="5b9f0-337">Sie können überprüfen, ob die heruntergeladenen Daten eine Liste von Objekten mit der Eigenschaften- **ID**, dem **Namen** und dem **Alter** der **Person** -Klasse sind.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="5b9f0-338">![Anzeigen des Web-API-Antwort Texts](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Anzeigen des Web-API-Antwort Texts")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="5b9f0-339">*Anzeigen des Web-API-Antwort Texts*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="5b9f0-340">Aufgabe 3 – Hinzufügen von Web-API-Hilfeseiten</span><span class="sxs-lookup"><span data-stu-id="5b9f0-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="5b9f0-341">Wenn Sie eine Web-API erstellen, ist es hilfreich, eine Hilfeseite zu erstellen, damit andere Entwickler wissen, wie Sie Ihre API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="5b9f0-342">Sie könnten die Dokumentationsseiten manuell erstellen und aktualisieren, aber es ist besser, Sie automatisch zu generieren, um eine Wartung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="5b9f0-343">In dieser Aufgabe verwenden Sie ein nuget-Paket zum automatischen Generieren von Web-API-Hilfeseiten für die Lösung.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="5b9f0-344">Wählen Sie **im Menü Extras** in Visual Studio den **nuget-Paket-Manager**aus, und klicken Sie dann auf Paket-Manager- **Konsole**.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-344">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="5b9f0-345">Führen Sie im Fenster der **Paket-Manager-Konsole** den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5b9f0-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="5b9f0-346">Das Paket **Microsoft. Aspnet. WebAPI. helppage** installiert die erforderlichen Assemblys und fügt MVC-Ansichten für die Hilfeseiten im Ordner **Bereiche/helppage** hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="5b9f0-347">![Bereich "helppage"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Bereich "helppage"")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="5b9f0-348">*Bereich "helppage"*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="5b9f0-349">Standardmäßig verfügen die Hilfeseiten über Platzhalter Zeichenfolgen für die Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="5b9f0-350">Sie können XML-Dokumentations Kommentare verwenden, um die Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="5b9f0-351">Um dieses Feature zu aktivieren, öffnen Sie die Datei **HelpPageConfig.cs** , die sich im Ordner **Bereiche/helppage/App\_Start** befindet, und heben Sie die Auskommentierung der folgenden Zeile auf:</span><span class="sxs-lookup"><span data-stu-id="5b9f0-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="5b9f0-352">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **myhybridsite**, wählen Sie **Eigenschaften** , und klicken Sie auf die Registerkarte **Erstellen** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="5b9f0-353">![Registerkarte Erstellen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Abschnitt "Build"")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="5b9f0-354">*Registerkarte Erstellen*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-354">*Build tab*</span></span>
5. <span data-ttu-id="5b9f0-355">Wählen Sie unter **Ausgabe**die Option **XML-Dokumentations Datei**aus.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="5b9f0-356">Geben Sie im Bearbeitungsfeld **App\_Data/XmlDocument. XML**ein.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="5b9f0-357">![Ausgabe Abschnitt der Registerkarte "Build"](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Ausgabe Abschnitt der Registerkarte "Build"")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="5b9f0-358">*Ausgabe Abschnitt der Registerkarte "Build"*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="5b9f0-359">Drücken Sie **STRG** + **S** , um die Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="5b9f0-360">Öffnen Sie die Datei **ApiPersonController.cs** aus dem Ordner **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="5b9f0-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="5b9f0-361">Geben Sie eine neue Zeile zwischen der *GetPeople* -Methoden Signatur und dem Kommentar *//Get API/apiperson* ein, und geben Sie dann drei Schrägstriche ein.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-362">Visual Studio fügt automatisch die XML-Elemente ein, die die Methoden Dokumentation definieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="5b9f0-363">Fügen Sie einen Übersichts Text und den Rückgabewert für die *GetPeople* -Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="5b9f0-364">Es sollte wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="5b9f0-365">Drücken Sie **F5** , um die Projekt Mappe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="5b9f0-366">Fügen Sie **/Help** an die URL in der Adressleiste an, um zur Hilfeseite zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="5b9f0-367">![ASP.net-Web-API Hilfeseite](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.net-Web-API Hilfeseite")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="5b9f0-368">*ASP.net-Web-API Hilfeseite*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b9f0-369">Der Hauptinhalt der Seite ist eine Tabelle mit APIs, gruppiert nach Controller.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="5b9f0-370">Die Tabelleneinträge werden mithilfe der **iapiexplorer** -Schnittstelle dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="5b9f0-371">Wenn Sie einen API-Controller hinzufügen oder aktualisieren, wird die Tabelle automatisch aktualisiert, wenn Sie das nächste Mal die Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="5b9f0-372">In der Spalte **API** werden die HTTP-Methode und der relative URI aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="5b9f0-373">Die Spalte **Beschreibung** enthält Informationen, die aus der Dokumentation der Methode extrahiert wurden.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="5b9f0-374">Beachten Sie, dass die Beschreibung, die Sie oberhalb der Methoden Definition hinzugefügt haben, in der Spalte Beschreibung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="5b9f0-375">![API-Methoden Beschreibung](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API-Methoden Beschreibung")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="5b9f0-376">*API-Methoden Beschreibung*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-376">*API method description*</span></span>
13. <span data-ttu-id="5b9f0-377">Klicken Sie auf eine API-Methode, um zu einer Seite mit detaillierteren Informationen zu navigieren, einschließlich Beispiel Antworttext.</span><span class="sxs-lookup"><span data-stu-id="5b9f0-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="5b9f0-378">![Detail Informationen (Seite)](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Informationen (Seite)")</span><span class="sxs-lookup"><span data-stu-id="5b9f0-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="5b9f0-379">*Seite mit detaillierten Informationen*</span><span class="sxs-lookup"><span data-stu-id="5b9f0-379">*Detailed information page*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5b9f0-380">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="5b9f0-380">Summary</span></span>

<span data-ttu-id="5b9f0-381">Durch die Durchführung dieses praktischen Labs haben Sie Folgendes gelernt:</span><span class="sxs-lookup"><span data-stu-id="5b9f0-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="5b9f0-382">Erstellen Sie eine neue Webanwendung mit der ASP.net-Funktion in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5b9f0-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="5b9f0-383">Integrieren mehrerer ASP.NET-Technologien in ein einzelnes Projekt</span><span class="sxs-lookup"><span data-stu-id="5b9f0-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="5b9f0-384">Generieren von MVC-Controllern und-Sichten aus ihren Modellklassen mithilfe von ASP.net</span><span class="sxs-lookup"><span data-stu-id="5b9f0-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="5b9f0-385">Generieren von Web-API-Controllern, die Funktionen wie die asynchrone Programmierung und den Datenzugriff über Entity Framework verwenden</span><span class="sxs-lookup"><span data-stu-id="5b9f0-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="5b9f0-386">Automatisches Generieren von Web-API-Hilfeseiten für Ihre Controller</span><span class="sxs-lookup"><span data-stu-id="5b9f0-386">Automatically generate Web API Help Pages for your controllers</span></span>
