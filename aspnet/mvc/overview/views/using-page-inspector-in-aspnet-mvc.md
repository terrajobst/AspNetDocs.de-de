---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Verwenden von Seitenprüfung in ASP.NET MVC | Microsoft-Dokumentation
author: rick-anderson
description: Seitenprüfung in Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser. Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432453"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="5f466-104">Verwenden der Seitenprüfung in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5f466-104">Using Page Inspector in ASP.NET MVC</span></span>

<span data-ttu-id="5f466-105">von Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="5f466-105">by Tim Ammann</span></span>

> <span data-ttu-id="5f466-106">Seitenprüfung in Visual Studio 2012 ist ein Webentwicklungs Tool mit einem integrierten Browser.</span><span class="sxs-lookup"><span data-stu-id="5f466-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="5f466-107">Wählen Sie ein beliebiges Element im integrierten Browser aus, und Seitenprüfung die Quelle und das CSS des Elements sofort hervorheben.</span><span class="sxs-lookup"><span data-stu-id="5f466-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="5f466-108">Sie können eine beliebige MVC-Ansicht durchsuchen, schnell nach den Quellen des gerenderten Markups suchen und die Browser Tools direkt in der Visual Studio-Umgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f466-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="5f466-109">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="5f466-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="5f466-110">In diesem Tutorial wird gezeigt, wie Sie Überprüfungsmodus aktivieren und dann schnell Markup und CSS innerhalb des Webprojekts suchen und bearbeiten können.</span><span class="sxs-lookup"><span data-stu-id="5f466-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="5f466-111">Im Tutorial wird ein MVC-Projekt verwendet, aber Sie können auch Seitenprüfung für [Web Forms](https://go.microsoft.com/?linkid=9802001) und andere ASP.NET-Anwendungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f466-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="5f466-112">Das Tutorial enthält die folgenden Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="5f466-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="5f466-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="5f466-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="5f466-114">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="5f466-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="5f466-115">Verwenden Sie Seitenprüfung, um zu einer Ansicht zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="5f466-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="5f466-116">Aktivieren von Überprüfungsmodus</span><span class="sxs-lookup"><span data-stu-id="5f466-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="5f466-117">Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup</span><span class="sxs-lookup"><span data-stu-id="5f466-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="5f466-118">Überprüfungsmodus und das HTML-Fenster</span><span class="sxs-lookup"><span data-stu-id="5f466-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="5f466-119">Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"</span><span class="sxs-lookup"><span data-stu-id="5f466-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="5f466-120">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="5f466-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="5f466-121">Verwenden der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="5f466-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="5f466-122">Zuordnung dynamischer Seitenelemente zu JavaScript</span><span class="sxs-lookup"><span data-stu-id="5f466-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="5f466-123">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5f466-123">Prerequisites</span></span>

- <span data-ttu-id="5f466-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="5f466-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="5f466-125">Um die neueste Version von Seitenprüfung zu erhalten, verwenden Sie den [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) , um das Windows Azure SDK für .NET 2,0 zu installieren.</span><span class="sxs-lookup"><span data-stu-id="5f466-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="5f466-126">Seitenprüfung ist mit Microsoft Web Developer Tools gebündelt.</span><span class="sxs-lookup"><span data-stu-id="5f466-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="5f466-127">Die neueste Version ist 1,3.</span><span class="sxs-lookup"><span data-stu-id="5f466-127">The latest version is 1.3.</span></span> <span data-ttu-id="5f466-128">Um zu überprüfen, welche Version Sie besitzen, führen Sie Visual Studio aus, und wählen **Microsoft Visual Studio** Sie im Menü **Hilfe** die Option Info aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="5f466-129">Erstellen einer Webanwendung</span><span class="sxs-lookup"><span data-stu-id="5f466-129">Create a Web Application</span></span>

<span data-ttu-id="5f466-130">Erstellen Sie zunächst eine Webanwendung, die Sie mit Seitenprüfung verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="5f466-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="5f466-131">Wählen Sie in Visual Studio **Datei** &gt; **Neues Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="5f466-132">Erweitern Sie auf der linken **Seite C#Visualisierung** , wählen Sie **Web**aus, und wählen Sie dann **ASP.net MVC4 Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Neue ASP.NET-MVC-Anwendung](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="5f466-134">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f466-134">Click **OK**.</span></span>

<span data-ttu-id="5f466-135">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Internet Anwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="5f466-136">Belassen Sie **Razor** als Standard Ansichts-Engine.</span><span class="sxs-lookup"><span data-stu-id="5f466-136">Leave **Razor** as the default view engine.</span></span>

![Neues ASP.NET MVC-Projekt-Internet Anwendung](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="5f466-138">Die Anwendung wird in der **Quell** Ansicht geöffnet.</span><span class="sxs-lookup"><span data-stu-id="5f466-138">The application opens in **Source** view.</span></span>

![Neue ASP.NET MVC-Anwendung in der Quell Ansicht](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="5f466-140">Nachdem Sie nun über eine Anwendung verfügen, mit der Sie arbeiten können, können Sie Seitenprüfung verwenden, um Sie zu überprüfen und zu ändern.</span><span class="sxs-lookup"><span data-stu-id="5f466-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="5f466-141">Verwenden Sie Seitenprüfung, um zu einer Ansicht zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="5f466-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="5f466-142">In Visual Studio 2012 können Sie mit der rechten Maustaste auf eine beliebige Ansicht in Ihrem Projekt klicken, **in Seitenprüfung anzeigen**auswählen und Seitenprüfung die Route herausfinden und die Seite anzeigen.</span><span class="sxs-lookup"><span data-stu-id="5f466-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="5f466-143">Erweitern Sie in **Projektmappen-Explorer**den Ordner " **views** " und dann den Ordner " **Home** ".</span><span class="sxs-lookup"><span data-stu-id="5f466-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="5f466-144">Klicken Sie mit der rechten Maustaste auf die Datei index. cshtml, und wählen Sie **in Seitenprüfung anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="5f466-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Index. cshtml in Seitenprüfung anzeigen](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="5f466-146">Standardmäßig ist Seitenprüfung als Fenster auf der linken Seite der Visual Studio-Umgebung angedockt.</span><span class="sxs-lookup"><span data-stu-id="5f466-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="5f466-147">Wenn Sie es vorziehen, können Sie es an anderer Stelle andocken oder das Fenster Abdocken.</span><span class="sxs-lookup"><span data-stu-id="5f466-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="5f466-148">Siehe Gewusst [wie: anordnen und Andocken von Fenstern](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f466-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="5f466-149">Im oberen Bereich des Fensters Seitenprüfung wird die aktuelle Seite in einem Browserfenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="5f466-150">Im unteren Bereich wird die Seite im HTML-Markup angezeigt, zusammen mit einigen Registerkarten, mit denen Sie verschiedene Aspekte der Seite untersuchen können.</span><span class="sxs-lookup"><span data-stu-id="5f466-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="5f466-151">Der untere Bereich ähnelt dem [Entwicklertools F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="5f466-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![ASP.NET MVC-Anwendung in Seitenprüfung](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="5f466-153">In diesem Tutorial verwenden Sie die Registerkarten **HTML** und **Stile** , um schnell zu navigieren und Änderungen an der Anwendung vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="5f466-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="5f466-154">Enableinspection-Modus</span><span class="sxs-lookup"><span data-stu-id="5f466-154">EnableInspection Mode</span></span>

<span data-ttu-id="5f466-155">Um Seitenprüfung in Überprüfungsmodus einzufügen, klicken Sie auf die Schaltfläche über **prüfen** .</span><span class="sxs-lookup"><span data-stu-id="5f466-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="5f466-156">Wenn Sie in Überprüfungsmodus den Mauszeiger über einen Teil der gerenderten Seite halten, wird das entsprechende Quell Markup oder der Code hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="5f466-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Überprüfungsmodus umschalten](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="5f466-158">Bewegen Sie nun den Mauszeiger über verschiedene Teile der Seite in Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="5f466-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="5f466-159">Wie Sie dies tun, ändert sich der Mauszeiger in ein großes Pluszeichen, und das untergeordnete Element wird hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="5f466-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Zeigen Sie mit dem Mauszeiger auf div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="5f466-161">Wenn Sie den Mauszeiger bewegen, werden die entsprechenden Razor-Syntax in der Quelldatei von Visual Studio hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="5f466-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="5f466-162">Wenn das HTML-Element aus einer anderen Quelldatei stammt, wird die Datei automatisch von Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="5f466-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="5f466-163">In Seitenprüfung zeigt die Registerkarte **HTML** den HTML-Code an, der aus dem Razor-Syntax generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="5f466-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="5f466-164">Wenn Sie den Mauszeiger bewegen, werden die HTML-Elemente hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="5f466-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="5f466-165">Auf der Registerkarte **Stile** werden die CSS-Regeln für das-Element angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="5f466-166">Verwenden von Seitenprüfung zum vornehmen von Änderungen an Markup</span><span class="sxs-lookup"><span data-stu-id="5f466-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="5f466-167">Mit Seitenprüfung können Sie Markup suchen, dessen Speicherort möglicherweise nicht offensichtlich ist.</span><span class="sxs-lookup"><span data-stu-id="5f466-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="5f466-168">Anschließend können Sie das Markup ändern und die resultierenden Änderungen anzeigen.</span><span class="sxs-lookup"><span data-stu-id="5f466-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="5f466-169">Um dies zu sehen, klicken Sie auf über **prüfen** , und Scrollen Sie dann im Seitenprüfung Fenster zum unteren Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="5f466-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="5f466-170">Wenn Sie den Mauszeiger in den Footerbereich bewegen, öffnet Seitenprüfung die Datei \_Layout. cshtml und hebt den Abschnitt der Layoutseite hervor, die Sie ausgewählt haben.</span><span class="sxs-lookup"><span data-stu-id="5f466-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="5f466-171">Wie Sie sehen, wird der Fußzeile in der Layoutdatei und nicht in der Ansicht selbst definiert.</span><span class="sxs-lookup"><span data-stu-id="5f466-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Fußzeile](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="5f466-173">Bewegen Sie nun den Mauszeiger über die Zeile mit dem <a id="a"> </a>Urheberrechts Hinweis.</span><span class="sxs-lookup"><span data-stu-id="5f466-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="5f466-174">Auf der Seite \_Layout. cshtml wird die entsprechende Zeile hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="5f466-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Vorder-und-Fußzeile hervorgehoben](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="5f466-176">Fügen Sie am Ende der Zeile in der Datei \_Layout. cshtml Text hinzu.</span><span class="sxs-lookup"><span data-stu-id="5f466-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="5f466-177">&lt;p&gt;&amp;kopieren; @DateTime.Now.Year-My ASP.NET MVC-Anwendungs Felsen!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="5f466-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="5f466-178">Drücken Sie nun Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste, um die Ergebnisse im Seitenprüfung Browserfenster anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5f466-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="5f466-180">Sie haben vielleicht schon einmal gedacht, dass der Fußzeile in der Datei "index. cshtml" definiert ist, sich jedoch im \_"Layout. cshtml" befunden hat und Seitenprüfung ihn für Sie gefunden hat.</span><span class="sxs-lookup"><span data-stu-id="5f466-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="5f466-181">Überprüfungsmodus und das HTML-Fenster</span><span class="sxs-lookup"><span data-stu-id="5f466-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="5f466-182">Im nächsten Schritt sehen Sie sich das HTML-Fenster und die Art der Zuordnung von Elementen an.</span><span class="sxs-lookup"><span data-stu-id="5f466-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="5f466-183">Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f466-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="5f466-184">Klicken Sie auf den oberen Teil der Seite, der "Ihr Logo hier" anzeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-184">Click the top part of the page that says "Your logo here".</span></span> <span data-ttu-id="5f466-185">Sie untersuchen ein bestimmtes Element ausführlicher, sodass sich die Anzeige im Browserfenster nicht mehr ändert, wenn Sie den Mauszeiger bewegen.</span><span class="sxs-lookup"><span data-stu-id="5f466-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="5f466-186">Bewegen Sie nun den Mauszeiger auf das **HTML** -Fenster.</span><span class="sxs-lookup"><span data-stu-id="5f466-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="5f466-187">Wenn Sie den Mauszeiger bewegen, wird Seitenprüfung das Element innerhalb des **HTML** -Fensters umrissen und das entsprechende Element im Browserfenster hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="5f466-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML-Fenster](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="5f466-189">Wie zuvor öffnet Seitenprüfung die Datei "\_Layout. cshtml" für Sie auf einer temporären Registerkarte. Klicken Sie auf die Registerkarte "\_Layout. cshtml", und das entsprechende Markup wird im Abschnitt &lt;Header&gt; für Sie hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="5f466-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Hervorgehobene Markup](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="5f466-191">Anzeigen einer Vorschau von CSS-Änderungen im Fenster "Stile"</span><span class="sxs-lookup"><span data-stu-id="5f466-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="5f466-192">Im nächsten Schritt verwenden Sie das Fenster Seitenprüfung **Stile** , um eine Vorschau der Änderungen an CSS anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5f466-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="5f466-193">Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f466-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="5f466-194">Bewegen Sie im Seitenprüfung Browserfenster den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="5f466-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Zeigen Sie mit dem Mauszeiger auf div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="5f466-196">Klicken Sie einmal in den Abschnitt div. Content-Wrapper, und bewegen Sie den Mauszeiger auf das Fenster **Stile** .</span><span class="sxs-lookup"><span data-stu-id="5f466-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="5f466-197">Im Fenster **Stile** werden alle CSS-Regeln für dieses Element angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="5f466-198">Scrollen Sie nach unten, um die. Featured. Content-Wrapper-Klassenauswahl zu suchen.</span><span class="sxs-lookup"><span data-stu-id="5f466-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="5f466-199">Deaktivieren Sie nun das Kontrollkästchen für die Eigenschaft Hintergrundfarbe.</span><span class="sxs-lookup"><span data-stu-id="5f466-199">Now clear the checkbox for the background-color property.</span></span>

![Hintergrundfarbe löschen](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="5f466-201">Beachten Sie, dass die Änderungs Vorschau sofort im Seitenprüfung Browserfenster angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="5f466-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="5f466-202">Aktivieren Sie das Kontrollkästchen erneut, doppelklicken Sie auf den Eigenschafts Wert, und ändern Sie ihn in rot.</span><span class="sxs-lookup"><span data-stu-id="5f466-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="5f466-203">Die Änderung wird sofort angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5f466-203">The change shows immediately:</span></span>

![Rote Hintergrundfarbe](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="5f466-205">Das Fenster **Stile** vereinfacht das Testen und die Vorschau von CSS-Änderungen, bevor Sie die Änderungen im Stylesheet selbst übertragen.</span><span class="sxs-lookup"><span data-stu-id="5f466-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="5f466-206">Automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="5f466-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="5f466-207">Diese Funktion erfordert die Version 1,3 von Seitenprüfung.</span><span class="sxs-lookup"><span data-stu-id="5f466-207">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="5f466-208">Mit der Funktion "Automatische CSS-Synchronisierung" können Sie eine CSS-Datei direkt bearbeiten und die Änderungen sofort im Seitenprüfung Browser anzeigen.</span><span class="sxs-lookup"><span data-stu-id="5f466-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="5f466-209">Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f466-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="5f466-210">Bewegen Sie im Seitenprüfung Browser den Mauszeiger über den Abschnitt "Home Page", bis die **div. Content-Wrapper-** Bezeichnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="5f466-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="5f466-211">Klicken Sie einmal, um dieses Element auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="5f466-211">Click once to select this element.</span></span>

<span data-ttu-id="5f466-212">Im Fenster **Stile** werden alle CSS-Regeln für dieses Element angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="5f466-213">Scrollen Sie nach unten, um die. Featured. Content-Wrapper-Klassenauswahl zu suchen.</span><span class="sxs-lookup"><span data-stu-id="5f466-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="5f466-214">Klicken Sie auf ". Featured. Content-Wrapper".</span><span class="sxs-lookup"><span data-stu-id="5f466-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="5f466-215">Seitenprüfung öffnet die CSS-Datei, mit der dieser Stil (Site. CSS) definiert wird, und hebt den entsprechenden CSS-Stil hervor.</span><span class="sxs-lookup"><span data-stu-id="5f466-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="5f466-216">Ändern Sie nun den Wert für `background-color` in "rot".</span><span class="sxs-lookup"><span data-stu-id="5f466-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="5f466-217">Die Änderung wird sofort im Seitenprüfung Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="5f466-218">Verwenden der CSS-Farbauswahl</span><span class="sxs-lookup"><span data-stu-id="5f466-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="5f466-219">Der CSS-Editor in Visual Studio 2012 verfügt über eine Farbauswahl, mit der Farben leicht ausgewählt und eingefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="5f466-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="5f466-220">Die Farbauswahl enthält eine Standardpalette von Farben, unterstützt Standardfarben, Hashcodes, RGB-, RGBA-, HSL-und HSLA-Farben und verwaltet eine Liste der Farben, die Sie zuletzt im Dokument verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="5f466-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="5f466-221">Im vorherigen Abschnitt haben Sie den Wert der `background-color`-Eigenschaft geändert.</span><span class="sxs-lookup"><span data-stu-id="5f466-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="5f466-222">Um die Farbauswahl aufzurufen, platzieren Sie die Einfügemarke nach dem Eigenschaftsnamen, und geben Sie **#** oder **RGB (** ein.</span><span class="sxs-lookup"><span data-stu-id="5f466-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Die CSS-Farbauswahl Leiste](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="5f466-224">Klicken Sie auf eine Farbe, um Sie auszuwählen, oder drücken Sie die nach-unten-Taste, und verwenden Sie dann die Pfeiltasten nach links und nach rechts, um die Farben zu durch</span><span class="sxs-lookup"><span data-stu-id="5f466-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="5f466-225">Wenn Sie eine Farbe aufrufen, wird der entsprechende Hexadezimalwert in der Vorschau angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5f466-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![Vorschau der Eigenschaft "Background-Color" in der Vorschau](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="5f466-227">Wenn die Farbleiste nicht die gewünschte Farbe hat, können Sie das Popup Fenster Farbauswahl verwenden.</span><span class="sxs-lookup"><span data-stu-id="5f466-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="5f466-228">Um es zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie auf der Tastatur einmal oder zweimal den Pfeil nach unten.</span><span class="sxs-lookup"><span data-stu-id="5f466-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS-Farbauswahl-Popup](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="5f466-230">Klicken Sie auf eine Farbe aus der vertikalen Leiste auf der rechten Seite.</span><span class="sxs-lookup"><span data-stu-id="5f466-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="5f466-231">Dies zeigt einen Farbverlauf für diese Farbe im Hauptfenster.</span><span class="sxs-lookup"><span data-stu-id="5f466-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="5f466-232">Wählen Sie eine Farbe direkt aus der vertikalen Leiste aus, indem Sie die EINGABETASTE drücken, oder klicken Sie auf einen beliebigen Punkt im Hauptfenster, um eine höhere Genauigkeit auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="5f466-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="5f466-233">Wenn auf dem Computerbildschirm eine Farbe vorhanden ist, die Sie verwenden möchten (Sie muss sich nicht in der Visual Studio-Benutzeroberfläche befinden), können Sie Ihren Wert mit dem Tool "Pipette" unten rechts erfassen.</span><span class="sxs-lookup"><span data-stu-id="5f466-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="5f466-234">Sie können auch die Deckkraft einer Farbe ändern, indem Sie den Schieberegler am unteren Rand der Farbauswahl verschieben.</span><span class="sxs-lookup"><span data-stu-id="5f466-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="5f466-235">Dadurch werden Farbwerte in RGBA-Werte geändert, da das RGBA-Format eine Deckkraft darstellen kann.</span><span class="sxs-lookup"><span data-stu-id="5f466-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="5f466-236">Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie dann ein Semikolon ein, um den Hintergrund Farb Eintrag in der Datei " *Site. CSS* " abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="5f466-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="5f466-237">Die Seitenprüfung Update Leiste</span><span class="sxs-lookup"><span data-stu-id="5f466-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="5f466-238">Seitenprüfung erkennt die Änderung an der Datei " *Site. CSS* " sofort und zeigt eine Warnung in einer Update Leiste an.</span><span class="sxs-lookup"><span data-stu-id="5f466-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Update Leiste](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="5f466-240">Um alle Dateien zu speichern und den Seitenprüfung Browser zu aktualisieren, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update Leiste.</span><span class="sxs-lookup"><span data-stu-id="5f466-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="5f466-241">Die Änderung in der Hervorhebungs Farbe wird im Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5f466-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="5f466-242">Zuordnung dynamischer Seitenelemente zu JavaScript</span><span class="sxs-lookup"><span data-stu-id="5f466-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="5f466-243">In modernen Webanwendungen werden Elemente auf der Seite häufig dynamisch mit JavaScript generiert.</span><span class="sxs-lookup"><span data-stu-id="5f466-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="5f466-244">Dies bedeutet, dass kein statisches Markup (HTML oder Razor) vorhanden ist, das diesen Seitenelementen entspricht.</span><span class="sxs-lookup"><span data-stu-id="5f466-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="5f466-245">Ab Version 1,3 können Seitenprüfung Elemente, die der Seite dynamisch hinzugefügt wurden, jetzt dem entsprechenden JavaScript-Code zuordnen.</span><span class="sxs-lookup"><span data-stu-id="5f466-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="5f466-246">Um dieses Feature zu veranschaulichen, verwenden wir die [Vorlage für die Einzelseiten Anwendung (Single Page Application, Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="5f466-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5f466-247">Die Spa-Vorlage erfordert das Update [ASP.net and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .</span><span class="sxs-lookup"><span data-stu-id="5f466-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>

<span data-ttu-id="5f466-248">Wählen Sie in Visual Studio **Datei** &gt; **Neues Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="5f466-249">Erweitern Sie auf der linken **Seite C#Visualisierung** , wählen Sie **Web**aus, und wählen Sie dann **ASP.net MVC4 Webanwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="5f466-250">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f466-250">Click **OK**.</span></span>

<span data-ttu-id="5f466-251">Wählen Sie im Dialogfeld **Neues ASP.NET MVC 4-Projekt** die Option **Single-Page-Anwendung**aus.</span><span class="sxs-lookup"><span data-stu-id="5f466-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="5f466-252">Erweitern Sie in Projektmappen-Explorer den Ordner " **views** " und dann den Ordner " **Home** ".</span><span class="sxs-lookup"><span data-stu-id="5f466-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="5f466-253">Klicken Sie mit der rechten Maustaste auf die Datei index. cshtml, und wählen Sie **in Seitenprüfung anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="5f466-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="5f466-254">Das erste, was im Seitenprüfung Browser angezeigt wird, ist eine Anmeldeseite.</span><span class="sxs-lookup"><span data-stu-id="5f466-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="5f466-255">Klicken Sie auf "registrieren", und erstellen Sie einen Benutzernamen und ein Kennwort.</span><span class="sxs-lookup"><span data-stu-id="5f466-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="5f466-256">Nachdem Sie sich angemeldet haben, werden Sie von der Anwendung protokolliert, und es wird eine Aufgabenliste mit einigen Beispiel Elementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="5f466-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="5f466-257">Klicken Sie auf über **prüfen** , um Seitenprüfung in Überprüfungsmodus einzufügen.</span><span class="sxs-lookup"><span data-stu-id="5f466-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="5f466-258">Klicken Sie im Seitenprüfung Browser auf eines der to-do-Elemente.</span><span class="sxs-lookup"><span data-stu-id="5f466-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="5f466-259">Beachten Sie, dass das-Element nicht blau hervorgehoben ist, sondern mit "js" neben dem Elementnamen hervorgehoben ist.</span><span class="sxs-lookup"><span data-stu-id="5f466-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="5f466-260">Dies gibt an, dass das Element dynamisch über das Skript erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="5f466-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="5f466-261">Außerdem wird ein orangefarbener **Unterstrich auf der Register** Karte "Registerkarte" angezeigt. Dies gibt an, dass der Bereich " **aufrufsstapel** " Weitere Informationen über das Element enthält.</span><span class="sxs-lookup"><span data-stu-id="5f466-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="5f466-262">Klicken Sie auf **die Register** Karte "Registerkarte". Der Bereich " **Aufrufe** " zeigt die aufrufsstapel für den JavaScript-Befehl an, der das Element erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="5f466-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="5f466-263">Aufrufe externer Bibliotheken, z. b. jQuery, werden reduziert, sodass Sie die Aufrufe Ihres Anwendungs Skripts leicht erkennen können.</span><span class="sxs-lookup"><span data-stu-id="5f466-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="5f466-264">Um den vollständigen Stapel, einschließlich der Aufrufe externer Bibliotheken, anzuzeigen, können Sie die Knoten mit der Bezeichnung "externe Bibliotheken" erweitern:</span><span class="sxs-lookup"><span data-stu-id="5f466-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="5f466-265">Wenn Sie auf ein Element in der aufrufsstapel klicken, öffnet Visual Studio die Codedatei und hebt das entsprechende Skript hervor.</span><span class="sxs-lookup"><span data-stu-id="5f466-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="5f466-266">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="5f466-266">See Also</span></span>

<span data-ttu-id="5f466-267">Einführung [in ASP.NET MVC 4 mit Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.NET-Website)</span><span class="sxs-lookup"><span data-stu-id="5f466-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="5f466-268">[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9-Video)</span><span class="sxs-lookup"><span data-stu-id="5f466-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="5f466-269">[Seitenprüfung-Fehlermeldungen](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="5f466-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
