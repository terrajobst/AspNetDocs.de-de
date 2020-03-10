---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4-Grundlagen | Microsoft-Dokumentation
author: rick-anderson
description: Diese praktische Übungseinheit basiert auf MVC (Model View Controller) Music Store, einer Lernprogramm Anwendung, die Schritt für Schritt die Verwendung von ASP.net MV erläutert und erläutert.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484569"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="0e1f3-103">ASP.NET MVC 4 Fundamentals</span><span class="sxs-lookup"><span data-stu-id="0e1f3-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="0e1f3-104">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="0e1f3-105">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="0e1f3-106">Diese praktische Übungseinheit basiert auf MVC (Model View Controller) Music Store, einer Lernprogramm Anwendung, die Schritt für Schritt zur Verwendung von ASP.NET MVC und Visual Studio führt und erläutert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="0e1f3-107">Im gesamten Lab erlernen Sie die Einfachheit, aber die Leistungsfähigkeit, diese Technologien zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="0e1f3-108">Sie beginnen mit einer einfachen Anwendung und erstellen Sie, bis Sie über eine voll funktionsfähige ASP.NET MVC 4-Webanwendung verfügen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="0e1f3-109">Diese Übungseinheit funktioniert mit ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="0e1f3-110">Wenn Sie die ASP.NET MVC 3-Version der Tutorial-Anwendung untersuchen möchten, finden Sie diese in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="0e1f3-111">Diese praktische Übungseinheit geht davon aus, dass der Entwickler Erfahrung in Webentwicklungs Technologien wie HTML und JavaScript hat.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="0e1f3-112">Der gesamte Beispielcode und die Code Ausschnitte sind im webcamps-Trainingskit enthalten, das unter [Microsoft-Web/webcamptrainingkit-Releases](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="0e1f3-113">Das für dieses Lab spezifische Projekt ist unter [ASP.NET MVC 4-Grundlagen](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="0e1f3-114">Die Music Store-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-114">The Music Store application</span></span>

<span data-ttu-id="0e1f3-115">Die Music Store-Webanwendung, die in diesem Lab erstellt wird, umfasst drei Hauptkomponenten: Einkaufen, Auschecken und Verwaltung.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="0e1f3-116">Besucher können Alben nach Genre durchsuchen, dem Warenkorb Alben hinzufügen, Ihre Auswahl überprüfen und schließlich mit dem Auschecken fortfahren und die Bestellung vervollständigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="0e1f3-117">Außerdem können Geschäfts Administratoren die verfügbaren Alben und deren Haupteigenschaften verwalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="0e1f3-118">![Bildschirme des Musik Stores](aspnet-mvc-4-fundamentals/_static/image1.png "Bildschirme des Musik Stores")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="0e1f3-119">*Bildschirme des Musik Stores*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="0e1f3-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="0e1f3-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="0e1f3-121">Die Music Store-Anwendung wird mit dem **Model View Controller (MVC)** erstellt, einem Architekturmuster, das eine Anwendung in drei Hauptkomponenten trennt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="0e1f3-122">**Modelle**: Modell Objekte sind die Teile der Anwendung, die die Domänen Logik implementieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="0e1f3-123">Häufig rufen Modell Objekte auch den Modell Zustand in einer Datenbank ab und speichern Sie.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="0e1f3-124">**Sichten:** Ansichten sind die Komponenten, die die Benutzeroberfläche der Anwendung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="0e1f3-125">In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="0e1f3-126">Ein Beispiel wäre die Bearbeitungs Ansicht von Alben, in der Textfelder und eine Dropdown Liste basierend auf dem aktuellen Zustand eines Album Objekts angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="0e1f3-127">**Controller:** Controller sind die Komponenten, die die Benutzerinteraktion verarbeiten, das Modell bearbeiten und letztendlich eine Ansicht auswählen, um die Benutzeroberfläche zu Rendering.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="0e1f3-128">In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="0e1f3-129">Das MVC-Muster hilft Ihnen beim Erstellen von Anwendungen, die die verschiedenen Aspekte der Anwendung (Eingabe Logik, Geschäftslogik und UI-Logik) trennen, während gleichzeitig eine lose Kopplung zwischen diesen Elementen ermöglicht wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="0e1f3-130">Diese Trennung hilft Ihnen bei der Verwaltung der Komplexität, wenn Sie eine Anwendung erstellen, da Sie sich auf einen Aspekt der Implementierung gleichzeitig konzentrieren kann.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="0e1f3-131">Darüber hinaus vereinfacht das MVC-Muster das Testen von Anwendungen und fördert außerdem die Verwendung der Test gesteuerten Entwicklung (Test-gesteuerte Entwicklung, TDD) zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="0e1f3-132">Das **ASP.NET-MVC** -Framework bietet eine Alternative zum ASP.net-Web Forms Muster zum Erstellen von ASP.NET MVC-basierten Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="0e1f3-133">Das **ASP.NET-MVC** -Framework ist ein schlankes, hochgradig testbares Präsentations Framework, das (wie bei Webformular basierten Anwendungen) in vorhandene ASP.NET-Features wie Masterseiten und Mitgliedschafts basierte Authentifizierung integriert ist, sodass Sie die gesamte Leistungsfähigkeit des .NET Framework-Kern Servers erhalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="0e1f3-134">Dies ist nützlich, wenn Sie bereits mit ASP.net Web Forms vertraut sind, da alle bereits verwendeten Bibliotheken auch in ASP.NET MVC 4 verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="0e1f3-135">Außerdem fördert die lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung auch die parallele Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="0e1f3-136">Beispielsweise kann ein Entwickler an der Ansicht arbeiten, ein zweiter Entwickler kann an der Controller Logik arbeiten, und ein Dritter Entwickler kann sich auf die Geschäftslogik im Modell konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="0e1f3-137">Ziele</span><span class="sxs-lookup"><span data-stu-id="0e1f3-137">Objectives</span></span>

<span data-ttu-id="0e1f3-138">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="0e1f3-139">Erstellen Sie eine ASP.NET MVC-Anwendung basierend auf dem Lernprogramm für Musikspeicher Anwendungen von Grund auf neu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="0e1f3-140">Hinzufügen von Controllern zum Verarbeiten von URLs zur Startseite der Website und zum Durchsuchen der Hauptfunktionen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="0e1f3-141">Fügen Sie eine Ansicht hinzu, um den angezeigten Inhalt zusammen mit dem Stil anzupassen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="0e1f3-142">Hinzufügen von Modellklassen, die Daten und Domänen Logik enthalten und verwalten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="0e1f3-143">Verwenden Sie das Modell Muster anzeigen, um Informationen von Controller Aktionen an die Ansichts Vorlagen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="0e1f3-144">Erkunden Sie die neue Vorlage ASP.NET MVC 4 für Internetanwendungen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="0e1f3-145">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-145">Prerequisites</span></span>

<span data-ttu-id="0e1f3-146">Zum Durchführen dieses Labs müssen Sie über Folgendes verfügen:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="0e1f3-147">[Visual Studio 2012 Express für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Weitere Informationen zur Installation finden Sie in [Anhang A](#AppendixA) )</span><span class="sxs-lookup"><span data-stu-id="0e1f3-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="0e1f3-148">Einrichten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-148">Setup</span></span>

<span data-ttu-id="0e1f3-149">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="0e1f3-150">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="0e1f3-151">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="0e1f3-152">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, können Sie den Anhang dieses Dokuments &quot;[Anhang C: Verwenden von Code Ausschnitten](#AppendixC)&quot;entnehmen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="0e1f3-153">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="0e1f3-153">Exercises</span></span>

<span data-ttu-id="0e1f3-154">Diese praktische Übungseinheit besteht aus den folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="0e1f3-155">Übung 1: Erstellen eines Musik Stores ASP.NET MVC-Webanwendungs Projekts</span><span class="sxs-lookup"><span data-stu-id="0e1f3-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="0e1f3-156">Übung 2: Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="0e1f3-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="0e1f3-157">Übung 3: übergeben von Parametern an einen Controller</span><span class="sxs-lookup"><span data-stu-id="0e1f3-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="0e1f3-158">Übung 4: Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="0e1f3-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="0e1f3-159">Übung 5: Erstellen eines Ansichts Modells</span><span class="sxs-lookup"><span data-stu-id="0e1f3-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="0e1f3-160">Übung 6: Verwenden von Parametern in der Ansicht</span><span class="sxs-lookup"><span data-stu-id="0e1f3-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="0e1f3-161">Übung 7: A Lap around ASP.NET MVC 4 New Template</span><span class="sxs-lookup"><span data-stu-id="0e1f3-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="0e1f3-162">Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="0e1f3-163">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="0e1f3-164">Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="0e1f3-165">Übung 1: Erstellen eines Musik Stores ASP.NET MVC-Webanwendungs Projekts</span><span class="sxs-lookup"><span data-stu-id="0e1f3-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="0e1f3-166">In dieser Übung erfahren Sie, wie Sie eine ASP.NET MVC-Anwendung in Visual Studio 2012 Express für das Web und in ihrer Hauptordner Organisation erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="0e1f3-167">Darüber hinaus erfahren Sie, wie ein neuer Controller hinzugefügt und eine einfache Zeichenfolge auf der Startseite der Anwendung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="0e1f3-168">Aufgabe 1: Erstellen des ASP.NET MVC-Webanwendungs Projekts</span><span class="sxs-lookup"><span data-stu-id="0e1f3-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="0e1f3-169">In dieser Aufgabe erstellen Sie ein leeres ASP.NET MVC-Anwendungsprojekt mithilfe der MVC-Vorlage für Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="0e1f3-170">Starten Sie **vs Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="0e1f3-171">Klicken Sie im Menü **Datei** auf **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="0e1f3-172">Wählen Sie im Dialogfeld **Neues Projekt** den Projekttyp **ASP.NET MVC 4-Webanwendung** aus, der sich unter **Visual C#** **Web** Template List befindet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="0e1f3-173">Ändern Sie den **Namen** in *mvcmusicstore*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="0e1f3-174">Legen Sie den Speicherort der Projekt Mappe in einem neuen Ordner " **Begin** " im Quellordner dieser Übung fest, z. b. **[your-Hol-path] \source\ex01-deatingmusicstoreproject\begin**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="0e1f3-175">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-175">Click **OK**.</span></span>

    <span data-ttu-id="0e1f3-176">![Dialog Feld "Neues Projekt erstellen"](aspnet-mvc-4-fundamentals/_static/image2.png "Dialog Feld "Neues Projekt erstellen"")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="0e1f3-177">*Dialog Feld "Neues Projekt erstellen"*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="0e1f3-178">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die **Basis** Vorlage aus, und vergewissern Sie sich, dass die ausgewählte **Ansichts-Engine** **Razor**ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="0e1f3-179">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-179">Click **OK**.</span></span>

    <span data-ttu-id="0e1f3-180">![Neues ASP.NET MVC 4-Projekt (Dialog Feld)](aspnet-mvc-4-fundamentals/_static/image3.png "Neues ASP.NET MVC 4-Projekt (Dialog Feld)")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="0e1f3-181">*Neues ASP.NET MVC 4-Projekt (Dialog Feld)*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="0e1f3-182">Aufgabe 2: Untersuchen der Lösungs Struktur</span><span class="sxs-lookup"><span data-stu-id="0e1f3-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="0e1f3-183">Das ASP.NET MVC-Framework enthält eine Visual Studio-Projektvorlage, mit der Sie Webanwendungen erstellen können, die das MVC-Muster unterstützen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="0e1f3-184">Diese Vorlage erstellt eine neue ASP.NET MVC-Webanwendung mit den erforderlichen Ordnern, Element Vorlagen und Konfigurationsdatei Einträgen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="0e1f3-185">In dieser Aufgabe überprüfen Sie die Lösungs Struktur, um die beteiligten Elemente und ihre Beziehungen zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="0e1f3-186">Die folgenden Ordner sind in der ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig eine &quot;Konvention über die Konfiguration&quot; Ansatzes verwendet und einige Standard Annahmen basierend auf den Benennungs Konventionen für Ordner vornimmt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="0e1f3-187">Nachdem das Projekt erstellt wurde, überprüfen Sie die Ordnerstruktur, die in der Projektmappen-Explorer auf der rechten Seite erstellt wurde:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="0e1f3-188">![ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="0e1f3-189">*ASP.NET MVC-Ordnerstruktur in Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="0e1f3-190">**Controller**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-190">**Controllers**.</span></span> <span data-ttu-id="0e1f3-191">Dieser Ordner enthält die Controller Klassen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="0e1f3-192">In einer MVC-basierten Anwendung sind Controller für die Handhabung von Endbenutzer Interaktionen, das Bearbeiten des Modells und schließlich das Auswählen einer Ansicht zum Rendering der Benutzeroberfläche verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="0e1f3-193">Das MVC-Framework erfordert, dass die Namen aller Controller mit &quot;Controller&quot;enden, z. b. HomeController, logincontroller oder ProductController.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="0e1f3-194">**Modelle**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-194">**Models**.</span></span> <span data-ttu-id="0e1f3-195">Dieser Ordner wird für Klassen bereitgestellt, die das Anwendungsmodell für die MVC-Webanwendung darstellen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="0e1f3-196">Dies schließt in der Regel den Code ein, der Objekte und die Logik für die Interaktion mit dem Datenspeicher definiert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="0e1f3-197">In der Regel befinden sich die tatsächlichen Modell Objekte in separaten Klassenbibliotheken.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="0e1f3-198">Wenn Sie jedoch eine neue Anwendung erstellen, können Sie Klassen einschließen und Sie zu einem späteren Zeitpunkt im Entwicklungszyklen in separate Klassenbibliotheken verschieben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="0e1f3-199">**Sichten**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-199">**Views**.</span></span> <span data-ttu-id="0e1f3-200">Dieser Ordner ist der empfohlene Speicherort für Ansichten, die Komponenten, die für die Anzeige der Benutzeroberfläche der Anwendung verantwortlich sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="0e1f3-201">In Sichten werden neben anderen Dateien, die sich auf das Rendern von Sichten beziehen, auch die Dateien aspx,. ascx,. cshtml und. Master verwendet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="0e1f3-202">Der Ordner views enthält einen Ordner für jeden Controller. der Ordner wird mit dem Controller Namen Präfix benannt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="0e1f3-203">Wenn Sie z. b. einen Controller mit dem Namen " **HomeController**" haben, enthält der Ordner "Views" einen Ordner namens "Home".</span><span class="sxs-lookup"><span data-stu-id="0e1f3-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="0e1f3-204">Wenn das ASP.NET-MVC-Framework eine Ansicht lädt, sucht es standardmäßig nach einer ASPX-Datei mit dem angeforderten Ansichts Namen im Ordner "views\controllername" (**views [ControllerName] [Action]. aspx**) oder (**views [Controller Name] [Action]. cshtml**) für Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0e1f3-205">Zusätzlich zu den zuvor aufgeführten Ordnern verwendet eine MVC-Webanwendung die Datei " **Global. asax** ", um globale URL-Routing Standardwerte festzulegen, und die Datei " **Web. config** " wird verwendet, um die Anwendung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="0e1f3-206">Aufgabe 3: Hinzufügen eines HomeController</span><span class="sxs-lookup"><span data-stu-id="0e1f3-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="0e1f3-207">In ASP.NET-Anwendungen, in denen das MVC-Framework nicht verwendet wird, wird die Benutzerinteraktion um Seiten organisiert, um Ereignisse von diesen Seiten zu erhöhen und zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="0e1f3-208">Im Gegensatz dazu ist die Benutzerinteraktion mit ASP.NET MVC-Anwendungen auf Controllern und ihren Aktionsmethoden organisiert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="0e1f3-209">Das ASP.NET-MVC-Framework ordnet URLs jedoch Klassen zu, die als Controller bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="0e1f3-210">Controller verarbeiten eingehende Anforderungen, behandeln Benutzereingaben und Interaktionen, führen die entsprechende Anwendungslogik aus und bestimmen die Antwort, die an den Client zurückgesendet werden soll (HTML anzeigen, Datei herunterladen, zu einer anderen URL umleiten usw.).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="0e1f3-211">Wenn HTML angezeigt wird, ruft eine Controller Klasse in der Regel eine separate Ansichts Komponente auf, um das HTML-Markup für die Anforderung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="0e1f3-212">In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="0e1f3-213">In dieser Aufgabe fügen Sie eine Controller Klasse hinzu, die URLs auf der Startseite der Music Store-Website behandelt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="0e1f3-214">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Controller** Ordner, wählen Sie **Hinzufügen** und dann **Controller** Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="0e1f3-215">![Controller Befehl hinzufügen](aspnet-mvc-4-fundamentals/_static/image5.png "Controller Befehl hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="0e1f3-216">*Controller Befehl hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="0e1f3-217">Das Dialogfeld **Controller hinzufügen** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="0e1f3-218">Nennen Sie den Controller *HomeController* , und klicken **Sie auf Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="0e1f3-219">![Controller Dialog hinzufügen](aspnet-mvc-4-fundamentals/_static/image6.png "Controller Dialog hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="0e1f3-220">*Controller Dialog hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="0e1f3-221">Die Datei **HomeController.cs** wird im Ordner **Controller** erstellt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="0e1f3-222">Damit der **HomeController** eine Zeichenfolge für die Index Aktion zurückgibt, ersetzen Sie die **Index** -Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="0e1f3-223">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="0e1f3-224">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="0e1f3-225">In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="0e1f3-226">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="0e1f3-227">Das Projekt wird kompiliert, und der lokale IIS-Webserver wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="0e1f3-228">Der lokale IIS-Webserver öffnet automatisch einen Webbrowser, der auf die URL des Webservers zeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="0e1f3-229">![Anwendung, die in einem Webbrowser ausgeführt wird](aspnet-mvc-4-fundamentals/_static/image7.png "Anwendung, die in einem Webbrowser ausgeführt wird")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="0e1f3-230">*Anwendung, die in einem Webbrowser ausgeführt wird*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-231">Der lokale IIS-Webserver führt die Website auf einer zufälligen, kostenlosen Portnummer aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="0e1f3-232">In der obigen Abbildung wird die Website unter `http://localhost:50103/`ausgeführt. Daher wird Port 50103 verwendet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="0e1f3-233">Die Portnummer kann variieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-233">Your port number may vary.</span></span>
2. <span data-ttu-id="0e1f3-234">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="0e1f3-235">Übung 2: Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="0e1f3-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="0e1f3-236">In dieser Übung erfahren Sie, wie Sie den Controller aktualisieren, um einfache Funktionen der Music Store-Anwendung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="0e1f3-237">Dieser Controller definiert Aktionsmethoden, um die folgenden spezifischen Anforderungen zu behandeln:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="0e1f3-238">Eine Listenseite der Musik-Genres im Music Store</span><span class="sxs-lookup"><span data-stu-id="0e1f3-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="0e1f3-239">Eine Seite zum Durchsuchen, die alle Musikalben für ein bestimmtes Genre auflistet</span><span class="sxs-lookup"><span data-stu-id="0e1f3-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="0e1f3-240">Eine Detailseite, auf der Informationen zu einem bestimmten Musikalbum angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="0e1f3-241">Im Rahmen dieser Übung geben diese Aktionen jetzt einfach eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="0e1f3-242">Aufgabe 1: Hinzufügen einer neuen StoreController-Klasse</span><span class="sxs-lookup"><span data-stu-id="0e1f3-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="0e1f3-243">In dieser Aufgabe fügen Sie einen neuen Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="0e1f3-244">Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="0e1f3-245">Klicken Sie im Menü **Datei** auf **Projekt öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="0e1f3-246">Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex02-kreatingacontroller\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="0e1f3-247">Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="0e1f3-248">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0e1f3-249">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0e1f3-250">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0e1f3-251">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0e1f3-252">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0e1f3-253">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0e1f3-254">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0e1f3-255">Fügen Sie den neuen Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-255">Add the new controller.</span></span> <span data-ttu-id="0e1f3-256">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Controllers** innerhalb der Projektmappen-Explorer, und wählen Sie **Hinzufügen** und dann den **Controller** -Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="0e1f3-257">Ändern Sie den **Controller Namen** in *StoreController*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="0e1f3-258">![Controller Dialog hinzufügen](aspnet-mvc-4-fundamentals/_static/image8.png "Controller Dialog hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="0e1f3-259">*Controller Dialog hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="0e1f3-260">Aufgabe 2: Ändern der StoreController-Aktionen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="0e1f3-261">In dieser Aufgabe ändern Sie die Controller Methoden, die als **Aktionen**bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="0e1f3-262">Aktionen sind für die Verarbeitung von URL-Anforderungen und das Ermitteln des Inhalts zuständig, der an den Browser oder den Benutzer gesendet werden soll, der die URL aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="0e1f3-263">Die **StoreController** -Klasse verfügt bereits über eine **Index** -Methode.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="0e1f3-264">Sie werden Sie später in dieser Übungseinheit verwenden, um die Seite zu implementieren, die alle Genres des Musik Stores auflistet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="0e1f3-265">Ersetzen Sie vorerst einfach die **Index** -Methode durch den folgenden Code, der eine Zeichenfolge &quot;Hello from Store. Index ()&quot;zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="0e1f3-266">(Code Ausschnitt- *ASP.NET MVC 4-Grundlagen-EX2 StoreController-Index*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="0e1f3-267">Fügen Sie **Browse** -und **Details** -Methoden hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="0e1f3-268">Fügen Sie zu diesem Zweck den folgenden Code in den **StoreController**ein:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="0e1f3-269">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX2 StoreController browseanddetails*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="0e1f3-270">Aufgabe 3: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="0e1f3-271">In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="0e1f3-272">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-273">Das Projekt wird auf der **Start** Seite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="0e1f3-274">Ändern Sie die URL, um die Implementierung der einzelnen Aktionen zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="0e1f3-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-275">**/Store**.</span></span> <span data-ttu-id="0e1f3-276">**&quot;Hello from Store. Index ()-&quot;** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="0e1f3-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-277">**/Store/Browse**.</span></span> <span data-ttu-id="0e1f3-278">**&quot;Hello from Store. Browse ()-&quot;** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="0e1f3-279">**/Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-279">**/Store/Details**.</span></span> <span data-ttu-id="0e1f3-280">**&quot;Hello from Store. Details ()-&quot;** wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="0e1f3-281">![Durchsuchen von storebrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Durchsuchen von storebrowse")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="0e1f3-282">*Durchsuchen/Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="0e1f3-283">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="0e1f3-284">Übung 3: übergeben von Parametern an einen Controller</span><span class="sxs-lookup"><span data-stu-id="0e1f3-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="0e1f3-285">Bis jetzt haben Sie Konstante Zeichen folgen von den Controllern zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="0e1f3-286">In dieser Übung erfahren Sie, wie Sie Parameter mithilfe der URL und der QueryString an einen Controller übergeben und dann die Methoden Aktionen mit Text auf den Browser reagieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="0e1f3-287">Aufgabe 1: Hinzufügen von Genre Parametern zu StoreController</span><span class="sxs-lookup"><span data-stu-id="0e1f3-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="0e1f3-288">In dieser Aufgabe verwenden Sie die **Abfrage Zeichenfolge** , um Parameter an die Methode zum **Durchsuchen** von Aktionen in **StoreController**zu senden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="0e1f3-289">Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="0e1f3-290">Klicken Sie im Menü **Datei** auf **Projekt öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="0e1f3-291">Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex03-passingparameterstoacontroller\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="0e1f3-292">Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="0e1f3-293">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0e1f3-294">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0e1f3-295">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0e1f3-296">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0e1f3-297">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0e1f3-298">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0e1f3-299">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0e1f3-300">Öffnen Sie die **StoreController** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-300">Open **StoreController** class.</span></span> <span data-ttu-id="0e1f3-301">Erweitern Sie dazu im **Projektmappen-Explorer**den Ordner **Controller** , und doppelklicken Sie auf **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="0e1f3-302">Ändern Sie die **Browse** -Methode, und fügen Sie einen Zeichen folgen Parameter hinzu, um einen bestimmten Genre anzufordern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="0e1f3-303">ASP.NET MVC übergibt automatisch alle QueryString-oder Formular Bereitstellungs Parameter mit dem Namen **Genre** an diese Aktionsmethode, wenn Sie aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="0e1f3-304">Ersetzen Sie hierzu die **Browse** -Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="0e1f3-305">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX3 StoreController browsemethod*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="0e1f3-306">Sie verwenden die hilfsprogrammmethode **HttpUtility. HtmlEncode** , um zu verhindern, dass Benutzer JavaScript in der Ansicht mit einem Link wie **/Store/Browse einfügen? Genre =&lt;Skript&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/Script&gt;** .</span><span class="sxs-lookup"><span data-stu-id="0e1f3-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="0e1f3-307">Weitere Erläuterungen finden Sie in [diesem MSDN-Artikel](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="0e1f3-308">Aufgabe 2: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="0e1f3-309">In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser und verwenden den **Genre** -Parameter.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="0e1f3-310">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-311">Das Projekt wird auf der **Start** Seite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="0e1f3-312">Ändern Sie die URL in */Store/Browse? Genre = Disco* , um zu überprüfen, ob die Aktion den Genre-Parameter empfängt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="0e1f3-313">![Durchsuchen von storebrowsergenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Durchsuchen von storebrowsergenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="0e1f3-314">*/Store/Browse durchsuchen? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="0e1f3-315">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="0e1f3-316">Aufgabe 3: Hinzufügen eines in die URL eingebetteten ID-Parameters</span><span class="sxs-lookup"><span data-stu-id="0e1f3-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="0e1f3-317">In dieser Aufgabe verwenden Sie die **URL** , um einen **ID** -Parameter an die **Details** -Aktionsmethode von **StoreController**zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="0e1f3-318">Die Standard Routing Konvention von ASP.NET MVC besteht darin, das Segment einer URL nach dem Namen der Aktionsmethode als Parameter mit dem Namen **ID**zu behandeln. Wenn die Aktionsmethode einen Parameter mit dem Namen ID hat, übergibt ASP.NET MVC automatisch das URL-Segment als Parameter an Sie.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="0e1f3-319">In URL **Store/Details/5**wird die **ID** als **5**interpretiert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="0e1f3-320">Ändern Sie die **Details** -Methode von **StoreController**, und fügen Sie einen **int** -Parameter mit dem Namen **ID**hinzu. Ersetzen Sie **hierzu die Details** -Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="0e1f3-321">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX3 StoreController detailsmethod*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="0e1f3-322">Aufgabe 4: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="0e1f3-323">In dieser Aufgabe testen Sie die Anwendung in einem Webbrowser und verwenden den **ID** -Parameter.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="0e1f3-324">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-325">Das Projekt wird auf der **Start** Seite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="0e1f3-326">Ändern Sie die URL in */Store/Details/5* , um sicherzustellen, dass die Aktion den ID-Parameter erhält.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="0e1f3-327">![Durchsuchen StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Durchsuchen StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="0e1f3-328">*Durchsuchen/Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="0e1f3-329">Übung 4: Erstellen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="0e1f3-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="0e1f3-330">Bisher haben Sie Zeichen folgen aus Controller Aktionen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="0e1f3-331">Obwohl es sich um ein nützliches Verfahren zum Verständnis der Funktionsweise von Controllern handelt, ist es nicht, wie ihre realen Webanwendungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="0e1f3-332">Sichten sind Komponenten, die einen besseren Ansatz zum Erstellen von HTML-Code zum Browser mit Vorlagen Dateien bieten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="0e1f3-333">In dieser Übung erfahren Sie, wie Sie eine layoutmasterseite hinzufügen, um eine Vorlage für gemeinsamen HTML-Inhalt einzurichten, ein Stylesheet, um das Aussehen und Verhalten der Site zu verbessern, und schließlich eine Ansichts Vorlage, mit der HomeController HTML zurückgeben kann.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a><span data-ttu-id="0e1f3-334">Aufgabe 1: Ändern der Datei \_"Layout. cshtml"</span><span class="sxs-lookup"><span data-stu-id="0e1f3-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="0e1f3-335">Mit der Datei **~/views/Shared/\_Layout. cshtml** können Sie eine Vorlage für gemeinsamen HTML-Code für die gesamte Website einrichten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="0e1f3-336">In dieser Aufgabe fügen Sie eine layoutmasterseite mit einem gemeinsamen Header mit Links zur Startseite und zum Speicherbereich hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="0e1f3-337">Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="0e1f3-338">Klicken Sie im Menü **Datei** auf **Projekt öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="0e1f3-339">Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex04-anatingaview\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="0e1f3-340">Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="0e1f3-341">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0e1f3-342">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0e1f3-343">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0e1f3-344">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0e1f3-345">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0e1f3-346">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0e1f3-347">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0e1f3-348">Die Datei <strong>\_"Layout. cshtml</strong> " enthält das HTML-Container Layout für alle Seiten auf der Website.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="0e1f3-349">Sie enthält das <strong>&lt;HTML-&gt;</strong> -Element für die HTML-Antwort sowie die <strong>&lt;Head-&gt;</strong> und <strong>&lt;Body&gt;</strong> -Elemente.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="0e1f3-350">mit <strong>@RenderBody()</strong> im HTML-Text werden Bereiche identifiziert, die Ansichts Vorlagen mit dynamischem Inhalt ausfüllen können.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="0e1f3-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="0e1f3-352">Fügen Sie eine gemeinsame Kopfzeile mit Links zur Startseite und zum Speicherbereich auf allen Seiten der Website hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="0e1f3-353">Fügen Sie dazu den folgenden Code unterhalb &lt;Body&gt;-Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="0e1f3-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="0e1f3-355">Fügen Sie ein div-ein, um den Textabschnitt jeder Seite zu Rendering.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="0e1f3-356">Ersetzen Sie <strong>@RenderBody()</strong> durch den folgenden hervorgehobenen CodeC#: ()</span><span class="sxs-lookup"><span data-stu-id="0e1f3-356">Replace <strong>@RenderBody()</strong> with the following highlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="0e1f3-357">Wussten Sie schon?</span><span class="sxs-lookup"><span data-stu-id="0e1f3-357">Did you know?</span></span> <span data-ttu-id="0e1f3-358">Visual Studio 2012 verfügt über Ausschnitte, mit denen Sie häufig verwendeten Code in HTML, Code Dateien und mehr hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="0e1f3-359">Probieren Sie es aus, indem Sie **&lt;div&gt;** eingeben und zweimal die **Tab** -Taste drücken, um ein umfassendes **div** -Tag einzufügen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="0e1f3-360">Aufgabe 2: Hinzufügen eines CSS-Stylesheets</span><span class="sxs-lookup"><span data-stu-id="0e1f3-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="0e1f3-361">Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die nur Stile enthält, die zum Anzeigen von grundlegenden Formularen und Validierungs Nachrichten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="0e1f3-362">Sie verwenden zusätzliche CSS und Images (die möglicherweise von einem Designer bereitgestellt werden), um das Erscheinungsbild der Website zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="0e1f3-363">In dieser Aufgabe fügen Sie ein CSS-Stylesheet hinzu, um die Stile der Site zu definieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="0e1f3-364">Die CSS-Datei und die Images sind im Ordner **source\assets\content** dieses Labs enthalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="0e1f3-365">Um Sie der Anwendung hinzuzufügen, ziehen Sie Ihren Inhalt aus einem **Windows-Explorer** -Fenster in Visual Studio Express für **Projektmappen-Explorer** das Web, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="0e1f3-366">![Ziehen von Stil Inhalt](aspnet-mvc-4-fundamentals/_static/image12.png "Ziehen von Stil Inhalt")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="0e1f3-367">*Ziehen von Stil Inhalt*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="0e1f3-368">Es wird ein Warn Dialogfeld angezeigt, in dem Sie aufgefordert werden, die Datei " **Site. CSS** " und einige vorhandene Images zu ersetzen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="0e1f3-369">Aktivieren Sie **für alle Elemente** übernehmen, und klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="0e1f3-370">Aufgabe 3: Hinzufügen einer Ansichts Vorlage</span><span class="sxs-lookup"><span data-stu-id="0e1f3-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="0e1f3-371">In dieser Aufgabe fügen Sie eine Ansichts Vorlage hinzu, um die HTML-Antwort zu generieren, die die layoutmasterseite und das in dieser Übung hinzugefügte CSS verwendet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="0e1f3-372">Wenn Sie beim Durchsuchen der Homepage der Website eine Ansichts Vorlage verwenden möchten, müssen Sie zunächst angeben, dass anstelle einer Zeichenfolge die Methode **HomeController Index** eine **Ansicht**zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="0e1f3-373">Öffnen Sie die **HomeController** -Klasse, und ändern Sie Ihre **Index** -Methode, um ein **Aktions Ergebnis**zurückzugeben, und lassen Sie **View ()** zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="0e1f3-374">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="0e1f3-375">Nun müssen Sie eine geeignete Ansichts Vorlage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="0e1f3-376">**Klicken Sie** dazu mit der rechten Maustaste in die **Index** Aktionsmethode, und wählen Sie **Ansicht hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="0e1f3-377">Dadurch wird das Dialog **Feld Ansicht hinzufügen** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="0e1f3-378">![Hinzufügen einer Sicht aus der Index-Methode](aspnet-mvc-4-fundamentals/_static/image13.png "Hinzufügen einer Sicht aus der Index-Methode")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="0e1f3-379">*Hinzufügen einer Sicht aus der Index-Methode*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="0e1f3-380">Das Dialog **Feld Ansicht hinzufügen** wird angezeigt, um eine Ansichts Vorlagen Datei zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="0e1f3-381">In diesem Dialogfeld wird standardmäßig der Name der Ansichts Vorlage aufgefüllt, sodass er mit der Aktionsmethode übereinstimmt, von der er verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="0e1f3-382">Da Sie im HomeController das Kontextmenü **Ansicht hinzufügen** innerhalb der **Index** Aktionsmethode verwendet haben, enthält das Dialog **Feld Ansicht hinzufügen** einen Index als Standard Ansichts Namen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="0e1f3-383">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-383">Click **Add**.</span></span>

    <span data-ttu-id="0e1f3-384">![Sicht Ansicht hinzufügen](aspnet-mvc-4-fundamentals/_static/image14.png "Sicht Ansicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="0e1f3-385">*Sicht Ansicht hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="0e1f3-386">Visual Studio generiert eine **Index. cshtml** -Ansichts Vorlage im Ordner " **views\home** " und öffnet diese.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="0e1f3-387">![Start Index Ansicht erstellt](aspnet-mvc-4-fundamentals/_static/image15.png "Start Index Ansicht erstellt")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="0e1f3-388">*Start Index Ansicht erstellt*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-389">Name und Speicherort der Datei " **Index. cshtml** " sind relevant und folgen den standardmäßigen ASP.NET-MVC-Benennungs Konventionen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="0e1f3-390">Der Ordner \views\**Home*\* entspricht dem Controller Namen (**Home** Controller).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-390">The folder \Views\**Home*\* matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="0e1f3-391">Der Ansichts Vorlagen Name (**Index**) entspricht der Controller Aktionsmethode, die die Ansicht anzeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="0e1f3-392">Auf diese Weise kann ASP.NET MVC den Namen oder den Speicherort einer Ansichts Vorlage nicht explizit angeben, wenn diese Benennungs Konvention verwendet wird, um eine Ansicht zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="0e1f3-393">Die generierte Ansichts Vorlage basiert auf der zuvor definierten **\_Layout. cshtml** -Vorlage.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="0e1f3-394">Aktualisieren Sie die viewbag. Title-Eigenschaft auf **Home**, und ändern Sie den Hauptinhalt in **die Startseite**, wie im folgenden Code gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="0e1f3-395">Wählen Sie im Projektmappen-Explorer **mvcmusicstore** -Projekt aus, und drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="0e1f3-396">Aufgabe 4: Überprüfung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-396">Task 4: Verification</span></span>

<span data-ttu-id="0e1f3-397">Um sicherzustellen, dass Sie alle Schritte in der vorherigen Übung ordnungsgemäß ausgeführt haben, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="0e1f3-398">Wenn die Anwendung in einem Browser geöffnet ist, sollten Sie Folgendes beachten:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="0e1f3-399">Die Index Aktionsmethode von HomeController wurde gefunden und zeigt die Ansichts Vorlage **\views\home\index.cshtml** an, obwohl der Code **Return View ()** aufgerufen hat, da die Ansichts Vorlage der Standard Benennungs Konvention folgte.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="0e1f3-400">Auf der Startseite wird die Begrüßungsnachricht angezeigt, die in der Ansichts Vorlage " **\views\home\index.cshtml** " definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="0e1f3-401">Auf der Startseite wird die Vorlage " **\_Layout. cshtml** " verwendet, sodass die Willkommensnachricht im HTML-Layout der Standard Website enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="0e1f3-402">![Start Index Ansicht mit dem definierten layoutpage-und Style-Stil](aspnet-mvc-4-fundamentals/_static/image16.png "Start Index Ansicht mit dem definierten layoutpage-und Style-Stil")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="0e1f3-403">*Start Index Ansicht mit dem definierten layoutpage-und Style-Stil*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="0e1f3-404">Übung 5: Erstellen eines Ansichts Modells</span><span class="sxs-lookup"><span data-stu-id="0e1f3-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="0e1f3-405">Bisher haben Sie in ihren Ansichten hart codiertes HTML angezeigt, aber um dynamische Webanwendungen zu erstellen, sollte die Ansichts Vorlage Informationen vom Controller erhalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="0e1f3-406">Ein gängiges Verfahren, das für diesen Zweck verwendet wird, ist das **ViewModel** -Muster, das es einem Controller ermöglicht, alle Informationen zu verpacken, die zum Generieren der entsprechenden HTML-Antwort erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="0e1f3-407">In dieser Übung erfahren Sie, wie Sie eine ViewModel-Klasse erstellen und die erforderlichen Eigenschaften hinzufügen: die Anzahl der Genres im Speicher und eine Liste dieser Genres.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="0e1f3-408">Außerdem wird StoreController so aktualisiert, dass das erstellte ViewModel verwendet wird, und schließlich wird eine neue Ansichts Vorlage erstellt, in der die erwähnten Eigenschaften auf der Seite angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="0e1f3-409">Aufgabe 1: Erstellen einer ViewModel-Klasse</span><span class="sxs-lookup"><span data-stu-id="0e1f3-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="0e1f3-410">In dieser Aufgabe erstellen Sie eine ViewModel-Klasse, mit der das Szenario für die Speicher Genre Auflistung implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="0e1f3-411">Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="0e1f3-412">Klicken Sie im Menü **Datei** auf **Projekt öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="0e1f3-413">Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex05-kreatingaviewmodel\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="0e1f3-414">Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="0e1f3-415">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0e1f3-416">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0e1f3-417">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0e1f3-418">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0e1f3-419">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0e1f3-420">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0e1f3-421">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0e1f3-422">Erstellen Sie einen **ViewModels** -Ordner für das ViewModel.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="0e1f3-423">Klicken Sie hierzu mit der rechten Maustaste auf das **mvcmusicstore** -Projekt der obersten Ebene, und wählen Sie **Hinzufügen** und dann **neuer Ordner**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="0e1f3-424">![Hinzufügen eines neuen Ordners](aspnet-mvc-4-fundamentals/_static/image17.png "Hinzufügen eines neuen Ordners")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="0e1f3-425">*Hinzufügen eines neuen Ordners*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="0e1f3-426">Nennen Sie den Ordner *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="0e1f3-427">![Ordner "ViewModels" in Projektmappen-Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "Ordner "ViewModels" in Projektmappen-Explorer")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="0e1f3-428">*Ordner "ViewModels" in Projektmappen-Explorer*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="0e1f3-429">Erstellen Sie eine **ViewModel** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="0e1f3-430">Klicken Sie hierzu mit der rechten Maustaste auf den zuletzt erstellten Ordner " **ViewModels** ", und wählen Sie **Hinzufügen** und dann **Neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="0e1f3-431">Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *StoreIndexViewModel.cs*, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="0e1f3-432">![Hinzufügen einer neuen Klasse](aspnet-mvc-4-fundamentals/_static/image19.png "Hinzufügen einer neuen Klasse")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="0e1f3-433">*Hinzufügen einer neuen Klasse*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-433">*Adding a new Class*</span></span>

    <span data-ttu-id="0e1f3-434">![Erstellen der storeindexviewmodel-Klasse](aspnet-mvc-4-fundamentals/_static/image20.png "Erstellen der storeindexviewmodel-Klasse")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="0e1f3-435">*Erstellen der storeindexviewmodel-Klasse*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="0e1f3-436">Aufgabe 2: Hinzufügen von Eigenschaften zur ViewModel-Klasse</span><span class="sxs-lookup"><span data-stu-id="0e1f3-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="0e1f3-437">Es gibt zwei Parameter, die von StoreController an die Ansichts Vorlage übermittelt werden müssen, um die erwartete HTML-Antwort zu generieren: die Anzahl der Genres im Speicher und eine Liste dieser Genres.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="0e1f3-438">In dieser Aufgabe fügen Sie diese beiden Eigenschaften der **storeindexviewmodel** -Klasse hinzu: " **numofgenres** (a Integer)" und " **Genres** " (eine Liste von Zeichen folgen).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="0e1f3-439">Fügen Sie die Eigenschaften " **numofgenres** " und " **Genres** " der **storeindexviewmodel** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="0e1f3-440">Fügen Sie zu diesem Zweck der Klassendefinition die folgenden zwei Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="0e1f3-441">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX5 storeindexviewmodel-Eigenschaften*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="0e1f3-442">Die **{Get; Set;}** -Notation verwendet die C#Funktion für automatisch implementierte Eigenschaften von.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="0e1f3-443">Er bietet die Vorteile einer Eigenschaft, ohne dass wir ein dahinter liegendes Feld deklarieren müssen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="0e1f3-444">Aufgabe 3: Aktualisieren von StoreController zur Verwendung von storeindexviewmodel</span><span class="sxs-lookup"><span data-stu-id="0e1f3-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="0e1f3-445">Die **storeindexviewmodel** -Klasse kapselt die Informationen, die erforderlich sind, um die **Index** Methode von **StoreController**an eine Ansichts Vorlage zu übergeben, um eine Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="0e1f3-446">In dieser Aufgabe aktualisieren Sie **StoreController** , damit das **storeindexviewmodel**verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="0e1f3-447">Öffnen Sie die **StoreController** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="0e1f3-448">![StoreController-Klasse wird geöffnet](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController-Klasse wird geöffnet")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="0e1f3-449">*StoreController-Klasse wird geöffnet*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="0e1f3-450">Fügen Sie am Anfang des **StoreController** -Codes den folgenden Namespace hinzu, um die **storeindexviewmodel** -Klasse aus dem **StoreController**zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="0e1f3-451">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX5 storeindexviewmodel mithilfe von ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="0e1f3-452">Ändern Sie die **Index** Aktionsmethode von **StoreController**so, dass Sie ein **storeindexviewmodel** -Objekt erstellt und auffüllt und es dann an eine Ansichts Vorlage übergibt, um eine HTML-Antwort damit zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-453">In Lab &quot;ASP.NET MVC-Modellen und Datenzugriff&quot; Sie Code schreiben, der die Liste der Speicher-Genres aus einer Datenbank abruft.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="0e1f3-454">Im folgenden Code erstellen Sie eine **Liste** von dummydatengenres, die das **storeindexviewmodel**auffüllen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="0e1f3-455">Nachdem das **storeindexviewmodel** -Objekt erstellt und eingerichtet wurde, wird es als Argument an die **View** -Methode übermittelt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="0e1f3-456">Dies gibt an, dass die Ansichts Vorlage dieses Objekt verwendet, um eine HTML-Antwort mit ihr zu generieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="0e1f3-457">Ersetzen Sie die **Index** -Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="0e1f3-458">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-EX5 StoreController Index-Methode*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="0e1f3-459">Wenn Sie mit nicht vertraut C#sind, können Sie davon ausgehen, dass die Verwendung von **var** bedeutet, dass die **ViewModel** -Variable spät gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="0e1f3-460">Das ist nicht korrekt: der C# Compiler verwendet den Typrückschluss basierend auf dem, was Sie der Variablen zuweisen, um zu bestimmen, dass **ViewModel** den Typ " **storeindexviewmodel**" hat.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="0e1f3-461">Außerdem erhalten Sie durch Kompilieren der lokalen **ViewModel** -Variablen als **storeindexviewmodel** -Typ eine Kompilierzeit Überprüfung und Visual Studio Code-Editor-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="0e1f3-462">Aufgabe 4: Erstellen einer Ansichts Vorlage, die storeindexviewmodel verwendet</span><span class="sxs-lookup"><span data-stu-id="0e1f3-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="0e1f3-463">In dieser Aufgabe erstellen Sie eine Ansichts Vorlage, die ein storeindexviewmodel-Objekt verwendet, das vom Controller zum Anzeigen einer Liste von Genres übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="0e1f3-464">Vor dem Erstellen der neuen Ansichts Vorlage erstellen wir das Projekt, damit das **Dialog Feld "Ansicht hinzufügen** " die Klasse " **storeindexviewmodel** " kennt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="0e1f3-465">Erstellen Sie das Projekt, indem Sie das Menü Element **Build** auswählen und dann **mvcmusicstore erstellen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="0e1f3-466">![Projekt wird aufgebaut](aspnet-mvc-4-fundamentals/_static/image22.png "Projekt wird aufgebaut")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="0e1f3-467">*Projekt wird aufgebaut*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-467">*Building the project*</span></span>
2. <span data-ttu-id="0e1f3-468">Erstellen Sie eine neue Ansichts Vorlage.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-468">Create a new View template.</span></span> <span data-ttu-id="0e1f3-469">Klicken Sie dazu mit der rechten Maustaste in die **Index** -Methode, und wählen Sie **Ansicht hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="0e1f3-470">![Hinzufügen einer Ansicht](aspnet-mvc-4-fundamentals/_static/image23.png "Hinzufügen einer Ansicht")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="0e1f3-471">*Hinzufügen einer Ansicht*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-471">*Adding a View*</span></span>
3. <span data-ttu-id="0e1f3-472">Da das **Dialog Feld Ansicht hinzufügen** von **StoreController**aufgerufen wurde, wird die Ansichts Vorlage standardmäßig in der Datei " **\views\store\index.cshtml** " hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="0e1f3-473">Aktivieren Sie das Kontrollkästchen **eine stark typisierte Ansicht erstellen** , und wählen Sie dann **storeindexviewmodel** als **Modell Klasse**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="0e1f3-474">Stellen Sie außerdem sicher, dass die ausgewählte Ansichts-Engine **Razor**ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="0e1f3-475">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-475">Click **Add**.</span></span>

    <span data-ttu-id="0e1f3-476">![Sicht Ansicht hinzufügen](aspnet-mvc-4-fundamentals/_static/image24.png "Sicht Ansicht hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="0e1f3-477">*Sicht Ansicht hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-477">*Add View Dialog*</span></span>

    <span data-ttu-id="0e1f3-478">Die Ansichts Vorlagen Datei " **\views\store\index.cshtml** " wird erstellt und geöffnet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="0e1f3-479">Basierend auf den Informationen, die im Dialog **Feld Ansicht hinzufügen** im letzten Schritt bereitgestellt wurden, erwartet die Ansichts Vorlage eine **storeindexviewmodel** -Instanz als Daten, die zum Generieren einer HTML-Antwort verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="0e1f3-480">Sie werden feststellen, dass die Vorlage eine `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#erbt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="0e1f3-481">Aufgabe 5: Aktualisieren der Ansichts Vorlage</span><span class="sxs-lookup"><span data-stu-id="0e1f3-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="0e1f3-482">In dieser Aufgabe aktualisieren Sie die Ansichts Vorlage, die in der letzten Aufgabe erstellt wurde, um die Anzahl der Genres und deren Namen innerhalb der Seite abzurufen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="0e1f3-483">Zum Ausführen von Code in der Ansichts Vorlage verwenden Sie @ Syntax (häufig als &quot;Code-Nuggets&quot;bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="0e1f3-484">Ersetzen Sie in der Datei " **Index. cshtml** " im Ordner " **Store** " den Code durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="0e1f3-485">Führen Sie eine Schleife für die Genre Liste in **storeindexviewmodel** aus, und erstellen Sie mithilfe einer **foreach** -Schleife eine HTML- **&lt;UL&gt;** Liste.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="0e1f3-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="0e1f3-487">Drücken Sie **F5** , um die Anwendung auszuführen und **/Store**zu durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="0e1f3-488">Die Liste der Genres, die im **storeindexviewmodel** -Objekt von **StoreController** an die Ansichts Vorlage übermittelt werden, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="0e1f3-489">![Anzeigen einer Liste von Genres](aspnet-mvc-4-fundamentals/_static/image26.png "Anzeigen einer Liste von Genres")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="0e1f3-490">*Anzeigen einer Liste von Genres*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="0e1f3-491">Schließen Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="0e1f3-492">Übung 6: Verwenden von Parametern in der Ansicht</span><span class="sxs-lookup"><span data-stu-id="0e1f3-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="0e1f3-493">In Übung 3 haben Sie gelernt, wie Sie Parameter an den Controller übergeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="0e1f3-494">In dieser Übung erfahren Sie, wie diese Parameter in der Ansichts Vorlage verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="0e1f3-495">Zu diesem Zweck werden Sie zunächst in Modellklassen eingeführt, die Ihnen helfen, Ihre Daten und Domänen Logik zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="0e1f3-496">Darüber hinaus erfahren Sie, wie Sie Links zu Seiten innerhalb der ASP.NET MVC-Anwendung erstellen, ohne sich Gedanken über die Codierung von URL-Pfaden machen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="0e1f3-497">Aufgabe 1: Hinzufügen von Modellklassen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="0e1f3-498">Anders als bei ViewModels, die nur zum Übergeben von Informationen vom Controller an die Ansicht erstellt werden, werden Modellklassen erstellt, um Daten und Domänen Logik zu speichern und zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="0e1f3-499">In dieser Aufgabe fügen Sie zwei Modellklassen hinzu, die diese Konzepte darstellen: **Genre** und **Album**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="0e1f3-500">Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="0e1f3-501">Klicken Sie im Menü **Datei** auf **Projekt öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="0e1f3-502">Navigieren Sie im Dialogfeld Projekt öffnen zu **source\ex06-usingparametersinview\begin**, wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="0e1f3-503">Alternativ können Sie mit der Lösung fortfahren, die Sie nach Abschluss der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="0e1f3-504">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="0e1f3-505">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="0e1f3-506">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="0e1f3-507">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="0e1f3-508">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="0e1f3-509">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="0e1f3-510">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="0e1f3-511">Fügen Sie eine **Genre** Modell Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="0e1f3-512">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Models** im **Projektmappen-Explorer**, und wählen Sie **Hinzufügen** und dann die Option **Neues Element** aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="0e1f3-513">Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *Genre.cs*, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="0e1f3-514">![Hinzufügen einer Klasse](aspnet-mvc-4-fundamentals/_static/image27.png "Hinzufügen einer Klasse")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="0e1f3-515">*Hinzufügen eines neuen Elements*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-515">*Adding a new item*</span></span>

    <span data-ttu-id="0e1f3-516">![Genre Modell Klasse hinzufügen](aspnet-mvc-4-fundamentals/_static/image28.png "Genre Modell Klasse hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="0e1f3-517">*Genre Modell Klasse hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="0e1f3-518">Fügen Sie der Genre Klasse eine **Name** -Eigenschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="0e1f3-519">Fügen Sie zu diesem Zweck den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-519">To do this, add the following code:</span></span>

    <span data-ttu-id="0e1f3-520">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="0e1f3-521">Fügen Sie der gleichen Prozedur wie zuvor eine **Album** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="0e1f3-522">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **Models** im **Projektmappen-Explorer**, und wählen Sie **Hinzufügen** und dann die Option **Neues Element** aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="0e1f3-523">Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *Album.cs*, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="0e1f3-524">Fügen Sie der Album-Klasse zwei Eigenschaften hinzu: **Genre** und **Title**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="0e1f3-525">Fügen Sie zu diesem Zweck den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-525">To do this, add the following code:</span></span>

    <span data-ttu-id="0e1f3-526">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="0e1f3-527">Aufgabe 2: Hinzufügen eines storebrow\viewmodel</span><span class="sxs-lookup"><span data-stu-id="0e1f3-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="0e1f3-528">In dieser Aufgabe wird ein **storebrowsviewmodel** verwendet, um die mit einem ausgewählten Genre abgleichen Alben anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="0e1f3-529">In dieser Aufgabe erstellen Sie diese Klasse und fügen dann zwei Eigenschaften hinzu, um das **Genre** und seine **Album**Liste zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="0e1f3-530">Fügen Sie eine **storebrowsviewmodel** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="0e1f3-531">Klicken Sie dazu mit der rechten Maustaste auf den Ordner **ViewModels** im **Projektmappen-Explorer**, und wählen Sie **Hinzufügen** und dann die Option **Neues Element** aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="0e1f3-532">Wählen Sie unter **Code**das **Klassen** Element aus, benennen Sie die Datei *StoreBrowseViewModel.cs*, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="0e1f3-533">Fügen Sie einen Verweis auf die Modelle in der **storebrowsviewmodel** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="0e1f3-534">Fügen Sie zu diesem Zweck den folgenden using-Namespace hinzu:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="0e1f3-535">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 usingmodel*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="0e1f3-536">Fügen Sie zwei Eigenschaften zur **storebrowsviewmodel** -Klasse hinzu: **Genre** und **Alben**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="0e1f3-537">Fügen Sie zu diesem Zweck den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-537">To do this, add the following code:</span></span>

    <span data-ttu-id="0e1f3-538">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 modelproperties*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="0e1f3-539">Was ist **List&lt;Album&gt;** ?: in dieser Definition wird die **List&lt;t&gt;** Type verwendet, wobei **t** den Typ einschränkt, zu dem Elemente dieser **Liste** gehören, in diesem Fall " **Album** " (oder einem seiner Nachfolger).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="0e1f3-540">Diese Fähigkeit zum Entwerfen von Klassen und Methoden, die die Angabe eines oder mehrerer Typen verzögern, bis die Klasse oder Methode von Client Code deklariert und instanziiert wird, ist eine C# Funktion der Sprache **Generika**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="0e1f3-541">**List&lt;t&gt;** ist die generische Entsprechung des **ArrayList** -Typs und steht im **System. Collections. Generic** -Namespace zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="0e1f3-542">Einer der Vorteile bei der Verwendung von **Generika** besteht darin, dass Sie bei der Angabe des Typs keine typüberprüfungs Vorgänge durchführen müssen, wie z. b. das Umwandeln der Elemente in ein **Album** wie bei einer **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="0e1f3-543">Aufgabe 3: Verwenden des neuen ViewModel in StoreController</span><span class="sxs-lookup"><span data-stu-id="0e1f3-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="0e1f3-544">In dieser Aufgabe ändern Sie die Aktionsmethoden " **Browse** " und " **Details** " von **StoreController**, um das neue " **storebrowsseviewmodel**" zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="0e1f3-545">Fügen Sie einen Verweis auf den Ordner Models in der **StoreController** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="0e1f3-546">Erweitern Sie hierzu den Ordner **Controllers** in der **Projektmappen-Explorer** , und öffnen Sie die **StoreController** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="0e1f3-547">Fügen Sie dann den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-547">Then add the following code:</span></span>

    <span data-ttu-id="0e1f3-548">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 usingmodelincontroller*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="0e1f3-549">Ersetzen Sie die **Browse** -Aktionsmethode, um die **storeviewbrowsecontroller** -Klasse zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="0e1f3-550">Sie erstellen ein Genre und zwei neue Alben-Objekte mit Dummydaten (in der nächsten praktischen Übungseinheit verwenden Sie echte Daten aus einer Datenbank).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="0e1f3-551">Ersetzen Sie hierzu die **Browse** -Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="0e1f3-552">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 browsemethod*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="0e1f3-553">Ersetzen Sie die " **Details** "-Aktionsmethode zur Verwendung der **storeviewbrowsecontroller** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="0e1f3-554">Sie erstellen ein neues **Album** -Objekt, das an die **Ansicht**zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="0e1f3-555">Ersetzen Sie **hierzu die Details** -Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="0e1f3-556">(Code Ausschnitt- *ASP.NET MVC 4 Fundamentals-Ex6 detailsmethod*)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="0e1f3-557">Aufgabe 4: Hinzufügen einer Vorlage zum Durchsuchen von Ansichten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="0e1f3-558">In dieser Aufgabe fügen Sie eine **browseinsicht** hinzu, um die für ein bestimmtes Genre gefundenen Alben anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="0e1f3-559">Vor dem Erstellen der neuen Ansichts Vorlage sollten Sie das Projekt so erstellen, dass das Dialog **Feld "Ansicht hinzufügen** " die zu verwendende **ViewModel** -Klasse kennt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="0e1f3-560">Erstellen Sie das Projekt, indem Sie das Menü Element **Build** auswählen und dann **mvcmusicstore erstellen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="0e1f3-561">Fügen Sie eine **browseinsicht** hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-561">Add a **Browse** View.</span></span> <span data-ttu-id="0e1f3-562">Klicken Sie hierzu mit der rechten Maustaste auf die Aktion zum **Durchsuchen** von **StoreController** , und klicken Sie auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="0e1f3-563">Überprüfen Sie im Dialogfeld **Ansicht hinzufügen** , ob der Ansichts Name **Durchsuchen**angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="0e1f3-564">Aktivieren Sie das Kontrollkästchen für **eine stark typisierte Ansicht erstellen** , und wählen Sie in der Dropdown Liste **Modell Klasse** die Option **storebrowsviewmodel** aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="0e1f3-565">Belassen Sie die anderen Felder mit ihrem Standardwert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="0e1f3-566">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-566">Then click **Add**.</span></span>

    <span data-ttu-id="0e1f3-567">![Hinzufügen einer browseinsicht](aspnet-mvc-4-fundamentals/_static/image29.png "Hinzufügen einer browseinsicht")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="0e1f3-568">*Hinzufügen einer browseinsicht*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="0e1f3-569">Ändern Sie die Datei **Browse. cshtml** , um die Informationen des Genres anzuzeigen, indem Sie auf das **storebrowseviewmodel** -Objekt zugreifen, das an die Ansichts Vorlage übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="0e1f3-570">Ersetzen Sie dazu den Inhalt durch Folgendes: (C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="0e1f3-571">Aufgabe 5: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="0e1f3-572">In dieser Aufgabe testen Sie, ob die **Browse** -Methode Alben aus der Aktion "Methode **Durchsuchen** " abruft.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="0e1f3-573">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-574">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-574">The project starts in the Home page.</span></span> <span data-ttu-id="0e1f3-575">Ändern Sie die URL in **/Store/Browse? Genre = Disco** , um zu überprüfen, ob die Aktion zwei Alben zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="0e1f3-576">![Store-Disco-Alben durchsuchen](aspnet-mvc-4-fundamentals/_static/image30.png "Store-Disco-Alben durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="0e1f3-577">*Store-Disco-Alben durchsuchen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="0e1f3-578">Aufgabe 6: Anzeigen von Informationen zu einem bestimmten Album</span><span class="sxs-lookup"><span data-stu-id="0e1f3-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="0e1f3-579">In dieser Aufgabe implementieren Sie die Ansicht " **Store/Details** ", um Informationen zu einem bestimmten Album anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="0e1f3-580">In dieser praktischen Übungseinheit ist alles, was Sie über das Album anzeigen werden, bereits in der **Ansichts** Vorlage enthalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="0e1f3-581">Anstatt also eine **storedetailsviewmodel** -Klasse zu erstellen, verwenden Sie die aktuelle **storebrowsviewmodel** -Vorlage, die das Album übergibt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="0e1f3-582">Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="0e1f3-583">Fügen Sie eine neue **Detail** Ansicht für die **Details** Aktionsmethode von **StoreController**hinzu.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="0e1f3-584">Klicken Sie dazu mit der rechten Maustaste auf die **Detail** Methode in der **StoreController** -Klasse, und klicken Sie auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="0e1f3-585">Überprüfen **Sie im Dialogfeld Ansicht hinzufügen** , ob der **Name der Sicht** **Details**lautet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="0e1f3-586">Aktivieren Sie das Kontrollkästchen für **eine stark typisierte Ansicht erstellen** , und wählen Sie im Dropdown Feld **Modell Klasse** die Option **Album** aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="0e1f3-587">Belassen Sie die anderen Felder mit ihrem Standardwert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="0e1f3-588">Klicken Sie anschließend auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-588">Then click **Add**.</span></span> <span data-ttu-id="0e1f3-589">Dadurch wird die Datei " **\views\store\details.cshtml** " erstellt und geöffnet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="0e1f3-590">![Hinzufügen einer Detailansicht](aspnet-mvc-4-fundamentals/_static/image31.png "Hinzufügen einer Detailansicht")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="0e1f3-591">*Hinzufügen einer Detailansicht*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="0e1f3-592">Ändern Sie die Datei " **Details. cshtml** ", um die Informationen des Albums anzuzeigen, indem Sie auf das an die Ansichts Vorlage über gegebene **Album** Objekt zugreifen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="0e1f3-593">Ersetzen Sie dazu den Inhalt durch Folgendes: (C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="0e1f3-594">Aufgabe 7: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="0e1f3-595">In dieser Aufgabe testen Sie, ob in der **Detail** Ansicht die Informationen des Albums von der **Details-Aktions** Methode abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="0e1f3-596">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-597">Das Projekt wird auf der **Start** Seite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="0e1f3-598">Ändern Sie die URL in **/Store/Details/5** , um die Informationen des Albums zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="0e1f3-599">![Alben-Detail durchsuchen](aspnet-mvc-4-fundamentals/_static/image32.png "Alben-Detail durchsuchen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="0e1f3-600">*Details zum Browser Album*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="0e1f3-601">Aufgabe 8: Hinzufügen von Links zwischen Seiten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="0e1f3-602">In dieser Aufgabe fügen Sie einen Link in der Speicher Ansicht hinzu, um einen Link in jedem Genre Namen mit der entsprechenden **/Store/Browse** -URL zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="0e1f3-603">Auf diese Weise wird bei einem Klick auf ein Genre (z. a. **Disco**) zu **/Store/Browse? Genre = Disco** -URL navigiert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="0e1f3-604">Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="0e1f3-605">Aktualisieren Sie die **Index** Seite, um der Seite " **Durchsuchen** " einen Link hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="0e1f3-606">Zu diesem Zweck erweitern Sie im **Projektmappen-Explorer** den Ordner **Sichten** , und doppelklicken Sie auf die Seite **Index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="0e1f3-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="0e1f3-607">Fügen Sie der Such Ansicht einen Link hinzu, der angibt, dass das Genre ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="0e1f3-608">Ersetzen Sie hierzu den folgenden hervorgehobenen Code in den **&lt;Li&gt;** Tags: (C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="0e1f3-609">ein anderer Ansatz wäre eine direkte Verknüpfung mit der Seite mit einem Code wie dem folgenden:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="0e1f3-610">&lt;a href =&quot;/Store/Browse? Genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="0e1f3-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="0e1f3-611">Obwohl dieser Ansatz funktioniert, hängt er von einer hart codierten Zeichenfolge ab.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="0e1f3-612">Wenn Sie den Controller später umbenennen, müssen Sie diese Anweisung manuell ändern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="0e1f3-613">Eine bessere Alternative ist die Verwendung einer **HTML** -Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="0e1f3-614">ASP.NET MVC enthält eine HTML-Hilfsmethode, die für solche Aufgaben verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="0e1f3-615">Die Hilfsmethode **HTML. Action Link ()** vereinfacht die Erstellung von HTML- **&lt;einer&gt;** Links und stellt sicher, dass URL-Pfade ordnungsgemäß URL-codiert sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="0e1f3-616">HTML. Action Link verfügt über mehrere über Ladungen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-616">Html.ActionLink has several overloads.</span></span> <span data-ttu-id="0e1f3-617">In dieser Übung verwenden Sie eine, die drei Parameter annimmt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="0e1f3-618">Linktext, in dem der Genre Name angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="0e1f3-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="0e1f3-619">Controller Aktionsname (**Durchsuchen**)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="0e1f3-620">Routen Parameterwerte und angeben des Namens (**Genre**) und des Werts (**Genre Name**)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="0e1f3-621">Aufgabe 9: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="0e1f3-622">In dieser Aufgabe testen Sie, ob die einzelnen Genres mit einem Link zur entsprechenden **/Store/Browse** -URL angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="0e1f3-623">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-624">Das Projekt wird auf der Startseite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-624">The project starts in the Home page.</span></span> <span data-ttu-id="0e1f3-625">Ändern Sie die URL in **/Store** , um sicherzustellen, dass jedes Genre mit der entsprechenden **/Store/Browse** -URL verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="0e1f3-626">![Durchsuchen von Genres mit Links zur Seite "Durchsuchen"](aspnet-mvc-4-fundamentals/_static/image33.png "Durchsuchen von Genres mit Links zur Seite "Durchsuchen"")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="0e1f3-627">*Durchsuchen von Genres mit Links zur Seite "Durchsuchen"*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="0e1f3-628">Aufgabe 10: Verwenden der dynamischen ViewModel-Auflistung zum Übergeben von Werten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="0e1f3-629">In dieser Aufgabe lernen Sie eine einfache und leistungsfähige Methode kennen, um Werte zwischen dem Controller und der Ansicht zu übergeben, ohne Änderungen am Modell vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="0e1f3-630">ASP.NET MVC 4 stellt die Auflistung &quot;ViewModel-&quot;bereit, die einem beliebigen dynamischen Wert zugewiesen werden kann und auf die auch innerhalb von Controllern und Ansichten zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="0e1f3-631">Sie verwenden nun die dynamische viewbag-Auflistung, um eine Liste mit &quot;**Sterne-Genres**&quot; vom Controller an die Ansicht zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="0e1f3-632">Die Speicher Index Sicht greift auf **ViewModel** zu und zeigt die Informationen an.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="0e1f3-633">Schließen Sie den Browser bei Bedarf, um zum Visual Studio-Fenster zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="0e1f3-634">Öffnen Sie **StoreController.cs** , und ändern Sie die **Index** -Methode, um eine Liste mit den Sternen Genres in der ViewModel-Auflistung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="0e1f3-635">Sie können auch die Syntax **viewbag [&quot;Sterne&quot;]** verwenden, um auf die Eigenschaften zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="0e1f3-636">Das Stern Symbol **&quot;Sterne. png&quot;** ist im Ordner " **source\asset\images** " dieses Labs enthalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="0e1f3-637">Um Sie der Anwendung hinzuzufügen, ziehen Sie Ihren Inhalt aus einem **Windows-Explorer** -Fenster in das **Projektmappen-Explorer** in Visual Web Developer Express, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="0e1f3-638">![Der Projekt Mappe wird ein Stern Bild hinzugefügt.](aspnet-mvc-4-fundamentals/_static/image34.png "Der Projekt Mappe wird ein Stern Bild hinzugefügt.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="0e1f3-639">*Der Projekt Mappe wird ein Stern Bild hinzugefügt.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="0e1f3-640">Öffnen Sie die Ansicht " **Store/Index. cshtml** ", und ändern Sie den Inhalt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="0e1f3-641">Sie lesen die &quot;&quot;-Eigenschaft in der **viewbag** -Auflistung und Fragen, ob der aktuelle Genre Name in der Liste enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="0e1f3-642">In diesem Fall wird ein Stern Symbol direkt zum Genre Link angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="0e1f3-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="0e1f3-644">Aufgabe 11: Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="0e1f3-645">In dieser Aufgabe testen Sie, ob die gesternten Genres ein Stern Symbol anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="0e1f3-646">Drücken Sie **F5** , um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="0e1f3-647">Das Projekt wird auf der **Start** Seite gestartet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="0e1f3-648">Ändern Sie die URL in **/Store** , um sicherzustellen, dass jedes der vorgestellten Genres die Bezeichnung "Achtung" hat:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="0e1f3-649">![Durchsuchen von Genres mit gestarteten Elementen](aspnet-mvc-4-fundamentals/_static/image35.png "Durchsuchen von Genres mit gestarteten Elementen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="0e1f3-650">*Durchsuchen von Genres mit gestarteten Elementen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="0e1f3-651">Übung 7: A Lap around ASP.NET MVC 4 New Template</span><span class="sxs-lookup"><span data-stu-id="0e1f3-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="0e1f3-652">In dieser Übung werden die Verbesserungen in den ASP.NET MVC 4-Projektvorlagen erläutert, wobei die relevantesten Features der neuen Vorlage erläutert werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="0e1f3-653">Aufgabe 1: Untersuchen der ASP.NET MVC 4-Internet Anwendungs Vorlage</span><span class="sxs-lookup"><span data-stu-id="0e1f3-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="0e1f3-654">Wenn Sie nicht bereits geöffnet ist, starten Sie **vs Express für Web**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="0e1f3-655">Wählen Sie die Datei aus. **Neu |** Menübefehl "Projekt".</span><span class="sxs-lookup"><span data-stu-id="0e1f3-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="0e1f3-656">Wählen Sie im Dialogfeld **Neues Projekt** die **Option C#Visual | Webvorlage** im linken Fensterbereich, und wählen Sie die **ASP.NET MVC 4-Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="0e1f3-657">**Nennen** Sie das *Projekt Musicstore* *, und*aktualisieren Sie den Projektmappennamen, und wählen Sie dann einen Speicherort aus (oder über **lassen Sie den**Standardwert)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="0e1f3-658">![Erstellen eines neuen ASP.NET MVC 4-Projekts](aspnet-mvc-4-fundamentals/_static/image36.png "Erstellen eines neuen ASP.NET MVC 4-Projekts")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="0e1f3-659">*Erstellen eines neuen ASP.NET MVC 4-Projekts*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="0e1f3-660">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Projektvorlage **Internet Anwendung** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="0e1f3-661">Beachten Sie, dass Sie entweder Razor oder aspx als Ansichts-Engine auswählen können.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="0e1f3-662">![Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung](aspnet-mvc-4-fundamentals/_static/image37.png "Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="0e1f3-663">*Erstellen einer neuen ASP.NET MVC 4-Internet Anwendung*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-664">Razor-Syntax wurde in ASP.NET MVC 3 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="0e1f3-665">Das Ziel besteht darin, die Anzahl der in einer Datei benötigten Zeichen und Tastatureingaben zu minimieren und so einen schnellen und flüssigen Codierungs Workflow zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="0e1f3-666">Razor nutzt vorhandene C#/VB (oder andere) Sprachkenntnisse und bietet eine Vorlagen Markup Syntax, die einen tollen HTML-Konstruktions Workflow ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="0e1f3-667">Drücken Sie **F5** , um die Projekt Mappe auszuführen und die erneuerte Vorlage anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="0e1f3-668">Sehen Sie sich die folgenden Features an:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="0e1f3-669">**Moderne Vorlagen**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-669">**Modern-style templates**</span></span>

        <span data-ttu-id="0e1f3-670">Die Vorlagen wurden erneuert und bieten modernere Stile.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="0e1f3-671">![ASP.NET MVC 4-Vorlagen mit Neuformatierung](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4-Vorlagen mit Neuformatierung")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="0e1f3-672">*ASP.NET MVC 4-Vorlagen mit Neuformatierung*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="0e1f3-673">**Adaptive Rendering**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="0e1f3-674">Sehen Sie sich die Größe des Browserfensters an, und beachten Sie, wie das Seitenlayout dynamisch an die neue Fenstergröße angepasst wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="0e1f3-675">Diese Vorlagen verwenden die Adaptive renderingtechnik, um auf Desktop-und mobilen Plattformen ohne Anpassung ordnungsgemäß zu rendern.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="0e1f3-676">![ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="0e1f3-677">*ASP.NET MVC 4-Projektvorlage in unterschiedlichen Browser Größen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="0e1f3-678">Schließen Sie den Browser, um den Debugger zu beenden und zu Visual Studio zurückzukehren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="0e1f3-679">Jetzt können Sie die Lösung untersuchen und einige der neuen Features kennenlernen, die von ASP.NET MVC 4 in der Projektvorlage eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="0e1f3-680">![ASP.net MVC4-Internet-Application-Project-Template](aspnet-mvc-4-fundamentals/_static/image40.png "Die Projektvorlage "ASP.NET MVC 4 Internet Application"")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="0e1f3-681">*Die Projektvorlage "ASP.NET MVC 4 Internet Application"*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="0e1f3-682">**HTML5-Markup**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-682">**HTML5 markup**</span></span>

       <span data-ttu-id="0e1f3-683">Durchsuchen Sie Vorlagen Sichten, um das neue Design Markup zu ermitteln, z. b. Open **about. cshtml** - **Ansicht im Basis** Ordner.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="0e1f3-684">![Neue Vorlage mit Razor-und HTML5-Markup](aspnet-mvc-4-fundamentals/_static/image41.png "Neue Vorlage mit Razor-und HTML5-Markup")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="0e1f3-685">*Neue Vorlage mit Razor-und HTML5-Markup*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="0e1f3-686">**Enthaltene JavaScript-Bibliotheken**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="0e1f3-687">**jQuery**: jQuery vereinfacht das Durchlaufen von HTML-Dokumenten, Ereignis Behandlung, Animationen und AJAX-Interaktionen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="0e1f3-688">**jQuery-Benutzeroberfläche**: Diese Bibliothek bietet Abstraktionen für Interaktion und Animation auf niedriger Ebene, erweiterte Effekte und Aufzähl Bare Widgets, die auf der jQuery JavaScript-Bibliothek aufbaut.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="0e1f3-689">Weitere Informationen zu jQuery und jQuery UI finden Sie in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="0e1f3-690">**Knockoutjs**: die ASP.NET MVC 4-Standardvorlage enthält jetzt " **knockoutjs**", ein JavaScript-MVVM-Framework, mit dem Sie umfangreiche und sehr reaktionsfähige Webanwendungen mit JavaScript und HTML erstellen können.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="0e1f3-691">Wie in ASP.NET MVC 3 sind auch jQuery-und jQuery-UI-Bibliotheken in ASP.NET MVC 4 enthalten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="0e1f3-692">Weitere Informationen zur Datei "knockoutjs" finden Sie unter diesem Link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="0e1f3-693">**Modernizr**: Diese Bibliothek wird automatisch ausgeführt, sodass Ihre Site bei Verwendung von HTML5-und CSS3-Technologien mit älteren Browsern kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="0e1f3-694">Weitere Informationen zur modernizr-Bibliothek finden Sie unter diesem Link: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="0e1f3-695">**Simplemembership in der Lösung enthalten**</span><span class="sxs-lookup"><span data-stu-id="0e1f3-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="0e1f3-696">Simplemembership wurde als Ersatz für das vorherige ASP.NET-Rollen-und Mitgliedschafts Anbieter System entwickelt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="0e1f3-697">Es verfügt über viele neue Features, die es dem Entwickler erleichtern, Webseiten auf flexiblere Weise zu schützen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="0e1f3-698">In der Internet Vorlage sind bereits einige Dinge zum Integrieren von simplemembership eingerichtet. der AccountController ist beispielsweise für die Verwendung von oauthwebsecurity (für die OAuth-Kontoregistrierung, Anmeldung, Verwaltung usw.) und die Websicherheit vorbereitet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="0e1f3-699">![Simplemembership in der Lösung enthalten](aspnet-mvc-4-fundamentals/_static/image42.png "Simplemembership in der Lösung enthalten")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="0e1f3-700">*Simplemembership in der Lösung enthalten*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="0e1f3-701">Weitere Informationen zu [oauthwebsecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) finden Sie in MSDN.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="0e1f3-702">Außerdem können Sie diese Anwendung in Windows Azure-Websites bereitstellen, [indem Sie Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy verwenden](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="0e1f3-703">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-703">Summary</span></span>

<span data-ttu-id="0e1f3-704">Durch die Durchführung dieses praktischen Labs haben Sie die Grundlagen von ASP.NET MVC kennengelernt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="0e1f3-705">Die Kernelemente einer MVC-Anwendung und ihre Interaktion</span><span class="sxs-lookup"><span data-stu-id="0e1f3-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="0e1f3-706">Erstellen einer ASP.NET-MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="0e1f3-707">Hinzufügen und Konfigurieren von Controllern zum Verarbeiten von Parametern, die über die URL und QueryString geleitet werden</span><span class="sxs-lookup"><span data-stu-id="0e1f3-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="0e1f3-708">Vorgehensweise beim Hinzufügen einer layoutmasterseite zum Einrichten einer Vorlage für gemeinsamen HTML-Inhalt, eines Stylesheets zum Verbessern von Aussehen und Verhalten und einer Ansichts Vorlage zum Anzeigen von HTML-Inhalten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="0e1f3-709">Verwenden des ViewModel-Musters zum Übergeben von Eigenschaften an die Ansichts Vorlage zum Anzeigen dynamischer Informationen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="0e1f3-710">Verwenden von Parametern, die an Controller in der Ansichts Vorlage übermittelt werden</span><span class="sxs-lookup"><span data-stu-id="0e1f3-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="0e1f3-711">Hinzufügen von Links zu Seiten in der ASP.NET MVC-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0e1f3-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="0e1f3-712">Vorgehensweise beim Hinzufügen und verwenden dynamischer Eigenschaften in einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="0e1f3-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="0e1f3-713">Verbesserungen in den ASP.NET MVC 4-Projektvorlagen</span><span class="sxs-lookup"><span data-stu-id="0e1f3-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="0e1f3-714">Anhang A: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="0e1f3-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="0e1f3-715">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="0e1f3-716">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="0e1f3-717">Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="0e1f3-718">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Windows Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="0e1f3-719">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-719">Click on **Install Now**.</span></span> <span data-ttu-id="0e1f3-720">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="0e1f3-721">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="0e1f3-722">![Installieren von Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="0e1f3-723">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="0e1f3-724">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="0e1f3-726">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="0e1f3-727">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-727">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="0e1f3-729">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-729">*Installation progress*</span></span>
6. <span data-ttu-id="0e1f3-730">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-730">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="0e1f3-732">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-732">*Installation completed*</span></span>
7. <span data-ttu-id="0e1f3-733">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="0e1f3-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="0e1f3-734">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="0e1f3-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="0e1f3-736">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0e1f3-737">Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="0e1f3-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="0e1f3-738">In diesem Anhang wird gezeigt, wie Sie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und die Anwendung veröffentlichen, die Sie durch die folgenden Schritte abgerufen haben. nutzen Sie dabei die von Windows Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="0e1f3-739">Aufgabe 1: Erstellen einer neuen Website über das Windows Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="0e1f3-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="0e1f3-740">Wechseln Sie zum [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmelde Informationen an, die Ihrem Abonnement zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-741">Mit Windows Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="0e1f3-742">Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-742">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="0e1f3-743">![Anmelden bei Windows Azure-Portal](aspnet-mvc-4-fundamentals/_static/image48.png "Anmelden bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="0e1f3-744">*Anmelden bei Windows Azure Verwaltungsportal*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="0e1f3-745">Klicken Sie in der Befehlsleiste auf **neu** .</span><span class="sxs-lookup"><span data-stu-id="0e1f3-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="0e1f3-746">![Erstellen einer neuen Website](aspnet-mvc-4-fundamentals/_static/image49.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="0e1f3-747">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="0e1f3-748">Klicken Sie auf **Compute** - | **Website**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="0e1f3-749">Wählen Sie dann **schneller** Fassung aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="0e1f3-750">Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-751">Eine Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="0e1f3-752">Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals auf der Windows Azure-Website bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="0e1f3-753">Sie enthält keine Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="0e1f3-754">![Erstellen einer neuen Website mithilfe der schneller Fassung](aspnet-mvc-4-fundamentals/_static/image50.png "Erstellen einer neuen Website mithilfe der schneller Fassung")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="0e1f3-755">*Erstellen einer neuen Website mithilfe der schneller Fassung*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="0e1f3-756">Warten Sie, bis die neue **Website** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="0e1f3-757">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="0e1f3-758">Überprüfen Sie, ob die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="0e1f3-759">![Navigieren zur neuen Website](aspnet-mvc-4-fundamentals/_static/image51.png "Navigieren zur neuen Website")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="0e1f3-760">*Navigieren zur neuen Website*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="0e1f3-761">![Website wird ausgeführt](aspnet-mvc-4-fundamentals/_static/image52.png "Website wird ausgeführt")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="0e1f3-762">*Website wird ausgeführt*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-762">*Web site running*</span></span>
6. <span data-ttu-id="0e1f3-763">Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="0e1f3-764">![Öffnen der Website-Verwaltungs Seiten](aspnet-mvc-4-fundamentals/_static/image53.png "Öffnen der Website-Verwaltungs Seiten")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="0e1f3-765">*Öffnen der Website-Verwaltungs Seiten*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="0e1f3-766">Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .</span><span class="sxs-lookup"><span data-stu-id="0e1f3-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-767">Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung auf einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungs Methoden erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="0e1f3-768">Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="0e1f3-769">**Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen auf Windows Azure-Websites zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="0e1f3-770">![Das Website-Veröffentlichungs Profil wird heruntergeladen.](aspnet-mvc-4-fundamentals/_static/image54.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="0e1f3-771">*Das Website-Veröffentlichungs Profil wird heruntergeladen.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="0e1f3-772">Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="0e1f3-773">In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung auf Windows Azure-Websites in Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="0e1f3-774">![Die Veröffentlichungs Profil Datei wird gespeichert.](aspnet-mvc-4-fundamentals/_static/image55.png "Das Veröffentlichungs Profil wird gespeichert.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="0e1f3-775">*Die Veröffentlichungs Profil Datei wird gespeichert.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="0e1f3-776">Aufgabe 2: Konfigurieren des Datenbankservers</span><span class="sxs-lookup"><span data-stu-id="0e1f3-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="0e1f3-777">Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="0e1f3-778">Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="0e1f3-779">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="0e1f3-780">Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Windows Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="0e1f3-781">Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="0e1f3-782">Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="0e1f3-783">Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="0e1f3-784">![Dashboard des SQL-Datenbankservers](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard des SQL-Datenbankservers")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="0e1f3-785">*Dashboard des SQL-Datenbankservers*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="0e1f3-786">In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="0e1f3-787">Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](aspnet-mvc-4-fundamentals/_static/image57.png)</span><span class="sxs-lookup"><span data-stu-id="0e1f3-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Client-IP-Adresse](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="0e1f3-789">*Client-IP-Adresse*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="0e1f3-790">Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Änderungen bestätigen](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="0e1f3-792">*Änderungen bestätigen*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="0e1f3-793">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="0e1f3-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="0e1f3-794">Kehren Sie zur ASP.NET MVC 4-Lösung zurück.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="0e1f3-795">Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="0e1f3-796">![Veröffentlichen der Anwendung](aspnet-mvc-4-fundamentals/_static/image60.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="0e1f3-797">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="0e1f3-798">Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="0e1f3-799">![Importieren des Veröffentlichungs Profils](aspnet-mvc-4-fundamentals/_static/image61.png "Importieren des Veröffentlichungs Profils")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="0e1f3-800">*Veröffentlichungs Profil wird importiert.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="0e1f3-801">Klicken Sie auf **Verbindung**überprüfen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-801">Click **Validate Connection**.</span></span> <span data-ttu-id="0e1f3-802">Klicken Sie nach Abschluss der Überprüfung auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e1f3-803">Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="0e1f3-804">![Die Verbindung wird überprüft.](aspnet-mvc-4-fundamentals/_static/image62.png "Die Verbindung wird überprüft.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="0e1f3-805">*Die Verbindung wird überprüft.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-805">*Validating connection*</span></span>
4. <span data-ttu-id="0e1f3-806">Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="0e1f3-807">![Webbereitstellungs Konfiguration](aspnet-mvc-4-fundamentals/_static/image63.png "Webbereitstellungs Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="0e1f3-808">*Webbereitstellungs Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="0e1f3-809">Konfigurieren Sie die Datenbankverbindung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="0e1f3-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="0e1f3-810">Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="0e1f3-811">Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="0e1f3-812">Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="0e1f3-813">Geben Sie einen neuen Datenbanknamen ein, z. b.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="0e1f3-814">![Konfigurieren der Ziel Verbindungs Zeichenfolge](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="0e1f3-815">*Konfigurieren der Ziel Verbindungs Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="0e1f3-816">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-816">Then click **OK**.</span></span> <span data-ttu-id="0e1f3-817">Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="0e1f3-818">![Erstellen der Datenbank](aspnet-mvc-4-fundamentals/_static/image65.png "Erstellen der Daten Bank Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="0e1f3-819">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-819">*Creating the database*</span></span>
7. <span data-ttu-id="0e1f3-820">Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="0e1f3-821">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-821">Then click **Next**.</span></span>

    <span data-ttu-id="0e1f3-822">![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](aspnet-mvc-4-fundamentals/_static/image66.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="0e1f3-823">*Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="0e1f3-824">Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="0e1f3-825">![Veröffentlichen der Webanwendung](aspnet-mvc-4-fundamentals/_static/image67.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="0e1f3-826">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="0e1f3-827">Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="0e1f3-828">![In Windows Azure veröffentlichte Anwendung](aspnet-mvc-4-fundamentals/_static/image68.png "In Windows Azure veröffentlichte Anwendung")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="0e1f3-829">*In Windows Azure veröffentlichte Anwendung*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="0e1f3-830">Anhang C: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="0e1f3-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="0e1f3-831">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="0e1f3-832">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="0e1f3-833">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-fundamentals/_static/image69.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="0e1f3-834">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="0e1f3-835">***So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)***</span><span class="sxs-lookup"><span data-stu-id="0e1f3-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="0e1f3-836">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="0e1f3-837">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="0e1f3-838">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="0e1f3-839">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="0e1f3-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="0e1f3-840">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="0e1f3-841">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](aspnet-mvc-4-fundamentals/_static/image70.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="0e1f3-842">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="0e1f3-843">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](aspnet-mvc-4-fundamentals/_static/image71.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="0e1f3-844">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="0e1f3-845">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](aspnet-mvc-4-fundamentals/_static/image72.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="0e1f3-846">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="0e1f3-847">So ***fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="0e1f3-848">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="0e1f3-849">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="0e1f3-850">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="0e1f3-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="0e1f3-851">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](aspnet-mvc-4-fundamentals/_static/image73.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="0e1f3-852">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="0e1f3-853">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](aspnet-mvc-4-fundamentals/_static/image74.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="0e1f3-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="0e1f3-854">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="0e1f3-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
