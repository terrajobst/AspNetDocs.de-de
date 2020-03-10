---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Praktische Übungseinheit: Visual Studio 2013 WebTools | Microsoft-Dokumentation'
author: rick-anderson
description: Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierte Windows-und Webprojekte. Sie enthält einen leistungsfähigen Text-Editor, der problemlos verwendet werden kann...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505005"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="93e98-104">Praktische Übungseinheit: Visual Studio 2013-WebTools</span><span class="sxs-lookup"><span data-stu-id="93e98-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="93e98-105">vom [Web Camps-Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="93e98-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="93e98-106">Webcamps-Trainingskit herunterladen</span><span class="sxs-lookup"><span data-stu-id="93e98-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="93e98-107">Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierte Windows-und Webprojekte.</span><span class="sxs-lookup"><span data-stu-id="93e98-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="93e98-108">Sie enthält einen leistungsfähigen Text-Editor, der problemlos verwendet werden kann, um eigenständige Dateien ohne ein Projekt zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="93e98-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="93e98-109">Visual Studio behält eine Analyse Struktur mit vollem Funktionsumfang bei, während Sie die einzelnen Dateien bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="93e98-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="93e98-110">Dies ermöglicht Visual Studio, eine unschöne automatische Vervollständigung und Dokument basierte Aktionen bereitzustellen und gleichzeitig die Entwicklung schneller und angenehmer zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="93e98-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="93e98-111">Diese Features sind besonders leistungsfähig in HTML-und CSS-Dokumenten.</span><span class="sxs-lookup"><span data-stu-id="93e98-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="93e98-112">Diese Leistungsfähigkeit steht auch für Erweiterungen zur Verfügung, sodass die Editoren einfach mit leistungsstarken neuen Features erweitert werden können, die Ihren Anforderungen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="93e98-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="93e98-113">Web Essentials ist eine Sammlung von (größtenteils) webbezogenen Erweiterungen für Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93e98-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="93e98-114">Es enthält viele neue IntelliSense-Vervollständigungen (insbesondere für CSS), neue browserlinkfeatures, automatisches jshint für JavaScript-Dateien, neue Warnungen für HTML und CSS sowie viele weitere Features, die für die moderne Webentwicklung von entscheidender Bedeutung sind.</span><span class="sxs-lookup"><span data-stu-id="93e98-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="93e98-115">Der gesamte Beispielcode und die Code Ausschnitte sind im Web Camps-Trainingskit enthalten, das unter [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="93e98-116">Übersicht</span><span class="sxs-lookup"><span data-stu-id="93e98-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="93e98-117">Ziele</span><span class="sxs-lookup"><span data-stu-id="93e98-117">Objectives</span></span>

<span data-ttu-id="93e98-118">In dieser praktischen Übungseinheit erfahren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="93e98-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="93e98-119">Verwenden neuer HTML-Editor-Funktionen, die in Web Essentials enthalten sind, z. b. umfangreiche HTML5-Code Ausschnitte</span><span class="sxs-lookup"><span data-stu-id="93e98-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="93e98-120">Verwenden neuer CSS-Editor-Features in WebEssentials, z. b. Farbauswahl und Browser Matrix-QuickInfo</span><span class="sxs-lookup"><span data-stu-id="93e98-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="93e98-121">Verwenden neuer JavaScript-Editor-Funktionen in WebEssentials, z. b. Extrahieren in Dateien und IntelliSense für alle HTML-Elemente</span><span class="sxs-lookup"><span data-stu-id="93e98-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="93e98-122">Austauschen von Daten zwischen Ihrem Browser und Visual Studio mithilfe von Browser Link</span><span class="sxs-lookup"><span data-stu-id="93e98-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="93e98-123">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="93e98-123">Prerequisites</span></span>

<span data-ttu-id="93e98-124">Zum Durchführen dieser praktischen Übungseinheit ist Folgendes erforderlich:</span><span class="sxs-lookup"><span data-stu-id="93e98-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="93e98-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) oder höher</span><span class="sxs-lookup"><span data-stu-id="93e98-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="93e98-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="93e98-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="93e98-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="93e98-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="93e98-128">Einrichten</span><span class="sxs-lookup"><span data-stu-id="93e98-128">Setup</span></span>

<span data-ttu-id="93e98-129">Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zuerst Ihre Umgebung einrichten.</span><span class="sxs-lookup"><span data-stu-id="93e98-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="93e98-130">Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zum **Quell** Ordner des Labs.</span><span class="sxs-lookup"><span data-stu-id="93e98-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="93e98-131">Klicken Sie mit der rechten Maustaste auf **Setup. cmd** , und wählen Sie **als Administrator ausführen** aus, um den Setup Vorgang zu starten, mit dem die Umgebung konfiguriert wird, und die Visual Studio-Code Ausschnitte für dieses Lab</span><span class="sxs-lookup"><span data-stu-id="93e98-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="93e98-132">Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion, um fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="93e98-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="93e98-133">Vergewissern Sie sich, dass Sie vor dem Ausführen des Setups alle Abhängigkeiten für dieses Lab geprüft haben.</span><span class="sxs-lookup"><span data-stu-id="93e98-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="93e98-134">Verwenden der Code Ausschnitte</span><span class="sxs-lookup"><span data-stu-id="93e98-134">Using the Code Snippets</span></span>

<span data-ttu-id="93e98-135">Im gesamten Lab-Dokument werden Sie angewiesen, Code Blöcke einzufügen.</span><span class="sxs-lookup"><span data-stu-id="93e98-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="93e98-136">Der größte Teil dieses Codes wird zur einfacheren Verwendung als Visual Studio Code Ausschnitte bereitgestellt, auf die Sie in Visual Studio 2013 zugreifen können, um das manuelle Hinzufügen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="93e98-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="93e98-137">Jede Übung wird von einer Start Lösung begleitet, die sich im Ordner " **Begin** " der Übung befindet und Ihnen ermöglicht, die einzelnen Übungen unabhängig von den anderen zu befolgen.</span><span class="sxs-lookup"><span data-stu-id="93e98-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="93e98-138">Beachten Sie, dass die während einer Übung hinzugefügten Code Ausschnitte in diesen Projektmappen fehlen und möglicherweise erst nach Abschluss der Übung funktionieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="93e98-139">Innerhalb des Quellcodes für eine Übung finden Sie auch einen **endordner** mit einer Visual Studio-Projekt Mappe mit dem Code, der aus der Ausführung der Schritte in der entsprechenden Übung resultiert.</span><span class="sxs-lookup"><span data-stu-id="93e98-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="93e98-140">Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, während Sie diese praktische Übungseinheit durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="93e98-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="93e98-141">Exerzitien</span><span class="sxs-lookup"><span data-stu-id="93e98-141">Exercises</span></span>

