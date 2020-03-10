---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Erstellen von Rest-APIs mit ASP.net-Web-API-ASP.NET 4. x
author: rick-anderson
description: 'Praktische Übungseinheit: Verwenden Sie die Web-API in ASP.NET 4. x, um eine einfache Rest-API für eine Contact Manager-Anwendung zu erstellen.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504267"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="677c5-103">Erstellen von Rest-APIs mit ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="677c5-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="677c5-104">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="677c5-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="677c5-105">Praktische Übungseinheit: Verwenden Sie die Web-API in ASP.NET 4. x, um eine einfache Rest-API für eine Contact Manager-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="677c5-106">Außerdem erstellen Sie einen Client, um die API zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="677c5-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="677c5-107">In den letzten Jahren wurde deutlich, dass HTTP nicht nur für die Einrichtung von HTML-Seiten vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="677c5-108">Außerdem handelt es sich um eine leistungsfähige Plattform zum Entwickeln von Web-APIs, die eine Reihe von Verben (Get, Post usw.) sowie einige einfache Konzepte wie *URIs* und *Header*verwendet.</span><span class="sxs-lookup"><span data-stu-id="677c5-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="677c5-109">ASP.net-Web-API ist ein Satz von Komponenten, die die HTTP-Programmierung vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="677c5-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="677c5-110">Da es auf der MVC-Laufzeit ASP.NET basiert, verarbeitet die Web-API automatisch die Transportdetails auf niedriger Ebene von http.</span><span class="sxs-lookup"><span data-stu-id="677c5-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="677c5-111">Gleichzeitig stellt die Web-API natürlich das HTTP-Programmiermodell zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="677c5-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="677c5-112">Ein Ziel der Web-API besteht darin, die Realität von http *nicht* zu abstrahieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="677c5-113">Folglich ist die Web-API sowohl flexibel als auch einfach zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="677c5-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="677c5-114">Der Rest-Architekturstil ist als effektive Methode für die Verwendung von http erwiesen, obwohl dies sicherlich nicht der einzige gültige Ansatz für http ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="677c5-115">Der Kontakt-Manager macht unter anderem den Rest für das auflisten, hinzufügen und Entfernen von Kontakten verfügbar.</span><span class="sxs-lookup"><span data-stu-id="677c5-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="677c5-116">Diese Übungseinheit erfordert grundlegende Kenntnisse von http und Rest und geht davon aus, dass Sie über grundlegende Kenntnisse in HTML, JavaScript und jQuery verfügen.</span><span class="sxs-lookup"><span data-stu-id="677c5-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="677c5-117">Die ASP.NET-Website verfügt über einen Bereich, der für das ASP.net-Web-API Framework bei [https://asp.net/web-api](https://asp.net/web-api)reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="677c5-118">Diese Site liefert weiterhin aktuelle Informationen, Beispiele und Neuigkeiten im Zusammenhang mit der Web-API. Überprüfen Sie Sie häufig, wenn Sie die Art der Erstellung von benutzerdefinierten Web-APIs vertiefen möchten, die für praktisch alle Geräte oder Entwicklungs-Frameworks verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="677c5-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="677c5-119">ASP.net-Web-API, ähnlich wie ASP.NET MVC 4, bietet eine hohe Flexibilität in Bezug auf die Trennung der Dienst Ebene von den Controllern, sodass Sie mehrere der verfügbaren Frameworks für die Abhängigkeitsinjektion recht einfach verwenden können.</span><span class="sxs-lookup"><span data-stu-id="677c5-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="677c5-120">Es gibt ein gutes Beispiel in MSDN, das zeigt, wie Sie Ninject für die Abhängigkeitsinjektion in einem ASP.net-Web-API Projekt verwenden, das Sie [hier](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)herunterladen können.</span><span class="sxs-lookup"><span data-stu-id="677c5-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="677c5-121">Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="677c5-122">Ziele</span><span class="sxs-lookup"><span data-stu-id="677c5-122">Objectives</span></span>

<span data-ttu-id="677c5-123">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="677c5-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="677c5-124">Implementieren einer Rest-Web-API</span><span class="sxs-lookup"><span data-stu-id="677c5-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="677c5-125">API von einem HTML-Client aus abrufen</span><span class="sxs-lookup"><span data-stu-id="677c5-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="677c5-126">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="677c5-126">Prerequisites</span></span>

<span data-ttu-id="677c5-127">Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:</span><span class="sxs-lookup"><span data-stu-id="677c5-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="677c5-128">[Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder Superior (Informationen zur Installation finden Sie in [Anhang B](#AppendixB) ).</span><span class="sxs-lookup"><span data-stu-id="677c5-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="677c5-129">Einrichten</span><span class="sxs-lookup"><span data-stu-id="677c5-129">Setup</span></span>

<span data-ttu-id="677c5-130">**Installieren von Code Ausschnitten**</span><span class="sxs-lookup"><span data-stu-id="677c5-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="677c5-131">Der Vorteil ist, dass ein Großteil des Codes, den Sie in diesem Lab verwalten, als Visual Studio-Code Ausschnitte verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="677c5-132">Um die Code Ausschnitte zu installieren, führen Sie **.\source\setup\codesnippeer-.vsi** -Datei aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="677c5-133">Wenn Sie mit den Visual Studio Code Ausschnitten nicht vertraut sind und wissen möchten, wie Sie Sie verwenden können, finden Sie den Anhang dieses Dokuments &quot;[Anhang A: Verwenden von Code Ausschnitten](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="677c5-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="677c5-134">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="677c5-134">Exercises</span></span>

