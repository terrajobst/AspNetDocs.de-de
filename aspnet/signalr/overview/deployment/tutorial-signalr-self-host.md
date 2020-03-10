---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: signalr Self-Host | Microsoft-Dokumentation'
author: bradygaster
description: In diesem Tutorial wird gezeigt, wie Sie einen selbst gehosteten signalr 2-Server erstellen und mit einem JavaScript-Client eine Verbindung damit herstellen. Im Tutorial V verwendete Software Versionen...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450165"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="f7534-104">Tutorial: signalr-Self-Host</span><span class="sxs-lookup"><span data-stu-id="f7534-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="f7534-105">von [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f7534-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="f7534-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="f7534-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="f7534-107">In diesem Tutorial wird gezeigt, wie Sie einen selbst gehosteten signalr 2-Server erstellen und mit einem JavaScript-Client eine Verbindung damit herstellen.</span><span class="sxs-lookup"><span data-stu-id="f7534-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f7534-108">Im Tutorial verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="f7534-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="f7534-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f7534-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f7534-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f7534-110">.NET 4.5</span></span>
> - <span data-ttu-id="f7534-111">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="f7534-111">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="f7534-112">Verwenden von Visual Studio 2012 mit diesem Tutorial</span><span class="sxs-lookup"><span data-stu-id="f7534-112">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="f7534-113">Gehen Sie folgendermaßen vor, um Visual Studio 2012 mit diesem Tutorial zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="f7534-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="f7534-114">Aktualisieren Sie den [Paket-Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="f7534-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="f7534-115">Installieren Sie den [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7534-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="f7534-116">Suchen Sie im Webplattform-Installer nach **ASP.net and Web Tools 2013,1 für Visual Studio 2012**, und installieren Sie ihn.</span><span class="sxs-lookup"><span data-stu-id="f7534-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="f7534-117">Dadurch werden Visual Studio-Vorlagen für signalr-Klassen wie z. b. **Hub**installiert.</span><span class="sxs-lookup"><span data-stu-id="f7534-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="f7534-118">Einige Vorlagen (z. b. **owin-Startklasse**) sind nicht verfügbar. Verwenden Sie für diese stattdessen eine Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="f7534-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f7534-119">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="f7534-119">Questions and comments</span></span>
>
> <span data-ttu-id="f7534-120">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="f7534-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f7534-121">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="f7534-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="f7534-122">Übersicht</span><span class="sxs-lookup"><span data-stu-id="f7534-122">Overview</span></span>

<span data-ttu-id="f7534-123">Ein signalr-Server wird in der Regel in einer ASP.NET-Anwendung in IIS gehostet, kann aber auch selbstgeh ostet (z. b. in einer Konsolenanwendung oder einem Windows-Dienst) mithilfe der Self-Host-Bibliothek verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7534-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="f7534-124">Diese Bibliothek basiert wie alle signalr 2 auf owin ([Open Web Interface for .net](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="f7534-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="f7534-125">Owin definiert eine Abstraktion zwischen .net-Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="f7534-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f7534-126">Owin entkoppelt die Webanwendung vom Server, wodurch owin ideal für das selbst Hosting einer Webanwendung in Ihrem eigenen Prozess außerhalb von IIS geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="f7534-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="f7534-127">Gründe für das Hosting in IIS:</span><span class="sxs-lookup"><span data-stu-id="f7534-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="f7534-128">Umgebungen, in denen IIS nicht verfügbar oder erwünscht ist, z. b. eine vorhandene Serverfarm ohne IIS.</span><span class="sxs-lookup"><span data-stu-id="f7534-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="f7534-129">Der Leistungs Aufwand von IIS muss vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="f7534-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="f7534-130">Die signalr-Funktionalität muss einer vorhandenen Anwendung hinzugefügt werden, die in einem Windows-Dienst, einer Azure-workerrolle oder einem anderen Prozess ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="f7534-130">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="f7534-131">Wenn eine Lösung aus Leistungsgründen als selbstgeh ostet entwickelt wird, wird empfohlen, auch die in IIS gehostete Anwendung zu testen, um den Leistungsvorteil zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="f7534-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="f7534-132">Dieses Tutorial enthält die folgenden Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="f7534-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="f7534-133">Erstellen des Servers</span><span class="sxs-lookup"><span data-stu-id="f7534-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="f7534-134">Zugreifen auf den Server mit einem JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="f7534-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="f7534-135">Erstellen des Servers</span><span class="sxs-lookup"><span data-stu-id="f7534-135">Creating the server</span></span>

<span data-ttu-id="f7534-136">In diesem Tutorial erstellen Sie einen Server, der in einer Konsolenanwendung gehostet wird. der Server kann jedoch in jeder Art von Prozess gehostet werden, z. b. in einem Windows-Dienst oder einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="f7534-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="f7534-137">Beispielcode zum Hosting eines signalr-Servers in einem Windows-Dienst finden Sie unter [Self-Hosting von signalr in einem Windows-Dienst](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="f7534-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="f7534-138">Öffnen Sie Visual Studio 2013 mit Administratorrechten.</span><span class="sxs-lookup"><span data-stu-id="f7534-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="f7534-139">Wählen Sie **Datei**, **Neues Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="f7534-140">Wählen Sie unter dem Knoten " **Visual C#**  " im Bereich **Vorlagen** die Option **Windows** aus, und wählen Sie die Vorlage **Konsolenanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="f7534-141">Nennen Sie das neue Projekt "signalrselfhost", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f7534-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="f7534-142">Öffnen Sie die nuget-Paket-Manager-Konsole, indem Sie **Tools** > **nuget-Paket-Manager** > Paket-Manager- **Konsole**</span><span class="sxs-lookup"><span data-stu-id="f7534-142">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="f7534-143">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="f7534-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="f7534-144">Mit diesem Befehl werden die selbst Host Bibliotheken signalr 2 dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f7534-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="f7534-145">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="f7534-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="f7534-146">Mit diesem Befehl wird die Bibliothek "Microsoft. owin. cors" dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f7534-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="f7534-147">Diese Bibliothek wird für die Domänen übergreifende Unterstützung verwendet, die für Anwendungen erforderlich ist, die signalr und einen Webseiten Client in verschiedenen Domänen hosten.</span><span class="sxs-lookup"><span data-stu-id="f7534-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="f7534-148">Da Sie den signalr-Server und den WebClient auf unterschiedlichen Ports Hosting, bedeutet dies, dass die Domänen übergreifende Kommunikation zwischen diesen Komponenten aktiviert werden muss.</span><span class="sxs-lookup"><span data-stu-id="f7534-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="f7534-149">Ersetzen Sie den Inhalt von "Program.cs" durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="f7534-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="f7534-150">Der obige Code umfasst drei Klassen:</span><span class="sxs-lookup"><span data-stu-id="f7534-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="f7534-151">**Programm**, einschließlich der **Main** -Methode, die den primären Ausführungs Pfad definiert.</span><span class="sxs-lookup"><span data-stu-id="f7534-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="f7534-152">Bei dieser Methode wird eine Webanwendung vom Typ **Startup** an der angegebenen URL (`http://localhost:8080`) gestartet.</span><span class="sxs-lookup"><span data-stu-id="f7534-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="f7534-153">Wenn für den Endpunkt Sicherheit erforderlich ist, kann SSL implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7534-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="f7534-154">Weitere Informationen finden [Sie unter Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat](https://msdn.microsoft.com/library/ms733791.aspx) .</span><span class="sxs-lookup"><span data-stu-id="f7534-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="f7534-155">**Start**: die Klasse, die die Konfiguration für den signalr-Server enthält (die einzige Konfiguration, die in diesem Tutorial verwendet wird, ist der `UseCors`) und der `MapSignalR`, der Routen für beliebige Hub-Objekte im Projekt erstellt.</span><span class="sxs-lookup"><span data-stu-id="f7534-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="f7534-156">**Myhub**, die signalr-Hub-Klasse, die von der Anwendung für Clients bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f7534-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="f7534-157">Diese Klasse verfügt über eine einzige Methode, **Send**, die von Clients aufgerufen wird, um eine Nachricht an alle anderen verbundenen Clients zu senden.</span><span class="sxs-lookup"><span data-stu-id="f7534-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="f7534-158">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-158">Compile and run the application.</span></span> <span data-ttu-id="f7534-159">Die Adresse, die auf dem Server ausgeführt wird, sollte in einem Konsolenfenster angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f7534-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="f7534-160">Wenn die Ausführung mit der Ausnahme `System.Reflection.TargetInvocationException was unhandled`fehlschlägt, müssen Sie Visual Studio mit Administratorrechten neu starten.</span><span class="sxs-lookup"><span data-stu-id="f7534-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="f7534-161">Die Anwendung wird beendet, bevor mit dem nächsten Abschnitt fortgefahren wird.</span><span class="sxs-lookup"><span data-stu-id="f7534-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="f7534-162">Zugreifen auf den Server mit einem JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="f7534-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="f7534-163">In diesem Abschnitt verwenden Sie den gleichen JavaScript-Client aus dem [Tutorial zu](../getting-started/tutorial-getting-started-with-signalr.md)den ersten Schritten.</span><span class="sxs-lookup"><span data-stu-id="f7534-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="f7534-164">Wir nehmen nur eine Änderung am Client vor, d. h. die Hub-URL wird explizit definiert.</span><span class="sxs-lookup"><span data-stu-id="f7534-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="f7534-165">Bei einer selbst gehosteten Anwendung ist der Server möglicherweise nicht unbedingt mit der Verbindungs-URL identisch (aufgrund von Reverseproxys und Lasten Ausgleichs Modulen), daher muss die URL explizit definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7534-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="f7534-166">Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Hinzufügen**, **Neues Projekt**aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="f7534-167">Wählen Sie den **webknoten** aus, und wählen Sie die Vorlage **ASP.NET Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="f7534-168">Nennen Sie das Projekt "javascriptclient", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f7534-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="f7534-169">Wählen Sie die **leere** Vorlage aus, und lassen Sie die Option verbleibende Optionen deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="f7534-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="f7534-170">Wählen Sie **Projekt erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="f7534-171">Wählen Sie in der Paket-Manager-Konsole das Projekt "javascriptclient" in der Dropdown-Dropdown- **Standard Projekt** aus, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f7534-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="f7534-172">Dieser Befehl installiert die signalr-und jQuery-Bibliotheken, die Sie auf dem Client benötigen.</span><span class="sxs-lookup"><span data-stu-id="f7534-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="f7534-173">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen**, **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="f7534-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="f7534-174">Wählen Sie den **webknoten** aus, und wählen Sie HTML-Seite aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="f7534-175">Nennen Sie die Seite **default. html**.</span><span class="sxs-lookup"><span data-stu-id="f7534-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="f7534-176">Ersetzen Sie den Inhalt der neuen HTML-Seite durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="f7534-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="f7534-177">Überprüfen Sie, ob die Skript Verweise hier mit den Skripts im Ordner "Scripts" des Projekts zu vergleichen sind.</span><span class="sxs-lookup"><span data-stu-id="f7534-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="f7534-178">Der folgende Code (der im obigen Codebeispiel hervorgehoben ist) ist das Hinzufügen, das Sie an dem Client vorgenommen haben, der im Lernprogramm "erste starteschlange" verwendet wurde (zusätzlich zum Aktualisieren des Codes auf signalr, Version 2 Beta).</span><span class="sxs-lookup"><span data-stu-id="f7534-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="f7534-179">Diese Codezeile legt die basisverbindungs-URL für signalr auf dem Server explizit fest.</span><span class="sxs-lookup"><span data-stu-id="f7534-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="f7534-180">Klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Start Projekte festlegen...** aus. Aktivieren Sie das Optionsfeld **mehrere Start Projekte** , und legen Sie die **Aktion** für beide Projekte auf **Start**fest.</span><span class="sxs-lookup"><span data-stu-id="f7534-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="f7534-181">Klicken Sie mit der rechten Maustaste auf "default. html", und wählen Sie **als Start Seite festlegen**aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="f7534-182">Führen Sie die Anwendung aus.</span><span class="sxs-lookup"><span data-stu-id="f7534-182">Run the application.</span></span> <span data-ttu-id="f7534-183">Der Server und die Seite werden gestartet.</span><span class="sxs-lookup"><span data-stu-id="f7534-183">The server and page will launch.</span></span> <span data-ttu-id="f7534-184">Möglicherweise müssen Sie die Webseite erneut laden (oder im Debugger **fortsetzen** ), wenn die Seite geladen wird, bevor der Server gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="f7534-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="f7534-185">Geben Sie im Browser einen Benutzernamen an, wenn Sie dazu aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="f7534-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="f7534-186">Kopieren Sie die URL der Seite in eine andere Browser Registerkarte oder ein anderes Fenster, und geben Sie einen anderen Benutzernamen</span><span class="sxs-lookup"><span data-stu-id="f7534-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="f7534-187">Sie können Nachrichten aus einem Browserbereich an den anderen senden, wie im Lernprogramm für die ersten Schritte.</span><span class="sxs-lookup"><span data-stu-id="f7534-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
