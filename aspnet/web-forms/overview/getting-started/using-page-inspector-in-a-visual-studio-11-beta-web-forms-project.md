---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Verwenden von Seitenprüfung für Visual Studio 2012 in ASP.net Web Forms | Microsoft-Dokumentation
author: rick-anderson
description: Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser. Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464943"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="44ebc-104">Verwenden der Seitenprüfung für Visual Studio 2012 in ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="44ebc-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>

<span data-ttu-id="44ebc-105">von Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="44ebc-105">by Tim Ammann</span></span>

> <span data-ttu-id="44ebc-106">Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser.</span><span class="sxs-lookup"><span data-stu-id="44ebc-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="44ebc-107">Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung die Quelle und das CSS des Elements sofort hervorheben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="44ebc-108">Sie können eine beliebige Seite in der Anwendung durchsuchen, schnell nach den Quellen des gerenderten Markups suchen und die Browser Tools direkt in der Visual Studio-Umgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="44ebc-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="44ebc-109">In diesem Tutorial wird gezeigt, wie Sie Überprüfungsmodus aktivieren und dann schnell CSS-Regeln und Text in Ihrem Webprojekt suchen und bearbeiten können.</span><span class="sxs-lookup"><span data-stu-id="44ebc-109">This tutorial shows how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="44ebc-110">In diesem Tutorial wird ein Web Forms Anwendungsprojekt verwendet, Sie können jedoch auch Seitenprüfung für Website Projekte und [MVC](https://go.microsoft.com/?linkid=9802002) -Anwendungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="44ebc-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="44ebc-111">Das Tutorial enthält die folgenden Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="44ebc-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="44ebc-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="44ebc-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="44ebc-113">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="44ebc-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="44ebc-114">Verwenden Sie Seitenprüfung, um die Anwendung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="44ebc-115">Aktivieren von Überprüfungsmodus</span><span class="sxs-lookup"><span data-stu-id="44ebc-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="44ebc-116">Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup</span><span class="sxs-lookup"><span data-stu-id="44ebc-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="44ebc-117">Überprüfungsmodus und das HTML-Fenster</span><span class="sxs-lookup"><span data-stu-id="44ebc-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="44ebc-118">Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"</span><span class="sxs-lookup"><span data-stu-id="44ebc-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="44ebc-119">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="44ebc-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="44ebc-120">Verwenden der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="44ebc-120">Using the CSS Color Picker</span></span>](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="44ebc-121">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="44ebc-121">Prerequisites</span></span>

- <span data-ttu-id="44ebc-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="44ebc-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="44ebc-123">Um die neueste Version von Seitenprüfung zu erhalten, verwenden Sie den [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) , um das Azure SDK für .NET 2,0 zu installieren.</span><span class="sxs-lookup"><span data-stu-id="44ebc-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="44ebc-124">Seitenprüfung ist mit Microsoft Web Developer Tools gebündelt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="44ebc-125">Die neueste Version ist 1,3.</span><span class="sxs-lookup"><span data-stu-id="44ebc-125">The latest version is 1.3.</span></span> <span data-ttu-id="44ebc-126">Um zu überprüfen, welche Version Sie besitzen, führen Sie Visual Studio aus, und wählen **Microsoft Visual Studio** Sie im Menü **Hilfe** die Option Info aus.</span><span class="sxs-lookup"><span data-stu-id="44ebc-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="44ebc-127">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="44ebc-127">Create a Web Application</span></span>

<span data-ttu-id="44ebc-128">Zuerst erstellen Sie eine Webanwendung, die Sie Seitenprüfung mit verwenden.</span><span class="sxs-lookup"><span data-stu-id="44ebc-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="44ebc-129">Wählen Sie in Visual Studio **Datei** &gt; **Neues Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="44ebc-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="44ebc-130">Erweitern Sie auf der linken **Seite C#Visualisierung** , wählen Sie **Web**aus, und wählen Sie dann **ASP.net Web Forms Anwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="44ebc-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Neue Web Forms Anwendung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="44ebc-132">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="44ebc-132">Click **OK**.</span></span>

<span data-ttu-id="44ebc-133">Die Anwendung wird in der **Quell** Ansicht geöffnet.</span><span class="sxs-lookup"><span data-stu-id="44ebc-133">The application opens in **Source** view.</span></span>

![Neue Web Forms Anwendung in der Quell Ansicht](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="44ebc-135">Nachdem Sie nun über eine Anwendung verfügen, mit der Sie arbeiten können, können Sie Seitenprüfung verwenden, um Sie zu überprüfen und zu ändern.</span><span class="sxs-lookup"><span data-stu-id="44ebc-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="44ebc-136">Verwenden Sie Seitenprüfung, um die Anwendung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="44ebc-137">Im nächsten Schritt wird die Anwendung mit Seitenprüfung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="44ebc-138">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie dann **in Seitenprüfung anzeigen aus**.</span><span class="sxs-lookup"><span data-stu-id="44ebc-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![In Seitenprüfung anzeigen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="44ebc-140">Wenn Seitenprüfung zum ersten Mal gestartet wird, wird es standardmäßig auf der linken Seite der Visual Studio-Umgebung als schmale Fenster angedockt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="44ebc-141">Lassen Sie Sie auf der linken Seite angedockt, legen Sie Sie auf eine für Sie bequeme Breite fest, oder docken Sie Sie in einem der Tool Bereiche oben, unten oder rechts an:</span><span class="sxs-lookup"><span data-stu-id="44ebc-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Andock Positionen Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="44ebc-143">Wenn Sie das Seitenprüfung Fenster abdocken, können Sie es außerhalb von Visual Studio oder sogar auf einem zweiten Monitor platzieren.</span><span class="sxs-lookup"><span data-stu-id="44ebc-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="44ebc-144">Wenn das Seitenprüfung Fenster nicht angedockt ist, **wechseln Sie zu Extras &gt;** **Optionen** &gt; **Umgebung** &gt; **Registerkarten und Fenster**, um zwischen Seitenprüfung und Visual Studio die Tab-Taste + Tab-Taste zu wechseln, und deaktivieren Sie unter " **Tab**" das Kontrollkästchen " **Floating Tool Windows immer" im oberen Bereich des Hauptfensters**:</span><span class="sxs-lookup"><span data-stu-id="44ebc-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Deaktivieren Sie das Kontrollkästchen "Floating Tool Windows" in Alt + Tab zwischen Visual Studio und dem nicht angedockten Seitenprüfung Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="44ebc-146">Im oberen Bereich des Fensters Seitenprüfung wird die aktuelle Seite in einem Browserfenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="44ebc-147">Der untere Bereich zeigt die Seite im HTML-Markup auf der linken Seite und einige Registerkarten auf der rechten Seite an, mit denen Sie verschiedene Aspekte der Seite überprüfen können.</span><span class="sxs-lookup"><span data-stu-id="44ebc-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="44ebc-148">Der untere Bereich ähnelt dem [Entwicklertools F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="44ebc-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="44ebc-149">(Im Gegensatz zu den Entwicklertools können Sie jedoch Seitenprüfung direkt in Visual Studio verwenden.)</span><span class="sxs-lookup"><span data-stu-id="44ebc-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="44ebc-151">In diesem Tutorial verwenden Sie den Seitenprüfung Browserbereich und die Registerkarten **HTML** und **Stile** , um Sie bei der schnellen Navigation und Änderung der Anwendung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="44ebc-152">Aktivieren von Überprüfungsmodus</span><span class="sxs-lookup"><span data-stu-id="44ebc-152">Enable Inspection Mode</span></span>

<span data-ttu-id="44ebc-153">Als nächstes sehen Sie, wie Seitenprüfung Überprüfungsmodus funktioniert.</span><span class="sxs-lookup"><span data-stu-id="44ebc-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="44ebc-154">Klicken Sie im Seitenprüfung Fenster auf die Schaltfläche über **prüfen** .</span><span class="sxs-lookup"><span data-stu-id="44ebc-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Element überprüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="44ebc-156">Um den Inspektions Modus in Aktion zu sehen, bewegen Sie den Mauszeiger über verschiedene Teile der Seite im Seitenprüfung Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="44ebc-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="44ebc-157">Wie Sie dies tun, ändert sich der Mauszeiger in ein großes Pluszeichen, und das untergeordnete Element wird hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="44ebc-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Zeigen Sie mit dem Mauszeiger auf div. Content-Wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="44ebc-159">Wenn Sie den Mauszeiger bewegen, beachten Sie, dass</span><span class="sxs-lookup"><span data-stu-id="44ebc-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="44ebc-160">Der Inhalt in der **Quell** Ansicht wird geändert, um das Markup anzuzeigen, das dem ausgewählten Element auf der Seite entspricht.</span><span class="sxs-lookup"><span data-stu-id="44ebc-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="44ebc-161">Das relevante Markup wird hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="44ebc-162">Wenn sich die Quelle in einer anderen Datei befindet, wird diese Datei in der Quell Ansicht geöffnet, wobei das relevante Markup hervorgehoben ist.</span><span class="sxs-lookup"><span data-stu-id="44ebc-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="44ebc-163">Das Markup, das auf der Registerkarte **HTML** in Seitenprüfung angezeigt wird, ändert sich auch entsprechend dem ausgewählten Element auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="44ebc-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="44ebc-164">Auf der Registerkarte **HTML** wird das relevante Markup aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="44ebc-165">Die Registerkarte **Stile** zeigt die CSS-Regeln an, die für die aktuelle Auswahl relevant sind.</span><span class="sxs-lookup"><span data-stu-id="44ebc-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="44ebc-166">Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup</span><span class="sxs-lookup"><span data-stu-id="44ebc-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="44ebc-167">Nun sehen Sie, wie Sie mithilfe von Seitenprüfung Markup oder Text suchen und Änderungen vornehmen können, deren Speicherort möglicherweise nicht sofort ersichtlich ist.</span><span class="sxs-lookup"><span data-stu-id="44ebc-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="44ebc-168">Legen Sie Seitenprüfung in Überprüfungsmodus, und Scrollen Sie zum unteren Rand der Startseite.</span><span class="sxs-lookup"><span data-stu-id="44ebc-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="44ebc-169">Sobald Sie den Footerbereich eingeben, öffnet Seitenprüfung die Layoutdatei " *Site. Master* " in der **Quell** Ansicht auf einer temporären Registerkarte rechts von den anderen Registerkarten und hebt den Abschnitt der von Ihnen ausgewählten Master Seite hervor.</span><span class="sxs-lookup"><span data-stu-id="44ebc-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="44ebc-170">Dadurch wird gezeigt, wie Seitenprüfung Inhalt auf einer Seite suchen und anzeigen können, die tatsächlich aus einer anderen als der ursprünglich geöffneten Datei stammt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Footer-Highlights in Überprüfungsmodus](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="44ebc-172">Bewegen Sie den Mauszeiger im Seitenprüfung Browserfenster über die Zeile mit dem Urheberrechts <a id="a"> </a>Hinweis.</span><span class="sxs-lookup"><span data-stu-id="44ebc-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="44ebc-173">Auf der Seite *Site. Master* wird die entsprechende Zeile hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Vorder-und-Fußzeile hervorgehoben](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="44ebc-175">Fügen Sie am Ende der Zeile in der Datei " *Site. Master* " Text hinzu.</span><span class="sxs-lookup"><span data-stu-id="44ebc-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="44ebc-176">&lt;p&gt;&amp;kopieren; &lt;%: DateTime. now. Year%&gt;-My ASP.NET Application Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="44ebc-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="44ebc-177">Drücken Sie nun Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste, um die Ergebnisse im Seitenprüfung Browserfenster anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="44ebc-179">Möglicherweise haben Sie sich der Fußzeile auf der Seite " *default. aspx* " befunden, sich jedoch auf der Seite "Master Layout" befunden und Seitenprüfung für Sie gefunden.</span><span class="sxs-lookup"><span data-stu-id="44ebc-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="44ebc-180">Überprüfungsmodus und das HTML-Fenster</span><span class="sxs-lookup"><span data-stu-id="44ebc-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="44ebc-181">Im nächsten Schritt sehen Sie sich das HTML-Fenster und die Art der Zuordnung von Elementen an.</span><span class="sxs-lookup"><span data-stu-id="44ebc-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="44ebc-182">Legen Sie Seitenprüfung in Überprüfungsmodus.</span><span class="sxs-lookup"><span data-stu-id="44ebc-182">Put Page Inspector in Inspection Mode.</span></span>

![Element überprüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="44ebc-184">Klicken Sie auf den oberen Teil der Seite, der "Ihr Logo hier" anzeigt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="44ebc-185">Sie untersuchen ein bestimmtes Element ausführlicher, sodass sich die Anzeige im Browserfenster nicht mehr ändert, wenn Sie den Mauszeiger bewegen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="44ebc-186">Bewegen Sie nun den Mauszeiger auf das **HTML** -Fenster.</span><span class="sxs-lookup"><span data-stu-id="44ebc-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="44ebc-187">Wenn Sie den Mauszeiger bewegen, wird Seitenprüfung das Element innerhalb des **HTML** -Fensters umrissen und das entsprechende Element im Browserfenster hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML-Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="44ebc-189">Wie zuvor öffnet Seitenprüfung die Datei " *Site. Master* " für Sie auf einer temporären Registerkarte. Klicken Sie auf die Registerkarte "Site. Master", und das entsprechende Markup wird im&gt; Abschnitt &lt;Header hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="44ebc-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Hervorgehobene Markup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="44ebc-191">Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"</span><span class="sxs-lookup"><span data-stu-id="44ebc-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="44ebc-192">Als Nächstes erfahren Sie, wie Sie das Fenster Seitenprüfung **Stile** zum Anzeigen einer Vorschau von Änderungen an CSS verwenden können.</span><span class="sxs-lookup"><span data-stu-id="44ebc-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="44ebc-193">Klicken Sie auf die Schaltfläche über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="44ebc-194">Bewegen Sie im Seitenprüfung Browserfenster den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="44ebc-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Zeigen auf Elemente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="44ebc-196">Klicken Sie einmal in den Abschnitt div. Content-Wrapper, und bewegen Sie den Mauszeiger auf das Fenster **Stile** .</span><span class="sxs-lookup"><span data-stu-id="44ebc-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="44ebc-197">Deaktivieren Sie das Kontrollkästchen für die Background-Color-Eigenschaft unter der. Featured. Content-Wrapper-Klasse, und aktivieren Sie das Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Hintergrundfarbe löschen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="44ebc-199">Beachten Sie, dass die Änderungs Vorschau sofort im Seitenprüfung Browserfenster angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="44ebc-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="44ebc-200">Aktivieren Sie das Kontrollkästchen erneut, doppelklicken Sie auf den Eigenschafts Wert, und ändern Sie ihn in `red`.</span><span class="sxs-lookup"><span data-stu-id="44ebc-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="44ebc-201">Die Änderung wird sofort angezeigt:</span><span class="sxs-lookup"><span data-stu-id="44ebc-201">The change shows immediately:</span></span>

![Rote Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="44ebc-203">Das Fenster **Stile** vereinfacht das Testen und die Vorschau von CSS-Änderungen, bevor Sie die Änderungen im Stylesheet selbst übertragen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="44ebc-204">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="44ebc-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="44ebc-205">Diese Funktion erfordert die Version 1,3 von Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="44ebc-205">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="44ebc-206">Mit der Funktion "Automatische CSS-Synchronisierung" können Sie eine CSS-Datei direkt bearbeiten und die Änderungen sofort im Seitenprüfung Browser anzeigen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="44ebc-207">Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="44ebc-208">Bewegen Sie im Seitenprüfung Browser den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="44ebc-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="44ebc-209">Klicken Sie einmal, um dieses Element auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-209">Click once to select this element.</span></span>

<span data-ttu-id="44ebc-210">Im Fenster **Stile** werden alle CSS-Regeln für dieses Element angezeigt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-210">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="44ebc-211">Scrollen Sie nach unten, um die. Featured. Content-Wrapper-Klassenauswahl zu suchen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="44ebc-212">Klicken Sie auf ". Featured. Content-Wrapper".</span><span class="sxs-lookup"><span data-stu-id="44ebc-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="44ebc-213">Seitenprüfung öffnet die CSS-Datei, mit der dieser Stil (Site. CSS) definiert wird, und hebt den entsprechenden CSS-Stil hervor.</span><span class="sxs-lookup"><span data-stu-id="44ebc-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS-Datei](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="44ebc-215">Ändern Sie nun den Wert für `background-color` in "rot".</span><span class="sxs-lookup"><span data-stu-id="44ebc-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="44ebc-216">Die Änderung wird sofort im Seitenprüfung Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-216">The change appears immediately in the Page Inspector browser.</span></span>

![Seitenprüfung Browser](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="44ebc-218">Verwenden der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="44ebc-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="44ebc-219">Als Nächstes erfahren Sie, wie Sie mit Seitenprüfung das CSS für markierten Text in der Standardanwendung schnell finden und ändern können.</span><span class="sxs-lookup"><span data-stu-id="44ebc-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="44ebc-220">In diesem Beispiel haben Sie festgelegt, dass Sie die blaue Hervorhebung nicht wünschen und Sie in eine andere Farbe ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="44ebc-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="44ebc-221">Klicken Sie auf die Schaltfläche **Suchen** .</span><span class="sxs-lookup"><span data-stu-id="44ebc-221">Click the **Inspect** button.</span></span>

![Element überprüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="44ebc-223">Bewegen Sie im Seitenprüfung Browserfenster den Mauszeiger über den markierten Text "Videos, Tutorials und Beispiele", sodass die CSS-Bezeichnung "Mark" angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="44ebc-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Zeigen auf das Markierungs Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="44ebc-225">Klicken Sie auf den Text, um ihn auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-225">Click the text to select it.</span></span> <span data-ttu-id="44ebc-226">Die entsprechende CSS-Markierung wird unten im Fenster " **Stile** " angezeigt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Markierungs Auswahl im Fenster "Stile"](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="44ebc-228">Klicken Sie auf die Markierungs Auswahl.</span><span class="sxs-lookup"><span data-stu-id="44ebc-228">Click the mark selector.</span></span> <span data-ttu-id="44ebc-229">Dadurch wird die Datei " *Site. CSS* " für die Webanwendung geöffnet.</span><span class="sxs-lookup"><span data-stu-id="44ebc-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="44ebc-230">Klicken Sie auf die Registerkarte Site. CSS, und das entsprechende CSS für den Selektor wird hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="44ebc-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Markierungs Auswahl im Stylesheet](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="44ebc-232">Wählen Sie die Zeile mit der Eigenschaft background-color aus, und entfernen Sie Sie.</span><span class="sxs-lookup"><span data-stu-id="44ebc-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="44ebc-233">Nun verwenden Sie die neue Visual Studio 2012-CSS-Farbauswahl, um eine neue Farbe für die Eigenschaft "Hintergrundfarbe **Markieren** " auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="44ebc-234">Verwenden der CSS-Farbauswahl in Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="44ebc-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="44ebc-235">Der CSS-Editor in Visual Studio 2012 verfügt über eine Farbauswahl, mit der Farben leicht ausgewählt und eingefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="44ebc-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="44ebc-236">Es verfügt über eine einfache Farbleiste und eine "Popdown"-Auswahl, die eine präzisere Steuerung bietet.</span><span class="sxs-lookup"><span data-stu-id="44ebc-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="44ebc-237">Die Farbauswahl enthält eine Standardpalette von Farben, unterstützt Standardfarben, Hashcodes, RGB-, RGBA-, HSL-und HSLA-Farben und verwaltet eine Liste der Farben, die Sie zuletzt im Dokument verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="44ebc-238">Geben Sie in der Zeile, in der die Background-Color-Eigenschaft war, "BC" ein, und drücken Sie den Pfeil nach unten.</span><span class="sxs-lookup"><span data-stu-id="44ebc-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="44ebc-239">Wenn Sie das erste Zeichen jedes Worts in eine durch Bindestriche getrennte Eigenschaft wie "Background-Color" eingeben, filtert IntelliSense die Liste so, dass nur die Eigenschaften angezeigt werden, die mit den folgenden Eigenschaften identisch sind:</span><span class="sxs-lookup"><span data-stu-id="44ebc-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Gefilterte IntelliSense-Werte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="44ebc-241">Geben Sie nun einen Doppelpunkt ein.</span><span class="sxs-lookup"><span data-stu-id="44ebc-241">Now type a colon.</span></span> <span data-ttu-id="44ebc-242">Wenn Sie dies tun, wird der vollständige Eigenschaften Name für die Hintergrundfarbe eingefügt.</span><span class="sxs-lookup"><span data-stu-id="44ebc-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="44ebc-243">Geben Sie **#** oder **RGB (** ) ein, und die Farbauswahl Leiste wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="44ebc-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Die CSS-Farbauswahl Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="44ebc-245">Um zu sehen, wie die Farbauswahl Leiste funktioniert, klicken Sie mit dem Mauszeiger auf die Farben, oder drücken Sie die nach-unten-Taste, und verwenden Sie dann die Pfeiltasten nach links und nach rechts, um die Farben zu durch</span><span class="sxs-lookup"><span data-stu-id="44ebc-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="44ebc-246">Wenn Sie eine Farbe aufrufen, wird der entsprechende Wert für die Eigenschaft background-color in der Vorschau angezeigt:</span><span class="sxs-lookup"><span data-stu-id="44ebc-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![Vorschau der Eigenschaft "Background-Color" in der Vorschau](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="44ebc-248">An diesem Punkt können Sie die EINGABETASTE drücken, um den Wert auszuwählen, und dann ein Semikolon (;) zum Vervollständigen des CSS-Eintrags.</span><span class="sxs-lookup"><span data-stu-id="44ebc-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="44ebc-249">Fahren Sie mit dem nächsten Abschnitt fort, damit Sie sehen können, wie das Popup Fenster für die Farbauswahl funktioniert.</span><span class="sxs-lookup"><span data-stu-id="44ebc-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="44ebc-250">Verwenden des Popups für die Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="44ebc-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="44ebc-251">Wenn die Farbleiste nicht die genaue Farbe hat, nach der Sie suchen, können Sie das Popup Fenster für die Farbauswahl verwenden.</span><span class="sxs-lookup"><span data-stu-id="44ebc-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="44ebc-252">Um es zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie auf der Tastatur einmal oder zweimal den Pfeil nach unten.</span><span class="sxs-lookup"><span data-stu-id="44ebc-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS-Farbauswahl-Popup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="44ebc-254">Klicken Sie auf eine Farbe aus der vertikalen Leiste auf der rechten Seite.</span><span class="sxs-lookup"><span data-stu-id="44ebc-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="44ebc-255">Dies zeigt einen Farbverlauf für diese Farbe im Hauptfenster.</span><span class="sxs-lookup"><span data-stu-id="44ebc-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="44ebc-256">Wählen Sie eine Farbe direkt aus der vertikalen Leiste aus, indem Sie die EINGABETASTE drücken, oder klicken Sie auf einen beliebigen Punkt im Hauptfenster, um eine höhere Genauigkeit auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="44ebc-257">Wenn auf dem Computerbildschirm eine Farbe vorhanden ist, die Sie verwenden möchten (Sie muss sich nicht in der Visual Studio-Benutzeroberfläche befinden), können Sie Ihren Wert mit dem Tool "Pipette" unten rechts erfassen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="44ebc-258">Sie können auch die Deckkraft einer Farbe ändern, indem Sie den Schieberegler am unteren Rand der Farbauswahl verschieben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="44ebc-259">Dadurch werden Farbwerte in RGBA-Werte geändert, da das RGBA-Format eine Deckkraft darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="44ebc-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="44ebc-260">Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie dann ein Semikolon ein, um den Hintergrund Farb Eintrag in der Datei " *Site. CSS* " abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="44ebc-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="44ebc-261">Die Seitenprüfung Update Leiste</span><span class="sxs-lookup"><span data-stu-id="44ebc-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="44ebc-262">Seitenprüfung erkennt sofort die Änderung an der Datei " *Site. CSS* " (oder in einer beliebigen Datei in der Anwendung) und zeigt eine Warnung in einer Update Leiste an.</span><span class="sxs-lookup"><span data-stu-id="44ebc-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Update Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="44ebc-264">Um alle Dateien zu speichern und den Seitenprüfung Browser zu aktualisieren, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste.</span><span class="sxs-lookup"><span data-stu-id="44ebc-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="44ebc-265">Die Änderung in der Hervorhebungs Farbe wird im Browser angezeigt:</span><span class="sxs-lookup"><span data-stu-id="44ebc-265">The change in the highlight color appears in the browser:</span></span>

![Hervorhebungs Farbe geändert](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="44ebc-267">Beachten Sie, dass Sie den Seitenprüfung Browser direkt in der Visual Studio-Umgebung aktualisiert haben.</span><span class="sxs-lookup"><span data-stu-id="44ebc-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="44ebc-268">Wenn Sie Seitenprüfung anstelle eines externen Browsers verwenden, können Sie im Editor bleiben, wenn Sie Ihre Webanwendungen entwickeln.</span><span class="sxs-lookup"><span data-stu-id="44ebc-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="44ebc-269">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="44ebc-269">See Also</span></span>

<span data-ttu-id="44ebc-270">[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9-Video)</span><span class="sxs-lookup"><span data-stu-id="44ebc-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="44ebc-271">[Seitenprüfung-Fehlermeldungen](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="44ebc-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
