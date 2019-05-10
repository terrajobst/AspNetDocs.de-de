---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Erstellen von Hilfeseiten für ASP.NET Web-API – ASP.NET 4.x
author: MikeWasson
description: Dieses Tutorial mit Code wird gezeigt, wie zum Erstellen von Hilfeseiten für ASP.NET Web-API in ASP.NET 4.x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125242"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="588a4-103">Erstellen von Hilfeseiten für ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="588a4-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="588a4-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="588a4-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="588a4-105">Dieses Tutorial mit Code wird gezeigt, wie zum Erstellen von Hilfeseiten für ASP.NET Web-API in ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="588a4-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="588a4-106">Bei der Erstellung einer Webs-API ist es oft sinnvoll, eine Seite zu erstellen, sodass andere Entwickler zum Aufrufen Ihrer API veranschaulicht werden.</span><span class="sxs-lookup"><span data-stu-id="588a4-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="588a4-107">Konnten Sie gesamte Dokumentation manuell erstellen, aber es ist besser, automatisch so weit wie möglich.</span><span class="sxs-lookup"><span data-stu-id="588a4-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="588a4-108">Um diese Aufgabe erleichtern, bietet ASP.NET Web-API für die automatische Generierung von Hilfeseiten eine Bibliothek zur Laufzeit an.</span><span class="sxs-lookup"><span data-stu-id="588a4-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="588a4-109">Erstellen von API-Hilfeseiten</span><span class="sxs-lookup"><span data-stu-id="588a4-109">Creating API Help Pages</span></span>

