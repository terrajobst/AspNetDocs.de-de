---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Teil 3: Ansichten und ViewModels | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 3 werden Sichten und ViewModels behandelt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451131"
---
# <a name="part-3-views-and-viewmodels"></a><span data-ttu-id="6c18c-104">Teil 3: Ansichten und ViewModels</span><span class="sxs-lookup"><span data-stu-id="6c18c-104">Part 3: Views and ViewModels</span></span>

<span data-ttu-id="6c18c-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6c18c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6c18c-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6c18c-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="6c18c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6c18c-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6c18c-109">In Teil 3 werden Sichten und ViewModels behandelt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-109">Part 3 covers Views and ViewModels.</span></span>

<span data-ttu-id="6c18c-110">Bisher haben wir soeben Zeichen folgen aus Controller Aktionen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="6c18c-111">Das ist eine gute Möglichkeit, um eine Vorstellung davon zu erhalten, wie Controller funktionieren, aber es ist nicht so, dass Sie eine echte Webanwendung erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="6c18c-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="6c18c-112">Wir wünschen eine bessere Möglichkeit, HTML-Code zu Browsern zu generieren, die unsere Website besuchen – eine, bei der wir Vorlagen Dateien verwenden können, um den HTML-Inhalt des sendebacks leichter anzupassen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="6c18c-113">Das ist genau das, was die Ansichten tun.</span><span class="sxs-lookup"><span data-stu-id="6c18c-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="6c18c-114">Hinzufügen einer Ansichts Vorlage</span><span class="sxs-lookup"><span data-stu-id="6c18c-114">Adding a View template</span></span>

