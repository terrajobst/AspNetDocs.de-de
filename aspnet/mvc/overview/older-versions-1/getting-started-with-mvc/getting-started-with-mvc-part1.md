---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Einführung in ASP.NET MVC | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden. Erstellen Sie eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469797"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="cb480-104">Einführung in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cb480-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="cb480-105">von [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="cb480-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="cb480-106">Eine aktualisierte Version, wenn dieses [Tutorial mithilfe von](../../getting-started/introduction/getting-started.md) [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="cb480-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="cb480-107">Im neuen Tutorial wird ASP.NET MVC 5 verwendet, das in diesem Tutorial viele Verbesserungen bietet.</span><span class="sxs-lookup"><span data-stu-id="cb480-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="cb480-108">Dies ist ein Einsteiger-Tutorial, in dem die Grundlagen von ASP.NET MVC vorgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="cb480-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="cb480-109">Sie erstellen eine einfache Webanwendung, die Daten aus einer Datenbank liest und in diese schreibt.</span><span class="sxs-lookup"><span data-stu-id="cb480-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="cb480-110">Besuchen Sie das [ASP.NET MVC Learning Center](../../../index.md) , um weitere ASP.NET MVC-Tutorials und-Beispiele zu finden.</span><span class="sxs-lookup"><span data-stu-id="cb480-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="cb480-111">Machen wir unsere erste ASP.NET MVC-Webanwendung mit [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="cb480-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="cb480-112">Wir erstellen eine kleine Movie List-Anwendung, mit der Filme erstellt und aufgelistet werden können.</span><span class="sxs-lookup"><span data-stu-id="cb480-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="cb480-113">Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="cb480-113">What You'll Build</span></span>

<span data-ttu-id="cb480-114">Im folgenden finden Sie zwei Screenshots der Anwendung, die Sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="cb480-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="cb480-115">Sie verfügen über eine einfache Tabelle mit Filmen mit verschiedenen Spalten.</span><span class="sxs-lookup"><span data-stu-id="cb480-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="cb480-116">[![Movie List-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb480-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="cb480-117">Und Sie haben ein Formular erstellen, damit wir der Liste Filme hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="cb480-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="cb480-118">[![Erstellen eines Films: Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cb480-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="cb480-119">Erlernte Fertigkeiten</span><span class="sxs-lookup"><span data-stu-id="cb480-119">Skills You'll Learn</span></span>

<span data-ttu-id="cb480-120">Dieses Tutorial vermittelt Ihnen die Grundlagen der Entwicklung einer ASP.NET MVC-Webanwendung mithilfe von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb480-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="cb480-121">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cb480-121">You'll learn:</span></span>

- <span data-ttu-id="cb480-122">Erstellen eines neuen ASP.NET MVC-Projekts</span><span class="sxs-lookup"><span data-stu-id="cb480-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="cb480-123">Erstellen einer neuen Datenbank mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="cb480-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="cb480-124">Erstellen von ASP.NET-MVC-Controllern und-Ansichten</span><span class="sxs-lookup"><span data-stu-id="cb480-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="cb480-125">Abrufen und Anzeigen von Daten</span><span class="sxs-lookup"><span data-stu-id="cb480-125">How to retrieve and display data</span></span>
- <span data-ttu-id="cb480-126">Vorgehensweise beim Bearbeiten von Daten und Aktivieren der Datenvalidierung</span><span class="sxs-lookup"><span data-stu-id="cb480-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="cb480-127">Aktualisieren des Datenbankschemas</span><span class="sxs-lookup"><span data-stu-id="cb480-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="cb480-128">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="cb480-128">Get Started</span></span>

<span data-ttu-id="cb480-129">Beginnen Sie mit der Ausführung von Visual Web Developer 2010 Express (ich nenne Sie jetzt "VWD"), und wählen Sie "Neues Projekt" auf dem Start Bildschirm aus.</span><span class="sxs-lookup"><span data-stu-id="cb480-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="cb480-130">Visual Web Developer ist eine IDE oder integrierte Entwicklerumgebung.</span><span class="sxs-lookup"><span data-stu-id="cb480-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="cb480-131">Ebenso wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE zum Erstellen von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="cb480-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="cb480-132">Es gibt eine Symbolleiste am oberen Rand der verschiedenen Optionen, die Ihnen zur Verfügung stehen, sowie das Menü, das Sie auch zum Auswählen der Datei verwenden können. Neues Projekt.</span><span class="sxs-lookup"><span data-stu-id="cb480-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="cb480-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="cb480-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="cb480-134">Erstellen Ihrer ersten Anwendung</span><span class="sxs-lookup"><span data-stu-id="cb480-134">Creating Your First Application</span></span>

<span data-ttu-id="cb480-135">Anwendungen können mit Visual Basic oder Visual C#erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="cb480-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="cb480-136">Wählen Sie jetzt auf der C# linken Seite Visualisierung aus, und wählen Sie dann "ASP.NET MVC 2-Webanwendung" aus.</span><span class="sxs-lookup"><span data-stu-id="cb480-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="cb480-137">Benennen Sie Ihr Projekt "Movies", und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="cb480-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="cb480-138">[Neues Projekt ![](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cb480-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="cb480-139">Auf der rechten Seite befindet sich die Projektmappen-Explorer, in der alle Dateien und Ordner in der Anwendung angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cb480-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="cb480-140">Im großen Fenster in der Mitte bearbeiten Sie den Code und verbringen die meiste Zeit.</span><span class="sxs-lookup"><span data-stu-id="cb480-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="cb480-141">Visual Studio hat eine Standardvorlage für das ASP.NET MVC-Projekt verwendet, das Sie gerade erstellt haben, sodass Sie momentan über eine funktionierende Anwendung verfügen, ohne etwas zu tun!</span><span class="sxs-lookup"><span data-stu-id="cb480-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="cb480-142">Dies ist eine einfache "Hallo Welt!</span><span class="sxs-lookup"><span data-stu-id="cb480-142">This is a simple "Hello World!</span></span> <span data-ttu-id="cb480-143">Das Projekt ist ein guter Ausgangspunkt für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="cb480-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="cb480-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="cb480-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="cb480-145">Wählen Sie die Schaltfläche "Wiedergabe" auf der Symbolleiste aus.</span><span class="sxs-lookup"><span data-stu-id="cb480-145">Select the "play" button to the toolbar.</span></span>

![Debuggen](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="cb480-147">Es handelt sich um einen grünen Pfeil, der auf das Recht zeigt, mit dem das Programm kompiliert und die Anwendung in einem Webbrowser gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="cb480-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="cb480-148">*Hinweis: Sie können die Taste F5 auf der Tastatur drücken oder Debuggen&gt;Debuggen starten im Menü Debuggen auswählen.*</span><span class="sxs-lookup"><span data-stu-id="cb480-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="cb480-149">Dies bewirkt, dass Visual Web Developer einen Webserver für die Entwicklung startet und die Webanwendung ausgeführt wird (es sind keine Konfigurationsschritte oder manuelle Schritte erforderlich, um dies zu aktivieren).</span><span class="sxs-lookup"><span data-stu-id="cb480-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="cb480-150">Anschließend wird ein Browser gestartet und zum Durchsuchen der Startseite der Anwendung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="cb480-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="cb480-151">Beachten Sie, dass die Adressleiste des Browsers "localhost" und nicht wie "example.com" lautet.</span><span class="sxs-lookup"><span data-stu-id="cb480-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="cb480-152">Der Grund hierfür ist, dass "localhost" immer auf Ihren eigenen lokalen Computer verweist. in diesem Fall wird die Anwendung ausgeführt, die wir soeben erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="cb480-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="cb480-153">[![Startseite](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="cb480-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="cb480-154">Diese Standardvorlage enthält standardmäßig zwei zu besuchende Seiten und eine einfache Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="cb480-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="cb480-155">Ändern Sie die Funktionsweise dieser Anwendung, und erfahren Sie etwas über ASP.NET MVC im Prozess.</span><span class="sxs-lookup"><span data-stu-id="cb480-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="cb480-156">Schließen Sie Ihren Browser, und lassen Sie Code ändern.</span><span class="sxs-lookup"><span data-stu-id="cb480-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cb480-157">Weiter</span><span class="sxs-lookup"><span data-stu-id="cb480-157">Next</span></span>](getting-started-with-mvc-part2.md)
