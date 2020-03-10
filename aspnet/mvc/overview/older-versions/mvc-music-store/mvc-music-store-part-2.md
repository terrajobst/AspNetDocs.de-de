---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Teil 2: Controller | Microsoft-Dokumentation'
author: jongalloway
description: In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden. In Teil 2 werden Controller behandelt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451191"
---
# <a name="part-2-controllers"></a><span data-ttu-id="13713-104">Teil 2: Controller</span><span class="sxs-lookup"><span data-stu-id="13713-104">Part 2: Controllers</span></span>

<span data-ttu-id="13713-105">von [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="13713-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="13713-106">Der MVC Music Store ist eine Lernprogramm Anwendung, die Schritt für Schritt erläutert, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="13713-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="13713-107">Der MVC Music Store ist eine einfache Beispiel Speicher Implementierung, die Musikalben online verkauft und grundlegende Funktionen für Website Verwaltung, Benutzeranmeldung und Warenkorb implementiert.</span><span class="sxs-lookup"><span data-stu-id="13713-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="13713-108">In dieser tutorialreihe werden alle Schritte erläutert, die zum Erstellen der ASP.NET MVC Music Store-Beispielanwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="13713-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="13713-109">In Teil 2 werden Controller behandelt.</span><span class="sxs-lookup"><span data-stu-id="13713-109">Part 2 covers Controllers.</span></span>

<span data-ttu-id="13713-110">Bei herkömmlichen Webframe Works werden eingehende URLs normalerweise Dateien auf dem Datenträger zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="13713-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="13713-111">Beispiel: eine Anforderung für eine URL wie "/Products.aspx" oder "/Products.php" wird möglicherweise von der Datei "Products. aspx" oder "Products. php" verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="13713-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="13713-112">Webbasierte MVC-Frameworks ordnen URLs auf etwas andere Weise dem Servercode zu.</span><span class="sxs-lookup"><span data-stu-id="13713-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="13713-113">Anstatt eingehende URLs zu Dateien zuzuordnen, ordnen Sie stattdessen URLs Methoden für Klassen zu.</span><span class="sxs-lookup"><span data-stu-id="13713-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="13713-114">Diese Klassen werden als "controller" bezeichnet und sind dafür verantwortlich, eingehende HTTP-Anforderungen zu verarbeiten, Benutzereingaben zu verarbeiten, Daten abzurufen und zu speichern und die Antwort zu bestimmen, die an den Client zurückgesendet werden soll (HTML anzeigen, Datei herunterladen, zu einer anderen umleiten). URL usw.).</span><span class="sxs-lookup"><span data-stu-id="13713-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="13713-115">Hinzufügen eines HomeController</span><span class="sxs-lookup"><span data-stu-id="13713-115">Adding a HomeController</span></span>

<span data-ttu-id="13713-116">Wir beginnen mit unserer MVC Music Store-Anwendung, indem wir eine Controller Klasse hinzufügen, die URLs zur Startseite unserer Website verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="13713-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="13713-117">Befolgen Sie die Standard Benennungs Konventionen von ASP.NET MVC, und nennen Sie es HomeController.</span><span class="sxs-lookup"><span data-stu-id="13713-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="13713-118">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner "Controllers", und wählen Sie "hinzufügen" und dann "Controller..." aus. s</span><span class="sxs-lookup"><span data-stu-id="13713-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="13713-119">Dadurch wird das Dialogfeld "Controller hinzufügen" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13713-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="13713-120">Nennen Sie den Controller "HomeController", und klicken Sie auf die Schaltfläche hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="13713-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="13713-121">Dadurch wird eine neue Datei erstellt, HomeController.cs mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="13713-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="13713-122">Um so einfach wie möglich zu beginnen, ersetzen wir die Index-Methode durch eine einfache Methode, die nur eine Zeichenfolge zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="13713-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="13713-123">Wir nehmen zwei Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="13713-123">We'll make two changes:</span></span>

- <span data-ttu-id="13713-124">Ändern Sie die Methode so, dass anstelle eines Aktions Ergebnisses eine Zeichenfolge zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="13713-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="13713-125">Ändern der Return-Anweisung, sodass "Hello from Home" zurückgegeben wird</span><span class="sxs-lookup"><span data-stu-id="13713-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="13713-126">Die-Methode sollte nun wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="13713-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="13713-127">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="13713-127">Running the Application</span></span>