<span data-ttu-id="588a4-110">Installieren Sie [ASP.NET und Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="588a4-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="588a4-111">Dieses Update ist Hilfeseiten in der Web-API-Projektvorlage integriert.</span><span class="sxs-lookup"><span data-stu-id="588a4-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="588a4-112">Als Nächstes erstellen Sie ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="588a4-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="588a4-113">Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="588a4-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="588a4-114">Die Vorlage erstellt auch die API-Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="588a4-114">The template also creates the API help pages.</span></span> <span data-ttu-id="588a4-115">Alle Codedateien für die Hilfeseite befinden sich im Ordner "Areas" des Projekts.</span><span class="sxs-lookup"><span data-stu-id="588a4-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="588a4-116">Wenn Sie die Anwendung ausführen, enthält der Startseite eine Verknüpfung mit der API-Hilfeseite an.</span><span class="sxs-lookup"><span data-stu-id="588a4-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="588a4-117">Auf der Startseite ist der relative Pfad ' / help '.</span><span class="sxs-lookup"><span data-stu-id="588a4-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="588a4-118">Diesen Link gelangen Sie zu einer Seite "API-Zusammenfassung".</span><span class="sxs-lookup"><span data-stu-id="588a4-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="588a4-119">Die MVC-Ansicht für diese Seite wird im Areas/HelpPage/Views/Help/Index.cshtml definiert.</span><span class="sxs-lookup"><span data-stu-id="588a4-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="588a4-120">Sie können diese Seite, um das Layout, Einführung, Titel, Stile usw. ändern bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="588a4-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="588a4-121">Der Hauptteil der Seite ist eine Tabelle mit APIs, die vom Netzwerkcontroller gruppiert.</span><span class="sxs-lookup"><span data-stu-id="588a4-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="588a4-122">Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="588a4-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="588a4-123">(Werde ich mehr über diese Schnittstelle später eingehen.) Wenn Sie einen neuen API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="588a4-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="588a4-124">Die "API"-Spalte enthält den HTTP-Methode und des relativen URI.</span><span class="sxs-lookup"><span data-stu-id="588a4-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="588a4-125">Die Spalte "Beschreibung" enthält die Dokumentation für jede API.</span><span class="sxs-lookup"><span data-stu-id="588a4-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="588a4-126">Die Dokumentation ist zunächst nur Platzhaltertext.</span><span class="sxs-lookup"><span data-stu-id="588a4-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="588a4-127">Im nächsten Abschnitt zeige ich Ihnen wie Dokumentation aus XML-Kommentare hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="588a4-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="588a4-128">Jede API verfügt über einen Link zu einer Seite mit ausführlicheren Informationen, einschließlich Beispiel Anforderungs- und Antworttext.</span><span class="sxs-lookup"><span data-stu-id="588a4-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="588a4-129">Hilfeseiten zu einem vorhandenen Projekt hinzufügen</span><span class="sxs-lookup"><span data-stu-id="588a4-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="588a4-130">Sie können Hilfeseiten zu einem vorhandenen Web-API-Projekt mithilfe von NuGet-Paket-Manager hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="588a4-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="588a4-131">Diese Option ist nützlich, die Sie von einer anderen Vorlage als die Vorlage "Web-API" beginnen.</span><span class="sxs-lookup"><span data-stu-id="588a4-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="588a4-132">Aus der **Tools** die Option **NuGet Paket-Manager**, und wählen Sie dann **Paket-Manager Konsole**.</span><span class="sxs-lookup"><span data-stu-id="588a4-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="588a4-133">In der [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Fenster, geben Sie einen der folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="588a4-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="588a4-134">Für eine **c#** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="588a4-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="588a4-135">Für eine **Visual Basic** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="588a4-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="588a4-136">Es gibt zwei Pakete, die für C#- und 1 für Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="588a4-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="588a4-137">Stellen Sie sicher, dass das Konto verwendet, das Ihr Projekt entspricht.</span><span class="sxs-lookup"><span data-stu-id="588a4-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="588a4-138">Dieser Befehl installiert die erforderlichen Assemblys und fügt die MVC-Ansichten für die Hilfeseiten (befindet sich im Ordner "Bereiche/HelpPage").</span><span class="sxs-lookup"><span data-stu-id="588a4-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="588a4-139">Sie müssen manuell einen Link zur Hilfeseite für die hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="588a4-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="588a4-140">Der URI ist ' / help '.</span><span class="sxs-lookup"><span data-stu-id="588a4-140">The URI is /Help.</span></span> <span data-ttu-id="588a4-141">Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="588a4-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="588a4-142">Stellen Sie außerdem sicher, um Bereiche zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="588a4-142">Also, make sure to register areas.</span></span> <span data-ttu-id="588a4-143">Fügen Sie in der Datei "Global.asax" den folgenden Code, der **Anwendung\_starten** -Methode, sofern sie nicht bereits angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="588a4-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="588a4-144">Hinzufügen der API-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="588a4-144">Adding API Documentation</span></span>

<span data-ttu-id="588a4-145">Standardmäßig werden in der Hilfe Seiten sind Platzhalter-Zeichenfolgen für die Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="588a4-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="588a4-146">Sie können [XML-Dokumentationskommentare](https://msdn.microsoft.com/library/b2s063f7.aspx) der Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="588a4-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="588a4-147">Um dieses Feature zu aktivieren, öffnen Sie die Bereiche/HelpPage/App-Datei\_Start/HelpPageConfig.cs und kommentieren Sie die folgende Zeile:</span><span class="sxs-lookup"><span data-stu-id="588a4-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="588a4-148">Jetzt aktivieren Sie XML-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="588a4-148">Now enable XML documentation.</span></span> <span data-ttu-id="588a4-149">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="588a4-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="588a4-150">Wählen Sie die **erstellen** Seite.</span><span class="sxs-lookup"><span data-stu-id="588a4-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="588a4-151">Klicken Sie unter **Ausgabe**, überprüfen Sie **XML-Dokumentationsdatei**.</span><span class="sxs-lookup"><span data-stu-id="588a4-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="588a4-152">Geben Sie in das Bearbeitungsfeld "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="588a4-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="588a4-153">Öffnen Sie als Nächstes den Code für die `ValuesController` -API-Controller, der im /Controllers/ValuesController.cs definiert ist.</span><span class="sxs-lookup"><span data-stu-id="588a4-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="588a4-154">Die Controllermethoden einige Dokumentationskommentare hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="588a4-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="588a4-155">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="588a4-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="588a4-156">Tipp: Wenn Sie positionieren den Textcursor in der Zeile über der Methode, und geben Sie drei Schrägstriche, fügt Visual Studio automatisch die XML-Elemente.</span><span class="sxs-lookup"><span data-stu-id="588a4-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="588a4-157">Sie können dann in die leeren Felder ausfüllen.</span><span class="sxs-lookup"><span data-stu-id="588a4-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="588a4-158">Jetzt erstellen Sie und führen Sie die Anwendung erneut aus, und navigieren Sie zu den Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="588a4-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="588a4-159">Die Dokumentationszeichenfolgen sollte in der API-Tabelle angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="588a4-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="588a4-160">Die Hilfeseite liest die Zeichenfolgen aus der XML-Datei zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="588a4-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="588a4-161">(Wenn Sie die Anwendung bereitstellen, stellen Sie sicher, dass die XML-Datei bereitstellen.)</span><span class="sxs-lookup"><span data-stu-id="588a4-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="588a4-162">Im Hintergrund</span><span class="sxs-lookup"><span data-stu-id="588a4-162">Under the Hood</span></span>

<span data-ttu-id="588a4-163">Hilfeseiten basieren auf der die **ApiExplorer** Klasse, die Bestandteil des Web-API-Framework ist.</span><span class="sxs-lookup"><span data-stu-id="588a4-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="588a4-164">Die **ApiExplorer** Klasse stellt das Material für das Erstellen einer Hilfeseite ein.</span><span class="sxs-lookup"><span data-stu-id="588a4-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="588a4-165">Für jede API **ApiExplorer** enthält ein **ApiDescription** , die die API beschreibt.</span><span class="sxs-lookup"><span data-stu-id="588a4-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="588a4-166">Zu diesem Zweck wird eine "API" als die Kombination von HTTP-Methode und des relativen URI definiert.</span><span class="sxs-lookup"><span data-stu-id="588a4-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="588a4-167">Hier sind z. B. einige unterschiedliche APIs:</span><span class="sxs-lookup"><span data-stu-id="588a4-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="588a4-168">/Api/Products abrufen</span><span class="sxs-lookup"><span data-stu-id="588a4-168">GET /api/Products</span></span>
- <span data-ttu-id="588a4-169">GET /api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="588a4-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="588a4-170">Veröffentlichen von Produkten/api /</span><span class="sxs-lookup"><span data-stu-id="588a4-170">POST /api/Products</span></span>

<span data-ttu-id="588a4-171">Wenn eine Controlleraktion mehrere HTTP-Methoden, unterstützt die **ApiExplorer** behandelt jede Methode als distinct-API.</span><span class="sxs-lookup"><span data-stu-id="588a4-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="588a4-172">Ausblenden eine API aus dem **ApiExplorer**, Hinzufügen der **ApiExplorerSettings** -Attribut auf die Aktion und den Satz *IgnoreApi* auf "true".</span><span class="sxs-lookup"><span data-stu-id="588a4-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="588a4-173">Sie können dieses Attribut auch auf den Controller, für den gesamten Controller ausschließen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="588a4-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="588a4-174">Die ApiExplorer Klasse ruft Dokumentationszeichenfolgen aus der **IDocumentationProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="588a4-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="588a4-175">Wie Sie gesehen haben, um die Hilfeseiten-Bibliothek bietet eine **IDocumentationProvider** Dokumentation von XML-Dokumentationszeichenfolgen abruft.</span><span class="sxs-lookup"><span data-stu-id="588a4-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="588a4-176">Der Code befindet sich im /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="588a4-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="588a4-177">Erhalten Sie Dokumentation aus einer anderen Quelle durch Schreiben eigener **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="588a4-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="588a4-178">Aufrufen, um diese verknüpfen, die **SetDocumentationProvider** Erweiterungsmethode, die in definierten **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="588a4-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="588a4-179">**ApiExplorer** ruft automatisch in die **IDocumentationProvider** Schnittstelle zum Abrufen von Dokumentationszeichenfolgen für jede API.</span><span class="sxs-lookup"><span data-stu-id="588a4-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="588a4-180">Er speichert sie in der **Dokumentation** Eigenschaft der **ApiDescription** und **ApiParameterDescription** Objekte.</span><span class="sxs-lookup"><span data-stu-id="588a4-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="588a4-181">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="588a4-181">Next Steps</span></span>

<span data-ttu-id="588a4-182">Sie sind nicht auf den hier gezeigten Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="588a4-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="588a4-183">In der Tat **ApiExplorer** ist nicht zum Erstellen von Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="588a4-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="588a4-184">Yao Huang Lin verfügt über einige großartige Blogbeiträge, rufen Sie vielleicht zum Nachdenken standardmäßig geschrieben:</span><span class="sxs-lookup"><span data-stu-id="588a4-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="588a4-185">ASP.NET Web API-Hilfeseite hinzugefügt einen einfachen Test-Client</span><span class="sxs-lookup"><span data-stu-id="588a4-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="588a4-186">Vornehmen von ASP.NET Web API-Hilfeseite für selbst gehostete Dienste arbeiten</span><span class="sxs-lookup"><span data-stu-id="588a4-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="588a4-187">Generieren zur Entwurfszeit von Hilfe Seite (oder clientarbeitsauslastungen) für ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="588a4-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="588a4-188">Erweiterte Hilfeseite-Anpassungen</span><span class="sxs-lookup"><span data-stu-id="588a4-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
