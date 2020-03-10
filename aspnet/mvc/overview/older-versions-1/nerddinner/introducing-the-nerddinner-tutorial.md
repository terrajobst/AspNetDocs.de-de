---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Einführung in das Lernprogramm zu nerddinner | Microsoft-Dokumentation
author: shanselman
description: Die beste Möglichkeit, ein neues Framework kennenzulernen, besteht darin, etwas mit diesem Framework zu erstellen. Dieses Tutorial führt Sie durch die Schritte zum Erstellen einer kleinen, aber abgeschlossenen Anwendung mithilfe von ASP.ne...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468933"
---
# <a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="b1f15-104">Einführung zum NerdDinner-Tutorial</span><span class="sxs-lookup"><span data-stu-id="b1f15-104">Introducing the NerdDinner Tutorial</span></span>

<span data-ttu-id="b1f15-105">von [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="b1f15-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="b1f15-106">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="b1f15-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="b1f15-107">Die beste Möglichkeit, ein neues Framework kennenzulernen, besteht darin, etwas mit diesem Framework zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="b1f15-108">In diesem Tutorial wird Schritt für Schritt erläutert, wie Sie eine kleine, aber abgeschlossene Anwendung mit ASP.NET MVC 1 erstellen, und es werden einige der grundlegenden Konzepte dahinter vorgestellt.</span><span class="sxs-lookup"><span data-stu-id="b1f15-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="b1f15-109">Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-tutorial"></a><span data-ttu-id="b1f15-110">Nerddinner-Tutorial</span><span class="sxs-lookup"><span data-stu-id="b1f15-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="b1f15-111">Die beste Möglichkeit, ein neues Framework kennenzulernen, besteht darin, etwas mit diesem Framework zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="b1f15-112">Dieses Tutorial führt Sie durch die Erstellung einer kleinen, aber abgeschlossenen Anwendung mithilfe von ASP.NET MVC und führt einige der grundlegenden Konzepte dahinter ein.</span><span class="sxs-lookup"><span data-stu-id="b1f15-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="b1f15-113">Die Anwendung, die wir erstellen, wird als "nerddinner" bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b1f15-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="b1f15-114">Nerddinner bietet Benutzern eine einfache Möglichkeit, Abendessen online zu finden und zu organisieren:</span><span class="sxs-lookup"><span data-stu-id="b1f15-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="b1f15-115">Mit "nerddinner" können registrierte Benutzer Abendessen erstellen, bearbeiten und löschen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="b1f15-116">Es erzwingt einen konsistenten Satz von Validierungs-und Geschäftsregeln in der gesamten Anwendung:</span><span class="sxs-lookup"><span data-stu-id="b1f15-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="b1f15-117">Besucher können eine AJAX-basierte Karte verwenden, um nach bevorstehenden Abendessen zu suchen, die in Ihrer Nähe gehalten werden:</span><span class="sxs-lookup"><span data-stu-id="b1f15-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="b1f15-118">Wenn Sie auf ein Dinner klicken, gelangen Sie zu einer Detailseite, auf der Sie mehr darüber erfahren können:</span><span class="sxs-lookup"><span data-stu-id="b1f15-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="b1f15-119">Wenn Sie an der Teilnahme an Dinner interessiert sind, können Sie sich auf der Website anmelden oder registrieren:</span><span class="sxs-lookup"><span data-stu-id="b1f15-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="b1f15-120">Sie können dann auf einen AJAX-basierten RSVP-Link klicken, um an dem Ereignis teilzunehmen:</span><span class="sxs-lookup"><span data-stu-id="b1f15-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="b1f15-121">Implementieren von nerddinner</span><span class="sxs-lookup"><span data-stu-id="b1f15-121">Implementing NerdDinner</span></span>

<span data-ttu-id="b1f15-122">Wir beginnen mit der "nerddinner"-Anwendung, indem wir den Befehl "File-&gt;New Project" in Visual Studio verwenden, um ein neues ASP.NET MVC-Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="b1f15-123">Anschließend fügen wir Funktionen und Features inkrementell hinzu.</span><span class="sxs-lookup"><span data-stu-id="b1f15-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="b1f15-124">Dabei werden die folgenden Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="b1f15-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="b1f15-125">Erstellen eines neuen ASP.NET MVC-Projekts</span><span class="sxs-lookup"><span data-stu-id="b1f15-125">How to create a new ASP.NET MVC Project</span></span>](create-a-new-aspnet-mvc-project.md)
2. [<span data-ttu-id="b1f15-126">Erstellen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="b1f15-126">How to create a database</span></span>](create-a-database.md)
3. [<span data-ttu-id="b1f15-127">Erstellen eines Modells mit Geschäftsregel Überprüfungen</span><span class="sxs-lookup"><span data-stu-id="b1f15-127">How to build a model with business rule validations</span></span>](build-a-model-with-business-rule-validations.md)
4. [<span data-ttu-id="b1f15-128">Verwenden von Controllern und Ansichten zum Implementieren einer Auflistung/Details-Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="b1f15-128">How to use controllers and views to implement a listing/details UI</span></span>](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [<span data-ttu-id="b1f15-129">Bereitstellen von CRUD (Create, Read, Update, DELETE)-Unterstützung für Datenformular Einträge</span><span class="sxs-lookup"><span data-stu-id="b1f15-129">How to provide CRUD (create, read, update, delete) data form entry support</span></span>](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [<span data-ttu-id="b1f15-130">Verwenden von ViewData und Implementieren von ViewModel-Klassen</span><span class="sxs-lookup"><span data-stu-id="b1f15-130">How to use ViewData and implement ViewModel classes</span></span>](use-viewdata-and-implement-viewmodel-classes.md)
7. [<span data-ttu-id="b1f15-131">Wieder verwenden der Benutzeroberfläche mithilfe von Masterseiten und partialen</span><span class="sxs-lookup"><span data-stu-id="b1f15-131">How to re-use UI using master pages and partials</span></span>](re-use-ui-using-master-pages-and-partials.md)
8. [<span data-ttu-id="b1f15-132">So implementieren Sie effizientes Datenpaging</span><span class="sxs-lookup"><span data-stu-id="b1f15-132">How to implement efficient data paging</span></span>](implement-efficient-data-paging.md)
9. [<span data-ttu-id="b1f15-133">Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="b1f15-133">How to secure applications using authentication and authorization</span></span>](secure-applications-using-authentication-and-authorization.md)
10. [<span data-ttu-id="b1f15-134">Verwenden von AJAX zum bereitzustellen dynamischer Updates</span><span class="sxs-lookup"><span data-stu-id="b1f15-134">How to use AJAX to deliver dynamic updates</span></span>](use-ajax-to-deliver-dynamic-updates.md)
11. [<span data-ttu-id="b1f15-135">Verwenden von AJAX zum Implementieren von Mapping-Szenarios</span><span class="sxs-lookup"><span data-stu-id="b1f15-135">How to use AJAX to implement mapping scenarios</span></span>](use-ajax-to-implement-mapping-scenarios.md)
12. [<span data-ttu-id="b1f15-136">Aktivieren von automatisierten Komponententests</span><span class="sxs-lookup"><span data-stu-id="b1f15-136">How to enable automated unit testing</span></span>](enable-automated-unit-testing.md)

<span data-ttu-id="b1f15-137">Sie können eine eigene Kopie von nerddinner von Grund auf erstellen, indem Sie jeden Schritt abschließen, den wir in diesem Kapitel Exemplarische Vorgehensweise ausführen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="b1f15-138">Alternativ können Sie eine vollständige Version des Quellcodes hier herunterladen: [nerddinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="b1f15-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="b1f15-139">Sie können auch optional [eine kostenlose PDF-Version dieses Tutorials herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) , wenn Sie das Tutorial offline lesen möchten.</span><span class="sxs-lookup"><span data-stu-id="b1f15-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="b1f15-140">Sie können entweder Visual Studio 2008 oder den kostenlosen Visual Web Developer 2008 Express verwenden, um die Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="b1f15-141">Sie können entweder SQL Server oder den kostenlosen SQL Server Express für die Datenbank verwenden.</span><span class="sxs-lookup"><span data-stu-id="b1f15-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="b1f15-142">Sie können ASP.NET MVC, Visual Web Developer 2008 Express und SQL Server Express (alle kostenlos) mithilfe von V2 des [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) installieren.</span><span class="sxs-lookup"><span data-stu-id="b1f15-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="b1f15-143">Fangen wir nun an...</span><span class="sxs-lookup"><span data-stu-id="b1f15-143">Now let's get started....</span></span>

<span data-ttu-id="b1f15-144">Nun, da wir uns mit der Bedeutung von "nerddinner" abkennen, können wir uns ein Rollup für die Ärmel durchführen und Code</span><span class="sxs-lookup"><span data-stu-id="b1f15-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="b1f15-145">Wir beginnen mit der Verwendung von Datei&gt;neues Projekt in Visual Studio, um die "nerddinner"-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1f15-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b1f15-146">Weiter</span><span class="sxs-lookup"><span data-stu-id="b1f15-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
