---
uid: single-page-application/overview/templates/hottowel-template
title: Vorlage für Hot-Handtuch | Microsoft-Dokumentation
author: madskristensen
description: Hothandtuch-Vorlage
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075059"
---
# <a name="hot-towel-template"></a><span data-ttu-id="83b4b-103">Hot Towel-Vorlage</span><span class="sxs-lookup"><span data-stu-id="83b4b-103">Hot Towel template</span></span>

<span data-ttu-id="83b4b-104">von [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="83b4b-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="83b4b-105">Die MVC-Vorlage "Hot Handtuch" wird von John Papa verfasst.</span><span class="sxs-lookup"><span data-stu-id="83b4b-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="83b4b-106">Wählen Sie die herunter zuladbare Version aus:</span><span class="sxs-lookup"><span data-stu-id="83b4b-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="83b4b-107">Hot-Handtuch-MVC-Vorlage für Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="83b4b-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="83b4b-108">MVC-Vorlage für das Hot-Handtuch für Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="83b4b-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="83b4b-109">Heißes Handtuch: da Sie nicht in die Spa wechseln möchten.</span><span class="sxs-lookup"><span data-stu-id="83b4b-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>

<span data-ttu-id="83b4b-110">Möchten Sie eine Spa erstellen, aber nicht entscheiden, wo Sie beginnen sollen?</span><span class="sxs-lookup"><span data-stu-id="83b4b-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="83b4b-111">Verwenden Sie das heiße Handtuch, und in Sekunden haben Sie eine Spa und alle Tools, die Sie für die Erstellung benötigen!</span><span class="sxs-lookup"><span data-stu-id="83b4b-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="83b4b-112">Hot-Handtuch ist ein idealer Ausgangspunkt für das Erstellen einer Single-Page-Anwendung (Single Page Application, Spa) mit ASP.net.</span><span class="sxs-lookup"><span data-stu-id="83b4b-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="83b4b-113">Standardmäßig verfügen Sie über eine modulare Struktur für Ihren Code, die Navigationsansicht, die Datenbindung, die umfassende Datenverwaltung und einfache, aber elegante Formate.</span><span class="sxs-lookup"><span data-stu-id="83b4b-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="83b4b-114">Hot-Handtuch bietet alles, was Sie zum Erstellen einer Spa benötigen, damit Sie sich auf Ihre APP konzentrieren können, nicht auf die Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="83b4b-115">Erfahren Sie mehr über das Entwickeln einer Spa aus [den Videos, Tutorials und Pluralsight-Kursen von John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="83b4b-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>

## <a name="application-structure"></a><span data-ttu-id="83b4b-116">Anwendungsstruktur</span><span class="sxs-lookup"><span data-stu-id="83b4b-116">Application Structure</span></span>

<span data-ttu-id="83b4b-117">Hot Handtuch Spa bietet einen app-Ordner, der die JavaScript-und HTML-Dateien enthält, die Ihre Anwendung definieren.</span><span class="sxs-lookup"><span data-stu-id="83b4b-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="83b4b-118">Im App-Ordner:</span><span class="sxs-lookup"><span data-stu-id="83b4b-118">Inside the App folder:</span></span>

- <span data-ttu-id="83b4b-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="83b4b-119">durandal</span></span>
- <span data-ttu-id="83b4b-120">services</span><span class="sxs-lookup"><span data-stu-id="83b4b-120">services</span></span>
- <span data-ttu-id="83b4b-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="83b4b-121">viewmodels</span></span>
- <span data-ttu-id="83b4b-122">views</span><span class="sxs-lookup"><span data-stu-id="83b4b-122">views</span></span>

<span data-ttu-id="83b4b-123">Der app-Ordner enthält eine Sammlung von Modulen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="83b4b-124">Diese Module Kapseln Funktionen und deklarieren Abhängigkeiten von anderen Modulen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="83b4b-125">Der Ordner "Views" enthält den HTML-Code für Ihre Anwendung, und der Ordner "ViewModels" enthält die Präsentationslogik für die Ansichten (ein gängiges MVVM-Muster).</span><span class="sxs-lookup"><span data-stu-id="83b4b-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="83b4b-126">Der Ordner Dienste eignet sich ideal für alle gängigen Dienste, die Ihre Anwendung möglicherweise benötigt, z. b. http-Datenabruf oder lokale Speicher Interaktion.</span><span class="sxs-lookup"><span data-stu-id="83b4b-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="83b4b-127">Es ist üblich, dass mehrere ViewModels Code aus den Dienst Modulen wieder verwenden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="83b4b-128">ASP.NET MVC-Server seitige Anwendungs Struktur</span><span class="sxs-lookup"><span data-stu-id="83b4b-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="83b4b-129">Hot-Handtuch baut auf der vertrauten und leistungsstarken ASP.NET MVC-Struktur auf.</span><span class="sxs-lookup"><span data-stu-id="83b4b-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="83b4b-130">App-\_starten</span><span class="sxs-lookup"><span data-stu-id="83b4b-130">App\_Start</span></span>
- <span data-ttu-id="83b4b-131">Inhalt</span><span class="sxs-lookup"><span data-stu-id="83b4b-131">Content</span></span>
- <span data-ttu-id="83b4b-132">Controllers</span><span class="sxs-lookup"><span data-stu-id="83b4b-132">Controllers</span></span>
- <span data-ttu-id="83b4b-133">Modelle</span><span class="sxs-lookup"><span data-stu-id="83b4b-133">Models</span></span>
- <span data-ttu-id="83b4b-134">Skripts</span><span class="sxs-lookup"><span data-stu-id="83b4b-134">Scripts</span></span>
- <span data-ttu-id="83b4b-135">Sichten</span><span class="sxs-lookup"><span data-stu-id="83b4b-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="83b4b-136">Ausgestattete Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="83b4b-136">Featured Libraries</span></span>

- <span data-ttu-id="83b4b-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="83b4b-137">ASP.NET MVC</span></span>
- <span data-ttu-id="83b4b-138">ASP.NET-Web-API</span><span class="sxs-lookup"><span data-stu-id="83b4b-138">ASP.NET Web API</span></span>
- <span data-ttu-id="83b4b-139">ASP.net-Weboptimierung: Bündelung und Minimierung</span><span class="sxs-lookup"><span data-stu-id="83b4b-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="83b4b-140">[Breeze. js](http://Breezejs.com) : umfassende Datenverwaltung</span><span class="sxs-lookup"><span data-stu-id="83b4b-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="83b4b-141">[Durandal. js](http://Durandaljs.com) -Navigation und Ansichts Komposition</span><span class="sxs-lookup"><span data-stu-id="83b4b-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="83b4b-142">[Knockout. js](http://Knockoutjs.com) -Daten Bindungen</span><span class="sxs-lookup"><span data-stu-id="83b4b-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="83b4b-143">[Erfordern von. js](http://requirejs.org) -Modularität mit AMD und Optimierung</span><span class="sxs-lookup"><span data-stu-id="83b4b-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="83b4b-144">" [Deastr. js](http://jpapa.me/c7toastr) "-Popup Meldungen</span><span class="sxs-lookup"><span data-stu-id="83b4b-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="83b4b-145">[Twitter-Bootstrap](https://twitter.github.com/bootstrap/) -robustes CSS-Format</span><span class="sxs-lookup"><span data-stu-id="83b4b-145">[Twitter Bootstrap](https://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="83b4b-146">Installieren über die Visual Studio 2012-Hot-Handtuch-Spa-Vorlage</span><span class="sxs-lookup"><span data-stu-id="83b4b-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="83b4b-147">Hot-Handtuch kann als Visual Studio 2012-Vorlage installiert werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="83b4b-148">Klicken Sie einfach auf `File` | `New Project`, und wählen Sie `ASP.NET MVC 4 Web Application`aus.</span><span class="sxs-lookup"><span data-stu-id="83b4b-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="83b4b-149">Wählen Sie dann die Vorlage "Hot Handtuch Single Page Application" aus, und führen Sie aus.</span><span class="sxs-lookup"><span data-stu-id="83b4b-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="83b4b-150">Installieren über das nuget-Paket</span><span class="sxs-lookup"><span data-stu-id="83b4b-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="83b4b-151">Hot-Handtuch ist auch ein nuget-Paket, das ein vorhandenes leeres ASP.NET MVC-Projekt erweitert.</span><span class="sxs-lookup"><span data-stu-id="83b4b-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="83b4b-152">Installieren Sie einfach mithilfe von nuget, und führen Sie dann aus.</span><span class="sxs-lookup"><span data-stu-id="83b4b-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="83b4b-153">Wie erstelle ich ein heißes Handtuch?</span><span class="sxs-lookup"><span data-stu-id="83b4b-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="83b4b-154">Fügen Sie einfach Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="83b4b-154">Simply start adding code!</span></span>

1. <span data-ttu-id="83b4b-155">Fügen Sie Ihren eigenen serverseitigen Code hinzu, vorzugsweise Entity Framework und WebAPI (was mit Breeze. js wirklich glänzt).</span><span class="sxs-lookup"><span data-stu-id="83b4b-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="83b4b-156">Hinzufügen von Ansichten zum Ordner "`App/views`"</span><span class="sxs-lookup"><span data-stu-id="83b4b-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="83b4b-157">Hinzufügen von ViewModels zum Ordner "`App/viewmodels`"</span><span class="sxs-lookup"><span data-stu-id="83b4b-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="83b4b-158">Hinzufügen von HTML-und Knockout-Daten Bindungen zu ihren neuen Ansichten</span><span class="sxs-lookup"><span data-stu-id="83b4b-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="83b4b-159">Aktualisieren der Navigations Routen in `shell.js`</span><span class="sxs-lookup"><span data-stu-id="83b4b-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="83b4b-160">Exemplarische Vorgehensweise für HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="83b4b-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="83b4b-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="83b4b-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="83b4b-162">Index. cshtml ist die Ausgangs Route und-Ansicht für die MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="83b4b-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="83b4b-163">Sie enthält alle standardmeta-Tags, CSS-Links und JavaScript-Verweise, die erwartet werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="83b4b-164">Der Text enthält ein einzelnes `<div>` in dem der gesamte Inhalt (ihre Ansichten) bei der Anforderung platziert wird.</span><span class="sxs-lookup"><span data-stu-id="83b4b-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="83b4b-165">Der `@Scripts.Render` verwendet "require. js", um den Einstiegspunkt für den Anwendungscode auszuführen, der in der `main.js`-Datei enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="83b4b-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="83b4b-166">Ein Begrüßungsbildschirm wird bereitgestellt, um zu veranschaulichen, wie ein Begrüßungsbildschirm erstellt wird, während Ihre APP geladen wird.</span><span class="sxs-lookup"><span data-stu-id="83b4b-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="83b4b-167">App/Main. js</span><span class="sxs-lookup"><span data-stu-id="83b4b-167">App/main.js</span></span>

<span data-ttu-id="83b4b-168">Die `main.js` Datei enthält den Code, der ausgeführt wird, sobald die App geladen wird.</span><span class="sxs-lookup"><span data-stu-id="83b4b-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="83b4b-169">An dieser Stelle möchten Sie Ihre Navigations Routen definieren, die Start Ansichten festlegen und Setup/Bootstrap ausführen, wie z. b. die Daten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="83b4b-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="83b4b-170">Die `main.js` Datei definiert mehrere der Durandal-Module, um den Start der Anwendung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="83b4b-171">Mit der define-Anweisung können die Modul Abhängigkeiten aufgelöst werden, sodass Sie für die Funktion verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="83b4b-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="83b4b-172">Zuerst werden die Debugmeldungen aktiviert, die Nachrichten zu den Ereignissen senden, die von der Anwendung im Konsolenfenster durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="83b4b-173">Der app. Start-Code weist Durandal Framework an, die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="83b4b-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="83b4b-174">Die Konventionen sind so festgelegt, dass Durandal weiß, dass alle Ansichten und ViewModels in denselben benannten Ordnern enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="83b4b-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="83b4b-175">Zum Schluss lädt der `app.setRoot` die `shell` mithilfe einer vordefinierten `entrance` Animation.</span><span class="sxs-lookup"><span data-stu-id="83b4b-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="83b4b-176">Sichten</span><span class="sxs-lookup"><span data-stu-id="83b4b-176">Views</span></span>

<span data-ttu-id="83b4b-177">Sichten befinden sich im Ordner "`App/views`".</span><span class="sxs-lookup"><span data-stu-id="83b4b-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="83b4b-178">Shell. html</span><span class="sxs-lookup"><span data-stu-id="83b4b-178">shell.html</span></span>

<span data-ttu-id="83b4b-179">Die `shell.html` enthält das Master Layout für den HTML-Code.</span><span class="sxs-lookup"><span data-stu-id="83b4b-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="83b4b-180">Alle anderen Ansichten werden an einer beliebigen Seite der `shell` Ansicht zusammengesetzt.</span><span class="sxs-lookup"><span data-stu-id="83b4b-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="83b4b-181">Hot-Handtuch bietet eine `shell` mit drei dieser Regionen: einen Header, einen Inhalts Bereich und eine Fußzeile.</span><span class="sxs-lookup"><span data-stu-id="83b4b-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="83b4b-182">Jede dieser Regionen wird mit Inhalt aus anderen Ansichten geladen, wenn Sie angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="83b4b-183">Die `compose` Bindungen für die Kopf-und Fußzeile sind im Hot-handtuch hart codiert, um auf die `nav`-und `footer` Ansichten zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="83b4b-184">Die Compose-Bindung für den Abschnitt `#content` ist an das aktive Element des `router` Moduls gebunden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="83b4b-185">Anders ausgedrückt: Wenn Sie auf einen Navigations Link klicken, wird die entsprechende Ansicht in diesem Bereich geladen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="83b4b-186">NAV. html</span><span class="sxs-lookup"><span data-stu-id="83b4b-186">nav.html</span></span>

<span data-ttu-id="83b4b-187">Die `nav.html` enthält die Navigationslinks für die Spa.</span><span class="sxs-lookup"><span data-stu-id="83b4b-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="83b4b-188">An dieser Stelle kann z. b. die Menüstruktur platziert werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="83b4b-189">Häufig handelt es sich hierbei um Daten gebundene (mit Knockout) an das `router` Modul, um die im `shell.js`definierte Navigation anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="83b4b-190">Knockout sucht nach den Daten Bindungs Attributen und bindet diese an das `shell` ViewModel, um die Navigations Routen anzuzeigen und eine ProgressBar (mithilfe von Twitter-Bootstrap) anzuzeigen, wenn das `router` Modul in der Mitte der Navigation von einer Ansicht zu einem anderen (siehe `router.isNavigating`) ist.</span><span class="sxs-lookup"><span data-stu-id="83b4b-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="83b4b-191">Home. html und Details. html</span><span class="sxs-lookup"><span data-stu-id="83b4b-191">home.html and details.html</span></span>

<span data-ttu-id="83b4b-192">Diese Ansichten enthalten HTML für benutzerdefinierte Ansichten.</span><span class="sxs-lookup"><span data-stu-id="83b4b-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="83b4b-193">Wenn auf den `home` Link im Menü der `nav` Ansicht geklickt wird, wird die `home` Ansicht in den Inhalts Bereich der `shell` Ansicht eingefügt.</span><span class="sxs-lookup"><span data-stu-id="83b4b-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="83b4b-194">Diese Ansichten können durch ihre eigenen benutzerdefinierten Ansichten erweitert oder ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="83b4b-195">Footer. html</span><span class="sxs-lookup"><span data-stu-id="83b4b-195">footer.html</span></span>

<span data-ttu-id="83b4b-196">Die `footer.html` enthält HTML, das in der Fußzeile unten in der `shell` Ansicht angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="83b4b-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="83b4b-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="83b4b-197">ViewModels</span></span>

<span data-ttu-id="83b4b-198">ViewModels finden Sie im Ordner "`App/viewmodels`".</span><span class="sxs-lookup"><span data-stu-id="83b4b-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="83b4b-199">Shell. js</span><span class="sxs-lookup"><span data-stu-id="83b4b-199">shell.js</span></span>

<span data-ttu-id="83b4b-200">Das `shell` ViewModel enthält Eigenschaften und Funktionen, die an die `shell` Ansicht gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="83b4b-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="83b4b-201">Häufig werden die Menü Navigations Bindungen gefunden (Weitere Informationen finden Sie in der `router.mapNav` Logik).</span><span class="sxs-lookup"><span data-stu-id="83b4b-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="83b4b-202">"Home. js" und "Details. js"</span><span class="sxs-lookup"><span data-stu-id="83b4b-202">home.js and details.js</span></span>

<span data-ttu-id="83b4b-203">Diese ViewModels enthalten die Eigenschaften und Funktionen, die an die `home` Ansicht gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="83b4b-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="83b4b-204">Außerdem enthält Sie die Darstellungs Logik für die Ansicht und ist der Verbindungsaufbau zwischen den Daten und der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="83b4b-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="83b4b-205">Dienste</span><span class="sxs-lookup"><span data-stu-id="83b4b-205">Services</span></span>

<span data-ttu-id="83b4b-206">Dienste befinden sich im Ordner "App/Dienste".</span><span class="sxs-lookup"><span data-stu-id="83b4b-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="83b4b-207">Im Idealfall könnten Ihre zukünftigen Dienste wie z. b. ein DataService-Modul, das für das Senden und Veröffentlichen von Remote Daten zuständig ist, platziert werden.</span><span class="sxs-lookup"><span data-stu-id="83b4b-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="83b4b-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="83b4b-208">logger.js</span></span>

<span data-ttu-id="83b4b-209">Hot-Handtuch stellt ein `logger` Modul im Ordner "Dienste" bereit.</span><span class="sxs-lookup"><span data-stu-id="83b4b-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="83b4b-210">Das `logger`-Modul eignet sich ideal für das Protokollieren von Nachrichten in der Konsole und an den Benutzer in Popup-Protokollen.</span><span class="sxs-lookup"><span data-stu-id="83b4b-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
