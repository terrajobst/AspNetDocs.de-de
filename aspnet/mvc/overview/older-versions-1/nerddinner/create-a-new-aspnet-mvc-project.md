---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Erstellen eines neuen ASP.NET MVC-Projekts | Microsoft-Dokumentation
author: microsoft
description: In Schritt 1 wird veranschaulicht, wie die grundlegende nerddinner-Anwendungs Struktur eingerichtet wird.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469239"
---
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="d6e9c-103">Erstellen eines neuen ASP.NET MVC-Projekts</span><span class="sxs-lookup"><span data-stu-id="d6e9c-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="d6e9c-104">von [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d6e9c-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d6e9c-105">PDF herunterladen</span><span class="sxs-lookup"><span data-stu-id="d6e9c-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d6e9c-106">Dies ist Schritt 1 des kostenlosen ["nerddinner"](introducing-the-nerddinner-tutorial.md) -Lernprogramms, in dem erläutert wird, wie eine kleine, aber komplette Webanwendung mit ASP.NET MVC 1 erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d6e9c-107">In Schritt 1 wird veranschaulicht, wie die grundlegende nerddinner-Anwendungs Struktur eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="d6e9c-108">Wenn Sie ASP.NET MVC 3 verwenden, empfiehlt es sich, die Tutorials " [Getting Started with MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) " oder " [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) " zu befolgen.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="d6e9c-109">Nerddinner Step 1: Datei&gt;neues Projekt</span><span class="sxs-lookup"><span data-stu-id="d6e9c-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="d6e9c-110">Wir beginnen mit unserer "nerddinner"-Anwendung, indem wir das Menü Element " **File-&gt;New Project** " in Visual Studio 2008 oder im kostenlosen Visual Web Developer 2008 Express auswählen.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="d6e9c-111">Dadurch wird das Dialogfeld "Neues Projekt" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="d6e9c-112">Um eine neue ASP.NET MVC-Anwendung zu erstellen, wählen wir auf der linken Seite des Dialog Felds den Knoten "Web" und dann auf der rechten Seite die Projektvorlage "ASP.NET MVC-Webanwendung" aus:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="d6e9c-113">*Wichtig: Stellen Sie sicher, dass Sie ASP.NET MVC heruntergeladen und installiert haben. andernfalls wird es nicht im Dialogfeld "Neues Projekt" angezeigt. Sie können v2 des [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) verwenden, wenn Sie es noch nicht installiert haben (ASP.NET MVC ist im Abschnitt "Webplattform-&gt;Frameworks und Laufzeiten" verfügbar).*</span><span class="sxs-lookup"><span data-stu-id="d6e9c-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="d6e9c-114">Wir nennen das neue Projekt, das wir erstellen, und klicken dann auf die Schaltfläche "OK", um es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="d6e9c-115">Wenn wir auf "OK" klicken, öffnet Visual Studio ein zusätzliches Dialogfeld, in dem Sie aufgefordert werden, optional auch ein Komponenten Testprojekt für die neue Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="d6e9c-116">Mit diesem Komponenten Testprojekt können wir automatisierte Tests erstellen, mit denen die Funktionalität und das Verhalten der Anwendung überprüft werden (was wir später in diesem Tutorial behandeln werden).</span><span class="sxs-lookup"><span data-stu-id="d6e9c-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="d6e9c-117">Die Dropdown Liste "Test Framework" im obigen Dialogfeld enthält alle verfügbaren ASP.NET MVC-Komponenten Test-Projektvorlagen, die auf dem Computer installiert sind.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="d6e9c-118">Versionen können für nunit, MbUnit und xUnit heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="d6e9c-119">Das integrierte Visual Studio-Komponenten Test-Framework wird ebenfalls unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="d6e9c-120">*Hinweis: das Visual Studio-Komponenten Test-Framework ist nur in Visual Studio 2008 Professional und höheren Versionen verfügbar. Wenn Sie vs 2008 Standard Edition oder Visual Web Developer 2008 Express verwenden, müssen Sie die nunit-, MbUnit-oder xUnit-Erweiterungen für ASP.NET MVC herunterladen und installieren, damit dieses Dialogfeld angezeigt wird. Wenn keine Test-Frameworks installiert sind, wird das Dialogfeld nicht angezeigt.*</span><span class="sxs-lookup"><span data-stu-id="d6e9c-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="d6e9c-121">Wir verwenden den Standardnamen "nerddinner. Tests" für das Testprojekt, das wir erstellen, und verwenden die Framework-Option "Visual Studio-Komponenten Test".</span><span class="sxs-lookup"><span data-stu-id="d6e9c-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="d6e9c-122">Wenn wir auf die Schaltfläche "OK" klicken, erstellt Visual Studio eine Projekt Mappe mit zwei Projekten, eine für die Webanwendung und eine für die Komponententests:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="d6e9c-123">Untersuchen der nerddinner Directory-Struktur</span><span class="sxs-lookup"><span data-stu-id="d6e9c-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="d6e9c-124">Wenn Sie eine neue ASP.NET MVC-Anwendung mit Visual Studio erstellen, werden dem Projekt automatisch eine Reihe von Dateien und Verzeichnissen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="d6e9c-125">ASP.NET MVC-Projekte verfügen standardmäßig über sechs Verzeichnisse der obersten Ebene:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="d6e9c-126">**Verzeichnis**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-126">**Directory**</span></span> | <span data-ttu-id="d6e9c-127">**Zweck**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="d6e9c-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-128">**/Controllers**</span></span> | <span data-ttu-id="d6e9c-129">Platzieren von Controller Klassen, die URL-Anforderungen verarbeiten</span><span class="sxs-lookup"><span data-stu-id="d6e9c-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="d6e9c-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-130">**/Models**</span></span> | <span data-ttu-id="d6e9c-131">Wo Sie Klassen platzieren, die Daten darstellen und bearbeiten</span><span class="sxs-lookup"><span data-stu-id="d6e9c-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="d6e9c-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-132">**/Views**</span></span> | <span data-ttu-id="d6e9c-133">Wo Sie Benutzeroberflächen-Vorlagen Dateien platzieren, die für das Rendern der Ausgabe verantwortlich sind</span><span class="sxs-lookup"><span data-stu-id="d6e9c-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="d6e9c-134">**"/Scripts"**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-134">**/Scripts**</span></span> | <span data-ttu-id="d6e9c-135">Wo Sie JavaScript-Bibliotheksdateien und-Skripts (. js) einfügen</span><span class="sxs-lookup"><span data-stu-id="d6e9c-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="d6e9c-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-136">**/Content**</span></span> | <span data-ttu-id="d6e9c-137">Wo Sie CSS-und Bilddateien und andere nicht dynamische/nicht-JavaScript-Inhalte einfügen</span><span class="sxs-lookup"><span data-stu-id="d6e9c-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="d6e9c-138">**/App\_Daten**</span><span class="sxs-lookup"><span data-stu-id="d6e9c-138">**/App\_Data**</span></span> | <span data-ttu-id="d6e9c-139">Wo Sie Datendateien speichern, die Sie lesen/schreiben möchten.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="d6e9c-140">ASP.NET MVC erfordert diese Struktur nicht.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="d6e9c-141">Entwickler, die an großen Anwendungen arbeiten, partitionieren die Anwendung in der Regel über mehrere Projekte hinweg, um Sie besser verwalten zu können (z. b. werden Datenmodell Klassen häufig in einem separaten Klassen Bibliotheksprojekt von der Webanwendung verwendet).</span><span class="sxs-lookup"><span data-stu-id="d6e9c-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="d6e9c-142">Die Standard Projektstruktur bietet allerdings eine gute Standardverzeichnis Konvention, die wir verwenden können, um zu verhindern, dass die Anwendungsprobleme bereinigt werden.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="d6e9c-143">Wenn wir das Verzeichnis "/Controllers" erweitern, stellen wir fest, dass Visual Studio dem Projekt standardmäßig zwei Controller Klassen – HomeController und AccountController – hinzugefügt hat:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="d6e9c-144">Wenn das Verzeichnis/views erweitert wird, werden drei Unterverzeichnisse –/Home,/Account und/Shared – sowie mehrere darin enthaltenen Vorlagen Dateien werden standardmäßig dem Projekt hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="d6e9c-145">Wenn Sie die Verzeichnisse/Content und "/Scripts" erweitern, finden Sie eine Site. CSS-Datei, mit der alle HTML-Dateien auf der Website formatiert werden, sowie JavaScript-Bibliotheken, die die Unterstützung von ASP.NET AJAX und jQuery innerhalb der Anwendung ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="d6e9c-146">Wenn wir das Projekt "nerddinner. Tests" erweitern, finden Sie zwei Klassen, die Komponenten Tests für unsere Controller Klassen enthalten:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="d6e9c-147">Diese Standard Dateien, die von Visual Studio hinzugefügt werden, bieten uns eine grundlegende Struktur für eine funktionierende Anwendung: vervollständigen mit Homepage, Info Seite, Kontoanmeldung/Abmelde-/Registrierungsseiten und eine nicht behandelte Fehlerseite (alle zusammen und sind standardmäßig Funktions bereit).</span><span class="sxs-lookup"><span data-stu-id="d6e9c-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="d6e9c-148">Ausführen der "nerddinner"-Anwendung</span><span class="sxs-lookup"><span data-stu-id="d6e9c-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="d6e9c-149">Sie können das Projekt ausführen, indem Sie entweder die Menü Elemente **Debuggen&gt;Debuggen starten** oder **Debuggen&gt;starten ohne Debugging** auswählen:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="d6e9c-150">Dadurch wird der integrierte ASP.net Web-Server gestartet, der in Visual Studio verfügbar ist, und die Anwendung wird ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="d6e9c-151">Im folgenden finden Sie die Startseite für das neue Projekt (URL: "/"), wenn es ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="d6e9c-152">Wenn Sie auf die Registerkarte "Info" klicken, wird eine Info-Seite angezeigt (URL: "/Home/About"):</span><span class="sxs-lookup"><span data-stu-id="d6e9c-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="d6e9c-153">Wenn Sie oben rechts auf den Link "Anmelden" klicken, gelangen Sie zu einer Anmeldeseite (URL: "/Account/LOGON").</span><span class="sxs-lookup"><span data-stu-id="d6e9c-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="d6e9c-154">Wenn kein Anmelde Konto vorhanden ist, können Sie auf den Link registrieren (URL: "/Account/Register") klicken, um einen Link zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="d6e9c-155">Der Code zum Implementieren der oben genannten Funktionen "Home", "about" und "Logout/Register" wurde beim Erstellen des neuen Projekts standardmäßig hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="d6e9c-156">Wir verwenden diese als Ausgangspunkt unserer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="d6e9c-157">Testen der "nerddinner"-Anwendung</span><span class="sxs-lookup"><span data-stu-id="d6e9c-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="d6e9c-158">Wenn wir die Professional Edition oder eine höhere Version von Visual Studio 2008 verwenden, können wir die integrierte IDE-Unterstützung für Komponententests in Visual Studio verwenden, um das Projekt zu testen:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="d6e9c-159">Wenn Sie eine der oben genannten Optionen auswählen, wird der Bereich "Testergebnisse" in der IDE geöffnet, und wir erhalten den Status "erfolgreich" und "Fehler" für die 27 Komponententests, die in unserem neuen Projekt enthalten sind und die integrierten Funktionen abdecken:</span><span class="sxs-lookup"><span data-stu-id="d6e9c-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="d6e9c-160">Später in diesem Tutorial erfahren Sie mehr über automatisierte Tests und zusätzliche Komponententests, die die von uns implementierten Anwendungs Funktionalität abdecken.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="d6e9c-161">Nächster Schritt</span><span class="sxs-lookup"><span data-stu-id="d6e9c-161">Next Step</span></span>

<span data-ttu-id="d6e9c-162">Wir haben jetzt eine grundlegende Anwendungs Struktur eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="d6e9c-163">Nun erstellen wir [eine Datenbank, in der die Anwendungsdaten gespeichert](create-a-database.md)werden.</span><span class="sxs-lookup"><span data-stu-id="d6e9c-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6e9c-164">[Zurück](introducing-the-nerddinner-tutorial.md)
> [Weiter](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="d6e9c-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