<span data-ttu-id="677c5-135">Diese praktische Übungseinheit umfasst die folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="677c5-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="677c5-136">Übung 1: Erstellen einer schreibgeschützten Web-API</span><span class="sxs-lookup"><span data-stu-id="677c5-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="677c5-137">Übung 2: Erstellen einer Web-API mit Lese-/Schreibzugriff</span><span class="sxs-lookup"><span data-stu-id="677c5-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="677c5-138">Übung 3: Verwenden der Web-API aus einem HTML-Client</span><span class="sxs-lookup"><span data-stu-id="677c5-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="677c5-139">Jede Übung wird von einem **endordner** begleitet, der die sich ergebende Lösung enthält, die Sie nach Abschluss der Übungen erhalten.</span><span class="sxs-lookup"><span data-stu-id="677c5-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="677c5-140">Sie können diese Lösung als Leitfaden verwenden, wenn Sie zusätzliche Hilfe beim Durcharbeiten der Übungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="677c5-141">Geschätzte Zeit bis zum Abschluss dieses Labs: **60 Minuten**.</span><span class="sxs-lookup"><span data-stu-id="677c5-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="677c5-142">Übung 1: Erstellen einer schreibgeschützten Web-API</span><span class="sxs-lookup"><span data-stu-id="677c5-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="677c5-143">In dieser Übung implementieren Sie die schreibgeschützten Get-Methoden für den Kontakt-Manager.</span><span class="sxs-lookup"><span data-stu-id="677c5-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="677c5-144">Aufgabe 1: Erstellen des API-Projekts</span><span class="sxs-lookup"><span data-stu-id="677c5-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="677c5-145">In dieser Aufgabe verwenden Sie die neuen ASP.NET-Webprojekt Vorlagen, um eine Web-API-Webanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="677c5-146">Führen Sie **Visual Studio 2012 Express für das Web aus**. wechseln Sie zu **Start** , und geben Sie **vs Express für Web** und drücken Sie dann die **Eingabe**Taste.</span><span class="sxs-lookup"><span data-stu-id="677c5-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="677c5-147">Wählen Sie im Menü **Datei** die Option **Neues Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="677c5-148">Wählen Sie **das C# visuelle Element aus |** Webprojekttyp aus der Strukturansicht Projekttyp, und wählen Sie dann den Projekttyp **ASP.NET MVC 4 Webanwendung**</span><span class="sxs-lookup"><span data-stu-id="677c5-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="677c5-149">Legen Sie den **Namen** des Projekts auf *ContactManager* und den Projektmappennamen auf Start *fest, und*klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="677c5-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="677c5-150">![Erstellen eines neuen ASP.NET MVC 4,0-Webanwendungs Projekts](build-restful-apis-with-aspnet-web-api/_static/image1.png "Erstellen eines neuen ASP.NET MVC 4,0-Webanwendungs Projekts")</span><span class="sxs-lookup"><span data-stu-id="677c5-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="677c5-151">*Erstellen eines neuen ASP.NET MVC 4,0-Webanwendungs Projekts*</span><span class="sxs-lookup"><span data-stu-id="677c5-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="677c5-152">Wählen Sie im Dialogfeld ASP.NET MVC 4 Project Type den Typ **Web-API** -Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="677c5-153">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="677c5-153">Click **OK**.</span></span>

    <span data-ttu-id="677c5-154">![Angeben des Web-API-Projekt Typs](build-restful-apis-with-aspnet-web-api/_static/image2.png "Angeben des Web-API-Projekt Typs")</span><span class="sxs-lookup"><span data-stu-id="677c5-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="677c5-155">*Angeben des Web-API-Projekt Typs*</span><span class="sxs-lookup"><span data-stu-id="677c5-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="677c5-156">Aufgabe 2: Erstellen der Kontakt-Manager-API-Controller</span><span class="sxs-lookup"><span data-stu-id="677c5-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="677c5-157">In dieser Aufgabe erstellen Sie die Controller Klassen, in denen sich API-Methoden befinden.</span><span class="sxs-lookup"><span data-stu-id="677c5-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="677c5-158">Löschen Sie die Datei mit dem Namen **ValuesController.cs** im Ordner **Controllers** aus dem Projekt.</span><span class="sxs-lookup"><span data-stu-id="677c5-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="677c5-159">Klicken Sie im Projekt mit der rechten Maustaste auf den Ordner **Controller** , und wählen Sie **Hinzufügen | Controller** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="677c5-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="677c5-160">![Dem Projekt wird ein neuer Controller hinzugefügt.](build-restful-apis-with-aspnet-web-api/_static/image3.png "Dem Projekt wird ein neuer Controller hinzugefügt.")</span><span class="sxs-lookup"><span data-stu-id="677c5-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="677c5-161">*Dem Projekt wird ein neuer Controller hinzugefügt.*</span><span class="sxs-lookup"><span data-stu-id="677c5-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="677c5-162">Wählen Sie im angezeigten Dialogfeld **Controller hinzufügen** im Menüvorlage den Eintrag **leerer API-Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="677c5-163">Benennen Sie die Controller-Klasse **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="677c5-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="677c5-164">Klicken Sie dann auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="677c5-164">Then, click **Add.**</span></span>

    <span data-ttu-id="677c5-165">![Verwenden des Dialog Felds "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image4.png "Verwenden des Dialog Felds "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers")</span><span class="sxs-lookup"><span data-stu-id="677c5-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="677c5-166">*Verwenden des Dialog Felds "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers*</span><span class="sxs-lookup"><span data-stu-id="677c5-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="677c5-167">Fügen Sie den folgenden Code zu **ContactController**hinzu.</span><span class="sxs-lookup"><span data-stu-id="677c5-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="677c5-168">(Code Ausschnitt- *Web-API Lab-Ex01-Get API-Methode*)</span><span class="sxs-lookup"><span data-stu-id="677c5-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="677c5-169">Drücken Sie **F5**, um die Anwendung zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="677c5-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="677c5-170">Die Standard Startseite für ein Web-API-Projekt sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="677c5-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="677c5-171">![Die Standard Startseite einer ASP.net-Web-API Anwendung](build-restful-apis-with-aspnet-web-api/_static/image5.png "Die Standard Startseite einer ASP.net-Web-API Anwendung")</span><span class="sxs-lookup"><span data-stu-id="677c5-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="677c5-172">*Die Standard Startseite einer ASP.net-Web-API Anwendung*</span><span class="sxs-lookup"><span data-stu-id="677c5-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="677c5-173">Drücken Sie im Internet Explorer-Fenster die Taste **F12** , um das Fenster **Entwicklertools** zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="677c5-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="677c5-174">Klicken Sie auf die Registerkarte **Netzwerk** , und klicken Sie dann auf die Schaltfläche **Erfassung starten** , um den Netzwerk Datenverkehr im Fenster zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="677c5-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="677c5-175">![Öffnen der Registerkarte "Netzwerk" und Initiieren der Netzwerk Erfassung](build-restful-apis-with-aspnet-web-api/_static/image6.png "Öffnen der Registerkarte "Netzwerk" und Initiieren der Netzwerk Erfassung")</span><span class="sxs-lookup"><span data-stu-id="677c5-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="677c5-176">*Öffnen der Registerkarte "Netzwerk" und Initiieren der Netzwerk Erfassung*</span><span class="sxs-lookup"><span data-stu-id="677c5-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="677c5-177">Fügen Sie die URL in der Adressleiste des Browsers mit **/API/Contact** an, und drücken Sie die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="677c5-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="677c5-178">Die Übertragungs Details werden im Fenster Netzwerk Erfassung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="677c5-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="677c5-179">Beachten Sie, dass der MIME-Typ der Antwort " **Application/JSON**" lautet.</span><span class="sxs-lookup"><span data-stu-id="677c5-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="677c5-180">Dies zeigt, wie das Standardausgabeformat JSON ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="677c5-181">![Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk Ansicht](build-restful-apis-with-aspnet-web-api/_static/image7.png "Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk Ansicht")</span><span class="sxs-lookup"><span data-stu-id="677c5-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="677c5-182">*Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk Ansicht*</span><span class="sxs-lookup"><span data-stu-id="677c5-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="677c5-183">Das Standardverhalten von Internet Explorer 10 besteht darin, zu Fragen, ob der Benutzer den Stream, der sich aus dem Web-API-Befehl ergibt, speichern oder öffnen möchte.</span><span class="sxs-lookup"><span data-stu-id="677c5-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="677c5-184">Bei der Ausgabe handelt es sich um eine Textdatei mit dem JSON-Ergebnis des Web-API-URL-Aufrufes.</span><span class="sxs-lookup"><span data-stu-id="677c5-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="677c5-185">Brechen Sie das Dialogfeld nicht ab, um den Inhalt der Antwort über das Tool Fenster "Entwickler" anzeigen zu können.</span><span class="sxs-lookup"><span data-stu-id="677c5-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="677c5-186">Klicken Sie auf die Schaltfläche **Gehe zu ausführlicher Ansicht** , um weitere Details zur Antwort dieses API-Aufrufes anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="677c5-187">![Zur detaillierten Ansicht wechseln](build-restful-apis-with-aspnet-web-api/_static/image8.png "Zur Detailansicht wechseln")</span><span class="sxs-lookup"><span data-stu-id="677c5-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="677c5-188">*Zur detaillierten Ansicht wechseln*</span><span class="sxs-lookup"><span data-stu-id="677c5-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="677c5-189">Klicken Sie auf die Registerkarte **Antwort** Text, um den tatsächlichen JSON-Antworttext anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="677c5-190">![Anzeigen des JSON-Ausgabe Texts im Netzwerkmonitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Anzeigen des JSON-Ausgabe Texts im Netzwerkmonitor")</span><span class="sxs-lookup"><span data-stu-id="677c5-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="677c5-191">*Anzeigen des JSON-Ausgabe Texts im Netzwerkmonitor*</span><span class="sxs-lookup"><span data-stu-id="677c5-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="677c5-192">Aufgabe 3: Erstellen der Kontakt Modelle und Erweitern des Kontakt Controllers</span><span class="sxs-lookup"><span data-stu-id="677c5-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="677c5-193">In dieser Aufgabe erstellen Sie die Controller Klassen, in denen sich API-Methoden befinden.</span><span class="sxs-lookup"><span data-stu-id="677c5-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="677c5-194">Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle** , **und wählen Sie Klasse...** über das Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="677c5-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="677c5-195">![Hinzufügen eines neuen Modells zur Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image10.png "Hinzufügen eines neuen Modells zur Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="677c5-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="677c5-196">*Hinzufügen eines neuen Modells zur Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="677c5-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="677c5-197">Benennen Sie im Dialogfeld **Neues Element hinzufügen** die neue Datei **Contact.cs** , und klicken Sie auf **hinzufügen.**</span><span class="sxs-lookup"><span data-stu-id="677c5-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="677c5-198">![Erstellen der neuen Contact-Klassendatei](build-restful-apis-with-aspnet-web-api/_static/image11.png "Erstellen der neuen Contact-Klassendatei")</span><span class="sxs-lookup"><span data-stu-id="677c5-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="677c5-199">*Erstellen der neuen Contact-Klassendatei*</span><span class="sxs-lookup"><span data-stu-id="677c5-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="677c5-200">Fügen Sie der **Contact** -Klasse den folgenden markierten Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="677c5-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="677c5-201">(Code Ausschnitt- *Web-API Lab-Ex01-Contact-Klasse*)</span><span class="sxs-lookup"><span data-stu-id="677c5-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="677c5-202">Wählen Sie in der **ContactController** -Klasse die Wort **Zeichenfolge** in der Methoden Definition der **Get** -Methode aus, und geben Sie das Wort *Contact*ein.</span><span class="sxs-lookup"><span data-stu-id="677c5-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="677c5-203">Nachdem das Wort eingegeben wurde, wird am Anfang des Word- **Kontakts**ein Indikator angezeigt.</span><span class="sxs-lookup"><span data-stu-id="677c5-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="677c5-204">Halten Sie die **STRG** -Taste gedrückt, und drücken Sie die Taste (.), oder klicken Sie mit der Maus auf das Symbol, um das Dialogfeld "Hilfe" im Code-Editor zu öffnen, um die **using** -Direktive für den Namespace "Models" automatisch auszufüllen.</span><span class="sxs-lookup"><span data-stu-id="677c5-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Verwenden der IntelliSense-Unterstützung für Namespace Deklarationen](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="677c5-206">*Verwenden der IntelliSense-Unterstützung für Namespace Deklarationen*</span><span class="sxs-lookup"><span data-stu-id="677c5-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="677c5-207">Ändern Sie den Code für die **Get** -Methode, sodass Sie ein Array von Kontakt Modell Instanzen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="677c5-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="677c5-208">(Code Ausschnitt- *Web-API Lab-Ex01-Rückgabe einer Liste von Kontakten*)</span><span class="sxs-lookup"><span data-stu-id="677c5-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="677c5-209">Drücken Sie **F5** , um die Webanwendung im Browser zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="677c5-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="677c5-210">Führen Sie die folgenden Schritte aus, um die Änderungen an der Antwort Ausgabe der API anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="677c5-211">Nachdem der Browser geöffnet wurde, drücken Sie **F12** , wenn die Entwicklertools noch nicht geöffnet sind.</span><span class="sxs-lookup"><span data-stu-id="677c5-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="677c5-212">Klicken Sie auf die Registerkarte **Netzwerk** .</span><span class="sxs-lookup"><span data-stu-id="677c5-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="677c5-213">Klicken Sie auf die Schaltfläche **Erfassung starten** .</span><span class="sxs-lookup"><span data-stu-id="677c5-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="677c5-214">Fügen Sie das URL-Suffix **/API/Contact** der URL in der Adressleiste hinzu, und drücken **Sie die Eingabe** Taste.</span><span class="sxs-lookup"><span data-stu-id="677c5-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="677c5-215">Klicken Sie auf die Schaltfläche **Gehe zu ausführlicher Ansicht** .</span><span class="sxs-lookup"><span data-stu-id="677c5-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="677c5-216">Wählen Sie die Registerkarte **Antworttext** aus. Es sollte eine JSON-Zeichenfolge angezeigt werden, die die serialisierte Form eines Arrays von Kontakt Instanzen darstellt.</span><span class="sxs-lookup"><span data-stu-id="677c5-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="677c5-217">![JSON-serialisierte Ausgabe eines komplexen Web-API-Methoden Aufrufes](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON-serialisierte Ausgabe eines komplexen Web-API-Methoden Aufrufes")</span><span class="sxs-lookup"><span data-stu-id="677c5-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="677c5-218">*JSON-serialisierte Ausgabe eines komplexen Web-API-Methoden Aufrufes*</span><span class="sxs-lookup"><span data-stu-id="677c5-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="677c5-219">Aufgabe 4: Extrahieren von Funktionen in eine Dienst Ebene</span><span class="sxs-lookup"><span data-stu-id="677c5-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="677c5-220">Diese Aufgabe veranschaulicht, wie Sie Funktionen in eine Dienst Ebene extrahieren, um Entwicklern das Trennen ihrer Dienst Funktionalität von der Controller Schicht zu erleichtern. so wird die Wiederverwendbarkeit der Dienste ermöglicht, die tatsächlich die Arbeit erledigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="677c5-221">Erstellen Sie im Projektmappenstamm einen neuen Ordner, und nennen Sie ihn **Services**.</span><span class="sxs-lookup"><span data-stu-id="677c5-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="677c5-222">Klicken Sie hierzu mit der rechten Maustaste auf das Projekt **ContactManager** , wählen Sie | **neuen Ordner** **Hinzufügen** aus, und nennen Sie ihn *Services*.</span><span class="sxs-lookup"><span data-stu-id="677c5-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="677c5-223">![Ordner zum Erstellen von Diensten](build-restful-apis-with-aspnet-web-api/_static/image14.png "Ordner zum Erstellen von Diensten")</span><span class="sxs-lookup"><span data-stu-id="677c5-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="677c5-224">*Ordner zum Erstellen von Diensten*</span><span class="sxs-lookup"><span data-stu-id="677c5-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="677c5-225">Klicken Sie mit der rechten Maustaste auf den Ordner **Dienste** und wählen Sie **Hinzufügen Klasse...** über das Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="677c5-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="677c5-226">![Hinzufügen einer neuen Klasse zum Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image15.png "Hinzufügen einer neuen Klasse zum Ordner "Dienste"")</span><span class="sxs-lookup"><span data-stu-id="677c5-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="677c5-227">*Hinzufügen einer neuen Klasse zum Ordner "Dienste"*</span><span class="sxs-lookup"><span data-stu-id="677c5-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="677c5-228">Wenn das Dialogfeld **Neues Element hinzufügen** angezeigt wird, nennen Sie die neue Klasse **contactrepository** , und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="677c5-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="677c5-229">![Erstellen einer Klassendatei, die den Code für die Dienst Ebene des Contact-Repository enthält](build-restful-apis-with-aspnet-web-api/_static/image16.png "Erstellen einer Klassendatei, die den Code für die Dienst Ebene des Contact-Repository enthält")</span><span class="sxs-lookup"><span data-stu-id="677c5-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="677c5-230">*Erstellen einer Klassendatei, die den Code für die Dienst Ebene des Contact-Repository enthält*</span><span class="sxs-lookup"><span data-stu-id="677c5-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="677c5-231">Fügen Sie der Datei **ContactRepository.cs** eine using-Direktive hinzu, um den Namespace "Models" einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="677c5-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="677c5-232">Fügen Sie der Datei **ContactRepository.cs** den folgenden hervorgehobenen Code hinzu, um die getallcontacts-Methode zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="677c5-233">(Code Ausschnitt- *Web-API Lab-Ex01-Contact-Repository*)</span><span class="sxs-lookup"><span data-stu-id="677c5-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="677c5-234">Öffnen Sie die Datei **ContactController.cs** , wenn Sie nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="677c5-235">Fügen Sie dem Namespace Deklarations Abschnitt der Datei die folgende using-Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="677c5-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="677c5-236">Fügen Sie der **ContactController.cs** -Klasse den folgenden hervorgehobenen Code hinzu, um ein privates Feld hinzuzufügen, das die Instanz des Repository darstellt, damit die übrigen Klassenmember die Dienst Implementierung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="677c5-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="677c5-237">(Code Ausschnitt- *Web-API Lab-Ex01-Contact Controller*)</span><span class="sxs-lookup"><span data-stu-id="677c5-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="677c5-238">Ändern Sie die **Get** -Methode so, dass Sie den Contact-Repository-Dienst nutzt.</span><span class="sxs-lookup"><span data-stu-id="677c5-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="677c5-239">(Code Ausschnitt- *Web-API Lab-Ex01-Rückgabe einer Liste von Kontakten über das Repository*)</span><span class="sxs-lookup"><span data-stu-id="677c5-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="677c5-240">Platzieren Sie einen Haltepunkt in der **Get** -Methoden Definition von **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="677c5-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="677c5-241">![Hinzufügen von Breakpoints zum Kontakt Controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Hinzufügen von Breakpoints zum Kontakt Controller")</span><span class="sxs-lookup"><span data-stu-id="677c5-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="677c5-242">*Hinzufügen von Breakpoints zum Kontakt Controller*</span><span class="sxs-lookup"><span data-stu-id="677c5-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="677c5-243">Drücken Sie **F5**, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="677c5-244">Wenn der Browser geöffnet wird, drücken Sie **F12** , um die Entwicklertools zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="677c5-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="677c5-245">Klicken Sie auf die Registerkarte **Netzwerk** .</span><span class="sxs-lookup"><span data-stu-id="677c5-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="677c5-246">Klicken Sie auf die Schaltfläche **Erfassung starten** .</span><span class="sxs-lookup"><span data-stu-id="677c5-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="677c5-247">Fügen Sie die URL in der Adressleiste mit dem Suffix **/API/Contact** an, und drücken **Sie die Eingabe** Taste, um den API-Controller zu laden.</span><span class="sxs-lookup"><span data-stu-id="677c5-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="677c5-248">Visual Studio 2012 sollte abbrechen, sobald die Ausführung der **Get** -Methode beginnt.</span><span class="sxs-lookup"><span data-stu-id="677c5-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="677c5-249">![Unterbrechen innerhalb der Get-Methode](build-restful-apis-with-aspnet-web-api/_static/image18.png "Unterbrechen innerhalb der Get-Methode")</span><span class="sxs-lookup"><span data-stu-id="677c5-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="677c5-250">*Unterbrechen innerhalb der Get-Methode*</span><span class="sxs-lookup"><span data-stu-id="677c5-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="677c5-251">Drücken Sie **F5**, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="677c5-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="677c5-252">Wechseln Sie zurück zu Internet Explorer, wenn dieser noch nicht im Fokus ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="677c5-253">Notieren Sie sich das Fenster Netzwerk Erfassung.</span><span class="sxs-lookup"><span data-stu-id="677c5-253">Note the network capture window.</span></span>

    <span data-ttu-id="677c5-254">![Netzwerk Ansicht in Internet Explorer, die die Ergebnisse des Web-API-Aufrufes anzeigt](build-restful-apis-with-aspnet-web-api/_static/image19.png "Netzwerk Ansicht in Internet Explorer, die die Ergebnisse des Web-API-Aufrufes anzeigt")</span><span class="sxs-lookup"><span data-stu-id="677c5-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="677c5-255">*Netzwerk Ansicht in Internet Explorer, die die Ergebnisse des Web-API-Aufrufes anzeigt*</span><span class="sxs-lookup"><span data-stu-id="677c5-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="677c5-256">Klicken Sie auf die Schaltfläche **Gehe zu ausführlicher Ansicht** .</span><span class="sxs-lookup"><span data-stu-id="677c5-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="677c5-257">Klicken Sie auf die Registerkarte **Antworttext** . Beachten Sie die JSON-Ausgabe des API-Aufrufes und die Darstellung der beiden von der Dienst Ebene abgerufenen Kontakte.</span><span class="sxs-lookup"><span data-stu-id="677c5-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="677c5-258">![Anzeigen der JSON-Ausgabe der Web-API im Fenster "Entwicklertools"](build-restful-apis-with-aspnet-web-api/_static/image20.png "Anzeigen der JSON-Ausgabe der Web-API im Fenster "Entwicklertools"")</span><span class="sxs-lookup"><span data-stu-id="677c5-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="677c5-259">*Anzeigen der JSON-Ausgabe der Web-API im Fenster "Entwicklertools"*</span><span class="sxs-lookup"><span data-stu-id="677c5-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="677c5-260">Übung 2: Erstellen einer Web-API mit Lese-/Schreibzugriff</span><span class="sxs-lookup"><span data-stu-id="677c5-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="677c5-261">In dieser Übung implementieren Sie Post-und Put-Methoden für den Contact Manager, um ihn mit Datenbearbeitungs Funktionen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="677c5-262">Aufgabe 1: Öffnen des Web-API-Projekts</span><span class="sxs-lookup"><span data-stu-id="677c5-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="677c5-263">In dieser Aufgabe bereiten Sie sich darauf vor, das in Übung 1 erstellte Web-API-Projekt zu verbessern, sodass Benutzereingaben akzeptiert werden können.</span><span class="sxs-lookup"><span data-stu-id="677c5-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="677c5-264">Führen Sie **Visual Studio 2012 Express für das Web aus**. wechseln Sie zu **Start** , und geben Sie **vs Express für Web** und drücken Sie dann die **Eingabe**Taste.</span><span class="sxs-lookup"><span data-stu-id="677c5-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="677c5-265">Öffnen Sie die Projekt Mappe " **Begin** " unter **Source/Ex02-Read Write tewebapi/BEGIN/** Folder.</span><span class="sxs-lookup"><span data-stu-id="677c5-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="677c5-266">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="677c5-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="677c5-267">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="677c5-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="677c5-268">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="677c5-269">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="677c5-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="677c5-270">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="677c5-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="677c5-271">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="677c5-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="677c5-272">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="677c5-273">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="677c5-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="677c5-274">Öffnen Sie die Datei **Services/contactrepository. cs** .</span><span class="sxs-lookup"><span data-stu-id="677c5-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="677c5-275">Aufgabe 2: Hinzufügen von datendauerhaftigkeits Features zur kontaktrepository-Implementierung</span><span class="sxs-lookup"><span data-stu-id="677c5-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="677c5-276">In dieser Aufgabe erweitern Sie die contactrepository-Klasse des in Übung 1 erstellten Web-API-Projekts, sodass Benutzereingaben und neue Kontakt Instanzen persistent gespeichert und akzeptiert werden können.</span><span class="sxs-lookup"><span data-stu-id="677c5-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="677c5-277">Fügen Sie der **contactrepository** -Klasse die folgende Konstante hinzu, um den Namen des Schlüssel namens des Webserver-Cache Elements später in dieser Übung darzustellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="677c5-278">Fügen Sie einen Konstruktor zum **contactrepository** hinzu, das den folgenden Code enthält.</span><span class="sxs-lookup"><span data-stu-id="677c5-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="677c5-279">(Code Ausschnitt- *Web-API Lab-Ex02-Contact Repository-Konstruktor*)</span><span class="sxs-lookup"><span data-stu-id="677c5-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="677c5-280">Ändern Sie den Code für die **getallcontacts** -Methode, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="677c5-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="677c5-281">(Code Ausschnitt- *Web-API Lab-Ex02-Get all Contacts*)</span><span class="sxs-lookup"><span data-stu-id="677c5-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="677c5-282">Dieses Beispiel dient zu Demonstrationszwecken und verwendet den Webserver Cache als Speichermedium, damit die Werte für mehrere Clients gleichzeitig verfügbar sind, anstatt einen Sitzungs Speichermechanismus oder eine Anforderungs Speicher Lebensdauer zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="677c5-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="677c5-283">Sie können Entity Framework, den XML-Speicher oder eine beliebige andere Vielfalt anstelle des Webserver Caches verwenden.</span><span class="sxs-lookup"><span data-stu-id="677c5-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="677c5-284">Implementieren Sie eine neue Methode mit dem Namen **savecontact** zur **contactrepository** -Klasse, um das Speichern eines Kontakts auszuführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="677c5-285">Die **savecontact** -Methode muss einen einzelnen **Contact** -Parameter verwenden und einen booleschen Wert zurückgeben, der den Erfolg oder Misserfolg angibt.</span><span class="sxs-lookup"><span data-stu-id="677c5-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="677c5-286">(Code Ausschnitt- *Web-API Lab-Ex02-Implementieren der savecontact-Methode*)</span><span class="sxs-lookup"><span data-stu-id="677c5-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="677c5-287">Übung 3: Verwenden der Web-API aus einem HTML-Client</span><span class="sxs-lookup"><span data-stu-id="677c5-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="677c5-288">In dieser Übung erstellen Sie einen HTML-Client, um die Web-API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="677c5-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="677c5-289">Dieser Client vereinfacht den Datenaustausch mit der Web-API mithilfe von JavaScript und zeigt die Ergebnisse in einem Webbrowser mithilfe von HTML-Markup an.</span><span class="sxs-lookup"><span data-stu-id="677c5-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="677c5-290">Aufgabe 1: Ändern der Index Ansicht zur Bereitstellung einer GUI zum Anzeigen von Kontakten</span><span class="sxs-lookup"><span data-stu-id="677c5-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="677c5-291">In dieser Aufgabe ändern Sie die Standard Index Ansicht der Webanwendung, um die Anforderung der Anzeige der Liste vorhandener Kontakte in einem HTML-Browser zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="677c5-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="677c5-292">Öffnen Sie **Visual Studio 2012 Express für Web** , wenn es nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="677c5-293">Öffnen Sie die Projekt Mappe " **Begin** " unter **Quell/Ex03-consumingwebapi/BEGIN/** Folder.</span><span class="sxs-lookup"><span data-stu-id="677c5-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="677c5-294">Andernfalls können Sie die **End** -Projekt Mappe, die Sie durch Abschließen der vorherigen Übung erhalten haben, weiterhin verwenden.</span><span class="sxs-lookup"><span data-stu-id="677c5-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="677c5-295">Wenn Sie die bereitgestellte **Begin** -Lösung geöffnet haben, müssen Sie einige fehlende nuget-Pakete herunterladen, bevor Sie den Vorgang fortsetzen.</span><span class="sxs-lookup"><span data-stu-id="677c5-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="677c5-296">Klicken Sie hierzu auf das Menü **Projekt** , und wählen Sie **nuget-Pakete verwalten**aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="677c5-297">Klicken Sie im Dialogfeld **nuget-Pakete verwalten** auf **Wiederherstellen** , um fehlende Pakete herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="677c5-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="677c5-298">Erstellen Sie abschließend die Projekt Mappe, indem Sie auf **Build** \*\* | Projekt Mappe erstellen klicken\*\*.</span><span class="sxs-lookup"><span data-stu-id="677c5-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="677c5-299">Einer der Vorteile der Verwendung von nuget besteht darin, dass Sie nicht alle Bibliotheken in Ihrem Projekt liefern müssen, um die Projektgröße zu verringern.</span><span class="sxs-lookup"><span data-stu-id="677c5-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="677c5-300">Mit nuget Power Tools können Sie durch Angabe der Paketversionen in der Datei "Packages. config" alle erforderlichen Bibliotheken herunterladen, wenn Sie das Projekt zum ersten Mal ausführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="677c5-301">Aus diesem Grund müssen Sie diese Schritte ausführen, nachdem Sie eine vorhandene Projekt Mappe in diesem Lab geöffnet haben.</span><span class="sxs-lookup"><span data-stu-id="677c5-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="677c5-302">Öffnen Sie die Datei " **Index. cshtml** ", die sich im Ordner **views/Home** befindet.</span><span class="sxs-lookup"><span data-stu-id="677c5-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="677c5-303">Ersetzen Sie den HTML-Code im div-Element durch den ID- **Text** , sodass er wie der folgende Code aussieht.</span><span class="sxs-lookup"><span data-stu-id="677c5-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="677c5-304">Fügen Sie am Ende der Datei den folgenden JavaScript-Code hinzu, um die HTTP-Anforderung an die Web-API auszuführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="677c5-305">Öffnen Sie die Datei **ContactController.cs** , wenn Sie nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="677c5-306">Platzieren Sie einen Haltepunkt in der **Get** -Methode der **ContactController** -Klasse.</span><span class="sxs-lookup"><span data-stu-id="677c5-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="677c5-307">![Platzieren eines Breakpoints in der Get-Methode des API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image21.png "Platzieren eines Breakpoints in der Get-Methode des API-Controllers")</span><span class="sxs-lookup"><span data-stu-id="677c5-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="677c5-308">*Platzieren eines Breakpoints in der Get-Methode des API-Controllers*</span><span class="sxs-lookup"><span data-stu-id="677c5-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="677c5-309">Drücken Sie **F5**, um das Projekt auszuführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-309">Press **F5** to run the project.</span></span> <span data-ttu-id="677c5-310">Der Browser lädt das HTML-Dokument.</span><span class="sxs-lookup"><span data-stu-id="677c5-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="677c5-311">Stellen Sie sicher, dass Sie die Stamm-URL Ihrer Anwendung durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="677c5-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="677c5-312">Nachdem die Seite geladen und das JavaScript ausgeführt wurde, wird der Breakpoint gedrückt, und die Codeausführung wird im Controller angehalten.</span><span class="sxs-lookup"><span data-stu-id="677c5-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="677c5-313">![Debuggen in Web-API-Aufrufe mithilfe von vs Express für Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debuggen in Web-API-Aufrufe mithilfe von vs Express für Web")</span><span class="sxs-lookup"><span data-stu-id="677c5-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="677c5-314">*Debuggen in den Web-API-Aufrufe mithilfe von Visual Studio 2012 Express für Web*</span><span class="sxs-lookup"><span data-stu-id="677c5-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="677c5-315">Entfernen Sie den Haltepunkt, und drücken Sie **F5** oder die **Schaltfläche** zum Debuggen der Symbolleiste, um das Laden der Ansicht im Browser fortzusetzen.</span><span class="sxs-lookup"><span data-stu-id="677c5-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="677c5-316">Sobald der Web-API-Befehl abgeschlossen ist, sollten Sie die vom Web-API-Befehl zurückgegebenen Kontakte sehen, die als Listenelemente im Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="677c5-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="677c5-317">![Ergebnisse der API-Aufrufe, die im Browser als Listenelemente angezeigt werden](build-restful-apis-with-aspnet-web-api/_static/image23.png "Ergebnisse der API-Aufrufe, die im Browser als Listenelemente angezeigt werden")</span><span class="sxs-lookup"><span data-stu-id="677c5-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="677c5-318">*Ergebnisse der API-Aufrufe, die im Browser als Listenelemente angezeigt werden*</span><span class="sxs-lookup"><span data-stu-id="677c5-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="677c5-319">Beenden des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="677c5-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="677c5-320">Aufgabe 2: Ändern der Index Ansicht zum Bereitstellen einer GUI zum Erstellen von Kontakten</span><span class="sxs-lookup"><span data-stu-id="677c5-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="677c5-321">In dieser Aufgabe ändern Sie weiterhin die Index Ansicht der MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="677c5-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="677c5-322">Der HTML-Seite wird ein Formular hinzugefügt, mit dem Benutzereingaben erfasst und an die Web-API gesendet werden, um einen neuen Kontakt zu erstellen. Außerdem wird eine neue Web-API-Controller Methode erstellt, um das Datum von der grafischen Benutzeroberfläche zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="677c5-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="677c5-323">Öffnen Sie die Datei **ContactController.cs** .</span><span class="sxs-lookup"><span data-stu-id="677c5-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="677c5-324">Fügen Sie der Controller Klasse mit dem Namen **Post** eine neue Methode hinzu, wie im folgenden Code gezeigt.</span><span class="sxs-lookup"><span data-stu-id="677c5-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="677c5-325">(Code Ausschnitt- *Web-API Lab-Ex03-Post-Methode*)</span><span class="sxs-lookup"><span data-stu-id="677c5-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="677c5-326">Öffnen Sie die Datei **Index. cshtml** in Visual Studio, wenn Sie nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="677c5-327">Fügen Sie der Datei den folgenden HTML-Code direkt nach der ungeordneten Liste hinzu, die Sie in der vorherigen Aufgabe hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="677c5-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="677c5-328">Fügen Sie im Skript Element am Ende des Dokuments den folgenden markierten Code hinzu, um Click-Ereignisse zu behandeln, die die Daten mithilfe eines HTTP Post-Aufrufes an die Web-API senden.</span><span class="sxs-lookup"><span data-stu-id="677c5-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="677c5-329">Platzieren Sie in **ContactController.cs**einen Haltepunkt in der **Post** -Methode.</span><span class="sxs-lookup"><span data-stu-id="677c5-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="677c5-330">Drücken Sie **F5** , um die Anwendung im Browser auszuführen.</span><span class="sxs-lookup"><span data-stu-id="677c5-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="677c5-331">Wenn die Seite im Browser geladen ist, geben Sie einen neuen Kontaktnamen und eine ID ein, und klicken Sie auf die Schaltfläche **Speichern** .</span><span class="sxs-lookup"><span data-stu-id="677c5-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="677c5-332">![Das HTML-Client Dokument, das im Browser geladen wird.](build-restful-apis-with-aspnet-web-api/_static/image24.png "Das HTML-Client Dokument, das im Browser geladen wird.")</span><span class="sxs-lookup"><span data-stu-id="677c5-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="677c5-333">*Das HTML-Client Dokument, das im Browser geladen wird.*</span><span class="sxs-lookup"><span data-stu-id="677c5-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="677c5-334">Wenn das Debugger-Fenster in der **Post** -Methode unterbrochen wird, sehen Sie sich die Eigenschaften des **Contact** -Parameters an.</span><span class="sxs-lookup"><span data-stu-id="677c5-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="677c5-335">Die Werte müssen mit den Daten verglichen werden, die Sie im Formular eingegeben haben.</span><span class="sxs-lookup"><span data-stu-id="677c5-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="677c5-336">![Das Kontakt Objekt, das vom Client an die Web-API gesendet wird.](build-restful-apis-with-aspnet-web-api/_static/image25.png "Das Kontakt Objekt, das vom Client an die Web-API gesendet wird.")</span><span class="sxs-lookup"><span data-stu-id="677c5-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="677c5-337">*Das Kontakt Objekt, das vom Client an die Web-API gesendet wird.*</span><span class="sxs-lookup"><span data-stu-id="677c5-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="677c5-338">Schrittweises Durchlaufen der Methode im Debugger, bis die **Antwort** Variable erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="677c5-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="677c5-339">Bei der Überprüfung **im Fenster Lokal** im Debugger sehen Sie, dass alle Eigenschaften festgelegt wurden.</span><span class="sxs-lookup"><span data-stu-id="677c5-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="677c5-340">![Die Antwort nach der Erstellung im Debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "Die Antwort nach der Erstellung im Debugger")</span><span class="sxs-lookup"><span data-stu-id="677c5-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="677c5-341">*Die Antwort nach der Erstellung im Debugger*</span><span class="sxs-lookup"><span data-stu-id="677c5-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="677c5-342">Wenn Sie **F5** drücken oder im Debugger auf **weiter** klicken, wird die Anforderung beendet.</span><span class="sxs-lookup"><span data-stu-id="677c5-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="677c5-343">Nachdem Sie wieder zum Browser gewechselt haben, wurde der neue Kontakt der Liste der Kontakte hinzugefügt, die von der **contactrepository** -Implementierung gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="677c5-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="677c5-344">![Der Browser reflektiert die erfolgreiche Erstellung der neuen Kontakt Instanz.](build-restful-apis-with-aspnet-web-api/_static/image27.png "Der Browser reflektiert die erfolgreiche Erstellung der neuen Kontakt Instanz.")</span><span class="sxs-lookup"><span data-stu-id="677c5-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="677c5-345">*Der Browser reflektiert die erfolgreiche Erstellung der neuen Kontakt Instanz.*</span><span class="sxs-lookup"><span data-stu-id="677c5-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="677c5-346">Außerdem können Sie diese Anwendung in Azure bereitstellen. Weitere Informationen hierzu finden Sie im [Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="677c5-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="677c5-347">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="677c5-347">Summary</span></span>

<span data-ttu-id="677c5-348">In dieser Übungseinheit haben Sie das neue ASP.net-Web-API Framework und die Implementierung von Rest-Web-APIs mit dem Framework eingeführt.</span><span class="sxs-lookup"><span data-stu-id="677c5-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="677c5-349">Von hier aus können Sie ein neues Repository erstellen, das die Daten Persistenz mithilfe einer beliebigen Anzahl von Mechanismen vereinfacht und diesen Dienst anstelle der einfachen bereitstellt, die in dieser Übungseinheit als Beispiel bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="677c5-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="677c5-350">Die Web-API unterstützt eine Reihe zusätzlicher Features, z. b. das Aktivieren der Kommunikation von nicht-HTML-Clients, die in einer beliebigen Sprache geschrieben wurden, die HTTP und JSON oder XML</span><span class="sxs-lookup"><span data-stu-id="677c5-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="677c5-351">Die Möglichkeit, eine Web-API außerhalb einer typischen Webanwendung zu hosten, ist ebenfalls möglich, und Sie können auch eigene Serialisierungsformate erstellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="677c5-352">Die ASP.NET-Website verfügt über einen Bereich, der für das ASP.net-Web-API Framework bei [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api)reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="677c5-353">Diese Site liefert weiterhin aktuelle Informationen, Beispiele und Neuigkeiten im Zusammenhang mit der Web-API. Überprüfen Sie Sie häufig, wenn Sie die Art der Erstellung von benutzerdefinierten Web-APIs vertiefen möchten, die für praktisch alle Geräte oder Entwicklungs-Frameworks verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="677c5-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="677c5-354">Anhang A: Verwenden von Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="677c5-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="677c5-355">Mit Code Ausschnitten haben Sie den gesamten Code, den Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="677c5-356">Im Lab-Dokument werden Sie genau wissen, wann Sie Sie verwenden können, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="677c5-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="677c5-357">![Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt](build-restful-apis-with-aspnet-web-api/_static/image28.png "Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt")</span><span class="sxs-lookup"><span data-stu-id="677c5-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="677c5-358">*Verwenden von Visual Studio-Code Ausschnitten zum Einfügen von Code in Ihr Projekt*</span><span class="sxs-lookup"><span data-stu-id="677c5-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="677c5-359">So fügen Sie einen Code Ausschnitt mithilfe der Tastatur hinzuC# (nur)</span><span class="sxs-lookup"><span data-stu-id="677c5-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="677c5-360">Platzieren Sie den Cursor an der Stelle, an der Sie den Code einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="677c5-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="677c5-361">Beginnen Sie mit der Eingabe des Ausschnitt namens (ohne Leerzeichen oder Bindestriche).</span><span class="sxs-lookup"><span data-stu-id="677c5-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="677c5-362">Sehen Sie sich an, wie IntelliSense übereinstimmende Code Ausschnitte anzeigt.</span><span class="sxs-lookup"><span data-stu-id="677c5-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="677c5-363">Wählen Sie den richtigen Ausschnitt aus (oder geben Sie die Eingabe fort, bis der gesamte Name des Ausschnitts ausgewählt ist).</span><span class="sxs-lookup"><span data-stu-id="677c5-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="677c5-364">Drücken Sie zweimal die Tab-Taste, um den Ausschnitt an der Cursorposition einzufügen.</span><span class="sxs-lookup"><span data-stu-id="677c5-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="677c5-365">![Beginnen Sie mit der Eingabe des Ausschnitt namens.](build-restful-apis-with-aspnet-web-api/_static/image29.png "Beginnen Sie mit der Eingabe des Ausschnitt namens.")</span><span class="sxs-lookup"><span data-stu-id="677c5-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="677c5-366">*Beginnen Sie mit der Eingabe des Ausschnitt namens.*</span><span class="sxs-lookup"><span data-stu-id="677c5-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="677c5-367">![Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.](build-restful-apis-with-aspnet-web-api/_static/image30.png "Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.")</span><span class="sxs-lookup"><span data-stu-id="677c5-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="677c5-368">*Drücken Sie TAB, um den markierten Ausschnitt auszuwählen.*</span><span class="sxs-lookup"><span data-stu-id="677c5-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="677c5-369">![Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.](build-restful-apis-with-aspnet-web-api/_static/image31.png "Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.")</span><span class="sxs-lookup"><span data-stu-id="677c5-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="677c5-370">*Drücken Sie erneut die Tab-Taste, und der Ausschnitt wird erweitert.*</span><span class="sxs-lookup"><span data-stu-id="677c5-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="677c5-371">So fügen Sie einen Code Ausschnitt mithilfe der Maus hinzuC#(, Visual Basic und XML)</span><span class="sxs-lookup"><span data-stu-id="677c5-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="677c5-372">Klicken Sie mit der rechten Maustaste auf den Ort, an dem Sie den Code Ausschnitt einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="677c5-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="677c5-373">Wählen Sie **Ausschnitt einfügen** und dann **meine Code Ausschnitte**aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="677c5-374">Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="677c5-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="677c5-375">![Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.](build-restful-apis-with-aspnet-web-api/_static/image32.png "Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.")</span><span class="sxs-lookup"><span data-stu-id="677c5-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="677c5-376">*Klicken Sie mit der rechten Maustaste darauf, wo Sie den Code Ausschnitt einfügen möchten, und wählen Sie Ausschnitt einfügen aus.*</span><span class="sxs-lookup"><span data-stu-id="677c5-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="677c5-377">![Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.](build-restful-apis-with-aspnet-web-api/_static/image33.png "Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.")</span><span class="sxs-lookup"><span data-stu-id="677c5-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="677c5-378">*Wählen Sie den entsprechenden Code Ausschnitt aus der Liste aus, indem Sie darauf klicken.*</span><span class="sxs-lookup"><span data-stu-id="677c5-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="677c5-379">Anhang B: Installieren von Visual Studio Express 2012 für das Web</span><span class="sxs-lookup"><span data-stu-id="677c5-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="677c5-380">Sie können **Microsoft Visual Studio Express 2012 für Web** oder eine andere &quot;Express&quot; Version mit dem **[Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx)** installieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="677c5-381">Die folgenden Anweisungen führen Sie durch die Schritte, die zum Installieren *von Visual Studio Express 2012 für Web* mithilfe von *Microsoft-Webplattform-Installer*erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="677c5-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="677c5-382">Wechseln Sie zu [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="677c5-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="677c5-383">Wenn Sie den Webplattform-Installer bereits installiert haben, können Sie ihn auch öffnen und nach dem Produkt &quot;<em>Visual Studio Express 2012 für das Web mit dem Azure SDK</em>&quot;suchen.</span><span class="sxs-lookup"><span data-stu-id="677c5-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="677c5-384">Klicken Sie auf **jetzt installieren**.</span><span class="sxs-lookup"><span data-stu-id="677c5-384">Click on **Install Now**.</span></span> <span data-ttu-id="677c5-385">Wenn Sie nicht über einen **Webplattform-Installer** verfügen, werden Sie umgeleitet, um Sie zuerst herunterzuladen und zu installieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="677c5-386">Nachdem der **Webplattform-Installer** geöffnet ist, klicken Sie auf **Installieren** , um das Setup zu starten.</span><span class="sxs-lookup"><span data-stu-id="677c5-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="677c5-387">![Installieren von Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installieren von Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="677c5-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="677c5-388">*Installieren von Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="677c5-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="677c5-389">Lesen Sie die Lizenzbedingungen für alle Produkte, und klicken Sie auf **ich** Stimme zu, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="677c5-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akzeptieren der Lizenzbedingungen](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="677c5-391">*Akzeptieren der Lizenzbedingungen*</span><span class="sxs-lookup"><span data-stu-id="677c5-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="677c5-392">Warten Sie, bis der Download-und Installationsvorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-392">Wait until the downloading and installation process completes.</span></span>

    ![Installationsstatus](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="677c5-394">*Installationsfortschritt*</span><span class="sxs-lookup"><span data-stu-id="677c5-394">*Installation progress*</span></span>
6. <span data-ttu-id="677c5-395">Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig**stellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-395">When the installation completes, click **Finish**.</span></span>

    ![Installation abgeschlossen](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="677c5-397">*Installation abgeschlossen*</span><span class="sxs-lookup"><span data-stu-id="677c5-397">*Installation completed*</span></span>
7. <span data-ttu-id="677c5-398">Klicken Sie zum Schließen des Webplattform-Installers auf **Beenden** .</span><span class="sxs-lookup"><span data-stu-id="677c5-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="677c5-399">Um Visual Studio Express für das Web zu öffnen, navigieren Sie zum **Start** Bildschirm, und beginnen Sie mit dem Schreiben &quot;**vs Express**&quot;und klicken Sie dann auf die Kachel **vs Express für Web** .</span><span class="sxs-lookup"><span data-stu-id="677c5-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express für Web-Kachel](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="677c5-401">*VS Express für Web-Kachel*</span><span class="sxs-lookup"><span data-stu-id="677c5-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="677c5-402">Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="677c5-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="677c5-403">In diesem Anhang wird erläutert, wie Sie eine neue Website aus dem Azure-Portal erstellen und die Anwendung, die Sie erhalten haben, mithilfe des Labs veröffentlichen. nutzen Sie dabei die von Azure bereitgestellte Funktion zur Web deploy Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="677c5-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="677c5-404">Aufgabe 1: Erstellen einer neuen Website aus dem Azure-Portal</span><span class="sxs-lookup"><span data-stu-id="677c5-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="677c5-405">Wechseln Sie zum [Azure-Verwaltungsportal](https://manage.windowsazure.com/) , und melden Sie sich mit den mit Ihrem Abonnement verknüpften Microsoft-Anmelde Informationen an.</span><span class="sxs-lookup"><span data-stu-id="677c5-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="677c5-406">Mit Azure können Sie 10 ASP.NET Websites kostenlos hosten und dann skalieren, wenn Ihr Datenverkehr zunimmt.</span><span class="sxs-lookup"><span data-stu-id="677c5-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="677c5-407">Sie können sich [hier](https://aka.ms/aspnet-hol-azure)registrieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="677c5-408">![Anmelden bei Windows Azure-Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Anmelden bei Windows Azure-Portal")</span><span class="sxs-lookup"><span data-stu-id="677c5-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="677c5-409">*Anmelden beim Portal*</span><span class="sxs-lookup"><span data-stu-id="677c5-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="677c5-410">Klicken Sie in der Befehlsleiste auf **neu** .</span><span class="sxs-lookup"><span data-stu-id="677c5-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="677c5-411">![Erstellen einer neuen Website](build-restful-apis-with-aspnet-web-api/_static/image40.png "Erstellen einer neuen Website")</span><span class="sxs-lookup"><span data-stu-id="677c5-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="677c5-412">*Erstellen einer neuen Website*</span><span class="sxs-lookup"><span data-stu-id="677c5-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="677c5-413">Klicken Sie auf **Compute** - | **Website**.</span><span class="sxs-lookup"><span data-stu-id="677c5-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="677c5-414">Wählen Sie dann **schneller** Fassung aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="677c5-415">Geben Sie eine verfügbare URL für die neue Website an, und klicken Sie auf **Website erstellen**.</span><span class="sxs-lookup"><span data-stu-id="677c5-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="677c5-416">Azure ist der Host für eine Webanwendung, die in der Cloud ausgeführt wird und die Sie steuern und verwalten können.</span><span class="sxs-lookup"><span data-stu-id="677c5-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="677c5-417">Mit der Option schneller Fassung können Sie eine abgeschlossene Webanwendung von außerhalb des Portals in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="677c5-418">Sie enthält keine Schritte zum Einrichten einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="677c5-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="677c5-419">![Erstellen einer neuen Website mithilfe der schneller Fassung](build-restful-apis-with-aspnet-web-api/_static/image41.png "Erstellen einer neuen Website mithilfe der schneller Fassung")</span><span class="sxs-lookup"><span data-stu-id="677c5-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="677c5-420">*Erstellen einer neuen Website mithilfe der schneller Fassung*</span><span class="sxs-lookup"><span data-stu-id="677c5-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="677c5-421">Warten Sie, bis die neue **Website** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="677c5-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="677c5-422">Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** -Spalte.</span><span class="sxs-lookup"><span data-stu-id="677c5-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="677c5-423">Überprüfen Sie, ob die neue Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="677c5-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="677c5-424">![Navigieren zur neuen Website](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navigieren zur neuen Website")</span><span class="sxs-lookup"><span data-stu-id="677c5-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="677c5-425">*Navigieren zur neuen Website*</span><span class="sxs-lookup"><span data-stu-id="677c5-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="677c5-426">![Website wird ausgeführt](build-restful-apis-with-aspnet-web-api/_static/image43.png "Website wird ausgeführt")</span><span class="sxs-lookup"><span data-stu-id="677c5-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="677c5-427">*Website wird ausgeführt*</span><span class="sxs-lookup"><span data-stu-id="677c5-427">*Web site running*</span></span>
6. <span data-ttu-id="677c5-428">Wechseln Sie zurück zum Portal, und klicken Sie in der Spalte **Name** auf den Namen der Website, um die Verwaltungs Seiten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="677c5-429">![Öffnen der Website-Verwaltungs Seiten](build-restful-apis-with-aspnet-web-api/_static/image44.png "Öffnen der Website-Verwaltungs Seiten")</span><span class="sxs-lookup"><span data-stu-id="677c5-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="677c5-430">*Öffnen der Website-Verwaltungs Seiten*</span><span class="sxs-lookup"><span data-stu-id="677c5-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="677c5-431">Klicken Sie auf der Seite **Dashboard** im Abschnitt **kurzer Blick** auf den Link **Veröffentlichungs Profil herunterladen** .</span><span class="sxs-lookup"><span data-stu-id="677c5-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="677c5-432">Das *Veröffentlichungs Profil* enthält alle Informationen, die zum Veröffentlichen einer Webanwendung in einer Azure für jede aktivierte Veröffentlichungs Methode erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="677c5-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="677c5-433">Das Veröffentlichungs Profil enthält die URLs, Benutzer Anmelde Informationen und Daten Bank Zeichenfolgen, die erforderlich sind, um eine Verbindung mit den einzelnen Endpunkten herzustellen und diese zu authentifizieren, für die eine Veröffentlichungs Methode aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="677c5-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="677c5-434">**Microsoft webmatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** unterstützen das Lesen von Veröffentlichungs Profilen, um die Konfiguration dieser Programme für das Veröffentlichen von Webanwendungen in Azure zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="677c5-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="677c5-435">![Das Website-Veröffentlichungs Profil wird heruntergeladen.](build-restful-apis-with-aspnet-web-api/_static/image45.png "Das Website-Veröffentlichungs Profil wird heruntergeladen.")</span><span class="sxs-lookup"><span data-stu-id="677c5-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="677c5-436">*Das Website-Veröffentlichungs Profil wird heruntergeladen.*</span><span class="sxs-lookup"><span data-stu-id="677c5-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="677c5-437">Laden Sie die Veröffentlichungs Profil Datei an einen bekannten Speicherort herunter.</span><span class="sxs-lookup"><span data-stu-id="677c5-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="677c5-438">In dieser Übung erfahren Sie, wie Sie diese Datei zum Veröffentlichen einer Webanwendung in Azure aus Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="677c5-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="677c5-439">![Die Veröffentlichungs Profil Datei wird gespeichert.](build-restful-apis-with-aspnet-web-api/_static/image46.png "Das Veröffentlichungs Profil wird gespeichert.")</span><span class="sxs-lookup"><span data-stu-id="677c5-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="677c5-440">*Die Veröffentlichungs Profil Datei wird gespeichert.*</span><span class="sxs-lookup"><span data-stu-id="677c5-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="677c5-441">Aufgabe 2: Konfigurieren des Datenbankservers</span><span class="sxs-lookup"><span data-stu-id="677c5-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="677c5-442">Wenn Ihre Anwendung SQL Server Datenbanken nutzt, müssen Sie einen SQL-Daten Bank Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="677c5-443">Wenn Sie eine einfache Anwendung bereitstellen möchten, die SQL Server nicht verwendet, können Sie diese Aufgabe überspringen.</span><span class="sxs-lookup"><span data-stu-id="677c5-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="677c5-444">Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="677c5-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="677c5-445">Sie können die SQL-Datenbankserver aus Ihrem Abonnement im Azure-Verwaltungs Portal unter **SQL-Datenbanken** | **Server** | **Dashboard des Servers**anzeigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="677c5-446">Wenn Sie keinen Server erstellt haben, können Sie ihn mithilfe der Schaltfläche **Hinzufügen** auf der Befehlsleiste erstellen.</span><span class="sxs-lookup"><span data-stu-id="677c5-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="677c5-447">Notieren Sie sich den **Servernamen und die URL, den Administrator Anmelde Namen und das Kennwort**, da Sie Sie in den nächsten Aufgaben verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="677c5-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="677c5-448">Erstellen Sie die Datenbank noch nicht, da Sie in einer späteren Phase erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="677c5-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="677c5-449">![Dashboard des SQL-Datenbankservers](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard des SQL-Datenbankservers")</span><span class="sxs-lookup"><span data-stu-id="677c5-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="677c5-450">*Dashboard des SQL-Datenbankservers*</span><span class="sxs-lookup"><span data-stu-id="677c5-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="677c5-451">In der nächsten Aufgabe testen Sie die Datenbankverbindung aus Visual Studio. aus diesem Grund müssen Sie Ihre lokale IP-Adresse in die Liste der **zulässigen IP-Adressen**des Servers einschließen.</span><span class="sxs-lookup"><span data-stu-id="677c5-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="677c5-452">Klicken Sie hierzu auf **Konfigurieren**, wählen Sie die IP-Adresse der **aktuellen Client-IP-Adresse** aus, und fügen Sie Sie in die Textfelder Start-IP- **Adresse** und End- **IP-Adresse** ein, und klicken Sie auf die Schaltfläche ![Add-Client-IP-Address-OK-Button](build-restful-apis-with-aspnet-web-api/_static/image48.png)</span><span class="sxs-lookup"><span data-stu-id="677c5-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Client-IP-Adresse](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="677c5-454">*Client-IP-Adresse*</span><span class="sxs-lookup"><span data-stu-id="677c5-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="677c5-455">Sobald die **Client-IP-Adresse** der Liste zulässige IP-Adressen hinzugefügt wurde, klicken Sie auf **Speichern** , um die Änderungen zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="677c5-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Änderungen bestätigen](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="677c5-457">*Änderungen bestätigen*</span><span class="sxs-lookup"><span data-stu-id="677c5-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="677c5-458">Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web deploy</span><span class="sxs-lookup"><span data-stu-id="677c5-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="677c5-459">Kehren Sie zur ASP.NET MVC 4-Lösung zurück.</span><span class="sxs-lookup"><span data-stu-id="677c5-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="677c5-460">Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Website Projekt, und wählen Sie **veröffentlichen**aus.</span><span class="sxs-lookup"><span data-stu-id="677c5-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="677c5-461">![Veröffentlichen der Anwendung](build-restful-apis-with-aspnet-web-api/_static/image51.png "Veröffentlichen der Anwendung")</span><span class="sxs-lookup"><span data-stu-id="677c5-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="677c5-462">*Veröffentlichen der Website*</span><span class="sxs-lookup"><span data-stu-id="677c5-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="677c5-463">Importieren Sie das Veröffentlichungs Profil, das Sie in der ersten Aufgabe gespeichert haben.</span><span class="sxs-lookup"><span data-stu-id="677c5-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="677c5-464">![Importieren des Veröffentlichungs Profils](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importieren des Veröffentlichungs Profils")</span><span class="sxs-lookup"><span data-stu-id="677c5-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="677c5-465">*Veröffentlichungs Profil wird importiert.*</span><span class="sxs-lookup"><span data-stu-id="677c5-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="677c5-466">Klicken Sie auf **Verbindung**überprüfen.</span><span class="sxs-lookup"><span data-stu-id="677c5-466">Click **Validate Connection**.</span></span> <span data-ttu-id="677c5-467">Klicken Sie nach Abschluss der Überprüfung auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="677c5-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="677c5-468">Die Überprüfung ist fertiggestellt, sobald ein grünes Häkchen neben der Schaltfläche Verbindung überprüfen angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="677c5-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="677c5-469">![Die Verbindung wird überprüft.](build-restful-apis-with-aspnet-web-api/_static/image53.png "Die Verbindung wird überprüft.")</span><span class="sxs-lookup"><span data-stu-id="677c5-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="677c5-470">*Die Verbindung wird überprüft.*</span><span class="sxs-lookup"><span data-stu-id="677c5-470">*Validating connection*</span></span>
4. <span data-ttu-id="677c5-471">Klicken Sie auf der Seite **Einstellungen** unter dem Abschnitt **Datenbanken** auf die Schaltfläche neben dem Textfeld der Datenbankverbindung (z. b. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="677c5-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="677c5-472">![Webbereitstellungs Konfiguration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Webbereitstellungs Konfiguration")</span><span class="sxs-lookup"><span data-stu-id="677c5-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="677c5-473">*Webbereitstellungs Konfiguration*</span><span class="sxs-lookup"><span data-stu-id="677c5-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="677c5-474">Konfigurieren Sie die Datenbankverbindung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="677c5-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="677c5-475">Geben Sie unter **Server Name** die URL des SQL-Datenbankservers unter Verwendung des *TCP:* -Präfix ein.</span><span class="sxs-lookup"><span data-stu-id="677c5-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="677c5-476">Geben Sie unter **Benutzername** den Anmelde Namen des Server Administrators ein.</span><span class="sxs-lookup"><span data-stu-id="677c5-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="677c5-477">Geben Sie unter **Kennwort** Ihren Server Administrator-Anmelde Kennwort ein.</span><span class="sxs-lookup"><span data-stu-id="677c5-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="677c5-478">Geben Sie einen neuen Datenbanknamen ein, z. b.: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="677c5-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="677c5-479">![Konfigurieren der Ziel Verbindungs Zeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurieren der Ziel Verbindungs Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="677c5-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="677c5-480">*Konfigurieren der Ziel Verbindungs Zeichenfolge*</span><span class="sxs-lookup"><span data-stu-id="677c5-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="677c5-481">Klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="677c5-481">Then click **OK**.</span></span> <span data-ttu-id="677c5-482">Wenn Sie zum Erstellen der Datenbank aufgefordert werden, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="677c5-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="677c5-483">![Erstellen der Datenbank](build-restful-apis-with-aspnet-web-api/_static/image56.png "Erstellen der Daten Bank Zeichenfolge")</span><span class="sxs-lookup"><span data-stu-id="677c5-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="677c5-484">*Erstellen der Datenbank*</span><span class="sxs-lookup"><span data-stu-id="677c5-484">*Creating the database*</span></span>
7. <span data-ttu-id="677c5-485">Die Verbindungs Zeichenfolge, die Sie zum Herstellen einer Verbindung mit der SQL-Datenbank in Windows Azure verwenden, wird im Textfeld Standardverbindung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="677c5-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="677c5-486">Klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="677c5-486">Then click **Next**.</span></span>

    <span data-ttu-id="677c5-487">![Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank](build-restful-apis-with-aspnet-web-api/_static/image57.png "Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank")</span><span class="sxs-lookup"><span data-stu-id="677c5-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="677c5-488">*Verbindungs Zeichenfolge mit Verweis auf SQL-Datenbank*</span><span class="sxs-lookup"><span data-stu-id="677c5-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="677c5-489">Klicken Sie auf der Seite **Vorschau** auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="677c5-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="677c5-490">![Veröffentlichen der Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image58.png "Veröffentlichen der Webanwendung")</span><span class="sxs-lookup"><span data-stu-id="677c5-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="677c5-491">*Veröffentlichen der Webanwendung*</span><span class="sxs-lookup"><span data-stu-id="677c5-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="677c5-492">Nachdem der Veröffentlichungs Vorgang abgeschlossen ist, wird die veröffentlichte Website in Ihrem Standardbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="677c5-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="677c5-493">![In Windows Azure veröffentlichte Anwendung](build-restful-apis-with-aspnet-web-api/_static/image59.png "In Windows Azure veröffentlichte Anwendung")</span><span class="sxs-lookup"><span data-stu-id="677c5-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="677c5-494">*In Azure veröffentlichte Anwendung*</span><span class="sxs-lookup"><span data-stu-id="677c5-494">*Application published to Azure*</span></span>
