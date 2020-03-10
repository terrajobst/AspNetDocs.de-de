---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Signalr 1. x Hubs-API-Handbuch-JavaScript-Client | Microsoft-Dokumentation
author: bradygaster
description: Dieses Dokument bietet eine Einführung in die Verwendung der Hubs-API für die signalr-Version 1,1 in JavaScript-Clients, z. b. Browser und Windows Store (winjs).
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431103"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="9dc79-103">Leitfaden für die SignalR 1.x-Hubs-API – JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="9dc79-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>

<span data-ttu-id="9dc79-104">von [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9dc79-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="9dc79-105">Dieses Dokument enthält eine Einführung in die Verwendung der Hubs-API für die signalr-Version 1,1 in JavaScript-Clients, z. b. Browser und Windows Store-Anwendungen (winjs).</span><span class="sxs-lookup"><span data-stu-id="9dc79-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="9dc79-106">Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server.</span><span class="sxs-lookup"><span data-stu-id="9dc79-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="9dc79-107">In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="9dc79-108">Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="9dc79-109">Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="9dc79-110">Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="9dc79-111">Eine Einführung in signalr, Hubs und persistente Verbindungen oder ein Tutorial, das zeigt, wie Sie eine komplette signalr-Anwendung erstellen, finden Sie unter [signalr-Getting Started](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="9dc79-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="9dc79-112">Übersicht</span><span class="sxs-lookup"><span data-stu-id="9dc79-112">Overview</span></span>

<span data-ttu-id="9dc79-113">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="9dc79-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="9dc79-114">Der generierte Proxy und dessen Funktionsweise</span><span class="sxs-lookup"><span data-stu-id="9dc79-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="9dc79-115">Verwendungszwecke des generierten Proxys</span><span class="sxs-lookup"><span data-stu-id="9dc79-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="9dc79-116">Client Einrichtung</span><span class="sxs-lookup"><span data-stu-id="9dc79-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="9dc79-117">Verweisen auf den dynamisch generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="9dc79-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="9dc79-118">Erstellen einer physischen Datei für den von signalr generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="9dc79-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="9dc79-119">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="9dc79-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="9dc79-120">$. Connection. Hub ist das gleiche Objekt, das von $. hubconnection () erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="9dc79-121">Asynchrone Ausführung der Start Methode</span><span class="sxs-lookup"><span data-stu-id="9dc79-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="9dc79-122">Einrichten einer Domänen übergreifenden Verbindung</span><span class="sxs-lookup"><span data-stu-id="9dc79-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="9dc79-123">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="9dc79-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="9dc79-124">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="9dc79-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="9dc79-125">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="9dc79-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="9dc79-126">Vorgehensweise beim erhalten eines Proxys für eine Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="9dc79-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="9dc79-127">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="9dc79-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="9dc79-128">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="9dc79-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="9dc79-129">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="9dc79-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="9dc79-130">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="9dc79-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="9dc79-131">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="9dc79-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="9dc79-132">Dokumentation zum Programmieren der Server-oder .NET-Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="9dc79-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="9dc79-133">Leitfaden für signalr-Hubs-API-Server</span><span class="sxs-lookup"><span data-stu-id="9dc79-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="9dc79-134">Leitfaden für signalr-Hubs-API: .NET-Client</span><span class="sxs-lookup"><span data-stu-id="9dc79-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="9dc79-135">Links zu API-Referenz Themen beziehen sich auf die .NET 4,5-Version der API.</span><span class="sxs-lookup"><span data-stu-id="9dc79-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="9dc79-136">Wenn Sie .NET 4 verwenden, finden Sie weitere Informationen unter [.NET 4-Version der API-Themen](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="9dc79-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="9dc79-137">Der generierte Proxy und dessen Funktionsweise</span><span class="sxs-lookup"><span data-stu-id="9dc79-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="9dc79-138">Sie können einen JavaScript-Client programmieren, um mit einem signalr-Dienst mit oder ohne einen Proxy zu kommunizieren, den signalr für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="9dc79-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="9dc79-139">Der Proxy für Sie vereinfacht die Syntax des Codes, den Sie verwenden, um eine Verbindung herzustellen, Methoden zu schreiben, die vom Server aufgerufen werden, und Methoden auf dem Server aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="9dc79-140">Wenn Sie Code zum Abrufen von Server Methoden schreiben, können Sie mit dem generierten Proxy die Syntax verwenden, die so aussieht, als ob Sie eine lokale Funktion ausgeführt haben: Sie können `serverMethod(arg1, arg2)` statt `invoke('serverMethod', arg1, arg2)`schreiben.</span><span class="sxs-lookup"><span data-stu-id="9dc79-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="9dc79-141">Die generierte Proxy Syntax ermöglicht außerdem einen sofortigen und verständlichen Client seitigen Fehler, wenn Sie einen Server Methodennamen falsch formatieren.</span><span class="sxs-lookup"><span data-stu-id="9dc79-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="9dc79-142">Wenn Sie die Datei, die die Proxys definiert, manuell erstellen, können Sie auch IntelliSense-Unterstützung für das Schreiben von Code zum Aufrufen von Server Methoden erhalten.</span><span class="sxs-lookup"><span data-stu-id="9dc79-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="9dc79-143">Angenommen, Sie haben die folgende Hub-Klasse auf dem Server:</span><span class="sxs-lookup"><span data-stu-id="9dc79-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="9dc79-144">In den folgenden Codebeispielen wird veranschaulicht, wie JavaScript-Code zum Aufrufen der `NewContosoChatMessage`-Methode auf dem Server und zum Empfangen von Aufrufen der `addContosoChatMessageToPage` Methode vom Server aussieht.</span><span class="sxs-lookup"><span data-stu-id="9dc79-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="9dc79-145">**Mit dem generierten Proxy**</span><span class="sxs-lookup"><span data-stu-id="9dc79-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="9dc79-146">**Ohne den generierten Proxy**</span><span class="sxs-lookup"><span data-stu-id="9dc79-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="9dc79-147">Verwendungszwecke des generierten Proxys</span><span class="sxs-lookup"><span data-stu-id="9dc79-147">When to use the generated proxy</span></span>

<span data-ttu-id="9dc79-148">Wenn Sie mehrere Ereignishandler für eine Client Methode registrieren möchten, die vom Server aufgerufen wird, können Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="9dc79-149">Andernfalls können Sie auswählen, ob der generierte Proxy verwendet werden soll oder nicht, basierend auf der Codierungs Einstellung.</span><span class="sxs-lookup"><span data-stu-id="9dc79-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="9dc79-150">Wenn Sie die Option nicht verwenden möchten, müssen Sie nicht auf die "signalr/Hubs"-URL in einem `script`-Element in Ihrem Client Code verweisen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="9dc79-151">Clientsetup</span><span class="sxs-lookup"><span data-stu-id="9dc79-151">Client setup</span></span>

<span data-ttu-id="9dc79-152">Ein JavaScript-Client erfordert Verweise auf jQuery und die signalr Core JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="9dc79-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="9dc79-153">Die jQuery-Version muss 1.6.4 oder größere spätere Versionen sein, z. b. 1.7.2, 1.8.2 oder 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="9dc79-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="9dc79-154">Wenn Sie sich für die Verwendung des generierten Proxys entscheiden, benötigen Sie auch einen Verweis auf die JavaScript-Datei des signalr-generierten Proxys.</span><span class="sxs-lookup"><span data-stu-id="9dc79-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="9dc79-155">Im folgenden Beispiel wird gezeigt, wie die Verweise in einer HTML-Seite aussehen können, die den generierten Proxy verwendet.</span><span class="sxs-lookup"><span data-stu-id="9dc79-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="9dc79-156">Diese Verweise müssen in dieser Reihenfolge enthalten sein: zuerst jQuery, signalr Core und schließlich signalr-Proxys.</span><span class="sxs-lookup"><span data-stu-id="9dc79-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="9dc79-157">Verweisen auf den dynamisch generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="9dc79-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="9dc79-158">Im vorangehenden Beispiel besteht der Verweis auf den von signalr generierten Proxy in der dynamischen Generierung von JavaScript-Code und nicht in einer physischen Datei.</span><span class="sxs-lookup"><span data-stu-id="9dc79-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="9dc79-159">Signalr erstellt den JavaScript-Code für den Proxy im Handumdrehen und bedient ihn als Reaktion auf die URL "/signalr/Hubs" an den Client.</span><span class="sxs-lookup"><span data-stu-id="9dc79-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="9dc79-160">Wenn Sie in der `MapHubs`-Methode eine andere Basis-URL für signalr-Verbindungen auf dem Server angegeben haben, ist die URL für die dynamisch generierte Proxy Datei Ihre benutzerdefinierte URL, an die "/Hubs" angehängt ist.</span><span class="sxs-lookup"><span data-stu-id="9dc79-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="9dc79-161">Verwenden Sie für Windows 8-JavaScript-Clients (Windows Store) die physische Proxy Datei anstelle der dynamisch generierten.</span><span class="sxs-lookup"><span data-stu-id="9dc79-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="9dc79-162">Weitere Informationen finden Sie weiter unten in diesem Thema unter Vorgehens [Weise beim Erstellen einer physischen Datei für den signalr-generierten Proxy](#manualproxy) .</span><span class="sxs-lookup"><span data-stu-id="9dc79-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="9dc79-163">Verwenden Sie in einer ASP.NET MVC 4 Razor-Ansicht die Tilde, um auf den Anwendungs Stamm in der Proxy Dateireferenz zu verweisen:</span><span class="sxs-lookup"><span data-stu-id="9dc79-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="9dc79-164">Weitere Informationen zur Verwendung von signalr in MVC 4 finden Sie unter Einführung in [signalr und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="9dc79-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="9dc79-165">Verwenden Sie in einer ASP.NET MVC 3 Razor-Ansicht `Url.Content` für den Proxy Datei Verweis:</span><span class="sxs-lookup"><span data-stu-id="9dc79-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="9dc79-166">Verwenden Sie in einer ASP.net-Web Forms Anwendung `ResolveClientUrl` für Ihren proxydateiverweis, oder registrieren Sie ihn über den ScriptManager mithilfe eines relativen Pfads (beginnend mit einer Tilde):</span><span class="sxs-lookup"><span data-stu-id="9dc79-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="9dc79-167">Verwenden Sie als allgemeine Regel dieselbe Methode zum Angeben der URL "/signalr/Hubs", die Sie für CSS-oder JavaScript-Dateien verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="9dc79-168">Wenn Sie eine URL ohne eine Tilde angeben, funktioniert die Anwendung in einigen Szenarios ordnungsgemäß, wenn Sie in Visual Studio mit IIS Express testen, schlägt jedoch mit einem 404-Fehler fehl, wenn Sie die Bereitstellung für vollständiges IIS ausführen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="9dc79-169">Weitere Informationen finden Sie unter **Auflösen von Verweisen auf Ressourcen** auf Stamm Ebene in [Webservern in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="9dc79-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="9dc79-170">Wenn Sie ein Webprojekt in Visual Studio 2012 im Debugmodus ausführen und Internet Explorer als Browser verwenden, können Sie die Proxy Datei in **Projektmappen-Explorer** unter **Skript Dokumenten**sehen, wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Von JavaScript generierte Proxy Datei in Projektmappen-Explorer](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="9dc79-172">Um den Inhalt der Datei anzuzeigen, doppelklicken Sie auf **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="9dc79-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="9dc79-173">Wenn Sie nicht Visual Studio 2012 und Internet Explorer verwenden oder sich nicht im Debugmodus befinden, können Sie auch den Inhalt der Datei durch Navigieren zur URL "/signalR/Hubs" erhalten.</span><span class="sxs-lookup"><span data-stu-id="9dc79-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="9dc79-174">Wenn Ihre Website beispielsweise auf `http://localhost:56699`ausgeführt wird, navigieren Sie in Ihrem Browser zu `http://localhost:56699/SignalR/hubs`.</span><span class="sxs-lookup"><span data-stu-id="9dc79-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="9dc79-175">Erstellen einer physischen Datei für den von signalr generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="9dc79-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="9dc79-176">Als Alternative zum dynamisch generierten Proxy können Sie eine physische Datei mit dem Proxycode erstellen und auf diese Datei verweisen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="9dc79-177">Dies kann zum Steuern der Zwischenspeicherung oder zum Bündeln von Verhalten oder zum Aufrufen von IntelliSense bei der Codierung von Aufrufen von Server Methoden verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="9dc79-178">Führen Sie die folgenden Schritte aus, um eine Proxy Datei zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="9dc79-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="9dc79-179">Installieren Sie das nuget-Paket [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="9dc79-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="9dc79-180">Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner *Tools* , der die Datei signalr. exe enthält.</span><span class="sxs-lookup"><span data-stu-id="9dc79-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="9dc79-181">Der Ordner Tools befindet sich an folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="9dc79-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="9dc79-182">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="9dc79-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="9dc79-183">Der Pfad zu Ihrer *dll* ist in der Regel der Ordner " *bin* " im Projektordner.</span><span class="sxs-lookup"><span data-stu-id="9dc79-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="9dc79-184">Dieser Befehl erstellt eine Datei namens " *Server. js* " im gleichen Ordner wie " *signalr. exe*".</span><span class="sxs-lookup"><span data-stu-id="9dc79-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="9dc79-185">Platzieren Sie die Datei " *Server. js* " in einem entsprechenden Ordner in Ihrem Projekt, benennen Sie Sie entsprechend Ihrer Anwendung um, und fügen Sie anstelle der Referenz "signalr/Hubs" einen Verweis darauf ein.</span><span class="sxs-lookup"><span data-stu-id="9dc79-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="9dc79-186">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="9dc79-186">How to establish a connection</span></span>

<span data-ttu-id="9dc79-187">Bevor Sie eine Verbindung herstellen können, müssen Sie ein Verbindungs Objekt erstellen, einen Proxy erstellen und Ereignishandler für Methoden registrieren, die vom Server aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="9dc79-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="9dc79-188">Stellen Sie beim Einrichten des Proxy-und Ereignishandler die Verbindung her, indem Sie die `start`-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="9dc79-189">Wenn Sie den generierten Proxy verwenden, müssen Sie das Verbindungs Objekt nicht in Ihrem eigenen Code erstellen, da der generierte Proxy Code dies für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="9dc79-190">**Herstellen einer Verbindung (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="9dc79-191">**Herstellen einer Verbindung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="9dc79-192">Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="9dc79-193">Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="9dc79-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="9dc79-194">Normalerweise registrieren Sie Ereignishandler, bevor Sie die `start`-Methode aufrufen, um die Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="9dc79-195">Wenn Sie einige Ereignishandler registrieren möchten, nachdem Sie die Verbindung hergestellt haben, können Sie dies tun. Sie müssen jedoch mindestens einen ihrer Ereignishandler registrieren, bevor Sie die `start`-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="9dc79-196">Ein Grund hierfür ist, dass es möglicherweise viele Hubs in einer Anwendung gibt, aber Sie sollten das `OnConnected`-Ereignis nicht auf jedem Hub auslassen, wenn Sie nur eine von Ihnen verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="9dc79-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="9dc79-197">Wenn die Verbindung hergestellt wird, weist das vorhanden sein einer Client Methode auf dem Proxy eines Hubs an, dass signalr das `OnConnected` Ereignis auslöst.</span><span class="sxs-lookup"><span data-stu-id="9dc79-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="9dc79-198">Wenn Sie keine Ereignishandler registrieren, bevor Sie die `start`-Methode aufrufen, können Sie Methoden für den Hub aufrufen, aber die `OnConnected`-Methode des Hubs wird nicht aufgerufen, und es werden keine Client Methoden vom Server aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="9dc79-199">$. Connection. Hub ist das gleiche Objekt, das von $. hubconnection () erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="9dc79-200">Wie Sie in den Beispielen sehen können, verweist `$.connection.hub` auf das Verbindungs Objekt, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="9dc79-201">Dabei handelt es sich um dasselbe Objekt, das Sie durch Aufrufen von `$.hubConnection()` erhalten, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="9dc79-202">Der generierte Proxy Code erstellt die Verbindung für Sie durch Ausführen der folgenden Anweisung:</span><span class="sxs-lookup"><span data-stu-id="9dc79-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Erstellen einer Verbindung in der generierten Proxy Datei](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="9dc79-204">Wenn Sie den generierten Proxy verwenden, können Sie mit `$.connection.hub` arbeiten, die Sie mit einem Verbindungs Objekt ausführen können, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="9dc79-205">Asynchrone Ausführung der Start Methode</span><span class="sxs-lookup"><span data-stu-id="9dc79-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="9dc79-206">Die `start`-Methode wird asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="9dc79-207">Sie gibt ein [Verzögertes jQuery-Objekt](http://api.jquery.com/category/deferred-object/)zurück. Dies bedeutet, dass Sie Rückruf Funktionen hinzufügen können, indem Sie Methoden wie `pipe`, `done`und `fail`aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="9dc79-208">Wenn Sie über Code verfügen, der nach dem Herstellen der Verbindung ausgeführt werden soll, z. b. einen Server Methodenaufrufe, fügen Sie diesen Code in eine Rückruffunktion ein, oder rufen Sie ihn über eine Rückruffunktion auf.</span><span class="sxs-lookup"><span data-stu-id="9dc79-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="9dc79-209">Die `.done` Rückruf Methode wird ausgeführt, nachdem die Verbindung hergestellt wurde, und nach dem Ausführen von Code, den Sie in ihrer `OnConnected` Ereignishandlermethode auf dem Server haben, wird die Ausführung beendet.</span><span class="sxs-lookup"><span data-stu-id="9dc79-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="9dc79-210">Wenn Sie die "jetzt verbundene"-Anweisung aus dem vorherigen Beispiel als nächste Codezeile nach dem `start`-Methoden Aufrufs (nicht in einem `.done`-Rückruf) ablegen, wird die `console.log` Zeile vor dem Herstellen der Verbindung ausgeführt, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="9dc79-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Falsche Methode zum Schreiben von Code, der nach dem Herstellen der Verbindung ausgeführt wird.](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="9dc79-212">Einrichten einer Domänen übergreifenden Verbindung</span><span class="sxs-lookup"><span data-stu-id="9dc79-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="9dc79-213">Wenn der Browser eine Seite aus `http://contoso.com`lädt, befindet sich die signalr-Verbindung in der Regel in der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="9dc79-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="9dc79-214">Wenn die Seite aus `http://contoso.com` eine Verbindung mit `http://fabrikam.com/signalr`herstellt, handelt es sich um eine Domänen übergreifende Verbindung.</span><span class="sxs-lookup"><span data-stu-id="9dc79-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="9dc79-215">Aus Sicherheitsgründen sind Domänen übergreifende Verbindungen standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="9dc79-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="9dc79-216">Stellen Sie sicher, dass Domänen übergreifende Verbindungen auf dem Server aktiviert sind, und geben Sie die Verbindungs-URL an, wenn Sie das Verbindungs Objekt erstellen, um eine Domänen übergreifende Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="9dc79-217">Signalr verwendet die geeignete Technologie für Domänen übergreifende Verbindungen, z. b. [JSONP](http://en.wikipedia.org/wiki/JSONP) oder [cors](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="9dc79-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="9dc79-218">Aktivieren Sie auf dem-Server Domänen übergreifende Verbindungen, indem Sie diese Option auswählen, wenn Sie die `MapHubs`-Methode aufgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="9dc79-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="9dc79-219">Geben Sie auf dem Client die URL an, wenn Sie das Verbindungs Objekt (ohne den generierten Proxy) erstellen oder bevor Sie die Start Methode (mit dem generierten Proxy) aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="9dc79-220">**Client Code, der eine Domänen übergreifende Verbindung (mit dem generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="9dc79-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="9dc79-221">**Client Code, der eine Domänen übergreifende Verbindung angibt (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="9dc79-222">Wenn Sie den `$.hubConnection`-Konstruktor verwenden, müssen Sie `signalr` nicht in die URL einschließen, da er automatisch hinzugefügt wird (es sei denn, Sie geben `useDefaultUrl` als `false`an).</span><span class="sxs-lookup"><span data-stu-id="9dc79-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="9dc79-223">Sie können mehrere Verbindungen mit unterschiedlichen Endpunkten erstellen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="9dc79-224">Legen Sie im Code nicht `jQuery.support.cors` auf "true" fest.</span><span class="sxs-lookup"><span data-stu-id="9dc79-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Legen Sie jQuery. Support. cors nicht auf "true" fest.](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="9dc79-226">Signalr übernimmt die Verwendung von JSONP oder cors.</span><span class="sxs-lookup"><span data-stu-id="9dc79-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="9dc79-227">Wenn Sie `jQuery.support.cors` auf true festlegen, wird JSONP deaktiviert, da signalr davon ausgeht, dass der Browser cors unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="9dc79-228">Wenn Sie eine Verbindung mit einer localhost-URL herstellen, wird Sie von Internet Explorer 10 nicht als Domänen übergreifende Verbindung betrachtet, sodass die Anwendung lokal mit IE 10 funktioniert, auch wenn Sie keine Domänen übergreifenden Verbindungen auf dem Server aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="9dc79-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="9dc79-229">Informationen zur Verwendung von Domänen übergreifenden Verbindungen mit Internet Explorer 9 finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="9dc79-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="9dc79-230">Weitere Informationen zur Verwendung Domänen übergreifender Verbindungen mit Chrome finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="9dc79-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="9dc79-231">Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="9dc79-232">Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="9dc79-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="9dc79-233">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="9dc79-233">How to configure the connection</span></span>

<span data-ttu-id="9dc79-234">Bevor Sie eine Verbindung herstellen, können Sie Abfrage Zeichen folgen Parameter angeben oder die Transportmethode angeben.</span><span class="sxs-lookup"><span data-stu-id="9dc79-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="9dc79-235">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="9dc79-235">How to specify query string parameters</span></span>

<span data-ttu-id="9dc79-236">Wenn Sie Daten an den Server senden möchten, wenn der Client eine Verbindung herstellt, können Sie dem Verbindungs Objekt Abfrage Zeichenfolgen-Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="9dc79-237">In den folgenden Beispielen wird gezeigt, wie Sie einen Abfrage Zeichen folgen Parameter im Client Code festlegen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="9dc79-238">**Legen Sie vor dem Aufrufen der Start Methode (mit dem generierten Proxy) einen Abfrage Zeichen folgen Wert fest.**</span><span class="sxs-lookup"><span data-stu-id="9dc79-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="9dc79-239">**Legen Sie vor dem Aufrufen der Start Methode (ohne den generierten Proxy) einen Abfrage Zeichen folgen Wert fest.**</span><span class="sxs-lookup"><span data-stu-id="9dc79-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="9dc79-240">Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="9dc79-241">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="9dc79-241">How to specify the transport method</span></span>

<span data-ttu-id="9dc79-242">Im Rahmen der Verbindungs Herstellung aushandiert ein signalr-Client normalerweise mit dem Server, um den optimalen Transport zu ermitteln, der von Server und Client unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="9dc79-243">Wenn Sie bereits wissen, welcher Transport Sie verwenden möchten, können Sie diesen Aushandlungs Prozess umgehen, indem Sie die Transportmethode angeben, wenn Sie die `start`-Methode aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="9dc79-244">**Client Code, der die Transportmethode angibt (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="9dc79-245">**Client Code, der die Transportmethode angibt (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="9dc79-246">Als Alternative können Sie mehrere Transportmethoden in der Reihenfolge angeben, in der Sie von signalr ausprobiert werden sollen:</span><span class="sxs-lookup"><span data-stu-id="9dc79-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="9dc79-247">**Client Code, der ein benutzerdefiniertes Transport Fall Back Schema (mit dem generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="9dc79-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="9dc79-248">**Client Code, der ein benutzerdefiniertes Transport Fall Back Schema (ohne den generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="9dc79-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="9dc79-249">Sie können die folgenden Werte verwenden, um die Transportmethode anzugeben:</span><span class="sxs-lookup"><span data-stu-id="9dc79-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="9dc79-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="9dc79-250">"webSockets"</span></span>
- <span data-ttu-id="9dc79-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="9dc79-251">"foreverFrame"</span></span>
- <span data-ttu-id="9dc79-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="9dc79-252">"serverSentEvents"</span></span>
- <span data-ttu-id="9dc79-253">"longabruf"</span><span class="sxs-lookup"><span data-stu-id="9dc79-253">"longPolling"</span></span>

<span data-ttu-id="9dc79-254">In den folgenden Beispielen wird gezeigt, wie Sie herausfinden, welche Transportmethode von einer Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="9dc79-255">**Client Code, der die Transportmethode anzeigt, die von einer Verbindung verwendet wird (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="9dc79-256">**Client Code, der die Transportmethode anzeigt, die von einer Verbindung verwendet wird (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="9dc79-257">Informationen zum Überprüfen der Transportmethode im Servercode finden Sie unter [ASP.net signalr Hubs API Guide-Server-How to Get Information about the Client from the context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="9dc79-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="9dc79-258">Weitere Informationen zu Transporten und Fallbacks finden Sie unter [Einführung in signalr-Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="9dc79-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="9dc79-259">Vorgehensweise beim erhalten eines Proxys für eine Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="9dc79-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="9dc79-260">Jedes von Ihnen erstellte Verbindungs Objekt kapselt Informationen zu einer Verbindung mit einem signalr-Dienst, der eine oder mehrere Hub-Klassen enthält.</span><span class="sxs-lookup"><span data-stu-id="9dc79-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="9dc79-261">Um mit einer Hub-Klasse zu kommunizieren, verwenden Sie ein Proxy Objekt, das Sie selbst erstellen (wenn Sie den generierten Proxy nicht verwenden) oder das für Sie generiert wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="9dc79-262">Auf dem Client ist der Name des Proxys eine Camel-/schreibversion des Namens der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="9dc79-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="9dc79-263">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="9dc79-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="9dc79-264">**Hub-Klasse auf dem Server**</span><span class="sxs-lookup"><span data-stu-id="9dc79-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="9dc79-265">**Beziehen eines Verweises auf den generierten Client Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="9dc79-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="9dc79-266">**Erstellen eines Client Proxys für die Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="9dc79-267">Wenn Sie Ihre Hub-Klasse mit einem `HubName` Attribut ergänzen, verwenden Sie den genauen Namen, ohne den Fall zu ändern.</span><span class="sxs-lookup"><span data-stu-id="9dc79-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="9dc79-268">**Hubklasse auf Server mit hubname-Attribut**</span><span class="sxs-lookup"><span data-stu-id="9dc79-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="9dc79-269">**Beziehen eines Verweises auf den generierten Client Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="9dc79-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="9dc79-270">**Erstellen eines Client Proxys für die Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="9dc79-271">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="9dc79-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="9dc79-272">Um eine Methode zu definieren, die vom Server von einem Hub aufgerufen werden kann, fügen Sie dem Hub-Proxy einen Ereignishandler hinzu, indem Sie die `client`-Eigenschaft des generierten Proxys verwenden oder die `on`-Methode aufzurufen, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="9dc79-273">Die Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="9dc79-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="9dc79-274">Fügen Sie den-Ereignishandler hinzu, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="9dc79-275">(Wenn Sie Ereignishandler hinzufügen möchten, nachdem Sie die `start`-Methode aufgerufen haben, lesen Sie den Hinweis unter [Herstellen einer Verbindung weiter](#establishconnection) oben in diesem Dokument, und verwenden Sie die Syntax, die zum Definieren einer Methode ohne Verwendung des generierten Proxys angezeigt wird.)</span><span class="sxs-lookup"><span data-stu-id="9dc79-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="9dc79-276">Beim Methodennamen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="9dc79-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="9dc79-277">Beispielsweise werden auf dem-Server `Clients.All.addContosoChatMessageToPage` auf dem-Server `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`oder `addcontosochatmessagetopage` auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="9dc79-278">**Methode auf dem Client definieren (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="9dc79-279">**Alternative Methode zum Definieren der Methode auf dem Client (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="9dc79-280">**Methode auf dem Client definieren (ohne den generierten Proxy oder beim Hinzufügen nach dem Aufruf der Start-Methode)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="9dc79-281">**Server Code, der die Client Methode aufruft**</span><span class="sxs-lookup"><span data-stu-id="9dc79-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="9dc79-282">Die folgenden Beispiele enthalten ein komplexes Objekt als Methoden Parameter.</span><span class="sxs-lookup"><span data-stu-id="9dc79-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="9dc79-283">**Methode auf dem Client definieren, die ein komplexes Objekt annimmt (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="9dc79-284">**Methode auf dem Client definieren, die ein komplexes Objekt annimmt (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="9dc79-285">**Server Code, der das komplexe Objekt definiert**</span><span class="sxs-lookup"><span data-stu-id="9dc79-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="9dc79-286">**Server Code, der die Client Methode mithilfe eines komplexen Objekts aufruft**</span><span class="sxs-lookup"><span data-stu-id="9dc79-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="9dc79-287">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="9dc79-287">How to call server methods from the client</span></span>

<span data-ttu-id="9dc79-288">Um eine Server Methode vom Client aufzurufen, verwenden Sie die `server`-Eigenschaft des generierten Proxys oder die `invoke`-Methode des Hub-Proxys, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="9dc79-289">Der Rückgabewert oder die Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="9dc79-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="9dc79-290">Übergeben Sie eine Camel-Case-Version des Methoden namens auf dem Hub.</span><span class="sxs-lookup"><span data-stu-id="9dc79-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="9dc79-291">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="9dc79-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="9dc79-292">In den folgenden Beispielen wird veranschaulicht, wie eine Server Methode aufgerufen wird, die keinen Rückgabewert hat, und wie eine Server Methode aufgerufen wird, die über einen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="9dc79-293">**Server Methode ohne hubmethodname-Attribut**</span><span class="sxs-lookup"><span data-stu-id="9dc79-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="9dc79-294">**Server Code, der das komplexe Objekt definiert, das in einem Parameter übergeben wird**</span><span class="sxs-lookup"><span data-stu-id="9dc79-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="9dc79-295">**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="9dc79-296">**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="9dc79-297">Wenn Sie die Hub-Methode mit einem `HubMethodName`-Attribut versehen haben, verwenden Sie diesen Namen, ohne den Fall zu ändern.</span><span class="sxs-lookup"><span data-stu-id="9dc79-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="9dc79-298">**Server Methode** mit einem hubmethodname-Attribut</span><span class="sxs-lookup"><span data-stu-id="9dc79-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="9dc79-299">**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="9dc79-300">**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="9dc79-301">In den vorangehenden Beispielen wird gezeigt, wie eine Server Methode aufgerufen wird, die über keinen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="9dc79-302">In den folgenden Beispielen wird gezeigt, wie eine Server Methode aufgerufen wird, die über einen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="9dc79-303">**Server Code für eine Methode, die über einen Rückgabewert verfügt**</span><span class="sxs-lookup"><span data-stu-id="9dc79-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="9dc79-304">**Die für den Rückgabewert verwendete Aktienklasse** .</span><span class="sxs-lookup"><span data-stu-id="9dc79-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="9dc79-305">**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="9dc79-306">**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="9dc79-307">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="9dc79-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="9dc79-308">Signalr stellt die folgenden Ereignisse für die Verbindungs Lebensdauer bereit, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="9dc79-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="9dc79-309">`starting`: wird ausgelöst, bevor Daten über die Verbindung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="9dc79-310">`received`: wird ausgelöst, wenn Daten über die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="9dc79-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="9dc79-311">Stellt die empfangenen Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="9dc79-311">Provides the received data.</span></span>
- <span data-ttu-id="9dc79-312">`connectionSlow`: wird ausgelöst, wenn der Client eine langsame oder häufig gelöschter Verbindung erkennt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="9dc79-313">`reconnecting`: wird ausgelöst, wenn der zugrunde liegende Transport mit dem erneuten Verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="9dc79-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="9dc79-314">`reconnected`: wird ausgelöst, wenn der zugrunde liegende Transport die Verbindung wieder hergestellt hat.</span><span class="sxs-lookup"><span data-stu-id="9dc79-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="9dc79-315">`stateChanged`: wird ausgelöst, wenn sich der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="9dc79-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="9dc79-316">Stellt den alten und den neuen Status bereit (Verbindung, verbunden, wiederherstellen oder Verbindung getrennt).</span><span class="sxs-lookup"><span data-stu-id="9dc79-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="9dc79-317">`disconnected`: wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="9dc79-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="9dc79-318">Wenn Sie z. b. Warnmeldungen anzeigen möchten, wenn Verbindungsprobleme auftreten, die zu spürbaren Verzögerungen führen können, behandeln Sie das `connectionSlow`-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="9dc79-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="9dc79-319">**Behandeln des connectionslow-Ereignisses (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="9dc79-320">**Behandeln des connectionslow-Ereignisses (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="9dc79-321">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="9dc79-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="9dc79-322">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="9dc79-322">How to handle errors</span></span>

<span data-ttu-id="9dc79-323">Der signalr-JavaScript-Client stellt ein `error` Ereignis bereit, für das Sie einen Handler hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="9dc79-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="9dc79-324">Sie können auch die Fail-Methode verwenden, um einen Handler für Fehler hinzuzufügen, die sich aus einem Aufruf der Server Methode ergeben.</span><span class="sxs-lookup"><span data-stu-id="9dc79-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="9dc79-325">Wenn Sie auf dem Server nicht explizit detaillierte Fehlermeldungen aktivieren, enthält das Ausnahme Objekt, das signalr nach einem Fehler zurückgibt, nur minimale Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="9dc79-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="9dc79-326">Wenn beispielsweise ein `newContosoChatMessage` Fehler auftritt, enthält die Fehlermeldung im Fehler Objekt den Wert "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`", dass detaillierte Fehlermeldungen an Clients in der Produktionsumgebung nicht gesendet werden sollen. Wenn Sie jedoch ausführliche Fehlermeldungen zu Problem Behandlungszwecken aktivieren möchten, verwenden Sie den folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="9dc79-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="9dc79-327">Im folgenden Beispiel wird gezeigt, wie Sie einen Handler für das Fehler Ereignis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="9dc79-328">**Hinzufügen eines Fehler Handlers (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="9dc79-329">**Hinzufügen eines Fehler Handlers (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="9dc79-330">Im folgenden Beispiel wird gezeigt, wie ein Fehler von einem Methodenaufruf behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="9dc79-331">**Behandeln eines Fehlers von einem Methodenaufruf (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="9dc79-332">**Behandeln eines Fehlers von einem Methodenaufruf (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="9dc79-333">Wenn ein Methodenaufruf fehlschlägt, wird das `error`-Ereignis ebenfalls ausgelöst, sodass der Code im `error` Methoden Handler und im `.fail`-Methoden Rückruf ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9dc79-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="9dc79-334">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="9dc79-334">How to enable client-side logging</span></span>

<span data-ttu-id="9dc79-335">Um die Client seitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging`-Eigenschaft für das Verbindungs Objekt fest, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="9dc79-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="9dc79-336">**Aktivieren der Protokollierung (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="9dc79-337">**Aktivieren der Protokollierung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="9dc79-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="9dc79-338">Um die Protokolle anzuzeigen, öffnen Sie die Entwicklertools Ihres Browsers, und navigieren Sie zur Registerkarte Konsole. Ein Tutorial mit Schritt-für-Schritt-Anweisungen und Screenshots, die die Vorgehensweise veranschaulichen, finden Sie unter [Server Broadcast mit ASP.net signalr-enable Logging](index.md).</span><span class="sxs-lookup"><span data-stu-id="9dc79-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