<span data-ttu-id="6c18c-115">Um eine Ansichts Vorlage zu verwenden, ändern wir die Methode HomeController Index so, dass ein Aktions Ergebnis zurückgegeben wird, und lassen Sie View () wie folgt zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="6c18c-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="6c18c-116">Die obige Änderung gibt an, dass anstelle einer Zeichenfolge eine "Ansicht" verwendet werden soll, um ein Ergebnis zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="6c18c-117">Nun fügen wir dem Projekt eine passende Ansichts Vorlage hinzu.</span><span class="sxs-lookup"><span data-stu-id="6c18c-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="6c18c-118">Zu diesem Zweck positionieren wir den Textcursor innerhalb der Index Aktionsmethode, klicken mit der rechten Maustaste und wählen "Ansicht hinzufügen" aus.</span><span class="sxs-lookup"><span data-stu-id="6c18c-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="6c18c-119">Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6c18c-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="6c18c-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6c18c-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="6c18c-121">Mit dem Dialogfeld "Ansicht hinzufügen" können Sie schnell und einfach Ansichts Vorlagen Dateien generieren.</span><span class="sxs-lookup"><span data-stu-id="6c18c-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="6c18c-122">Standardmäßig wird im Dialogfeld "Ansicht hinzufügen" der Name der zu erstellenden Ansichts Vorlage vorab aufgefüllt, sodass Sie mit der Aktionsmethode übereinstimmt, von der Sie verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="6c18c-123">Da wir das Kontextmenü "Ansicht hinzufügen" innerhalb der Index ()-Aktionsmethode des HomeController verwendet haben, enthält das obige Dialogfeld "Add View" (Ansicht hinzufügen) "index", da der Ansichts Name standardmäßig bereits aufgefüllt ist.</span><span class="sxs-lookup"><span data-stu-id="6c18c-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="6c18c-124">Wir müssen die Optionen in diesem Dialogfeld nicht ändern, und klicken Sie auf die Schaltfläche hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="6c18c-125">Wenn Sie auf die Schaltfläche Hinzufügen klicken, erstellt Visual Web Developer eine neue Index. cshtml-Ansichts Vorlage für uns im Verzeichnis "\views\home" und erstellt den Ordner, wenn er nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="6c18c-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="6c18c-126">Der Name und der Ordner Speicherort der Datei "index. cshtml" sind wichtig und folgen den standardmäßigen ASP.NET-MVC-Benennungs Konventionen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="6c18c-127">Der Verzeichnisname \views\home entspricht dem Controller mit dem Namen HomeController.</span><span class="sxs-lookup"><span data-stu-id="6c18c-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="6c18c-128">Der Ansichts Vorlagen Name, Index, entspricht der Controller Aktionsmethode, die die Ansicht anzeigt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="6c18c-129">ASP.NET MVC ermöglicht uns, den Namen oder den Speicherort einer Ansichts Vorlage nicht explizit anzugeben, wenn diese Benennungs Konvention verwendet wird, um eine Ansicht zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="6c18c-130">Standardmäßig wird die Ansichts Vorlage \views\home\index.cshtml gerenden, wenn wir Code wie unten im HomeController schreiben:</span><span class="sxs-lookup"><span data-stu-id="6c18c-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="6c18c-131">Visual Web Developer hat die Ansichts Vorlage "index. cshtml" erstellt und geöffnet, nachdem wir im Dialogfeld "Ansicht hinzufügen" auf die Schaltfläche "hinzufügen" geklickt haben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="6c18c-132">Der Inhalt der Datei "index. cshtml" ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="6c18c-133">In dieser Ansicht wird der Razor-Syntax verwendet, der präziser ist als die Web Forms Ansichts-Engine, die in ASP.net Web Forms und früheren Versionen von ASP.NET MVC verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="6c18c-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="6c18c-134">Die Web Forms Ansichts-Engine ist weiterhin in ASP.NET MVC 3 verfügbar, aber viele Entwickler haben festgestellt, dass die Razor-Ansichts-Engine ASP.net die MVC-Entwicklung wirklich gut passt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="6c18c-135">In den ersten drei Zeilen wird der Seitentitel mithilfe von "viewbag. Title" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="6c18c-136">Wir sehen uns an, wie dies in Kürze ausführlicher funktioniert, aber zuerst aktualisieren wir den Text Überschriften Text und zeigen die Seite an.</span><span class="sxs-lookup"><span data-stu-id="6c18c-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="6c18c-137">Aktualisieren Sie das Tag &lt;H2&gt; auf "Dies ist die Startseite", wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="6c18c-138">Das Ausführen der Anwendung zeigt, dass der neue Text auf der Startseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="6c18c-139">Verwenden eines Layouts für allgemeine Standort Elemente</span><span class="sxs-lookup"><span data-stu-id="6c18c-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="6c18c-140">Die meisten Websites verfügen über Inhalte, die von vielen Seiten gemeinsam genutzt werden: Navigation, Fußzeilen, Logo Bilder, Stylesheet-Verweise usw. Die Razor-Ansichts-Engine vereinfacht diese Verwaltung mithilfe einer Seite namens \_Layout. cshtml, die automatisch für uns im Ordner/views/Shared erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="6c18c-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="6c18c-141">Doppelklicken Sie auf diesen Ordner, um die unten gezeigten Inhalte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="6c18c-142">Der Inhalt unserer einzelnen Ansichten wird mit dem Befehl "@RenderBody()" angezeigt, und alle allgemeinen Inhalte, die außerhalb von angezeigt werden sollen, können dem \_Layout. cshtml-Markup hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="6c18c-143">Wir möchten, dass unser MVC Music Store über einen allgemeinen Header mit Links zu unserer Startseite und dem Store-Bereich auf allen Seiten der Website verfügt. daher fügen wir ihn der Vorlage direkt oberhalb dieser @RenderBody()-Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="6c18c-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="6c18c-144">Aktualisieren des Stylesheets</span><span class="sxs-lookup"><span data-stu-id="6c18c-144">Updating the StyleSheet</span></span>