<span data-ttu-id="13713-128">Nun führen wir die Website aus.</span><span class="sxs-lookup"><span data-stu-id="13713-128">Now let's run the site.</span></span> <span data-ttu-id="13713-129">Wir können unseren Web-Server starten und die Website mit einem der folgenden Schritte ausprobieren:</span><span class="sxs-lookup"><span data-stu-id="13713-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="13713-130">Auswählen des Menü Elements Debuggen ⇨ Debuggen starten</span><span class="sxs-lookup"><span data-stu-id="13713-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="13713-131">Klicken Sie in der Symbolleiste auf die grüne Pfeil Schaltfläche ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="13713-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="13713-132">Verwenden Sie die Tastenkombination F5.</span><span class="sxs-lookup"><span data-stu-id="13713-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="13713-133">Mit einem der oben aufgeführten Schritte wird das Projekt kompiliert und dann die ASP.NET Development Server, die in Visual Web Developer integriert ist, gestartet.</span><span class="sxs-lookup"><span data-stu-id="13713-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="13713-134">In der unteren Ecke des Bildschirms wird eine Benachrichtigung angezeigt, die anzeigt, dass die ASP.NET Development Server gestartet wurde, und zeigt die Portnummer an, unter der Sie ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="13713-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="13713-135">Visual Web Developer öffnet dann automatisch ein Browserfenster, dessen URL auf den Webserver zeigt.</span><span class="sxs-lookup"><span data-stu-id="13713-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="13713-136">Auf diese Weise können wir die Webanwendung schnell ausprobieren:</span><span class="sxs-lookup"><span data-stu-id="13713-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="13713-137">Das war ziemlich schnell – wir haben eine neue Website erstellt, eine Funktion mit drei Zeilen hinzugefügt, und wir haben Text in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="13713-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="13713-138">Nicht in der Abwehr von Raketen Wissenschaften, aber es ist ein Start.</span><span class="sxs-lookup"><span data-stu-id="13713-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="13713-139">*Hinweis: Visual Web Developer enthält die ASP.NET Development Server, die Ihre Website auf einer zufälligen kostenlosen "Port"-Nummer ausführen wird. Im obigen Screenshot wird die Website unter `http://localhost:26641/`ausgeführt. Daher wird Port 26641 verwendet. Die Portnummer wird unterschiedlich angezeigt. Wenn wir über URLs wie/Store/Browse in diesem Tutorial sprechen, erfolgt die Portnummer. Wenn Sie die Portnummer 26641 aufrufen, bedeutet das Navigieren zu/Store/Browse, zu `http://localhost:26641/Store/Browse`zu navigieren.*</span><span class="sxs-lookup"><span data-stu-id="13713-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="13713-140">Hinzufügen eines StoreController</span><span class="sxs-lookup"><span data-stu-id="13713-140">Adding a StoreController</span></span>

<span data-ttu-id="13713-141">Wir haben einen einfachen HomeController hinzugefügt, der die Startseite der Website implementiert.</span><span class="sxs-lookup"><span data-stu-id="13713-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="13713-142">Nun fügen wir einen weiteren Controller hinzu, mit dem wir die Browser Funktionalität unseres Musik Stores implementieren.</span><span class="sxs-lookup"><span data-stu-id="13713-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="13713-143">Der Store-Controller unterstützt drei Szenarien:</span><span class="sxs-lookup"><span data-stu-id="13713-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="13713-144">Eine Listenseite der Musikgenres in unserem Music Store</span><span class="sxs-lookup"><span data-stu-id="13713-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="13713-145">Eine Seite zum Durchsuchen, auf der alle Musikalben eines bestimmten Genres aufgelistet sind.</span><span class="sxs-lookup"><span data-stu-id="13713-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="13713-146">Eine Detailseite, auf der Informationen zu einem bestimmten Musikalbum angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="13713-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="13713-147">Wir beginnen mit dem Hinzufügen einer neuen StoreController-Klasse.</span><span class="sxs-lookup"><span data-stu-id="13713-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="13713-148">Wenn Sie dies noch nicht getan haben, beenden Sie die Ausführung der Anwendung, indem Sie entweder den Browser schließen oder das Menü Element Debuggen ⇨ beenden auswählen.</span><span class="sxs-lookup"><span data-stu-id="13713-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="13713-149">Fügen Sie nun einen neuen StoreController hinzu.</span><span class="sxs-lookup"><span data-stu-id="13713-149">Now add a new StoreController.</span></span> <span data-ttu-id="13713-150">Wie bei HomeController wird dies durchgeführt, indem Sie in der Projektmappen-Explorer mit der rechten Maustaste auf den Ordner "Controllers" klicken und das Menü Element Add-&gt;Controller auswählen.</span><span class="sxs-lookup"><span data-stu-id="13713-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="13713-151">Unser neuer StoreController verfügt bereits über eine "index"-Methode.</span><span class="sxs-lookup"><span data-stu-id="13713-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="13713-152">Wir verwenden diese "index"-Methode, um unsere Auflistungs Seite zu implementieren, die alle Genres in unserem Musikspeicher auflistet.</span><span class="sxs-lookup"><span data-stu-id="13713-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="13713-153">Wir fügen außerdem zwei zusätzliche Methoden hinzu, um die beiden anderen Szenarien zu implementieren, die von StoreController behandelt werden sollen: Durchsuchen und Details.</span><span class="sxs-lookup"><span data-stu-id="13713-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="13713-154">Diese Methoden (Index, Browse und Details) im Controller werden als "Controller Aktionen" bezeichnet, und wie Sie bereits mit der HomeController. Index ()-Aktionsmethode gesehen haben, besteht ihre Aufgabe darin, auf URL-Anforderungen zu reagieren und (im allgemeinen) den Inhalt zu ermitteln. sollte an den Browser oder den Benutzer zurückgesendet werden, der die URL aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="13713-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="13713-155">Wir starten unsere StoreController-Implementierung, indem wir die theindex ()-Methode ändern, um die Zeichenfolge "Hello from Store. Index ()" zurückzugeben, und wir fügen ähnliche Methoden für "Browse ()" und "Details ()" hinzu:</span><span class="sxs-lookup"><span data-stu-id="13713-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="13713-156">Führen Sie das Projekt erneut aus, und Durchsuchen Sie die folgenden URLs:</span><span class="sxs-lookup"><span data-stu-id="13713-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="13713-157">/Store</span><span class="sxs-lookup"><span data-stu-id="13713-157">/Store</span></span>
- <span data-ttu-id="13713-158">/Store/Browse</span><span class="sxs-lookup"><span data-stu-id="13713-158">/Store/Browse</span></span>
- <span data-ttu-id="13713-159">/Store/Details</span><span class="sxs-lookup"><span data-stu-id="13713-159">/Store/Details</span></span>

