---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Getting Started with ASP.NET 4,7 Web Forms und Visual Studio 2017 | Microsoft-Dokumentation
author: Erikre
description: Diese Schritt-für-Schritt-tutorialreihe vermittelt Ihnen die Grundlagen zum Entwickeln einer ASP.net-Web Forms Anwendung mit ASP.NET 4,7 und Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458343"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="5b053-103">Getting Started with ASP.NET 4,5 Web Forms und Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="5b053-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>

<span data-ttu-id="5b053-104">[Herunterladen eines Wingtip Toys-C#Beispielprojekts ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [Herunterladen von E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b053-104">[Download Wingtip Toys Sample Project (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="5b053-105">In dieser tutorialreihe wird gezeigt, wie Sie eine ASP.net Web Forms-Anwendung mit ASP.NET 4,5 und Microsoft Visual Studio 2017 erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b053-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="5b053-106">Einführung</span><span class="sxs-lookup"><span data-stu-id="5b053-106">Introduction</span></span>

<span data-ttu-id="5b053-107">Diese tutorialreihe führt Sie durch die Erstellung einer ASP.net Web Forms-Anwendung mit Visual Studio 2017 und ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="5b053-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="5b053-108">Sie erstellen eine Anwendung mit dem Namen **Wingtip Toys** -eine vereinfachte Store Front-Website, die Elemente online verkauft.</span><span class="sxs-lookup"><span data-stu-id="5b053-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="5b053-109">Während der Reihe werden die neuen ASP.NET 4,5-Features hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="5b053-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="5b053-110">Zielgruppe</span><span class="sxs-lookup"><span data-stu-id="5b053-110">Target audience</span></span>

<span data-ttu-id="5b053-111">Entwickler, die mit ASP.net Web Forms vertraut sind, sind die Zielgruppe für diese tutorialreihe.</span><span class="sxs-lookup"><span data-stu-id="5b053-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="5b053-112">Sie sollten über einige Kenntnisse in den folgenden Bereichen verfügen:</span><span class="sxs-lookup"><span data-stu-id="5b053-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="5b053-113">Objektorientierte Programmierung (OOP) und Sprachen</span><span class="sxs-lookup"><span data-stu-id="5b053-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="5b053-114">Webentwicklung (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="5b053-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="5b053-115">Relationale Datenbanken</span><span class="sxs-lookup"><span data-stu-id="5b053-115">Relational databases</span></span>
- <span data-ttu-id="5b053-116">N-schichtige Architektur</span><span class="sxs-lookup"><span data-stu-id="5b053-116">N-tier architecture</span></span>

<span data-ttu-id="5b053-117">Um diese Bereiche zu überprüfen, sollten Sie den folgenden Inhalt untersuchen:</span><span class="sxs-lookup"><span data-stu-id="5b053-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="5b053-118">Erste Schritte mit Visual C#</span><span class="sxs-lookup"><span data-stu-id="5b053-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="5b053-119">[Webentwicklung](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, jQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="5b053-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="5b053-120">Relationale Datenbank</span><span class="sxs-lookup"><span data-stu-id="5b053-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="5b053-121">Architektur mit mehreren Ebenen</span><span class="sxs-lookup"><span data-stu-id="5b053-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="5b053-122">Anwendungs Features</span><span class="sxs-lookup"><span data-stu-id="5b053-122">Application features</span></span>

<span data-ttu-id="5b053-123">Zu den in dieser Reihe dargestellten ASP.net Web Form-Features gehören:</span><span class="sxs-lookup"><span data-stu-id="5b053-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="5b053-124">Das Webanwendungs Projekt (nicht das Website Projekt)</span><span class="sxs-lookup"><span data-stu-id="5b053-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="5b053-125">Web Forms</span><span class="sxs-lookup"><span data-stu-id="5b053-125">Web Forms</span></span>
- <span data-ttu-id="5b053-126">Master Seiten, Konfiguration</span><span class="sxs-lookup"><span data-stu-id="5b053-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="5b053-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="5b053-127">Bootstrap</span></span>
- <span data-ttu-id="5b053-128">Entity Framework Code First, localdb</span><span class="sxs-lookup"><span data-stu-id="5b053-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="5b053-129">Anforderungs Validierung</span><span class="sxs-lookup"><span data-stu-id="5b053-129">Request Validation</span></span>
- <span data-ttu-id="5b053-130">Stark typisierte Daten Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="5b053-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="5b053-131">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="5b053-131">Model Binding</span></span>
- <span data-ttu-id="5b053-132">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="5b053-132">Data Annotations</span></span>
- <span data-ttu-id="5b053-133">Wert Anbieter</span><span class="sxs-lookup"><span data-stu-id="5b053-133">Value Providers</span></span>
- <span data-ttu-id="5b053-134">SSL und OAuth</span><span class="sxs-lookup"><span data-stu-id="5b053-134">SSL and OAuth</span></span>
- <span data-ttu-id="5b053-135">ASP.net Identity, Konfiguration und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="5b053-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="5b053-136">Unaufdringliche Validierung</span><span class="sxs-lookup"><span data-stu-id="5b053-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="5b053-137">Routing</span><span class="sxs-lookup"><span data-stu-id="5b053-137">Routing</span></span>
- <span data-ttu-id="5b053-138">ASP.NET – Fehlerbehandlung</span><span class="sxs-lookup"><span data-stu-id="5b053-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="5b053-139">Anwendungsszenarien und-Aufgaben</span><span class="sxs-lookup"><span data-stu-id="5b053-139">Application scenarios and tasks</span></span>

<span data-ttu-id="5b053-140">Zu den Aufgaben der Lernprogramm Reihe gehören</span><span class="sxs-lookup"><span data-stu-id="5b053-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="5b053-141">Erstellen, überprüfen und Ausführen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="5b053-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="5b053-142">Erstellen einer Datenbankstruktur</span><span class="sxs-lookup"><span data-stu-id="5b053-142">Creating a database structure</span></span>
- <span data-ttu-id="5b053-143">Initialisieren und Seeding einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="5b053-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="5b053-144">Anpassen der Benutzeroberfläche mit Stilen, Grafiken und einer Master Seite</span><span class="sxs-lookup"><span data-stu-id="5b053-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="5b053-145">Hinzufügen von Seiten und Navigation</span><span class="sxs-lookup"><span data-stu-id="5b053-145">Adding pages and navigation</span></span>
- <span data-ttu-id="5b053-146">Anzeigen von Menü Details und Produktdaten</span><span class="sxs-lookup"><span data-stu-id="5b053-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="5b053-147">Erstellen eines Einkaufswagen</span><span class="sxs-lookup"><span data-stu-id="5b053-147">Creating a shopping cart</span></span>
- <span data-ttu-id="5b053-148">Hinzufügen von SSL-und OAuth-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="5b053-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="5b053-149">Hinzufügen einer Zahlungsmethode</span><span class="sxs-lookup"><span data-stu-id="5b053-149">Adding a payment method</span></span>
- <span data-ttu-id="5b053-150">Einschließen einer Administrator Rolle und eines Benutzers für die Anwendung</span><span class="sxs-lookup"><span data-stu-id="5b053-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="5b053-151">Einschränken des Zugriffs auf bestimmte Seiten und Ordner</span><span class="sxs-lookup"><span data-stu-id="5b053-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="5b053-152">Hochladen einer Datei in die Webanwendung</span><span class="sxs-lookup"><span data-stu-id="5b053-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="5b053-153">Implementieren der Eingabevalidierung</span><span class="sxs-lookup"><span data-stu-id="5b053-153">Implementing input validation</span></span>
- <span data-ttu-id="5b053-154">Routen für die Webanwendung werden registriert</span><span class="sxs-lookup"><span data-stu-id="5b053-154">Registering routes for the web application</span></span>
- <span data-ttu-id="5b053-155">Implementieren der Fehlerbehandlung und der Fehler Protokollierung</span><span class="sxs-lookup"><span data-stu-id="5b053-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="5b053-156">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5b053-156">Overview</span></span>

<span data-ttu-id="5b053-157">Diese tutorialreihe richtet sich an Benutzer, die mit Programmier Konzepten vertraut sind, aber noch nicht in ASP.net Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5b053-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="5b053-158">Wenn Sie bereits mit ASP.net Web Forms vertraut sind, können Sie mit dieser Reihe noch mehr über neue ASP.NET 4,5-Features erfahren.</span><span class="sxs-lookup"><span data-stu-id="5b053-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="5b053-159">Leser, die mit Programmier Konzepten und ASP.net-Web Forms nicht vertraut sind, finden Sie in den zusätzlichen Web Forms Tutorials im Abschnitt " [Getting Started](../../../index.md) " auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="5b053-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="5b053-160">Die in dieser tutorialreihe bereitgestellte ASP.NET 4,5 umfasst die folgenden Features:</span><span class="sxs-lookup"><span data-stu-id="5b053-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="5b053-161">Eine einfache Benutzeroberfläche zum Erstellen von Projekten, die [Unterstützung für viele ASP.NET-Frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC und die Web-API) bietet.</span><span class="sxs-lookup"><span data-stu-id="5b053-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="5b053-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), ein Layout, Design, Design und reaktionsfähiges Design Framework.</span><span class="sxs-lookup"><span data-stu-id="5b053-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="5b053-163">[ASP.net Identity](../../../../identity/index.md)ein neues ASP.NET-Mitgliedschaftssystem, das in allen ASP.NET-Frameworks identisch funktioniert und mit anderen Webhosting-Software als IIS funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5b053-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="5b053-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5b053-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="5b053-165">Ein Update der-Entity Framework, die Folgendes ermöglicht:</span><span class="sxs-lookup"><span data-stu-id="5b053-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="5b053-166">Abrufen und Bearbeiten von Daten als stark typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="5b053-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="5b053-167">Asynchrones Zugreifen auf Daten</span><span class="sxs-lookup"><span data-stu-id="5b053-167">Access data asynchronously</span></span>
  - <span data-ttu-id="5b053-168">Behandeln vorübergehender Verbindungsfehler</span><span class="sxs-lookup"><span data-stu-id="5b053-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="5b053-169">SQL-Anweisungen protokollieren</span><span class="sxs-lookup"><span data-stu-id="5b053-169">Log SQL statements</span></span>

<span data-ttu-id="5b053-170">Eine komplette Liste der ASP.NET 4,5-Features finden Sie unter [ASP.net and Web Tools für Visual Studio 2013 Anmerkungen](../../../../visual-studio/overview/2013/release-notes.md)zu dieser Version.</span><span class="sxs-lookup"><span data-stu-id="5b053-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="5b053-171">Die Wingtip Toys-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="5b053-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="5b053-172">Die folgenden Screenshots stammen aus der ASP.net Web Forms-Anwendung, die Sie in dieser tutorialreihe erstellen.</span><span class="sxs-lookup"><span data-stu-id="5b053-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="5b053-173">Wenn Sie die Anwendung in Visual Studio ausführen, wird die folgende Web-Startseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b053-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys-Standardseite](introduction-and-overview/_static/image1.png)

<span data-ttu-id="5b053-175">Sie können sich als neuer Benutzer registrieren oder sich als vorhandener Benutzer anmelden.</span><span class="sxs-lookup"><span data-stu-id="5b053-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="5b053-176">Der obere Navigationsbereich enthält Links zu Produktkategorien und deren Produkten aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5b053-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="5b053-177">Wenn Sie **Produkte**auswählen, werden alle verfügbaren Produkte angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b053-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys-Produkte](introduction-and-overview/_static/image2.png)

<span data-ttu-id="5b053-179">Wenn Sie ein bestimmtes Produkt auswählen, werden Produktdetails angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5b053-179">If you select a specific product, product details are displayed.</span></span>

![Wingtip Toys-Produkt Details](introduction-and-overview/_static/image3.png)

<span data-ttu-id="5b053-181">Als Benutzer können Sie sich mit Web Forms Vorlagen Standardfunktionalität registrieren und anmelden.</span><span class="sxs-lookup"><span data-stu-id="5b053-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="5b053-182">In diesem Tutorial wird auch erläutert, wie Sie sich mit einem vorhandenen Gmail-Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="5b053-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="5b053-183">Außerdem können Sie sich als Administrator anmelden, um Produkte aus der Datenbank hinzuzufügen und daraus zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="5b053-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys-anmelden](introduction-and-overview/_static/image4.png)

<span data-ttu-id="5b053-185">Wenn Sie sich als Benutzer angemeldet haben, können Sie Produkte zum Warenkorb hinzufügen und mit PayPal Auschecken.</span><span class="sxs-lookup"><span data-stu-id="5b053-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="5b053-186">Die Beispielanwendung ist so konzipiert, dass Sie in der Developer Sandbox von PayPal funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5b053-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="5b053-187">Es findet keine tatsächliche Money-Transaktion statt.</span><span class="sxs-lookup"><span data-stu-id="5b053-187">No actual money transaction takes place.</span></span>

![Wingtip Toys: Einkaufswagen](introduction-and-overview/_static/image5.png)

<span data-ttu-id="5b053-189">PayPal bestätigt Ihre Konto-, Bestell-und Zahlungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="5b053-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="5b053-191">Nach der Rückgabe von PayPal können Sie Ihre Bestellung überprüfen und vervollständigen.</span><span class="sxs-lookup"><span data-stu-id="5b053-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys-Bestell Überprüfung](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="5b053-193">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5b053-193">Prerequisites</span></span>

<span data-ttu-id="5b053-194">Vergewissern Sie sich, dass die folgende Software auf Ihrem Computer installiert ist, bevor Sie beginnen:</span><span class="sxs-lookup"><span data-stu-id="5b053-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="5b053-195">[Microsoft Visual Studio 2017 oder Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="5b053-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="5b053-196">Der .NET Framework wird automatisch installiert.</span><span class="sxs-lookup"><span data-stu-id="5b053-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="5b053-197">Diese tutorialreihe verwendet Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="5b053-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="5b053-198">Diese tutorialreihe können Sie entweder mithilfe von oder Microsoft Visual Studio 2017 durchführen.</span><span class="sxs-lookup"><span data-stu-id="5b053-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="5b053-199">Beachten Sie die folgenden Informationen zu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5b053-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="5b053-200">Microsoft Visual Studio 2017 und Microsoft Visual Studio Community 2017 werden in dieser tutorialreihe als *Visual Studio* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="5b053-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="5b053-201">Visual Studio 2017 wird neben allen älteren Versionen installiert, die bereits installiert sind.</span><span class="sxs-lookup"><span data-stu-id="5b053-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="5b053-202">Websites, die in früheren Versionen erstellt wurden, können in Visual Studio 2017 geöffnet und weiterhin in früheren Versionen geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="5b053-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="5b053-203">Wenn Sie Visual Studio zum ersten Mal gestartet haben, wird davon ausgegangen, dass Sie die *Webentwicklungs* Einstellungen ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="5b053-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="5b053-204">Weitere Informationen finden Sie unter Gewusst [wie: Auswählen von Einstellungen für die Webentwicklungs Umgebung](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b053-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="5b053-205">Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie mit dem Erstellen des in dieser tutorialreihe dargestellten Webprojekts beginnen.</span><span class="sxs-lookup"><span data-stu-id="5b053-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="5b053-206">Herunterladen der Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="5b053-206">Download the sample application</span></span>

 <span data-ttu-id="5b053-207">Sie können die abgeschlossene Beispielanwendung jederzeit von der MSDN Samples-Website herunterladen:</span><span class="sxs-lookup"><span data-stu-id="5b053-207">You can download the completed sample application at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="5b053-208">[Getting Started with ASP.NET 4,5 Web Forms and Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="5b053-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="5b053-209">Dieser Download umfasst die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="5b053-209">This download has the following items:</span></span>

- <span data-ttu-id="5b053-210">Die Beispielanwendung im Ordner *wingtiptoys* .</span><span class="sxs-lookup"><span data-stu-id="5b053-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="5b053-211">Die Ressourcen, die zum Erstellen der Beispielanwendung im Ordner *wingtiptoys-Assets* im Ordner *wingtiptoys* verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5b053-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="5b053-212">Der Download ist eine *ZIP* -Datei.</span><span class="sxs-lookup"><span data-stu-id="5b053-212">The download is a *.zip* file.</span></span> <span data-ttu-id="5b053-213">Um das abgeschlossene Projekt anzuzeigen, das in dieser tutorialreihe erstellt wird *C#* , suchen Sie den Ordner in der ZIP-Datei, und wählen Sie ihn</span><span class="sxs-lookup"><span data-stu-id="5b053-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="5b053-214">Speichern Sie C# den Ordner in dem Ordner, den Sie für die Arbeit mit Visual Studio-Projekten verwenden.</span><span class="sxs-lookup"><span data-stu-id="5b053-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="5b053-215">Standardmäßig lautet der Ordner Visual Studio 2017-Projekte wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5b053-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="5b053-216"><strong>C:\Benutzer&#92;</strong>  <strong><em>&lt;Benutzername&gt;</em></strong> <strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="5b053-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="5b053-217">Benennen Sie ***C#*** den Ordner in ***wingtiptoys***um.</span><span class="sxs-lookup"><span data-stu-id="5b053-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="5b053-218">Wenn Sie bereits über einen Ordner mit dem Namen *wingtiptoys* im Ordner "Projekte" verfügen, benennen Sie den vorhandenen *C#* Ordner vorübergehend um, bevor Sie den Ordner in *wingtiptoys*umbenennen.</span><span class="sxs-lookup"><span data-stu-id="5b053-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="5b053-219">Um das abgeschlossene Projekt auszuführen, öffnen Sie den Ordner *wingtiptoys* , und doppelklicken Sie auf die Datei *wingtiptoys. sln* .</span><span class="sxs-lookup"><span data-stu-id="5b053-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="5b053-220">Visual Studio 2017 öffnet das Projekt.</span><span class="sxs-lookup"><span data-stu-id="5b053-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="5b053-221">Klicken Sie anschließend in **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei *default. aspx* , und wählen Sie **in Browser anzeigen aus**.</span><span class="sxs-lookup"><span data-stu-id="5b053-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="5b053-222">ASP.net-Web Forms Quiz zum Überprüfen von Inhalten</span><span class="sxs-lookup"><span data-stu-id="5b053-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="5b053-223">Nachdem Sie die tutorialreihe abgeschlossen haben, erstellen Sie ein Quiz, um Ihr Wissen zu testen und wichtige Konzepte zu verstärken</span><span class="sxs-lookup"><span data-stu-id="5b053-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="5b053-224">Jede Frage enthält eine Erläuterung und Links zu weiteren Anleitungen.</span><span class="sxs-lookup"><span data-stu-id="5b053-224">Each question provides an explanation and links to additional guidance.</span></span>

* [<span data-ttu-id="5b053-225">ASP.net Web Forms Quiz</span><span class="sxs-lookup"><span data-stu-id="5b053-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="5b053-226">Lernprogramm Unterstützung und Kommentare</span><span class="sxs-lookup"><span data-stu-id="5b053-226">Tutorial support and comments</span></span>

<span data-ttu-id="5b053-227">Für Fragen und Kommentare verwenden Sie den Q-und einen-Abschnitt, der auf der Beispielseite mit den ersten Schritten [mit ASP.NET 4,5 Web Forms und Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="5b053-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="5b053-228">Kommentare zu dieser tutorialreihe sind willkommen.</span><span class="sxs-lookup"><span data-stu-id="5b053-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="5b053-229">Wenn diese tutorialreihe aktualisiert wird, wird jeder Versuch unternommen, Korrekturen oder Verbesserungsvorschläge in Erwägung zu gezogen.</span><span class="sxs-lookup"><span data-stu-id="5b053-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="5b053-230">Wenn ein Fehler auftritt, können die entsprechenden Fehlermeldungen verwirrend sein, und es gibt keine gute Erklärung, wie Sie Sie beheben können.</span><span class="sxs-lookup"><span data-stu-id="5b053-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="5b053-231">Hilfe finden Sie in den ASP.net- [Foren](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="5b053-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="5b053-232">Eine weitere gute Quelle ist der Q-und A-Abschnitt der Beispielseite " [Getting Started with ASP.NET 4,5 Web Forms and Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)".</span><span class="sxs-lookup"><span data-stu-id="5b053-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="5b053-233">Weiter</span><span class="sxs-lookup"><span data-stu-id="5b053-233">Next</span></span>](create-the-project.md)