<span data-ttu-id="6c18c-145">Die leere Projektvorlage enthält eine sehr optimierte CSS-Datei, die nur Stile zum Anzeigen von Validierungs Meldungen enthält.</span><span class="sxs-lookup"><span data-stu-id="6c18c-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="6c18c-146">Unser Designer hat einige zusätzliche CSS-und Bilddateien bereitgestellt, um das Erscheinungsbild für unsere Website zu definieren, sodass wir diese jetzt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="6c18c-147">Die aktualisierten CSS-Dateien und-Images sind im Inhaltsverzeichnis von MvcMusicStore-Assets. zip enthalten, das unter [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store)verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="6c18c-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="6c18c-148">Wir wählen beide in Windows-Explorer aus und legen Sie in Visual Web Developer im Inhalts Ordner unserer Projekt Mappe ab, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="6c18c-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="6c18c-149">Sie werden aufgefordert zu bestätigen, dass Sie die vorhandene Datei "Site. CSS" überschreiben möchten.</span><span class="sxs-lookup"><span data-stu-id="6c18c-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="6c18c-150">Klicken Sie auf Ja.</span><span class="sxs-lookup"><span data-stu-id="6c18c-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="6c18c-151">Der Inhalts Ordner der Anwendung wird nun wie folgt angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6c18c-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="6c18c-152">Führen Sie nun die Anwendung aus, und sehen Sie sich an, wie sich unsere Änderungen auf der Startseite ansehen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="6c18c-153">Sehen wir uns an, was sich geändert hat: die Index Aktionsmethode von HomeController hat gefunden und die Vorlage \views\home\index.cshtmlview angezeigt, auch wenn der Code "Return View ()" genannt wird, weil unsere Ansichts Vorlage der Standard Benennungs Konvention folgte.</span><span class="sxs-lookup"><span data-stu-id="6c18c-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="6c18c-154">Auf der Startseite wird eine einfache Willkommensnachricht angezeigt, die in der Ansichts Vorlage "\views\home\index.cshtml" definiert ist.</span><span class="sxs-lookup"><span data-stu-id="6c18c-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="6c18c-155">Auf der Startseite wird unsere Vorlage "\_Layout. cshtml" verwendet, sodass die Willkommensnachricht im HTML-Layout der Standard Website enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="6c18c-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="6c18c-156">Verwenden eines Modells zum Übergeben von Informationen an unsere Ansicht</span><span class="sxs-lookup"><span data-stu-id="6c18c-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="6c18c-157">Eine Ansichts Vorlage, in der nur hart codiertes HTML angezeigt wird, macht keine sehr interessante Website.</span><span class="sxs-lookup"><span data-stu-id="6c18c-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="6c18c-158">Zum Erstellen einer dynamischen Website möchten wir stattdessen Informationen von unseren Controller Aktionen an unsere Ansichts Vorlagen übergeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="6c18c-159">Im Model-View-Controller-Muster bezieht sich der Begriff Model auf Objekte, die die Daten in der Anwendung darstellen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="6c18c-160">Modell Objekte entsprechen häufig Tabellen in der Datenbank, jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="6c18c-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="6c18c-161">Controller Aktionsmethoden, die ein "action result" zurückgeben, können ein Modell Objekt an die Ansicht übergeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="6c18c-162">Dies ermöglicht es einem Controller, alle Informationen, die erforderlich sind, um eine Antwort zu generieren, ordnungsgemäß zu verpacken und diese Informationen dann an eine Ansichts Vorlage zu übergeben, die zum Generieren der entsprechenden HTML-Antwort verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6c18c-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="6c18c-163">Dies ist am einfachsten zu verstehen, wenn Sie in Aktion angezeigt wird. beginnen wir also.</span><span class="sxs-lookup"><span data-stu-id="6c18c-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="6c18c-164">Zunächst erstellen wir einige Modellklassen, die Genres und Alben in unserem Geschäft darstellen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="6c18c-165">Beginnen wir mit der Erstellung einer Genre Klasse.</span><span class="sxs-lookup"><span data-stu-id="6c18c-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="6c18c-166">Klicken Sie mit der rechten Maustaste auf den Ordner "Models" in Ihrem Projekt, wählen Sie die Option "Klasse hinzufügen" aus, und geben Sie der Datei den Namen "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="6c18c-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="6c18c-167">Fügen Sie dann der erstellten Klasse eine Eigenschaft für den öffentlichen Zeichen folgen Namen hinzu:</span><span class="sxs-lookup"><span data-stu-id="6c18c-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="6c18c-168">*Hinweis: für den Fall, dass Sie sich Fragen, verwendet die {Get; Set;} C#-Notation die automatisch implementierte Eigenschaft "Properties". Dies bietet uns die Vorteile einer Eigenschaft, ohne dass wir ein dahinter liegendes Feld deklarieren müssen.*</span><span class="sxs-lookup"><span data-stu-id="6c18c-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="6c18c-169">Führen Sie als nächstes die gleichen Schritte aus, um eine Album-Klasse (mit dem Namen Album.cs) zu erstellen, die einen Titel und eine Genre-Eigenschaft aufweist:</span><span class="sxs-lookup"><span data-stu-id="6c18c-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="6c18c-170">Nun können wir StoreController so ändern, dass Ansichten verwendet werden, die dynamische Informationen aus unserem Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="6c18c-171">Wenn zurzeit zu Demonstrationszwecken unsere Alben basierend auf der Anforderungs-ID benannt wurden, könnten diese Informationen wie in der folgenden Ansicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="6c18c-172">Zunächst ändern wir die Aktion "Store-Details", sodass die Informationen für ein einzelnes Album angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="6c18c-173">Fügen Sie am Anfang der **storecontrollers** -Klasse eine using-Anweisung hinzu, um den mvcmusicstore. Models-Namespace einzuschließen. Daher müssen Sie nicht jedes Mal, wenn Sie die Album-Klasse verwenden möchten, mvcmusicstore. Models. Album eingeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="6c18c-174">Der Abschnitt "using-Direktiven" dieser Klasse sollte nun wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="6c18c-175">Als nächstes aktualisieren wir die Aktion "Details Controller", sodass anstelle einer Zeichenfolge ein "action result" zurückgegeben wird, wie dies bei der Index Methode von HomeController der Fall war.</span><span class="sxs-lookup"><span data-stu-id="6c18c-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="6c18c-176">Nun können wir die Logik ändern, um ein Album Objekt an die Ansicht zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="6c18c-177">Später in diesem Tutorial werden die Daten aus einer Datenbank abgerufen – aber zurzeit werden wir "Dummydaten" verwenden, um zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="6c18c-178">*Hinweis: Wenn Sie mit nicht vertraut C#sind, können Sie davon ausgehen, dass die Verwendung von "var" bedeutet, dass die Album-Variable spät gebunden ist. Das ist nicht korrekt – der C# Compiler verwendet den Typrückschluss basierend auf dem, was wir der Variablen zuweisen, um zu bestimmen, ob das Album vom Typ "Album" ist, und die lokale Album Variable als einen albummtyp zu kompilieren, sodass wir die Kompilierzeit Überprüfung und die Visual Studio Code-Editor-Unterstützung erhalten.*</span><span class="sxs-lookup"><span data-stu-id="6c18c-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="6c18c-179">Nun erstellen wir eine Ansichts Vorlage, in der das Album zum Generieren einer HTML-Antwort verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="6c18c-180">Bevor wir dies tun, müssen wir das Projekt so erstellen, dass das Dialogfeld "Ansicht hinzufügen" die neu erstellte Album-Klasse kennt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="6c18c-181">Sie können das Projekt erstellen, indem Sie das Menü Element Debuggen ⇨ Build mvcmusicstore auswählen (um das Projekt mit der Tastenkombination STRG + UMSCHALT + B zu erstellen).</span><span class="sxs-lookup"><span data-stu-id="6c18c-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="6c18c-182">Nachdem wir nun unsere unterstützenden Klassen eingerichtet haben, sind wir bereit, unsere Ansichts Vorlage zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="6c18c-183">Klicken Sie mit der rechten Maustaste in die Detail Methode, und wählen Sie "Ansicht hinzufügen" aus. über das Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="6c18c-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="6c18c-184">Wir erstellen eine neue Ansichts Vorlage wie zuvor mit dem HomeController.</span><span class="sxs-lookup"><span data-stu-id="6c18c-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="6c18c-185">Da wir Sie aus dem StoreController erstellen, wird Sie standardmäßig in der Datei "\views\store\index.cshtml" generiert.</span><span class="sxs-lookup"><span data-stu-id="6c18c-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="6c18c-186">Anders als zuvor aktivieren wir das Kontrollkästchen "eine stark typisierte Ansicht erstellen".</span><span class="sxs-lookup"><span data-stu-id="6c18c-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="6c18c-187">Dann wählen wir unsere "Album"-Klasse in der Dropdown Liste "Datenklasse anzeigen" aus.</span><span class="sxs-lookup"><span data-stu-id="6c18c-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="6c18c-188">Dadurch wird das Dialogfeld "Ansicht hinzufügen" dazu geführt, eine Ansichts Vorlage zu erstellen, die erwartet, dass ein Album-Objekt zur Verwendung an Sie übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="6c18c-189">Wenn wir auf die Schaltfläche "Add" (hinzufügen) klicken, wird die Ansichts Vorlage "\views\store\details.cshtml" erstellt, die den folgenden Code enthält.</span><span class="sxs-lookup"><span data-stu-id="6c18c-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="6c18c-190">Beachten Sie die erste Zeile, die angibt, dass diese Ansicht stark typisiert ist.</span><span class="sxs-lookup"><span data-stu-id="6c18c-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="6c18c-191">Die Razor-Ansichts-Engine erkennt, dass ihr ein Album-Objekt weitergereicht wurde, sodass wir leicht auf Modell Eigenschaften zugreifen können und sogar den Vorteil von IntelliSense im Visual Web Developer-Editor haben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="6c18c-192">Aktualisieren Sie das Tag &lt;H2&gt;, sodass die Title-Eigenschaft des Albums angezeigt wird, indem Sie diese Zeile so ändern, dass Sie wie folgt aussieht.</span><span class="sxs-lookup"><span data-stu-id="6c18c-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="6c18c-193">Beachten Sie, dass IntelliSense ausgelöst wird, wenn Sie den Zeitraum nach dem @Model-Schlüsselwort eingeben, das die Eigenschaften und Methoden anzeigt, die die Album-Klasse unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="6c18c-194">Nun führen wir das Projekt erneut aus und besuchen die URL/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="6c18c-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="6c18c-195">Wir sehen uns die Details eines Albums wie unten dargestellt an.</span><span class="sxs-lookup"><span data-stu-id="6c18c-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="6c18c-196">Nun erstellen wir ein ähnliches Update für die Aktionsmethode "Store durchsuchen".</span><span class="sxs-lookup"><span data-stu-id="6c18c-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="6c18c-197">Aktualisieren Sie die Methode so, dass Sie ein Aktions Ergebnis zurückgibt, und ändern Sie die Methoden Logik, damit ein neues Genre Objekt erstellt und an die Ansicht zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="6c18c-198">Klicken Sie mit der rechten Maustaste in die Browse-Methode, und wählen Sie "Ansicht hinzufügen..." Fügen Sie im Kontextmenü eine Ansicht hinzu, die stark typisiert ist, fügen Sie der Genre Klasse einen stark typisierten hinzu.</span><span class="sxs-lookup"><span data-stu-id="6c18c-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="6c18c-199">Aktualisieren Sie das Element &lt;H2&gt; im Ansichts Code (in/views/Store/Browse.cshtml), um die Genre Informationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="6c18c-200">Führen Sie nun das Projekt erneut aus, und navigieren Sie zu/Store/Browse? Genre = Disco-URL.</span><span class="sxs-lookup"><span data-stu-id="6c18c-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="6c18c-201">Wir sehen, dass die Seite zum Durchsuchen wie unten dargestellt angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="6c18c-202">Zum Schluss erstellen wir ein etwas komplexeres Update der Aktionsmethode und der Ansicht des **Speicher Indexes** , um eine Liste aller Genres in unserem Geschäft anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="6c18c-203">Dazu verwenden wir eine Liste der Genres als Modell Objekt und nicht nur ein einzelnes Genre.</span><span class="sxs-lookup"><span data-stu-id="6c18c-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="6c18c-204">Klicken Sie mit der rechten Maustaste in die Aktionsmethode Store-Index, und wählen Sie Ansicht hinzufügen wie zuvor aus, wählen Sie Genre als Modell Klasse aus, und klicken Sie auf die Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="6c18c-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="6c18c-205">Zunächst ändern wir die @model Deklaration, um anzugeben, dass die Sicht mehrere Genre Objekte erwartet, anstatt nur eine zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="6c18c-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="6c18c-206">Ändern Sie die erste/Store/Index.cshtml Zeile wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6c18c-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="6c18c-207">Dadurch wird der Razor-Ansichts-Engine mitgeteilt, dass Sie mit einem Modell Objekt arbeitet, das mehrere Genre Objekte enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="6c18c-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="6c18c-208">Wir verwenden ein IEnumerable-&lt;Genre&gt; anstatt eine Liste&lt;Genre&gt; da es allgemeinerer ist. Dadurch können wir unseren Modelltyp später in einen beliebigen Objekttyp ändern, der die IEnumerable-Schnittstelle unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="6c18c-209">Als nächstes durchlaufen wir die Genre Objekte im Modell, wie im nachfolgenden Code der abgeschlossenen Ansicht gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="6c18c-210">Beachten Sie, dass bei der Eingabe dieses Codes vollständige IntelliSense-Unterstützung vorhanden ist, wenn wir "@Model" eingeben.</span><span class="sxs-lookup"><span data-stu-id="6c18c-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="6c18c-211">Wir sehen alle Methoden und Eigenschaften, die von einem IEnumerable-Element vom Typ Genre unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="6c18c-212">In unserer "foreach"-Schleife weiß Visual Web Developer, dass jedes Element vom Typ Genre ist, sodass für jeden Genre-Typ IntelliSense angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="6c18c-213">Im nächsten Schritt untersuchte die Gerüstbau Funktion das Genre Objekt und stellte fest, dass jede eine Name-Eigenschaft hat. Sie führt Sie durch und schreibt Sie. Außerdem werden die Links "Bearbeiten", "Details" und "Löschen" für die einzelnen Elemente generiert.</span><span class="sxs-lookup"><span data-stu-id="6c18c-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="6c18c-214">Wir nutzen dies später in unserem Store Manager, aber vorerst möchten wir stattdessen eine einfache Liste verwenden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="6c18c-215">Wenn wir die Anwendung ausführen und zu/Store navigieren, sehen wir, dass sowohl die Anzahl als auch die Liste der Genres angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="6c18c-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="6c18c-216">Hinzufügen von Links zwischen Seiten</span><span class="sxs-lookup"><span data-stu-id="6c18c-216">Adding Links between pages</span></span>

