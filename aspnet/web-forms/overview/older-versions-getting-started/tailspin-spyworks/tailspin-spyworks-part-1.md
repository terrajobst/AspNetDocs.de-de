---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Teil 1: Datei > Neues Projekt | Microsoft-Dokumentation'
author: JoeStagner
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden. Teil 1 umfasst Übersicht und Datei/Neues Projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516537"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="b26a2-104">Teil 1: Datei > Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="b26a2-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="b26a2-105">von [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="b26a2-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="b26a2-106">Tailspin SpyWorks veranschaulicht, wie einfach es ist, leistungsstarke, skalierbare Anwendungen für die .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b26a2-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="b26a2-107">Es zeigt, wie die großartigen neuen Features in ASP.NET 4 verwendet werden, um einen Online Shop zu erstellen, einschließlich Einkaufs-, Checkout-und Verwaltungsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="b26a2-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="b26a2-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der Beispielanwendung Tailspin SpyWorks ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b26a2-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="b26a2-109">Teil 1 umfasst Übersicht und Datei/Neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="b26a2-109">Part 1 covers Overview and File/New Project.</span></span>

## <a id="_Toc260221666"></a><span data-ttu-id="b26a2-110">Übersicht über</span><span class="sxs-lookup"><span data-stu-id="b26a2-110">Overview</span></span>

<span data-ttu-id="b26a2-111">Dieses Tutorial ist eine Einführung in ASP.net WebForms.</span><span class="sxs-lookup"><span data-stu-id="b26a2-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="b26a2-112">Wir fangen langsam an, sodass die Webentwicklung für Einsteiger in Ordnung ist.</span><span class="sxs-lookup"><span data-stu-id="b26a2-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="b26a2-113">Die Anwendung, die wir aufbauen werden, ist ein einfacher Online-Speicher.</span><span class="sxs-lookup"><span data-stu-id="b26a2-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)

<span data-ttu-id="b26a2-114">Besucher können Produkte nach Kategorie durchsuchen:</span><span class="sxs-lookup"><span data-stu-id="b26a2-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="b26a2-115">Sie können ein einzelnes Produktanzeigen und zum Warenkorb hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="b26a2-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="b26a2-116">Sie können Ihren Warenkorb überprüfen und alle Elemente entfernen, die Sie nicht mehr benötigen:</span><span class="sxs-lookup"><span data-stu-id="b26a2-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="b26a2-117">Beim Auschecken werden Sie zur Eingabe aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="b26a2-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="b26a2-118">Nach der Bestellung wird ein einfacher Bestätigungsbildschirm angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b26a2-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)

<span data-ttu-id="b26a2-119">Wir beginnen mit der Erstellung eines neuen ASP.net WebForms-Projekts in Visual Studio 2010, und wir fügen inkrementell Features hinzu, um eine komplette funktionsfähige Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b26a2-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="b26a2-120">Dabei werden die Datenbankzugriffs-, Listen-und Raster Ansichten, Daten Aktualisierungs Seiten, Datenvalidierung, die Verwendung von Masterseiten für konsistentes Seitenlayout, AJAX, Validierung, Benutzer Mitgliedschaft und mehr behandelt.</span><span class="sxs-lookup"><span data-stu-id="b26a2-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="b26a2-121">Sie können Schritt für Schritt ausführen, oder Sie können die abgeschlossene Anwendung von [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/) herunterladen.</span><span class="sxs-lookup"><span data-stu-id="b26a2-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="b26a2-122">Sie können entweder Visual Studio 2010 oder den kostenlosen Visual Web Developer 2010 aus [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)verwenden.</span><span class="sxs-lookup"><span data-stu-id="b26a2-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="b26a2-123">Zum Erstellen der Anwendung können Sie entweder SQL Server oder die kostenlose SQL Server Express verwenden, um die Datenbank zu hosten.</span><span class="sxs-lookup"><span data-stu-id="b26a2-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="b26a2-124">Datei/Neues Projekt</span><span class="sxs-lookup"><span data-stu-id="b26a2-124">File / New Project</span></span>

