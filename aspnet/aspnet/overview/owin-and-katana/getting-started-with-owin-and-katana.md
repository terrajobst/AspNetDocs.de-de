---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Einstieg in owin und Katana | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472443"
---
# <a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="4e537-102">Erste Schritte mit OWIN und Katana</span><span class="sxs-lookup"><span data-stu-id="4e537-102">Getting Started with OWIN and Katana</span></span>

<span data-ttu-id="4e537-103">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4e537-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4e537-104">[Open Web Interface for .net (owin)](http://owin.org/) definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="4e537-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="4e537-105">Durch die Entkopplung des Webservers von der Anwendung vereinfacht owin das Erstellen von Middleware für die .net-Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="4e537-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="4e537-106">Außerdem vereinfacht owin das Portieren von Webanwendungen auf andere Hosts&#8212;, z. b. das Self-Hosting in einem Windows-Dienst oder einem anderen Prozess.</span><span class="sxs-lookup"><span data-stu-id="4e537-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="4e537-107">Owin ist eine in der Community befindliche Spezifikation, keine Implementierung.</span><span class="sxs-lookup"><span data-stu-id="4e537-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="4e537-108">Das Katana-Projekt ist ein Satz von Open-Source-owin-Komponenten, die von Microsoft entwickelt wurden.</span><span class="sxs-lookup"><span data-stu-id="4e537-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="4e537-109">Eine allgemeine Übersicht über owin und Katana finden Sie [in der Übersicht über Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="4e537-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="4e537-110">In diesem Artikel gehe ich direkt in den Code ein, um zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="4e537-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="4e537-111">In diesem Tutorial wird [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)verwendet, Sie können jedoch auch Visual Studio 2012 verwenden.</span><span class="sxs-lookup"><span data-stu-id="4e537-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="4e537-112">Einige der Schritte unterscheiden sich in Visual Studio 2012, was ich unten beachten werde.</span><span class="sxs-lookup"><span data-stu-id="4e537-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="4e537-113">Hosten von owin in IIS</span><span class="sxs-lookup"><span data-stu-id="4e537-113">Host OWIN in IIS</span></span>

<span data-ttu-id="4e537-114">In diesem Abschnitt hosten wir owin in IIS.</span><span class="sxs-lookup"><span data-stu-id="4e537-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="4e537-115">Mit dieser Option können Sie die Flexibilität und Zusammenfassung einer owin-Pipeline zusammen mit dem ausgereiften Funktions Satz von IIS kombinieren.</span><span class="sxs-lookup"><span data-stu-id="4e537-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="4e537-116">Mit dieser Option wird die owin-Anwendung in der ASP.net Request-Pipeline ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="4e537-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="4e537-117">Erstellen Sie zunächst ein neues ASP.NET-Webanwendungs Projekt.</span><span class="sxs-lookup"><span data-stu-id="4e537-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="4e537-118">(Verwenden Sie in Visual Studio 2012 den Projekttyp ASP.net Empty Webanwendung.)</span><span class="sxs-lookup"><span data-stu-id="4e537-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="4e537-119">Wählen Sie im Dialogfeld **Neues ASP.net-Projekt** die Vorlage **leer** aus.</span><span class="sxs-lookup"><span data-stu-id="4e537-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="4e537-120">Hinzufügen von NuGet-Paketen</span><span class="sxs-lookup"><span data-stu-id="4e537-120">Add NuGet Packages</span></span>

<span data-ttu-id="4e537-121">Fügen Sie als nächstes die erforderlichen nuget-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="4e537-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="4e537-122">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="4e537-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4e537-123">Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="4e537-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="4e537-124">Startklasse hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4e537-124">Add a Startup Class</span></span>

<span data-ttu-id="4e537-125">Fügen Sie als nächstes eine owin-Startklasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="4e537-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="4e537-126">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**und dann **Neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="4e537-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="4e537-127">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **owin-Startklasse**aus.</span><span class="sxs-lookup"><span data-stu-id="4e537-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="4e537-128">Weitere Informationen zum Konfigurieren der Startup-Klasse finden Sie unter [owin-Startklassen Erkennung](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="4e537-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="4e537-129">Fügen Sie der `Startup1.Configuration`-Methode folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="4e537-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="4e537-130">Mit diesem Code wird der owin-Pipeline eine einfache Middleware hinzugefügt, die als Funktion implementiert wird, die eine **Microsoft. owin. iowincontext** -Instanz empfängt.</span><span class="sxs-lookup"><span data-stu-id="4e537-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="4e537-131">Wenn der Server eine HTTP-Anforderung empfängt, ruft die owin-Pipeline die Middleware auf.</span><span class="sxs-lookup"><span data-stu-id="4e537-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="4e537-132">Die Middleware legt den Inhaltstyp für die Antwort fest und schreibt den Antworttext.</span><span class="sxs-lookup"><span data-stu-id="4e537-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="4e537-133">Die owin-Startklassen Vorlage ist in Visual Studio 2013 verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4e537-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="4e537-134">Wenn Sie Visual Studio 2012 verwenden, fügen Sie einfach eine neue leere Klasse mit dem Namen "`Startup1`" hinzu, und fügen Sie den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="4e537-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="4e537-135">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="4e537-135">Run the Application</span></span>

<span data-ttu-id="4e537-136">Drücken Sie F5, um das Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="4e537-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="4e537-137">Visual Studio öffnet ein Browserfenster zum `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="4e537-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="4e537-138">Die Seite sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="4e537-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="4e537-139">Selbst Hosten von owin in einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="4e537-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="4e537-140">Es ist einfach, diese Anwendung in einem benutzerdefinierten Prozess von IIS-Hosting in Self-Hosting zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="4e537-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="4e537-141">Beim IIS-Hosting fungiert IIS sowohl als HTTP-Server als auch als Prozess, der den Dienst hostet.</span><span class="sxs-lookup"><span data-stu-id="4e537-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="4e537-142">Beim Self-Hosting erstellt die Anwendung den Prozess und verwendet die **HttpListener** -Klasse als HTTP-Server.</span><span class="sxs-lookup"><span data-stu-id="4e537-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="4e537-143">Erstellen Sie in Visual Studio eine neue Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="4e537-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="4e537-144">Geben Sie im Fenster der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="4e537-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="4e537-145">Fügen Sie dem Projekt aus Teil 1 dieses Tutorials eine `Startup1` Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="4e537-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="4e537-146">Sie müssen diese Klasse nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="4e537-146">You don't need to modify this class.</span></span>

<span data-ttu-id="4e537-147">Implementieren Sie die `Main`-Methode der Anwendung wie folgt.</span><span class="sxs-lookup"><span data-stu-id="4e537-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="4e537-148">Wenn Sie die Konsolenanwendung ausführen, beginnt der Server mit dem lauschen an `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="4e537-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="4e537-149">Wenn Sie in einem Webbrowser zu dieser Adresse navigieren, wird die Seite "Hello World" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4e537-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="4e537-150">Owin-Diagnose hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4e537-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="4e537-151">Das Microsoft. owin. Diagnostics-Paket enthält Middleware, die nicht behandelte Ausnahmen abfängt und eine HTML-Seite mit Fehlerdetails anzeigt.</span><span class="sxs-lookup"><span data-stu-id="4e537-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="4e537-152">Diese Seite funktioniert ähnlich wie die ASP.NET-Fehlerseite, die manchmal auch als "[gelber Bildschirm von Tod](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (Ysod) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="4e537-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="4e537-153">Wie bei Ysod ist die Katana-Fehlerseite während der Entwicklung nützlich. es wird jedoch empfohlen, Sie im Produktionsmodus zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="4e537-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="4e537-154">Um das Diagnosepaket in Ihrem Projekt zu installieren, geben Sie den folgenden Befehl im Fenster Paket-Manager-Konsole ein:</span><span class="sxs-lookup"><span data-stu-id="4e537-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="4e537-155">Ändern Sie den Code in der `Startup1.Configuration`-Methode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="4e537-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="4e537-156">Verwenden Sie jetzt STRG + F5, um die Anwendung ohne Debuggen auszuführen, sodass Visual Studio bei der Ausnahme nicht unterbricht.</span><span class="sxs-lookup"><span data-stu-id="4e537-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="4e537-157">Die Anwendung verhält sich wie zuvor, bis Sie zu `http://localhost/fail`navigieren. zu diesem Zeitpunkt löst die Anwendung die Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="4e537-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="4e537-158">Die Middleware der Fehlerseite fängt die Ausnahme ab und zeigt eine HTML-Seite mit Informationen über den Fehler an.</span><span class="sxs-lookup"><span data-stu-id="4e537-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="4e537-159">Sie können auf die Registerkarten klicken, um die Umgebungsvariablen Stapel, Abfrage Zeichenfolge, Cookies, Anforderungs Header und owin anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4e537-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="4e537-160">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4e537-160">Next Steps</span></span>

- [<span data-ttu-id="4e537-161">Erkennung der OWIN-Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="4e537-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="4e537-162">Verwenden von owin für Self-Host-ASP.net-Web-API</span><span class="sxs-lookup"><span data-stu-id="4e537-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="4e537-163">Verwenden von owin zum selbst Hosten von signalr</span><span class="sxs-lookup"><span data-stu-id="4e537-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