<span data-ttu-id="6c18c-217">Unsere/Store-URL, die Genres auflistet, listet derzeit die Genre Namen einfach als nur-Text auf.</span><span class="sxs-lookup"><span data-stu-id="6c18c-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="6c18c-218">Wir ändern dies, sodass anstelle von nur-Text die Genre Namen mit der entsprechenden/Store/Browse-URL verknüpft werden, damit das Klicken auf ein Musikgenre wie "Disco" zur/Store/Browse? Genre = Disco-URL navigiert.</span><span class="sxs-lookup"><span data-stu-id="6c18c-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="6c18c-219">Wir könnten die Ansichts Vorlage "\views\store\index.cshtml" aktualisieren, um diese Links mithilfe von Code wie unten auszugeben **(geben Sie diesen Link nicht ein, da wir ihn verbessern werden)** :</span><span class="sxs-lookup"><span data-stu-id="6c18c-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="6c18c-220">Dies funktioniert, kann aber später zu Problemen führen, da es auf einer hart codierten Zeichenfolge basiert.</span><span class="sxs-lookup"><span data-stu-id="6c18c-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="6c18c-221">Wenn wir beispielsweise den Controller umbenennen wollten, müssten wir den Code suchen, der nach links sucht, die aktualisiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="6c18c-222">Ein alternativer Ansatz, den wir verwenden können, ist die Verwendung einer HTML-Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="6c18c-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="6c18c-223">ASP.NET MVC enthält HTML-Hilfsmethoden, die über den Code der Ansichts Vorlage verfügbar sind, um eine Vielzahl allgemeiner Aufgaben wie diese auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="6c18c-224">Die "HTML. Action Link ()"-Hilfsmethode ist eine besonders nützliche Methode und vereinfacht das Erstellen von HTML-&lt;einer&gt; Links und kümmert sich um lästige Details wie das sicherstellen, dass URL-Pfade ordnungsgemäß URL-codiert sind.</span><span class="sxs-lookup"><span data-stu-id="6c18c-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="6c18c-225">HTML. Action Link () verfügt über mehrere verschiedene über Ladungen, die es ermöglichen, so viele Informationen anzugeben, wie Sie für Ihre Verknüpfungen benötigen.</span><span class="sxs-lookup"><span data-stu-id="6c18c-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="6c18c-226">Im einfachsten Fall geben Sie nur den Linktext und die Aktionsmethode an, zu der Sie wechseln, wenn auf dem Client auf den Link geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="6c18c-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="6c18c-227">Beispielsweise können wir eine Verknüpfung mit der "/Store/" Index ()-Methode auf der Seite "Store-Details" mit dem Linktext "Gehe zum Speicher Index" mithilfe des folgenden Aufrufes aufrufen:</span><span class="sxs-lookup"><span data-stu-id="6c18c-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="6c18c-228">*Hinweis: in diesem Fall müssen wir den Controller Namen nicht angeben, da wir einfach eine Verbindung mit einer anderen Aktion innerhalb desselben Controllers herstellen, der die aktuelle Ansicht rendert.*</span><span class="sxs-lookup"><span data-stu-id="6c18c-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="6c18c-229">Unsere Links zur Seite "Durchsuchen" müssen jedoch einen Parameter übergeben. Daher verwenden wir eine andere Überladung der HTML. Action Link-Methode, die drei Parameter annimmt:</span><span class="sxs-lookup"><span data-stu-id="6c18c-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="6c18c-230">Linktext, in dem der Genre Name angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="6c18c-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="6c18c-231">Controller Aktionsname (Durchsuchen)</span><span class="sxs-lookup"><span data-stu-id="6c18c-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="6c18c-232">Routen Parameterwerte und angeben des Namens (Genre) und des Werts (Genre Name)</span><span class="sxs-lookup"><span data-stu-id="6c18c-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="6c18c-233">Im folgenden wird erläutert, wie wir diese Links in die Speicher Index Ansicht schreiben:</span><span class="sxs-lookup"><span data-stu-id="6c18c-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="6c18c-234">Wenn wir nun das Projekt erneut ausführen und auf die/Store/-URL zugreifen, wird eine Liste mit Genres angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6c18c-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="6c18c-235">Jedes Genre ist ein Hyperlink – Wenn Sie darauf klicken, gelangen wir zur URL "/Store/Browse? Genre = *[Genre]* ".</span><span class="sxs-lookup"><span data-stu-id="6c18c-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="6c18c-236">Der HTML-Code für die Genre Liste sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="6c18c-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> <span data-ttu-id="6c18c-237">[Zurück](mvc-music-store-part-2.md)
> [Weiter](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="6c18c-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
