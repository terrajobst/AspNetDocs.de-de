---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Erstellen von Hilfeseiten für ASP.net-Web-API-ASP.NET 4. x
author: MikeWasson
description: In diesem Tutorial mit Code wird gezeigt, wie Sie Hilfeseiten für ASP.net-Web-API in ASP.NET 4. x erstellen.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448617"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="bb3d4-103">Erstellen von Hilfeseiten für ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="bb3d4-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="bb3d4-104">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bb3d4-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bb3d4-105">In diesem Tutorial mit Code wird gezeigt, wie Sie Hilfeseiten für ASP.net-Web-API in ASP.NET 4. x erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="bb3d4-106">Wenn Sie eine Web-API erstellen, ist es oft hilfreich, eine Hilfeseite zu erstellen, damit andere Entwickler wissen, wie Sie Ihre API aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="bb3d4-107">Sie könnten die gesamte Dokumentation manuell erstellen, aber es ist besser, so weit wie möglich automatisch zu generieren.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="bb3d4-108">Um diese Aufgabe zu vereinfachen, stellt ASP.net-Web-API eine Bibliothek für die automatische Erstellung von Hilfeseiten zur Laufzeit bereit.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="bb3d4-109">Erstellen von API-Hilfeseiten</span><span class="sxs-lookup"><span data-stu-id="bb3d4-109">Creating API Help Pages</span></span>