<span data-ttu-id="93e98-142">Diese praktische Übungseinheit umfasst die folgenden Übungen:</span><span class="sxs-lookup"><span data-stu-id="93e98-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="93e98-143">Arbeiten mit Browser Link und Web Essentials</span><span class="sxs-lookup"><span data-stu-id="93e98-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="93e98-144">Nutzen von Code Ausschnitten und IntelliSense</span><span class="sxs-lookup"><span data-stu-id="93e98-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="93e98-145">Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungs Sammlungen auswählen.</span><span class="sxs-lookup"><span data-stu-id="93e98-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="93e98-146">Jede vordefinierte Sammlung ist so konzipiert, dass Sie einem bestimmten Entwicklungsstil entspricht, und bestimmt Fensterlayouts, das Editor-Verhalten, IntelliSense-Code Ausschnitte und Dialogfeld Optionen.</span><span class="sxs-lookup"><span data-stu-id="93e98-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="93e98-147">In den Prozeduren in dieser Übungseinheit werden die Aktionen beschrieben, die zum Ausführen einer bestimmten Aufgabe in Visual Studio bei Verwendung der **allgemeinen Entwicklungs Einstellungs** Auflistung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="93e98-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="93e98-148">Wenn Sie eine andere Einstellungs Sammlung für Ihre Entwicklungsumgebung auswählen, kann es Unterschiede in den Schritten geben, die Sie berücksichtigen sollten.</span><span class="sxs-lookup"><span data-stu-id="93e98-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="93e98-149">Übung 1: Arbeiten mit Browser Link und Web Essentials</span><span class="sxs-lookup"><span data-stu-id="93e98-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="93e98-150">**WebEssentials** ist eine Visual Studio-Erweiterung, die eine Vielzahl nützlicher Features für die moderne Webentwicklung bietet. Sie konzentriert sich hauptsächlich darauf, die Webentwicklung viel schneller und angenehmer zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="93e98-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="93e98-151">Sie können Web Essentials aus dem Erweiterungs Katalog in Visual Studio installieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="93e98-152">**Browser Link** ist ein neues Feature, das in Visual Studio 2013 enthalten ist, das einen Kanal zwischen der Visual Studio-IDE und einem beliebigen geöffneten Browser zum Austauschen von Daten zwischen Ihrer Webanwendung und Visual Studio bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="93e98-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="93e98-153">Web Essentials erweitert den Browser Link mit Tools, um das DOM-Objektmodell und die CSS-Stile Ihrer Webseiten direkt aus dem Browser zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="93e98-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="93e98-154">In dieser Übung untersuchen Sie einige der Features, die von **Web Essentials** und **Browser Link** unterstützt werden, um eine einfache Quiz Seite zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="93e98-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="93e98-155">Aufgabe 1: Ausführen des Projekts in mehreren Browsern</span><span class="sxs-lookup"><span data-stu-id="93e98-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="93e98-156">In dieser Aufgabe konfigurieren Sie die Webanwendung so, dass Sie in mehreren Browsern gleichzeitig ausgeführt wird, was für Browser übergreifende Tests nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="93e98-157">Öffnen Sie **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="93e98-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="93e98-158">Wählen Sie im Menü **Datei** die Option Öffnen aus.  **Projekt/Projekt Mappe...** und navigieren Sie zu **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** im **Quell** Ordner des Labs (c:\webcampstk\hol\vswebtooling\source).</span><span class="sxs-lookup"><span data-stu-id="93e98-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="93e98-159">Wählen Sie **BEGIN. sln** aus, und klicken Sie auf **Öffnen**.</span><span class="sxs-lookup"><span data-stu-id="93e98-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="93e98-160">Erweitern Sie in der Visual Studio-Symbolleiste das Menü Browser, und wählen Sie **Durchsuchen mit...** aus.</span><span class="sxs-lookup"><span data-stu-id="93e98-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="93e98-161">![Menüoption "Durchsuchen"](visual-studio-2013-web-tools/_static/image1.png "Durchsuchen... im Menü Browser")</span><span class="sxs-lookup"><span data-stu-id="93e98-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="93e98-162">*Menüoption "Durchsuchen"*</span><span class="sxs-lookup"><span data-stu-id="93e98-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="93e98-163">Wählen Sie im Dialogfeld **Durchsuchen mit** den Optionen **Google Chrome** und **Internet Explorer** aus, indem Sie die **STRG** -Taste gedrückt halten, und klicken Sie dann auf **als Standard festlegen**.</span><span class="sxs-lookup"><span data-stu-id="93e98-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="93e98-164">![Dialogfeld "Durchsuchen"](visual-studio-2013-web-tools/_static/image2.png "Dialogfeld "Durchsuchen"")</span><span class="sxs-lookup"><span data-stu-id="93e98-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="93e98-165">*Auswählen mehrerer Standardbrowser*</span><span class="sxs-lookup"><span data-stu-id="93e98-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="93e98-166">Google Chrome und Internet Explorer sollten jetzt als Standardbrowser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="93e98-167">Klicken Sie auf **Abbrechen** , um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="93e98-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="93e98-168">![Google Chrome und Internet Explorer als Standardbrowser](visual-studio-2013-web-tools/_static/image3.png "Google Chrome-und Internet Explorer-Standardbrowser")</span><span class="sxs-lookup"><span data-stu-id="93e98-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="93e98-169">*Google Chrome und Internet Explorer als Standardbrowser*</span><span class="sxs-lookup"><span data-stu-id="93e98-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-170">Nach dem Konfigurieren der Standardbrowser wird die Option **mehrere Browser** im Menü Browser ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="93e98-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="93e98-171">![Mehrere Browser](visual-studio-2013-web-tools/_static/image4.png "Mehrere Browser")</span><span class="sxs-lookup"><span data-stu-id="93e98-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="93e98-172">Drücken Sie **STRG** + **F5** , um die Anwendung ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="93e98-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="93e98-173">Wenn beide Browserfenster geöffnet sind, platzieren Sie eine von Ihnen über der anderen, um die Aktualisierungen auf beiden Browsern gleichzeitig anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="93e98-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="93e98-174">Die Browser sollten eine Webseite mit einem hellblauen Rechteck anzeigen.</span><span class="sxs-lookup"><span data-stu-id="93e98-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="93e98-175">![Platzieren eines Browsers über dem anderen](visual-studio-2013-web-tools/_static/image5.png "Platzieren eines Browsers über dem anderen")</span><span class="sxs-lookup"><span data-stu-id="93e98-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="93e98-176">*Platzieren eines Browsers über dem anderen*</span><span class="sxs-lookup"><span data-stu-id="93e98-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="93e98-177">Schließen Sie die Browser nicht.</span><span class="sxs-lookup"><span data-stu-id="93e98-177">Do not close the browsers.</span></span> <span data-ttu-id="93e98-178">Diese werden in der nächsten Aufgabe verwendet.</span><span class="sxs-lookup"><span data-stu-id="93e98-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="93e98-179">Aufgabe 2: Verwenden der Zen-Codierung zum Erstellen von HTML-Elementen</span><span class="sxs-lookup"><span data-stu-id="93e98-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="93e98-180">Bei der **Zen-Codierung** handelt es sich um ein Editor-Plug-in für schnelle HTML-, XML-, XSL-und andere strukturierte Code Formate, die Codieren und bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="93e98-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="93e98-181">Der Kern dieses Plug-ins ist eine leistungsstarke Abkürzung-Engine, mit der Sie Ausdrücke wie CSS-Selektoren in HTML-Code erweitern können.</span><span class="sxs-lookup"><span data-stu-id="93e98-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="93e98-182">Die Zen-Codierung ist eine schnelle Möglichkeit zum Schreiben von HTML mithilfe einer Syntax der CSS-Stil Auswahl.</span><span class="sxs-lookup"><span data-stu-id="93e98-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="93e98-183">In dieser Übung verwenden Sie die von Web Essentials bereitgestellte Zen-Codierungsfunktion, um die HTML-Schaltflächen zu generieren, die die Optionen der Frage darstellen.</span><span class="sxs-lookup"><span data-stu-id="93e98-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="93e98-184">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93e98-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="93e98-185">Öffnen Sie die Datei " **Index. cshtml** ", die sich im Ordner **views** | **Home** befindet.</span><span class="sxs-lookup"><span data-stu-id="93e98-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="93e98-186">Ersetzen **Sie den&lt;!--TODO: hier hinzufügen-Optionen&gt;** Kommentar durch den folgenden Code, und drücken Sie die **Tab**-Taste.</span><span class="sxs-lookup"><span data-stu-id="93e98-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="93e98-187">Der Code sollte auf HTML erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="93e98-188">![Erweitertes HTML](visual-studio-2013-web-tools/_static/image6.png "Erweitertes HTML")</span><span class="sxs-lookup"><span data-stu-id="93e98-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="93e98-189">*Erweitertes HTML*</span><span class="sxs-lookup"><span data-stu-id="93e98-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-190">Weitere Informationen zur Zen-Codierungs Syntax finden Sie im folgenden [Artikel](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="93e98-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="93e98-191">Klicken Sie auf die Schaltfläche Verbindungs **Browser aktualisieren** , um beide Browser zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="93e98-192">![Verknüpfte Browser aktualisieren](visual-studio-2013-web-tools/_static/image7.png "Verknüpfte Browser aktualisieren")</span><span class="sxs-lookup"><span data-stu-id="93e98-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="93e98-193">*Verknüpfte Browser aktualisieren*</span><span class="sxs-lookup"><span data-stu-id="93e98-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="93e98-194">![Internet Explorer-Seite aktualisiert mit vier Schaltflächen](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-Seite aktualisiert mit vier Schaltflächen")</span><span class="sxs-lookup"><span data-stu-id="93e98-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="93e98-195">*Internet Explorer-Seite aktualisiert mit vier Schaltflächen*</span><span class="sxs-lookup"><span data-stu-id="93e98-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="93e98-196">![Google Chrome-Seite mit vier Schaltflächen aktualisiert](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-Seite mit vier Schaltflächen aktualisiert")</span><span class="sxs-lookup"><span data-stu-id="93e98-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="93e98-197">*Google Chrome-Seite mit vier Schaltflächen aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="93e98-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="93e98-198">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93e98-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="93e98-199">Sie haben der Seite die Schaltflächen hinzugefügt, aber Sie müssen dennoch eine Vorlagen Frage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="93e98-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="93e98-200">Zu diesem Zweck verwenden Sie ein neues Feature in Web Essentials namens " **Lorem Ipsum Generator**".</span><span class="sxs-lookup"><span data-stu-id="93e98-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="93e98-201">Suchen Sie das **div** -Element mit dem **Class** -Attribut **Front**.</span><span class="sxs-lookup"><span data-stu-id="93e98-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="93e98-202">Fügen Sie den folgenden Code als erstes untergeordnetes Element des **div**-Elements hinzu, und drücken Sie die **Tab**-Taste.</span><span class="sxs-lookup"><span data-stu-id="93e98-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="93e98-203">Der Code sollte auf HTML erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="93e98-204">!["Lorem ipsum" automatisch generiert](visual-studio-2013-web-tools/_static/image10.png ""Lorem ipsum" automatisch generiert")</span><span class="sxs-lookup"><span data-stu-id="93e98-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="93e98-205">*"Lorem ipsum" automatisch generiert*</span><span class="sxs-lookup"><span data-stu-id="93e98-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-206">Im Rahmen der Zen-Codierung können Sie jetzt den Lorem ipsum-Code direkt im HTML-Editor generieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="93e98-207">Geben Sie einfach " **Lorem** " und "Treffer **Registerkarte** " ein, und ein 30-Wort-ipsum-Text wird eingefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="93e98-208">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="93e98-208">E.g.</span></span> <span data-ttu-id="93e98-209">*lorem10* fügt 10 Lorem ipsum-Wörter ein.</span><span class="sxs-lookup"><span data-stu-id="93e98-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="93e98-210">Sie fügen am Anfang der Frage ein Logo hinzu, indem Sie ein weiteres neues Feature in Web Essentials mit dem Namen " **Lorem Pixel Generator**" verwenden.</span><span class="sxs-lookup"><span data-stu-id="93e98-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="93e98-211">Fügen Sie den folgenden Code als erstes untergeordnetes Element des **div** -Elements mit **Container** als **Klassen** Wert hinzu, und drücken Sie die **Tab**-Taste.</span><span class="sxs-lookup"><span data-stu-id="93e98-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="93e98-212">Der Code sollte in HTML erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="93e98-213">![Lorem-Pixel automatisch generiert](visual-studio-2013-web-tools/_static/image11.png "Lorem-Pixel automatisch generiert")</span><span class="sxs-lookup"><span data-stu-id="93e98-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="93e98-214">*Lorem-Pixel automatisch generiert*</span><span class="sxs-lookup"><span data-stu-id="93e98-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-215">Im Rahmen der Zen-Codierung können Sie auch den Lorem-pixelcode direkt im HTML-Editor generieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="93e98-216">Geben Sie einfach **pix-200 x 200-animals** ein, und klicken Sie auf die **Registerkarte** , und ein **IMG** -Tag mit einem 200 x 200-Bild eines Tieres wird eingefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="93e98-217">Weitere Informationen finden Sie unter [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="93e98-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="93e98-218">Klicken Sie auf die Schaltfläche Verbindungs **Browser aktualisieren** , um beide Browser zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="93e98-219">![Internet Explorer: automatisch generiertes Bild und Text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer: automatisch generiertes Bild und Text")</span><span class="sxs-lookup"><span data-stu-id="93e98-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="93e98-220">*Internet Explorer: automatisch generiertes Bild und Text*</span><span class="sxs-lookup"><span data-stu-id="93e98-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="93e98-221">![Google Chrome-automatisch generiertes Bild und Text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-automatisch generiertes Bild und Text")</span><span class="sxs-lookup"><span data-stu-id="93e98-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="93e98-222">*Google Chrome-automatisch generiertes Bild und Text*</span><span class="sxs-lookup"><span data-stu-id="93e98-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-223">Da das Bild beim Hinzufügen des Code Ausschnitts zufällig ausgewählt wird, kann sich das in den Browsern angezeigte Bild unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="93e98-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="93e98-224">Schließen Sie die Browser nicht.</span><span class="sxs-lookup"><span data-stu-id="93e98-224">Do not close the browsers.</span></span> <span data-ttu-id="93e98-225">Diese werden in der nächsten Aufgabe verwendet.</span><span class="sxs-lookup"><span data-stu-id="93e98-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="93e98-226">Aufgabe 3: Aktualisieren einer Style-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="93e98-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="93e98-227">In dieser Aufgabe verwenden Sie die Funktion zum über **prüfen** des Browsers, um den genauen Speicherort zu ermitteln, an dem das jeweilige DOM-Element generiert wird, und aktualisieren anschließend die Color-Eigenschaft dieses Elements mithilfe einer Farbauswahl, die von Web Essentials bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="93e98-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="93e98-228">Drücken Sie im Internet Explorer-Browser **STRG** + **alt** + **I** , um den Überprüfungs Modus zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="93e98-229">Bewegen Sie den Mauszeiger über den hellen blauen Rahmen, und klicken Sie auf.</span><span class="sxs-lookup"><span data-stu-id="93e98-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="93e98-230">![Bewegen des Zeigers über den hellen blauen Rahmen](visual-studio-2013-web-tools/_static/image14.png "Bewegen des Zeigers über den hellen blauen Rahmen")</span><span class="sxs-lookup"><span data-stu-id="93e98-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="93e98-231">*Bewegen des Zeigers über den hellen blauen Rahmen*</span><span class="sxs-lookup"><span data-stu-id="93e98-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="93e98-232">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93e98-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="93e98-233">Beachten Sie, dass das HTML-Element, das Sie im Browser ausgewählt haben, auch im HTML-Editor von Visual Studio ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="93e98-234">![Im HTML-Editor von Visual Studio ausgewähltes HTML-Element](visual-studio-2013-web-tools/_static/image15.png "Im HTML-Editor von Visual Studio ausgewähltes HTML-Element")</span><span class="sxs-lookup"><span data-stu-id="93e98-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="93e98-235">*Im HTML-Editor von Visual Studio ausgewähltes HTML-Element*</span><span class="sxs-lookup"><span data-stu-id="93e98-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="93e98-236">Sie aktualisieren nun die **Front** -CSS-Klasse, um die Formatierung des ausgewählten Elements zu ändern.</span><span class="sxs-lookup"><span data-stu-id="93e98-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="93e98-237">Drücken Sie hierzu **STRG** +  **,** um das Suchfeld **zu** suchen.</span><span class="sxs-lookup"><span data-stu-id="93e98-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="93e98-238">Geben Sie **Site. CSS** ein und drücken **Sie die Eingabe** Taste, um die Datei zu öffnen</span><span class="sxs-lookup"><span data-stu-id="93e98-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="93e98-239">![Datei Site. CSS wird geöffnet](visual-studio-2013-web-tools/_static/image16.png "Datei Site. CSS wird geöffnet")</span><span class="sxs-lookup"><span data-stu-id="93e98-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="93e98-240">*Datei Site. CSS wird geöffnet*</span><span class="sxs-lookup"><span data-stu-id="93e98-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="93e98-241">Drücken Sie **STRG** + **F** , und geben Sie **. Flip-Container. Front** ein, um die CSS-Auswahl zu suchen.</span><span class="sxs-lookup"><span data-stu-id="93e98-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="93e98-242">Klicken Sie in der Border-Eigenschaft der-Klasse auf das helle blaue Quadrat, um die Farbauswahl zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="93e98-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="93e98-243">![Öffnen der Farbauswahl](visual-studio-2013-web-tools/_static/image17.png "Öffnen der Farbauswahl")</span><span class="sxs-lookup"><span data-stu-id="93e98-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="93e98-244">*Öffnen der Farbauswahl*</span><span class="sxs-lookup"><span data-stu-id="93e98-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="93e98-245">Erweitern Sie die Farbauswahl, indem Sie auf die Chevron-Schaltfläche klicken und eine neue Farbe auswählen.</span><span class="sxs-lookup"><span data-stu-id="93e98-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="93e98-246">![Erweitern der Farbauswahl](visual-studio-2013-web-tools/_static/image18.png "Erweitern der Farbauswahl")</span><span class="sxs-lookup"><span data-stu-id="93e98-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="93e98-247">*Erweitern der Farbauswahl*</span><span class="sxs-lookup"><span data-stu-id="93e98-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="93e98-248">Drücken Sie **STRG** + **alt** + **Eingabe** Taste, um verknüpfte Browser zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="93e98-249">Wechseln Sie zu Internet Explorer, und beachten Sie, dass sich die Farbe des Rahmens geändert hat.</span><span class="sxs-lookup"><span data-stu-id="93e98-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="93e98-250">![Internet Explorer-Rahmenfarbe aktualisiert](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-Rahmenfarbe aktualisiert")</span><span class="sxs-lookup"><span data-stu-id="93e98-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="93e98-251">*Internet Explorer-Rahmenfarbe aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="93e98-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="93e98-252">Wechseln Sie zu Google Chrome, und beachten Sie, dass sich die Farbe des Rahmens geändert hat.</span><span class="sxs-lookup"><span data-stu-id="93e98-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="93e98-253">![Google Chrome-Rahmenfarbe aktualisiert](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-Rahmenfarbe aktualisiert")</span><span class="sxs-lookup"><span data-stu-id="93e98-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="93e98-254">*Google Chrome-Rahmenfarbe aktualisiert*</span><span class="sxs-lookup"><span data-stu-id="93e98-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="93e98-255">Wechseln Sie zurück zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93e98-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="93e98-256">Wechseln Sie zum Ende der Datei " **Site. CSS** ", und drücken Sie **STRG** + **F** , um die **. BTN** -Auswahl zu suchen.</span><span class="sxs-lookup"><span data-stu-id="93e98-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="93e98-257">Beachten Sie, dass die Eigenschaft **-webkit-border-radius** grün unterstrichen ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="93e98-258">![-webkit-border-Radius-Eigenschaft des BTN-Selektor](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-Radius-Eigenschaft des BTN-Selektor")</span><span class="sxs-lookup"><span data-stu-id="93e98-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="93e98-259">*-webkit-border-Radius-Eigenschaft des BTN-Selektor*</span><span class="sxs-lookup"><span data-stu-id="93e98-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="93e98-260">Platzieren Sie die Einfügemarke in der **-webkit-border-radius-** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="93e98-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="93e98-261">Unter dem ersten Buchstaben des ersten Worts der Eigenschaft sollte eine blaue Linie angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="93e98-262">Dies ist das **Smarttag**.</span><span class="sxs-lookup"><span data-stu-id="93e98-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="93e98-263">Drücken Sie die **STRG** - +  **.**</span><span class="sxs-lookup"><span data-stu-id="93e98-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="93e98-264">Öffnen Sie das Vorschlags Menü, und klicken Sie auf **Fehlende Standard Eigenschaft hinzufügen (border-radius)** .</span><span class="sxs-lookup"><span data-stu-id="93e98-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="93e98-265">![Fehlenden Standard Eigenschafts Vorschlag hinzufügen](visual-studio-2013-web-tools/_static/image22.png "Fehlenden Standard Eigenschafts Vorschlag hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="93e98-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="93e98-266">*Fehlenden Standard Eigenschafts Vorschlag hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="93e98-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="93e98-267">Die **Border-RADIUS-** Eigenschaft wird der CSS-Regel automatisch hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="93e98-268">![Fehlende Standard Eigenschaft wurde hinzugefügt.](visual-studio-2013-web-tools/_static/image23.png "Fehlende Standard Eigenschaft wurde hinzugefügt.")</span><span class="sxs-lookup"><span data-stu-id="93e98-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="93e98-269">*Fehlende Standard Eigenschaft wurde hinzugefügt.*</span><span class="sxs-lookup"><span data-stu-id="93e98-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="93e98-270">Bewegen Sie den Mauszeiger über die **Border-RADIUS-** Eigenschaft, um die **Browser Matrix**-QuickInfo anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="93e98-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="93e98-271">Die **Browser Matrix** -QuickInfo zeigt die Verfügbarkeit der Eigenschaft in jedem Browser an.</span><span class="sxs-lookup"><span data-stu-id="93e98-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="93e98-272">![Browser Matrix-QuickInfo](visual-studio-2013-web-tools/_static/image24.png "Browser Matrix-QuickInfo")</span><span class="sxs-lookup"><span data-stu-id="93e98-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="93e98-273">*Browser Matrix-QuickInfo*</span><span class="sxs-lookup"><span data-stu-id="93e98-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="93e98-274">Beachten Sie, dass der Wert der **Border-RADIUS-** Eigenschaft immer noch unterstrichen ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="93e98-275">Bewegen Sie den Zeiger auf den Wert, um die Warnmeldung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="93e98-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="93e98-276">![Eigenschaft Wert für Border-Radius-Eigenschaft](visual-studio-2013-web-tools/_static/image25.png "Eigenschaft Wert für Border-Radius-Eigenschaft")</span><span class="sxs-lookup"><span data-stu-id="93e98-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="93e98-277">*Eigenschaft Wert für Border-Radius-Eigenschaft*</span><span class="sxs-lookup"><span data-stu-id="93e98-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="93e98-278">Entfernen Sie die Einheit des **Border-RADIUS-** Eigenschafts Werts, wie von der QuickInfo vorgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="93e98-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="93e98-279">Da **Border-RADIUS** die Standard Eigenschaft für die Definition von abgerundeten Rahmen Ecken ist, können Sie die **-webkit-border-radius** -Eigenschaft und den Wert aus der CSS-Regel entfernen.</span><span class="sxs-lookup"><span data-stu-id="93e98-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="93e98-280">Platzieren Sie die **Einfügemarke in der Word-Wrap-** Eigenschaft, und beachten Sie, dass das Smarttag auch unten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="93e98-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="93e98-281">Öffnen Sie das Menü, und klicken Sie auf **fehlende Hersteller Spezifikationen hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="93e98-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="93e98-282">![Vorschlag für fehlende Hersteller Besonderheiten hinzufügen](visual-studio-2013-web-tools/_static/image26.png "Vorschlag für fehlende Hersteller Besonderheiten hinzufügen")</span><span class="sxs-lookup"><span data-stu-id="93e98-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="93e98-283">*Vorschlag für fehlende Hersteller Besonderheiten hinzufügen*</span><span class="sxs-lookup"><span data-stu-id="93e98-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="93e98-284">Die **-MS-Word-Wrap** -Eigenschaft wird der CSS-Regel automatisch hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="93e98-285">![Anbieterspezifische Eigenschaft hinzugefügt](visual-studio-2013-web-tools/_static/image27.png "Anbieterspezifische Eigenschaft hinzugefügt")</span><span class="sxs-lookup"><span data-stu-id="93e98-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="93e98-286">*Anbieterspezifische Eigenschaft hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="93e98-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="93e98-287">Aufgabe 4: Aktualisieren des HTML-Codes aus dem Browser</span><span class="sxs-lookup"><span data-stu-id="93e98-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="93e98-288">In dieser Aufgabe verwenden Sie die **Entwurfs Modus** -Funktion des Browser Links, um das DOM-Objekt aus dem Browser zu bearbeiten und die Änderungen an die HTML-Quelldatei in Visual Studio zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="93e98-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="93e98-289">Drücken Sie in Google Chrome **STRG** + **alt** + **D** , um den Entwurfs Modus zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="93e98-290">Bewegen Sie den Mauszeiger über die Bezeichnung " **Lorem ipsum dolor sit amet** ", und klicken Sie auf.</span><span class="sxs-lookup"><span data-stu-id="93e98-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="93e98-291">![Bearbeiten der Frage](visual-studio-2013-web-tools/_static/image28.png "Bearbeiten der Frage")</span><span class="sxs-lookup"><span data-stu-id="93e98-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="93e98-292">*Bearbeiten der Frage*</span><span class="sxs-lookup"><span data-stu-id="93e98-292">*Editing the question*</span></span>
3. <span data-ttu-id="93e98-293">Ein Cursor sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-293">A cursor should appear.</span></span> <span data-ttu-id="93e98-294">Ersetzen Sie den ursprünglichen Text durch das *aussehen, wenn ich eine längere Frage schreibe?* , und drücken Sie dann **ESC** , um den Entwurfs Modus zu beenden.</span><span class="sxs-lookup"><span data-stu-id="93e98-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="93e98-295">![Frage bearbeitet](visual-studio-2013-web-tools/_static/image29.png "Frage bearbeitet")</span><span class="sxs-lookup"><span data-stu-id="93e98-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="93e98-296">*Frage bearbeitet*</span><span class="sxs-lookup"><span data-stu-id="93e98-296">*Question edited*</span></span>
4. <span data-ttu-id="93e98-297">Wechseln Sie zurück zu Visual Studio, und öffnen Sie die Datei **Index. cshtml**, sofern diese nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="93e98-298">Beachten Sie, dass der innere Text des **&lt;p-&gt;** Elements aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="93e98-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="93e98-299">![Aktualisierte Frage auf der HTML-Seite](visual-studio-2013-web-tools/_static/image30.png "Aktualisierte Frage auf der HTML-Seite")</span><span class="sxs-lookup"><span data-stu-id="93e98-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="93e98-300">*Aktualisierte Frage auf der HTML-Seite*</span><span class="sxs-lookup"><span data-stu-id="93e98-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="93e98-301">Aufgabe 5: Überprüfen von SEO-bezogenen Warnungen</span><span class="sxs-lookup"><span data-stu-id="93e98-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="93e98-302">Bei der **Suchmaschinenoptimierung** (SEO) handelt es sich um den Prozess, bei dem der Rang einer Website in der Liste der Ergebnisse einer Suchmaschine höher ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="93e98-303">Je höher die Website Rangfolge ist, desto genauer wird Sie aufgelistet. je mehr Besucher die Website von der Suchmaschine erhalten.</span><span class="sxs-lookup"><span data-stu-id="93e98-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="93e98-304">Web Essentials umfasst ein analytisches Tool, das HTML untersucht, die gefundenen Probleme meldet und Hilfe zur Behebung von Problemen bietet.</span><span class="sxs-lookup"><span data-stu-id="93e98-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="93e98-305">Wechseln Sie zum Menü **Ansicht** , und klicken Sie auf **Fehlerliste** , um das Fenster **Fehlerliste** zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="93e98-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="93e98-306">![Im Menü "Ansicht" Fehlerliste](visual-studio-2013-web-tools/_static/image31.png "Im Menü "Ansicht" Fehlerliste")</span><span class="sxs-lookup"><span data-stu-id="93e98-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="93e98-307">*Im Menü "Ansicht" Fehlerliste*</span><span class="sxs-lookup"><span data-stu-id="93e98-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="93e98-308">Beachten Sie, dass eine SEO-Warnung mit dem Hinweis angezeigt wird, dass ein **&lt;Meta&gt;** -Tag für die Seiten Beschreibung fehlt.</span><span class="sxs-lookup"><span data-stu-id="93e98-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="93e98-309">Doppelklicken Sie auf den Eintrag SEO Warning, um ihn zu beheben.</span><span class="sxs-lookup"><span data-stu-id="93e98-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="93e98-310">![Fehlerliste (Fenster)](visual-studio-2013-web-tools/_static/image32.png "Fehlerliste (Fenster)")</span><span class="sxs-lookup"><span data-stu-id="93e98-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="93e98-311">*Fehlerliste (Fenster)*</span><span class="sxs-lookup"><span data-stu-id="93e98-311">*Error List window*</span></span>
3. <span data-ttu-id="93e98-312">Klicken Sie im Dialogfeld **Web Essentials** auf **Ja** , um eine Beschreibung &lt;Meta&gt;-Tags einzufügen.</span><span class="sxs-lookup"><span data-stu-id="93e98-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="93e98-313">![WebEssentials (Dialogfeld)](visual-studio-2013-web-tools/_static/image33.png "WebEssentials (Dialogfeld)")</span><span class="sxs-lookup"><span data-stu-id="93e98-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="93e98-314">*WebEssentials (Dialogfeld)*</span><span class="sxs-lookup"><span data-stu-id="93e98-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="93e98-315">Der Editor für **\_Layout. cshtml** wird geöffnet, und das Tag **&lt;Meta&gt;** wird automatisch dem **Head** -Abschnitt der HTML-Datei hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="93e98-316">![Das Meta-Tag wurde automatisch auf _Layout Seite hinzugefügt.](visual-studio-2013-web-tools/_static/image34.png "Das Meta-Tag wurde automatisch auf _Layout Seite hinzugefügt.")</span><span class="sxs-lookup"><span data-stu-id="93e98-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="93e98-317">*Das Meta-Tag wurde \_Layoutseite automatisch hinzugefügt*</span><span class="sxs-lookup"><span data-stu-id="93e98-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="93e98-318">Ändern Sie den Wert des **Content** -Attributs in *geekquiz* , und speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="93e98-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="93e98-319">Übung 2: Nutzen von Code Ausschnitten und IntelliSense</span><span class="sxs-lookup"><span data-stu-id="93e98-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="93e98-320">Mit Web Essentials wurde der HTML-Editor mit zusätzlicher Funktionalität erweitert.</span><span class="sxs-lookup"><span data-stu-id="93e98-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="93e98-321">In dieser Übung werden einige neue Features angezeigt, die beim Entwickeln von Webanwendungen hilfreich sind.</span><span class="sxs-lookup"><span data-stu-id="93e98-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="93e98-322">Aufgabe 1: Verwenden von IntelliSense in HTML-Dokumenten</span><span class="sxs-lookup"><span data-stu-id="93e98-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="93e98-323">Das erste neue Feature, das in dieser Aufgabe angezeigt wird, wird als **dynamischer IntelliSense**bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="93e98-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="93e98-324">Dynamische IntelliSense liest andere Tags und Attribute, um die möglichen IDs abzuleiten, die Sie verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="93e98-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="93e98-325">In dieser Aufgabe erstellen Sie ein neues HTML-Formular Element, das eine Bezeichnung und ein Eingabefeld enthält.</span><span class="sxs-lookup"><span data-stu-id="93e98-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="93e98-326">Dann fügen Sie der Bezeichnung ein **for** -Attribut hinzu, um es an die Eingabe zu binden, und es werden IntelliSense-Vorschläge auf der Grundlage der IDs der Eingaben im Bereich angezeigt.</span><span class="sxs-lookup"><span data-stu-id="93e98-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="93e98-327">Öffnen Sie **Visual Studio Express 2013 für das Web** und die Projekt Mappe " **BEGIN. sln** " im Ordner " **Source/EX2-takingbegünstigeof codesnippeer-andintellisense/BEGIN** ".</span><span class="sxs-lookup"><span data-stu-id="93e98-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="93e98-328">Alternativ können Sie mit der Lösung fortfahren, die Sie in der vorherigen Übung abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="93e98-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="93e98-329">Öffnen Sie in **Projektmappen-Explorer**die Datei " **Index. cshtml** ", die sich im Ordner " **views** | **Home** " befindet.</span><span class="sxs-lookup"><span data-stu-id="93e98-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="93e98-330">Fügen Sie das folgende Formular innerhalb des **&lt;Abschnitts&gt;** -Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="93e98-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="93e98-331">(Code Ausschnitt- *VisualStudio2013WebTooling* - *ex2* - *Formular*)</span><span class="sxs-lookup"><span data-stu-id="93e98-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="93e98-332">Dem Eingabetag sollte eine Bezeichnung mit einer Beschreibung des Felds vorangestellt sein.</span><span class="sxs-lookup"><span data-stu-id="93e98-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="93e98-333">Fügen Sie die folgende Bezeichnung vor dem Eingabetag hinzu.</span><span class="sxs-lookup"><span data-stu-id="93e98-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="93e98-334">Das **for** -Attribut einer **&lt;Bezeichnung&gt;** gibt an, an welches Formular Element eine Bezeichnung gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="93e98-335">Der Wert des Attributs sollte gleich der ID des zugehörigen Elements sein.</span><span class="sxs-lookup"><span data-stu-id="93e98-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="93e98-336">Fügen Sie das **for** -Attribut der **&lt;Bezeichnung&gt;** -Elements hinzu.</span><span class="sxs-lookup"><span data-stu-id="93e98-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="93e98-337">Wie in der folgenden Abbildung gezeigt, wird der &quot;Name&quot; Wert im Feld IntelliSense angezeigt, basierend auf der ID der Elemente innerhalb desselben Bereichs (das einschließende **&lt;Formular&gt;** ).</span><span class="sxs-lookup"><span data-stu-id="93e98-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="93e98-338">![Anzeige der ID in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Anzeige der ID in IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="93e98-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="93e98-339">*Anzeige der ID in IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="93e98-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="93e98-340">Löschen Sie das zuletzt hinzugefügte **&lt;Formular&gt;** Element und dessen Inhalt.</span><span class="sxs-lookup"><span data-stu-id="93e98-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="93e98-341">Aufgabe 2: Verwenden von HTML-Code Ausschnitten</span><span class="sxs-lookup"><span data-stu-id="93e98-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="93e98-342">HTML5 hat mehr als 25 neue Semantik Tags eingeführt.</span><span class="sxs-lookup"><span data-stu-id="93e98-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="93e98-343">Visual Studio verfügte bereits über IntelliSense-Unterstützung für diese Tags, Visual Studio 2013 jedoch das Schreiben von Markup durch Hinzufügen neuer Code Ausschnitte schneller und einfacher.</span><span class="sxs-lookup"><span data-stu-id="93e98-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="93e98-344">Obwohl diese Tags nicht kompliziert sind, sind Sie mit einigen kleinen Feinheiten verknüpft, wie z. b. das Hinzufügen der richtigen Codec-Fallbacks für das *audiotag* .</span><span class="sxs-lookup"><span data-stu-id="93e98-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="93e98-345">In dieser Aufgabe werden die HTML-Code Ausschnitte für das audiotag angezeigt.</span><span class="sxs-lookup"><span data-stu-id="93e98-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="93e98-346">Geben Sie in der Datei **Index. cshtml** **&lt;AUD** innerhalb des **&lt;Abschnitts&gt;** -Element ein, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="93e98-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="93e98-347">![Einfügen eines Audioelements](visual-studio-2013-web-tools/_static/image36.png "Einfügen eines Audioelements")</span><span class="sxs-lookup"><span data-stu-id="93e98-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="93e98-348">*Einfügen eines Audioelements*</span><span class="sxs-lookup"><span data-stu-id="93e98-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="93e98-349">Drücken Sie zweimal die **Tab** -Taste, und beachten Sie, dass der folgende Code auf der Seite hinzugefügt wird und der Cursor auf dem **src** -Attribut der ersten Quelle platziert wird.</span><span class="sxs-lookup"><span data-stu-id="93e98-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="93e98-350">Wenn Sie die **Tab** -Taste zweimal drücken, wird der Code Ausschnitt eingefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="93e98-351">Der Audioausschnitt zeigt die Standardverwendung des *Audiotags* mit zwei Quelldateien für eine verbesserte Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="93e98-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="93e98-352">Löschen Sie die zweite Zeile, und aktualisieren Sie die Quelle der ersten Zeile mit dem folgenden Link zum webcampstv Katana Show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="93e98-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="93e98-353">Der resultierende Code ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="93e98-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="93e98-354">Die Quelldatei " *katanaproject. MP3* " wird als Beispiel verwendet.</span><span class="sxs-lookup"><span data-stu-id="93e98-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="93e98-355">Wenn Sie möchten, können Sie eine andere Quelle verwenden.</span><span class="sxs-lookup"><span data-stu-id="93e98-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="93e98-356">Drücken Sie **STRG** + **S** , um die Datei zu speichern.</span><span class="sxs-lookup"><span data-stu-id="93e98-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="93e98-357">Drücken Sie **STRG** + **F5** , um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="93e98-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="93e98-358">Beachten Sie, dass der Anwendung ein Audioplayer hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="93e98-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="93e98-359">![Audioplayer in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audioplayer in Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="93e98-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="93e98-360">*Audioplayer in Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="93e98-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="93e98-361">![Audioplayer in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audioplayer in Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="93e98-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="93e98-362">*Audioplayer in Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="93e98-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="93e98-363">Schließen Sie die Browser nicht.</span><span class="sxs-lookup"><span data-stu-id="93e98-363">Do not close the browsers.</span></span> <span data-ttu-id="93e98-364">Diese werden in der nächsten Aufgabe verwendet.</span><span class="sxs-lookup"><span data-stu-id="93e98-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="93e98-365">Aufgabe 3: Verwenden von IntelliSense in JavaScript-Dokumenten</span><span class="sxs-lookup"><span data-stu-id="93e98-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="93e98-366">Mit Web Essentials 2013 werden in Stylesheets und HTML-Seiten eine Liste von IDs und Klassennamen erzeugt.</span><span class="sxs-lookup"><span data-stu-id="93e98-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="93e98-367">In dieser Aufgabe erfahren Sie, wie diese Listen die Unterstützung von JavaScript IntelliSense in Web Essentials 2013 verbessern.</span><span class="sxs-lookup"><span data-stu-id="93e98-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="93e98-368">Fügen Sie in der Datei " **Index. cshtml** " den folgenden Code hinzu, um ein **Skripttag** für JavaScript-Code zu definieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="93e98-369">Fügen Sie den folgenden Code innerhalb des **Skripttags** hinzu, um die fertige Rückruffunktion zu definieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="93e98-370">(Code Ausschnitt- *VisualStudio2013WebTooling* - *ex2* - - *Funktion*)</span><span class="sxs-lookup"><span data-stu-id="93e98-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="93e98-371">Platzieren Sie die Einfügemarke im **Skripttags** , und drücken Sie **STRG** +  **.**</span><span class="sxs-lookup"><span data-stu-id="93e98-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="93e98-372">, um das Vorschlags Menü zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="93e98-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="93e98-373">Klicken Sie **auf in Datei extrahieren**.</span><span class="sxs-lookup"><span data-stu-id="93e98-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="93e98-374">![Vorschlag zum Extrahieren von JavaScript in Dateien](visual-studio-2013-web-tools/_static/image39.png "Vorschlag zum Extrahieren von JavaScript in Dateien")</span><span class="sxs-lookup"><span data-stu-id="93e98-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="93e98-375">*Vorschlag zum Extrahieren von JavaScript in Dateien*</span><span class="sxs-lookup"><span data-stu-id="93e98-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="93e98-376">Wählen Sie im Fenster **Speichern** unter den Ordner **Skripts** aus, benennen Sie die Datei **init. js** , und klicken Sie auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="93e98-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="93e98-377">![Als Fenster speichern](visual-studio-2013-web-tools/_static/image40.png "Als Fenster speichern")</span><span class="sxs-lookup"><span data-stu-id="93e98-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="93e98-378">*Als Fenster speichern*</span><span class="sxs-lookup"><span data-stu-id="93e98-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-379">Die Datei " **init. js** " wird erstellt, und der Inhalt des Skripts wird in die Datei verschoben.</span><span class="sxs-lookup"><span data-stu-id="93e98-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="93e98-380">![Init. js-Datei, die mit dem enthaltenen Inhalt erstellt wurde](visual-studio-2013-web-tools/_static/image41.png "Init. js-Datei, die mit dem enthaltenen Inhalt erstellt wurde")</span><span class="sxs-lookup"><span data-stu-id="93e98-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="93e98-381">*Init. js-Datei, die mit dem enthaltenen Inhalt erstellt wurde*</span><span class="sxs-lookup"><span data-stu-id="93e98-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="93e98-382">Öffnen Sie die Datei **Index. cshtml** , und überprüfen Sie, ob das Skript-Tag durch einen Verweis auf die Datei **init. js** ersetzt wurde.</span><span class="sxs-lookup"><span data-stu-id="93e98-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="93e98-383">![Init. js-html-Referenz](visual-studio-2013-web-tools/_static/image42.png "Init. js-html-Referenz")</span><span class="sxs-lookup"><span data-stu-id="93e98-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="93e98-384">*Init. js-html-Referenz*</span><span class="sxs-lookup"><span data-stu-id="93e98-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="93e98-385">Wechseln Sie zum **Projektmappen-Explorer** , und beachten Sie, dass die Datei " **init. js** " automatisch in der Lösung enthalten war.</span><span class="sxs-lookup"><span data-stu-id="93e98-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="93e98-386">![In der Lösung enthaltene Datei "init. js"](visual-studio-2013-web-tools/_static/image43.png "In der Lösung enthaltene Datei "init. js"")</span><span class="sxs-lookup"><span data-stu-id="93e98-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="93e98-387">*In der Lösung enthaltene Datei "init. js"*</span><span class="sxs-lookup"><span data-stu-id="93e98-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="93e98-388">Wechseln Sie zurück zur Datei " **init. js** ", um den **Ready** -Funktions Rückruf zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="93e98-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="93e98-389">Fügen Sie in der Funktions Rückruf Definition, die an *Ready*zurückgegeben wird, den folgenden Code hinzu, um alle Elemente nach einem bestimmten Klassen Attribut zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="93e98-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="93e98-390">Drücken Sie **STRG** + **LEERTASTE** zwischen den Anführungszeichen innerhalb des **getElementsByClassName** -Funktions Aufrufes.</span><span class="sxs-lookup"><span data-stu-id="93e98-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="93e98-391">![IntelliSense für die getElementsByClassName-Funktion wird angezeigt.](visual-studio-2013-web-tools/_static/image44.png "IntelliSense für die getElementsByClassName-Funktion wird angezeigt.")</span><span class="sxs-lookup"><span data-stu-id="93e98-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="93e98-392">*IntelliSense für die getElementsByClassName-Funktion wird angezeigt.*</span><span class="sxs-lookup"><span data-stu-id="93e98-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-393">Beachten Sie, dass IntelliSense die Klassen anzeigt, die in den Projekt Stylesheets definiert sind.</span><span class="sxs-lookup"><span data-stu-id="93e98-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="93e98-394">Ersetzen Sie die Zeile, die Sie erstellt haben, durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="93e98-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="93e98-395">Positionieren Sie den Cursor nach **au** innerhalb der Anführungszeichen in der **GetElementsByTagName** -Funktion, und drücken Sie **STRG** + **LEERTASTE**.</span><span class="sxs-lookup"><span data-stu-id="93e98-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="93e98-396">![IntelliSense für die getelementbytagname-Methode wird angezeigt.](visual-studio-2013-web-tools/_static/image45.png "IntelliSense für die getelementbytagname-Methode wird angezeigt.")</span><span class="sxs-lookup"><span data-stu-id="93e98-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="93e98-397">*IntelliSense für die GetElementsByTagName-Methode wird angezeigt.*</span><span class="sxs-lookup"><span data-stu-id="93e98-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="93e98-398">Wählen Sie **&quot;Audio&quot;** aus der Liste, und drücken **Sie die Eingabe**Taste.</span><span class="sxs-lookup"><span data-stu-id="93e98-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="93e98-399">Das Ergebnis ist in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="93e98-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="93e98-400">![Abrufen von Audioelementen](visual-studio-2013-web-tools/_static/image46.png "Abrufen von Audioelementen")</span><span class="sxs-lookup"><span data-stu-id="93e98-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="93e98-401">*Abrufen von Audioelementen*</span><span class="sxs-lookup"><span data-stu-id="93e98-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="93e98-402">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Datei " **init. js** " im Ordner " **Scripts** ", und wählen Sie im Menü **Web Essentials** die Option **Minify JavaScript File (s)** aus.</span><span class="sxs-lookup"><span data-stu-id="93e98-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="93e98-403">![JavaScript-Datei (en) minimieren](visual-studio-2013-web-tools/_static/image47.png "Minimieren von JavaScript-Dateien")</span><span class="sxs-lookup"><span data-stu-id="93e98-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="93e98-404">*JavaScript-Datei (en) minimieren*</span><span class="sxs-lookup"><span data-stu-id="93e98-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="93e98-405">Wenn Sie zur Aktivierung der automatischen Minimierung aufgefordert werden, wenn sich die Quelldatei ändert, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="93e98-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="93e98-406">![Aktivieren der automatischen minierungs Warnung](visual-studio-2013-web-tools/_static/image48.png "Aktivieren der automatischen minierungs Warnung")</span><span class="sxs-lookup"><span data-stu-id="93e98-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="93e98-407">*Aktivieren der automatischen minierungs Warnung*</span><span class="sxs-lookup"><span data-stu-id="93e98-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="93e98-408">Die Datei " **init. min. js** " wird erstellt und als Abhängigkeit von der Datei " **init. js** " hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="93e98-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="93e98-409">![Init. min. js-Datei erstellt](visual-studio-2013-web-tools/_static/image49.png "Init. min. js-Datei erstellt")</span><span class="sxs-lookup"><span data-stu-id="93e98-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="93e98-410">*Init. min. js-Datei erstellt*</span><span class="sxs-lookup"><span data-stu-id="93e98-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="93e98-411">Öffnen Sie die Datei " **init. min. js** ", und beachten Sie, dass die Datei minimiert ist.</span><span class="sxs-lookup"><span data-stu-id="93e98-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="93e98-412">![Inhalt der Datei "init. min. js"](visual-studio-2013-web-tools/_static/image50.png "Inhalt der Datei "init. min. js"")</span><span class="sxs-lookup"><span data-stu-id="93e98-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="93e98-413">*Inhalt der Datei "init. min. js"*</span><span class="sxs-lookup"><span data-stu-id="93e98-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="93e98-414">Fügen Sie in der Datei **init. js** den folgenden Code unterhalb des **GetElementsByTagName** -Funktions Aufrufes ein, um alle Audioelemente wiederzugeben.</span><span class="sxs-lookup"><span data-stu-id="93e98-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="93e98-415">(Code Ausschnitt- *VisualStudio2013WebTooling* - *ex2* - *playaudioelements*)</span><span class="sxs-lookup"><span data-stu-id="93e98-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="93e98-416">Klicken Sie auf **STRG** + **S** , um die Datei zu speichern.</span><span class="sxs-lookup"><span data-stu-id="93e98-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="93e98-417">Da die minierte Datei bereits geöffnet ist, wird ein Dialogfeld angezeigt, das besagt, dass die Datei außerhalb des Quell-Editors geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="93e98-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="93e98-418">Klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="93e98-418">Click **Yes**.</span></span>

    <span data-ttu-id="93e98-419">![Microsoft Visual Studio Warnung](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio Warnung")</span><span class="sxs-lookup"><span data-stu-id="93e98-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="93e98-420">*Microsoft Visual Studio Warnung*</span><span class="sxs-lookup"><span data-stu-id="93e98-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="93e98-421">Wechseln Sie zurück zur Datei " **init. min. js** ", um zu überprüfen, ob die Datei mit dem neuen Code aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="93e98-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="93e98-422">![Die Datei "init. min. js" wurde aktualisiert.](visual-studio-2013-web-tools/_static/image52.png "Die Datei "init. min. js" wurde aktualisiert.")</span><span class="sxs-lookup"><span data-stu-id="93e98-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="93e98-423">*Die Datei "init. min. js" wurde aktualisiert.*</span><span class="sxs-lookup"><span data-stu-id="93e98-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="93e98-424">Klicken Sie auf die Schaltfläche zum **Aktualisieren des Browser Links** .</span><span class="sxs-lookup"><span data-stu-id="93e98-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="93e98-425">Nachdem beide Browser aktualisiert wurden, werden die Audioplayer, die Sie in der vorherigen Aufgabe gesehen haben, automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="93e98-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="93e98-426">![In der Ansicht enthaltene Audioplayer](visual-studio-2013-web-tools/_static/image53.png "In der Ansicht enthaltene Audioplayer")</span><span class="sxs-lookup"><span data-stu-id="93e98-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="93e98-427">*In der Ansicht enthaltene Audioplayer*</span><span class="sxs-lookup"><span data-stu-id="93e98-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="93e98-428">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="93e98-428">Summary</span></span>

<span data-ttu-id="93e98-429">Durch die Durchführung dieses praktischen Labs haben Sie Folgendes gelernt:</span><span class="sxs-lookup"><span data-stu-id="93e98-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="93e98-430">Verwenden neuer HTML-Editor-Funktionen, die in Web Essentials enthalten sind, z. b. umfangreiche HTML5-Code Ausschnitte</span><span class="sxs-lookup"><span data-stu-id="93e98-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="93e98-431">Verwenden neuer CSS-Editor-Features in WebEssentials, z. b. Farbauswahl und Browser Matrix-QuickInfo</span><span class="sxs-lookup"><span data-stu-id="93e98-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="93e98-432">Verwenden neuer JavaScript-Editor-Funktionen in WebEssentials, z. b. Extrahieren in Dateien und IntelliSense für alle HTML-Elemente</span><span class="sxs-lookup"><span data-stu-id="93e98-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="93e98-433">Austauschen von Daten zwischen Ihrem Browser und Visual Studio mithilfe von Browser Link</span><span class="sxs-lookup"><span data-stu-id="93e98-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
