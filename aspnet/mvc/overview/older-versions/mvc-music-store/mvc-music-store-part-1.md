---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Teil 1: Übersicht und Datei > Neues Projekt | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. Teil 1 umfasst Übersicht und Datei > Neues Projekt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451281"
---
# <a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="7e71a-104">Teil 1: Übersicht und Datei > Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="7e71a-104">Part 1: Overview and File->New Project</span></span>

<span data-ttu-id="7e71a-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7e71a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7e71a-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7e71a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7e71a-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="7e71a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="7e71a-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="7e71a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7e71a-109">Teil 1 umfasst Übersicht und Datei&gt;neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="7e71a-109">Part 1 covers Overview and File-&gt;New Project.</span></span>

## <a name="overview"></a><span data-ttu-id="7e71a-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7e71a-110">Overview</span></span>

<span data-ttu-id="7e71a-111">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Web Developer für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7e71a-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="7e71a-112">Wir fangen langsam an, sodass die Webentwicklung für Einsteiger in Ordnung ist.</span><span class="sxs-lookup"><span data-stu-id="7e71a-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="7e71a-113">Die Anwendung, die wir aufbauen, ist ein einfacher Music Store.</span><span class="sxs-lookup"><span data-stu-id="7e71a-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="7e71a-114">Es gibt drei Hauptkomponenten für die Anwendung: "Shopping", "Checkout" und "Administration".</span><span class="sxs-lookup"><span data-stu-id="7e71a-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="7e71a-115">Besucher können Alben nach Genre durchsuchen:</span><span class="sxs-lookup"><span data-stu-id="7e71a-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="7e71a-116">Sie können ein einzelnes Album anzeigen und dem Warenkorb hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="7e71a-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="7e71a-117">Sie können Ihren Warenkorb überprüfen und alle Elemente entfernen, die Sie nicht mehr benötigen:</span><span class="sxs-lookup"><span data-stu-id="7e71a-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="7e71a-118">Wenn Sie das Auschecken fortsetzen, werden Sie zur Anmeldung oder Registrierung für ein Benutzerkonto aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="7e71a-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="7e71a-119">Nachdem Sie ein Konto erstellt haben, können Sie die Bestellung vervollständigen, indem Sie Versand-und Zahlungsinformationen ausfüllen.</span><span class="sxs-lookup"><span data-stu-id="7e71a-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="7e71a-120">Um dies zu gewährleisten, führen wir eine beeindruckende herauf Stufung durch: alles kostenlos, wenn Sie den Promotioncode "Free" eingeben!</span><span class="sxs-lookup"><span data-stu-id="7e71a-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="7e71a-121">Nach der Bestellung wird ein einfacher Bestätigungsbildschirm angezeigt:</span><span class="sxs-lookup"><span data-stu-id="7e71a-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="7e71a-122">Zusätzlich zu den kundenseitigen Seiten erstellen wir auch einen Administrator Abschnitt, der eine Liste der Alben anzeigt, von denen Administratoren Alben erstellen, bearbeiten und löschen können:</span><span class="sxs-lookup"><span data-stu-id="7e71a-122">In addition to customer-facing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="7e71a-123">1. Datei&gt; neues Projekt</span><span class="sxs-lookup"><span data-stu-id="7e71a-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="7e71a-124">Installieren der Software</span><span class="sxs-lookup"><span data-stu-id="7e71a-124">Installing the software</span></span>

<span data-ttu-id="7e71a-125">In diesem Tutorial wird zunächst ein neues ASP.NET MVC 3-Projekt mit dem kostenlosen Visual Web Developer 2010 Express (kostenlos) erstellt. Anschließend fügen wir die Features inkrementell hinzu, um eine komplette funktionsfähige Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7e71a-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="7e71a-126">Auf diese Weise werden Datenbankzugriffe, Formular Bereitstellungs Szenarien, die Datenüberprüfung, die Verwendung von Masterseiten für konsistentes Seitenlayout, die Verwendung von AJAX für Seiten Aktualisierungen und Validierung, Benutzeranmeldung und mehr behandelt.</span><span class="sxs-lookup"><span data-stu-id="7e71a-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="7e71a-127">Sie können Schritt für Schritt ausführen, oder Sie können die abgeschlossene Anwendung aus [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store)herunterladen.</span><span class="sxs-lookup"><span data-stu-id="7e71a-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="7e71a-128">Sie können entweder Visual Studio 2010 SP1 oder Visual Web Developer 2010 Express SP1 (eine kostenlose Version von Visual Studio 2010) verwenden, um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7e71a-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="7e71a-129">Wir verwenden die SQL Server Compact (auch kostenlos) zum Hosten der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7e71a-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="7e71a-130">Stellen Sie sicher, dass Sie die unten aufgeführten Voraussetzungen installiert haben, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="7e71a-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="7e71a-131">[Erforderliche Komponenten für Visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="7e71a-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="7e71a-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="7e71a-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="7e71a-133">[SQL Server Compact 4,0]: einschließlich Laufzeit-und Tool Unterstützung</span><span class="sxs-lookup"><span data-stu-id="7e71a-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>

### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="7e71a-134">Erstellen eines neuen ASP.NET MVC 3-Projekts</span><span class="sxs-lookup"><span data-stu-id="7e71a-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="7e71a-135">Wir beginnen mit der Auswahl von "Neues Projekt" im Menü "Datei" in Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="7e71a-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="7e71a-136">Dadurch wird das Dialogfeld "Neues Projekt" geöffnet.</span><span class="sxs-lookup"><span data-stu-id="7e71a-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="7e71a-137">Wählen Sie auf der linken C# Seite die Visual&gt;-Webvorlagen Gruppe aus, und wählen Sie dann die Vorlage "ASP.NET MVC 3-Webanwendung" in der mittleren Spalte aus.</span><span class="sxs-lookup"><span data-stu-id="7e71a-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="7e71a-138">Nennen Sie Ihr Projekt mvcmusicstore, und klicken Sie auf die Schaltfläche OK.</span><span class="sxs-lookup"><span data-stu-id="7e71a-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="7e71a-139">Dadurch wird ein sekundäres Dialogfeld angezeigt, in dem wir einige MVC-spezifische Einstellungen für das Projekt erstellen können.</span><span class="sxs-lookup"><span data-stu-id="7e71a-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="7e71a-140">Wählen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="7e71a-140">Select the following:</span></span>

<span data-ttu-id="7e71a-141">Projektvorlage: Wählen Sie leer aus.</span><span class="sxs-lookup"><span data-stu-id="7e71a-141">Project Template - select Empty</span></span>

<span data-ttu-id="7e71a-142">Engine anzeigen: Wählen Sie Razor aus.</span><span class="sxs-lookup"><span data-stu-id="7e71a-142">View Engine - select Razor</span></span>

<span data-ttu-id="7e71a-143">HTML5-Semantik Markup aktivieren (aktiviert)</span><span class="sxs-lookup"><span data-stu-id="7e71a-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="7e71a-144">Stellen Sie sicher, dass Ihre Einstellungen wie unten dargestellt lauten, und klicken Sie dann auf die Schaltfläche OK.</span><span class="sxs-lookup"><span data-stu-id="7e71a-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="7e71a-145">Dadurch wird das Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="7e71a-145">This will create our project.</span></span> <span data-ttu-id="7e71a-146">Werfen wir einen Blick auf die Ordner, die der Anwendung in der Projektmappen-Explorer auf der rechten Seite hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="7e71a-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="7e71a-147">Die leere MVC 3-Vorlage ist nicht vollständig leer – Sie fügt eine grundlegende Ordnerstruktur hinzu:</span><span class="sxs-lookup"><span data-stu-id="7e71a-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="7e71a-148">ASP.NET MVC nutzt einige grundlegende Benennungs Konventionen für Ordnernamen:</span><span class="sxs-lookup"><span data-stu-id="7e71a-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="7e71a-149">**Ordner**</span><span class="sxs-lookup"><span data-stu-id="7e71a-149">**Folder**</span></span> | <span data-ttu-id="7e71a-150">**Zweck**</span><span class="sxs-lookup"><span data-stu-id="7e71a-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="7e71a-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="7e71a-151">**/Controllers**</span></span> | <span data-ttu-id="7e71a-152">Controller reagieren auf Eingaben aus dem Browser, entscheiden, was mit der Anwendung geschehen soll, und geben eine Antwort an den Benutzer zurück.</span><span class="sxs-lookup"><span data-stu-id="7e71a-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="7e71a-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="7e71a-153">**/Views**</span></span> | <span data-ttu-id="7e71a-154">Ansichten enthalten unsere UI-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="7e71a-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="7e71a-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="7e71a-155">**/Models**</span></span> | <span data-ttu-id="7e71a-156">Modelle enthalten und bearbeiten Daten</span><span class="sxs-lookup"><span data-stu-id="7e71a-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="7e71a-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="7e71a-157">**/Content**</span></span> | <span data-ttu-id="7e71a-158">Dieser Ordner enthält unsere Images, CSS und andere statische Inhalte.</span><span class="sxs-lookup"><span data-stu-id="7e71a-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="7e71a-159">**"/Scripts"**</span><span class="sxs-lookup"><span data-stu-id="7e71a-159">**/Scripts**</span></span> | <span data-ttu-id="7e71a-160">Dieser Ordner enthält die JavaScript-Dateien.</span><span class="sxs-lookup"><span data-stu-id="7e71a-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="7e71a-161">Diese Ordner sind auch in einer leeren ASP.NET MVC-Anwendung enthalten, da das ASP.NET-MVC-Framework standardmäßig eine Konvention für die Konfiguration verwendet und einige Standard Annahmen basierend auf den Benennungs Konventionen für Ordner vornimmt.</span><span class="sxs-lookup"><span data-stu-id="7e71a-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="7e71a-162">Beispielsweise suchen Controller standardmäßig nach Ansichten im Ordner "Views", ohne dass Sie diese explizit in Ihrem Code angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="7e71a-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="7e71a-163">Wenn Sie mit den Standard Konventionen arbeiten, verringert sich der Code, den Sie schreiben müssen, und kann auch anderen Entwicklern das Verständnis ihres Projekts erleichtern.</span><span class="sxs-lookup"><span data-stu-id="7e71a-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="7e71a-164">Diese Konventionen werden bei der Erstellung unserer Anwendung ausführlicher erläutert.</span><span class="sxs-lookup"><span data-stu-id="7e71a-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7e71a-165">Weiter</span><span class="sxs-lookup"><span data-stu-id="7e71a-165">Next</span></span>](mvc-music-store-part-2.md)