<span data-ttu-id="13713-160">Durch den Zugriff auf diese URLs werden die Aktionsmethoden in unserem Controller aufgerufen und Zeichen folgen Antworten zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="13713-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="13713-161">Das ist großartig, aber es handelt sich hierbei nur um konstante Zeichen folgen.</span><span class="sxs-lookup"><span data-stu-id="13713-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="13713-162">Wir machen Sie dynamisch, sodass Sie Informationen aus der URL übernehmen und in der Seiten Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="13713-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="13713-163">Zuerst ändern wir die Methode zum Durchsuchen, um einen QueryString-Wert aus der URL abzurufen.</span><span class="sxs-lookup"><span data-stu-id="13713-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="13713-164">Zu diesem Zweck können Sie der Aktionsmethode einen "Genre"-Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="13713-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="13713-165">Wenn wir dies tun, übergibt MVC automatisch alle QueryString-oder Form Post-Parameter mit dem Namen "Genre" an unsere Aktionsmethode, wenn Sie aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="13713-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="13713-166">*Hinweis: die Benutzereingaben werden mithilfe der hilfsprogrammmethode HttpUtility. HtmlEncode bereinigt. Dadurch wird verhindert, dass Benutzer JavaScript in unserer Ansicht mit einem Link wie/Store/Browse einfügen? Genre =&lt;Skript&gt;Window. Location = 'http://hackersite.com'&lt;/Script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="13713-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="13713-167">Navigieren wir nun zu/Store/Browse? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="13713-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="13713-168">Als nächstes ändern wir die Aktion "Details", um einen Eingabeparameter mit dem Namen "ID" anzuzeigen und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="13713-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="13713-169">Anders als bei der vorherigen Methode wird der ID-Wert nicht als QueryString-Parameter einbettet.</span><span class="sxs-lookup"><span data-stu-id="13713-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="13713-170">Stattdessen Betten wir ihn direkt in die URL ein.</span><span class="sxs-lookup"><span data-stu-id="13713-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="13713-171">Beispiel:/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="13713-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="13713-172">Mit ASP.NET MVC können wir dies problemlos tun, ohne dass Sie etwas konfigurieren müssen.</span><span class="sxs-lookup"><span data-stu-id="13713-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="13713-173">Die Standard Routing Konvention von ASP.NET MVC besteht darin, das Segment einer URL nach dem Namen der Aktionsmethode als Parameter mit dem Namen "ID" zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="13713-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="13713-174">Wenn die Aktionsmethode einen Parameter mit dem Namen ID hat, übergibt ASP.NET MVC das URL-Segment automatisch als Parameter an Sie.</span><span class="sxs-lookup"><span data-stu-id="13713-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="13713-175">Führen Sie die Anwendung aus, und navigieren Sie zu/Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="13713-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="13713-176">Wir wollen uns nun weitermachen, was wir bisher getan haben:</span><span class="sxs-lookup"><span data-stu-id="13713-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="13713-177">Wir haben in Visual Web Developer ein neues ASP.NET MVC-Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="13713-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="13713-178">Wir haben die grundlegende Ordnerstruktur einer ASP.NET MVC-Anwendung erläutert.</span><span class="sxs-lookup"><span data-stu-id="13713-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="13713-179">Wir haben gelernt, wie unsere Website mithilfe der ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="13713-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="13713-180">Wir haben zwei Controller Klassen erstellt: einen HomeController und einen StoreController.</span><span class="sxs-lookup"><span data-stu-id="13713-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="13713-181">Wir haben unseren Controllern Aktionsmethoden hinzugefügt, die auf URL-Anforderungen reagieren und Text an den Browser zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="13713-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="13713-182">[Zurück](mvc-music-store-part-1.md)
> [Weiter](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="13713-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