<span data-ttu-id="b26a2-125">Zunächst wählen wir das neue Projekt aus dem Menü "Datei" in Visual Studio aus.</span><span class="sxs-lookup"><span data-stu-id="b26a2-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="b26a2-126">Dadurch wird das Dialogfeld "Neues Projekt" geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b26a2-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="b26a2-127">Wählen Sie die Gruppe " C# Visual/Web-Vorlagen" auf der linken Seite aus, und wählen Sie dann die Vorlage "ASP.NET Webanwendung" in der mittleren Spalte aus.</span><span class="sxs-lookup"><span data-stu-id="b26a2-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="b26a2-128">Benennen Sie Ihr Projekt tailspinspyworks, und klicken Sie auf die Schaltfläche OK.</span><span class="sxs-lookup"><span data-stu-id="b26a2-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="b26a2-129">Dadurch wird das Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="b26a2-129">This will create our project.</span></span> <span data-ttu-id="b26a2-130">Werfen wir einen Blick auf die Ordner, die in der Anwendung in der Projektmappen-Explorer auf der rechten Seite enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="b26a2-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="b26a2-131">Die leere Projekt Mappe ist nicht vollständig leer – Sie fügt eine grundlegende Ordnerstruktur hinzu:</span><span class="sxs-lookup"><span data-stu-id="b26a2-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="b26a2-132">Beachten Sie die Konventionen, die von der Standard Projektvorlage ASP.NET 4 implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="b26a2-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="b26a2-133">Der Ordner "Account" implementiert eine einfache Benutzeroberfläche für ASP. .Net-Mitgliedschafts Subsystem.</span><span class="sxs-lookup"><span data-stu-id="b26a2-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="b26a2-134">Der Ordner "Scripts" dient als Repository für Client seitige JavaScript-Dateien, und die jQuery. js-Kerndateien werden standardmäßig zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="b26a2-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="b26a2-135">Der Ordner "Styles" wird verwendet, um die Visualisierungen unserer Websites (CSS-Stylesheets) zu organisieren.</span><span class="sxs-lookup"><span data-stu-id="b26a2-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="b26a2-136">Wenn Sie F5 drücken, um die Anwendung auszuführen und die Seite "default. aspx" zu reneln, wird Folgendes angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b26a2-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="b26a2-137">Unsere erste Anwendungs Erweiterung besteht darin, die Datei "Style. CSS" aus der standardmäßigen WebForms-Vorlage durch die CSS-Klassen und die zugehörigen Bilddateien zu ersetzen, die die visuelle Asthetik für unsere Tailspin SpyWorks-Anwendung darstellen.</span><span class="sxs-lookup"><span data-stu-id="b26a2-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="b26a2-138">Danach wird unsere default. aspx-Seite wie folgt gerendert.</span><span class="sxs-lookup"><span data-stu-id="b26a2-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="b26a2-139">Beachten Sie die Bild links oben rechts auf der Seite und die Menü Elemente, die der Master Seite hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="b26a2-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="b26a2-140">Nur die Links "Anmelden" und "Konto" verweisen auf Seiten, die vorhanden (von der Standardvorlage generiert) und auf die restlichen Seiten, die wir beim Erstellen der Anwendung implementieren werden.</span><span class="sxs-lookup"><span data-stu-id="b26a2-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="b26a2-141">Außerdem verschieben wir die Master Seite in das Verzeichnis "Stile".</span><span class="sxs-lookup"><span data-stu-id="b26a2-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="b26a2-142">Obwohl dies nur eine bevorzugte Einstellung ist, ist es möglicherweise etwas einfacher, wenn wir die Anwendung in Zukunft "Skinable" machen.</span><span class="sxs-lookup"><span data-stu-id="b26a2-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="b26a2-143">Danach müssen wir die Verweise der Master Seite in allen ASPX-Dateien ändern, die von den standardmäßigen ASP.net WebForms-Seiten generiert werden.</span><span class="sxs-lookup"><span data-stu-id="b26a2-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b26a2-144">Weiter</span><span class="sxs-lookup"><span data-stu-id="b26a2-144">Next</span></span>](tailspin-spyworks-part-2.md)
