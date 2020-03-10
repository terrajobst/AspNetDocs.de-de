---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: 'ASP.net signalr Hubs-API-Handbuch: JavaScript-Client | Microsoft-Dokumentation'
author: bradygaster
description: Dieses Dokument enthält eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in JavaScript-Clients, z. b. Browser und Windows Store (winjs) Applicat...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431289"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="cf3a6-103">ASP.net signalr Hubs-API-Handbuch-JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="cf3a6-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="cf3a6-104">Dieses Dokument enthält eine Einführung in die Verwendung der Hubs-API für signalr Version 2 in JavaScript-Clients, z. b. Browser und Windows Store-Anwendungen (winjs).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="cf3a6-105">Die signalr Hubs-API ermöglicht das Ausführen von Remote Prozedur aufrufen (Remote Procedure Calls, RPCs) von einem Server an verbundene Clients und von Clients an den Server.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="cf3a6-106">In Servercode definieren Sie Methoden, die von Clients aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Client ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="cf3a6-107">Im Client Code definieren Sie Methoden, die vom Server aufgerufen werden können, und Sie können Methoden aufrufen, die auf dem Server ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="cf3a6-108">Signalr kümmert sich um die gesamte Client-zu-Server-Grundlagen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="cf3a6-109">Signalr bietet auch eine API auf niedrigerer Ebene, die als persistente Verbindungen bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="cf3a6-110">Eine Einführung in signalr, Hubs und persistente Verbindungen finden Sie unter [Introduction to signalr (Einführung in signalr](../getting-started/introduction-to-signalr.md)).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="cf3a6-111">In diesem Thema verwendete Software Versionen</span><span class="sxs-lookup"><span data-stu-id="cf3a6-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="cf3a6-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="cf3a6-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="cf3a6-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="cf3a6-113">.NET 4.5</span></span>
> - <span data-ttu-id="cf3a6-114">Signalr Version 2</span><span class="sxs-lookup"><span data-stu-id="cf3a6-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="cf3a6-115">Vorherige Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="cf3a6-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="cf3a6-116">Informationen zu früheren Versionen von signalr finden Sie unter [signalr ältere Versionen](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="cf3a6-117">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="cf3a6-117">Questions and comments</span></span>
>
> <span data-ttu-id="cf3a6-118">Bitte informieren Sie sich darüber, wie Ihnen dieses Tutorial gefallen hat und was wir in den Kommentaren unten auf der Seite verbessern konnten.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="cf3a6-119">Wenn Sie Fragen haben, die nicht direkt mit dem Tutorial zusammenhängen, können Sie Sie im [ASP.net signalr-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder in [StackOverflow.com](http://stackoverflow.com/)veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="cf3a6-120">Übersicht</span><span class="sxs-lookup"><span data-stu-id="cf3a6-120">Overview</span></span>

<span data-ttu-id="cf3a6-121">Dieses Dokument enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="cf3a6-122">Der generierte Proxy und dessen Funktionsweise</span><span class="sxs-lookup"><span data-stu-id="cf3a6-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="cf3a6-123">Verwendungszwecke des generierten Proxys</span><span class="sxs-lookup"><span data-stu-id="cf3a6-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="cf3a6-124">Client Einrichtung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="cf3a6-125">Verweisen auf den dynamisch generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="cf3a6-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="cf3a6-126">Erstellen einer physischen Datei für den von signalr generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="cf3a6-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="cf3a6-127">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="cf3a6-128">$. Connection. Hub ist das gleiche Objekt, das von $. hubconnection () erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="cf3a6-129">Asynchrone Ausführung der Start Methode</span><span class="sxs-lookup"><span data-stu-id="cf3a6-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="cf3a6-130">Einrichten einer Domänen übergreifenden Verbindung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="cf3a6-131">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="cf3a6-132">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="cf3a6-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="cf3a6-133">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="cf3a6-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="cf3a6-134">Vorgehensweise beim erhalten eines Proxys für eine Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="cf3a6-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="cf3a6-135">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="cf3a6-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="cf3a6-136">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="cf3a6-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="cf3a6-137">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="cf3a6-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="cf3a6-138">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="cf3a6-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="cf3a6-139">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="cf3a6-140">Dokumentation zum Programmieren der Server-oder .NET-Clients finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="cf3a6-141">Leitfaden für signalr-Hubs-API-Server</span><span class="sxs-lookup"><span data-stu-id="cf3a6-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="cf3a6-142">Leitfaden für signalr-Hubs-API: .NET-Client</span><span class="sxs-lookup"><span data-stu-id="cf3a6-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="cf3a6-143">Die signalr 2-Serverkomponente ist nur in .NET 4,5 verfügbar (obwohl es einen .NET-Client für signalr 2 auf .NET 4,0 gibt).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="cf3a6-144">Der generierte Proxy und dessen Funktionsweise</span><span class="sxs-lookup"><span data-stu-id="cf3a6-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="cf3a6-145">Sie können einen JavaScript-Client programmieren, um mit einem signalr-Dienst mit oder ohne einen Proxy zu kommunizieren, den signalr für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="cf3a6-146">Der Proxy für Sie vereinfacht die Syntax des Codes, den Sie verwenden, um eine Verbindung herzustellen, Methoden zu schreiben, die vom Server aufgerufen werden, und Methoden auf dem Server aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="cf3a6-147">Wenn Sie Code zum Abrufen von Server Methoden schreiben, können Sie mit dem generierten Proxy die Syntax verwenden, die so aussieht, als ob Sie eine lokale Funktion ausgeführt haben: Sie können `serverMethod(arg1, arg2)` statt `invoke('serverMethod', arg1, arg2)`schreiben.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="cf3a6-148">Die generierte Proxy Syntax ermöglicht außerdem einen sofortigen und verständlichen Client seitigen Fehler, wenn Sie einen Server Methodennamen falsch formatieren.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="cf3a6-149">Wenn Sie die Datei, die die Proxys definiert, manuell erstellen, können Sie auch IntelliSense-Unterstützung für das Schreiben von Code zum Aufrufen von Server Methoden erhalten.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="cf3a6-150">Angenommen, Sie haben die folgende Hub-Klasse auf dem Server:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="cf3a6-151">In den folgenden Codebeispielen wird veranschaulicht, wie JavaScript-Code zum Aufrufen der `NewContosoChatMessage`-Methode auf dem Server und zum Empfangen von Aufrufen der `addContosoChatMessageToPage` Methode vom Server aussieht.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="cf3a6-152">**Mit dem generierten Proxy**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="cf3a6-153">**Ohne den generierten Proxy**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="cf3a6-154">Verwendungszwecke des generierten Proxys</span><span class="sxs-lookup"><span data-stu-id="cf3a6-154">When to use the generated proxy</span></span>

<span data-ttu-id="cf3a6-155">Wenn Sie mehrere Ereignishandler für eine Client Methode registrieren möchten, die vom Server aufgerufen wird, können Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="cf3a6-156">Andernfalls können Sie auswählen, ob der generierte Proxy verwendet werden soll oder nicht, basierend auf der Codierungs Einstellung.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="cf3a6-157">Wenn Sie die Option nicht verwenden möchten, müssen Sie nicht auf die "signalr/Hubs"-URL in einem `script`-Element in Ihrem Client Code verweisen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="cf3a6-158">Clientsetup</span><span class="sxs-lookup"><span data-stu-id="cf3a6-158">Client setup</span></span>

<span data-ttu-id="cf3a6-159">Ein JavaScript-Client erfordert Verweise auf jQuery und die signalr Core JavaScript-Datei.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="cf3a6-160">Die jQuery-Version muss 1.6.4 oder größere spätere Versionen sein, z. b. 1.7.2, 1.8.2 oder 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="cf3a6-161">Wenn Sie sich für die Verwendung des generierten Proxys entscheiden, benötigen Sie auch einen Verweis auf die JavaScript-Datei des signalr-generierten Proxys.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="cf3a6-162">Im folgenden Beispiel wird gezeigt, wie die Verweise in einer HTML-Seite aussehen können, die den generierten Proxy verwendet.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="cf3a6-163">Diese Verweise müssen in dieser Reihenfolge enthalten sein: zuerst jQuery, signalr Core und schließlich signalr-Proxys.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="cf3a6-164">Verweisen auf den dynamisch generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="cf3a6-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="cf3a6-165">Im vorangehenden Beispiel besteht der Verweis auf den von signalr generierten Proxy in der dynamischen Generierung von JavaScript-Code und nicht in einer physischen Datei.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="cf3a6-166">Signalr erstellt den JavaScript-Code für den Proxy im Handumdrehen und bedient ihn als Reaktion auf die URL "/signalr/Hubs" an den Client.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="cf3a6-167">Wenn Sie in der `MapSignalR`-Methode eine andere Basis-URL für signalr-Verbindungen auf dem Server angegeben haben, ist die URL für die dynamisch generierte Proxy Datei Ihre benutzerdefinierte URL, an die "/Hubs" angehängt ist.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="cf3a6-168">Verwenden Sie für Windows 8-JavaScript-Clients (Windows Store) die physische Proxy Datei anstelle der dynamisch generierten.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="cf3a6-169">Weitere Informationen finden Sie weiter unten in diesem Thema unter Vorgehens [Weise beim Erstellen einer physischen Datei für den signalr-generierten Proxy](#manualproxy) .</span><span class="sxs-lookup"><span data-stu-id="cf3a6-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="cf3a6-170">Verwenden Sie in einer ASP.NET MVC 4-oder 5 Razor-Ansicht die Tilde, um auf den Anwendungs Stamm in der Proxy Dateireferenz zu verweisen:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="cf3a6-171">Weitere Informationen zur Verwendung von signalr in MVC 5 finden Sie unter [Getting Started with signalr and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="cf3a6-172">Verwenden Sie in einer ASP.NET MVC 3 Razor-Ansicht `Url.Content` für den Proxy Datei Verweis:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="cf3a6-173">Verwenden Sie in einer ASP.net-Web Forms Anwendung `ResolveClientUrl` für Ihren proxydateiverweis, oder registrieren Sie ihn über den ScriptManager mithilfe eines relativen Pfads (beginnend mit einer Tilde):</span><span class="sxs-lookup"><span data-stu-id="cf3a6-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="cf3a6-174">Verwenden Sie als allgemeine Regel dieselbe Methode zum Angeben der URL "/signalr/Hubs", die Sie für CSS-oder JavaScript-Dateien verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="cf3a6-175">Wenn Sie eine URL ohne eine Tilde angeben, funktioniert die Anwendung in einigen Szenarios ordnungsgemäß, wenn Sie in Visual Studio mit IIS Express testen, schlägt jedoch mit einem 404-Fehler fehl, wenn Sie die Bereitstellung für vollständiges IIS ausführen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="cf3a6-176">Weitere Informationen finden Sie unter **Auflösen von Verweisen auf Ressourcen** auf Stamm Ebene in [Webservern in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx) auf der MSDN-Website.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="cf3a6-177">Wenn Sie ein Webprojekt in Visual Studio 2017 im Debugmodus ausführen und Internet Explorer als Browser verwenden, können Sie die Proxy Datei in **Projektmappen-Explorer** unter **Skripts**sehen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="cf3a6-178">Um den Inhalt der Datei anzuzeigen, doppelklicken Sie auf **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="cf3a6-179">Wenn Sie nicht Visual Studio 2012 oder 2013 und Internet Explorer verwenden oder wenn Sie sich nicht im Debugmodus befinden, können Sie auch den Inhalt der Datei erhalten, indem Sie zur URL "/signalR/Hubs" navigieren.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="cf3a6-180">Wenn Ihre Website beispielsweise auf `http://localhost:56699`ausgeführt wird, navigieren Sie in Ihrem Browser zu `http://localhost:56699/SignalR/hubs`.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="cf3a6-181">Erstellen einer physischen Datei für den von signalr generierten Proxy</span><span class="sxs-lookup"><span data-stu-id="cf3a6-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="cf3a6-182">Als Alternative zum dynamisch generierten Proxy können Sie eine physische Datei mit dem Proxycode erstellen und auf diese Datei verweisen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="cf3a6-183">Dies kann zum Steuern der Zwischenspeicherung oder zum Bündeln von Verhalten oder zum Aufrufen von IntelliSense bei der Codierung von Aufrufen von Server Methoden verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="cf3a6-184">Führen Sie die folgenden Schritte aus, um eine Proxy Datei zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="cf3a6-185">Installieren Sie das nuget-Paket [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="cf3a6-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="cf3a6-186">Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner *Tools* , der die Datei signalr. exe enthält.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="cf3a6-187">Der Ordner Tools befindet sich an folgendem Speicherort:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="cf3a6-188">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="cf3a6-189">Der Pfad zu Ihrer *dll* ist in der Regel der Ordner " *bin* " im Projektordner.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="cf3a6-190">Dieser Befehl erstellt eine Datei namens " *Server. js* " im gleichen Ordner wie " *signalr. exe*".</span><span class="sxs-lookup"><span data-stu-id="cf3a6-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="cf3a6-191">Platzieren Sie die Datei " *Server. js* " in einem entsprechenden Ordner in Ihrem Projekt, benennen Sie Sie entsprechend Ihrer Anwendung um, und fügen Sie anstelle der Referenz "signalr/Hubs" einen Verweis darauf ein.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="cf3a6-192">Einrichten einer Verbindung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-192">How to establish a connection</span></span>

<span data-ttu-id="cf3a6-193">Bevor Sie eine Verbindung herstellen können, müssen Sie ein Verbindungs Objekt erstellen, einen Proxy erstellen und Ereignishandler für Methoden registrieren, die vom Server aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="cf3a6-194">Stellen Sie beim Einrichten des Proxy-und Ereignishandler die Verbindung her, indem Sie die `start`-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="cf3a6-195">Wenn Sie den generierten Proxy verwenden, müssen Sie das Verbindungs Objekt nicht in Ihrem eigenen Code erstellen, da der generierte Proxy Code dies für Sie erledigt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="cf3a6-196">**Herstellen einer Verbindung (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="cf3a6-197">**Herstellen einer Verbindung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="cf3a6-198">Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="cf3a6-199">Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="cf3a6-200">Standardmäßig ist der Hub-Speicherort der aktuelle Server. Wenn Sie eine Verbindung mit einem anderen Server herstellen, geben Sie die URL an, bevor Sie die `start`-Methode aufrufen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="cf3a6-201">Normalerweise registrieren Sie Ereignishandler, bevor Sie die `start`-Methode aufrufen, um die Verbindung herzustellen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="cf3a6-202">Wenn Sie einige Ereignishandler registrieren möchten, nachdem Sie die Verbindung hergestellt haben, können Sie dies tun. Sie müssen jedoch mindestens einen ihrer Ereignishandler registrieren, bevor Sie die `start`-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="cf3a6-203">Ein Grund hierfür ist, dass es möglicherweise viele Hubs in einer Anwendung gibt, aber Sie sollten das `OnConnected`-Ereignis nicht auf jedem Hub auslassen, wenn Sie nur eine von Ihnen verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="cf3a6-204">Wenn die Verbindung hergestellt wird, weist das vorhanden sein einer Client Methode auf dem Proxy eines Hubs an, dass signalr das `OnConnected` Ereignis auslöst.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="cf3a6-205">Wenn Sie keine Ereignishandler registrieren, bevor Sie die `start`-Methode aufrufen, können Sie Methoden für den Hub aufrufen, aber die `OnConnected`-Methode des Hubs wird nicht aufgerufen, und es werden keine Client Methoden vom Server aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="cf3a6-206">$. Connection. Hub ist das gleiche Objekt, das von $. hubconnection () erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="cf3a6-207">Wie Sie in den Beispielen sehen können, verweist `$.connection.hub` auf das Verbindungs Objekt, wenn Sie den generierten Proxy verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="cf3a6-208">Dabei handelt es sich um dasselbe Objekt, das Sie durch Aufrufen von `$.hubConnection()` erhalten, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="cf3a6-209">Der generierte Proxy Code erstellt die Verbindung für Sie durch Ausführen der folgenden Anweisung:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Erstellen einer Verbindung in der generierten Proxy Datei](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="cf3a6-211">Wenn Sie den generierten Proxy verwenden, können Sie mit `$.connection.hub` arbeiten, die Sie mit einem Verbindungs Objekt ausführen können, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="cf3a6-212">Asynchrone Ausführung der Start Methode</span><span class="sxs-lookup"><span data-stu-id="cf3a6-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="cf3a6-213">Die `start`-Methode wird asynchron ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="cf3a6-214">Sie gibt ein [Verzögertes jQuery-Objekt](http://api.jquery.com/category/deferred-object/)zurück. Dies bedeutet, dass Sie Rückruf Funktionen hinzufügen können, indem Sie Methoden wie `pipe`, `done`und `fail`aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="cf3a6-215">Wenn Sie über Code verfügen, der nach dem Herstellen der Verbindung ausgeführt werden soll, z. b. einen Server Methodenaufrufe, fügen Sie diesen Code in eine Rückruffunktion ein, oder rufen Sie ihn über eine Rückruffunktion auf.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="cf3a6-216">Die `.done` Rückruf Methode wird ausgeführt, nachdem die Verbindung hergestellt wurde, und nach dem Ausführen von Code, den Sie in ihrer `OnConnected` Ereignishandlermethode auf dem Server haben, wird die Ausführung beendet.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="cf3a6-217">Wenn Sie die "jetzt verbundene"-Anweisung aus dem vorherigen Beispiel als nächste Codezeile nach dem `start`-Methoden Aufrufs (nicht in einem `.done`-Rückruf) ablegen, wird die `console.log` Zeile vor dem Herstellen der Verbindung ausgeführt, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Falsche Methode zum Schreiben von Code, der nach dem Herstellen der Verbindung ausgeführt wird.](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="cf3a6-219">Einrichten einer Domänen übergreifenden Verbindung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="cf3a6-220">Wenn der Browser eine Seite aus `http://contoso.com`lädt, befindet sich die signalr-Verbindung in der Regel in der gleichen Domäne `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="cf3a6-221">Wenn die Seite aus `http://contoso.com` eine Verbindung mit `http://fabrikam.com/signalr`herstellt, handelt es sich um eine Domänen übergreifende Verbindung.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="cf3a6-222">Aus Sicherheitsgründen sind Domänen übergreifende Verbindungen standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="cf3a6-223">In signalr 1. x wurden Domänen übergreifende Anforderungen von einem einzelnen enablecrossdomain-Flag gesteuert.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="cf3a6-224">Dieses Flag hat sowohl JSONP-als auch cors-Anforderungen gesteuert.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="cf3a6-225">Um die Flexibilität zu erhöhen, wurde die gesamte cors-Unterstützung aus der Serverkomponente von signalr entfernt (JavaScript-Clients verwenden weiterhin üblicherweise cors, wenn erkannt wird, dass der Browser Sie unterstützt), und neue owin-Middleware wurde zur Unterstützung dieser Szenarios zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="cf3a6-226">Wenn JSONP auf dem Client erforderlich ist (um Domänen übergreifende Anforderungen in älteren Browsern zu unterstützen), muss es explizit aktiviert werden, indem `EnableJSONP` für das `HubConfiguration` Objekt auf `true`festgelegt wird, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="cf3a6-227">JSONP ist standardmäßig deaktiviert, da es weniger sicher als cors ist.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="cf3a6-228">**Hinzufügen von "Microsoft. owin. cors" zu Ihrem Projekt:** Um diese Bibliothek zu installieren, führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="cf3a6-229">Mit diesem Befehl wird dem Projekt die 2.1.0-Version des Pakets hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="cf3a6-230">Aufrufen von usecors</span><span class="sxs-lookup"><span data-stu-id="cf3a6-230">Calling UseCors</span></span>

 <span data-ttu-id="cf3a6-231">Der folgende Code Ausschnitt veranschaulicht, wie Domänen übergreifende Verbindungen in signalr 2 implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="cf3a6-232">**Implementieren von Domänen übergreifenden Anforderungen in signalr 2**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="cf3a6-233">Der folgende Code veranschaulicht, wie cors oder JSONP in einem signalr 2-Projekt aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="cf3a6-234">In diesem Codebeispiel werden `Map` und `RunSignalR` anstelle von `MapSignalR`verwendet, sodass die cors-Middleware nur für die signalr-Anforderungen ausgeführt wird, die cors-Unterstützung benötigen (anstatt für den gesamten Datenverkehr an dem Pfad, der in `MapSignalR`angegeben ist). Map kann auch für alle anderen Middleware verwendet werden, die für ein bestimmtes URL-Präfix ausgeführt werden muss, und nicht für die gesamte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="cf3a6-235">Legen Sie im Code nicht `jQuery.support.cors` auf "true" fest.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Legen Sie jQuery. Support. cors nicht auf "true" fest.](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="cf3a6-237">Signalr übernimmt die Verwendung von cors.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="cf3a6-238">Wenn Sie `jQuery.support.cors` auf true festlegen, wird JSONP deaktiviert, da signalr davon ausgeht, dass der Browser cors unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="cf3a6-239">Wenn Sie eine Verbindung mit einer localhost-URL herstellen, wird Sie von Internet Explorer 10 nicht als Domänen übergreifende Verbindung betrachtet, sodass die Anwendung lokal mit IE 10 funktioniert, auch wenn Sie keine Domänen übergreifenden Verbindungen auf dem Server aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="cf3a6-240">Informationen zur Verwendung von Domänen übergreifenden Verbindungen mit Internet Explorer 9 finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="cf3a6-241">Weitere Informationen zur Verwendung Domänen übergreifender Verbindungen mit Chrome finden Sie in [diesem StackOverflow-Thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="cf3a6-242">Im Beispielcode wird die Standard-URL "/signalr" verwendet, um eine Verbindung mit Ihrem signalr-Dienst herzustellen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="cf3a6-243">Weitere Informationen zum Angeben einer anderen Basis-URL finden Sie unter [ASP.net signalr Hubs API Guide-Server-the/signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="cf3a6-244">Konfigurieren der Verbindung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-244">How to configure the connection</span></span>

<span data-ttu-id="cf3a6-245">Bevor Sie eine Verbindung herstellen, können Sie Abfrage Zeichen folgen Parameter angeben oder die Transportmethode angeben.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="cf3a6-246">Angeben von Abfrage Zeichen folgen Parametern</span><span class="sxs-lookup"><span data-stu-id="cf3a6-246">How to specify query string parameters</span></span>

<span data-ttu-id="cf3a6-247">Wenn Sie Daten an den Server senden möchten, wenn der Client eine Verbindung herstellt, können Sie dem Verbindungs Objekt Abfrage Zeichenfolgen-Parameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="cf3a6-248">In den folgenden Beispielen wird gezeigt, wie Sie einen Abfrage Zeichen folgen Parameter im Client Code festlegen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="cf3a6-249">**Legen Sie vor dem Aufrufen der Start Methode (mit dem generierten Proxy) einen Abfrage Zeichen folgen Wert fest.**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="cf3a6-250">**Legen Sie vor dem Aufrufen der Start Methode (ohne den generierten Proxy) einen Abfrage Zeichen folgen Wert fest.**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="cf3a6-251">Im folgenden Beispiel wird gezeigt, wie ein Abfrage Zeichen folgen Parameter im Servercode gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="cf3a6-252">Angeben der Transportmethode</span><span class="sxs-lookup"><span data-stu-id="cf3a6-252">How to specify the transport method</span></span>

<span data-ttu-id="cf3a6-253">Im Rahmen der Verbindungs Herstellung aushandiert ein signalr-Client normalerweise mit dem Server, um den optimalen Transport zu ermitteln, der von Server und Client unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="cf3a6-254">Wenn Sie bereits wissen, welcher Transport Sie verwenden möchten, können Sie diesen Aushandlungs Prozess umgehen, indem Sie die Transportmethode angeben, wenn Sie die `start`-Methode aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="cf3a6-255">**Client Code, der die Transportmethode angibt (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="cf3a6-256">**Client Code, der die Transportmethode angibt (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="cf3a6-257">Als Alternative können Sie mehrere Transportmethoden in der Reihenfolge angeben, in der Sie von signalr ausprobiert werden sollen:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="cf3a6-258">**Client Code, der ein benutzerdefiniertes Transport Fall Back Schema (mit dem generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="cf3a6-259">**Client Code, der ein benutzerdefiniertes Transport Fall Back Schema (ohne den generierten Proxy) angibt**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="cf3a6-260">Sie können die folgenden Werte verwenden, um die Transportmethode anzugeben:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="cf3a6-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="cf3a6-261">"webSockets"</span></span>
- <span data-ttu-id="cf3a6-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="cf3a6-262">"foreverFrame"</span></span>
- <span data-ttu-id="cf3a6-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="cf3a6-263">"serverSentEvents"</span></span>
- <span data-ttu-id="cf3a6-264">"longabruf"</span><span class="sxs-lookup"><span data-stu-id="cf3a6-264">"longPolling"</span></span>

<span data-ttu-id="cf3a6-265">In den folgenden Beispielen wird gezeigt, wie Sie herausfinden, welche Transportmethode von einer Verbindung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="cf3a6-266">**Client Code, der die Transportmethode anzeigt, die von einer Verbindung verwendet wird (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="cf3a6-267">**Client Code, der die Transportmethode anzeigt, die von einer Verbindung verwendet wird (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="cf3a6-268">Informationen zum Überprüfen der Transportmethode im Servercode finden Sie unter [ASP.net signalr Hubs API Guide-Server-How to Get Information about the Client from the context Property](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="cf3a6-269">Weitere Informationen zu Transporten und Fallbacks finden Sie unter [Einführung in signalr-Transporte und Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="cf3a6-270">Vorgehensweise beim erhalten eines Proxys für eine Hub-Klasse</span><span class="sxs-lookup"><span data-stu-id="cf3a6-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="cf3a6-271">Jedes von Ihnen erstellte Verbindungs Objekt kapselt Informationen zu einer Verbindung mit einem signalr-Dienst, der eine oder mehrere Hub-Klassen enthält.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="cf3a6-272">Um mit einer Hub-Klasse zu kommunizieren, verwenden Sie ein Proxy Objekt, das Sie selbst erstellen (wenn Sie den generierten Proxy nicht verwenden) oder das für Sie generiert wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="cf3a6-273">Auf dem Client ist der Name des Proxys eine Camel-/schreibversion des Namens der Hub-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="cf3a6-274">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="cf3a6-275">**Hub-Klasse auf dem Server**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="cf3a6-276">**Beziehen eines Verweises auf den generierten Client Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="cf3a6-277">**Erstellen eines Client Proxys für die Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="cf3a6-278">Wenn Sie Ihre Hub-Klasse mit einem `HubName` Attribut ergänzen, verwenden Sie den genauen Namen, ohne den Fall zu ändern.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="cf3a6-279">**Hubklasse auf Server mit hubname-Attribut**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="cf3a6-280">**Beziehen eines Verweises auf den generierten Client Proxy für den Hub**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="cf3a6-281">**Erstellen eines Client Proxys für die Hub-Klasse (ohne generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="cf3a6-282">Definieren von Methoden auf dem Client, der vom Server aufgerufen werden kann</span><span class="sxs-lookup"><span data-stu-id="cf3a6-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="cf3a6-283">Um eine Methode zu definieren, die vom Server von einem Hub aufgerufen werden kann, fügen Sie dem Hub-Proxy einen Ereignishandler hinzu, indem Sie die `client`-Eigenschaft des generierten Proxys verwenden oder die `on`-Methode aufzurufen, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="cf3a6-284">Die Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="cf3a6-285">Fügen Sie den-Ereignishandler hinzu, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="cf3a6-286">(Wenn Sie Ereignishandler hinzufügen möchten, nachdem Sie die `start`-Methode aufgerufen haben, lesen Sie den Hinweis unter [Herstellen einer Verbindung weiter](#establishconnection) oben in diesem Dokument, und verwenden Sie die Syntax, die zum Definieren einer Methode ohne Verwendung des generierten Proxys angezeigt wird.)</span><span class="sxs-lookup"><span data-stu-id="cf3a6-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="cf3a6-287">Beim Methodennamen wird die Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="cf3a6-288">Beispielsweise werden auf dem-Server `Clients.All.addContosoChatMessageToPage` auf dem-Server `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`oder `addcontosochatmessagetopage` auf dem Client ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="cf3a6-289">**Methode auf dem Client definieren (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="cf3a6-290">**Alternative Methode zum Definieren der Methode auf dem Client (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="cf3a6-291">**Methode auf dem Client definieren (ohne den generierten Proxy oder beim Hinzufügen nach dem Aufruf der Start-Methode)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="cf3a6-292">**Server Code, der die Client Methode aufruft**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="cf3a6-293">Die folgenden Beispiele enthalten ein komplexes Objekt als Methoden Parameter.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="cf3a6-294">**Methode auf dem Client definieren, die ein komplexes Objekt annimmt (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="cf3a6-295">**Methode auf dem Client definieren, die ein komplexes Objekt annimmt (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="cf3a6-296">**Server Code, der das komplexe Objekt definiert**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="cf3a6-297">**Server Code, der die Client Methode mithilfe eines komplexen Objekts aufruft**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="cf3a6-298">Vorgehensweise beim Abrufen von Server Methoden vom Client</span><span class="sxs-lookup"><span data-stu-id="cf3a6-298">How to call server methods from the client</span></span>

<span data-ttu-id="cf3a6-299">Um eine Server Methode vom Client aufzurufen, verwenden Sie die `server`-Eigenschaft des generierten Proxys oder die `invoke`-Methode des Hub-Proxys, wenn Sie den generierten Proxy nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="cf3a6-300">Der Rückgabewert oder die Parameter können komplexe Objekte sein.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="cf3a6-301">Übergeben Sie eine Camel-Case-Version des Methoden namens auf dem Hub.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="cf3a6-302">Signalr nimmt diese Änderung automatisch vor, damit JavaScript-Code JavaScript-Konventionen entsprechen kann.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="cf3a6-303">In den folgenden Beispielen wird veranschaulicht, wie eine Server Methode aufgerufen wird, die keinen Rückgabewert hat, und wie eine Server Methode aufgerufen wird, die über einen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="cf3a6-304">**Server Methode ohne hubmethodname-Attribut**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="cf3a6-305">**Server Code, der das komplexe Objekt definiert, das in einem Parameter übergeben wird**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="cf3a6-306">**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="cf3a6-307">**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="cf3a6-308">Wenn Sie die Hub-Methode mit einem `HubMethodName`-Attribut versehen haben, verwenden Sie diesen Namen, ohne den Fall zu ändern.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="cf3a6-309">**Server Methode** mit einem hubmethodname-Attribut</span><span class="sxs-lookup"><span data-stu-id="cf3a6-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="cf3a6-310">**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="cf3a6-311">**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="cf3a6-312">In den vorangehenden Beispielen wird gezeigt, wie eine Server Methode aufgerufen wird, die über keinen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="cf3a6-313">In den folgenden Beispielen wird gezeigt, wie eine Server Methode aufgerufen wird, die über einen Rückgabewert verfügt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="cf3a6-314">**Server Code für eine Methode, die über einen Rückgabewert verfügt**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="cf3a6-315">**Die für den Rückgabewert verwendete Aktienklasse** .</span><span class="sxs-lookup"><span data-stu-id="cf3a6-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="cf3a6-316">**Client Code, der die Server Methode aufruft (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="cf3a6-317">**Client Code, der die Server Methode aufruft (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="cf3a6-318">Behandeln von Ereignissen der Verbindungs Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="cf3a6-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="cf3a6-319">Signalr stellt die folgenden Ereignisse für die Verbindungs Lebensdauer bereit, die Sie behandeln können:</span><span class="sxs-lookup"><span data-stu-id="cf3a6-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="cf3a6-320">`starting`: wird ausgelöst, bevor Daten über die Verbindung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="cf3a6-321">`received`: wird ausgelöst, wenn Daten über die Verbindung empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="cf3a6-322">Stellt die empfangenen Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-322">Provides the received data.</span></span>
- <span data-ttu-id="cf3a6-323">`connectionSlow`: wird ausgelöst, wenn der Client eine langsame oder häufig gelöschter Verbindung erkennt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="cf3a6-324">`reconnecting`: wird ausgelöst, wenn der zugrunde liegende Transport mit dem erneuten Verbinden beginnt.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="cf3a6-325">`reconnected`: wird ausgelöst, wenn der zugrunde liegende Transport die Verbindung wieder hergestellt hat.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="cf3a6-326">`stateChanged`: wird ausgelöst, wenn sich der Verbindungsstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="cf3a6-327">Stellt den alten und den neuen Status bereit (Verbindung, verbunden, wiederherstellen oder Verbindung getrennt).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="cf3a6-328">`disconnected`: wird ausgelöst, wenn die Verbindung getrennt wurde.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="cf3a6-329">Wenn Sie z. b. Warnmeldungen anzeigen möchten, wenn Verbindungsprobleme auftreten, die zu spürbaren Verzögerungen führen können, behandeln Sie das `connectionSlow`-Ereignis.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="cf3a6-330">**Behandeln des connectionslow-Ereignisses (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="cf3a6-331">**Behandeln des connectionslow-Ereignisses (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="cf3a6-332">Weitere Informationen finden Sie unter [verstehen und behandeln von Verbindungs Lebensdauer-Ereignissen in signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="cf3a6-333">Behandeln von Fehlern</span><span class="sxs-lookup"><span data-stu-id="cf3a6-333">How to handle errors</span></span>

<span data-ttu-id="cf3a6-334">Der signalr-JavaScript-Client stellt ein `error` Ereignis bereit, für das Sie einen Handler hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="cf3a6-335">Sie können auch die Fail-Methode verwenden, um einen Handler für Fehler hinzuzufügen, die sich aus einem Aufruf der Server Methode ergeben.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="cf3a6-336">Wenn Sie auf dem Server nicht explizit detaillierte Fehlermeldungen aktivieren, enthält das Ausnahme Objekt, das signalr nach einem Fehler zurückgibt, nur minimale Informationen über den Fehler.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="cf3a6-337">Wenn beispielsweise ein `newContosoChatMessage` Fehler auftritt, enthält die Fehlermeldung im Fehler Objekt den Wert "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`", dass detaillierte Fehlermeldungen an Clients in der Produktionsumgebung nicht gesendet werden sollen. Wenn Sie jedoch ausführliche Fehlermeldungen zu Problem Behandlungszwecken aktivieren möchten, verwenden Sie den folgenden Code auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="cf3a6-338">Im folgenden Beispiel wird gezeigt, wie Sie einen Handler für das Fehler Ereignis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="cf3a6-339">**Hinzufügen eines Fehler Handlers (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="cf3a6-340">**Hinzufügen eines Fehler Handlers (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="cf3a6-341">Im folgenden Beispiel wird gezeigt, wie ein Fehler von einem Methodenaufruf behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="cf3a6-342">**Behandeln eines Fehlers von einem Methodenaufruf (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="cf3a6-343">**Behandeln eines Fehlers von einem Methodenaufruf (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="cf3a6-344">Wenn ein Methodenaufruf fehlschlägt, wird das `error`-Ereignis ebenfalls ausgelöst, sodass der Code im `error` Methoden Handler und im `.fail`-Methoden Rückruf ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="cf3a6-345">Aktivieren der Client seitigen Protokollierung</span><span class="sxs-lookup"><span data-stu-id="cf3a6-345">How to enable client-side logging</span></span>

<span data-ttu-id="cf3a6-346">Um die Client seitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging`-Eigenschaft für das Verbindungs Objekt fest, bevor Sie die `start`-Methode zum Herstellen der Verbindung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="cf3a6-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="cf3a6-347">**Aktivieren der Protokollierung (mit dem generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="cf3a6-348">**Aktivieren der Protokollierung (ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="cf3a6-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="cf3a6-349">Um die Protokolle anzuzeigen, öffnen Sie die Entwicklertools Ihres Browsers, und navigieren Sie zur Registerkarte Konsole. Ein Tutorial mit Schritt-für-Schritt-Anweisungen und Screenshots, die die Vorgehensweise veranschaulichen, finden Sie unter [Server Broadcast mit ASP.net signalr-enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="cf3a6-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
