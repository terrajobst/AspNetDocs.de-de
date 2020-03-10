---
uid: single-page-application/overview/templates/emberjs-template
title: Emberjs-Vorlage | Microsoft-Dokumentation
author: xqiu
description: EmberJS-Vorlage
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467157"
---
# <a name="emberjs-template"></a><span data-ttu-id="7eb08-103">EmberJS-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7eb08-103">EmberJS template</span></span>

<span data-ttu-id="7eb08-104">von [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="7eb08-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="7eb08-105">Die emberjs-MVC-Vorlage wird von Nathan, Thiago Santos und Xinyang Qiu geschrieben.</span><span class="sxs-lookup"><span data-stu-id="7eb08-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="7eb08-106">Herunterladen der emberjs-MVC-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7eb08-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)

<span data-ttu-id="7eb08-107">Die emberjs-Spa-Vorlage ist so konzipiert, dass Sie den Einstieg in das Erstellen von interaktiven Client seitigen Web-Apps mithilfe von emberjs erleichtern.</span><span class="sxs-lookup"><span data-stu-id="7eb08-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="7eb08-108">Die Single-Page-Anwendung (Single-Page Application, Spa) ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt und dann die Seite dynamisch aktualisiert, anstatt neue Seiten zu laden.</span><span class="sxs-lookup"><span data-stu-id="7eb08-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="7eb08-109">Nach dem anfänglichen Laden der Seite kommuniziert die Spa mit dem Server über AJAX-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="7eb08-110">AJAX ist nichts neues, aber heute gibt es JavaScript-Frameworks, die das Erstellen und Verwalten einer großen anspruchsvollen Spa-Anwendung vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="7eb08-111">Außerdem vereinfachen HTML 5 und CSS3 das Erstellen umfassender Benutzeroberflächen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="7eb08-112">Die emberjs-Spa-Vorlage verwendet die [Ember](http://emberjs.com/) -JavaScript-Bibliothek, um Seiten Aktualisierungen von AJAX-Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="7eb08-113">Ember. js verwendet die Datenbindung, um die Seite mit den neuesten Daten zu synchronisieren.</span><span class="sxs-lookup"><span data-stu-id="7eb08-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="7eb08-114">Auf diese Weise müssen Sie keinen Code schreiben, der die JSON-Daten durchläuft und das DOM aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="7eb08-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="7eb08-115">Stattdessen fügen Sie deklarative Attribute in den HTML-Code ein, der Ember. js mitteilt, wie die Daten dargestellt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="7eb08-116">Auf der Serverseite ist die emberjs-Vorlage fast identisch mit der [Spa-Vorlage von knockoutjs](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="7eb08-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="7eb08-117">Er verwendet ASP.NET MVC, um HTML-Dokumente zu verarbeiten, und ASP.net-Web-API, um AJAX-Anforderungen vom Client zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="7eb08-118">Weitere Informationen zu den Aspekten der Vorlage finden Sie in der Dokumentation zu der [knockoutjs-Vorlage](../introduction/knockoutjs-template.md) .</span><span class="sxs-lookup"><span data-stu-id="7eb08-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="7eb08-119">Dieses Thema konzentriert sich auf die Unterschiede zwischen der Knockout-Vorlage und der emberjs-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7eb08-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="7eb08-120">Erstellen eines emberjs-Spa-Vorlagen Projekts</span><span class="sxs-lookup"><span data-stu-id="7eb08-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="7eb08-121">Um die Vorlage herunterzuladen und zu installieren, klicken Sie oben auf die Schaltfläche herunterladen</span><span class="sxs-lookup"><span data-stu-id="7eb08-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="7eb08-122">Möglicherweise müssen Sie Visual Studio neu starten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="7eb08-123">Wählen Sie im Bereich **Vorlagen** die Option **installierte Vorlagen** aus, und erweitern Sie den Knoten **visuelle C#**  Knoten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="7eb08-124">Wählen Sie unter **Visualisierung C#** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="7eb08-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7eb08-125">Wählen Sie in der Liste der Projektvorlagen die Option **ASP.NET MVC 4-Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="7eb08-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7eb08-126">Geben Sie dem Projekt einen Namen, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7eb08-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="7eb08-127">Wählen Sie im Assistenten für **neue Projekte** die Option **Ember. js-Spa-Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="7eb08-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="7eb08-128">Übersicht über die emberjs-Spa-Vorlage</span><span class="sxs-lookup"><span data-stu-id="7eb08-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="7eb08-129">Die emberjs-Vorlage verwendet eine Kombination aus "jQuery", "Ember. js" und "Lenker. js", um eine reibungslose interaktive Benutzeroberfläche zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="7eb08-130">Ember. js ist eine JavaScript-Bibliothek, die ein Client seitiges MVC-Muster verwendet.</span><span class="sxs-lookup"><span data-stu-id="7eb08-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="7eb08-131">Eine *Vorlage*, die in der Vorlagen Sprache des handbars geschrieben ist, beschreibt die Benutzeroberfläche der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7eb08-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="7eb08-132">Im Releasemodus wird der Compiler für den [Lenker](https://github.com/Myslik/csharp-ember-handlebars) verwendet, um die Vorlage für den Lenker zu bündeln und zu kompilieren.</span><span class="sxs-lookup"><span data-stu-id="7eb08-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="7eb08-133">Ein *Modell* speichert die Anwendungsdaten, die vom Server abgerufen werden (TODO-Listen und TODO-Elemente).</span><span class="sxs-lookup"><span data-stu-id="7eb08-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="7eb08-134">Ein *Controller* speichert den Anwendungs Zustand.</span><span class="sxs-lookup"><span data-stu-id="7eb08-134">A *controller* stores application state.</span></span> <span data-ttu-id="7eb08-135">Controller stellen häufig Modelldaten für die entsprechenden Vorlagen dar.</span><span class="sxs-lookup"><span data-stu-id="7eb08-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="7eb08-136">Eine *Sicht* übersetzt primitive Ereignisse aus der Anwendung und übergibt diese an den Controller.</span><span class="sxs-lookup"><span data-stu-id="7eb08-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="7eb08-137">Ein *Router* verwaltet den Anwendungs Status, wobei URLs und Vorlagen synchron bleiben.</span><span class="sxs-lookup"><span data-stu-id="7eb08-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="7eb08-138">Außerdem kann die Ember-Datenbibliothek verwendet werden, um JSON-Objekte (die vom Server über eine Rest-API abgerufen werden) und die Client Modelle zu synchronisieren.</span><span class="sxs-lookup"><span data-stu-id="7eb08-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="7eb08-139">Die Vorlage emberjs Spa organisiert die Skripts in acht Ebenen:</span><span class="sxs-lookup"><span data-stu-id="7eb08-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="7eb08-140">WebAPI\_Adapter. js, WebAPI\_serializer. js: erweitert die Ember-Datenbibliothek, um mit ASP.net-Web-API zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="7eb08-141">Scripts/Hilfsprogramme. js: definiert neue Ember-Lenker Hilfen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="7eb08-142">Scripts/app. js: erstellt die APP und konfiguriert den Adapter und das Serialisierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="7eb08-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="7eb08-143">Scripts/App/Models/\*. js: definiert die Modelle.</span><span class="sxs-lookup"><span data-stu-id="7eb08-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="7eb08-144">Scripts/App/views/\*. js: definiert die Ansichten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="7eb08-145">Scripts/App/Controllers/\*. js: definiert die Controller.</span><span class="sxs-lookup"><span data-stu-id="7eb08-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="7eb08-146">Scripts/App/Routes, Scripts/App/Router. js: definiert die Routen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="7eb08-147">Templates/\*. HSB: definiert die Lenker Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="7eb08-148">Betrachten wir einige dieser Skripts ausführlicher.</span><span class="sxs-lookup"><span data-stu-id="7eb08-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="7eb08-149">Modelle</span><span class="sxs-lookup"><span data-stu-id="7eb08-149">Models</span></span>

<span data-ttu-id="7eb08-150">Die Modelle werden im Ordner "Scripts/App/Models" definiert.</span><span class="sxs-lookup"><span data-stu-id="7eb08-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="7eb08-151">Es gibt zwei Modelldateien: "dedoitem. js" und "dedolist. js".</span><span class="sxs-lookup"><span data-stu-id="7eb08-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="7eb08-152">**TODO. Model. js** definiert die Client seitigen (Browser-) Modelle für die Aufgabenlisten.</span><span class="sxs-lookup"><span data-stu-id="7eb08-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="7eb08-153">Es gibt zwei Modellklassen: "tdoitem" und "tdolist".</span><span class="sxs-lookup"><span data-stu-id="7eb08-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="7eb08-154">Im Ember sind Modelle Unterklassen von DS. Modells.</span><span class="sxs-lookup"><span data-stu-id="7eb08-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="7eb08-155">Ein Modell kann über Eigenschaften mit Attributen verfügen:</span><span class="sxs-lookup"><span data-stu-id="7eb08-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="7eb08-156">Modelle können Beziehungen zu anderen Modellen definieren:</span><span class="sxs-lookup"><span data-stu-id="7eb08-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="7eb08-157">Modelle können über berechnete Eigenschaften verfügen, die an andere Eigenschaften gebunden werden:</span><span class="sxs-lookup"><span data-stu-id="7eb08-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="7eb08-158">Modelle können Beobachter Funktionen aufweisen, die aufgerufen werden, wenn sich eine beobachtete Eigenschaft ändert:</span><span class="sxs-lookup"><span data-stu-id="7eb08-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="7eb08-159">Ansichten</span><span class="sxs-lookup"><span data-stu-id="7eb08-159">Views</span></span>

<span data-ttu-id="7eb08-160">Die Sichten werden im Ordner Scripts/App/views definiert.</span><span class="sxs-lookup"><span data-stu-id="7eb08-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="7eb08-161">Eine Ansicht übersetzt Ereignisse aus der Benutzeroberfläche der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7eb08-161">A view translates events from the application UI.</span></span> <span data-ttu-id="7eb08-162">Ein Ereignishandler kann an Controller Funktionen zurückgreifen oder den Datenkontext einfach direkt aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="7eb08-163">Der folgende Code ist z. b. aus views/"" "" "" "" "" "" ""</span><span class="sxs-lookup"><span data-stu-id="7eb08-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="7eb08-164">Definiert die Ereignis Behandlung für ein Eingabe Textfeld.</span><span class="sxs-lookup"><span data-stu-id="7eb08-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="7eb08-165">Controller</span><span class="sxs-lookup"><span data-stu-id="7eb08-165">Controller</span></span>

<span data-ttu-id="7eb08-166">Die Controller werden im Ordner Skripts/App/Controller definiert.</span><span class="sxs-lookup"><span data-stu-id="7eb08-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="7eb08-167">Erweitern Sie `Ember.ObjectController`, um ein einzelnes Modell darzustellen:</span><span class="sxs-lookup"><span data-stu-id="7eb08-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="7eb08-168">Ein Controller kann auch eine Sammlung von Modellen darstellen, indem `Ember.ArrayController`erweitert wird.</span><span class="sxs-lookup"><span data-stu-id="7eb08-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="7eb08-169">So stellt z. b. "-Objektlisten Controller" ein Array von `todoList`-Objekten dar.</span><span class="sxs-lookup"><span data-stu-id="7eb08-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="7eb08-170">Der Controller sortiert nach ToDoList-ID in absteigender Reihenfolge:</span><span class="sxs-lookup"><span data-stu-id="7eb08-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="7eb08-171">Der Controller definiert eine Funktion mit dem Namen `addTodoList`, mit der eine neue ""-Liste erstellt und dem Array hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="7eb08-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="7eb08-172">Um zu sehen, wie diese Funktion aufgerufen wird, öffnen Sie die Vorlagen Datei namens todolisttemplate. html im Ordner Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="7eb08-173">Der folgende Vorlagen Code bindet eine Schaltfläche an die `addTodoList`-Funktion:</span><span class="sxs-lookup"><span data-stu-id="7eb08-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="7eb08-174">Der Controller enthält auch eine `error`-Eigenschaft, die eine Fehlermeldung enthält.</span><span class="sxs-lookup"><span data-stu-id="7eb08-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="7eb08-175">Im folgenden finden Sie den Vorlagen Code zum Anzeigen der Fehlermeldung (auch in "" "" "" "" "" "" ""</span><span class="sxs-lookup"><span data-stu-id="7eb08-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="7eb08-176">Routen</span><span class="sxs-lookup"><span data-stu-id="7eb08-176">Routes</span></span>

<span data-ttu-id="7eb08-177">"Router. js" definiert die Routen und die anzuzeigende Standardvorlage, richtet den Anwendungs Zustand ein und passt URLs zu Routen an:</span><span class="sxs-lookup"><span data-stu-id="7eb08-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="7eb08-178">"Dedolistroute. js" lädt Daten für "dedolistroute", indem die Funktion "Setupcontroller" überschrieben wird:</span><span class="sxs-lookup"><span data-stu-id="7eb08-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="7eb08-179">Ember verwendet Benennungs Konventionen, um URLs, Routennamen, Controller und Vorlagen abzugleichen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="7eb08-180">Weitere Informationen finden Sie unter [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) in der emberjs-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7eb08-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="7eb08-181">Vorlagen</span><span class="sxs-lookup"><span data-stu-id="7eb08-181">Templates</span></span>

<span data-ttu-id="7eb08-182">Der Ordner "Templates" enthält vier Vorlagen:</span><span class="sxs-lookup"><span data-stu-id="7eb08-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="7eb08-183">Application. HSB: die Standardvorlage, die gerendert wird, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="7eb08-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="7eb08-184">about. HSB: die Vorlage für die Route "/about".</span><span class="sxs-lookup"><span data-stu-id="7eb08-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="7eb08-185">Index. HSB: die Vorlage für die "/"-Stamm Route.</span><span class="sxs-lookup"><span data-stu-id="7eb08-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="7eb08-186">"tdolist. HSB": die Vorlage für die Route "/ToDo".</span><span class="sxs-lookup"><span data-stu-id="7eb08-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="7eb08-187">\_navbar. HSB: die Vorlage definiert das Navigationsmenü.</span><span class="sxs-lookup"><span data-stu-id="7eb08-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="7eb08-188">Die Anwendungs Vorlage verhält sich wie eine Master Seite.</span><span class="sxs-lookup"><span data-stu-id="7eb08-188">The application template acts like a master page.</span></span> <span data-ttu-id="7eb08-189">Sie enthält eine Kopfzeile, eine Fußzeile und ein "{{Outlet}}", um in Abhängigkeit von der Route andere Vorlagen in einzufügen.</span><span class="sxs-lookup"><span data-stu-id="7eb08-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="7eb08-190">Weitere Informationen zu Anwendungs Vorlagen in Ember finden Sie unter [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="7eb08-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="7eb08-191">Die Vorlage "/todoList" enthält zwei Schleifen Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="7eb08-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="7eb08-192">Die äußere Schleife ist `{{#each controller}}`, und die innere Schleife wird `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="7eb08-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="7eb08-193">Der folgende Code zeigt eine integrierte `Ember.Checkbox` Sicht, eine angepasste `App.TodoItemEditView`und eine Verknüpfung mit einer `deleteTodo` Aktion.</span><span class="sxs-lookup"><span data-stu-id="7eb08-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="7eb08-194">Die `HtmlHelperExtensions`-Klasse, die in Controllers/htmlhelperextensions. cs definiert ist, definiert eine Hilfsfunktion zum Zwischenspeichern und Einfügen von Vorlagen Dateien, wenn **Debug** in der Datei "Web. config" auf " **true** " festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="7eb08-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExtensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="7eb08-195">Diese Funktion wird von der ASP.NET MVC-Ansichts Datei aufgerufen, die in views/Home/app. cshtml definiert ist:</span><span class="sxs-lookup"><span data-stu-id="7eb08-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="7eb08-196">Ohne Argumente aufgerufen, rendert die Funktion alle Vorlagen Dateien im Vorlagen Ordner.</span><span class="sxs-lookup"><span data-stu-id="7eb08-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="7eb08-197">Sie können auch einen Unterordner oder eine bestimmte Vorlagen Datei angeben.</span><span class="sxs-lookup"><span data-stu-id="7eb08-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="7eb08-198">Wenn **Debug** in "Web. config" den Wert " **false** " hat, enthält die Anwendung das Paket Element "~/Bundles/Templates".</span><span class="sxs-lookup"><span data-stu-id="7eb08-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="7eb08-199">Dieses Bündel Element wird in BundleConfig.cs mithilfe der compilerbibliothek hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="7eb08-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
