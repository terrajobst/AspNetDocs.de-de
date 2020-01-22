---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 80000b366203eff4b9524b7a5995832753b9eed3
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519049"
---
# <a name="adding-a-controller"></a><span data-ttu-id="a8050-102">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="a8050-102">Adding a Controller</span></span>

<span data-ttu-id="a8050-103">von [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="a8050-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="a8050-104">MVC steht für *Model-View-Controller*.</span><span class="sxs-lookup"><span data-stu-id="a8050-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="a8050-105">MVC ist ein Muster zum Entwickeln von Anwendungen, die gut entworfen, testfähig und einfach zu verwalten sind.</span><span class="sxs-lookup"><span data-stu-id="a8050-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="a8050-106">MVC-basierte Anwendungen enthalten Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a8050-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="a8050-107">**M** odels: Klassen, die die Daten der Anwendung darstellen und Validierungs Logik verwenden, um Geschäftsregeln für diese Daten zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="a8050-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="a8050-108">**V** iews: Vorlagen Dateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.</span><span class="sxs-lookup"><span data-stu-id="a8050-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="a8050-109">**C** ontroller: Klassen, die eingehende Browser Anforderungen verarbeiten, Modelldaten abrufen und dann Ansichts Vorlagen angeben, die eine Antwort an den Browser zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="a8050-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="a8050-110">Wir behandeln all diese Konzepte in dieser tutorialreihe und zeigen Ihnen, wie Sie Sie zum Erstellen einer Anwendung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="a8050-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="a8050-111">Zunächst erstellen wir eine Controller Klasse.</span><span class="sxs-lookup"><span data-stu-id="a8050-111">Let's begin by creating a controller class.</span></span> <span data-ttu-id="a8050-112">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner *Controller* , und klicken Sie dann auf **Hinzufügen**und dann auf **Controller**</span><span class="sxs-lookup"><span data-stu-id="a8050-112">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="a8050-113">Klicken Sie im Dialogfeld **Gerüst hinzufügen** auf **MVC 5 Controller-leer**, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a8050-113">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  

<span data-ttu-id="a8050-114">Benennen Sie den neuen Controller "helloworldcontroller", und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a8050-114">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Controller hinzufügen](adding-a-controller/_static/image3.png)

<span data-ttu-id="a8050-116">Beachten Sie in **Projektmappen-Explorer** , dass eine neue Datei mit dem Namen *HelloWorldController.cs* und einem neuen Ordner " *views\helloworld*" erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="a8050-116">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="a8050-117">Der Controller ist in der IDE geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a8050-117">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="a8050-118">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="a8050-118">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="a8050-119">Die Controller Methoden geben als Beispiel eine HTML-Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="a8050-119">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="a8050-120">Der Controller hat den Namen `HelloWorldController`, und die erste Methode heißt `Index`.</span><span class="sxs-lookup"><span data-stu-id="a8050-120">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="a8050-121">Wir rufen es in einem Browser auf.</span><span class="sxs-lookup"><span data-stu-id="a8050-121">Let's invoke it from a browser.</span></span> <span data-ttu-id="a8050-122">Führen Sie die Anwendung aus (drücken Sie F5 oder STRG + F5).</span><span class="sxs-lookup"><span data-stu-id="a8050-122">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="a8050-123">Fügen Sie im Browser &quot;HelloWorld-&quot; an den Pfad in der Adressleiste an.</span><span class="sxs-lookup"><span data-stu-id="a8050-123">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="a8050-124">(In der Abbildung unten ist es z. b. `http://localhost:1234/HelloWorld.`) Die Seite im Browser sieht wie der folgende Screenshot aus.</span><span class="sxs-lookup"><span data-stu-id="a8050-124">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="a8050-125">In der obigen Methode gab der Code direkt eine Zeichenfolge zurück.</span><span class="sxs-lookup"><span data-stu-id="a8050-125">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="a8050-126">Sie haben das System angewiesen, nur einige HTML-Code zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="a8050-126">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="a8050-127">ASP.NET MVC ruft in Abhängigkeit von der eingehenden URL verschiedene Controller Klassen (und verschiedene Aktionsmethoden) auf.</span><span class="sxs-lookup"><span data-stu-id="a8050-127">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="a8050-128">Die standardmäßige URL-Routing Logik, die von ASP.NET MVC verwendet wird, verwendet das folgende Format, um zu bestimmen, welcher Code aufgerufen werden soll</span><span class="sxs-lookup"><span data-stu-id="a8050-128">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="a8050-129">Sie legen das Format für das Routing in der *App\_Start/routeconfig. cs-* Datei fest.</span><span class="sxs-lookup"><span data-stu-id="a8050-129">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="a8050-130">Wenn Sie die Anwendung ausführen und keine URL-Segmente angeben, wird standardmäßig der "Home"-Controller und die Aktionsmethode "index" verwendet, die im Abschnitt "Standard" des obigen Codes angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="a8050-130">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="a8050-131">Der erste Teil der URL bestimmt die auszuführende Controller Klasse.</span><span class="sxs-lookup"><span data-stu-id="a8050-131">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="a8050-132">Daher wird */HelloWorld* der `HelloWorldController`-Klasse zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a8050-132">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="a8050-133">Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse, die ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="a8050-133">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="a8050-134">*/HelloWorld/Index* würde also bewirken, dass die `Index`-Methode der `HelloWorldController` Klasse ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a8050-134">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="a8050-135">Beachten Sie, dass wir nur zu */HelloWorld* navigieren mussten und die `Index`-Methode standardmäßig verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a8050-135">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="a8050-136">Dies liegt daran, dass eine Methode mit dem Namen `Index` die Standardmethode ist, die auf einem Controller aufgerufen wird, wenn Sie nicht explizit angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a8050-136">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="a8050-137">Der dritte Teil des URL-Segments (`Parameters`) ist für Routendaten.</span><span class="sxs-lookup"><span data-stu-id="a8050-137">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="a8050-138">Weiter unten in diesem Tutorial werden die Weiterleitungs Daten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a8050-138">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="a8050-139">Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="a8050-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="a8050-140">Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge zurück &quot;dies die Willkommens Aktionsmethode...&quot;.</span><span class="sxs-lookup"><span data-stu-id="a8050-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="a8050-141">Die MVC-Standard Zuordnung ist `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="a8050-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="a8050-142">Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="a8050-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="a8050-143">Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="a8050-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="a8050-144">Ändern Sie das Beispiel so geringfügig, dass Sie einige Parameterinformationen von der URL an den Controller übergeben können (z. b. */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="a8050-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="a8050-145">Ändern Sie die `Welcome`-Methode so, dass Sie zwei Parameter enthält, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="a8050-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="a8050-146">Beachten Sie, dass der Code C# die optionale-Parameter-Funktion verwendet, um anzugeben, dass der `numTimes`-Parameter den Standardwert 1 hat, wenn kein Wert für diesen Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="a8050-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="a8050-147">Sicherheitshinweis: der obige Code verwendet " [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) ", um die Anwendung vor böswilliger Eingabe (JavaScript) zu schützen.</span><span class="sxs-lookup"><span data-stu-id="a8050-147">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="a8050-148">Weitere Informationen finden Sie unter Gewusst [wie: schützen vor Skript Exploits in einer Webanwendung durch Anwenden der HTML-Codierung auf](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)Zeichen folgen.</span><span class="sxs-lookup"><span data-stu-id="a8050-148">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>

 <span data-ttu-id="a8050-149">Führen Sie die Anwendung aus, und navigieren Sie zur Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="a8050-149">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="a8050-150">Sie können für `name` und `numtimes` in der URL verschiedene Werte ausprobieren.</span><span class="sxs-lookup"><span data-stu-id="a8050-150">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="a8050-151">Das [ASP.NET MVC-Modell Bindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfrage Zeichenfolge in der Adressleiste den Parametern in der-Methode zu.</span><span class="sxs-lookup"><span data-stu-id="a8050-151">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="a8050-152">Im obigen Beispiel wird das URL-Segment (`Parameters`) nicht verwendet. die Parameter `name` und `numTimes` werden als [Abfrage](http://en.wikipedia.org/wiki/Query_string)Zeichenfolgen weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="a8050-152">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="a8050-153">Das Platzhalterzeichen ?</span><span class="sxs-lookup"><span data-stu-id="a8050-153">The ?</span></span> <span data-ttu-id="a8050-154">(Fragezeichen) in der obigen URL ist ein Trennzeichen, und die Abfrage Zeichenfolgen folgen.</span><span class="sxs-lookup"><span data-stu-id="a8050-154">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="a8050-155">Das Zeichen &amp; trennt Abfragezeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="a8050-155">The &amp; character separates query strings.</span></span>

<span data-ttu-id="a8050-156">Ersetzen Sie die Willkommens Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a8050-156">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="a8050-157">Führen Sie die Anwendung aus, und geben Sie die folgende URL ein: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="a8050-157">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="a8050-158">Diesmal entsprach das dritte URL-Segment dem Routen Parameter `ID.` die `Welcome` Aktionsmethode einen Parameter (`ID`) enthält, der mit der URL-Spezifikation in der `RegisterRoutes`-Methode übereinstimmte.</span><span class="sxs-lookup"><span data-stu-id="a8050-158">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="a8050-159">In ASP.NET MVC-Anwendungen ist es typischer, Parameter als Routendaten zu übergeben (wie wir mit der ID oben), als Abfrage Zeichenfolgen zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="a8050-159">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="a8050-160">Sie können auch eine Route hinzufügen, um sowohl die `name` als auch die `numtimes` in den Parametern als Routendaten in der URL zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="a8050-160">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="a8050-161">Fügen Sie in der Datei *App\_start\routeconfig.cs* die "Hello"-Route hinzu:</span><span class="sxs-lookup"><span data-stu-id="a8050-161">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="a8050-162">Führen Sie die Anwendung aus, und navigieren Sie zu `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="a8050-162">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="a8050-163">Bei vielen MVC-Anwendungen funktioniert die Standardroute problemlos.</span><span class="sxs-lookup"><span data-stu-id="a8050-163">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="a8050-164">Sie werden später in diesem Tutorial erfahren, wie Sie Daten mithilfe der Modell Bindung übergeben, und Sie müssen die Standardroute für diese nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="a8050-164">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="a8050-165">In diesen Beispielen hat der Controller den &quot;VC-&quot; Teil von MVC ausgeführt – das heißt, die Ansicht und der Controller funktionieren.</span><span class="sxs-lookup"><span data-stu-id="a8050-165">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="a8050-166">Der Controller gibt direkt HTML zurück.</span><span class="sxs-lookup"><span data-stu-id="a8050-166">The controller is returning HTML directly.</span></span> <span data-ttu-id="a8050-167">Normalerweise möchten Sie nicht, dass Controller den HTML-Code direkt zurückgeben, da dieser Code sehr umständlich ist.</span><span class="sxs-lookup"><span data-stu-id="a8050-167">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="a8050-168">Stattdessen verwenden wir in der Regel eine separate Ansichts Vorlagen Datei, um die HTML-Antwort zu generieren.</span><span class="sxs-lookup"><span data-stu-id="a8050-168">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="a8050-169">Sehen wir uns nun an, wie wir dies tun können.</span><span class="sxs-lookup"><span data-stu-id="a8050-169">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a8050-170">[Zurück](getting-started.md)
> [Weiter](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a8050-170">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
