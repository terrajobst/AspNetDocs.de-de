---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Aktivieren der Windows-Authentifizierung in Katana | Microsoft-Dokumentation
author: MikeWasson
description: 'In diesem Artikel wird gezeigt, wie Sie die Windows-Authentifizierung in Katana aktivieren. Es umfasst zwei Szenarien: die Verwendung von IIS zum Hosten von Katana und das Verwenden von HttpListener für das Self-Host-Zertifikat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500295"
---
# <a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="5e3fe-104">Aktivieren der Windows-Authentifizierung in Katana</span><span class="sxs-lookup"><span data-stu-id="5e3fe-104">Enabling Windows Authentication in Katana</span></span>

<span data-ttu-id="5e3fe-105">von [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5e3fe-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5e3fe-106">In diesem Artikel wird gezeigt, wie Sie die Windows-Authentifizierung in Katana aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="5e3fe-107">Es umfasst zwei Szenarien: die Verwendung von IIS zum Hosten von Katana und das Verwenden von HttpListener für das selbst Hosting von Katana in einem benutzerdefinierten Prozess.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="5e3fe-108">Vielen Dank an Barry dorrane, David Matson und Chris Ross zum Überprüfen dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>

<span data-ttu-id="5e3fe-109">Katana ist die Microsoft-Implementierung von [owin](http://owin.org/), dem Open Web Interface für .net.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="5e3fe-110">Eine Einführung in owin und Katana finden Sie [hier](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="5e3fe-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="5e3fe-111">Die owin-Architektur verfügt über mehrere Ebenen:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="5e3fe-112">Host: verwaltet den Prozess, in dem die owin-Pipeline ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="5e3fe-113">Server: öffnet einen Netzwerksocket und lauscht auf Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="5e3fe-114">Middleware: verarbeitet die HTTP-Anforderung und-Antwort.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="5e3fe-115">Katana verfügt zurzeit über zwei Server, von denen beide die integrierte Windows-Authentifizierung unterstützen:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="5e3fe-116">**Microsoft. owin. Host. systemWeb**.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="5e3fe-117">Verwendet IIS mit der ASP.NET-Pipeline.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="5e3fe-118">**Microsoft. owin. Host. HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="5e3fe-119">Verwendet [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e3fe-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="5e3fe-120">Bei diesem Server handelt es sich zurzeit um die Standardoption für das Selbsthosting von Katana.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="5e3fe-121">Katana stellt derzeit keine owin-Middleware für die Windows-Authentifizierung bereit, da diese Funktionalität bereits auf den Servern verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="5e3fe-122">Windows-Authentifizierung in IIS</span><span class="sxs-lookup"><span data-stu-id="5e3fe-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="5e3fe-123">Mithilfe von "Microsoft. owin. Host. systemWeb" können Sie die Windows-Authentifizierung in IIS einfach aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="5e3fe-124">Beginnen Sie mit der Erstellung einer neuen ASP.NET-Anwendung, indem Sie die Projektvorlage "ASP.net Empty Webanwendung" verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="5e3fe-125">Fügen Sie als nächstes nuget-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-125">Next, add NuGet packages.</span></span> <span data-ttu-id="5e3fe-126">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5e3fe-127">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="5e3fe-128">Fügen Sie nun eine Klasse mit dem Namen `Startup` mit folgendem Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="5e3fe-129">Das ist alles, was Sie benötigen, um eine "Hello World"-Anwendung für owin zu erstellen, die auf IIS ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="5e3fe-130">Drücken Sie F5, um die Anwendung zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-130">Press F5 to debug the application.</span></span> <span data-ttu-id="5e3fe-131">„Hello World!“ muss auf</span><span class="sxs-lookup"><span data-stu-id="5e3fe-131">You should see "Hello World!"</span></span> <span data-ttu-id="5e3fe-132">im Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="5e3fe-133">Als nächstes aktivieren wir die Windows-Authentifizierung in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="5e3fe-134">Wählen Sie im Menü **Ansicht** die Option **Eigenschaften**aus.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="5e3fe-135">Klicken Sie in Projektmappen-Explorer auf den Projektnamen, um die Projekteigenschaften anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="5e3fe-136">Legen Sie im **Eigenschaften** Fenster die **anonyme Authentifizierung** auf **deaktiviert** fest, und legen Sie die **Windows-Authentifizierung** auf **aktiviert**fest.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="5e3fe-137">Wenn Sie die Anwendung in Visual Studio ausführen, müssen IIS Express die Windows-Anmelde Informationen des Benutzers anfordern.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="5e3fe-138">Dies können Sie mithilfe von " [fddler](http://fiddler2.com/home) " oder einem anderen HTTP-Debugtool sehen.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="5e3fe-139">Im folgenden finden Sie ein Beispiel für eine HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="5e3fe-140">Die WWW-Authenticate-Header in dieser Antwort zeigen an, dass der Server das [Aushandlungs](http://www.ietf.org/rfc/rfc4559.txt) Protokoll unterstützt, das entweder Kerberos oder NTLM verwendet.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="5e3fe-141">Wenn Sie die Anwendung später auf einem Server bereitstellen, führen Sie die folgenden [Schritte](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) aus, um die Windows-Authentifizierung in IIS auf diesem Server zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="5e3fe-142">Windows-Authentifizierung in HttpListener</span><span class="sxs-lookup"><span data-stu-id="5e3fe-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="5e3fe-143">Wenn Sie Microsoft. owin. Host. HttpListener für die selbst gehosteter Katana verwenden, können Sie die Windows-Authentifizierung direkt auf der **HttpListener** -Instanz aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="5e3fe-144">Erstellen Sie zunächst eine neue Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-144">First, create a new console application.</span></span> <span data-ttu-id="5e3fe-145">Fügen Sie als nächstes nuget-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-145">Next, add NuGet packages.</span></span> <span data-ttu-id="5e3fe-146">Klicken Sie **im Menü** Extras auf **nuget-Paket-Manager**, und wählen Sie dann Paket-Manager- **Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5e3fe-147">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="5e3fe-148">Fügen Sie nun eine Klasse mit dem Namen `Startup` mit folgendem Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="5e3fe-149">Diese Klasse implementiert das gleiche "Hello World"-Beispiel von vor, aber Sie legt auch die Windows-Authentifizierung als Authentifizierungsschema fest.</span><span class="sxs-lookup"><span data-stu-id="5e3fe-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="5e3fe-150">Starten Sie in der `Main`-Funktion die owin-Pipeline:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="5e3fe-151">Sie können eine Anforderung in "fddler" senden, um zu bestätigen, dass die Anwendung die Windows-Authentifizierung verwendet:</span><span class="sxs-lookup"><span data-stu-id="5e3fe-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="5e3fe-152">Verwandte Themen</span><span class="sxs-lookup"><span data-stu-id="5e3fe-152">Related Topics</span></span>

[<span data-ttu-id="5e3fe-153">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="5e3fe-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="5e3fe-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="5e3fe-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="5e3fe-155">Grundlegendes zur owin-Formular Authentifizierung in MVC 5</span><span class="sxs-lookup"><span data-stu-id="5e3fe-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