<span data-ttu-id="bb3d4-110">Installieren Sie [ASP.net and Web Tools 2012,2-Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="bb3d4-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="bb3d4-111">Mit diesem Update werden Hilfeseiten in die Web-API-Projektvorlage integriert.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="bb3d4-112">Erstellen Sie als nächstes ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="bb3d4-113">Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="bb3d4-114">Die Vorlage erstellt außerdem die API-Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-114">The template also creates the API help pages.</span></span> <span data-ttu-id="bb3d4-115">Alle Code Dateien für die Hilfeseite werden im Ordner "Bereiche" des Projekts platziert.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="bb3d4-116">Wenn Sie die Anwendung ausführen, enthält die Startseite einen Link zur API-Hilfeseite.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="bb3d4-117">Auf der Startseite lautet der relative Pfad/Help.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="bb3d4-118">Über diesen Link gelangen Sie zu einer API-Zusammenfassungs Seite.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="bb3d4-119">Die MVC-Ansicht für diese Seite wird in Bereiche/helppage/views/Help/Index. cshtml definiert.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="bb3d4-120">Sie können diese Seite bearbeiten, um Layout, Einführung, Titel, Stile usw. zu ändern.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="bb3d4-121">Der Hauptteil der Seite ist eine Tabelle mit APIs, gruppiert nach Controller.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="bb3d4-122">Die Tabelleneinträge werden mithilfe der **iapiexplorer** -Schnittstelle dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="bb3d4-123">(Ich werde später noch mehr über diese Schnittstelle sprechen.) Wenn Sie einen neuen API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="bb3d4-124">In der Spalte "API" sind die HTTP-Methode und der relative URI aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="bb3d4-125">Die Spalte "Beschreibung" enthält die Dokumentation für jede API.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="bb3d4-126">Zunächst ist die Dokumentation nur Platzhalter Text.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="bb3d4-127">Im nächsten Abschnitt zeige ich Ihnen, wie Sie Dokumentationen aus XML-Kommentaren hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="bb3d4-128">Jede API verfügt über einen Link zu einer Seite mit detaillierteren Informationen, wie z. b. Anforderungs-und Antwort Texte.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="bb3d4-129">Hinzufügen von Hilfeseiten zu einem vorhandenen Projekt</span><span class="sxs-lookup"><span data-stu-id="bb3d4-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="bb3d4-130">Mit dem nuget-Paket-Manager können Sie einem vorhandenen Web-API-Projekt Hilfeseiten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="bb3d4-131">Diese Option ist nützlich, wenn Sie von einer anderen Projektvorlage als der Vorlage "Web-API" starten.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="bb3d4-132">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="bb3d4-133">Geben Sie im Fenster der [Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) einen der folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="bb3d4-134">Für eine **C#** -Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="bb3d4-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="bb3d4-135">Für eine **Visual Basic** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="bb3d4-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="bb3d4-136">Es gibt zwei Pakete, eine für C# und eine für Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="bb3d4-137">Stellen Sie sicher, dass Sie die Anwendung verwenden, die dem Projekt entspricht.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="bb3d4-138">Mit diesem Befehl werden die erforderlichen Assemblys installiert, und die MVC-Ansichten für die Hilfeseiten (im Ordner "Bereiche/helppage") werden hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="bb3d4-139">Sie müssen der Hilfeseite manuell einen Link hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="bb3d4-140">Der URI ist/Help.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-140">The URI is /Help.</span></span> <span data-ttu-id="bb3d4-141">Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="bb3d4-142">Stellen Sie außerdem sicher, dass Sie die Bereiche registrieren.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-142">Also, make sure to register areas.</span></span> <span data-ttu-id="bb3d4-143">Fügen Sie in der Datei "Global. asax" den folgenden Code in die **Anwendungs\_Start** -Methode ein, sofern diese nicht bereits vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="bb3d4-144">API-Dokumentation hinzufügen</span><span class="sxs-lookup"><span data-stu-id="bb3d4-144">Adding API Documentation</span></span>

<span data-ttu-id="bb3d4-145">Standardmäßig verfügen die Hilfeseiten über Platzhalter Zeichenfolgen für die Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="bb3d4-146">Sie können [XML-Dokumentations Kommentare](https://msdn.microsoft.com/library/b2s063f7.aspx) verwenden, um die Dokumentation zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="bb3d4-147">Um dieses Feature zu aktivieren, öffnen Sie die Datei Bereiche/helppage/App\_Start/helppageconfig. cs, und heben Sie die Auskommentierung der folgenden Zeile auf:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="bb3d4-148">Aktivieren Sie nun die XML-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-148">Now enable XML documentation.</span></span> <span data-ttu-id="bb3d4-149">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**aus.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="bb3d4-150">Wählen Sie die Seite **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="bb3d4-151">Überprüfen Sie unter **Ausgabe**die **XML-Dokumentations Datei**.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="bb3d4-152">Geben Sie im Bearbeitungsfeld "App\_Data/XmlDocument. xml" ein.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="bb3d4-153">Öffnen Sie als nächstes den Code für den `ValuesController` API-Controller, der in/Controllers/ValuesController.cs. definiert ist.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="bb3d4-154">Fügen Sie den Controller Methoden einige Dokumentations Kommentare hinzu.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="bb3d4-155">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="bb3d4-156">Tipp: Wenn Sie die Einfügemarke in der Zeile oberhalb der Methode positionieren und drei Schrägstriche eingeben, fügt Visual Studio automatisch die XML-Elemente ein.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="bb3d4-157">Anschließend können Sie die Leerzeichen ausfüllen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="bb3d4-158">Erstellen Sie jetzt die Anwendung, und führen Sie Sie erneut aus, und navigieren Sie zu den Hilfeseiten.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="bb3d4-159">Die Dokumentations Zeichenfolgen sollten in der API-Tabelle angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="bb3d4-160">Die Hilfeseite liest die Zeichen folgen aus der XML-Datei zur Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="bb3d4-161">(Stellen Sie beim Bereitstellen der Anwendung sicher, dass Sie die XML-Datei bereitstellen.)</span><span class="sxs-lookup"><span data-stu-id="bb3d4-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="bb3d4-162">Im Hintergrund</span><span class="sxs-lookup"><span data-stu-id="bb3d4-162">Under the Hood</span></span>

<span data-ttu-id="bb3d4-163">Die Hilfeseiten basieren auf der **apiexplorer** -Klasse, die Teil des Web-API-Frameworks ist.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="bb3d4-164">Die **apiexplorer** -Klasse stellt das Rohmaterial zum Erstellen einer Hilfeseite bereit.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="bb3d4-165">**Apiexplorer** enthält für jede API eine **apideabo-App** , die die API beschreibt.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="bb3d4-166">Zu diesem Zweck wird eine "API" als Kombination aus HTTP-Methode und relativer URI definiert.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="bb3d4-167">Hier sind z. b. einige unterschiedliche APIs:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="bb3d4-168">Get/API/Products</span><span class="sxs-lookup"><span data-stu-id="bb3d4-168">GET /api/Products</span></span>
- <span data-ttu-id="bb3d4-169">Get/API/Products/{ID}</span><span class="sxs-lookup"><span data-stu-id="bb3d4-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="bb3d4-170">Nach/API/Products</span><span class="sxs-lookup"><span data-stu-id="bb3d4-170">POST /api/Products</span></span>

<span data-ttu-id="bb3d4-171">Wenn eine Controller Aktion mehrere HTTP-Methoden unterstützt, behandelt der **apiexplorer** jede Methode als eine eindeutige API.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="bb3d4-172">Um eine API aus dem **apiexplorer**auszublenden, fügen Sie der Aktion das Attribut **apiexplorersettings** hinzu, und legen Sie *ignoreapi* auf true fest.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="bb3d4-173">Sie können dieses Attribut auch dem Controller hinzufügen, um den gesamten Controller auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="bb3d4-174">Die apiexplorer-Klasse ruft Dokumentations Zeichenfolgen von der **idocumentationprovider** -Schnittstelle ab.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="bb3d4-175">Wie Sie bereits gesehen haben, bietet die Hilfeseiten Bibliothek einen **idocumentationprovider** , der die Dokumentation aus XML-Dokumentations Zeichenfolgen abruft.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="bb3d4-176">Der Code befindet sich in/Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="bb3d4-177">Sie können die Dokumentation aus einer anderen Quelle abrufen, indem Sie Ihren eigenen **idocumentationprovider**schreiben.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="bb3d4-178">Um es zu verknüpfen, wenden Sie die in **helppageconfigurationextensions** definierte **setdocumentationprovider** -Erweiterungsmethode an.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="bb3d4-179">**Apiexplorer** ruft automatisch die **idocumentationprovider** -Schnittstelle auf, um Dokumentations Zeichenfolgen für jede API zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="bb3d4-180">Sie speichert Sie in der- **Dokumentations** Eigenschaft der Objekte **apide-** und **apiparameterdescription** .</span><span class="sxs-lookup"><span data-stu-id="bb3d4-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb3d4-181">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="bb3d4-181">Next Steps</span></span>

<span data-ttu-id="bb3d4-182">Sie sind nicht auf die hier gezeigten Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="bb3d4-183">Tatsächlich ist **apiexplorer** nicht auf das Erstellen von Hilfeseiten beschränkt.</span><span class="sxs-lookup"><span data-stu-id="bb3d4-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="bb3d4-184">Yao Huang Lin hat einige tolle Blogbeiträge verfasst, um Ihnen die folgenden Vorteile zu bringen:</span><span class="sxs-lookup"><span data-stu-id="bb3d4-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="bb3d4-185">Hinzufügen eines einfachen Test Clients zur ASP.net-Web-API Hilfeseite</span><span class="sxs-lookup"><span data-stu-id="bb3d4-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="bb3d4-186">Erstellen ASP.net-Web-API Hilfeseite in selbstgeh osteten Diensten</span><span class="sxs-lookup"><span data-stu-id="bb3d4-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="bb3d4-187">Entwurfszeit Generierung der Hilfeseite (oder des Clients) für ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="bb3d4-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="bb3d4-188">Erweiterte Anpassungen der Hilfeseite</span><span class="sxs-lookup"><span data-stu-id="bb3d4-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
